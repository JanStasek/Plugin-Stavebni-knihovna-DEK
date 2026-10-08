# Žádost o zveřejnění pluginu – podklady

Podklady pro odeslání pluginu do adresáře pluginů Claude
(formulář pro zařazení pluginu). Texty jsou v angličtině, protože ji
formulář vyžaduje; český popis je v README pluginu.

## Kontrolní seznam před odesláním

- [x] Manifest `stavebni-knihovna-dek/.claude-plugin/plugin.json` – `claude plugin validate` prochází
- [x] Marketplace `.claude-plugin/marketplace.json` – `claude plugin validate` prochází
- [x] MCP server `https://api.deksoft.eu/mcp` odpovídá (ověřeno 2026-10-08 dotazem `searchConstructions`)
- [x] README s popisem, příklady, instalací a kontaktem
- [x] Zásady ochrany osobních údajů: https://deksoft.eu/ochrana-osobnich-udaju (v README a v `plugin.json` jako `privacyPolicyUrl`)
- [x] Ikona `stavebni-knihovna-dek/icon.png` (v `plugin.json` jako `icon`); údaje pro výpis v adresáři (`displayName`, `documentationUrl`, `termsOfServiceUrl`) v `plugin.json`
- [x] Licence Apache-2.0 + `NOTICE` s povinností uvést zdroj (`LICENSE`, `NOTICE`, pole `license` v `plugin.json`)
- [x] Repozitář je veřejný
- [ ] Sloučit větev s úpravami do `main` (instalace z GitHubu bere výchozí větev)
- [ ] Volitelně: přesunout repozitář pod organizaci DEKSOFT a aktualizovat URL v `plugin.json`, README a v instalačních příkazech
- [ ] Otestovat instalaci z GitHubu: `/plugin marketplace add JanStasek/Plugin-Stavebni-knihovna-DEK` → `/plugin install stavebni-knihovna-dek@dek` → `/skladba plochá střecha, U ≤ 0,16`

## Údaje do formuláře

| Pole | Hodnota |
|---|---|
| Plugin name | `stavebni-knihovna-dek` |
| Display name | Stavební knihovna DEK (DEK Building Library) |
| Icon | `stavebni-knihovna-dek/icon.png` |
| Version | 0.4.4 |
| Publisher | DEKSOFT |
| Contact e-mail | info@deksoft.eu |
| Website | https://deksoft.eu |
| Repository | https://github.com/JanStasek/Plugin-Stavebni-knihovna-DEK |
| Plugin path | `stavebni-knihovna-dek` (marketplace `dek`) |
| Category | Construction / Engineering |
| MCP server | `https://api.deksoft.eu/mcp` (remote HTTP, no authentication) |
| Terms | https://deksoft.eu/programy/licencnipodminky |
| Privacy policy | https://deksoft.eu/ochrana-osobnich-udaju |
| License | Apache-2.0 (attribution via NOTICE) |
| Languages | Czech (data), Czech + English (queries) |

### Short description
Search the DEK Building Library: building assemblies, materials, EPD data and ÚRS cost items for Czech construction projects.

### Full description
Stavební knihovna DEK brings the DEK Building Library into Claude. Architects,
designers, engineers and estimators can search thousands of verified building
assemblies (roofs, external and internal walls, floors, ceilings) and
construction materials and products, and get their technical parameters –
layers and thicknesses, thermal transmittance U and thermal resistance R,
fire resistance, waterproofing reliability, BIM and technical-sheet links.

The plugin also provides Environmental Product Declaration (EPD) data
(GWP and other indicators by life-cycle module A1–D, with thickness
conversion factors) and ÚRS price-system items for cost estimating of an
assembly.

Contents:
- MCP connector to the DEKSOFT API (read-only, no account required)
- Skill `stavebni-knihovna` – how to search, read and present library data
  correctly (never inventing values, EPD recalculation, ÚRS item handling,
  citing the DEK Building Library as the source)
- Slash commands `/skladba`, `/material`, `/epd`, `/oceneni`

### Example prompts
1. „Navrhni skladbu ploché střechy s PVC fólií na trapézovém plechu s U do 0,16.“
   (Suggest a flat-roof assembly with PVC membrane on trapezoidal sheet, U ≤ 0.16.)
2. „Čím můžu ve skladbě nahradit EPS izolaci kvůli požadavku na nehořlavost?“
   (What can replace EPS insulation in the assembly for non-combustibility?)
3. „Jaká je GWP fáze A1–A3 pro 200 mm EPS 100 S?“
   (What is the A1–A3 GWP of 200 mm EPS 100 S?)
4. „Připrav položky ÚRS pro ocenění této střešní skladby na 450 m².“
   (Prepare ÚRS cost items for this roof assembly, 450 m².)

### Data and safety
- All tools are read-only; the plugin does not write or modify any data.
- No authentication, API keys or personal data are required. User queries
  are sent to `api.deksoft.eu` only to search the library.
- Outputs are design support material and do not replace project
  documentation or assessment by an authorized person; the skill instructs
  Claude to state this.

### MCP tools
`searchConstructions`, `searchMaterials`, `listItemsIds`,
`getConstructionById`, `getMaterialById`, `listEpdProducts`,
`getEpdByProductId`, `aiItemPricing`
