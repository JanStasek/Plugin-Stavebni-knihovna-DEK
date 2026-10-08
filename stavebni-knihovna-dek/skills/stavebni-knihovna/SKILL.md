---
name: stavebni-knihovna
description: Práce se Stavební knihovnou DEK (DEK Building Library) – vyhledání a porovnání skladeb konstrukcí a systémů (střechy, obvodové a vnitřní stěny, podlahy, stropy), stavebních materiálů a výrobků, jejich technických parametrů (vrstvy, tloušťka, součinitel prostupu tepla U, tepelný odpor R, požární odolnost, spolehlivost hydroizolace) a environmentálních dat EPD (GWP, fáze životního cyklu A1–D). Použij vždy, když uživatel navrhuje nebo posuzuje stavební konstrukci, hledá skladbu, materiál, náhradu materiálu, výrobek DEK/DEKTRADE, uhlíkovou stopu nebo EPD stavebního výrobku – i když knihovnu výslovně nejmenuje. Also use for English queries about building assemblies, construction materials, U-values, fire ratings or EPD data from DEK.
---

# Stavební knihovna DEK

Data vždy čerpej z nástrojů MCP serveru `stavebni-knihovna-dek`. Nikdy si
nevymýšlej skladby, parametry, ID ani hodnoty EPD z vlastní paměti. Když
nástroj nic relevantního nevrátí, řekni to a navrhni jiné formulace.

## Nástroje

| Nástroj | K čemu |
|---|---|
| `searchConstructions` | Vyhledá skladby/systémy (`action=items`) nebo jejich katalogy (`action=catalogues`) |
| `searchMaterials` | Vyhledá materiály/výrobky nebo jejich katalogy |
| `listItemsIds` | Vrátí ID všech položek v katalogu (`type` = `skladby` / `materialy`, `katalog` = ID) |
| `getConstructionById` | Detail skladby: vrstvy, alternativy, U, R, požární odolnost, poznámky, tipy, odkazy |
| `getMaterialById` | Detail materiálu: výrobce, tloušťky, ETIM, přiřazené skladby, dostupnost EPD |
| `listEpdProducts` | Přehled všech výrobků s daty EPD |
| `getEpdByProductId` | Detailní data EPD jednoho výrobku |
| `aiItemPricing` | Základní technické informace o skladbě a položky cenové soustavy ÚRS pro její ocenění |


## Vyhledávání

- Vyhledávání kombinuje fulltext a sémantiku. Pište dotazy česky a věcně
  („plochá střecha s PVC fólií na trapézovém plechu“), ne jen kódy.
- `ratio` (0–1, výchozí 0,3): pro přesné názvy a kódy výrobků sniž k 0;
  pro popisné dotazy („zateplení dřevostavby s difuzně otevřenou fasádou“)
  zvyš k 0,6–0,8.
- `limit` výchozí 10; pro porovnání stačí 5–10 a pro přehled katalogu
  použij `action=catalogues`.
- Výsledky obsahují `matchScore`. Pracuj s nejrelevantnějšími a vždy si
  načti detail, než uvedeš jakékoli parametry. Krátký popis z vyhledávání
  na parametry nestačí.
- **Hledej víc formulacemi.** Jeden dotaz často nevrátí všechny vhodné
  položky. Před výběrem kandidátů polož 2–4 dotazy, které se liší:
  - obecností („plochá střecha PVC“ i plný popis s požadavky),
  - materiálem a variantou (u střech např. izolace EPS / PIR / minerální
    vata, kotvená / přitížená / lepená fólie),
  - hodnotou `ratio` (jednou nízkou, jednou vysokou).
  Vždy polož i aspoň jeden dotaz s názvoslovím systémů DEK, jinak se
  hlavní systémové skladby často nenajdou: „DEK Střecha“ / „DEKROOF“,
  „DEK Obvodová stěna“, „DEK Vnitřní nosná stěna“, „DEK Příčka“,
  „DEK Fasádní systém“ / „DEKTHERM“, „DEK Podlaha“ / „DEKFLOOR“,
  „DEK Strop“ (např. „DEKROOF kotvená fólie PVC“).
  Výsledky slouč, odstraň duplicity podle `itemId` a detail načti jen
  u nejslibnějších kandidátů. Když po několika formulacích nic vhodného
  nenajdeš, řekni to a uveď, co jsi zkoušel.

## Postup u skladeb

1. Ujasni typ konstrukce, druh stavby, požadavky (U, požární odolnost,
   akustika, pochozí/zelená střecha, podlahové topení) a omezení
   (celková tloušťka, nosná konstrukce). Chybí-li zásadní údaj, zeptej se
   jednou; jinak hledej a předpoklad uveď.
