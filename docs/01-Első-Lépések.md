# Előkészületek

A veziókezelő (*Version Control System -- VCS*) egy szoftverrendszer, amely rögzíti a változásokat fájlokon az idő folyamán.
Programozói szemszögből ezek a fájlok programkódot tartalmaznak, de kb. *mindent is* lehet kezelni.
Minden változtatás az előző állapothoz képest kerül meghatározásra inkrementálisan, így könnyű az állapotok között váltani, visszavonni egy vagy több változtatást.

Amennyiben feltételezzük, hogy egy projekten több fejlesztő dolgozik, két fajta verziókezelő szoftvert különböztethetünk meg: elosztott és centralizált.
Centralizált esetben egy ún. *központi repository* létezik egy szerveren, minden fejlesztő onnan kéri el az aktuális állapotot és oda küldi be a módosításait.
Csak ez a központi állapot létezik.

Az elosztott verziókezelőkről pedig a projekt további része szól.

## Elosztott verziókezelés

Elosztott verziókezelés során is létezik egy központi repository, mely tárolja az állapotokat és a teljes *history*t.
Ebben az esetben a kliensek nem csak az aktuális állapotot kérik el a központi szervertől, hanem a teljes historyval együtt egy tükörképet kapnak meg.
Amennyiben a központi szerver elérhetetlenné válik bármilyen oknál fogva, a kliensek birtokában van minden állomány a veszteségmentes visszaállításhoz.

| ![Elosztott verziókezelő. Forrás: [1]](img/01-elosztott-verziokezelo.PNG) |
|:--:|
| *Elosztott verziókezelő. Forrás: [1]* |

## Mi az a Git?

Röviden: A Git egy elosztott verziókezelő szoftver.

Fentebb leírásra került, hogyan is működik egy verziókezelő, mit tárol.
A Git egy miniatűr fájlrendszerként képzeli el a saját életét, melyben állapotok folyamát tárolja.
Minden esetben, amikor egy állapotot mentünk, a Git az egész projektről készít egy snapshotot, viszont a nem változott fájlokat nem tárolja le újra.
Ezen fájlok az előző snapshot megfelelő fájljára mutatnak.

Így, amennyiben letöltjük a legújabb állapotot a szerverről (legyen ez most `Version 5`), akkor az tudja magáról, hogy milyen fájlok módosultak az előző állapothoz képest (`Version 4`), de azokról a fájlokról is van információja, melyek a két verzió között nem módosultak.
A 4-es verzió is tudja magáról, hogy milyen módosításokat tárol és milyen fájlok nem módosultak.
Ha ezt a láncot a legelső állapottól kezdve végig vezetjük az 5-ös verzióig, akkor megkapjuk a projekt jelenlegi állapotát.
A megoldás miatt viszont a jelenlegi állapot meghatározásához szükség van a teljes történetre.

| ![Változások tárolása az idő folyamán. Forrás: [1]](img/01-adattarolas.PNG) |
|:--:|
| *Változások tárolása az idő folyamán. Forrás: [1]* |

Jelen példában a `1. verziónál` hozzáadásra került három fájl (A, B, C).
A `2. verzió` két fájlt módosított, ami A1-el és C1-el van jelölve, viszont a B nem változott (referencia az előző verzióra).
A `3. verzió` csak a C1-et változtatta, míg a többit érintetlenül hagyta.
A `4. verzió` az A1 és B fájlokat változtatta meg, továbbá az `5. verzió` az B1-et és a C2-t.

A Git működik úgy is, ha nincs központi szerver, mivel minden szükséges fájl a lokális gépen rendelkezésre áll.
Ha a VPN épp nem szeretne működni vagy egy utazás közben is teljes funkcionalitással használható a verziókezelő (természetesen, ha a kollégáinkkal is meg szeretnénk osztani legújabb munkánkat, akkor kelleni fog hálózati kapcsolódás).

Azt is fontos megemlíteni, hogy a Git csak hozzáad információt, sosem töröl.
Ez azt jelenti, hogy ha egy fájlt (pl. `C3` az előző példában) törölni szeretnénk, akkor egy `Version 6` fog keletkezni, amely tartalmazza a `C3 törlése` információt.
Így, ha mégis kellene az a fájl, vagy egy részét szeretnénk felhasználni, akkor a `Version 5`-re váltva újra megtalálhatjuk azt.

## Telepítés

