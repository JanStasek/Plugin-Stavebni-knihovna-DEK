---
description: Připrav podklad pro ocenění skladby z DEK v cenové soustavě ÚRS
argument-hint: <skladba (název, kód nebo popis), případně plocha a tloušťky vrstev, např. "DEKROOF 02, 450 m2, EPS 300 mm">
---
Použij skill `stavebni-knihovna`. Najdi skladbu podle zadání: $ARGUMENTS

Načti k ní položky cenové soustavy ÚRS nástrojem `aiItemPricing` a předlož
je v tabulce podle oddílu „Ocenění skladby (ÚRS)“ ve skillu – pro tloušťky
zadané uživatelem, jinak pro základní konfiguraci. Je-li zadaná plocha,
přepočti množství tam, kde to měrná jednotka umožňuje, a výpočet ukaž.
