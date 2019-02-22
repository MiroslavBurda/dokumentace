
- dva motory krokové
- nebo dva motory stejnosměrné
krokové jsou těžké, ale snadno se řídí 
( driver A4988 ? https://laskarduino.cz/motory-radice/230296-ramps14-a4988-driver-pro-krokove-motory.html?gclid=Cj0KCQjw3KzdBRDWARIsAIJ8TMTr7jBotNDdzGEkYwIw2qcue6EFNyIPFudkRrNCGd1tOALisVZyrQ0aAlVqEALw_wcB  ) 
rameno robotické -> 4-5 serv 
baterie na cca 24V
- motor na vysouvání kostek žlutý 

## Řízení 





## Napájení 

-extra baterie pro řídící elektroniku 
-6 serv 
-drivery pro 2 krokové motory 
-stejnosměrný motorek žlutý pro výsuv 
?? servo pro pohyb kamery ?? 

## Napájení serv 

-i malé/mikro servo si při rozjezdu vezme 1-3 A, větší klidně i 20 A, pokud mu je nedodám, může při rozjezdu dojít 
k poklesu napájecího napětí a tím k posunu nuly na servu - servo je potom nepřesné 
-napájení: servo potřebuje max. 6 V, 2xbaterie 18650 Li-On dává 8,4V nabitá, 6,6V vybitá, proto potřebuju stabilizátor 
ideálně s malým úbytkem napětí (něco jako 7805) Stabilizátor přebytky výkonu mění v teplo, proto se musí dobře chladit 
pokud bude mít servo o něco méně než 5V, možná fungovat bude, ale není to jisté 
-před stabilizátor je potřeba zařadit ještě do série diodu (alespoň 3A a počítat s úbytkem napětí na ní), aby servo, na kterém je zrovna 
úbytek napětí, nebralo napětí ostatním servům 
-na přívodních vodičích vzniká při rozjezdu velká indukčnost, proto musí být co nejkratší, ideálně jednotky cm, především u velkých serv 
-zjistit odběry proudů u serv při rozjezdu 
-přívodní vodiče napětí pro servo musí být silnější, např. 1,5 mm pro velké servo, pouze datový vodič bude klasický 


## Sehnat: 
- 6 serv (2x 20 kg , 1x 10 kg, 3 x 5 kg) 
- 6 mikroserv 


## 
- 
- konstrukce z hliníkových profilů + překližka a pod 

- 
3D Builder, autodesk thinker  
jxservo.com 



