# Kontribúció

[git-egylet#11](https://github.com/vincedani/git-egylet/issues/11)

## Fork

[git-egylet#9](https://github.com/vincedani/git-egylet/issues/9)

## Pull / Merge Request

[git-egylet#10](https://github.com/vincedani/git-egylet/issues/10)

## Irányelvek

Minden projekt más, különböző emberek tartják őket karban, így a kontribúciós irányelvek is mások lehetnek projektenként.
Legtöbbször a projekt gyökér könyvtárában található `CONTRIBUTING.md` formalizálja hogyan kell történnie az új elemek hozzáadásának.
A formalitás mértéke nagyban függ a fejlesztők számától is, minél többen fejlesztenek egy adott projektet, annál több konfliktus lép fel a kódban, melyet menedzselni kell.
Ezen felül még fontos megemlíteni a különböző írási jogosultságokat: Esetleg csak a projekt fenntartója – maintainer – mergelhet?
Vagy peer-review után bárki?

### Commit

Mielőtt egy projektbe változtatást küldenénk, érdemes a commitot újra átnézni.
Igényesség hiányát mutatja pl. amennyiben a sorok végén whitespace karakterek maradnak, melyet a `git diff --check` parancs segítségével lehet ellenőrizni.
A konzolon pirossal jelzi azokat a helyeket, ahol whitespace karakterek vannak a sorok végén.

Továbbá a commitot leíró üzenetnek is megvan a formája, melyet előszeretettel használnak:

```txt
Cím (max 50 karakter)

Részletes leírás.
```

Példa a Pro Git könyvből:

```txt
Capitalized, short (50 chars or less) summary

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of an email and the rest of the text as the body. The blank
line separating the summary from the body is critical (unless you omit
the body entirely); tools like rebase will confuse you if you run the
two together.

Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
or "Fixes bug." This convention matches up with commit messages generated
by commands like git merge and git revert.

Further paragraphs come after blank lines.

- Bullet points are okay, too

- Typically a hyphen or asterisk is used for the bullet, followed by a
  single space, with blank lines in between, but conventions vary here

- Use a hanging indent
```

Egysoros log esetén csak a cím látszik, amennyiben az felkeltette az érdeklődést, meg lehet tekinteni a részleteket is.
Törekedni kell olyan leírást készíteni, melyből más fejlesztők is megérthetik mit tartalmaz a commit anélkül, hogy megnéznék a tényleges tartalmát.

A következő ajánlás, hogy minden commit logikailag egybefüggő módosításokat tartalmazzon.
Amennyiben egy commit több probléma megoldását tartalmazza, akkor az esetleges visszaállítása erőforrásigényes feladat – ha csak egy részét szeretnénk használni, szét kell bontani azt.
Az egyes commitoknak külön-külön is meg kell állniuk a helyüket (pl. egy commit a logolás hozzáadása, vagy egy új funkcionalitás bevezetése).

## Review folyamat

[git-egylet#12](https://github.com/vincedani/git-egylet/issues/12)
