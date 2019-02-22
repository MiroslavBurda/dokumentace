


## joystick připojit přes lorris 

otevři lorris
připoj se k zařízení (nebo ne) 
vyber analyzér > script 
pravým na horní lištu > zobrazit zdrojový kód scriptu 
z ikonek vyberu žárovičku (příklady) > vyberu joystick 
po vybrání by se mělo objevit menu, ze kterého vyberu připojený joystick 
důležitá je metoda  axesChanged(axes) a buttonChanged(id, state):
pro posílání dat  do čipu je v příkladu Controls metoda lorris.sendData

