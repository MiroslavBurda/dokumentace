## RBC 

https://rbcontrol.robotikabrno.cz - web s příklady kódu pro RBC, u nejspodnějšího je i příklad pro *platformio.ini*

https://github.com/RoboticsBrno/RB3201-RBControl-library

Dokumentace RBControl knihovny:  
https://roboticsbrno.github.io/RB3201-RBControl-library/index.html - popis namespace rb


### Deska

ultrazvuk : na trigger stačí 3,3V, Echo posílá 5V, musí se snížit na 3,3V buď převodníkem úrovní
nebo napěťovým děličem (ten funguje pouze jednosměrmně) 

převodník úrovní je napojený na piny: SDA , SCL, (neboli I2C) 
a dále na piny IO14 a IO4 (jsou součásti skupin pro inteligentní LED úplně vpravo na desce) 

I2C je na desce celkem 4x: 3x samostatná skupina (č.18), po čtvrté ve skupině pinů (č.20) 
(já: a co skupina č. 3?)

Drivery pro motory se dají paralelizovat (pouze na stejné desce), to znamená, že mohou motoru dávat 2x vetší proud. 

Na desce vpravo nahoře je pin IO35:  
 je na něm vyveden pin z ESP32 a současně je na stejný pin přivedeno ENC8B - B enkodér pro 8. motor (každý enkodér má dva vývody, A a B).
Podobně i další piny, kde jsou enkodéry.

Stabilizátor na desce (č.9) dodává 1A, proudová rezerva je asi 0.5A, když zkusím tahat víc, tak se stabilizátor vypne, neb má v sobě pojistku.

Pod ESP32 je jumper, pokud není zkratovaný, tak piezzo nepípá.

kabely od hallových sond pasují přesně do pinhedů u motorů 

motory jsou v SW jsou definované 0-7, i když jsou číslované 1-8 

na chladiči stabilizátoru uprostřed desky je zem, na chladičích vstupních tranzistorů je napájecí napětí 


#### Expandér

Expandér pinů má tříbitovou adresu, takže jich může být 8 tohoto typu (expandéry jsou různých typů, včetně "rychlých" expandérů, které zvládnou i ovládání modelářských serv).

Ve výchozím zapojení jsou všechny adresní piny přivedené k zemi, tj. nastavené na 0, 
tomu odpovídá adresa expandéru, viz datasheet expandéru, přitom jeden adresní pin 
se přes proškrábnutí propojky a přivedením napětí dá snadno změnit 

Expandéry jsou pomalé, proto jsou vhodné např. pro řízení motorů a pod. , ale ne pro např. modelářské servo.


### LED na RBC 

Mezi každým pinem a RBC nebo expandérem je ochranný rezistor. Proto lze např. LED připojit k libovolnému výstupnímu pinu bez nutnosti dalšího rezistoru. 

#### Inteligentní LED:   
očekávaný proudový odběr:   
driver pro každou 1mA + samotná LED 20-30mA každá 

Piny pro inteligentní LED  (úplně vpravo na desce):  
odshora dolů: GND, 5V, IO14, GND, 5V, IO4, přitom horní visí na horním spínaném zdroji a spodní na druhém odshora, bez nich tam bude pouze signál 

#### Výkonové LED

se  nejlíp připojí přes odpor na baterku, případně na spínaný zdroj (ohlídat napětí)

### Tlačítka 
Jak připojit k RBC další tlačítko? Mezi pin tlačítka a zem.

Stane se něco, když se reset nebo off drží delší dobu? Ne. 


## Programování RBC

#### Postup 
Pro programování si ode mě nechte poslat poslední verzi **celého** projektu, 
kde budou aktuální knihovny   
Potom do *main.cpp* napíšete nebo zkopírujete váš zdrojový kód nebo upravíte ten původní. 


man->motor(LEFT_MOTOR)->drive(256 * (c - '0'), 64, nullptr); // parametry: počet tiků, výkon, funkce 
vevnitř je schovaný čtení enkodéru, to zajistí otočení kola o daný počet tiků (96 tiků na otáčku) daným výkonem (-100, 100) a po skončení může zavolat danou funkci 

<> značí šablonu 
když napíšu funkci nebo třídu jako šablonu, tak můžu jedním kódem obsáhnout sadu podobných variant a konkrétní typ proměnné nebo hodnotu doplnit až až při použití v kódu (parametr musí být známý při kompilaci) 
kompilátor potom z šablony vytvoří konkrétní funkce nebo třídy - viz parser.hpp 

reference musí vždy ukazovat někam a vždy na stejné místo v paměti 
ukazatel nemusí ukazovat nikam a místo, kam ukazuje, se může měnit 

#### Časovač typu timeout

více časovačů -> ano, kolik se vleze do paměti (stovky) , jsou postavené na funkci micros(); když smyčka dojde k němu, tak zkontroluje, jestli přetekl časovač, pokud ano, tak provede, co se po něm chce. Zároveň je nutné ho resetovat (metoda .ack() ). 

**Závěr**: nezdržuje běh programu jako funkce delay(), ale čeká, až k němu program ve smyčce dojde -> může trvat déle než nastavená hodnota. 

#### časovač typu schedule 

jede v milisekundách a přesně, protože je postavený na přerušení

knihovna atomic.h (pouze pro ESP, ne pro Arduino) má v sobě knihovnu mutex.h 
mutex zamkne kritickou část kódu, provede, co je potřeba a pak ji zase odemkne 
atomic to má v sobě interně, uživatel to nemusí řešit 


---------

U serva je nějaká chyba, která způsobuje, že nestačí zapnout attach, ale je potřeba poslat do serva první pokyn, aby začal attach posílat do serva signál.





