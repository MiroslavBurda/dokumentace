

# PID regulátory

postup: základ je vyladit regulátor P, až začne oscilovat. P samotné nepojede rovně, vždycky bude oscilovat. Potom P ubrat trochu a malou trochu přidávat regulátor I, tak že se bude přibližovat (pomalu) k ideální dráze. D zajistí rychlé přibližování k ideální dráze, ale je citlivé na šumy - bývají s tím problémy 
závěr: pro první pokusy používat pouze PI 


pro regulaci rychlosti je potřeba časová smyčka max. 10 ms, čím míň, tím líp 
pokud chci regulovat polohu, toto omezení neplatí a můžu použít 
jednoduchý enkodér optický - odraz na paprscích černá-bílá přilepeném na kolech

https://www.pololu.com/docs/0J21/7.c - PID regulátory pro 3pi

https://valter.byl.cz/plynula-regulace-pid


http://www.pohonnatechnika.cz/pid-regulator

http://www.embedded.com/design/prototyping-and-development/4211211/PID-without-a-PhD

https://www.root.cz/clanky/stavime-kvadrokopteru-bezpecnost-pid-regulator/
 

class PID {
  float suma_err = 0;
  float predchozi_err = 0;
  public float P_const = 0;
  public float I_const = 0;
  public float D_const = 0;
  float err = 0;
  float ret = 0;

public float get_PID(float zadany_naklon, float aktualni_naklon, float diff_t) {
  err           =  zadany_naklon - aktualni_naklon;
  suma_err     +=  err;
  ret           =  P_const*err +
                   I_const*suma_err*diff_t +
                   D_const*(err - predchozi_err) / diff_t;
  predchozi_err =  err;
  return ret;
  }
}

---------------------------------------------------------------------------------

Moje vysvetleni: PID regulator funguje tak. Mame nejaky chybovy signal napr. odchylku aktualni rychlosti otaceni od pozadnovane rychlosti otaceni. Pak mame nejaky signal co ridime napr. napeti na motoru.

PID regulator funguje tak, ze vememe ten chybovy signal, jeho derivaci a integral. A tyto 3 signaly namichane nejakymi konstantami. Napr. 3x signal + 4x integral -2x derivaci. A to je ten ridici signal.

Ty cisla se nastavi tak, aby se to nerozkmitavalo, reagovalo rychle, neprekmitavalo proste aby to chodilo tak ze se nam to bude libit. Bud se to vyzkousi, nebo jsou na to na webu nejake systematicke algoritmy jak to vyladit.

Ona totiž každá ta složka (proporcionální, intergrační a derivační) mají i nějaké vlastnosti zajímavé pro regulaci, ne že se to zesílí, popř. zintegruje nebo zderivuje, ale např. integrační složka neustále zesiluje a tím přispívá k lepšímu dosažení požadované hodnoty ( narozdíl od proporcionální, která jen zesílí o KONSTANTA krát ), ale také zesiluje šumy, derivační složka zase pomáhá mimo jiné odstraňovat šumy, atd. No a to už tam pisateli Clockovi chybí, ale protože v Teorii automatického řízení je tento vzorec něco jako pro rybáře vědět co je udice, tak se běžně používá. Nakonec se tím opravdu jen proženou čísla.

-------------
Konstaty budou v dalším článku, já používám (Kp, Ki, Kd): x* 0.2, x* Kp*2, x* Kp/3
kde x nastavím na požadovanou "ostrost" reakcí.

https://en.wikipedia.org/wiki/Ziegler%E2%80%93Nichols_method

To nastavování podle Zieglera-Nicholse je určitě v pořádku ve verzi se zabezpečenou kvadrikoptérou. Vy jste si tím prošel, takže předpokládám, že jste splašeného koně už viděl :-) .
Pokud se týká vašeho kódu, já bych osobně rozhodně ošetřil omezení rozsahu regulace (0-100% buzení motoru) a zcela určitě i nějakou ochranu proti přeintegrování. Zlepší a zrychlí to regulaci v některých situacích a eliminuje řadu možných selhání regulace, zejména při případné akrobacii. Předpokládám ale, že jste něco takového udělal a kód je jen principiální ukázkou řešení. Je samozřejmé, že to bude zase o kus složitější, ale zde rozhodně pro dobrý účel.

Doplním, pokud by to někdo hledal, že se jedná o anti-windup techniku.

--------------
float supp = 0;
supp =                    get_pid( // Funkce PID regulátoru.
                          &pid_ns, // Reference na strukturu s daty PID regulátoru pro North-South osu.
       command.x + command.trim_x, // Požadovaný náklon (z joysticku) + korekční hodnota (trimr).
                           ypr[1], // Náklon kvadrokoptéry v NS-ose získaný z IMU.
                        1/LOOP_HZ  // časová perioda, s jakou probíhá update motorů.
                                );
     power_n = command.pwr - supp; // command.pwr je hodnota výkonu definovaná na joysticku, kde
     power_s = command.pwr + supp; // na jedné straně k výkonu připočtu supp, na druhé ho zase odečtu.

supp = get_pid(&pid_ew, command.y + command.trim_y, ypr[2], 1/LOOP_HZ); //to samé pro EW osu
    power_e = command.pwr + supp;
    power_w = command.pwr - supp;

supp = get_pid(&pid_z, command.z, gyroZ, 1/LOOP_HZ); // PID pro korekci rotace. Vstupem je úhlové zrychlení okolo osy Z.
    power_n += supp; // Korekce pro rotaci probíhá na všech
    power_s += supp; // motorech. Cílem je dosáhnout rozdílu
    power_e -= supp; // výkonu na obou osách, čímž je vyvíjena
    power_w -= supp; // rotace. 
----------------------
https://forum.root.cz/index.php?topic=15603.0

anti-windup:
http://www.polyx.com/_ari/slajdy/Pr-ARI-11-Controllers.pdf


http://forum.robodoupe.cz/viewtopic.php?t=260 - diskuze k věci 


http://uprt.vscht.cz/kminekm/mrt/F3/F3k34-sprg.htm
Volba typu regulátoru
Tuto volbu provádíme především podle požadavků technologického procesu. 
Již bylo řečeno, že z prakticky užívaných typů máme k dispozici regulátory s vlastnostmi P, PI, PD a PID. 
Pro konkrétní případ jednoduchého regulačního obvodu z nich vybíráme zhruba podle následujících zásad:

P regulátor volíme pro méně náročné aplikace, kde nám nevadí trvalá regulační odchylka a preferujeme jednoduché a levné řešení,
PI regulátor patří k nejběžněji používaným a volíme jej pro středně náročné aplikace, u kterých vyžadujeme, aby pracovaly bez trvalé regulační odchylky,
PD regulátor se příliš často nepoužívá; co do trvalé regulační odchylky se chová stejně jako regulátor P, složka D však zesiluje jeho reakci na rychlost změny regulační odchylky, 
takže se uplatní při nepříliš náročné regulaci rychlých dějů,
PID regulátor je vhodný pro náročné aplikace, pracuje bez trvalé regulační odchylky a je schopen dobře regulovat i rychlé děje.

https://projects.adamh.cz/doku.php?id=linefollower - možná se dá použít 
https://robotika.cz/guide/control/cs
https://robotika.cz/guide/odometry/cs
https://robotika.cz/guide/localization/cs

https://www.vutbr.cz/www_base/zav_prace_soubor_verejne.php?file_id=147998 - možná má smysl to pročíst 







