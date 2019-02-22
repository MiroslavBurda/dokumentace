
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