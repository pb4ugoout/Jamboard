# Jamboard Carrier
## This document is a work in progress

The carrier in the Jamboard is using an off the shelf nVidia Jetson TX1 module for compute resources. Google has very kindly fused the SoC so it is unfortunately useless for any other purpose unless their RSA keys can be brute forced. However, this module can be removed and replaced very easily once you gain access to the internals of the Jamboard. The currently available version of L4T Ubuntu provided by nVidia Jetpack 4.6.4 works when flashed to a Jetson TX1 or TX2. I was unable to get the touchscreen working, but I didn't put much time into sourcing a driver for it.

The carrier board does provide a recovery mechanism for the installed Jetson module. Using the USB port on the rear IO panel, and the Volume Up button on the Jamboard, you can place the Jetson into force recovery mode to flash it. Power off or unplug the Jamboard. Press and hold the Volume Up button and either apply power or press and release the power button. Continue holding the Volume Up button for several seconds. The display will flash and then remain off. Connect a Linux computer to the micro USB port on the rear IO panel, or directly to the carrier board, and run lsusb in terminal. You should see an entry for NVIDIA and possibly APX. This indicates that your computer is able to see the Jetson module in APX or recovery mode.

The carrier appears to be a custom board made by Google. There are multiple input and outputs for the various devices inside the Jamboard. I will attempt to detail each connector and what I find about them.

![Labels](assets/carrier_labeled.jpeg)

## IO Details

A.) 400-pin Samtec expansion connector for the Jetson TX module

B.) 32-pin dual row JST connector between the carrier and the main PSU board (detailed elsewhere)

C.) Momentary switch that functions as a system reset switch

D.) 15-pin ribbon cable headed under the plastic side IO shroud. (Need to investigate further).

E.) 8-pin dual row JST connected to the side IO PCB. Only 7 pins are utilized and the brown wire is not twisted with the rest of the wires.

F.) 8-pin single row JST connected to the rear IO PCB. All 8 pins are populated and the black wire is not twiseted with the rest.

G.) Micro-USB port directly connect to the Jetson module. This is connected to the rear IO module with an uncommon, but, off the shelf usb micro-b to micro-b cable.

H.) Gigabit ethernet port connected to the rear IO PCB via a STP Cat5 jumper.

I.) USB 3.0 Type-A port connected to the side IO PCB.

J.) DisplayPort input connect to the side IO PCB. (Used for a displayport input over usb type-c).

K.) HDMI input connected to the rear IO PCB.

L.) HDMI input connected to the side IO PCB.

M.) 51-pin eDP output directly connected to the LCD panel.

N.) 12-pin single row JST connected to the Raspt touch control pcb. It splits off at the controller into a 6-pin single row JST for unknown purposes and a 5-pin single row JST for what looks like USB data.

O.) 6-pin JST supplying power from the carrier to the Raspt controller PCB.

P.) 2-pin JST for the Power/Status LED at the bottom front of the Jamboard.

Q.) Momentary switch. Connects to item "Z". Traces through a pull down down resistor and then into a via. Comes out of the back of the board near Z and then heads into another via under the IC.

R.) 5-pin single row JST connected to the power button control PCB.

S.) 4-pin single row JST connected to the left and right speakers mounted at the bottom of the Jamboard.

T.) Unknown IC under a heat sink.

U.) 4-pin single row JST of unknown purpose.

V.) 4-pin single row JST of unknown purpose.

W.) 6-pin single row JST. Runs over to a couple of level shifters. Still unsure what function this serves. I can't figure out why you would add a header that only serves to translate for an external device with no connection back to anything on the board.

X.) HDMI connector of unknown purpse.

Y.) 2-pin jumper of unknown purpose.

Z.) Unknown IC under large heat sink. This is a video processor of some kind. It handles sending video to the LCD from inputs K and L.

## Individual Pinouts

### B.) See here: [Power Breakout](power_header.md) 

### E.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
    5.)
  
    6.)
  
    7.)
  
    8.)
  
### F.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
    5.)
  
    6.)
  
    7.)
  
    8.)
  
### N.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
    5.)
  
    6.)
  
    7.)
  
    8.)
  
    9.)
  
    10.)
  
    11.)
  
    12.)
  
### O.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
    5.)
  
    6.)
  
### R.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
    5.)
  
### U.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
### V.)

    1.)
  
    2.)
  
    3.)
  
    4.)
  
### W.) Runs over to two NXP NTS0102GD bi-directional level shifters (U4502 and U4503).

    1.) Pin 7 - VCC(B) on both shifters
    
    2.) Pin 8 - B1 on U4503
  
    3.) Pin 8 - B1 on U4502
  
    4.) GND
  
    5.) Pin 1 - B2 on U4503
  
    6.) Pin 1 - B2 on U4502
  
### Y.)

    1.)
  
    2.)
