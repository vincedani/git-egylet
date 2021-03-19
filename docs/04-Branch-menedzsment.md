# Branchek

Fejlesztés során van egy fő ág, amely tartalmazza azt az állapotot, mely eljuthat a megrendelőhöz.
Ezen felül azonban sokszor szükség van elágazásokra, mely során egy committól indulva fejleszthetünk -- pl. egy featuret -- anélkül, hogy a fő ág állapotát megváltoztathatnánk.
Ezek az elágazások a `branch`ek, melyek a Git egyik erőssége.

A branchek "könnyű" szerkezetek: új létrehozása, branchek közötti váltás, törlés; gyors műveletek és nem szükséges hozzájuk nagy mennyiségű adat másolása.

Alapvetően minden commit -- kivéve az első -- tárol egy pointert (mutató) az előző commitra, azaz a commit `A` rendelkezik információval arról, hogy ki az őse, milyen ágon lehet eljutni arra az állapotra a snapshotokon keresztül, amit a commit képvisel.
Ilyen pointer a branch is, egy mutató egy commitra.
Ezek a mutatók változnak, azaz ha az `A` commitra mutat, majd változtatás történik és létrejön egy új commit, akkor a branch átvándorol az új commitra.

Az alapértelmezett branch a `master`, melyet a `git init` hozza létre.
A hiedelemmel ellentétben ez nem egy kitüntetett branch, semmivel sem több mint a többi, csak az `init` után a legtöbb fejlesztő nem törtődik vele és úgy hagyja (GitHubon létrehozott projektek már `main` branchel rendelkeznek).
Ha nem hozunk létre más brancheket, akkor az `init` utáni commitok hatására a `master` mindig az utoljára létrehozott commitra mutat.

## Alapműveletek

### Létrehozás

Egy új branch létrehozásánál létrejön egy új, nevesített pointer, ami a jelenlegi commitra mutat.
A brancheket a `git branch <név>` paranccsal lehet létrehozni.
Az előző dokumentumokban létrehozott projekt repositoryját folytatva:

```bash
# git branch <név>
$ git branch test_branch
$ git lg1
* 2a740e2 - (6 days ago) Harmadik commit. - Felhasználó Név (HEAD -> master, test_branch)
* 2bfb2ec - (3 weeks ago) Második commit. - Felhasználó Név
* 81f766d - (3 weeks ago) Első commit. - Felhasználó Név
```

Jelenlegi állapotban létre lett hozva a `test_branch`, viszont még a `master` az aktuálisan aktív, azaz a `HEAD` mutató oda mutat.
A `HEAD` egy speciális pointer, amely azt jelképezi, hogy melyik állapot az aktív: `2a740e2` commit `master` branch.
Amennyiben változtatás történik és egy új commit, akkor a `test_branch` marad a `2a740e2` commiton és a `master` fog az új commitra mutatni.

Az, hogy jelenleg melyik branch aktív, a `git branch` paranccsal is ellenőrizhető: a `*` karakterrel jelölt az.

```bash
$ git branch
* master
  test_branch
```

### Váltás

Branchek közötti váltásra a `git checkout <név>` szolgál, hatására az aktív branch a paramétereben kapott nevű lesz.

```bash
$ git checkout test_branch
Switched to branch 'test_branch'
$ git branch
  master
* test_branch
```

Az `lg1` parancs kimentetét vizsgálva látható, hogy a `HEAD` mutató most a `test_branch`-re mutat.
A `log` parancs csak azokat a brancheket mutatja ami a `HEAD` alatti historyban van, így megeshet, hogy egy-egy branchet nem mutat a log, viszont ott van.
A `--all` kapcsolóval ez mellőzhető.

```bash
$ git lg1
* 2a740e2 - (6 days ago) Harmadik commit. - Felhasználó Név (HEAD -> test_branch, master)
* 2bfb2ec - (3 weeks ago) Második commit. - Felhasználó Név
* 81f766d - (3 weeks ago) Első commit. - Felhasználó Név
```

