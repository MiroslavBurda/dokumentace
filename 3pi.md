# 3pi
## Manuály pro 3pi
https://robotikabrno.cz/sokolska/harmonogram-krouzku/krouzek-2015-2016
Kroužek č. 17 a další

Konkrétně: 

http://technika.junior.cz/trac/wiki/pololu_3pi

https://github.com/Tasssadar/3piLib/blob/master/3piLibPack.h

https://github.com/Tasssadar/3piLib/wiki


## Úvodní video: 
https://www.youtube.com/watch?v=Yj6yPNEwbaI&feature=youtu.be 
### Obsah úvodního videa: 
Zapnuté 3pi - měly by svítit ze spodu dvě (zelené) LED 

BT modul má jednu zaslepenou díru na pin, který musí pasovat na useknutý pin na robotovi. 
Také je obvykle popis na BT modulu směrem dopředu (od displeje ven).

Když je připojený bluetooth modul, jedna zelená led by měla svítit a druhá červená nebo oranžová blikat.

zapnout lorris - modul programátor - připojení -> měl bych uvidět můj (virtuální) port, pod kterým se zub našel a aktivoval 
podle potřeby ho přejmenuju a připojím se, přitom pro zub není potřeba nic nastavovat (možnosti nastavení se tady nepoužijí) 
po připojení by blikající červená LED měla svítit 
Tlačítko Start/Stop zahajuje/ukončuje komunikaci s robotem, ale neodpojuje se od robota. 
Aby bylo poznat, že vše funguje, měl by robot posílat nějaká data. 
atmel studio - soubor projektu má koncovku atsin a ikonu berušky -> otevře projekt, vpravo vyberu file solution - zobrazí soubory .h a .cpp
Menu Build -> Build Solution -> provede překlad (Build) a vytvoří mimo jiné v adresáři Debug soubor .hex
zpět do Lorris -> otevřít soubor .hex (a zkontrolovat podle cesty a času, že je to on) 
tlačítko Stop, tlačítko Programovat, tlačítko Start 

# Použití 3pi

3pi má zásadní problém v tom, že nemá volné piny pro připojení čehokoliv. Takže buď se použije např. pro sledování čáry tak, jak je, 
nebo se k němu musí připojit rozšiřující deska. Pro každou rozšiřující desku se musí: 
a) najít piny, kam se připojí (obětují se některé funkce základního 3pi)
b) napsat knihovnu pro komunikaci mezi 3pi a rozšiřující deskou 
c) vyřešit uchycení a napájení rozšiřující desky

### Možné rozšiřující desky

https://www.pololu.com/product/2152 - máme v krabici 3pi
https://www.pololu.com/product/2150 - máme v krabici 3pi
na testování dobré, ale máme pouze 1 kus a je poměrně drahý -> když se poláme, je to v ... 
programování: https://www.mbed.com/en/


Petr Bobčík rozšiřující deska na 3pi: 
https://robotikabrno.cz/robotarna/projekty-a-socky/soc-2017-petr-bobcik-nadstavbova-deska-pro-robota-pololu-3pi

Enkodéry na MISO, MOSI (SPI) - je potřeba PB3 a PB4
jedna kompletní osazená deska Petrova je v krabici se 3pi - problém: obtížná replikovatelnost a nejsou dopsané knihovny(API), musely by dopsat 
Kuba (nebo Petr?), prý by měly fungovat enkodéry a měření vzdáleností (myslí si Jarek); deska nejede (myslí si Petr)

Možná by se dala použít ALKS:
a) máme jí hodně
b) umíme na ní hodně věcí udělat/zprovoznit


# Poznámky

bluesoleil space: program byl použitý, protože bluetooth bez něj nejelo 

-------------------------------------------------
Atmel Studio: lepší projekty kopírovat a přepisovat 

void run() - v něm bude setup + nekonečná smyčka

nový projekt - C++ Executable project, 
potom v Solution explorer přidat "junior.h" :
pravý klik na název projektu, -> add -> existing item ... 



### Mail od Vojty Bočka: 
POWER tlačítko je vedle displeje nalevo.
   
Spárovat BT modul s telefonem nebo s počítačem, PIN je 0007.
  
Přes mobilní nebo počítačovou lorris naflashovat přes avr232boot program PIDLine: 
https://raw.githubusercontent.com/Tasssadar/PIDLine/master/default/PIDLine.hex (pořeba "uložit jako" v prohližeči).
    
Pokud nejde v programátoru stopnout/naflashovat, bude potřeba pomocí shupita flashnout správný bootloader: 
https://raw.githubusercontent.com/Tasssadar/3piLib/master/bootloader-m328-20000000-115200-usart0-wdt-PB4-eeprom.hex
V tuhle chvíli funguje ježdění po čáře, ovládá se pomocí tlačítek na zadní straně 3pi.
    
Nastavení joysticku v mobilní lorris je přiložené.
    
Občas se těsně po připojení špatně přijmou data z joysticku 
a 3pi se rozjede plnou rychlostí vpřed - v takovém případě použij restartovací tlačitko vlevo od displeje.

Mail + screenshot je ze 7.11. na gmail.com

-------------
P.S. Vojta má půjčené 3pi č. 2



### Testovací program pro 3pi

```
/*
 * _3pi_BatteryControl.cpp
 *
 * Created: 23/10/2014 18:03:09
 *  Author: noname
 */ 

#include "3piLibPack.h"

void run()
{
	display.printNumToXY(getBatteryVoltage(), 4,0);
	send_int(rs232, getBatteryVoltage());
	rs232.send("\n");
	
	waitForButton(BUTTON_A);	// cekam na tlacitko
	setMotorPower(100,100);		// jedu 100
	delay(1000);				// cekam 1000 ms = 1 s
	setMotorPower(0,0);			// zastavim
	
	while(1)
	{
		display.printNumToXY(getBatteryVoltage(), 4,0);
		rs232.send("\n");
		rs232.sendNumber(getBatteryVoltage());
		delay(100);
	}
}
```

