# Távoli repositoryk

Verziókezelést egyedül is lehet művelni, viszont akkor kerül legtöbbször előtérbe, amikor csapatban dolgozunk.
Ahhoz viszont, hogy együtt tudjunk működni másokkal, ismerni kell a távoli repositorykat és azokkal történő műveleteket.

Távoli (`remote`) repository esetében a projekt egy verziója valahol egy szerveren megtalálható (ld. első lépések/"Elosztott verziókezelő." ábra).

Nem kötelezően csak egy szerveren élhet, több remote kezelése is lehetséges egyidejűleg. példa:
Egy projekt open-source -- a forráskód teljes mértékben megtalálható az interneten -- ciklikus releasek történnek, amikor is a hónap utolsó munkanapján megtörténik a kód megosztása.
A fejlesztés viszont egy privát repositoryban történik, amit csak bizonyos fejlesztői réteg ér el, ilyen esetben lehet egy `public` és egy `private` távoli repository, és a különböző műveleteket a priváton végzik el.

A fejlesztői csapat minden tagja rendelkezik egy saját, letöltött repositoryval és időközönként feltölti a változtatásait a központi szerverre.

## Távoli repository kezelése

Alapvetően két módon találkozhatunk távoli repositorykkal.
Az első esetben egy lokálisan létrehozott repositoryhoz hozzáadunk egy távolit, majd ide töltjük fel a már lokálisan létező kódot.
A másik irány pedig ennek a fordítottja, valahol már létezik a repository és mi letöltjük magunkhoz.

Először folytassuk az előzőleg létrehozott lokális repositorynk gondozását.
Ha fel szeretnénk tölteni a kódbázist valahova, először is meg kell mondani a célt.
Amennyiben nincs még ilyen, GitHubon egy új repository létrehozásával lehet a problémát orvosolni.

```bash
# git remote add <név> <url>
$ git remote add origin https://github.com/<user>/tmp.git
$ git remote -v
origin  https://github.com/<user>/tmp.git (fetch)
origin  https://github.com/<user>/tmp.git (push)
```

Az `add` parancs kiadása után már látható is a távoli repository, mely `origin` névre lett keresztelve.
Ha ez nem tetszik, más néven szeretnénk használni, akkor nem kell törölni és újra hozzáadni, át is lehet nevezni.

```bash
# git remote rename <régi név> <új név>
$ git remote rename origin uj_nev
$ git remote -v
uj_nev  https://github.com/<user>/tmp.git (fetch)
uj_nev  https://github.com/<user>/tmp.git (push)
```

El is lehet távolítani a kapcsolatot a lokális repository és az éterben létező párja közül.

```bash
# git remote remove <név>
$ git remote remove uj_nev
$ git remote -v
```

A `remove` parancs után nem lehetséges kód fel és letöltése, hiszen nincs kapcsolódási pont.

A másik irányt pedig a `clone` parancs biztosítja.
Ebben az esetben már egy létező repositoryt töltünk le a teljes historyval, minden committal együtt.

```bash
$ git clone https://github.com/vincedani/git-egylet.git
Cloning into 'git-egylet'...
remote: Enumerating objects: 35, done.
remote: Counting objects: 100% (35/35), done.
remote: Compressing objects: 100% (24/24), done.
remote: Total 35 (delta 12), reused 28 (delta 8), pack-reused 0
Unpacking objects: 100% (35/35), 107.40 KiB | 193.00 KiB/s, done.

$ cd git-egylet
$ git remote -v
origin  https://github.com/vincedani/git-egylet.git (fetch)
origin  https://github.com/vincedani/git-egylet.git (push)
```

A `clone` parancs alapesetben a repository nevével megegyező mappába másolja a fájlokat, ha ezt felül szeretnénk írni, akkor megadható a mappa neve `git clone <url> [<mappa név>]`.

## Kód le- és feltöltése