* Windows: A grafikus telepítőt [innen](https://git-scm.com/download/win) lehet letölteni.
* Linux: A telepítéshez használt parancsok [itt](https://git-scm.com/download/win) találhatók.

Jelen dokumentáció (és az azt követő videók) *Ubuntu 20.04 subsystem for Windows 10* operációs rendszerrel vannak bemutatva.

## Konfiguráció

A telepítés után pár dolog beállításra szorul.
Ezt a konfigurációt elég egyszer megtenni a telepítés után, de bármikor módosítható.

Mindennemű konfiguráció a `git config` parancs segítségével történik.
A jelenlegi beállításokat a `--list` kapcsolóval tudjuk megjeleníteni.

```bash
$ git config --list --show-origin
file:/etc/gitconfig     filter.lfs.clean=git-lfs clean -- %f
file:/etc/gitconfig     filter.lfs.smudge=git-lfs smudge -- %f
file:/etc/gitconfig     filter.lfs.process=git-lfs filter-process
file:/etc/gitconfig     filter.lfs.required=true
file:/home/felhasználó/.gitconfig user.name=Felhasználó Név
file:/home/felhasználó/.gitconfig user.email=email@cím.hu
...
```

### Rendszerszintű

A rendszerszintű konfiguráció a `/etc/gitconfig` fájlban található.
Tartalmazza azon beállításokat, melyek a rendszerben az összes repositoryra vonatkoznak.

### Felhasználó szintű

A felhasználó szintű konfiguráció a `~/.gitconfig` (vagy `~/.config/git/config`) fájlban található.
Ide helyezhetők olyan beállítások, mint a különleges felhasználói parancsok, vagy preferált szövegszerkesztő.

Ha ezt szeretnénk módosítani, akkor `git config --global` parancs után kell megadni a kívánt beállítást.

### Repository szintű

A repository `.git/config` fálja tartalmazza azokat a beállításokat, amelyek egy-egy projektre vonatkoznak.
Ilyen pl. ha az A projekten más e-mail címmel kell commitolni, mint a B-n.

Minden szint felülírja az előzőt, így ha a felhasználói konfigurációs fájlban az van benne, hogy a preferált szövegszerkesztő a `nano`, de a projekten külön be van állítva a `vi`, akkor a `vi` lesz megnyitva (és soha ki nem lesz belőle lépve...).

### Identitás beállítása

Ahhoz, hogy a telepítés után használható legyen a verziókezelő, a saját identitásunkat be kell állítani a következő parancsok segítségével.

```bash
$ git config --global user.name "Felhasználó Név"
$ git config --global user.email email@cím.hu
```

Minden commit-nak van egy szerzője, amit a kommitolás pillanatában a konfigurációs fájlból nyer ki a rendszer.

### Preferált szövegszerkesztő

Ha a Git-nek nincs külön megmondva, hogy milyen szövegszerkesztőt használjon (hogy mire, később...), az operációs rendszer alapértelmezett szövegszerkesztőjét használja.

Ha ez nem felel meg az érdeklődésednek, ezt is be lehet állítani.

```bash
$ git config --global core.editor code # Visual Studio Code.
$ git config --global core.editor nano # Nano.
```

### Fájlok figyelmen kívül hagyása

Tegyük fel, hogy egy C++ programot írunk, mely fordítás után az `a.out`-ban testesül meg.

A forrást igen, viszont a binárist egyáltalán nem szeretnénk verziókezelni, hiszen a C++ forrásokból mindig elő tudjuk állítani azt és a forráskód változása után a bináris is mindig változik.
Ezen okoknál fogva meg szeretnénk mondani a Git-nek, hogy ezt a fájlt ne vegye figyelembe.

Erre szolgál a `.gitignore` fájl, ami szintén lehet globális (`~/.gitignore`) vagy lokális, a projekthez kapcsolódó.
A projekthez kapcsolódó ignore fájlt be kell kommitálni.

Példák.

```bash
.vscode # VS Code által létrehozott mappa.
.antlr

a.out
build
*.yaml # Minden yaml kiterjesztésű fájl.
```

Jelen projekt `.gitignore` fájlja a dokumentum írása pillanatában:

```txt
*.txt
*.pdf
```

Miért?
A `pandoc` eszközzel Markdown formátumból PDF-et tudok előállítani, így mindennemű pdf-et nem szeretnék megtartani (ugyanazon indoknál fogva, mint az `a.out`).
A különböző eszmefuttatásaimat pedig `txt` fájlokba írom bele mielőtt a feledés homálya szállna rájuk.
Azokat sem szeretném kommitálni mielőtt kiforrott gondolat lesz belőlük.