Általában -- ha nem ugyanarra a commitra mutatnak -- a branchek közötti váltás fájlváltozással jár, azaz megváltozik a repository állapota a megfelelő commit által reprezentált snapshotra.

Látható, hogy ha egy új branchet szeretnénk létrehozni és átállni rá, akkor ez két parancsba kerül.
Ez a `checkout` egyik kapcsolójával megoldható egy lépésben:

```bash
$ git checkout -b test2
Switched to a new branch 'test2'
$ git branch
  master
* test2
  test_branch
```

A `checkout` parancs nem csak branchek közötti váltásra használható, hanem commitokra is.
Ekkor egy úgynevezett `detached HEAD` állapotba kerül a repository.
A parancs hatására az alábbi üzenet fogad, ami tudtunkra adja, hogy sikeresen átváltottunk a `2bfb2ec` commitra, létrehozhatunk itt egy új branchet, viszont ha most valami változik, az a már létező brancheinket nem változtatja.
Volt szó arról, hogy minden commit rendelkezik egy mutatóval az őt megelőző commitra, így ha itt változtatunk valamit, attól még a `master` branch által mutatott commit szülei nem fognak eltűnni, így valid marad az az állapot.

```bash
$ git checkout 2bfb2ec
Note: switching to '2bfb2ec'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 2bfb2ec Második commit.
```

Az üzenet nem `checkout`, hanem `switch` parancsot említ, amely egy másik parancs a branchek közötti váltás megvalósítására.
A `git switch <név>` megegyezik a `checkout`-os megfelelőjével, míg a `checkout -b <név>`-hez z `switch -c <név>` tartozik.
A `switch -` az előző branchre tér vissza (mint a `cd -`).

### Törlés

Amennyiben már nincs szükségünk egy branchre akkor törölhető a `branch --delete <név>` paranccsal.

```bash
$ git branch --delete test2
```

Természetesen csak a nem aktív branchek törölhetők, így a `test2` törléséhez előbb át kell váltani egy tetszőleges másikra.

## Elágazások és kezelésük

Belátható, hogy ha branchek vannak haszálva, akkor a git history előbb-utóbb kettéválik (a kettő jelképes, annyi felé ahány branch létezik), így több "párhuzamos valóság" létezik a repositoryban.
Minden branch egy-egy állapotot reprezentál, melyek között a taglalt módszerrel lehet váltani.

Tekintsük meg a következő állapotot:

```bash
$ git lg1
* 1c3a0cc - (7 seconds ago) Ötödik commit. - Felhasználó Név (HEAD -> master)
| * 539bbef - (32 seconds ago) Negyedik commit. - Felhasználó Név (test_branch)
|/
* 2a740e2 - (6 days ago) Harmadik commit. - Felhasználó Név (origin/master)
* 2bfb2ec - (3 weeks ago) Második commit. - Felhasználó Név
* 81f766d - (3 weeks ago) Első commit. - Felhasználó Név
```

A `2a740e2` commit két másiknak is a szülője, azaz innen két különböző állapot is elérhető.
A `master` branchen lévő "Ötödik commit.", illetve a `test_branch`-en lévő "Negyedik commit."

| ![Elágazott history: [Git Graph]](img/04-elagazas.PNG) |
|:--:|
| *Elágazott history állapota. Forrás: [Git Graph]* |****

Az ábrán félkövérrel van kiemelve az az állapot, melyre a `HEAD` mutat.

Ez a látvány nem egyedi amennyiben több fejlesztő is dolgozik egy repositoryn.
Pl. dolgozom a `test_branch`en, amin egy új funkcionalitást valósítok meg.
Ez mellett a kollégám a `master` branchre commitált hibajavítást és máris jelen van az állapot.

### Merge

