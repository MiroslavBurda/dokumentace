### Antiviry shazují Lorris 
musí se vypnout alespoň ochrana v reálném čase -> AVG, Avast ... 

## Joystick připojit přes lorris 

otevři lorris

připoj se k zařízení (nebo ne) 

vyber analyzér > script 

pravým na horní lištu > zobrazit zdrojový kód scriptu 

z ikonek vyberu žárovičku (příklady) > vyberu joystick 

po vybrání by se mělo objevit menu, ze kterého vyberu připojený joystick 

důležitá je metoda  axesChanged(axes) a buttonChanged(id, state):
pro posílání dat  do čipu je v příkladu Controls metoda lorris.sendData

## Kalibrace joysticku
Start > Ovládací panely > Zařízení a tiskárny, 
poté pravý klik na joystick a vybrat nastavení herního zařízení, 
poté klik na tlačítko vlastnosti, následně vybrat záložku Nastavení a klik na kalibrovat.


## Lorris 
aby fungoval skript, musím ho použít -> zelená šipka v menu nahoře nebo F5, totéž pro každou změnu skriptu 

terminál si otevřu na připojení, které vidím při programování ALKS 
terminál i analyzér používají stejné připojení, pouze v různých záložkách, takže se připojují i odpojují současně 

pro programování (z VSCode) musím terminál (z Lorris) odpojit a po naprogramování opět připojit 

nový terminál otevřu klepnutím na modré plus vedle záložek 

když chci rozdělit okno, chytím záložku a táhnu např. doleva, musím se ukazatelemm myši dostat přesně 
na červené "rozdělit", které se úplně vlevo objeví , až se zežlutí, tak pustím 

část skriptu v příkladu pro Lorris 1. varianta

```python
def axesChanged(axes):
    terminal.appendText("Axes changed: " + str(axes) + "\n");
    res = "";
    for i in range(0, joystick.getNumAxes()):
        res += str(i) + ": " + str(joystick.getAxisVal(i)) + ", "; # SEE THIS: Get actual axis value. From -32768 to 32767
    terminal.appendText(res + "\n\n");
    
    val = joystick.getAxisVal(0)
    lorris.sendData([0x80, val >> 8, val & 0xFF ]) 
```    

část skriptu v příkladu pro Lorris 2. varianta

```python
# Called on axes change. "axes" is Array of ints with ids of axes which were changed
def axesChanged(axes):
    terminal.appendText("Axes changed: " + str(axes) + "\n");
    res = "";
    hodnoty = [0x80]
    for i in range(0, joystick.getNumAxes()):
        hodnoty.append(joystick.getAxisVal(i)>>8 )
        res += str(i) + ": " + str(joystick.getAxisVal(i)) + ", "; # SEE THIS: Get actual axis value. From -32768 to 32767
    terminal.appendText(res + "\n\n");
    
    lorris.sendData(hodnoty) 

# Called when button status is changed, for each button separatedly
# Pressed: state == 1
def buttonChanged(id, state):
    terminal.appendText("Button " + str(id) + ", state " + str(state) + "\n");
    lorris.sendData([0x81, id, state]) 
```

Odpovídající program v C pro ALKS pro 2. variantu 

```C
#include <Arduino.h>
#include <Wire.h>
#include "Pixy2I2C.h"
#include "time.hpp"
#include "Pixy2Line.h"

static const uint32_t i2c_freq = 400000;

Pixy2I2C pixy;
static const uint8_t Pixy_addr = 0x54;
byte L_R_light = 0; 

void setup() {
    Serial.begin (115200);
    Serial.print ("Starting.../n");
    pinMode(L_R, OUTPUT); 

   // digitalWrite(L_R, HIGH);
}

 timeout send_data { msec(100) }; // timeout zajistuje posilani dat do PC kazdych 100 ms

void loop() // this part works in cycle 
{
    if (send_data) {
        send_data.ack();
        // Serial.println (millis());
        if (L_R_light == 0) L_R_light = 1; else  L_R_light = 0;
        digitalWrite(L_R, L_R_light); 
        
    }
    int8_t i[7];
    if (Serial.available() > 0) {
        uint8_t test = Serial.read();
        if (test == 0x80) 
            for (uint8_t x = 0; x < 6; x++)  {
                i[x] = Serial.read();
                Serial.print(i[x], DEC);
                Serial.print(" ");
            }
        else if  ( test == 0x81 ) {
            Serial.println(Serial.read(), DEC);
            Serial.println(Serial.read(), DEC);
        } 
    Serial.println(" ");
    }  
}
```

## Připojení joysticku k EV3 pomocí bluetooth 

1. Na EV3 musí být program, který přijme vše, co se z bluetooth vyšle, 
pokud zůstanou vysílaná data "v luftě", tak program na kostce časem zhavaruje 

2. PC se musí spárovat s kostkou (na obojím povolit BT a na PC vyhledat kostku, 
toto se provede pouze jednou pro každé zařízení, podruhé už je zařízení v seznamu 
vyhledaných, dokonce i když je vyplé nebo mimo dosah a znova se proto nenajde)

3. potom se přes Lorris na kostku připojit (připojením přes BT se  vytvoří další COM port, 
ten se potom vybere v Lorris (vlastně se vytvoří 2 com porty, musíte vybrat/tipnout ten správný) 
nastavovat přenos se v lorris nemusí 
V analyzéru vytvořte nový skript, klik na žárovičku (vzorové příklady), otevře se okno, 
přepnout nahoře na programování v Pythonu, vybrat příklad pro joystick, 
z Tasemnice stáhnout kód pro EV3 protokol, 
- technika.tasemnice.eu.trac   - komunikace joystick - PC - lego 
yunibear - transmitter - v C napsaná vysílací strana pro Lego 

vše složit dohromady a spustit 

-----------
musí se napřed 1) najít 2) spárovat 3) připojit v Lorris na vhodný COM 
defalutní pin 1234 - v PC se snažím připojit, na kostce to potvrdím, na PC to pak vytvoří COM port, ten najdu v Lorris 
(vytvoří dva COM porty, jeden pro směr do kostky, druhý pro směr do PC, přitom oba komunikují oběma směry, 
ten, co se připojuje déle, je ten správný pro připojení Lorris )
na PC se připojuje Bluetooth 
až se změní ikonka vlevo nahoře, tak je vše připojeno 

při vysílání se musí dodržet legový protokol, jinak to ta kostka nepobere a nic neudělá 


