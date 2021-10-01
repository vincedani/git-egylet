# Prancs jegyzék

Az alábbi oldal az eddig áttekintett git parancsok rövid leírását, linket az érintett szekcióra és linket a hivatalos dokumentációra tartalmaz.

* [Add](#Add)
* [Branch](#Branch)
* [Checkout](#Checkout)
* [Clean](#Clean)
* [Commit](#Commit)
* [Config](#Config)
* [Fetch](#Fetch)
* [Init](#Init)
* [Log](#Log)
* [Merge](#Merge)
* [Pull](#Pull)
* [Push](#Push)
* [Rebase](#Rebase)
* [Remote](#Remote)
* [Stash](#Stash)
* [Status](#Status)

## Git parancsok

### Add
A `git add` módosított és új fájlok hozzáadására jó.

Az [Alapok](02-Alapok.md#commitok) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-add) található.

### Branch
A `git branch` branchek létrehozására, listázására és törlésére használható.

A [Branch menedzsment](04-Branch-menedzsment.md) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-branch) található.

### Checkout
A `git checkout` branchek közötti váltásra vagy a branch változtatásainak visszaállítására jó.

A [Branch menedzsment](04-Branch-menedzsment.md#Váltás) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-checkout) található.

### Clean
A `git clean` a nem verziókövetett fájlok törlésére szolgál.

A [Branch menedzsment](04-Branch-menedzsment.md#clean-állapot-létrehozása) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-clean) található.

### Commit
A `git commit` a módosítások véglegesítésére szolgál.

Az [Alapok](02-Alapok.md#commitok) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-commit) található.

### Config
A `git config` a Konfiguráció lekérdezésére és beállítására használható.

Az [Első lépések](docs/01-Első-Lépések.md#Konfiguráció) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-config) található.

### Fetch
A `git fetch` a távoli repository változtatásainak letöltésére szolgál.

A [Távoli repositoryk](03-Távoli-repository.md#kód-le--és-feltöltése) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-fetch) található.

### Init
A `git init` új repository létrehozására szolgál.

Az [Alapok](02-Alapok.md#Init) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-init) található.

### Log
A `git log` a commitok listázására való.

Az [Alapok](02-Alapok.md#elrontottam) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-log) található.

### Merge
A `git merge` fejlesztési ágak összevonására alkalmas.

A [Branch menedzsment](04-Branch-menedzsment.md#Merge) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-merge) található.

### Stash
A `git stash` az aktuális változtatások stash-ben elmentésére, vagy a korábban stash-ben elhelyezett változtatás visszaállítására szolgál.

A [Branch menedzsment](04-Branch-menedzsment.md#állapotok-kezelése) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-stash) található.

### Status
A `git status` a repository aktuális állapotának lekérdezésére való.

Az [Alapok](02-Alapok.md#Init) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-status) található.

### Pull
A `git pull` a távoli repository letöltésére szolgál.
Egy `git fetch` és `git merge FETCH_HEAD` kombinációja.

A [Távoli repositoryk](03-Távoli-repository.md#kód-le--és-feltöltése) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-pull) található.

### Push
A `git push` a lokális commitok távoli repository-ba feltöltésére szolgál.

A [Távoli repositoryk](03-Távoli-repository.md#kód-le--és-feltöltése) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-push) található.

### Rebase
A `git rebase` a branch commitjait a másik branchre egyessével alkalmazza.

A [Branch menedzsment](04-Branch-menedzsment.md#Rebase) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-rebase) található.

### Remote
A `git remote` távoli repositoryk hozzáadására és listázására.

A [Távoli repositoryk](03-Távoli-repository.md##távoli-repository-kezelése) fejezetben ismerkedtünk meg vele.

Hivatalos leírás [itt](https://git-scm.com/docs/git-remote) található.
