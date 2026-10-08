---
description: Připrav podklad pro ocenění skladby z DEK v cenové soustavě ÚRS
argument-hint: <skladba (název, kód nebo popis) a případně plocha, např. "DEK ROOF 01, 450 m2">
---
Použij skill `stavebni-knihovna`. Najdi skladbu podle zadání: $ARGUMENTS

Načti k ní položky cenové soustavy ÚRS nástrojem `aiItemPricing` a předlož
je v tabulce. Je-li zadaná plocha, přepočti množství tam, kde to měrná
jednotka umožňuje, a výpočet ukaž.
