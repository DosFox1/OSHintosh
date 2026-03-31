# OSHintosh
OSHintosh - an open source hardware 68000 Macintosh. None of the hardware is from Apple!
DISCLAIMER
This board has not been built yet, and is to be treated as untested! 

# Renders of the Board
<img width="1153" height="922" alt="OSHintosh Top Low Res" src="https://github.com/user-attachments/assets/6172a52b-ca96-4468-98c5-6761f669c19e" />
<img width="1153" height="922" alt="OSHintosh Bottom Low Res" src="https://github.com/user-attachments/assets/d7e16204-f0f5-4879-932b-297293a49fdb" />

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
3) 1 x ATTINY85v (RTC)

# Hard to Source Components:
Please Note that the following ICs may be difficult to source:
1) 15.6672mhz oscillator (ebay may have some, if not, custom oscillators can be ordered)
2) Zilog Z8530 SCC
3) 6522 VIA
4) 41256 DRAM (x16!)

Please also note this assumes that you have access to either original Macintosh Peripherals, or will add your own adapters!
The OSHintosh uses the original quadrature 9 pin mouse, as well as the original RJ Keyboard. 

# PAL JED Files:
The PAL Equations can be found on the Macintosh Archive:
https://wiki.pldarchive.co.uk/index.php?title=Macintosh_128k/512k/Plus
TODO - convert PAL files to GALs, and extract equations 

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

