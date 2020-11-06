
NEMÁ OCHRANU PROTI PŘEPÓLOVÁNÍ !!!!! 

FOC - field oriented control 
tři fáze můžu zapojit libovolně 
když prohodím AB na enkodéru, tak se změní směr otáčení -> Odrive si to přebere 
pin Z - indexový pin, který mají některé enkodéry (1x za otáčku) , není potřeba zapojovat 

brzdný rezistor R47, 50W 
připojení z počítače: klasicky přes USB 

Odrive elektronika běží na 3,3V (tj. na nožičkách je 3,3V), ale je 5V kompatibilní 

připojení z robota - UART ... viz dokumentace 

Odrive je dělaný na to, aby byl napájený z baterek 
nedá se napájet z USB 

https://odriverobotics.com/ - vše potřebné, hlavně dokumentace 

bez enkodéru lze, ale nejmenší rozlišení je 1/počet polů na motoru a má spodní minimum otáček a nerozjede se při velké zátěži - měl nějaké problémy, Honza Mrázek psal pull-request
https://github.com/madcowswe/ODrive/tree/master/Arduino/ODriveArduino - knihovna a ukázka programu 


motory lze používat, dokud se motor teplem nezničí (neodymy v motorech 120 st. C), dobré 250 st. C (nezničí se hned)
maximální hodnoty výstupních napětí a proudů do motoru s KV=1500 se nastavuje špičkový 30A 


stav: 
-IDLE : volnoběh (Odrive motor neřídí)  
my máme verzi 24V 

je potřeba nastavit: 
- max. proud motorem 
- počet magnetických párů 
- počet hran na otáčku?  
- ULOŽIT VŠECHNU KONFIGURACI (jinak je to jenom v RAM ) + restart Odrive

při každém zapnutí se musí zkalibrovat motor (potřebuje si dát motor do kotextu s enkorérem)
Přitom by motor neměl být na pružinové zátěži (rostoucí zátěž s rostoucí vzdáleností)

https://github.com/madcowswe/ODriveHardware/blob/master/v3/v3.3docs/mech_dimensions.PNG - rozměry, na výšku cca 4 cm

Zprovoznění 
--------------
https://docs.odriverobotics.com - nainstalovat python 3 a  odrivetool - funguje z příkazové řádky, ukončuje se Ctrl+D, aby se dal rozjet, musí být Odrive připojený k PC a zapnutý 


https://gitlab.com/p87942130/ui_odrivetool - grafická nástavba pro odrivetool - stáhnout jako zip, rozbalit, a podle pokynu v readme.md doinstalovat ovladače pro grafy a grafické rozhraní (tlačítka) a spustit z příkazové řádky 
přitom do příkazové řádky patří: python odrivetool_UI.py


idle - vypnutý výkonový stupeň (řízení motoru )
startovní sekvence - lze použít i později 

1) motor calibration - zjištění odporu a indukčnosti vinutí - pouští do něj různé frekvence (píská) a měří fázový posuv - dá se uložit do Odrivu (musí se zvlášť dát Save)
2) encoder offset - zjištění natočení motoru (vzájemná poloha rotoru a statoru - pomalé točení tam a zpet
tyto dva údaje 1) a 2) odrive potřebuje, aby mohl přesně řídit motor -> zjištění pomocí enkodérů , proto točí kolem 
encoder index - pokud má vyvedený indexový drát (Z) - je to jedna konkrétní poloha v otáčce, je možné ho použít místo encoder offset, je to rychlejší,  může takto zjišťovat polohu

sensorless control  - řízení bez enkodérů - tam se žádné kalibrace nedělají, ale motor se nedá řídit pomalu a přesně 
(v podstatě se z toho stane modelářský regulátor )
Full calibration = motor calibration + encoder offset

Closed loop - uzavřená smyčka řízení -> zapne řízení motoru (nutná podmínka - hotová kalibrace)

Závěr: pro start motoru potřebuju encoder offset zapnout , po jeho konci spadne Odrive do Idle, potom zapnout Close loop -> a můžu jezdit 
if chci vypnout řízení motorů -> volnoběh, šetří baterku, -> přepnu do Idle , dokud nevypnu napájení, 

tak potom po Idle stačí znova zapnout Close loop 

if je chyba, tak reboot restartuje celý odrive, potom je potřeba ho znovu připojit (conect)

taky je možné Odrive resetovat -> chybový stav se vymaže 

Pro držení odrive v resetu je potřeba přes rezistor cca 3-10k připojit pin NRST k zemi, současně připojit na vybraný pin ESP. 
Když se na pin ESP přivede log1, tak se Odrive zapne. Pokud bude resetované nebo vypnuté ESP, bude resetovaný i Odrive. 


spuštění odrive tool: 
připojit se do spráovného adresáře a napsat:
python odrivetool_UI.py

tři způsoby řízení:
1) momentové - motor působí konstatní silou - nic to neříká o rychlosti ani o poloze (terminologie Odrivu: řízení proudu) 
2) rychlostní řízení - nic to neříká o poloze, když nejde do motoru signál o nenulové rychlosti, motor se brání otáčení, ale zůstane v nové poloze 
3) polohové řízení - snaží se udržet polohu a když nejde do motoru signál o změně, tak se vrací se na tu polohu, která byla naposledy zadaná 

Odrive má 4 logické vrstvy:
