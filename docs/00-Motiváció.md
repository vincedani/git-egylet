# Motiváció

Van egy jó ötletem, rávettem magam a kezdésre, írogatom a kis programomat, az első felmerülő kérdés pedig az, hogy *miért is kellene ezen felül szórakoznom a verziókezelővel*?
Amikor középiskolás voltam, egy infós versenyre készítettünk ketten egy játékot.
Abban az időben azt sem tudtam, hogy van verziókezelő szoftver és megkönnyíthetné az életemet.
Amikor a másik srác csinált valamit, azt átküldte nekem e-mailben tömörítve, frissítettem a projektet a dolgaival, majd visszaküldtem zip-ben az eredményt.
Ez így ment egy évig, visszagondolva elég szörnyű volt.

Nem is kell ennyire előre szaladni, nem szükséges a probléma felvetéséhez több ember.
Elég abba belegondolni, hogy leülök magam és elkezdek kódolgatni annak reményében, hogy előbb-utóbb lesz egy működő szoftver.
Írogatom és rájövök, hogy -- jó esetben -- egy dolgot kegyetlenül elszúrtam, mégis úgy lenne jó ahogy fél órával ezelőtt állt.
Az is egy megoldás, hogy rátenyerelek a CTRL Z párosra és várom a csodát, de ez nem túl hatékony.
A másik módszer, hogy időközönként kimentem egy backup mappába a projekt fájlokat és reménykedem, hogy ott lesz amit szeretnék.

Ebből a két példából is látszik, hogy valahogy jó lenne menedzselni a változásokat, viszont itt még nem esett sok szó a változások történelméről.

Tegyük fel, hogy a harmadik műszakbéli munkám kifizetődik, elkészül a kis szoftver, megveszi a sarki bolt a v1-et, hozzájuk került a bináris.
Közben fejlesztgetem tovább annak reményében, hogy másnak is el tudom adni, kicsinosítva talán a másik utcában a szerszámüzlet is megveszi.
Létrejön a v2, ami sokkal több funkcionalitással bír.

Ebben a pillanatban a sarki boltból telefonálnak, hogy ha bizonyos kvantumállapotok fennálnak akkor elszáll a program.
Szeretnék a javítást, hiszen azt ígértem nekik, hogy ha bármi gondjuk van, akkor megoldom.
Most kellene egy v1.1 is, viszont már v2 van és nem is tudom mi hogy állt a kódban a v1 idején akkor oda kell adnom nekik ingyen a v2-t lebutítva, mert az van meg.
Nem tudom kijavítani a régi verziót, mert a kód abban a formában *nincs meg*.

Abban az esetben, ha történelmet vezetünk a kód evolúciójáról, akkor az ilyen felmerülő problémákkal könnyebben megküzdhetünk.
Az imént említett időközönkénti kódállapot mentésénél egy hatékonyabb módszer lehet, ha csak a változást mentjük mindig el.
Például van egy 100 soros fájl, változtattam benne 2 sort.
Ezesetben az esetben elég tárhelypazarló módszernek tűnik, ha minden snapshot esetén 100 sort mentünk, ennél jobb megoldásnak tűnhet ha csak a változást mentjük el.
Ezzel tárhelyet spórolhatunk.

Ezt a változás-menedzsmentet tudják jól kezelni a mai verziókezelő szoftverek, amilyen a Git is.
Természetesen több technológia létezik és azoknak több megvalósítása, sőt lehet írni sajátot is, ha arra van igény.

Jelen projekt a Git-ről szól és arról a metodológiáról, amit képvisel.

Amikor én elkezdtem dolgozni másodéves egyetemistaként, azt sem tudtam mi a Git, hogy kell használni és időbe telt mire belejöttem.
Ezt az időt szeretném lerövidíteni *neked*.
