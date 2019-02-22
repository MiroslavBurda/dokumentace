# Podněty ke zpracování do _dokumentace - neroztříděné


- Příklad plánu výroby robota 
- udělat rychle nákupy  pro alks
- jak umisťovat díly na překližku ( 4 mm překližka topol 100/1,1 radius 0,3 ) když něco nefunguje, proklikat vícekrát dokola 
- bitbucket pro soukromé repozitáře www.bitbucket.org -> přidat do textu 
- https://navody.arduino-shop.cz/navody-k-produktum/vyvojova-deska-esp32.html : zprovoznění ESP32 pod Arduino IDE

- http://www.state-machine.com/
- https://en.wikipedia.org/wiki/UML_state_machine
- https://en.wikipedia.org/wiki/Event-driven_programming
- \usepackage{makeidx} % slouží pro vytváření indexů/rejstříků -> prý existuje \usepackage{imakeidx}
- https://www.tme.eu/cz/  https://www.soselectronic.cz/
- https://www.tme.eu/cz/details/esp32-devkitc/vyvojove-kity-ostatni/espressif/
- ESP32 DEVKITC - aliexpress
https://www.aliexpress.com/item/1pcs-ESP-32S-ESP-32-Development-Board-WiFi-Wireless-Bluetooth-Antenna-Module-For-Arduino-2-4GHz/32837344610.html?ws_ab_test=searchweb0_0,searchweb201602_1_10065_10068_10059_10884_10887_10696_100031_10084_10083_10103_10618_10304_10307_10820_10821_10302,searchweb201603_60,ppcSwitch_4_ppcChannel&algo_expid=4e59e96c-86d2-404c-9408-780d715a48ab-26&algo_pvid=4e59e96c-86d2-404c-9408-780d715a48ab&transAbTest=ae803_2&priceBeautifyAB=0
- program posterazor - freeware - vyrábí A4 z plakátů pro tisk 

- https://www.python.org/downloads/

- https://naucse.python.cz/course/pyladies/  Python pro úplné začátečníky 

- do Lorris doplnit odkaz http://tasssadar.github.io/Lorris/cz/index.html

- technika.tasemnice.eu.trac   - komunikace joystick - PC - lego 
yunibear - transmitter - v C napsaná vysílací strana pro Lego 

musí se napřed 1) najít 2) spárovat 3) připojit v Lorris na vhodný COM 
defalutní pin 1234 - v PC se snažím připojit, na kostce to potvrdím, na PC to pak vytvoří COM port, ten najdu v Lorris 
(vytvoří dva COM porty, jeden pro směr do kostky, druhý pro směr do PC, přitom oba komunikují oběma směry, 
ten, co se připojuje déle, je ten správný pro připojení Lorris )
na PC se připojuje Bluetooth 
až se změní ikonka vlevo nahoře, tak je vše připojeno 

při vysílání se musí dodržet legový protokol, jinak to ta kostka nepobere a nic neudělá 

- https://arduino-shop.cz/arduino/3182-led-displej-7-segmentovy-8-znaku-max7219-cerveny.html
https://www.broadcom.com/products/optical-sensors/integrated-ambient-light-proximity-sensors/apds-9930
proximity sensor IR LED - 3-10cm I2C 

- https://navody.arduino-shop.cz/navody-k-produktum/led-displej-max7219.html
LED displej 8 znaků I2C



#alks

mají asi 25 alks, 
samotná alks 50,- Kč
osazení cca 150,- Kč 
součástky ?? -> NAKOUPIT
arduino nano z číny 50,- Kč 
esp32 300,- může být i z česka, rozdíl je malý 

## ať si všechno píšou a zálohují 



##Micro:bit
- české stránky 
- simulátor na anglických 
- připojování zařízení v reálu musí někdo zkontrolovat, jinak hrozí zničení desky 
- připojování zařízení (motory, serva, ....) v reálu NEODPOVÍDAJÍ zapojení zobrazeném v simulátoru  
- Jirka po debatě s Kubou říkal, že je ideální na simulátor, ale ne pro robota 
- Jarek: poměrně málo pinů, nemáme dořešené silové obvody, na 1 pinu výstup pouze 5mA, to nestačí ani na LED

- lze připojovat další senzory 


##Baterie na Robotárně a v míčkoflusu - Panasonic NCR18650PF

Panasonic NCR18650PF
https://lygte-info.dk/review/batteries2012/Orbtronic%2018650PD%202900mAh%20(Black)%20UK.html

## Driver DRV8833

https://www.dx.com/cs/p/1a-drv8833-dual-motor-driver-module-full-bridge-driver-for-arduino-444737?tc=CZK&ta=CZ&gclid=CjwKCAjwio3dBRAqEiwAHWsNVZI1OFzlk1F0zp9HKbXgQsIOgOh0NN7lSES-B3Vujni9Hseh85foFBoCsc4QAvD_BwE#.W6NN2bi1gqE

## starší kroužky 

Záznamy kroužek 2017-2018:
https://www.youtube.com/watch?v=2JSfwRe2czI&list=PL9oWsg5StwvaR75fhpp8OLeW4F5wTJRSd

Záznamy s 3pi
https://robotikabrno.cz/sokolska/harmonogram-krouzku/krouzek-2015-2016


## ARM 


## Prostředí pro programování mobilů 

https://www.blynk.cc/   <- tip od Jarka Párala 
Pocked code <- tip ze sočky Martiny Hanusové 

