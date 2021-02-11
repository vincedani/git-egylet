# Alapok

A Git alapvetően három állapotot különböztet meg egy fájl szemszögéből tekintve: `modified` (módosított), `staged` (módosított és már elfogadott -- biztosan van jobb frázis) és `committed` (komittált).

* `modified`: a fájl módosult, de még nem történtek lépések afelé, hogy a következő commitba bekerüljön.
* `staged`: a fájl módosult és meg lett jelölve az állapot, hogy a következő commitba bekerüljön. Abban az esetben ha egy `staged` fájlt tovább módosítunk, akkor az új módosítások már a `modified` állapotban látszanak (1 fájl 2 állapottal).
* `committed`: A `staged` fájl a commit parancs hatására bekerül a repositoryba és ott is marad.

Az állapotokkal összhangban három különböző szekciót különböztetünk meg: `repository` (.git mappa), `staging area` és `working directory`.

| ![Állapotok. Forrás: [1]](img/02-szekciok.PNG) |
|:--:|
| *Állapotok. Forrás: [1]* |****

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
Egyel több fájl került bele, vagy egyel kevesebb?
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

Van egy hash, ami azonosítja az egységet, van neki egy szezője és egy időpont amikor elkészült.

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

* y: módosítás hozzáadása,
* n: módosítás mellőzése,
* q: kilépés, az eddig végrehajtott hozzáadások megmaradnak,
* a: módosítás és minden ami következik hozzáadása,
* d: módosítás és minden ami következik mellőzése,
* g: módosítás választása,
* j: módosítás döntés nélkül hagyása, és a következő el nem döntöttre ugrás,
* J: módosítás döntés nélkül hagyása, és a következőre ugrás,
* k: módosítás döntés nélkül hagyása, és az előző el nem döntöttre ugrás,
* K: módosítás döntés nélkül hagyása, és az előzőre ugrás,
* s: jelenlegi módosítás kisebb darabokra tördelése,
* ?: help kiíratása

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

TODO.
