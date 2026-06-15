Test project
===========================

Účel/Zadání/Funkce
-----------------------

Stovky ktere ukazuji cas na sedmisegmentu pomoci tlacitka zapiname a vypaname stopky.

Schéma zapojení
-----------------------


    +3.3V
      |
  [Blue-Board STM8S103F3]
      |
      |-- PC3 --[100Ω]-- SEG_a --|
      |-- PC4 --[100Ω]-- SEG_b --|
      |-- PC5 --[100Ω]-- SEG_c --|  [7-SEG DISPLEJ]
      |-- PC6 --[100Ω]-- SEG_d --|   Common Cathode
      |-- PC7 --[100Ω]-- SEG_e --|       |
      |-- PA1 --[100Ω]-- SEG_f --|      GND
      |-- PA2 --[100Ω]-- SEG_g --|
      |
      |-- PB4 ---[BTN START/STOP]--- GND
      |-- PB7 ---[BTN LAP]---------- GND
      |-- PD3 ---[BTN MODE]--------- GND
      |
      |-- PB5 --- (onboard LED — již zapojeno)
      |
      |-- PD5 (TX) --> RX [USB-UART] --> USB --> PC (pyserial)
      |
     GND


Popis funkce
-----------------------

pomoci tlacitka se zacnou na sedmisegmentu pocitat sekundy
po opetovnem stisknuti tlacitka se cas stopne a na sedmisegmentu budu svitit dany cas 


ToDo
-----------------------

chybi vyresit problem s tim ze kdyz zmacknu tlacitko v nejakem bode tak ty svetelka neustale sviti i pres reset funkce.

Zhodnocení
-----------------------

Na tomto projektu jsem se naučíl to ze nikdy nevim jestli je problem v zapojeni nebo v programu a naucil jsem se vyuzivat ruzne prislusenstvi ktere mi ukazalo jestli to funguje nebo ne.

Svou práci bych ohodnotil vyborne protoze ten cas a usily ktere jsou do toho dal tak mne urcite nejak posunulo ku predu a u tehle prace mne opravdu bavilo kombinot fyzickou vec s programem.

Svou práci bych ohodnotil výborně. Nepracoval jsem sice samostatně pozadal jsme si o pomoc od kamarada ale to bylo jenom ze zacatku zbytek jsem dodelaval sam.