2. `searchConstructions` několika formulacemi (viz Vyhledávání) → vyber
   1–3 kandidáty → `getConstructionById`.
3. Předlož každou variantu takto:
   - název a `productCode`,
   - vrstvy v pořadí, jak je vrací API, s tloušťkami (a zmínkou o
     dostupných alternativách vrstev, pokud jsou),
   - celková tloušťka, U a R v **základní konfiguraci** (uveď to výslovně –
     při změně tloušťky izolace se hodnoty mění),
   - požární odolnost (`fireRatingsSummary`), spolehlivost hydroizolace
     dle ČHIS, pokud je k dispozici,
   - podstatné poznámky a tipy (`notes`, `tips`),
   - odkazy: `bimLibraryUrl`, technické listy vrstev, výpočet
     v DEKSOFT Tepelná technika 1D (`thermalTransCalcUrl`), video.
     U jiné tloušťky izolace U a R sám nepřepočítávej; odkaž na výpočet
     přes `thermalTransCalcUrl`.
4. Pro porovnání více skladeb použij tabulku.

## Postup u materiálů

`searchMaterials` → `getMaterialById`. Uveď výrobce, značku, dostupné
tloušťky, katalog, odkaz do knihovny a skladby, ve kterých je výrobek
použit (`assignedConstructions`). Při hledání náhrady materiálu ve skladbě
vycházej z `alternatives` u dané vrstvy.

## EPD a uhlíková stopa

1. U výrobku zkontroluj `epdData: true`; pro přehled použij `listEpdProducts`.
2. `getEpdByProductId` vrací základní hodnoty pro deklarovanou jednotku.
3. **Přepočtové faktory:** pokud `ConversionFactorsByThickness.considered`
   je `true`, vynásob základní hodnoty faktorem pro zvolenou tloušťku
   (u `oneValueForAllStages` jedním faktorem pro všechny fáze, jinak
   faktorem příslušné fáze). Výpočet ukaž. Pokud `considered` je
   `false`, hodnoty nepřepočítávej.
4. Vždy uveď deklarovanou jednotku, normu EPD, zahrnuté fáze životního
   cyklu (moduly, které nejsou zahrnuty, nesčítej ani nedoplňuj), platnost
   (`validTo`) a programového operátora. Upozorni na EPD s prošlou platností.
5. Při sčítání za celou skladbu jasně odliš, které vrstvy EPD mají a které ne.
6. **Jednotky ukazatelů:** API je u ukazatelů nevrací. Uváděj je podle
   ČSN EN 15804+A2 (tabulka níže) a jednou poznamenej, že jde o jednotky
   dle normy, ne z dat EPD. Platí jen pro EPD podle EN 15804+A2; u jiné
   normy (`usedStandard`) jednotky neuváděj a řekni, že je výstup neobsahuje.

| Klíč v API | Ukazatel | Jednotka (na deklarovanou jednotku) |
|---|---|---|
| `GWPTotal`, `GWPFossil`, `GWPBiogenic`, `GWPLuluc` | GWP | kg CO₂ ekv. |
| `ozoneDepletionPotential` | ODP | kg CFC-11 ekv. |
| `acidificationPotencial` | AP | mol H⁺ ekv. |
| `freshwaterEP` | EP-sladká voda | kg P ekv. |
| `seawaterEP` | EP-moře | kg N ekv. |
| `soilEP` | EP-terestrické | mol N ekv. |
| `groundLevelOzone` | POCP | kg NMVOC ekv. |
| `mineralsMetalsADP` | ADP-minerály a kovy | kg Sb ekv. |
| `fossilFuelsADP` | ADP-fosilní zdroje | MJ |
| `waterScarcityPotential` | WDP | m³ světového ekv. odebrané vody |
| `potentialDiseasePM` | PM | výskyt onemocnění |
| `isotopeU235Exposure` | IRP | kBq U235 ekv. |
| `ETPfw` | ETP-fw | CTUe |
| `HTPc`, `HTPnc` | HTP-c, HTP-nc | CTUh |
| `SQP` | SQP | bezrozměrné |
| `consPERE` … `consPENRT`, `consRSF`, `consNRSF`, `exportedEnergy` | energie, paliva | MJ |
| `consSM`, `constructionUnitsReuse`, `materialsRecycling`, `materialsEnergyRecovery` | materiály | kg |
| `consFW` | čistá voda | m³ |
| `hazardousWasteDisposed`, `otherWasteDisposed`, `radioactiveWasteDisposed` | odpady | kg |

## Ocenění skladby (ÚRS)

Když uživatel chce skladbu ocenit, sestavit rozpočet nebo výkaz výměr,
použij `aiItemPricing` s ID skladby z vyhledávání nebo detailu.

