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


	#include "EV3_LCDDisplay.h" 
	#include "EV3_Thread.h" 
	using namespace ev3_c_api;

	int EV3_main()   {
		// bool bRet = Draw_Text(3, E_Font_Normal, false, "Hello_Word");
		if ( !Draw_Text(3, E_Font_Normal, false, "Hello_Word") ) return 1;
		EV3_Sleep(5000);
		return 0;
	}

po úspěšném compile dát build, 

zapnout kostku, počkat, až naběhne, pak ještě chvilku počkat, 

ikona download  bez SD karty se  mi program nezobrazí na kostce, ale bude tam, 
ikona start > zapnu program z počítače 
pro provoz bez kabelu potřebuju SD kartu 

program s logovacím souborem: 

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

Jak se dostat ke vzniklému souboru: 
na liště EV3Toolbar tlačítko FileExplorer -> otevřu, přejdu do adresáře SD_card, zkopíruju na local a otevřu 






