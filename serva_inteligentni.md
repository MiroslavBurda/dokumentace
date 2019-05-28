
 rbc().initSmartServoBus(2, UART_NUM_2, GPIO_NUM_14); 
    // pocet serv (MUSI byt spravne), cislo  hardwarove seriove linky, pin, na kterém jsou serva připojena (všechna na jednom) IO14 by default

    // ID je tady od 0, ale HW je od 1! - toto Hadrwarove ID se musi nastavit specialni destickou - pouze jednou 
    rbc().servoBus().limit(0, 0_deg, 240_deg); // ID, minimalni, maximalni hodnota - toto se nastavuje pouze jednou
    rbc().servoBus().set(0, 120_deg, 150); 
    // ID, cilova poloha, rychlost, [zrychleni a zpomaleni na zacatku a konci pohybu, 1.f toto vypina]

typy serv:
LX-15D
LX-16A

