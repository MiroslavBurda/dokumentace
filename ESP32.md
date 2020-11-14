## ESP vs. ATMega

- esp umí dynamickou správu paměti 
- esp umí hardwerově ovládat spoustu periferií (nemusí se to programovat, stačí to nastavit),
např.  enkodér, 
-esp má 3 UART 
- esp má dvě jádra, řádově vyšší frekvenci a paměť 
- esp má wifi a bluetooth 
- esp už má v sobě cosi jako operační systém a je zde k dispozici celá cpp standardní knihovna 


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

------------
- https://navody.arduino-shop.cz/navody-k-produktum/vyvojova-deska-esp32.html : zprovoznění ESP32 pod Arduino IDE
------------
wifi: esp připojuje k počítači  
potřebuješ, aby wifi vytvářel počítač a esp se připojovalo, lze to i naopak, ale to by 
připojování trvalo hrozně dlouho 

bluetooth : počítač se připojuje k esp -> když se do esp nahraje nový program, 
tak se esp restartuje -> musíš se připojit znovu , řešení: program napsat univerzálnější 
a pak ladit konstanty po sériové lince přes bluetooth 

------------------
http://esp-idf.readthedocs.io/en/latest/api-reference/peripherals/ledc.html
https://github.com/espressif/arduino-esp32
https://github.com/espressif/arduino-esp32/blob/master/cores/esp32/esp32-hal-ledc.c
https://github.com/kriswiner/ESP32/blob/master/PWM/ledcWrite_demo_ESP32.ino

