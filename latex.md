
https://en.wikibooks.org/wiki/LaTeX/Installing_Extra_Packages

je potřeba soubory .ins a .dtx nahrát do adresáře, kde je pdflatex, pak ho spustit z příkazové řádky, 
a pokračovat podle pokynů 

- nejlepší je nainstalovat vše, celý obrovský balík, jinak vám pořád něco chybí a doinstalovávání není pro začátečníky 
- na rozbalování archívů ve windows se osvědčil 7-zip https://www.7-zip.org

Menu: Volby >Nastavit TeXstudio >sekce příkazy, položka Texindy:  
C:\texlive\2018\bin\win32\texindy.exe -I latex --language czech  --codepage utf8  %.idx
potom spustit pomocí: 
Nástroje > příkazy > TexIndy 

https://www.fi.muni.cz/lemma/PB029/practices/rejstrik/#tvorba-rejstriku-preklad

https://www.root.cz/clanky/jak-na-latex-rejstrik-balikem-index/

hyperindex	true	Makes the page numbers of index entries into hyperlinks
in: https://www.overleaf.com/learn/latex/Hyperlinks

-------------------------
instalace nových balíčků ve win: 
spustit: C:\texlive\2018\bin\win32\tlanunch.exe
v něm spustit tex live command prompt 
na řádek napsat: tlmgr install [název balíčku bez přípony]
a počkat chvíli na výsledek 
podobně pro aktualizaci balíčků napsat: 
tlmgr update --list
