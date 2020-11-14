OSciloskop
-------------

Podržím libovolné tlačítko nebo kolečko  2 sekundy - objeví se k němu podrobná nápověda (anglicky). 
Menu ke každému tlačítku se zobrazuje vždy dole na obrazovce a ovládá se tlačítky pod obrazovkou.  
Tlačítko Help zobrazí menu, které  obsahuje mimo jiné: "Getting started" a  "Training singals". 

Vzorkovací frekvence (bity za sekundu) musí být nejméně 10x vyšší než analogová šířka pásma, tj. frekvence signálu, který měřím (v Hz).


Pokud připojuju sondy, musí mít každá připojenou svoji vlastní zem, 
aby byl minimalizovaný šum.

Sondy (probe) mají klobouček s háčkem pro snadné uchopení měřeného drátu. Když se klobouček sundá, lze měřit dotykově hrotem. 

Každá sonda má také pobočný drát zakončený "krokodýlem". Ten se připojuje vždy na zem měřeného obvodu. Pokud ho nepřipojíte, 
bude se vám na kabelu sondy indukovat šum z okolí.

Červený křížek na sondách slouží ke zkalibrování sond pomocí otočného kondenzátoru a signálu "Probe".

Osciloskop měří pouze velmi rychle napětí.
Není stavěný jako přesné měření napětí, spíše orientační, na vstupu je pouze 8 bitový převodník napětí, ale měří přesně časy, takže je schopen zobrazit *průběh* napětí. 

Agilent/Keysight tech. DSO-X 2024A 
sonda x 10 - dělí vstupní signál 10

osciloskop si pamatuje poslední nastavení 

bezpečnost - zvádne 300 V RMS = efektivní hodnota 
dekódovat digitální sběrnici umí pouze analogové vstupy - 
digitální umí pouye zobrayit a měřit časové parametrz 
např. SPI má 4 kabelz 

všechna kolečka se dají mačkat 

Default setup - uvede do výchozího nastavení (doporučeno na začátek práce)

kalibrace sondy (většinou se neřeší) 
menu sondy: 
probe: dělička v sondě (musí se nastavit stejně jak na sondě), mělo by to být, obvyklá hodnota je 10:1
zapnu střídavou vazbu: odstraní stejnosměrnou složku signálu 
invert - zobrazuje kladnou složku dolů místo nahoru 
BW limit - potlačuje signály nad 20 MHz (tuto hodnotu na tomto osc. nelze měnít) - redukuje šumy, které nás obvykle nezajímají  
 
 velké kolečko - zesílení 
 malé - offset - posun nuly= offset 
 tlačítko Mode Coupling 
 tl. Force Trigger - okamžitě zahájí měření (v normal módu) 
 
 trigger - říká : teď začni měřit 
 může mít pouze jeden vstup ten lze velmi různě navolit 
 mode: auto - čeká "nějakou dobu" na signál z triggeru a pak začne měřit  
 normal - čeká na signál z triggeru neomezeně dlouho 
 Coupling DC - měří všechno 
 Coupling AC - potlačí stejnosměrné složky signálu 
 LF Reject - potlačení pomalých frekvencí 
 
 HF Reject - potlačení vysokofrekvenční složky 
 Noise Rej - potlačení šumu   nepoznal jsem rozdíl  - pouze na "rozumně tvrdém" signálu 
 Holdoff - nastavím dobu, po kterou ignoruju další signály
 
 nastavení úrovně, jejíž překročení spouští měření = treshold  
 
 Nastavení času 
  
 velké kolečko nastavení šířky periody, 
 malé kolečko - nastavení posunu vůči počátku 
 tl. lupa - hodně zvětší 
 
 tl. Run/stop - červená neměří, zelená měří, 
 tl. single - žlutá svítí - udělá právě jedno měření - naplní obsah paměti 
 
 
 time mode - normal - to co obvykle  
           - měření v reálném čase - vhodné pro pomalá měření (točení potenciometrem )

           
XY - dvě napětí
           
           
           
Wave  gen - modře svítí - zapnuto / nesvítí vypnuto - viz help 

napětí se nastavuje pro každý kanál zvlášť, čas je pro všechny kanály vždy stejny 

menu měření tl. Meas 

nastavuju si, co všechno chci měřit -> až 4 věci zaráz

menu Kurzor - tl. - Cursors = pravítka -> dvě vodorovná a dvě svislá - lze tím měřit zcela manuálně 

nejčastější použití - sledování PWM a signálů na sběrnicích 

tl. Refs. referenční signály - umí si pamatovat dva a srovnávat s nimi aktuální průběh 
************************ jak se s tím pracuje 

tl. Math. umí arit. výpočty se dvěma signály , taky umí Fourrierovu analýzu signálu 

tl. Digital - nastavení měření na digitálních vstupech 

tl.  serial - serial decode mode  
treshold dekódování sběrnic -> nastavím, kde končí log. O a Log. 1  

Auto scale - snaží se sám nastavit do nejoptimálnějšího sledování signálu 

pokud držím nějaké tlačítko 2 sekundy, zobrazí se nápověda 
pokud někdo nastaví jako jazyk korejštinu a spol: tl Help, potom třetí tl. zleva pod obrazovkou vyvolá nabídku jazyků -> zvolím zpět angličtinu 

lze uložit až 10 svých nastavení osciloskopu - jak ? ******************


 
***************************
- autoscale menu: acq mode ??  
- tl. navigate: ***********************
tl. search: ************************
tl.acquire  - segmented memory ***************



do souboru ukládá každých 2,5 mikro sec. , když je nastavených 100MSa/s **************
-----------------------------------
posun počátku času vlevo, střed, vpravo: tl. Horiz menu: Time Ref 

dva signály, které nejsou ze stejných hodin (stejného čipu), se obvykle na monitoru posunují vůči sobě -> vypnout 
měření (stop) a změřit Off-line, co potřebuju 



http://www.elektroraj.cz/2014/12/10/jak-vybrat-osciloskop/
http://www.elektroraj.cz/2014/04/01/osciloskop-a-spektr-analyzator-zdarma/

http://www.elektroraj.cz/2017/01/31/stavebnice-osciloskopu-jye-tech-dso-shell/
https://www.banggood.com/Orignal-JYE-Tech-DS0150-15001K-DSO-SHELL-DIY-Digital-Oscilloscope-Kit-With-Housing-p-1093865.html?p=5608046322424201609F&cur_warehouse=CN





duty-cycle 25% : obdélníkový signál má 25% na High
Off set : posun signálu po ose Y 

náš osciloskop má taky generátor signálu: 
- obdélník, trojúhelník, .... , šum 
lze plynule nastavovat frekvenci 
max 5V 



některé sondy na vstupu dělí napětí 1:10 

"pobočný drát" u sondy je zem, sonda je signál 




z reklamy: 

Kanály Osciloskopu:
    2 Analogové, +1 Digitálních 
Šířka Pásma:
    50MHz 
Vzorkovací Frekvence:
    1GSPS 
Hloubka Paměti Displeje:
    100 kpts 
Vypočítaná Doba Náběhu:
    7ns  
10:1 a 1:1 přepínatelné pasivní sondy 
******************************************************************************
unipolární tranzistory: 
N kanál je varianta NPN 

tranzistor je řízený napětím 

čím větší frekvence, tím větší jsou spínací ztráty 

Source - připojím zdroj  
Drain  - připojím spotřebič = motor 
Gate  - přivedu napětí proto Source, když je dost vysoké (nad 2-3 volty), otevře tranzistor 