**Struktura výstupu:** `item.layers[]` jsou vrstvy skladby; každá má
`pricing[]` – jednu sadu položek pro každou dostupnou tloušťku
(`thickness`) – a v ní `p9Items[]` s položkami ÚRS (`code`,
`description`, `unit`, `price`, `currency`) a množstvím na jednotku
skladby (`quantity.value`, obvykle na 1 m²). `commercialP9` u položky
jsou ceníkové položky DEK. `item.pricing.pricingDescription` popisuje,
co cena zahrnuje a co ne. Výstup bývá dlouhý – zpracuj ho celý.

- **Výběr tloušťky:** z každé vrstvy použij jen jednu sadu `pricing` –
  pro tloušťku ze základní konfigurace skladby (`defaultThickness`
  z `getConstructionById`), nebo pro tloušťku, kterou zadal uživatel.
  Sady různých tlouštěk nikdy nesčítej. Vybrané tloušťky uveď. Když
  zadaná tloušťka v `pricing` není, řekni to a nabídni dostupné.
- **Tabulka:** po vrstvách s kódem, popisem, měrnou jednotkou, množstvím
  na m², množstvím celkem, jednotkovou cenou a cenou celkem – kódy, popisy,
  jednotky, ceny a množství přesně tak, jak je nástroj vrátí. Nic
  neupravuj a žádné položky nepřidávej z vlastní paměti.
- **Přepočet:** množství celkem = `quantity.value` × plocha (nebo jiný
  rozměr) zadaný uživatelem, jen kde to měrná jednotka umožňuje;
  výpočet ukaž. Bez zadané plochy uveď hodnoty na 1 m².
- **Co nepřepočítávat:** položky s množstvím 0 nebo bez množství
  (např. vtoky, příplatky za tloušťku) a vrstvy bez ocenění (např.
  kotvení) vypiš zvlášť jako „doplnit podle projektu“ a do součtu je
  nezahrnuj.
- **Součet:** uveď orientační cenu celkem a za m² bez DPH, jen z položek
  s množstvím.
- **Co cena zahrnuje:** výhrady převezmi z `pricingDescription`
  (zkráceně, bez HTML) – nevymýšlej vlastní. Uveď cenovou úroveň nebo
  období, pokud je výstup obsahuje.
- **Ceny DEK:** ceníkové položky DEK (`commercialP9`, kódy `DEK.…`)
  standardně neuváděj. Na konci jednou větou nabídni, že je můžeš
  doplnit. Když o ně uživatel požádá, přidej je jako samostatné sloupce
  (kód, popis, cena DEK) k příslušné položce ÚRS, přesně podle výstupu.
- Připomeň, že výsledek je podklad pro rozpočet, který je potřeba ověřit
  v rozpočtovém programu s aktuální cenovou soustavou ÚRS.

## Zásady

- Výstupy jsou podklad pro návrh, nenahrazují projektovou dokumentaci ani
  posouzení autorizovanou osobou; u návrhových doporučení to krátce připomeň.
- Hodnoty uváděj s jednotkami z API a odbornou terminologii podle ČSN / EN.
- Dlouhý výstup nástroje (vyhledávání, EPD, ocenění), který klient uloží
  do souboru, čti nástrojem pro čtení souborů (Read), ne příkazy
  v terminálu (`jq`, `grep`, `python` apod.), aby uživatel nemusel nic
  povolovat.
- Do popisu, porovnání ani doporučení neuváděj vlastnosti, které
  nástroje nevrátily (cena, hmotnost, dostupnost, životnost apod.), ani
  s výhradou „neověřeno“ a ani nepřímo („levnější“, „lehčí“, „běžnější“).
  Doporučení opírej jen o hodnoty z knihovny. Chybí-li údaj, který
  uživatel potřebuje, napiš, že ho knihovna neuvádí. Cenu získáš jen přes
  `aiItemPricing`.
- Nenabízej výpočty v externích aplikacích DEKSOFT (Tepelná technika 1D,
  BIM knihovna, konfigurátor) – nemáš k nim přístup. Dej uživateli odkaz
  (např. `thermalTransCalcUrl`), aby výpočet nebo úpravu provedl sám.
  Sám umíš jen vyhledat jinou skladbu nebo jinou variantu z knihovny.
- Na konci odpovědi s daty z knihovny uveď zdroj: „Zdroj: Stavební
  knihovna DEK, DEKSOFT (deksoft.eu)“ a odkazy na použité položky,
  jsou-li k dispozici.
- Odpovídej v jazyce uživatele; data z knihovny jsou česky, názvy výrobků
  nepřekládej.
