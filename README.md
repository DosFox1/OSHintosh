# OSHintosh
OSHintosh - an open source hardware 68000 Macintosh. None of the hardware is from Apple!


<img width="960" height="1280" alt="OSHintosh Complete" src="https://github.com/user-attachments/assets/1d2f49e7-35ee-4e27-8aa9-2caed53af253" />


<img width="1280" height="960" alt="OSHintosh operating" src="https://github.com/user-attachments/assets/60c03e0a-cb30-4e7e-a6a7-27f8e6af7850" />


# DISCLAIMER
The initial VDEV1 board required a large amount of bodges to become operational, these are as follows:

1) Added missing track between pin 1 of the 74ls161s, and pin 8 of the inverter
2) 1x missing 47R resistor TSEN2 from pin 11 of the TSG and ASG to ground
3) 4x missing 2k2 pull up resistors on address lines A9, A10, A11, A12
4) 2x missing 3k3 pull up resistors on !AS and !VIAIRQ
5) pin 4 of IC43 should be connected to -12v, not GND
6) pin 12 of IC18 should be connected to track VA11, not track VA12
7) pin 8 of IC343 should be connected to +12v, not 5V
8) errors on silkscreen for R68, R69, R70 and R71 (Schematic and BOM correct, silkscreen incorrect)
9) errors on silkscreen for R40 and R43 (Schematic and BOM correct, silkscreen incorrect)


VDEV2 has been completed and uploaded. This version should fix all problems with the VDEV1 board, but has not been tested. 

<img width="960" height="1280" alt="OSHintosh VDEV1 Bodges" src="https://github.com/user-attachments/assets/a0c2d828-421a-4911-9253-bc2f433e80fa" />

# Renders of the Board


<img width="6004" height="4803" alt="OSHintosh Top High Res" src="https://github.com/user-attachments/assets/ea4aeb39-d378-431b-bf56-ed1de330ecb5" />
<img width="6004" height="4803" alt="OSHintosh Bottom High Res" src="https://github.com/user-attachments/assets/847d08ab-2f7c-43c2-b040-5f411c60d348" />

# So, what is it?
In a nutshell, the OSHintosh is a Macintosh 512Ke Logic Board - but with the exception that power it provided by an ATX power supply, there is an integrated Pico Scan Converter, as well as a SCHWIM to replace the IWM.
The system can boot from an integrated original ROMinator, with the option to access further files from a LocalTalk server. 

There is no SCSI, No Floppy Drive or any other Mass Storage - it is effectively a ROM Workstation!
It could be considered to be the spiritual sucessor to the Macintosh XO, which eventually became the Macintosh Classic.

The board has also been designed in only two layers, which reduces the cost of the board even further.
JLCPCB suggest that five boards will cost £30 - so only £6 per board. Your mileage may vary. 
41256 RAM was chosen, as it at least avoids the issue of trying to source 30 pin RAM sockets - which are weirdly expensive now. 
It also makes the entire thing a DIP THT Macintosh!

This makes it the first Macintosh designed on a two layer PCB since the "SCC Word Wide" Prototype from 1982!
The board is more or less ATX shaped, but I didn't place the mounting holes to be ATX compliant. Sorry!

This is intended to fit somewhere between the excellent ITX Plus by Max1zzz and the equally excellent PicoMac by evansm7.  
I say this as the RAM is limited to 512K and there is no mass storage, but it does have "real" Localtalk ports to communicate with an AppleTalk network - as well as sound!

# Programmer Requirements:
A Ti866 or similar programmer is recommended. 
In total there are nine programmable ICs that need to be programmable. These are:
1) 6 x 16V8 GALs
2) 2 x 39F040 EEPROMs
3) 1 x ATTINY85 (RTC)

# Hard (ish) to Source Components:
Please Note that the following ICs may be difficult to source:
1) 15.6672mhz oscillator (ebay may have some, if not, custom oscillators can be ordered)
2) Zilog Z8530 SCC
3) 6522 VIA (Demik has noted that the W65C22N6TPG should be a suitable VIA, that's still being made!)
4) 41256 DRAM (x16!)

Please also note this assumes that you have access to either original Macintosh Peripherals, or will add your own adapters!
The OSHintosh uses the original quadrature 9 pin mouse, as well as the original RJ Keyboard. 

