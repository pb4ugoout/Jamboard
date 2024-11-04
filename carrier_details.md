# Jamboard Carrier
## This document is a work in progress

The carrier in the Jamboard is using an off the shelf nVidia Jetson TX1 module for compute resources. Google has very kindly fused the SoC so it is unfortunately useless for any other purpose unless their RSA keys can be brute forced. However, this module can be removed and replaced very easily once you gain access to the internals of the Jamboard. The currently available version of L4T Ubuntu provided by nVidia Jetpack 4.6.4 works when flashed to a Jetson TX1 or TX2. I was unable to get the touchscreen working, but I didn't put much time into sourcing a driver for it.

The carrier board does provide a recovery mechanism for the installed Jetson module. Using the USB port on the rear IO panel, and the Volume Up button on the Jamboard, you can place the Jetson into force recovery mode to flash it. Power off or unplug the Jamboard. Press and hold the Volume Up button and either apply power or press and release the power button. Continue holding the Volume Up button for several seconds. The display will flash and then remain off. Connect a Linux computer to the micro USB port on the rear IO panel, or directly to the carrier board, and run lsusb in terminal. You should see an entry for NVIDIA and possibly APX. This indicates that your computer is able to see the Jetson module in APX or recovery mode.

The carrier appears to be a custom board made by Google. There are multiple input and outputs for the various devices inside the Jamboard. I will attempt to detail each connector and what I find about them.

![Labels](assets/carrier_labeled.jpeg)

## IO Details

A.) 400-pin Samtec expansion connector for the Jetson TX module (https://github.com/pb4ugoout/Jamboard/blob/main/assets/JetsonTX1_OEM_Product_DesignGuide_09172018.pdf)

B.) 32-pin dual row JST connector between the carrier and the main PSU board (detailed elsewhere)

C.) Momentary switch that functions as a system reset switch

D.) 15-pin FFC/FPC ribbon cable connected to an NFC module behind the shroud for the side IO module. 

E.) 8-pin dual row JST connected to the side IO PCB. Only 7 pins are utilized and the brown wire is not twisted with the rest of the wires.

F.) 8-pin single row JST connected to the rear IO PCB. All 8 pins are populated and the black wire is not twisted with the rest.

G.) Micro-USB port directly connect to the Jetson module. This is connected to the rear IO module with an uncommon, but, off the shelf usb micro-b to micro-b cable.

H.) Gigabit ethernet port connected to the rear IO PCB via a STP Cat5 jumper.

I.) USB 3.0 Type-A port connected to the side IO PCB.

J.) DisplayPort input connect to the side IO PCB. (Used for a displayport input over usb type-c).

K.) HDMI input connected to the rear IO PCB.

L.) HDMI input connected to the side IO PCB.

M.) 51-pin eDP output directly connected to the LCD panel.

N.) 12-pin single row JST connected to the Rapt Hazeldos/Hazelred touch control pcb. It splits off at the controller into a 6-pin single row JST for unknown purposes and a 5-pin single row JST for what looks like USB data with two ground wires.

O.) 6-pin JST supplying power from the carrier to the Rapt controller PCB.

P.) 2-pin JST for the Power/Status LED at the bottom front of the Jamboard.

Q.) Momentary switch. Connects to item "Z". Traces through a pull down down resistor and then into a via. Comes out of the back of the board near Z and then heads into another via under the IC. Also splits off at the pull down resistor to the collector pads for a transistor that wasn't installed. Unconfirmed but this is likely a reset button for the Milestone processor (Z). It could also be an input select button. Need to test further on a working unit.

R.) 5-pin single row JST connected to the power button control PCB.

S.) 4-pin single row JST connected to the left and right speakers mounted at the bottom of the Jamboard.

T.) [ST Micro STA339BW](assets/STA339BW.jpeg) 2.1ch Digital Audio System on a chip. (https://www.st.com/resource/en/datasheet/sta339bw.pdf)

U.) 4-pin single row JST. Possibly goes to IC T

V.) 4-pin single row JST. Possibly goes to IC Z.

W.) 6-pin single row JST. UART0. On the stock TX1 module, UART is enabled in the bootloader but disable in ODMDATA so it doesn't output anything by default.

X.) HDMI Video Out - Confirmed that this is the video output from the Jetson. It connects to the HDMI port here and then passes through it down to the Milestone processor (label Z). Audio is pulled out after this port and sent separately to IC T, the ST Micro audio SoC.

Y.) 2-pin jumper of unknown purpose.

Z.) [Milestone Semiconductor MST9U23T1](assets/MST9U23T1.jpeg). Unable to find a datasheet or any details about this chip but we know it handles sending video to the LCD from inputs K and L.

## Individual Pinouts

### B.) See here: [Power Breakout](power_header.md) 

### E.) Power and GPIO to Side IO Module

    1.) VCC - 24.5v
  
    2.) GND but NC - No wire in the corresponding cable
  
    3.) VCC - 24.5V
  
    4.) GND
  
    5.) Digital Audio from HDMI Input - Parallel to Port 1 on Header F
  
    6.) 
  
    7.) Goes to U4404 (Level Shifter) then to B6 on the SoC - I2C_PM_DAT
  
    8.) Goes to U4404 (Level Shifter) then to A6 on the SoC - I2C_PM_CLK
  
### F.) Connected to Rear IO Module

    1.) Digital Audio from HDMI Input - Parallel to Port 5 on Header E
  
    2.) TOSLINK - Vin (Data Input)
  
    3.) TOSLINK - Vcc (Supply Voltage)
  
    4.) GND
  
    5.) Ethernet port Activity LED
  
    6.) Ethernet Port Link LED 
  
    7.) Ethernet port Link LED
  
    8.) Ethernet Port Activity LED
  
### N.) Rapt Touch Controller Communication

    1.) USB Power - +5V
  
    2.) USB Data -
  
    3.) USB Data +
  
    4.) GND
  
    5.) GND
  
    6.)
  
    7.)
  
    8.)
  
    9.)
  
    10.)
  
    11.)
  
    12.)
  
### O.)

    1.) Vcc - 11.8V
  
    2.) Vcc - 11.8V
  
    3.) Vcc - 11.8V
  
    4.) GND
  
    5.) GND
  
    6.) GND
  
### R.)Side Panel Buttons.

    1.) Power Button
  
    2.) Vol - Button
  
    3.) Vol + Button
  
    4.) Input Button
  
    5.) GND
  
### U.) Unknown connection to IC T. Assuming it's a comm protocol. Need a logic analyzer.

    1.) Vcc - 3.3V
  
    2.) 
  
    3.)
  
    4.) GND
  
### V.) Unknown connection to IC Z. Assuming it's comm protocol. Need a logic analyzer.
    1.) Vcc - 5.2V
  
    2.)
  
    3.)
  
    4.) GND
  
### W.) UART0 - DEBUG - Runs over to two NXP NTS0102GD bi-directional level shifters (U4502 and U4503).

    1.) VCC(B) on both shifters (3.3v)
    
    2.) B1 on U4503 - A1 to H12 (UART0-Tx)
  
    3.) B1 on U4502 - A1 to G12 (UART0-Rx)
  
    4.) GND
  
    5.) B2 on U4503 - A2 to G11 (UART0-RTS)
  
    6.) B2 on U4502 - A2 to H11 (UART0-CTS)
  
### Y.)

    1.)
  
    2.)
