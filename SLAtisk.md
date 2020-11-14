
# Seminář SLA tisk 

### Honza Mrázek 26.9.2019 na Robotárně

## Postup stručně:


1. Z CADu  vytvořím .stl soubor, ve sliceru vytvořím soubor .cbddlp a nahraju ho do SLA tiskárny. 
2. Vytisknu model nebo kopyto pro formu, umeju a vytvrdím UV světlem. 
3. Do kopyta odleju silikonovou formu. 
4. Do silikonové formy odleju výrobek z polyuretanu.  

## Postup podrobně:

### Cad a slicer

#### Poznámky k Fusion 360: 

- do formy se přidávají díly: insert derive - odvozené přidané díly mohu potom upravovat 
- napřed vymodeluju díly, vložím je do formy (pozitiv), přidám díry pro odtok 
- potom vymodeluju silikonovou formu (kvádr a odečtu jednotlivé díly - nástroj combine) (negativ)
-  naskládám všechno k sobě a použiju funkce inspect a funkce interference - zjistí, jestli se díly nikde neprotínají 
- víko formy co nejjednodušší (bez prolisů a pod) s výtokovými otvory 
- pokud je možné udělat úkos, je to lepší, stačí 2 stupně 
- nemá smysl šetřit materiálem, pokud tisknu jeden díl   
- pokud udělám v rámci šetření materiálem dutinu, musím v ní mít odtokový otvor (lépe taky druhý zavzdušňovací), jinak budu mít po dokončení tisku dutinu plnou kapalného materiálu
- maximální rozměry modelu: 120x68x150 mm

#### Slicer