Ahhoz, hogy a "Negyedik commit."-ban található módosítások a masteren is láthatók legyenek, össze kell *mergelni* a két branchet.
Ehhez a `git merge` parancs használható.
Először arra a branchre kell átállni, amelyen a merge eredményét szeretnénk látni (jelenlegi állapotban már a `master` az aktív branch).

```bash
$ git merge test_branch
Auto-merging 01-pelda.txt
CONFLICT (content): Merge conflict in 01-pelda.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Mivel mindkét commit ugyanazt a fájlt módosította, így konfliktus keletkezett, melyet a git nem tud megoldani automatikusan.
A következő állapot jelentkezik:

```bash
git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   01-pelda.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

És ha megvizsgáljuk a fájlt, módosult:

```txt
<<<<<<< HEAD
Commit a master branchen.
=======
Commit a test_branch branchen.
>>>>>>> test_branch
```

A `<<<<<<< HEAD` és az `=======` karakterek közötti szöveg a `current change`, azaz azon a branchen lévő módosítás, amin állunk (`Commit a master branchen.`), míg a `=======` és az `>>>>>>> test_branch` között rész taralmazza az `incoming change`-t, azaz a mergelendő branchről érkező módosításokat.
A `conflict` feloldásához ki kell választani azt, hogy mely módosítás maradjon meg a továbbiakban és mely legyen eldobva, de mindkettő is megtartható.
A ténylegesen fontos feladat, hogy a `<<<<<<< HEAD`, `=======` és a `>>>>>>> test_branch` sorok eltűnjenek, akkor érzékeli úgy a Git, hogy meg lett oldva a probléma.

Tegyünk így, töröljük ki ezeket a sorokat, így mindkét változtatás megmarad.

```txt
14|lapokkal, és legutóbb softwarekkel mint például az Aldus Pagemaker.
15|
16|Commit a master branchen.
17|Commit a test_branch branchen.
18|
```

Ezután hozzá kell adni a változott fájlt és `commit`álni.

```bash
$ git add 01-pelda.txt
$ git commit
```

A commit szövege már ki lett töltve automatikusan.

| ![Merge commit: [Git Graph]](img/04-merge-commit.PNG) |
|:--:|
| *Merge commit. Forrás: [Git Graph]* |****

Létrejött egy új commit a `masteren`, a `test_branch` ott maradt, ahol eddig volt.

A merge egy másik módja a `fast forward` merge, mely esetén nincs olyan elágazás, mint a példában.

| ![Fast forward merge: [Git Graph]](img/04-ff-merge.PNG) |
|:--:|
| *Fast forward merge. Forrás: [Git Graph]* |****

Jelen állapotban egy `fast forward` merge hajtható végre:

```bash
$ git checkout master
$ git merge test_branch2
Updating dba6b18..6f51f56
Fast-forward
 01-pelda.txt | 1 +
 1 file changed, 1 insertion(+)
```

A parancsok hatására a `master` branch is a "Hatodik commit."-ra vándorolt, merge commit nélkül.

A már kellő brancheket ki lehet törölni, hiszen már a master branchen is megvan az a munka, melyet szeretnénk.

```bash
$ git branch -d test_branch test_branch2
Deleted branch test_branch (was 539bbef).
Deleted branch test_branch2 (was 6f51f56).
```

### Rebase

A merge mellett a `rebase` egy másik módszer arra, hogy az elágazott állapotokat újra egységessé tegyük.
Rebase esetén nincs új, harmadik merge commit, hanem az ágat teljes mértékben átmozgatja a git a cél branchre és ott megpróbálja újra alkalmazni a módosításokat.

| ![Elágazott history állapota: [Git Graph]](img/04-rebase.PNG) |
|:--:|
| *Elágazott history állapota: [Git Graph]* |****

Ugyanaz a végeredmény kellene, mint a merge esetében: a master branchen ott legyen a "Hetedik commit." és a "Nyolcadig commit." is, viszont egy plusz merge commit nélkül.