Fontos megjegyezni, hogy különböző felhasználók különböző jogosultságokkal rendelkezhetnek a távoli repositoryk esetében.
Ilyen megszorítás lehet például a `master` vagy `main` branchek írásjoga, vagy a teljes repository írásvédettsége.

Minden munka, ami a lokális repositorynkon történt, lokális marad amíg expliciten nem töltjük fel a szerverre.
Ilyen állapotban van az eddig létrehozott két commit is:

```bash
$ git lg1
* 2bfb2ec - (2 weeks ago) Második commit. - Felhasználó Név (HEAD -> master)
* 81f766d - (2 weeks ago) Első commit. - Felhasználó Név
```

**Feltöltés.**
A commitok feltöltésére a `push` parancs szolgál, bővebben a `git push <remote> <branch>` (a branchekről később lesz szó).
Íly módon a lokális `<branch>`-et feltölti a megjelölt szerverre.

```bash
$ git push origin master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 877 bytes | 109.00 KiB/s, done.
Total 6 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To https://github.com/<user>/tmp.git
 * [new branch]      master -> master
```

Amennyiben autentikációhoz kötött a feltöltés/letöltés, a parancssorban meg lehet adni a felhasználónevet és a jelszót.
Az outputból látható, hogy egy új branch került feltöltésre, melynek neve `master`.
Ha a GitHub készít egy `init` commitot, akkor a fő branch valószínűleg a `main` lesz politikai okokból, viszont jelen git verzió parancssorból még a `mastert` használja.
Az `lg1` parancs kiadása után látható, hogy már két referencia is van a "Második commit."-ra: a lokális és az `origin` master branche is ide mutat.

```bash
$ git lg1
* 2bfb2ec - (2 weeks ago) Második commit. - Felhasználó Név (HEAD -> master, origin/master)
* 81f766d - (2 weeks ago) Első commit. - Felhasználó Név
```

Módosítsuk a fájlt és készítsünk egy "Harmadik commit."-ot.

```bash
$ git lg1
* 2a740e2 - (3 seconds ago) Harmadik commit. - Felhasználó Név (HEAD -> master)
* 2bfb2ec - (2 weeks ago) Második commit. - Felhasználó Név (origin/master)
* 81f766d - (2 weeks ago) Első commit. - Felhasználó Név
```

Látható, hogy a lokális `master` branch a `2a740e2` commitra mutat, míg az `origin/master` maradt ott, ahol eddig volt.
Említve volt már, hogy az elvégzett munka csak akkor kerül publikálásra, ha az expliciten kérve van, így ha az a cél, hogy a harmadik commit is fenn legyen GitHubon, akkor újra ki kell adni a `git push origin master` parancsot.

**Letöltés.**
Tegyük fel, hogy `clone`oztam a projektet, viszont még csak akkor amikor a "Második commit." volt fenn és a harmadikról nem is tudok.
Ahhoz, hogy hozzájussak az azóta feltöltött információkhoz, újra meg kell keresnem a szervert és letölteni róla a hiányzó fájlokat.
Ez kétféle módon tehető meg: `fetch` és `pull`.
A `fetch` parancs letölti a lokálisan meg nem lévő fájlokat, míg a `pull` ezen felül végrehajt egy `merge` utasítást is (hogy az mit jelent, később).

Legtöbb esetben a `pull` elégséges megoldás, a `04-Branch-menedzsment` dokumentumban találhatók esettanulmányok olyan esetekre, amikor további lépések szükségesek.

```bash
$ git pull
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 271 bytes | 6.00 KiB/s, done.
From https://github.com/<user>/tmp
   2bfb2ec..2a740e2  master     -> origin/master
Updating 2bfb2ec..2a740e2
Fast-forward
 01-pelda.txt | 1 +
 1 file changed, 1 insertion(+)
```

A `pull` parancs hatására az új "Harmadik commit." is letöltődött és egy összegzést is kiír, hogy mi változott az újonnan letöltött commitokban.
