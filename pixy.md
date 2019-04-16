
dokumentace k Pixy2:
https://docs.pixycam.com/wiki/doku.php?id=wiki:v2:pixy2_full_api#plugin_include__wiki__v2__ccc_api

nákup za CZK: 
https://cz.mouser.com/ProductDetail/SparkFun/SEN-14678?qs=gTYE2QTfZfThcsWCHU6Kqw%3D%3D

úvodní strana webu 
https://pixycam.com/pixy2/

### Piny na ALKS pro I2C (Pixy): 

SDA - 27, druhý vpravo nahoře vnitřní řada 
SCL - 14, první vpravo nahoře vnitřní řada 
pinout ALKS je u mě v souborech 

### Pixy - zkušenosti a ovládání 

dokud pixy nepřipojíme, tak se s námi obslužný a kalibrační program v PC (Pixymon) nebaví 
totéž program v ESP32, napájení Pixy: 5V, datové kabely zvládnou 3,3V > přímo připojit k RBC nebo k ALKS 

File > Configure > Pixy parametrs > Tuning 
Camera brightness cca 100
Signature range cca 3,8 - 4 
> Interface 
Data out port I2C

Zapnout/vypnout  osvětlení LED:
Action > Toggle lamp 

Nastavení hledání čáry (Pixy2):
Program > line_tracking

Nastavení hledání barevných bloků:
Program > color_connected_components

Nastavení hledané barvy: 
Action > Set signature 1 ... 

Nastavení jména dané barvy:
File > Configure > Pixy parametrs > signature labels 

## Příklad zdrojového kódu 

tento program je také na 
https://rbcontrol.robotikabrno.cz/index.php

#include <Arduino.h>
#include <Wire.h>
#include "Pixy2I2C.h"
#include "time.hpp"
#include "Pixy2Line.h"

static const uint32_t i2c_freq = 400000;

Pixy2I2C pixy;
static const uint8_t Pixy_addr = 0x54;
byte L_R_light = 0;   // cervena dioda indikuje provoz

void setup() {
    Serial.begin (115200);
    Serial.print ("Starting.../n");
   
//Wire.begin
 // inicializace Pixy2
    pixy.init(Pixy_addr);
    Serial.println (pixy.changeProg("line"));
    pinMode(L_R, OUTPUT); 

   // digitalWrite(L_R, HIGH);
}

 timeout send_data { msec(500) }; // timeout zajistuje posilani dat do PC kazdych 500 ms

void loop() // this part works in cycle 
{
    if (send_data) {      // if časovač dosáhne nastavené hodnoty, proveď
        send_data.ack(); // nuluje časovač send_data 
        // Serial.println (millis());
        if (L_R_light == 0) L_R_light = 1; else  L_R_light = 0;
        digitalWrite(L_R, L_R_light); 
        pixy.line.getMainFeatures();
        if (pixy.line.numVectors) pixy.line.vectors->print();
        if (pixy.line.numIntersections) pixy.line.intersections->print();
    }
}

----------------------------------------------------------------