Ebben a `rebase` parancs segíthet:

```bash
$ git checkout master
$ git rebase feature
$ git merge feature
```

Mint a merge esetén, itt is a cél branchre először át kell lépni, majd a "Nyolcadik commit."-ot újra alkalmazni az új állapotra (rebase).
Ebben az állapotban a merge már `fast-forward` módban végrehajtható, nem keletkezik plusz egy merge commit.

A kétlépéses `checkout + rebase` megoldható egyben is a `git rebase <hova> <honnan>`, ami a fenti parancsokkal egyenértékű (a merge kivételével).

Ha a merge előtt mindig alkalmazzuk a rebase-t, akkor a history lineáris marad, melyet egy projektnél megkövetelhetnek.

| ![Rebaselt elágazás: [Git Graph]](img/04-rebased.PNG) |
|:--:|
| *Rebaselt elágazás: [Git Graph]* |****

Rebase során az eredeti commit elvész és egy megegyező tartalmú jön létre máshol, így kellemetlenségeket okozhat ha mások a rebaselt commitra dolgoztak.

### Interaktív rebase

Ahogyan interaktívan lehetett commitolni, úgy rebaselni is, ami nagy potenciált rejt magában.
Tegyük fel, hogy a `feature branch` tovább fejlődött, készült két új commit is, amiből csak egyet kellene a masterre rebaselni.
Ezt interaktív rebase használatával tehetjük meg.

| ![Interaktív rebase: [Git Graph]](img/04-interactive-rebase.PNG) |
|:--:|
| *Interaktív rebase: [Git Graph]* |****

A "9. commit." elnevezésű commit nem kell, viszont a 10. igen, a 11. commit után.

```bash
$ git rebase -i master feature
drop 7e62da7 9. commit.
pick 155ad9c 10. commit.
# Mentés, conflict feloldás és rebase --continue ha kell.
```

A parancsok után a history a következő, a kidobott commit nélkül.

| ![Interaktív rebase után: [Git Graph]](img/04-interactive-rebased.PNG) |
|:--:|
| *Interaktív rebase után: [Git Graph]* |****

A fájlba beleírt `drop` és `pick` a következő lehetőségek közül került kiválasztásra, attól függően (a kapcsolólista nem teljes, a gyakran alkalmazott opciókat tartalmazza):

* `p`, `pick` `<commit>` = a commit használata módosítás nélkül,
* `r`, `reword` `<commit>` = a commit használata, de az üzenet átírása,
* `e`, `edit` `<commit>` = a commit használata, de megváltoztatható a tartalma (amend),
* `s`, `squash` `<commit>` = a commit használata, de a szülőjébe beleolvasztva (2 commit -> 1 commit),
* `f`, `fixup` `<commit>` = mint a squash, de eldobja az üzenetet,
* `d`, `drop` `<commit>` = a commit eldobása

Nem csak két branchet lehet rebaselni, egy branch történetét is át lehet írni.
A `git rebase -i HEAD~N` az utolsó N darab commitot terjeszti elő rebasere, így lehetőség van egy régebbi commit szövegét, vagy tartalmát megváltoztatni.
Ezzel a módszerrel, a legelső commitot nem lehet megváltoztatni, ahhoz a `--root` parancs szükszéges: `git rebase -i --root`.

| ![A legelső commit rebaselése: [Git Graph]](img/04-rebase-root.PNG) |
|:--:|
| *A legelső commit rebaselése: [Git Graph]* |****

## Git Flow

Alapvetően minden vállalat, minden projekt különböző munkafolyamatot alkalmazhat, viszont általánosan megfogalmazható pár megállapítás.
Az, hogy egy projekt merge commitokat, vagy rebaselést és egy egyenes fő-ágat preferál, kb. mindegy is: alkalmazkodni kell hozzá.

