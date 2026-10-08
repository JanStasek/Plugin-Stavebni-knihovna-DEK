# Pluginy DEKSOFT pro Claude

Marketplace `dek` s pluginem [**stavebni-knihovna-dek**](stavebni-knihovna-dek/README.md) –
Stavební knihovna DEK v Claude: skladby konstrukcí, materiály a výrobky,
data EPD a položky cenové soustavy ÚRS.

```
/plugin marketplace add JanStasek/Plugin-Stavebni-knihovna-DEK
/plugin install stavebni-knihovna-dek@dek
```

## Struktura
```
.claude-plugin/marketplace.json        # katalog marketplace „dek“
stavebni-knihovna-dek/
  .claude-plugin/plugin.json           # manifest pluginu
  .mcp.json                            # MCP konektor https://api.deksoft.eu/mcp
  skills/stavebni-knihovna/SKILL.md    # skill
  commands/                            # /skladba, /material, /epd, /oceneni
```

Kontrola: `claude plugin validate .` a `claude plugin validate stavebni-knihovna-dek`.
