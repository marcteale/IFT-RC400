# Hearth and Home IFT-RC400 Fireplace Remote
These are my notes regarding my attempts at reverse engineering the Hearth and Home IFT-RC400 fireplace remote control.  I'm posting them on Github so they may be of use to someone with more experience.  If anyone disovers this doc and finds it useful, I'd love to hear your feedback and results.

# Hardware
The IFT-RC400 uses a 100-pin Microchip Technology PIC24FJ256DA210 at its core.  I did not investigate underneath what I assume is RF shielding on the back of the board.

## Buttons
There are two buttons accessible from the outside of the unit that can be pressed with a bent paperclip.

### Left (unmarked)
Resets the unit.

### Right ("P")
Described in the manual as "the programming hole."  Starts the "Brand/Dealer Programming Procedure" during installation.

## Switch
The switch inside the battery compartment is a child lock that enables and disables the touchscreen.

## Chip Markings
1. U2 and U11: HC374 / 16K<sup>G4</sup> / ACG5
2. U3: 23F032B / SM / 2113V6W
3. U4: ISSI IS62WV25616EBLL-45TLI / EE1634X1 2039
4. U7: 17 TI BFM

## ICSP Header
There is a six pin header accessible inside the battery compartment, presumably for testing and programming in the field.  I was able to beep out the corresponding PIC24FJ256DA210 pins using a multimeter.

I've assigned the pin numbers starting at what I presume to be pin 1, as marked by the triangle on the case.

| ICSP header pinout  |                   |        |
| ------------------- | ----------------- | ------ |
| 2. V<sub>DD</sub>   | 4. RP6            | 6. NC  |
| 1. ?                | 3. V<sub>SS</sub> | 5. RP7 |
| △ (triangle on case)|                  |        |

### Notes on pins

1. **?:** I couldn't identify this pin's other end.  Nothing beeped out on the PIC, and the trace runs underneath the LCD screen.  I wasn't willing to remove the screen's metal backplate to follow it any further.  A replacement remote is $329!
2. **V<sub>DD</sub>**
3. **V<sub>SS</sub>**
4. **RP6/ICSPCLK/PGEC:** PIC24FJ256DA210 pin 26
5. **RP7/ICSPDAT/PGED:** PIC24FJ256DA210 pin 27
6. **NC:** Unless the board has more than the visible top and bottom layers, this pin goes nowhere.

# Attempted firmware dump
I attempted to read the PIC's memory using a PICkit 3 and MPLAB X IDE v6.20.  Unfortunately, I only received FF's and 00's in response.

I did make one useful discovery while looking at the configuration bits menu: JTAG is (theoretically) accessible.  There are no test points or unpopulated headers on the board, which means soldering directly to the PIC.  I don't have the equipment or skill to solder anything that small, so that's where my investigation ended.

# Photos
## Front
<img alt="Front of board" src="https://github.com/user-attachments/assets/2bc546a7-36b4-4b7b-91c9-4dac8d336c95" width="150">

## Rear
<img alt="Back of board 1" src="https://github.com/user-attachments/assets/d52a8e0b-1151-41cc-80b6-3523e1b3ced2" width="150">
<img alt="Back of board 2" src="https://github.com/user-attachments/assets/d941c182-32a3-481e-bb20-4f8694f7f786" width="150">
<img alt="Back of board 3" src="https://github.com/user-attachments/assets/ff9482a4-17ad-429c-975f-91f83fb0b68a" width="150">

## Chips
<img alt="U1" src="https://github.com/user-attachments/assets/afaf1004-f6e3-41b7-80cb-809eb0961274" width="150">
<img alt="U4 and U2" src="https://github.com/user-attachments/assets/ad326cad-380f-4e18-a1f2-55de05260c20" width="150">
<img alt="U11" src="https://github.com/user-attachments/assets/3ec6a00e-89ee-4000-add3-16c6c18fbdcd" width="150">

# Other
* FCC ID: ULE2326-110
* IC ID: 6732A-2326110
* RF frequency range: 912.4-918 MHz
* Part number: 2326-110

# Links
* [PIC Microcontroller firmware dumping guide](https://www.rapid7.com/blog/post/2019/04/30/extracting-firmware-from-microcontrollers-onboard-flash-memory-part-3-microchip-pic-microcontrollers/)
* [Installation manual](https://downloads.hearthnhome.com/installManuals/Addendums/2326_982_IFT-RC400IFT-ACM_Install_Manual.pdf)
* [FCC filing](https://fccid.io/ULE2326-110)