A *Git Flow* definiál pár hússzú élettartamú branchet és sok-sok rövid élettartamút.
A `master` branch azt az állapotot reprezentálja ami utoljára a megredenlőhöz került, emellett létezik egy fejlesztési ág `dev`, (`develop`, `development`), melyet a releasek során bemergelnek a `masterbe`.

A `dev` branchből leágaznak a fejlesztők különböző `topic`okat megvalósítandó fejlesztéseikhez -- `feature`, `bugfix` --, majd a munka befejeztével ezeket a brancheket bemergelik a `dev`be.

Ehhez a folyamathoz legtöbbször társul egy CI/CD, mely sikeres lefutása esetén történhet meg a merge.

| ![Git Flow: [Git Graph]](img/04-git-flow.PNG) |
|:--:|
| *Git Flow. Forrás: [Medium](https://medium.com/devsondevs/gitflow-workflow-continuous-integration-continuous-delivery-7f4643abb64f)* |****

## Távoli branchek

A távoli branchek ugyanolyan mutatók, mint a lokális repositoryban találhatók, viszont nem ugyanúgy kell kezelni őket.
A `remote show <név>` parancs megmutatja hogy milyen távoli branchek vannak, illetve azok közül melyek vannak kapcsolatban a lokális társaikkal.

```bash
# git remote show <név>
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/<user>/tmp.git
  Push  URL: https://github.com/<user>/tmp.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local ref configured for 'git push':
    master pushes to master (fast-forwardable)
```

A kimenet alapján csak a `master` branch létezik az upstreamben és a lokális master branch pusholásakor a git arra a branchre pushol.
Az ábrákon látható is volt az `origin/master` amely még mindig a "Harmadik commit"-on áll.
Ahhoz, hogy a GitHubon létező repository is ugyanabba az állapotba kerüljön, mint ami el lett követve lokálisan ki kell adni a `git push origin master` parancsot.

```bash
$ git push origin master
Enumerating objects: 14, done.
Counting objects: 100% (14/14), done.
Delta compression using up to 4 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (12/12), 1015 bytes | 101.00 KiB/s, done.
Total 12 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), completed with 1 local object.
To https://github.com/<user>/tmp.git
   2a740e2..6f51f56  master -> master
```

Vizsgáljuk meg azt az állapotot, amikor egy kollégám felpusholta a munkáját, viszont nálam még nem frissült a repository és összhangba kellene hozni az upstreamban lévő munkát a sajátommal.

| ![Elagazott remote: [Git Graph]](img/04-elagazott-remote.PNG) |
|:--:|
| *Elágazott remote. Forrás: [Git Graph]* |****

Hogy mindkettőnk munkája megmaradjon, a `pull origin master --rebase` parancs segíthet.

```bash
$ git pull origin master --rebase
From https://github.com/<user>/tmp
 * branch            master     -> FETCH_HEAD
First, rewinding head to replay your work on top of it...
Applying: Hatodik commit, nem tudtam a másikról.
Using index info to reconstruct a base tree...
M       01-pelda.txt
Falling back to patching base and 3-way merge...
Auto-merging 01-pelda.txt
CONFLICT (content): Merge conflict in 01-pelda.txt
error: Failed to merge in the changes.
Patch failed at 0001 Hatodik commit, nem tudtam a másikról.
hint: Use 'git am --show-current-patch' to see the failed patch
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```

Újra egy konfliktus keletkezik, mert mindkét commit megváltoztatta a repositoryban lévő egyetlen egy fájlt.
A conflict megoldása után `git rebase --continue` parancs segítségével befejezhető a pull.

| ![Elagazott remote pullolva: [Git Graph]](img/04-elagazott-remote-pull.PNG) |
|:--:|
| *Elágazott remote pullolva. Forrás: [Git Graph]* |****

**Távoli branch követése.**
Ha egy, csak a távoli repositoryn létező branchet szeretnénk követni a lokális repositoryban, akkor kapcsolatot kell kialakítani a két branch között (mint amilyen a master -- origin/master között van).

```bash
# git checkout --track <remote>/<branch>
$ git checkout --track origin/test_branch
Branch test_branch set up to track remote branch test_branch from origin.
Switched to a new branch 'test_branch'
```

Amennyiben más névvel szeretnénk lokálisan kezelni a branchet, mint ami az upstreamben van, az is megtehető a következő paranccsal:

```bash
# git checkout -b lokális_nev <remote>/branch
$ git checkout -b daninja_branch origin/test_branch
```

Ha már egy meglévő lokális branchet szeretnénk összekötni egy távolival, akkor a `git branch -u <remote>/<branch>` parancs segíthet.

```bash
$ git branch -vv
* daninja_branch 23435c5 Hatodik commit, nem tudtam a másikról.
  master         23435c5 [origin/master: ahead 1] Hatodik commit, nem tudtam a másikról.
```

### Távoli branch törlése

Távoli branchek a `push --delete` parancs segítségével törölhetők.

```bash
$ git push origin --delete daninja_branch
To https://github.com/<user>/tmp.git
 - [deleted]         daninja_branch
```

## Állapotok kezelése

Megeshet, hogy egy rész munkálatai során nagyobb prioritású feladat érkezik és át kell váltanunk egy másik branchre.
A félkész munkát lehet commitolni is, viszont egy könnyedebb megoldás a `stash` alkalmazása, mely során `clean state` keletkezik, azaz az utolsó commit állapotára áll vissza a Git, azzal, hogy a félkész munkát vissza lehet állítani.

A stash az összes követett fájlt és `staged` változtatást egybegyúr egy úgynevezett `stash`be, melyet később újra alkalmazni lehet, akár egy másik branchen is.

A gyógypéldát módosítva a `status` a következő kimenetet adja:

```bash
$ git status
On branch feature
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   01-pelda.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git stash
Saved working directory and index state WIP on feature: 881882b 10. commit.

$ git status
On branch feature
nothing to commit, working tree clean
```

A `stash` elmentette az állapotot a `WIP on on feature: 881882b 10. commit` elnevezésú ideiglenes helyre.
A `stash list` segítségével kilistázhatók a létrehozott állományok.

```bash
$ git stash list
stash@{0}: WIP on feature: 881882b 10. commit.
```

Ha az elmentett állapotot újra alkalmazni szeretnénk -- bárhol --, akkor az `apply` segítségével tehetjük meg.

```bash
# git stash apply [referencia]
$ git stash apply stash@{0}
On branch feature
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   01-pelda.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

A referencia megadása opcionális, hiányában az utolsó `stash`t fogja alkalmazni.
Az alkalmazás után is megmaradt a `stash@{0}`, a `list` parancs kimenetében látható.
Törléséhez a `git stash drop referencia` parancs szükséges.

Egy új fájl hozzáadása esetében, amely még nem a repository része, a `stash` nem menti le, viszont expliciten megkérhetjük rá az `--include-untracked` kapcsoló segítségével.

### Clean állapot létrehozása

A `stash` segítségével lementettük a lementeni való félkész munkát, viszont ha maradt még `untracked` fájl, a repository még mindig tartalmaz változásokat, így nehéz lesz mozogni branchek között.
A probléma megoldására szolgál a `git clean`, mely megtisztítja a repositoryt mindennemű `untracked` fájltól.

A `clean` a `-f` (force) kapcsoló nélkül nem csinál semmit, viszont ha először meg szeretnénk tudni, hogy "mit csinálna", megadható neki a `--dry-run` kapcsoló.
Ennek hatására kiírja a konzolra, hogy mit törölne, de nem teszi meg.

```bash
$ git clean --dry-run
Would remove 03-pelda.txt

$ git clean --force
Removing 03-pelda.txt
```
