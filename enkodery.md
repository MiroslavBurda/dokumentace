
## (jednoduchý) enkodér - zjistí rychlost, ale ne směr 


## co je to kvadraturní enkodér 
vrtulka, listy 90 stupnu, dvě fotozávory posunuté navzájem o 45 stupnu 
z toho, který se předbíhá nebo zpožďuje, se určí směr 
z delky obdelnikoveho pulzu nebo počtu vzestupných hran za čas se určí rychlost 

ESP32 stihne enkodéry upočítat, arduino to nestihne 
ztrácejí se pulzy a pak to dává chyby 

ESP32 a Xmega to řeší hardwerově a máš přímo v paměti číslo, které odpovídá rychlosti motoru, 
ATMega to přímo neumí a musí se to řešit softwarově, nejjednodušeji asi pomocí přerušení 

------------
rozběhat enkodéry, potom je měřit průběžně - nevýhoda: nemůžu použít delay -> řešení je stavový automat

z enkodéru je možné číst polohu motoru a z toho při otáčení motoru rukou zjistit, jak je potřeba program nastavit 