A few thoughts: 

Events at work would be way easier with wireless comms. Unfortunately the boss will never go for a commercial system (reasonably so)
It would also be a neat project to show off when applying for internships. And a distraction from studying for exams (not necessarily a good thing tbh)


Some goals / design requirements:

            Cost            |       $10ish per unit, at scale.
            -----------------------------------------------------------------------------------------------------------------------
            Range           |       500 - 800m LOS. Depending on the need for certification obvs, this is OSHW & I'm a student.
                            |       The system should function in a crowded room & I'd expect coverage around the ground floor of
                            |       where I live / work.
            -----------------------------------------------------------------------------------------------------------------------
            Protocol        |       TBD. Some sort of mesh system would be a good idea. A chain topology is not suitable, as it makes
                            |       the whole thing very fragile. [2]
            -----------------------------------------------------------------------------------------------------------------------
            Processor       |       TBD
            
            
And some things that have already been decided:

            Power source    |       LiPoly battery or similar. Minimum 8hr battery life.
            -----------------------------------------------------------------------------------------------------------------------
            Connectivity    |       USB C charging built in. Exposed serial port or JTAG for programming. Perhaps both? Should use
                            |       standard 10pin header for use with a BlackMagic probe. RJ45 could be an option for serial. [1]
                            |       
                            |       Headset, Microphone & PTT on 3.5mm jacks. Mini XLR could be explored?
            -----------------------------------------------------------------------------------------------------------------------
            UI / UX         |       Ease of use is a top priority. The whole thing should be set and forget. 
                            |       An OLED display and a rotary encoder / pushbutton can do a lot.
                            |       Must haves: 
                            |       Battery indicator w dedicated pushbutton
                            |       A way to display the band & connection strength if that's important to the protocol chosen
                            |       "PTT active" display. Control on the unit as well as through the external connector
            

[1]: Future things to explore: Wired Networking? Could be of interest for wider situations or RF noisy enviroments. Possiblity of a base 
station for future expansion. I have an RPi Pico with a WizNet SPI-10/100 ethernet chip on it that could be handy

[2] I'm taliking out my ass here. No idea what I'm going to use but it'll come down to the chipset chosen / cost / range / licensing / etc etc etc



Possible architecture:

                        -----------                                                                              -----------                  --------
                        | TX & RX | ------ ? ------|            --------------------            |----- SPI ----- | W51000s | --- ETHERNET --- | RJ45 |
                        -----------                |            |                  |            |                -----------                  --------
                                                   |----------- |                  | -----------|
                                                                |      RP2040      |
                                                   |----------- |                  | -----------|
  --------------        -----------                |            |                  |            |                ----------------
  | MICROPHONE | ------ |  CODEC  | ------ ? ------|            --------------------            |--------------- | PUSH TO TALK |
  --------------        -----------                                |     |       |                               ----------------
                             |                                     |     |       |
  ----------------           |                                     |     |       |
  | AUDIO OUTPUT | ----------|      -----------------          --------  |   ----------                 --------
  ----------------                  | 10 PIN HEADER | -------- | JTAG |  |   | SERIAL | --- RS-485? --- | RJ45 |
                                    -----------------          --------  |   ----------                 --------
                                                                         |  
      |----------------------------- USB 1.1 ----------------------------| 
      |
  ---------       -------------------       -----------       --------------
  | USB-C | ----- | BATTERY CHARGER | ----- | BATTERY | ----- | REGULATORS |
  ---------       -------------------       -----------       --------------

Part table:

            RADIO                   |                                   |    BATTERY CHARGER    |
            -----------------------------------------------------------------------------------------------------------------------
            PROCESSOR               |    RP2040 [3]                     |    BATTERY            |
            -----------------------------------------------------------------------------------------------------------------------
            MICROPHONE              |                                   |    REGULATORS         |
            -----------------------------------------------------------------------------------------------------------------------
            AUDIO CODEC             |                                   |                       |
            -----------------------------------------------------------------------------------------------------------------------
            ETHERNET INTERFACE      |    WIZNet W5100S [4]              |                       |
            -----------------------------------------------------------------------------------------------------------------------
            
[3] I'm choosing this processor because I have a devboard for it sitting on my desk :)
[4] The RP2040 dev board is a W5100S-EVB-Pico by WIZNet which has this IC on it. It's an SPI connected 10/100 Ethernet controller / MAC

