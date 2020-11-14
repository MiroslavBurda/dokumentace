
## Baterie na Robotárně a v míčkoflusu - Panasonic NCR18650PF

Panasonic NCR18650PF
https://lygte-info.dk/review/batteries2012/Orbtronic%2018650PD%202900mAh%20(Black)%20UK.html

u LiOn musí elektronika hlídat podvybití baterek a včas je odpojit, jinak se zničí -> PowerBanky už mají takovou elektroniku v sobě 
zase ale pokud nemají dostatečný odběr (robot zrovna stojí), tak se odpojí a nedodávají vůbec nic

- Li-Pol plné 4,2~V, neměly by klesnout pod 3,5~V, vybité jsou 3,3~V
- baterie 18650 Li-On dává 4,2V nabitá, 3,3V vybitá

### AA + AAA baterie nabíjecí

* 0,9 V -> jsou normálně vybité 
* 0,7 V -> jsou maximálně vybité, půjdou nabít


### Baterie se rychle vybíjejí - co s tím? 

- jednoduché řešení neexistuje 
- záložní baterky a neustále dobíjet  


## Servo
servo není odpojitelné stoptlačítkem -> třídrátové stoptlačítko  a třetí drát pro elektroniku,která odpojí servo 

na všech dosavadních robotech se vždycky odpojovaly baterky 
neřešitelný problém při jejich odpojení je,že se indukují napěŤové špičky 
if se vyp. tlač. připojí až za vnh, tak vnh ma ze strany motorů ochrany proti těmto špičkám 
(ale chybí mi pak možnost vypínat čtyřčlánek pro serva  - nemám jak je odpojit 





## Zkušenosti a zrady 

- když se chová robot podivně -> daná část chvíli funguje a chvíli nefunguje, tak to můžou být vybité baterie 
- když se připojí servo na pouze vstupní pin, tak to nebude fungovat 
- firmware RBC odpojí všechny výstupy (motory, serva) když napětí na baterii klesne pod 7,2 V -> ochrana baterií před podvybitím a tím zničením 
- maximální napětí: inteligentní serva by měla zvládnout přímo napětí z dvoučlánku Li-Pol tj. běžně 7,4V, prý zvládne (tom Vavrinec) i 8,4V 

--------------

- powerbanka stačí 1A 


## Napájení serv 

-i malé/mikro servo si při rozjezdu vezme 1-3 A, větší klidně i 20 A, pokud mu je nedodám, může při rozjezdu dojít 
k poklesu napájecího napětí a tím k posunu nuly na servu - servo je potom nepřesné 

-napájení: hloupé servo potřebuje max. 6 V, 2xbaterie 18650 Li-On dává 8,4V nabitá, 6,6V vybitá, proto potřebuju stabilizátor 
ideálně s malým úbytkem napětí (něco jako 7805) Stabilizátor přebytky výkonu mění v teplo, proto se musí dobře chladit 
pokud bude mít servo o něco méně než 5V, možná fungovat bude, ale není to jisté 

-před stabilizátor je potřeba zařadit ještě do série diodu (alespoň 3A a počítat s úbytkem napětí na ní), aby servo, na kterém je zrovna 
úbytek napětí, nebralo napětí ostatním servům 

-na přívodních vodičích vzniká při rozjezdu velká indukčnost, proto musí být co nejkratší, ideálně jednotky cm, především u velkých serv 

-přívodní vodiče napětí pro servo musí být silnější, např. 1,5 mm pro velké servo, pouze datový vodič bude klasický 

### Napájení ze zdroje pro mobil 

545043 YwRobot 
https://www.petervis.com/Raspberry_PI/Breadboard_Power_Supply/YwRobot_Breadboard_Power_Supply.html

## Velké nabíječky

nákup bez zdroje
- levnější 
- zdroje se ztrácí
- musíš mít vlastní zdroj 
    
nákup se zdrojem -> naopak 

konektory na výstupu si každý vybírá a pájí samostatně podle toho, jaké má baterky (pro LiPol 4 piny, 6 pinů, pro NiCd XT60 nebo JST )
výkony od 100W nahoru, pro běžný provoz postačí základní 
nekupovat v běžných obchodech, ale v (internetových) prodejnách pro modeláře 

doporučený typ: ISDT např. na www.reichard.com 
na Robotárně mají raytronic c14 (60 W) a rozbočky na výstupu a zdroj byl v tom 

na nabíjení článku LiPoL můžou být potřeba až 2 A -> 5-ti článek LiPol -> 10 A  

## Ostatní

- step down dává větší proudy, ale víc se rozkmitává než 7805 stabilizátor 

#### Inteligentní LED:   
očekávaný proudový odběr: driver pro každou 1mA + samotná LED 20-30mA každá 
