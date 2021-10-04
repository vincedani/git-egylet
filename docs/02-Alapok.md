# Alapok

A Git alapvetően három állapotot különböztet meg egy fájl szemszögéből tekintve: `modified` (módosított), `staged` (módosított és már elfogadott -- biztosan van jobb frázis) és `committed` (komittált).

* `modified`: a fájl módosult, de még nem történtek lépések afelé, hogy a következő commitba bekerüljön.
* `staged`: a fájl módosult és meg lett jelölve az állapot, hogy a következő commitba bekerüljön. Abban az esetben ha egy `staged` fájlt tovább módosítunk, akkor az új módosítások már a `modified` állapotban látszanak (1 fájl 2 állapottal).
* `committed`: A `staged` fájl a commit parancs hatására bekerül a repositoryba és ott is marad.

Az állapotokkal összhangban három különböző szekciót különböztetünk meg: `repository` (.git mappa), `staging area` és `working directory`.

| ![Állapotok. Forrás: [1]](img/02-szekciok.PNG) |
|:--:|
| *Állapotok. Forrás: [1]* |

A módosult fájlokat -- amik `modified` állapotban vannak -- a `working directory` tartalmazza.
Ezek a `stage` hatására (később) átkerülnek a `staging area`-ba, majd a `commit` hatására (szintén később) bekerül a repositoryba.

Minden amit Git-tel teszünk, egy checksum-on (SHA-1) keresztül hashelődik és így kerül tárolásra.
Később csak ezt a hasht kell tudni ahhoz, hogy elérjünk egy állapotot.
Ez azt is jelenti, hogy nem lehet úgy megváltoztatni egy állapotot, hogy a Git ne venné észre (a checksum változni fog ha bármi máshogy áll).

## Init

Két módon kerülhetünk kapcsolatba egy Git repositoryval.
Először is egy lokális mappát (üres vagy sem -- mindegy) átkonvertálunk, a másik megoldás pedig, hogy egy meglévő "repót" klónozunk a hálózatról.
Az `Alapok` szekció lokális műveletekkel foglalkozik, a klónozást egy későbbi dokumentum taglalja.

Tegyük fel, hogy van egy üres mappánk, `~/dev/01-alapok` az útvonala.
A mappa repositoryvá alakításához először bele kell lépni, majd kiadni az inicializáló parancsot.

```bash
$ cd ~/dev/01-alapok
$ git init
Initialized empty Git repository in ~/dev/01-alapok/.git/
$ ls -A
.git
```

Látható, hogy egy `.git` nevű mappa jött létre a kiadott parancs hatására.
Ez a mappa tartalmazza a repository összes fájlját.

Most, hogy van egy üres repository, adjuk hozzá a következő fájlt: `01-pelda.txt`.

```txt
Első sor.
Második sor.
Harmadik sor.

A Lorem Ipsum egy egyszerû szövegrészlete, szövegutánzata
a betûszedõ és nyomdaiparnak. A Lorem Ipsum az 1500-as
évek óta standard szövegrészletként szolgált az iparban;

mikor egy ismeretlen nyomdász összeállította a betûkészletét
és egy példa-könyvet vagy szöveget nyomott papírra, ezt
használta. Nem csak 5 évszázadot élt túl, de az elektronikus
betûkészleteknél is változatlanul megmaradt. Az 1960-as években
népszerûsítették a Lorem Ipsum részleteket magukbafoglaló Letraset
lapokkal, és legutóbb softwarekkel mint például az Aldus Pagemaker.
```

Ha az eddig elmondottak igazak, akkor a `working directory`-ban van az új fájl.
Ennek megbizonyosodásához a `git status` parancs használható, mely a `working directory`-ról és a `staging area`-ról mond némi információt.

