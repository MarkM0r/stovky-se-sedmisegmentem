# stovky-se-sedmisegmentem
Pocitani sekund na sedmisegmentu s start stop tlacitkem

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
