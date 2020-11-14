Školení Petr Štourač na Robotárně, 2020

Fusion 360
------------

export do STL: pravým tlačítkem vlevo na browseru klepnu na daný díl (body) a Save as STL,
Refinement doporučeno: High 

doporučený úhel od kolmice 30 stupňů, nad 70 stupňů už se to vytisknout nedá bez podpěr 

díly se dají potom zabrousit (PLA se u broušení natavuje, musí se buď krátce nebo chladit)

převisy nad 10 mm je potřeba tisknout s podporami (tiskárna není schopna tisknout do vzduchu)
pro malé převisy tiskárna hodně zpomalí chod a pouští větrák, aby to drželo 

u zaoblených hran (rádius) budou vznikat plošky, dá se to eliminovat malou tloušťkou vrstvy

první vrstvy se více přitlačují při tisku na podložku, aby dobře chytily, takže budou trochu spláclé, detaily do 1mm od podložky se tím rozpleskou 

https://www.prusa3d.com/prusaslicer/

https://www.prusaprinters.org/ - prusa komunita 

nastavení: trysky 0,4 mm -> max. tloušťka vrstvy je 0,4 mm, obvyklá je 0,3 mm; minimum 0,1 mm 

Na robotárně jsou tiskárny: 
* Original Prusa i3 MK3S MMU2S 
* Original Prusa i3 MK3S  (preferovaná)

Slicer: 
------------
výplň doporučená 25% aby to něco vydrželo 
15% aby věci dobře vypadaly

pokud je např. 70% tak to 
1) dlouho trvá, 
2) výrobek je křehčí, není pružný 
3) vrstvy nacpané k sobě mají sklon se od sebe odlepovat

perimetry: počet okrajových vrstev, doporučené jsou dvě 

warp - odlepení od podložky - stává se u ABS 

instance - chová se více kusů stejně 

kopie (Ctrl+C, Ctrl+V) - chová se každý kus zvlášť 

filament (struna) nastavení: 

Prusament PLA - měkne už při 50 St. - nevhodné pro postprocessing -> bude se tavit při broušení, doporučeno očistit desku izopropylem 

Prusament PETG - vyšší teplotní odolnost (snadné broušení), měkne nad 80 st. lesklý povrch, nečistit podložku, tisknout na hrubou podložku nebo osahat rukama (kvůli tuku z rukou), protože má velkou přilnavost a z hladké podložky se to nedá moc dostat 

Prusa ABS - hodně pevné, měkne až nad 11O st. 
při výplni nad 30% se u většího modelu vrstvy separují od sebe 

průměr filamentu 1.75mm 

násobič extruze - jak moc bude extrudér vytlačovat materiál, doporučená hodnota je 95%-105% 

tichý režim na rozdíl od normálního nedetekuje kolize tiskové hlavy s čímkoliv, 
po detekci kolize se tiskárna vrací do výchozí polohy
a pak naváže, kde skončila, pokud se kolize nedetekuje, tak prostě tiskárna jede pořád pryč 

na konci se vyexportuje G-code, ten se uloží na SD kartu - Pruša s jiným médiem nekomunikuje 

Po vložení SD karty do tiskárny se zvolí soubor s G-kódem (potvrdit stiskem),
tiskárna se sama nahřeje a spustí tisk, po dokončení tisku se vyjme podložka a ohne v obou směrech pro oddělení 
dílu, pro malé díly se použije plastová špachtle 

-----------------------------------------------
Pozn: 
* flex filament se nikdy netiskne s podporami

* ABS se musí tisknout v boxu, protože jinak by malé změny teploty (průvan) způsobily odlepení vrstev od sebe 