Soubor .stl nahraju do sliceru [Chitubox](https://www.elegoo.com/tutorial/Elegoo%20MARS%203D%20Printer%202019.08.8.zip) a vytvořím soubor .cbddlp. Ten nahraju na flashku a flashka připojím do tiskárny. Doba tisku je cca 15 sekund na vrstvu tlustou 0.05 mm. Přitom první 4 vrstvy se musí vytvrdit mnohem déle, cca 60 sekund.

Slicer Chitubox nedokáže rozlišit "ostrůvky", které ve vrstvě pod sebou nic nemají a při pokusu o jejich tisk by se proto nespojily s předchozí vrstvou a dost pravděpodobně by někam odplavaly. Program [Photon File Validator](https://github.com/Photonsters/PhotonFileValidator) to zvládne. Slicer Chitubox umí přidat podpěry (záložka vpravo nahoře), funkce all je přidá všechny, jinak se přidávají po jedné. [Průšův Slicer](https://www.prusa3d.cz/prusaslicer/) podpěry zvládá líp.  

Pak je potřeba ve Chituboxu nastavit parametry tisku podle konkrétního materiálu  -> settings -> print. Když se to nechá exponovat moc krátce, tak se model nevytvrdí. Když moc dlouho, tak model trochu "bobtná", protože se vytvrdí i trochu materiálu okolo modelu.

V blízkosti modelu se při stejné dávce UV rezin vytvrzuje rychleji, toho se využívá při snaze o vytištění hladkého povrchu.  

Volba antialiasing level: 8 znamená, že Chitubox vytváří šedé, částečně exponované, pixely osmi úrovní šedi ^[ Ve skutečnosti pro jednu vrstvu
 vytvoří 8 obrázků místo jednoho a každý obrázek se osvěcuje 1/8 doby, kterou by se jinak osvěcovala jedna vrstva jako celek. Taky tím 8x naroste velikost výsledného souboru, který může mít klidně i přes 1 GB.]. 

#### Podpěry

Části modelu k sobě lepí vteřinové lepidlo, taky lze na modely kápnout rezin a pak na to posvítit UV světlem. 

Většinu problémů s obtížně řešitelnými místy (díry v modelu) řeší náklon modelu ve sliceru.  
Když chci novou vrstvu s velkým převisem, tak první vytvrzené vrstvy převisu jsou velmi tenké a mají sklon kroutit, opět to řeší náklon modelu.

Honza napsal program [ElegooMarsUtility](https://github.com/yaqwsx/ElegooMarsUtility), který vezme naslicovaný soubor a provede erozi, to znamená
odebere přebývající nárůst vzniklý delším vytvrzováním rezinu, jednak prvních vrstev (kolik je potřeba, tj. hodně), jednak všech dalších vrstev (mnohem míň).  

### Tisk

Na Robotárně je [SLA](https://cs.wikipedia.org/wiki/Stereolitografie) tiskárna [Elegoo Mars](https://www.elegoo.com/product/elegoo-mars-uv-photocuring-lcd-3d-printer/).

Pro samotný tisk se používá SLA rezin (=umělá pryskyřice). Různé druhy tuhnou různě rychle, nutno vyzkoušet. Tabulka expozičních dob pro různé resiny je [tady](https://docs.google.com/spreadsheets/d/1dV-mvE8IojXSNiUHf8fHf_eHJGAQzbeFEalcP4bdnsg/edit?fbclid=IwAR0Qol7JbK_3IkK8oZQguovB68OiaFHPLNQpw2yk3XwjwY5IvTyjHAqU3tw#gid=0).

Před prvním tiskem  a potom čas od času se musí nastavit horní deska, na které vzniká model, do roviny vůči nádobce s rezinem. 

Je dobré na mít při manipulaci rukavice, někteří lidé na něj mají silné alergické reakce. Když se hned umeje vodou a mýdlem, tak Honza nepozoroval podráždění. 

Rezin je hodně citlivý na UV záření, tj. i na denní světlo. Rezin má v sobě prekurzory tužidla -> po osvícení UV se prekurzor rozpadne na tužidlo a zbytek, který nás nezajímá. Tužidlo zpevní prostor osvícený UV a blízké okolí. Pokud je tiskárna pod červeným poklopem, rezin v ní netuhne a může v ní zůstat několik týdnů. 

Podle Honzy nejlepší rezin je šedý od firmy Sariya tech, téměř nesmrdí a rychle se z něj vyrábí. Výrobek není tak křehký jako z rezinu dodávaného výrobcem. Nekupovat z USA, je to drahé (poštovné, DPH).

Nejdostupnější rezin je *Elegoo LCD UV 405 nm rapid resin 1000g* na Amazon DE. Na šedé barvě je nejlíp vidět kazy (na černé a bílé jsou vidět blbě).

Po dokončení tisku model umyju v izopropylalkoholu. Pokud model následně suším např. papírovým kapesníkem, musím rezin na kapesníku nechat pomocí UV záření vytvrdit a potom můžu kapesník vyhodit. Nesmím ho vyhodit mokrý. Taky nesmím modely umývat vodou, nevytvrzený rezin je hodně jedovatý. Také izopropylalkohol znečištěný rezinem nechám vytvrdit (např. na slunci) a potom teprve tuhý vyhazuju do odpadu. 

Vytvrzování: hotový umytý model osvítím UV cca 5-10 minut. Honza na to má UV LED pásky uvnitř kovové trubky (kouřovod). Kov stíní a odvádí teplo z LED.    

### Silikonová forma

Formy pro odlévání se vyrábí ze silikonu [Lukopren N5221](https://www.lucebni.cz/cs/index.php?controller=attachment&id_attachment=43). Je velmi pevný, pružný, je velmi tolerantní na míchání (= na přesnost poměru obou složek) a je netoxický. Do něj se přimíchává 3% hmotnosti tužidla. Pro rychlejší tuhnutí lze přidat 0,5-0,75% hmotnosti vody, ale není to potřeba, stačí vzdušná vlhkost. 

Nízké řady Lukoprenu je možné mezi sebou míchat, protože používají stejné tužidlo. 
Osvěčilo se 90% N5221 a 10% N1521 - forma je pevnější. Existuje taky N1000, který fakticky funguje jako ředidlo pro ostatní Lukopreny.

Čím líp Lukopren teče do formy vyrobené v předchozích krocích, tím horší jsou mechanické vlastnosti výsledné silikovoné formy. Lukoprenu míchejte o 10 procent a ještě 5 gramů navíc. Aby se z lukoprenu odstranily bubliny, je nutné ho míchat za nízkého tlaku. Teprve po odstranění bublin se přidává tužidlo  a po opětovném promíchání (ideálně opět za nízkého tlaku) se leje do formy/kopyta. 

Pro výrobu nízkého tlaku Honza používá [potravinářskou vakuovačku](https://www.mall.cz/vakuovacky/maxxo-vmprofi?gclid=CjwKCAjwldHsBRAoEiwAd0JybcJCjSkBzwfDstq4hDjbX9ry9-HailZAa-Snq00SSkfWKjqS6W-ilRoCYfIQAvD_BwE). Je nutné ji zapínat opakovaně, protože má v sobě časovač na vypínání. Dostane se na 0,2 baru. V nádobě, kterou chci vakuovat, je malá díra pro opětovné napuštění vzduchu utěsněná špuntem.  

Výrobce Lukoprenu uvádí na svých stránkách [postupy výroby forem](https://www.lucebni.cz/cs/index.php?controller=attachment&id_attachment=44).

### Odlévání polyuretanu

Polyuretanů jsou opět různé druhy, obecně jsou o něco pevnější než ABS (materiál pro dílky z Lega).
*Rencast 5146* - půl hodiny se to může zpracovávat, po 16 hodinách to může jít z formy a za 24 hodin je to pevné , 60 zlotých 
*Rencast fc52* - 4 minuty na zpracování, za hodinu  to může jít z formy, za 4 hodiny pevné. Oboje k dostání na modelarnia24.pl.

Rencast FC52 má zhruba třetinovou pevnost oproti Rencast 5146, papírově srovnatelnou s resinem Syria Blu.

Polyuretan je lepidlo, lepí všechno a všechno (výjimky: nelepí silikon a mikroténové sáčky). Polyuretanům hodně vadí vzdušná vlhkost, když se do nich dostane, tak se ztrácí jejich vlastnosti (pevnost a pod.).
Řešení: po odlití do formy buď zavakuovat do sáčku se silikagelem nebo alespoň se silikagelem uzavřít do těsné krabičky. 

Dále se musí přesně namíchat, jinak opět ztrácí jejich vlastnosti. 
Osvědčila se tabulka pro převod hmotnosti na objem -> odměřuje se to stříkačkami (popsanými - nesmí se při opakovaném použítí zaměnit).
Polyuretany se velmi dobře barví, přitom pigmentu stačí kapka. 
Polyuretany jsou dvouskožkové, polyol - základ, izokyanát - tužidlo.  
I u polyuretanu se musí odstranit bubliny, takže opět je dobré ho míchat za nízkého tlaku, třeba i rovnou s pigmentem, po odstranění bublin opět přimíchat tužidlo, promíchat a nalít do formy. 

Velmi výživné informace o odlévání modelů jsou [zde](http://lcamtuf.coredump.cx/gcnc/full/) (na výrobu kopyt používá autor stránek frézku, nikoliv 3D tiskárnu). 

Odlévání epoxidové pryskyřice: pryskyřice, ideálně se skelnými vlákny, se rozmíchá, aby byla pokud možno bez bublinek a 
naleje se do silikonové formy.

