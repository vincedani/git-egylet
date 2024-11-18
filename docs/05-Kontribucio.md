# Kontribúció

[git-egylet#11](https://github.com/vincedani/git-egylet/issues/11)

## Fork

A Git egyik erőssége, hogy támogatja az osztott fejlesztést. Ennek egyik fontos eleme a _fork_, ami lehetővé teszi, hogy egy meglévő repository másolatát hozzuk létre, amely teljes mértékben elkülönül az eredetitől. Ez különösen hasznos, ha nyílt forráskódú projektekhez szeretnénk hozzájárulni, vagy ha a saját fejlesztéseinket egy már létező projekt alapján szeretnénk végezni.

A forkok elsősorban _hosting platformokon_ (GitHub, GitLab, Bitbucket stb.) jelennek meg, és a következőket nyújtják:

- Saját másolat a kódbázisról, amiben tetszőlegesen dolgozhatsz.
- Az eredeti repository változásai könnyen integrálhatók.
- A forkolt repositoryban történt változások könnyen integrálhatóak az eredeti repositoryba (_pull requestek_).

### Fork létrehozása

Egy projekt forkolása egyszerű folyamat, amely általában a hosting platform webes felületén történik. Vegyük példaként a GitHubot:

1. **Keresd meg a forkolni kívánt repositoryt.**
2. **Kattints a "Fork" gombra.**
   - A gomb általában az oldal jobb felső részén található.
3. **Válaszd ki a célfiókot.**
   - Ha több GitHub-fiókod vagy szervezeted van, megadhatod, hova szeretnéd forkolni a repositoryt.
4. **Várj a másolat elkészüléséig.**

Ezután a fork az általad választott fiókhoz vagy szervezethez tartozik. A webes felületen láthatod, hogy ez egy fork, mert megjelenik az "Forked from `<eredeti-repo>`" felirat.

### Fork használata

Miután létrejött a fork, a helyi gépen való munkához klónozd le a forkolt repositoryt:

```bash
$ git clone https://github.com/<felhasználó>/<forkolt-repo>.git
$ cd <forkolt-repo>
```

Ha a forkot egy másik repository alapján hoztad létre, érdemes az eredeti repositoryt (_upstream_) is hozzáadni, hogy nyomon tudd követni annak változásait:

```bash
$ git remote add upstream https://github.com/<eredeti-felhasználó>/<eredeti-repo>.git
$ git remote -v
origin    https://github.com/<felhasználó>/<forkolt-repo>.git (fetch)
origin    https://github.com/<felhasználó>/<forkolt-repo>.git (push)
upstream  https://github.com/<eredeti-felhasználó>/<eredeti-repo>.git (fetch)
upstream  https://github.com/<eredeti-felhasználó>/<eredeti-repo>.git (push)
```

Miután a fork naprakész, dolgozhatsz saját ágaidon, commitolhatsz, pusholhatsz stb. (lásd az előző fejezeteket).

### Fork vs. branch

Bár a fork és a branch hasonló célt szolgálhat (elkülöníteni a fejlesztéseket), van néhány fontos különbség:

| **Tulajdonság**         | **Fork**                                                                | **Branch**                                          |
| ----------------------- | ----------------------------------------------------------------------- | --------------------------------------------------- |
| **Meghatározás**        | Egy teljesen különálló repository másolat.                              | Egy meglévő repository része.                       |
| **Hol jön létre?**      | Hosting platformokon (pl. GitHub, GitLab).                              | Lokálisan vagy a távoli tárolóban.                  |
| **Cél**                 | Egy projekt elkülönített fejlesztése másik repositoryban.               | A meglévő kódbázison belüli fejlesztés.             |
| **Hozzáférés**          | Csak az a felhasználó/szervezet fér hozzá, aki a forkot létrehozta.     | A repository minden hozzájárulója számára elérhető. |
| **Upstream kapcsolat**  | Az upstream repository változásait manuálisan kell szinkronizálni.      | Automatikusan része az eredeti repositorynak.       |
| **Kód megosztása**      | Pull requesteken keresztül történik az eredeti repositoryval.           | Commit és merge segítségével az alapághoz.          |
| **Alkalmazási terület** | Ha a fejlesztéshez nincs közvetlen hozzáférés az eredeti repositoryhoz. | Ha a fejlesztés az eredeti repository része.        |
| **Karbantartás**        | Az upstream változásokat kézzel kell követni és merge-elni.             | Az upstream változások automatikusan elérhetők.     |

#### Mikor használj forkot?

- Ha egy nyílt forráskódú projekthez szeretnél hozzájárulni, de nincs írási hozzáférésed az eredeti repositoryhoz.
- Ha egy projektet szeretnél elágaztatni és új irányban fejleszteni (például saját verzió készítése).
- Ha a változtatásaid szeretnéd, hogy privátok maradjanak az eredeti repositoryhoz képest.

#### Mikor használj branchet?

- Ha csapaton belül dolgozol, és hozzáférsz az eredeti repositoryhoz.
- Ha kisebb, ideiglenes módosításokat szeretnél elkülöníteni az alapágtól (pl. hibajavítás, új funkció fejlesztése).
- Ha gyors integrációra van szükség a fő projekttel.

### Forkok jelentősége

A nyílt forráskódú közösségekben a fork kulcsfontosságú szerepet játszik. A közreműködők:

- Forkolják az eredeti repositoryt.
- Dolgoznak saját ágaikon, anélkül, hogy az eredeti kódbázist érintenék.
- Pull requestet nyújtanak be az eredeti repository fejlesztőinek.

Ez a folyamat biztosítja, hogy az eredeti repositoryban csak a jóváhagyott, ellenőrzött módosítások jelenjenek meg.

## Pull Request (PR) / Merge Request (MR)

A **Pull Request** (GitHub) és a **Merge Request** (GitLab) egyaránt a kódbázis módosításainak integrálására szolgáló eszközök. Ezek lehetővé teszik a fejlesztők számára, hogy módosításokat javasoljanak, ellenőrizzék azokat, és adott esetben egyesítsék őket az eredeti kódbázissal.

## Mi az a Pull/Merge Request?

Egy PR/MR egy dokumentált kérés arra, hogy a módosításaid (branch, fork stb.) kerüljenek be az eredeti kódbázisba. Ez általában tartalmazza:

- A módosítások részletes leírását.
- A kapcsolódó kódváltoztatásokat (diff-ek formájában).
- A változtatások okát és a probléma magyarázatát.
- Esetlegesen automatizált teszteredményeket vagy megjegyzéseket.

## Egy Pull Request lépései

1. **Branch vagy fork létrehozása**

   - Az új fejlesztés vagy javítás külön ágban történik.
   - Ha nincs hozzáférésed az eredeti repositoryhoz, forkot használsz.

2. **Változtatások végrehajtása**

   - A fejlesztő elvégzi a szükséges módosításokat, majd commitolja őket a branch-be.

3. **Pull/Merge Request nyitása**

   - A PR/MR-t a platformon (GitHub, GitLab stb.) keresztül nyitod meg.
   - Megadod a leírást, a módosítás célját és minden kapcsolódó részletet.

4. **Kódellenőrzés (_code review_)**

   - Más fejlesztők, tesztelők vagy a projekt adminisztrátorai átnézik a kódot.
   - Megjegyzéseket írhatnak, javaslatokat tehetnek, vagy visszautasíthatják a kérést.

5. **Tesztelés**

   - Automatizált CI/CD folyamatok ellenőrzik, hogy a változtatások nem törik meg a kódot.
   - Szükség esetén manuális tesztelés is történhet.

6. **Véglegesítés és egyesítés (_merge_)**
   - Ha a változtatások megfelelnek, a PR/MR-t jóváhagyják és összevonják az alapággal.

## Miért fontos a Pull/Merge Request?

- **Együttműködés**: A PR/MR lehetőséget biztosít arra, hogy a csapat többi tagja átnézze a változtatásokat.
- **Minőségbiztosítás**: A kódellenőrzés és a tesztelés segít az esetleges hibák feltárásában.
- **Dokumentáció**: Minden változtatás egyértelműen követhető, dokumentált és visszakövethető.

## Kulcsfogalmak

| **Fogalom**           | **Magyarázat**                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Diff**              | A változtatások megjelenítése soronként (pl. mi került hozzáadva vagy eltávolítva).                                                  |
| **Code Review**       | Egy vagy több fejlesztő által végzett ellenőrzés a változtatások minőségének biztosítása érdekében.                                  |
| **Approval**          | Az ellenőrzés során adott jóváhagyás, hogy a változtatások megfelelnek a követelményeknek.                                           |
| **Conflict**          | Akkor fordul elő, ha egy másik ág változásai ütköznek a PR/MR tartalmával. Ezeket kézzel kell feloldani.                             |
| **Squash Merge**      | Egy egyesítési stratégia, amely egyetlen commitba tömöríti az összes változtatást.                                                   |
| **Rebase**            | Az ág újraalapozása egy másik ág alapján. Báziscsere.                                                                                |
| **CI/CD Integration** | A Continuous Integration/Continuous Deployment folyamatok automatikusan ellenőrzik a PR/MR kompatibilitását (pl. tesztek futtatása). |

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
