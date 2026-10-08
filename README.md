# Stavební knihovna DEK – plugin pro Claude

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

## Instalace (Claude Code)
```
/plugin marketplace add <org>/stavebni-knihovna-dek
/plugin install stavebni-knihovna-dek@dek
```

## Podmínky a podpora
- Licenční podmínky: https://deksoft.eu/programy/licencnipodminky
- Kontakt: info@deksoft.eu
- TODO: odkaz na zásady ochrany osobních údajů
