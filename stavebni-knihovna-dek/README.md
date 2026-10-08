# Stavební knihovna DEK – plugin pro Claude

<img src="icon.png" alt="Stavební knihovna DEK" width="96" height="96">

Plugin zpřístupňuje v Claude Stavební knihovnu DEK: skladby konstrukcí
a systémů, materiály a výrobky,
jejich environmentální data (EPD) a položky cenové soustavy ÚRS pro
ocenění skladeb.

## Obsah
- **MCP konektor** `stavebni-knihovna-dek` (`https://api.deksoft.eu/mcp`)
- **Skill** `stavebni-knihovna` – jak vyhledávat, číst a prezentovat data,
  včetně přepočtu EPD podle tloušťky
- **Příkazy** `/skladba`, `/material`, `/epd`, `/oceneni`

## Příklady použití
- „Navrhni skladbu ploché střechy s PVC fólií na trapézovém plechu s U do 0,16.“
- „Čím můžu ve skladbě nahradit EPS izolaci kvůli požadavku na nehořlavost?“
- „Jaká je GWP fáze A1–A3 pro 200 mm EPS 100 S?“
- „Připrav položky ÚRS pro ocenění této střešní skladby na 450 m².“

## Požadavky
Plugin se připojuje ke vzdálenému MCP serveru `https://api.deksoft.eu/mcp`
(HTTP). Nevyžaduje přihlášení ani API klíč a nic neinstaluje lokálně.

## Instalace (Claude Code)
```
/plugin marketplace add JanStasek/Plugin-Stavebni-knihovna-DEK
/plugin install stavebni-knihovna-dek@dek
```

## Licence
Plugin (skill, příkazy, konfigurace) je svobodný software pod licencí
[Apache License 2.0](LICENSE). Při jeho šíření nebo úpravách je nutné
zachovat soubor [NOTICE](NOTICE) a uvést zdroj: **Stavební knihovna DEK,
DEKSOFT – https://www.deksoft.eu**.

Data z knihovny (skladby, materiály, EPD, položky ÚRS) nejsou součástí
licence pluginu a řídí se licenčními podmínkami DEKSOFT.

## Podmínky a podpora
- Licenční podmínky dat: https://deksoft.eu/programy/licencnipodminky
- Ochrana osobních údajů: https://deksoft.eu/ochrana-osobnich-udaju
- Kontakt: info@deksoft.eu