# GAL JED Files:
The GALs were sourced from the PLD archive:
https://wiki.pldarchive.co.uk/index.php?title=Macintosh_128k/512k/Plus
For completeness, they have been included here under "OSHintosh GALs".

Programming can be completed under minipro with:
```
minipro -p GAL16V8* -w (GAL)16V8.jed
```

Equations will be uploaded... eventually.<img width="1280" height="960" alt="OSHintosh operating" src="https://github.com/user-attachments/assets/7f629b0b-7202-4aff-b3ec-e2b5da57b712" />


# Programming the Attiny85 with Minipro 

The ATTINY85 RTC firmware as developed by pgreenland (https://github.com/pgreenland/attinyrtc) can be programmed with a Ti866 under Minipro:
The following erases the ATTINY85:
```
minipro -p ATTINY85 -E
```
Then, the fuses as defined by the file attiny_RTC.fuses is flashed
```
minipro -p ATTINY85 -c config -w attiny_RTC.fuses -e
```
Finally, the firmware is flashed to the ATTINY:
```
minipro -p ATTINY85 -e -c code -f ihex -w ATtinyRTC.hex
```

# Producing new Rominator Disks with a Minipro 

Normally, the ROMinator is designed to be programmed using the BMOW flasher tool onboard the macintosh.
However, this assumes we already have a method of adding mass storage to the Macintosh, which the OSHintosh lacks. 
(https://www.bigmessowires.com/mac-rom-inator/)

As it transpires, the ROMinator just literally tacks an 885K .DSK image onto the end of the modified ROM.
Therefore, the best way to produce a modded ROM is to use mini vmac (or any other suitable emulator!) to modify a copy of the dsk image 
"rominator-disk.dsk" with the desired files and programs
. I would suggest just deleting any unwanted files rather than erasing the disk, as I feel it might break something if it is erased. 
Once content with the resulting disk, the "raw" rominator binary (basically the rominator rom without the disk image) and the new disk should be concatenated together. 

On a Unix machine, this can be done with cat:
```
cat ROMINATOR_RAW.bin oshintosh_disk.dsk > OSHINTOSH_COMBINED.bin
```

In this example, I am using my oshintosh_disk.dsk to create OSHINTOSH_COMBINED.bin.

The next step is to split the comnbined .bin file into two high and low images for the OSHintosh. 
To generate the HIGH ROM:
```
srec_cat -o HIGH.bin -binary OSHINTOSH_COMBINED.bin -binary -split 2 0
```

To generate the LOW ROM:
```
srec_cat -o LOW.bin -binary OSHINTOSH_COMBINED.bin -binary -split 2 1
```

Finally, the two resulting images can then be flashed to the EEPROMs:
```
minipro -p SST39SF040 -w OSHINTOSH_LOW.bin 
```
```
minipro -p SST39SF040 -w OSHINTOSH_HIGH.bin
```

If successful, the OSHintosh should "ping" rather than the usual chime, and when "R" is pressed, the OSHintosh should boot from the ROM.

# Note regarding AppleShare, Chooser and the OSHintosh Serial Ports

If you are wishing to use appleshare to run programs on the OSHintosh, it is best that you use the OSHintosh Disk.dsk files as a starting point. 
This is due to the variant of chooser being the version recoveredfrom the Macintosh Classic ROM Disk. Normally, Chooser does not start up on a locked disk, except for the variant that is found on the Macintosh Classic ROM Disk.
As such, start with the system file as found in OSHintosh Disk.dsk, or OSHintosh Disk 6.0.8.dsk, as this includes this version of appleshare and chooser.

Additionally, the printer port/Appleshare is connector CN3 (middle connector), and the Modem connector is CN2 (leftmost connector). 

<img width="960" height="1280" alt="AppleShare on the OSHintosh" src="https://github.com/user-attachments/assets/391768f0-6cb2-4ce3-8ea0-90117cf2bb48" />


# Acknowledgements:
Many thanks to:
AlextheCat
Bitsavers
Bolle
Daniel Kottke
GuruThree 
HKZ
Mattmos
Max1zzz
Pgreenland
The PLD Archive

and many others!

