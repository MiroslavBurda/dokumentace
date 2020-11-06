
```c++
# blikání led: 

#include "RBControl_manager.hpp"

extern "C" void app_main() {
    // Initialize the robot manager
    rb::Manager man;
	while true {
		man.leds().green(true);
		delay(1000);
		man.leds().green(false);
		delay(1000); 
	}
}
```

```c++

#include <Arduino.h>
#include <format.h>
using fmt::print;

#include "RBControl_manager.hpp"
#include <Wire.h>
#include "Adafruit_TCS34725.h"
#include "Pixy2I2C.h"

#include "wifi.hpp"
#include "time.hpp"

#include "parser.hpp"

// pokud se nepovede neco inicializovat (WiFi, RGB senzor), program se zasekne v teto funkci
void trap()
{
    print(Serial, "trap\n");
    while(1);
}

static const uint32_t i2c_freq = 400000;

// kam se ma pripojit WiFi; jmeno site a heslo je v souboru credentials.hpp
WiFiClient debug;
WiFiClient data;
static const IPAddress Debug_IP { 192, 168, 42, 55 };
static const uint16_t Debug_port = 12345;
static const uint16_t Data_port = 12346;

// kam je co pripojene
static const rb::MotorId Encoder_port = rb::MotorId::M1;
static const uint8_t Encoder_btn = rb::EA2;

static const uint8_t QRD_pin = analogInPinToBit(39);

static const uint8_t TCS_led_pin = rb::EA0;

static const uint8_t UTS_trig =  rb::EA1;
static const uint8_t UTS_echo = rb::IO14;

static const uint8_t Pixy_addr = 0x54;

// funkce umoznujici pristup k RBC (motory, enkodery, baterka, ledky, tlacitka, ...)
rb::Manager& rbc() {
    static rb::Manager man(false);
    return man;
}

Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_1X);

Pixy2I2C pixy;

void setup() {
    Serial.begin(115200);
    print(Serial, "RBC_sensor_demo\n");
    // inicializace WiFi
    if (!wifi::connect()) trap();
    if (!debug.connect(Debug_IP, Debug_port)) {
        Serial.println("Can not connect to the debug server");
        trap();
    }
    if (!data.connect(Debug_IP, Data_port)) {
        debug.println("Can not connect to the data server");
        trap();
    }
    print(debug, "RBC_sensor_demo\n");
    // inicializace RBC
    rbc();
    // inicializace enkoderu
    rbc().expander().pinMode(Encoder_btn, INPUT_PULLUP);
    // inicializace QRD
    analogReadResolution(10);
    // inicializace RGB
    if (!tcs.begin(TCS34725_ADDRESS, &rbc().wire())) {
        debug.println("Can not connect to the RGB sensor");
        trap();
    }
    rbc().expander().pinMode(TCS_led_pin, OUTPUT);
    rbc().expander().digitalWrite(TCS_led_pin, 1);
    // inicializace UTS
    rbc().expander().pinMode(UTS_trig, OUTPUT);
    pinMode(UTS_echo, INPUT_PULLUP);
    // inicializace Pixy2
    pixy.init(Pixy_addr);
    // reset I2C grequency back to 400 kHz
    rbc().wire().begin(-1, -1, i2c_freq);
    debug.println("Starting main loop\n");
}

timeout send_data { msec(100) }; // timeout zajistuje posilani dat do PC kazdych 100 ms
Parser<0xFF, 250> packet; // komunikacni protokol, kterym se posilaji data do Lorris (parametry 0xFF = start byte, 250 = velikost paketu - kolik bajtu umi najednou poslat)

void loop() {
    // posli data kdyz vyprsi casovac a restartuj ho
    if (send_data) {
        send_data.ack();
        // posilani enkoderu (0x01 identifikace paketu v Lorris - nahodne vybrane cislo)
        packet.send(data, 0x01,
            rbc().motor(Encoder_port)->encoder()->value(), // precteni hodnoty enkoderu
            uint8_t(rbc().expander().digitalRead(Encoder_btn)) ); // precteni tlacitka na enkoderu
        // poslani QRD
        packet.send(data, 0x02,
            analogRead(QRD_pin));
        // precteni a poslani RGB
        float r, g, b;
        tcs.getRGB(&r, &g, &b);
        packet.send(data, 0x03,
            uint8_t(r),
            uint8_t(g),
            uint8_t(b) );
        // zmereni a poslani UTS
        rbc().expander().digitalWrite(UTS_trig, HIGH);
        wait(usec(10));
        rbc().expander().digitalWrite(UTS_trig, LOW);
        packet.send(data, 0x04,
            pulseIn(UTS_echo, 1, 1000000));
        // pixy
        int8_t blocks = pixy.ccc.getBlocks();
        print(debug, "Vidim {} bloku:\n", blocks);
        for(int i = 0; i != blocks; ++i)
        {
            print(debug, "\t");
            pixy.ccc.blocks[i].print(debug);
        }
        print(debug, "\n");
    }
    // prijem dat z terminalu v Lorris
    if (debug.available()) {
        char c = debug.read();
        switch (c) {
        case '\r':
            debug.write('\n');
            break;
        case 'l': // zhasnuti LED u RGB senzoru
            rbc().expander().digitalWrite(TCS_led_pin, 0);
            break;
        case 'L': // rozsviceni LED u RGB senzoru
            rbc().expander().digitalWrite(TCS_led_pin, 1);
            break;
        case '1':
            pixy.setLamp(1, 1);
            break;
        case '0':
            pixy.setLamp(0, 0);
            break;
        }
    }
}

```