## Pájecí pasta z tábora 

https://www.gme.cz/gelove-tavidlo-future-rework-jelly-30-g

## Když nejede poprvé upload do ESP32

Pokud je build programu v pořádku a při prvním pokusu o nahrání programu do čipu na windows se objeví hláška: 

Looking for upload port…

Error: Please specify upload_port for environment or use global --upload-port option

může pomoct instalace virtuálního com portu pomocí driveru:  

CP2102 USB to UART Bridge Controller

https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers 

dále: 

To find the USB port: Hit WindowsKey-X, select Device Manager, plug in the device and observe what's listed under Ports (COM & LPT) - the one that just appeared has the port in brackets (COMn).

Then in platformio.ini in your PlatformIo initialised project folder, you specify the port as a line under the platform section (env: square brackets line): upload_port = com9 or whatever you got from Device Manager.

I hope this helps.

 ## Krokové motory 
 
 unipolární / bipolární 
 krok, půlkrok, mikrokrok 
 driver - buď udělá fázový posun nebo nebo přímo posílám po SPI o kolik kroků se má otočit 
 4 piny enable - drží motor když se neotáčí, dir - směr otáčení, ??? - jednotlivé kroky 
 unipolární - má společné + a na krajích cívky mínus , má 5 drátů 
 univerzální motor 8 drátů -> dvě cívky se dají zapojit do série (velký proud), paralelně (napětí velké) a když se spojí, je z toho unipolární 
 -> je nutné koupit driver, připojit podle datasheetu na motor a zkusit rozchodit 
 

## Eagle 

http://paja-trb.cz/eagle/eagle_navod_2.html 
návod jednoduchý s překlady použitých pojmů 
design se liší, ale význam je zůstává 

https://www.robotikabrno.cz/sokolska/harmonogram-krouzku/krouzek-2014-2015 - 2x záznam s výukou Eaglu 

---------------------
https://www.root.cz/clanky/externi-seriove-sbernice-spi-a-i2c/
https://navody.arduino-shop.cz/technikuv-blog/prehled-komunikacnich-rozhrani-arduino-platforem.html

-------------------------------
Modelářská vysílačka - vysílač a přijímač na 2,4GHz
Používá PPM Modulaci 
přijímač má vyvedeno piny podle počtu kanálů + může mít vyveden ten PPM kanál 


magnetometr - je to k něčemu potřeba, nebo stačí gyroskop GY - 521 - viz Petr Bobčík 
Držák na Pixy univerzální -> Kratochvíl dostal za úkol

ovladani_dalkovych_robotu.md







------------------------------------------------------------------- 
- dodělat checklist do dokumentace
normální serva znají svoji polohu ale já ji neumím z nich při připojení vyčítst -> je nutné před zapnutím to servo ručně seřídit do požadované 
vstupní polohy, jinak s sebou "švihnou" 

--------------------------------------------------
vize: použít pult pro inspiraci, pult i moduly z překližky vyřezané na laseru, pěnu podle situace (možná taky na laseru), nebo použít na zakrytování také překližku z laseru  
do pultu dát ALKS a k ní připojit potřebné periferie - komunikovat s robotem budou přes bluetooth v ESP32 na obou stranách
napájení Powerbankou přes USB 
přidat Displej? ovládání displeje přes ALKS?  
nebo vzít playstation ovladač + Lorris na NTB a potřebná nastavení mít na NTB 
Dodržovat pravidla o složených závorkách? 
http://technika.junior.cz/trac/wiki/eurobot/reference/Formátování_zdrojových_textů
--------------------------------------------------------------
https://robotikabrno.cz/sokolska/projekty - OVládací pult, Lorris, 
https://www.youtube.com/channel/UCxLGjiNwJ9u7Wdb1kahaHgg - kanál Robotika Brno na Youtube
http://technika.junior.cz/trac/wiki/SMDkecy - Pájení SMD - stále aktuální

http://technika.junior.cz/trac/wiki/commPinout
http://technika.junior.cz/trac/wiki/shupito - stále aktuální
http://technika.junior.cz/trac/wiki/cestne_prohlaseni - NEJAKTUÁLNĚJŠÍ
http://technika.junior.cz/trac/wiki/lego
http://technika.junior.cz/trac/attachment/wiki/lego/joystick_lorris.py ?? asi pouze pro lego 

https://rbcontrol.robotikabrno.cz - pridat vse výše do dokumentace

-----------------------------------
Záznamy s 3pi https://robotikabrno.cz/sokolska/harmonogram-krouzku/krouzek-2015-2016

--------------------------

https://www.pololu.com/product/2130 -driver DRV8833

---------------------------------

- V případě commitování do manuálu dejte do commit message na začátek text "RoboticsManual: VÁŠ POPIS COMMITU"
- Pokud děláte reference v textu, nepoužívejte v nich nikdy diakritiku (viz tenhle případ) - rozbije to překlad do HTML. 

u stop tlačítka se hodí indikace, že je stiknuté 

----------------------------------------------------------
workspace - rozložení pracovní plochy

wifi: esp připojuje k počítači  
potřebuješ, aby wifi vytvářel počítač a esp se připojovalo, lze to i naopak, ale to by 
připojování trvalo hrozně dlouho 

bluetooth : počítač se připojuje k esp -> když se do esp nahraje nový program, 
tak se esp restartuje -> musíš se připojit znovu , řešení: program napsat univerzálnější 
a pak ladit konstanty po sériové lince přes bluetooth 
