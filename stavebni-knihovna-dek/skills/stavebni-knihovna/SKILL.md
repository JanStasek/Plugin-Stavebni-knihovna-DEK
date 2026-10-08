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

## Postup u skladeb

1. Ujasni typ konstrukce, druh stavby, požadavky (U, požární odolnost,
   akustika, pochozí/zelená střecha, podlahové topení) a omezení
   (celková tloušťka, nosná konstrukce). Chybí-li zásadní údaj, zeptej se
   jednou; jinak hledej a předpoklad uveď.
2. `searchConstructions` → vyber 1–3 kandidáty → `getConstructionById`.
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

## Ocenění skladby (ÚRS)

Když uživatel chce skladbu ocenit, sestavit rozpočet nebo výkaz výměr,
použij `aiItemPricing` s ID skladby z vyhledávání nebo detailu.

- Položky ÚRS předlož v tabulce s kódem položky, popisem, měrnou jednotkou
  a dalšími údaji přesně tak, jak je nástroj vrátí. Kódy ani popisy
  položek neupravuj a žádné nepřidávej z vlastní paměti.
- Množství přepočítávej na plochu nebo rozměr zadaný uživatelem jen tam,
  kde to měrná jednotka umožňuje, a výpočet ukaž. Kde si přepočtem nejsi
  jistý (např. kotvení, prořezy, doplňky), řekni to.
- Pokud výstup obsahuje ceny, uveď i jejich cenovou úroveň nebo období,
  pokud je k dispozici. Připomeň, že výsledek je podklad pro rozpočet,
  který je potřeba ověřit v rozpočtovém programu s aktuální cenovou
  soustavou ÚRS.

## Zásady

- Výstupy jsou podklad pro návrh, nenahrazují projektovou dokumentaci ani
  posouzení autorizovanou osobou; u návrhových doporučení to krátce připomeň.
- Hodnoty uváděj s jednotkami z API a odbornou terminologii podle ČSN / EN.
- Odpovídej v jazyce uživatele; data z knihovny jsou česky, názvy výrobků
  nepřekládej.
