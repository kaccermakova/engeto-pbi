# Edinburgh Bikes – Power BI report

Projekt vznikl v rámci ENGETO Datové akademie a analyzuje provoz sdílených kol v Edinburghu od září 2018 do června 2021.

Výsledkem je interaktivní report v Power BI, který propojuje provozní data o jízdách, počasí a informace o jednotlivých stanicích. Report je navržený tak, aby bylo možné rychle přejít od celkového přehledu k detailnějšímu pohledu na čas, počasí nebo konkrétní stanice.

## Co lze v reportu zjistit

Report odpovídá především na tyto otázky:

- jak se počet jízd měnil v průběhu měsíců a let,
- které dny v týdnu byly nejvytíženější,
- jak se využívání kol lišilo mezi pracovním týdnem a víkendem,
- jaký vztah měl počet jízd k teplotě a srážkám,
- které stanice patřily mezi nejvytíženější.

Soubor `edinburgh_bikes.pbix` obsahuje čtyři pohledy: Přehled, Vývoj v čase, Počasí a Stanice.

## Co je potřeba k použití

Pro běžné otevření stačí Power BI Desktop na Windows. Data jsou uložená přímo v souboru v režimu Import, takže pro prohlížení hotového reportu není nutné připojení ke zdrojovým CSV souborům.

Pokud má dojít k aktualizaci dat, jsou potřeba původní soubory:

- `jizdy_den.csv`
- `pocasi_den.csv`
- `stanice.csv`

V Power Query je následně nutné upravit parametr `DataFolder` tak, aby ukazoval na složku se zdrojovými daty. Cesta musí končit lomítkem.

## Jak jsou data využita

Data o jízdách pokrývají období od 15. 9. 2018 do 30. 6. 2021. Údaje o počasí jsou dostupné do 31. 10. 2020, takže část reportu zaměřená na počasí pracuje s kratším obdobím.

Stanice jsou propojené s jízdami přes jejich identifikátor a časovou osu zajišťuje samostatná kalendářní tabulka.

Zdrojové CSV soubory se při zpracování nemění. Úpravy datových typů, názvů a pomocných sloupců probíhají až v Power Query a v datovém modelu Power BI.

## Dokumentace

Podrobnější popis datového modelu, výpočtů, jednotlivých stránek reportu, hlavních výsledků a omezení je uveden v [PRUVODNI_LISTINA.md](PRUVODNI_LISTINA.md).

## Využití AI

Při práci na projektu byla AI využita pouze podpůrně, zejména pro kontrolu již vytvořeného SQL kódu a stylistickou úpravu textové dokumentace. Zpracování dat, výpočty a výsledky byly ověřovány nad zdrojovými daty.