```bash
$ git status
On branch master
No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        01-pelda.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Az látható, hogy az aktuális branch a `master`, még nincsenek commitok és van egy `untracked` fájl.

Érdemes megjegyezni az olvasó számára, hogy a `master`, vagyis a fő fejlesztési ág, csupán logikai definícó, a git szempontjából nem kitüntetett.
A `master` elenevezés használatára az iparágban terjedő trend a `main` elnevezés (lásd github).
Mi a `git` által jelenleg alapértelmezettnek használt `master`-ként hivatkozunk a továbbiakban erre.

## Commitok

Ahhoz, hogy a `01-pelda.txt` a `staging area`-ba kerüljön, vagy a status szerinti `tracked files` kategóriába, hozzá kell adni.
Ahogyan a status szövege meg is sejteti, a `git add <FILE> [<FILE>, ...]` hajta végre.
Ha minden fájlt hozzá szereténk adni, ahhoz a `git add .` segít hozzá.

```bash
$ git add 01-pelda.txt
$ git status
On branch master
No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   01-pelda.txt
```

Látható, hogy már olyan változtatásnak tekinti a fájlt, amit commitálni fog a következő `commit` parancs hatására.
Amennyiben egy fájlt úgy tüntet fel a státusz, hogy a következő commit része lesz, viszont meggoldoljuk magunkat, akkor a `git rm --cached <FILE>` parancs visszateszi azt az `untracked files` szekció alá.

Adjuk ki a `git commit` parancsot!
A konfigurációban beállított szövegszerkesztő nyílik meg (pl. *nano*), ahová egy a commitot leíró üzenetet írhatunk.
Szöveg megadása kötelező.
Munkahelyi környezetben -- legtöbbször -- megkövetelik a commitok dokumentációját, hiszen később így lehet könnyen azonosítani, hogy mit tartalmaz.
Legyen a szöveg az, hogy "Első commit.", majd mentsünk és zárjuk be a szövegszerkesztőt (nano esetében CTRL+O, CTRL+X).

```bash
$ git commit
[master (root-commit) 8c1bd52] Első commit.
 1 file changed, 15 insertions(+)
 create mode 100644 01-pelda.txt
```

Amennyiben tudjuk, hogy csak egy egyszerű, egysoros üzenetet szeretnénk hagyni a későbbi önmagunknak, a `--message` kapcsolóval megadhatjuk úgy, hogy nem nyílik meg a szövegszerkesztő (pl. `git commit --message "Első commit."`).

### Elrontottam?

Felmerülhet a kérdés, hogy mi van ha elrontottam a commitot?
Eggyel több fájl került bele, vagy eggyel kevesebb?
Olyan változtatás is belekerült ami még nincs kész teljesen, ez akkor már ott marad?

A bevezetőben el lett mesélve, hogy a Git csak hozzáad információt, el sosem vesz, akkor most a bénaságomat mindenki látni fogja?
Nem feltétlenül.
**A commitokat lehet módosítani.**

Először vizsgáljuk meg azt, hogy most mit lát a Git a ténykedésemből.
A teljesség igénye nélkül adjuk ki a `git log` parancsot.

```bash
$ git log
commit 8c1bd5230b08d0ef6de19ba5366b32beba49fd47 (HEAD -> master)
Author: Felhasználó Név <email@cím.hu>
Date:   Thu Feb 11 19:32:16 2021 +0100

    Első commit.
```

Van egy hash, ami azonosítja az egységet, van neki egy szerzője és egy időpont amikor elkészült.

Kiderült, hogy a 7. és a 9. sor közé egy felesleges sortörés ékelődött, így néz most ki a szöveg:

```txt
7 |évek óta standard szövegrészletként szolgált az iparban;
8 |
9 |mikor egy ismeretlen nyomdász összeállította a betûkészletét
```

Úgy illene kinéznie a commitnak, hogy ez a sor nincs ott, töröljük hát ki.

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   01-pelda.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

Egy, már a repositoryban létező fálj módosult és ez a módosítás nem fog bekerülni a következő commitba.
Adjuk hozzá, és módosítsuk az előző commitot:

```bash
$ git add 01-pelda.txt
$ git commit --amend
[master 81f766d] Első commit.
 Date: Thu Feb 11 19:32:16 2021 +0100
 1 file changed, 14 insertions(+)
 create mode 100644 01-pelda.txt
```

Az `--amend` kapcsoló jelzi a Git-nek, hogy az utolsó commitot módosítsa.
Változott a hash, és az időpont.

### Nem szeretném az összes módosítást becommitálni

Tegyük fel, hogy egy fájlban két függvényt szeretnék megírni, az egyik kész van a másikat pedig még álmodom.
A kész függvényre szüksége van a kollégáimnak, ezért azt kell commitálnom, viszont a félkész dolgokat nem osztanám meg ha nem muszáj.

Ennek illusztrálására bővítsük két új sorral a meglévő fájlt.

```txt
 4|Negyedik sor.
