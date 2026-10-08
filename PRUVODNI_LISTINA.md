# Edinburgh Bikes – průvodní listina k Power BI reportu

Zadání: [PBI Projekt na portálu ENGETO](https://portal.engeto.com/study/1c9fafa1-bff0-4a58-8694-a00583705b7c/project/22e10a3f-ab63-4eac-bdc5-e93d436fa821/assignment)

Report se zaměřuje na využívání sdílených kol v Edinburghu. Kombinuje denní údaje o jízdách, počasí a stanovištích a umožňuje sledovat provoz z několika různých pohledů.

Místo popisu po jednotlivých technických částech je dokumentace uspořádána podle toho, jak report dává smysl číst: nejdřív stručný souhrn, potom hlavní analytické oblasti a až následně datové a technické pozadí.

## Rychlý souhrn

Za sledované období bylo zaznamenáno celkem **438 259 jízd**.

Základní hodnoty za celé období:

| Ukazatel | Hodnota |
|---|---:|
| Celkový počet jízd | 438 259 |
| Průměr jízd za den | 430 |
| Průměrná délka jízdy | 32,5 min |
| Aktivní stanice | 198 |

Provoz byl nejvyšší od května do srpna 2020, kdy každý měsíc překročil 30 tisíc jízd. V létě 2019 se měsíční hodnoty pohybovaly přibližně mezi 10 a 18 tisíci.

Nejrušnějším dnem v týdnu je sobota s průměrem 485 jízd. Nejnižší průměr má pondělí s 397 jízdami.

Výrazný rozdíl je vidět i podle teploty. Při průměrné teplotě nad 15 °C vychází přibližně 809 jízd denně, zatímco pod 5 °C pouze 257.

Nejvytíženější stanice:

1. Meadows East – 17 390 jízd
2. Bristo Square – 12 846 jízd
3. St Andrew Square – 12 218 jízd

## Čtyři pohledy na stejná data

### Celkový provoz

Stránka Přehled slouží jako vstupní bod do reportu.

Obsahuje karty s hlavními ukazateli, měsíční vývoj jízd, srovnání dnů v týdnu, vývoj průměrné délky jízdy a rozdělení mezi pracovní dny a víkendy.

Filtry `Rok`, `Typ dne` a `Stanice` umožňují rychle zúžit celý pohled.

Textové pole „Ve zkratce“ shrnuje hlavní závěry za celé období. Je to statický text, takže nereaguje na filtry.

### Sezónnost a změny v čase

Stránka Vývoj v čase je zaměřena na porovnání měsíců a let.

Spojnicový graf ukazuje průběh jednotlivých roků po měsících. Matice doplňuje konkrétní hodnoty pro kombinaci rok × měsíc.

Dále jsou zobrazeny karty s celkovým počtem jízd, průměrem jízd za den a změnou oproti předchozímu roku.

Míra `Změna oproti loňsku %` se zobrazuje pouze tehdy, pokud je pro srovnávací období dostupných alespoň 95 % dnů. Z tohoto důvodu není smysluplná pro všechny kombinace filtrů.

### Vliv počasí

Stránka Počasí propojuje denní provoz s meteorologickými údaji.

Každý bod v bodovém grafu představuje jeden den a porovnává průměrnou teplotu s počtem jízd.

Další grafy ukazují průměrný provoz podle teplotních pásem a podle kategorie srážek.

Karty doplňují:

- průměrnou teplotu,
- srážky úhrnem,
- počet dní se srážkami,
- průměrný vítr.

Korelace mezi průměrnou teplotou a počtem jízd vychází 0,53. Tento výpočet byl proveden mimo Power BI z denních dat a slouží jako doplňující ukazatel.

Ve dnech bez srážek vychází průměrně 472 jízd, zatímco ve dnech se srážkami od 1 mm přibližně 432 jízd.

### Stanice a jejich vytíženost

Stránka Stanice porovnává jednotlivá stanoviště.

Bublinový graf využívá zeměpisnou šířku a délku a velikostí bodu zobrazuje počet jízd.

Pruhový graf je omezený na 15 nejvytíženějších stanic a matice obsahuje úplný přehled všech stanic.

Některé názvy stanic se opakují, proto je v modelu vytvořen pomocný sloupec `Stanice`, který kombinuje název a číslo stanice.

Mezi stránkami se přepíná navigátorem stránek, který je umístěný na každé stránce reportu. Vizuály se vzájemně filtrují kliknutím na prvek v grafu nebo na průřez.

## Odkud data pocházejí

Report využívá tři zdrojové CSV soubory.

### Jízdy

Tabulka `Jizdy` obsahuje 67 164 řádků.

Jeden řádek odpovídá jednomu dni a jedné stanici a obsahuje:

- počet jízd (`pocet_jizd`),
- průměrnou délku jízdy v sekundách (`prumerna_delka_s`).

Časový rozsah je od 15. 9. 2018 do 30. 6. 2021, celkem 1 020 dní bez chybějícího dne.

### Počasí

Tabulka `Pocasi` obsahuje 792 řádků.

Pro každý den jsou k dispozici:

- průměrná teplota,
- maximální teplota,
- minimální teplota,
- srážky,
- průměrný vítr.

Data pokrývají období od 1. 9. 2018 do 31. 10. 2020.

### Stanice

Tabulka `Stanice` obsahuje 198 řádků.

Každý záznam obsahuje číslo a název stanice, popis a zeměpisné souřadnice.

Všechny stanice použité v tabulce `Jizdy` mají odpovídající záznam v této tabulce.

## Model a výpočty

Časovou osu zajišťuje kalkulovaná tabulka `Kalendar`, která obsahuje 1 034 dní od 1. 9. 2018 do 30. 6. 2021.

Vztahy v modelu:

- `Jizdy[Datum]` → `Kalendar[Datum]`
- `Pocasi[Datum]` → `Kalendar[Datum]`
- `Jizdy[stanice_id]` → `Stanice[stanice_id]`

Vztah mezi `Pocasi` a `Kalendar` používá filtrování v obou směrech, aby bylo možné filtrovat jízdy podle meteorologických podmínek.

Kalendář obsahuje pomocné sloupce pro rok, čtvrtletí, měsíc, den v týdnu a typ dne.

V tabulce `Pocasi` jsou vytvořena teplotní pásma:

- pod 5 °C,
- 5–10 °C,
- 10–15 °C,
- 15 °C a více.

Srážky jsou rozděleny na:

- beze srážek,
- slabé do 5 mm,
- silný déšť od 5 mm.

V modelu je také připravena hierarchie `Rok hierarchie` (Rok → Čtvrtletí → Měsíc).

Průměrná délka jízdy je počítána jako vážený průměr podle počtu jízd, aby stanice s velmi malým provozem neměly nepřiměřený vliv na celkový výsledek.

Použité míry zahrnují mimo jiné `Celkem jízd`, `Jízd za den`, `Průměrná délka jízdy (min)`, `Aktivní stanice`, `Změna oproti loňsku %`, `Průměrná teplota`, `Srážky celkem (mm)` a `Průměrný vítr`.

Textové míry `Jízd celkem` a `Srážky úhrnem` slouží pouze pro zobrazení celého čísla na kartách bez automatického zkrácení Power BI.

## Omezení při interpretaci

Časové rozsahy jízd a počasí nejsou shodné. Jízdy pokračují až do 30. 6. 2021, zatímco počasí je dostupné pouze do 31. 10. 2020. Analýza počasí proto pracuje jen s obdobím, kde se obě datové sady překrývají.

Rok 2018 obsahuje jízdy pouze od poloviny září a rok 2021 pouze do konce června. Celkové roční součty těchto let proto nejsou přímo srovnatelné s úplnými roky.

V seznamu stanic jsou také dočasné a eventové stanice. Některé mají za celé období jen několik jízd. Medián je 1 065 jízd na stanici.

Klasický mapový vizuál nebyl použit. Pro zobrazení polohy stanic slouží bublinový graf podle zeměpisných souřadnic, protože mapové vizuály jsou v Power BI Desktop ve výchozím nastavení vypnuté.

Hierarchie `Rok hierarchie` je v modelu připravená, ale žádný aktuální vizuál ji nepoužívá.
