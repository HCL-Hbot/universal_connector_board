# Healthboth PCB Design
This document outlines the contribution to a PCB design for project Healthbot, which is part of the Health Concept Lab of HAN University of Applied Sciences. It is part of a semester 6 student project including 3 Embedded Systems Engineers and 4 Industrial Design Engineers. The goal for this project was developing a social robot for healthcare, with several functionalities. This PCB is made to be integrated in the robot, but also made quite universal, so it can be used in new robot designs. Because this was a first version, please look at the recommendations below!

## Components
Some components on the PCB that were needed are the I/O Expander, voltage regulators and an UART multiplexer. 
  ### I/O Expander
  Because the GPIO pins of the Pico were completely full and we still needed some, we added an I/O Expander which works with I2C. This expander has 8 I/O channels,   which are also all connected. 
  ### Voltage regulators
  Because of the several needed voltages for the components, two regulators are on the PCB. There is 12 Volt coming in to the PCB via the block terminal. This is     guided to a TSR 1-2433, which converts it to 3.3V. Also a 5V regulator is on the PCB, which was more complicated because it has to have a high output current due   to the ledstrips needing lots of current. 
  ### UART Multiplexer
  The motor driver modules can be controlled via UART communication. Because the Pico doesn't have lots of UART channels, and are already needed for                  communication between the RPI4 and the Pico e.g., an UART Multiplexer was added. Although we don't use it to control our motor drivers, this is still a      possibility because of this multiplexer. This also provided the board with 2 extra UART possibilities for whatever component needs it. 


## !! Issues 
1) The 5V regulator did not work. It is not clear if the regulator wasn't solderded correctly, or the buck converter circuit was not working. Anyways, there was a short between GND and 12V somewhere around the regulator. 
2) Each display needs a identical reset line. The reset line is apparently used to to sync, or atleast stabilize, even though there are seperate chip select lines. 
3) We do not know if the UART multiplexer works, because we don't use it and we needed a pin from the Pico to resolve the previous issue (see Temporary solutions below).

### Temporary solutions
1) Because of safety and time shortage, we just left out the 5V regulator and its circuit that did not work. We now provide 5V via the extra Vcc pins that were added originally as voltage output pins.
2) We used the Pico GPIO pin 21 as extra reset line for the 2nd display. This was originally the 'INH' of the UART Multiplexer. See image below for the connection made. Here can be seen that the trace on the PCB from GPIO21 to the UART Mux was deliberately damaged and replaced with a connection to the reset pin of the 2nd display.

![Afbeelding van WhatsApp op 2024-06-13 om 12 24 14_29f4cc0a](https://github.com/HCL-Hbot/universal_connector_board/assets/114147170/b0830bbf-6515-4914-b825-d5cc7d796db5)


## Recommendations
Below are recommendations for the next group that gets to design a PCB for project Healthbot. These are additions to the previous named issues that need to be adjusted. These recommendations are up to the next developer to decide.

1) Add explicit header pins next to the PICO. This can be useful to easily attach external equipment like a Logic Analyzer or TTL-module, or simply wires to a breadboard if needed. (You can see in the picture of the PCB that we soldered them on for now).
2) Flip the display pin order. This is convenient for testing and prototyping, because the displays can than just be connected (temporarily) to headers on the PCB without the need for several cables. 
3) Add more silk layer documenation. Some of this was already actually done in the design, but some of these labels were skipped during production for some reason. (Also adding small hints for Vcc, gnd and other pins could be useful for connecting the components, when not using JST connectors).
4) Add a working 5V regulator, with enough output current (>6A). Try to not use a regulator that is too small, this was a problem in the first version. 
5) Add status LEDs and just an onboard LED to control.
6) Add a couple of buttons, like to reset the PICO for example. Also, power on/off buttons would be nice (could be tricky for the RPI, but would be useful).