14|Tizennegyedik sor.
```

A `git diff` paranccsal kiderül, hogy milyen módosításokat érzékelt a Git.

```diff
diff --git a/01-pelda.txt b/01-pelda.txt
index 7e1cba4..8a8488b 100644
--- a/01-pelda.txt
+++ b/01-pelda.txt
@@ -1,7 +1,7 @@
 Első sor.
 Második sor.
 Harmadik sor.
-
+Negyedik sor.
 A Lorem Ipsum egy egyszerû szövegrészlete, szövegutánzata
 a betûszedõ és nyomdaiparnak. A Lorem Ipsum az 1500-as
 évek óta standard szövegrészletként szolgált az iparban;
@@ -11,4 +11,4 @@ használta. Nem csak 5 évszázadot élt túl, de az elektronikus
 betûkészleteknél is változatlanul megmaradt. Az 1960-as években
 népszerûsítették a Lorem Ipsum részleteket magukbafoglaló Letraset
 lapokkal, és legutóbb softwarekkel mint például az Aldus Pagemaker.
-
+Tizennegyedik sor.
```

A `-`-al jelzett sorok törlődtek, a `+`-al jelzettek pedig hozzáadódtak, ebből is látszik, hogy a verziókezelő sor szinten követi a változtatásokat.
A "Negyedik sor." szöveget szeretném a következő commitban tudni, míg a "Tizennegyedik sor."-t nem.
Ez a `git add -p` segítségével tehető meg, mely egy interaktív shellt nyit és minden változtatásra megkérdezi a felhasználótól, hogy szeretné-e hozzáadni a `staging area`-hoz.

```bash
$ git add -p
diff --git a/01-pelda.txt b/01-pelda.txt
index 7e1cba4..8a8488b 100644
--- a/01-pelda.txt
+++ b/01-pelda.txt
@@ -1,7 +1,7 @@
 Első sor.
 Második sor.
 Harmadik sor.
-
+Negyedik sor.
 A Lorem Ipsum egy egyszerû szövegrészlete, szövegutánzata
 a betûszedõ és nyomdaiparnak. A Lorem Ipsum az 1500-as
 évek óta standard szövegrészletként szolgált az iparban;
(1/2) Stage this hunk [y,n,q,a,d,j,J,g,/,e,?]?
```

Két változtatás van és az elsőt dönthetjük el most, a releváns választások:

* `y`: módosítás hozzáadása,
* `n`: módosítás mellőzése,
* `q`: kilépés, az eddig végrehajtott hozzáadások megmaradnak,
* `a`: módosítás és minden ami következik hozzáadása,
* `d`: módosítás és minden ami következik mellőzése,
* `g`: módosítás választása,
* `j`: módosítás döntés nélkül hagyása, és a következő el nem döntöttre ugrás,
* `J`: módosítás döntés nélkül hagyása, és a következőre ugrás,
* `k`: módosítás döntés nélkül hagyása, és az előző el nem döntöttre ugrás,
* `K`: módosítás döntés nélkül hagyása, és az előzőre ugrás,
* `s`: jelenlegi módosítás kisebb darabokra tördelése,
* `?`: help kiíratása

A többi opcióhoz a `man git add` ad részletes leírást.
Válasszuk ki az `y`-t az "Negyedik sor."-hoz, majd `n`-t a "Tizennegyedik sor."-hoz.
A `git status` most azt mutatja, hogy ugyanaz a fájl két helyen is létezik.

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   01-pelda.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   01-pelda.txt
```

A `git diff` segítségével látható az a változtatás, ami nem került hozzáadásra, míg a `git diff --staged` a hozzáadott változtatást mutatja meg.
Amennyiben több fájl módosult és csak egyet szeretnénk megtekinteni, akkor a `git diff <FILE>` segítségével megtehetjük.
Commitoljuk be a hozzáadott módosítást a `git commit -m "Második commit."` parancs segítségével.

## History

Most, hogy már van két commit a projektben, nézzük őket vissza.
A `git log` parancs már volt említve, most részletesen is bemutatásra kerül.

