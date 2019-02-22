Zprovoznění EV3: 

pokud nemůže najít (např. při kompilaci) cestu k EV3 Extension, tak zadám 
C:\Program Files (x86)\JSiebSW\Cpp4Robots

Nainstaluju MS Visual studio a CPP4Robots podle návodu na www.cpp4robots.cz
Založím nový projekt File > New > Project , pojmenujete a necháte zatržené "Create directory for solution" , to vytvoří 
adresář se jménem projektu a několik souborů, které budu potřebovat 
Nechám vytvořit nový projekt 

Otevřu 
Solution Explorer ( View > Solution Explorer ) , vyberu adresář projektu, potom adresář Main, v něm EV3_main.cpp -> otevřu poklepáním 

použiju ikonu Nápověda z panelu CPP4Robots, kliknu na Lego EV3 API, potom EV3 Brick User Interface > LCD Display > 

zkopíruju "#include "EV3_LCDDisplay.h" " do  EV3_main.cpp na začátek , ale #include se podtrhne , protože neví kde ho má hledat. 
Řešení: Říct MS visual studiu, kde má najít soubory include : 

Solution Explorer, 
pravým na EV3_main.cpp, otevře se okno Property pages, vlevo vyberu C/C++ / General , vpravo vyberu 1. položku 
"Aditional include Directories" , nastavíte 
C:\Program Files (x86)\JSiebSW\Cpp4Robots\TargetDevice\Lego\EV3\API\include

Dále je potřeba otevřít okno Output : View > Output 

a Error List: View >  Error List 

další řádek: podle nápovědy je : Namespace : ev3_c_api , což my musíme napsat takto: 

using namespace ev3_c_api;

aby se něco vypisovalo, použiju z nápovědy níže funkci Draw_Text a doplním potřebné parametry 

celý program je zde: 

#include "EV3_LCDDisplay.h" 
using namespace ev3_c_api;

int EV3_main()   {
	bool bRet = Draw_Text(3, E_Font_Normal, false, "Hello_Word");
	if (bRet == false) return 1;
	return 0;
}
varianta: 
int EV3_main()   {
	if ( !Draw_Text(3, E_Font_Normal, false, "Hello_Word") ) return 1;
	return 0;
}

doplnit program o čas čekání, aby bylo na displeji něco vidět 
----------------------------
#include "EV3_LCDDisplay.h" 
#include "EV3_Thread.h" 
using namespace ev3_c_api;

int EV3_main()   {
	// bool bRet = Draw_Text(3, E_Font_Normal, false, "Hello_Word");
	if ( !Draw_Text(3, E_Font_Normal, false, "Hello_Word") ) return 1;
	EV3_Sleep(5000);
	return 0;
}
----------------------
po úspěšném compile dát build, 

Zapnout kostku, počkat, až naběhne, pak ještě chvilku počkat, 

ikona download  bez SD karty se  mi program nezobrazí na kostce , ale bude tam, 
ikona start > zapnu program z počítače 
pro provoz bez kabelu potřebuju SD kartu 

namespace myev3{

	void test() {
	}
} // vytvořím vlastní namespace

using namespace myev3; // zpřístupní funkce z myev3, jinak je linker nevidí 

myev3::test(); // použij funkci test z namespace myev3 
::test(); //  použij funkci test z globálního namespace 


program s logovacím souborem: 
----------------------------
#include "EV3_LCDDisplay.h" 
#include "EV3_Thread.h" 
#include "EV3_File.h"

using namespace ev3_c_api;

int EV3_main()   {
	T_HandleFile hfile = CreateFile( "EV3_dbg.log" , false, true);
	if (!hfile) 
		return 1;
	WriteFileString(hfile, "State 1\r\n");
	if ( !Draw_Text(3, E_Font_Normal, false, "Hello_Word") ) 
		return 1;
	WriteFileString(hfile, "State 2\r\n");
	EV3_Sleep(5000);
	WriteFileString(hfile, "State 3\r\n"); // \n je znak konce řádku
	CloseFile(hfile);
	return 0;
}
------------------------------------
Jak se dostat ke vzniklému souboru: 
na liště EV3Toolbar tlačítko FileExplorer -> otevřu, přejdu do adresáře SD_card, zkopíruju na local a otevřu 

makecode.mindstorms.com 

## ev3manager.education.lego.com -> instalace upgrade firmware -> pro provoz  makecode.mindstorms.com  je nutná verze 1.10 

---------------------------------------------------
číselné proměnné 
(un)signed char 1 bajt - číslo  // char 1 byte je znaková proměnná  
short 2 bajty 
int 4 bajty
long int 4 bajty 
long long int 8 bajtů 

bool 

float 4 bajty
long float 8 bajtů 
zápis s desetinnou tečkou 
sizeof(float) - zjistí velikost datového typu nebo proměnné v bajtech 

výstup do konzole: 
cout << "co chci vypsat" >> endl;
--------------------------------------------------

#Ukazatele 

##include <iostream>
using namespace std;

int a = 0; 
int* p = &a;  // *znamená, že p ukazatel   &a je oblast, na kterou se dá ukazovat    lze zapsat i takto:  int *p = &a;

cout << a >> endl; // totéž vypíše cout << *p >> endl;

arimetika ukazatelů 
p=p+1 // posune se o jedno místo v paměti, ale protože int má 4 bajty, tak se posune o 4 bajty dál 

----------------------------------------------------------

Reference - je nový název pro stejnou proměnnou 

int a = 10; 
int& r = a;  // definuju referenci r na proměnnou a 

--------------------------------------------------------------

Pole 

int a[5] = {0};  // vytvořili jsme 5 proměnných typu int (pole a), ve všech je 0
navíc se proměnná a chová jako ukazatel na toto pole takže *a ukazuje na první prvek pole a[0] a po příkazu a=a+2 bude *a ukazovat na třetí prvek pole a[2]

taky jsou možné zápisy: 

int a[5] = {0, 1, 2, 3, 4};
int a[] = {0, 1, 2, 3, 4};
int a[10] = {0, 1, 2, 3, 4}; // do ostatních prvků v poli doplní nuly 
--------------------------------------------------------------------------

Konstanty 

const int c = 20; // klasická konstanta 

a = 10;
int* const p =&a  // konstantní ukazatel - ukazuje pořád na stejné místo v paměti, tu pamět samotnou můžu měnit, const je vpravo od *
const int* p = &a // ukazatel na konstantu - konstanta a se nemění, ukazatel se může posouvat , const je vlevo od * 

můžu mít i konstantní reference, potom reference se nemění, ale proměnná, na kterou referuju, se měnit může 

--------------------------------------------------------------

Funkce 

int sum(int a, int b){  // volání hodnotou - klasické 
	int s = a + b;
	return s;
}

int main() {
	int x = sum(2, 4);  
 
}
**************************************** 
int sum(int& a, int& b){ // volání proměnné odkazem (pomocí reference) -> může přepsat vstupní proměnné !!
	a = 10; b = 20;
	int s = a + b;
	return s;
}

int main() {
	int x1 = 2;
	int x2 = 3;
	int x = sum(x1, x2); 
 
}

**************************************** 
int sum(const int& a, const int& b){ // volání proměnné odkazem (pomocí reference) použití const zabrání nechtěnému přepsání 
	a = 10; b = 20;
	int s = a + b;
	return s;
}

int main() {
	int x1 = 2;
	int x2 = 3;
	int x = sum(x1, x2); 
 
}





