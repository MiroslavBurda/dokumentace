wiki:common_mistake
Bootloader Nefunguje po nahrátí:
máte stoplý číp
nenastavili jste pojistky přes programator

http://technika.junior.cz/trac/wiki/shupito

https://www.youtube.com/watch?v=Soc5FjWho74 (první část) 
jak nainstalovat ovladače k Shupitu: 
Je potřeba vypnout v systému vynucování podpisu ovladačů kvůli tomu je potřeba restartovat windows speciálním způsobem:
Menu Start -> vypnout -> zmáčknout a držet levý shift -> kliknout na restart 
objeví se nabídka, vybrat troubleshoots -> advanced options -> startup settings -> restart 
při restartu se objeví nabídka startup settings -> vybrat nabídku 7 - disable drive signature enforcement 
https://www.youtube.com/watch?v=Md2z8Ukox0M (druhá část)
na webu technika.tasemnice.eu/trac/wiki/shupito 
stáhnout ovladače pro verzi 2.3
Start -> napiš "device manager" a otevři ho
připoj Shupito k počítači, dvojklikni, vyber "Update driver" - a proveď update 

zapni lorris, programátor -> vyber připojení Shupito 
u Shupita očekává externí napájení, pokud tam není, musí se zapnout vlastní napájení Shupita (volba VCCIO )
klik na Stop, if nic, tak klik na mode, volba SPI, pokud je zatržená, tak prokliknout, jinak zvolit a opět Stop
pokud Lorris nerozpozná čip, je potřeba najít soubor "shupito chip devs" někde na tracku a doplnit do něj indetifikátor toho čipu,
který chceš programovat + zjistit, jak jsou pro ty čipy nakonfigurované "některé věci" (z datasheetu - prý obtížné)
Nemělo by se stát, že se sejde napájení externí a interní (z počítače), proto je dobré připojovat Shupito s volbou (interní) napájení vypnuto 
Je potřeba vybrat správný mód pro programování: pro Megu a ATtiny SPI, pro XMegu PDI, ostatní podle názvu
Program určený do čipu nahraju do Shupita tlačítkem Load/otevřít
Frekvence programování musí být minimálně 8x nižší, než je frekvence procesoru (aby to procesor pobral)

Popisky na Shupitu: 1. řada - SPI, 2. řada PDI, 3. řada JTAG - odkud ???
Druhá sériová linka pouze na 3,3 V vedle USB. 
Shupito tunel - vlastně sériová linka skrz Shupito?? 

Od 26. minuty jsou pojistky a spol - to už nepíšu 