```bash
commit 2bfb2ec1b436ef41a3f8e269a70d099a46c1e502 (HEAD -> master)
Author: Felhasználó Név <email@cím.hu>
Date:   Thu Feb 11 20:46:59 2021 +0100

    Második commit.

commit 81f766da144b96ae43d17ad5f447eb0c400d3f39
Author: Felhasználó Név <email@cím.hu>
Date:   Thu Feb 11 19:32:16 2021 +0100

    Első commit.
```

Az olvasás alulról felfelé történik, a legutoljára létrehozott commit a legfelső.
A felső commit -- `2bfb2e` -- szülője a `81f766` hashű commit, az pedig első entitás lévén szülő nélkülinek tekintendő.
Az első sor végén még látható egy zárójelpár: `(HEAD -> master)`.
A `HEAD` jelzi, hogy jelen állapotban hol állunk (commit `2bfb2e`) és ez megegyezik a `master` branchel.
A `--patch` kapcsoló hozzáadásával a fenti információ mellé még a commitokban tárolt változásokat is láthatjuk.

Ha csak azt szeretnénk látni, hogy egy-egy commit milyen fájlokat változtatott általánosan, akkor a `--stat` parancsot használhatjuk.

```bash
commit 2bfb2ec1b436ef41a3f8e269a70d099a46c1e502 (HEAD -> master)
Author: Felhasználó Név <email@cím.hu>
Date:   Thu Feb 11 20:46:59 2021 +0100

    Második commit.

 01-pelda.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

commit 81f766da144b96ae43d17ad5f447eb0c400d3f39
Author: Felhasználó Név <email@cím.hu>
Date:   Thu Feb 11 19:32:16 2021 +0100

    Első commit.

 01-pelda.txt | 14 ++++++++++++++
 1 file changed, 14 insertions(+)
```

Látható, hogy hány fájl változott, azon belül is hány sor lett hozzáadva és törölve.

Két commit vizsgálata közben átlátható ez a forma, viszont több esetében már nehézkes lehet keresni követni.
Formázható a log kimenete a `--pretty=format:` vagy `--format=format:` kapcsolóval.
A `--graph` kapcsoló ezen felül szöveges gráf formátumban reprezentálja a logot.

```bash
$ git log --pretty=format:"%h %s" --graph
* 2bfb2ec Második commit.
* 81f766d Első commit.
```

Személy szerint, amit sűrűn használok az a következő:

```bash
$ git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
* 2bfb2ec - (11 days ago) Második commit. - Felhasználó Név (HEAD -> master)
* 81f766d - (11 days ago) Első commit. - Felhasználó Név
```

Ebben a formában látható, hogy mikor követték el a commitot, ki tette és a branchek közötti összefüggést `\|/` karakterekkel szimbolizálja.

A formátumban használt rövidítések:

* `%h` - rövidített commit hash,
* `%ar` - commit ideje, relatívan a parancs kiadásához viszonyítva,
* `%s` - a commit üzenete,
* `%an` - szerző neve,
* `%d` - branchek, HEAD megjelenítése.

Ilyen hosszú parancs ritkán megjegyzendő, a konfigurációs fájlba el lehet menteni új parancsként:
`nano ~/.gitconfig`

```txt
[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
```

Ezután használható a `git lg1` parancs ugyanazzal az eredménnyel, amit az előző kódrészlet mutatott be.

Ha tudjuk a keresendő commit szövegét, pl a parser modul módosításakor a `[parser]` prefix odakerül minden commit message elejére, akkor kereshetünk erre a szóra: `git log --grep parser`.

Ha csak egy bizonyos fájl módosításait tartalmazó commitokat szeretnénk látni, akkor pedig a `git log -- path/to/file` használható.

## Visszavonás

Amennyiben változtattunk egy fájlt és vissza szeretnénk állítani a legutolsó commit állapotára, akkor a következő megoldással élhetünk: `git restore [--staged] <file>...`.
Ahogyan a `status` üzenete is mutatja, ha hozzá van adva a fájl, akkor a `restore --staged 01-pelda.txt` `unstaged` állapotra változtatja az állapotát, és ezután a `restore 01-pelda.txt` pedig visszaállítja az utolsó commit állapotára.

Ha egy fájlt létrehoztunk a repositoryban, de még nem volt becommitálva, akkor `rm <file>` is megfelelő annak eltávolítására.
