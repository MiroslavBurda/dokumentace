moje knihovna má mít příponu .h a uvozovky a být umístěná ve stejném adresáři jako hlavní program 

## Číselné proměnné 
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


## Ukazatele 

```C
#include <iostream>
using namespace std;

int a = 0; 
int* p = &a;  // *znamená, že p ukazatel   &a je oblast, na kterou se dá ukazovat    lze zapsat i takto:  int *p = &a;

cout << a >> endl; // totéž vypíše cout << *p >> endl;


Arimetika ukazatelů 

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
---------------------------------------------------

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


