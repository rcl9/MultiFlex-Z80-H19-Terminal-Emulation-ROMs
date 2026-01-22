# New H19/VT52 firmware support for the Multiflex Video Display Terminal Kit of the 1982 era (3 ROM images)

Between June 1984 and February 1988 the author wrote this new Heath H19/VT52 (and basic ADM3A) emulation
support for the stock Multiflex Z80 Video Display Terminal Kit sold by Exceltronix of Toronto
Canada. The original stock firmware which came with the terminal was quite primitive at best,
only providing very basic text sending and receiving functionality. It was a nice piece of
hardware but with lackluster software support.

<div style="text-align:center">
<img src="/docs/multiflex terminal board (1982).jpg" alt="" style="width:75%; height:auto;">
</div>

If you are an existing owner of this terminal board then it is quite easy to migrate to
this new expanded level of terminal emulation by burning 1, 2 or 3 ROM images (2716, 2732 and 2764) and
making some optional minor hardware modifications. Source code and documentaiton is provided in
this Github repository so that you can modify the code as you may see fit.

It was stated in the [May 1984 issue](https://www.worldradiohistory.com/CANADA/ETI/80s/ETI-1984-05-Canada.pdf) of *Electronics Today International* (Canadian edition, page
37) that well over 500 of these boards had already been sold. As such, there should still be
many of them out in the open and ripe for these quick software and hardware upgrades.

The author purchased the first of two of his terminal boards in June 1984 for the 'low cost' of
CDN$218 ($149 for the board and $35 for the case + tax, during their special pricing promotion).
In current dollars, this would be CDN$590. The 1983 advertised list price for a pre-assembled and
tested kit was CDN$319 + 7% provincial tax which would be $918 in current dollars. Keep in mind
that entry level commercial terminals cost 3 to 4 times more than this kit at the time, hence why
they sold so many of these kits.

## History of the MultiFlex Z80 Video Display Terminal Kit

Multiflex Technology Inc. was one of the companies of the Exceltronix group controlled by Eugen
Hutka. In 1979, Hutka founded Exceltronix with its retail office at 319 College Street in
Toronto. In the early 1980s, Multiflex was developing complete computer systems based on the
Zilog Z80, Motorola 68000, and Intel 8086 processors together with a variety of peripheral and
expansion cards. The company also manufactured stand-alone intelligent terminals, video display
kits, as well as a range of other electronic products.

The *MultiFlex Z80 Video Display Terminal Kit* was introduced in 1982 and seemed to hit its
marketing peak in 1983 and early 1984 after which there was little mention of it probably due
to the rise of aftermarket IBM PC clones and related hardware + software.

## Multiflex Video Display Terminal Kit overview from various Exceltronix Catalogs of 1983 and 1984

The Multiflex video display terminal was originally designed as a low cost access unit.
The terminal is a semi-intelligent system controlled by a Z80A microprocessor and a 6845 CRT
controller chip. The keyboard is fully ASCII encoded and the character generator contains the
full 128-character set as well as a 128-character alternate set both of which are in 5x7
dot matrix format. The screen display is 80 characters by 24 lines if the unit is hooked to
an external monitor or 64 by 24 if run through an RF modulator to a TV. There are 3
software selectable attributes (dim, reverse video, and alternate character set) which can
be chosen one at a time for the whole screen. Also included are 2 RS232 ports:
one for a modem and one so that a printer can be attached to the terminal. The Multiflex
Video Display Terminal has provisions for an on board modem freeing a serial port. It
was first offered by Exceltronix in 1982.

<div style="text-align:center">
<img src="/Exceltronix Sales Catalogs/Exceltronics Catalog - 1983 - 2.jpg" alt="" style="width:30%; height:auto;">  <img src="/Exceltronix Sales Catalogs/Exceltronics Catalog - 1984.jpg" alt="" style="width:30%; height:auto;">
</div>

## New Features Of the Updated Firmware

- Emulation modes: VT52 or (very basic) ADM3A
- Five new video modes have been added to the original "24 x80" mode. These new video modes require the (generally) simple hardware level modification to IC 41.
   - 24 x 80
   - 25 x 80 (with status line)
   - 40 x 96, interlaced
   - 44 x 96, interlaced (with status line)
   - 24 x 96
   - 25 x 96 (with status line)

- Three screens with 8k installed memory:
   - Screen 1: 26 x 96 max
   - Screen 2: 26 x 96 max
   - Screen 3: 24 x 80 max
- The selection of baud rates was increased from 10 to 15.
- Screen saver mode with variable timeouts (in mins) of 10, 20 and 28.
- H19 function key emulation (with the modified keyboard ROM). In the keyboard ROM the codes
for SHIFT-CONTROL 1-8 have been mapped to F0 thru F7h. The software then maps it to
'ESC + Character' for transmit to the server (refer to the end of this file).
- Enhanced H19 character graphics mode via a revised character generator ROM for 0x5E through
0x7F (refer to the file '*H-19 terminal graphic definitions (new).pdf*' for the hand made dot
matrix definitions.
- Status line on/off
- Baud rates from 110 to 9600 on two RS-232 serial ports
- A basic set up ADM3A control codes: ^^, ^K, ^L and ^Z

- And a new, interactive setup screen:

<div style="text-align:center">
<img src="/docs/options setup screen capture.jpg" alt="" style="width:50%; height:auto;">
</div>

## Hardware Revision Instructions for your existing Multiflex Terminal

1. It is **highly recommended** that you re-cap the board with newer electrolytic and tantalum capacitors, especially
if you have not turned on the terminal board in 30 to 40 years. Be forewarned! Tantalums are generally
prone to exploding due to their dielectric crystalization. Unless you choose to properly 'reform'
the larger power supply filter electrolytics, they should really be replaced otherwise they
will probably exhibit excess ripple and potentially degrade and/or show a high(er) ESR.

2. You will want to burn a 2764 EPROM with the image from '*2764 - Terminal ROM - v2.6 - August 20 1986.rom*'.

3. If you would like to have H19 function keys on the keyboard (refer to the 'Features' section above)
then burn a 2716 EPROM with the image from '*2716 - Keyboard Encoder v6 - February 12 1988.rom*'.

4. If you would like to have H19 compliant graphic characters then burn a 2732 EPROM with the
image from '*2732 - Character Generator v4 - August 21 1988.rom*'. There is a PDF with the
hand-drawn definitions of each of the graphic characters.

5. If your terminal uses a 10Mhz crystal then it is recommended to replace it with a 13Mhz version
to speed up the Z80A processor. Newer boards manufactured by Multiflex may have had these modifications
made by default. The following minor hardware changes are also necessary:

        - Remove capacitor C4.
        - Jumper the crystal so that it is directly connected to pins 4 and 5 of IC U35 (74LS124, dual voltage-controlled oscillators).
        - Lift out pins 2 and 3 from IC35 from the socket. Jumper pin 2 to pin 16 (+5v). Jumper pin 3 to ground (pin 8).

6. The 5 new video modes require that IC U41 (74LS163, 4 bit binary counter) be removed from its socket. Pins 9
and 12 of that IC need to be soldered together on the bottom of the board (this makes the VSYNC
pulse the normal 16 scan lines that the 6845 expects for our newly expanded interlace mode).

## Local vs Remote Mode

During the era of computer terminals like the ADM3A, VT52 and VT100 (ie. the 1970s and early
1980s), accessing a remote computer was often done via a very slow 110 or 300 baud
acoustically coupled modem. Additionally, in some countries it was very expensive to remain
online with a telephone line for any appreciable time and hence offline text editing would
be more cost effecitve. As such, this terminal has a Local and Remote mode of operation.

In Remote mode, keyboard data entry is directly sent to the remote computer and the screen
displays characters received from the remote computer. There is no local editing
capabilities of the displayed text in this mode.

In Local mode, there are 3 different screens of text buffers. The idea is that text can
be downloaded from the remote computer to one of these 3 screen buffers and locally edited
in an offline mode. A Wordstar-like editor has been added to this new firmware for this exact
purpose. After the text has been edited locally it can then be transmitted back to the server.
Multiflex also stated in their User's Guide that a printer could be attached to the terminal
and hence used as a 'word processor'.

Enter the setup screen from remote mode with **Shift+Control+ESC** or **Shift+Caps+Control+ESC**.

## Remote mode VT52 ESCape parameters

One parameter ESCape sequences:

| Function Name | Keypress    | Description    |
| :-----: | :---: | :---: |
| cupline      	| A | Up cursor |
| curs$dwn	| B | Down cursor |
| curs$rite     | C | Right cursor |
| cur$left	| D | Left cursor |
| clr$scn	| E | Clear display and home |
| graf$on	| F | Enter graphics mode |
| graf$off	| G | Exit graphics mode |
| chome		| H | Home cursor |
| rev$lf	| I | Reverse line feed |
| eras$end	| J | Erase to end of screen |
| eras$right	| K | Erase right (to EOL) |
| ins$line	| L | Insert line |
| del$line	| M | Delete line |
| del$char	| N | Delete character |
| insert$off	| O | Exit insert character mode |
| eras$top	| b | Erase to top |
| cnull		| f | Alternate character set |
| cnull		| g | Normal character set |
| save$curs	| j | Save cursor position |
| recall$curs	| k | Recall cursor position |
| eras$line	| l | Erase entire line |
| read$curs	| n | Read cursor address |
| eras$left	| o | Erase left |
| csrtatr	| p | Enter reverse video mode |
| cstpatr	| q | Exit reverse video mode (normal video) |
| wrap$on	| v | Wrap around on |
| wrap$off	| w | Wrap around off |
| init$switches	| z | Reset switches to power on state |
| insert$on	| @ | Enter insert character mode |
| dis$keybd	| } | Disable keyboard |
| en$keybd	| { | Enable keyboard |
| trans$stat	| ] | Transmit status line |
| trans$page	| # | Transmit page |
| identify	| Z | Identify terminal as VT52 (ESC / K)|

Note about F0 - F7 function keys:

        - With the modified keyboard ROM, SHIFT-CONTROL 1-5 have been mapped to F0 through to F4 and then transmitted as ESC + S, T, U, V or W.

        - SHIFT-CONTROL 6-8 have been mapped to F5 through to F7 and then transmitted as ESC + P (Blue), Q (Red) or R (Gray).

Two parameter VT52 ESCape sequences:

| Function Name | Keypress    | Description    |
| :-----: | :---: | :---: |
| set$baud	| r | Set baud rate (Bn) |
| set$crtc$mode	| s | Set CRTC mode |
| test$mode	| t | Test mode |
| set$mode	| x | Show the setup screen |
| set$mode	| X | Set mode controls (Ps-x) |
| reset$mode	| y | Reset mode controls (Ps-y) |

Where 'Bn' = A (110 baud), B (150), C (300), D (600), E (1200), F (1800), G (2000), H (2400), I (3600), J (4800), K (7200) and L (9600)

| Value | Ps-x | Ps-y |
| :-----: | :---: | :---: |
| 0 | Blinking cursor (default) | Disable blinking cursor |
| 1 | Enable 25th line  | Disable 25th line |
| 2 | End of line beep |  |
| 3 | Hold screen mode | Exit hold screen mode |
| 4 | Block cursor | Underscore cursor |
| 5 | Cursor off | Cursor on |
| 6 | Keypad shifted mode (not supported) | Keypad unshifted mode (not supported) |
| 7 | Alternate keypad mode (not supported)| Exit alternate keypad mode (not supported) |
| 8 | Auto line feed on receipt of CR | No auto line feed |
| 9 | Auto CR on receipt of line feed | No auto CR |
| : | Select the second screen | |
| ; | Select the DIM attribute | Disable the DIM attribute |
| < | Turn Xon/Xoff handshaking off | Turn Xon/Xoff handshaking on  |

Three paramter VT52 ESCape sequences for direct cursor positioning: 'Y' + row and column (function = vt$direct$curs$cmd).

Three paramter VDM3A ESCape sequences for direct cursor positioning: '=' (function = adm$direct$curs$cmd).

## Local Mode Wordstar-like page editing commands

When the terminal is in Local mode, the following key sequences can be used to edit the screen
text buffers. Wordstar-like editing sequences have been added into the new terminal firmware.

| Function Name | Keypress    | Description    |
| :-----: | :---: | :---: |
| cstpatr	| ^B	| Normal video |
| cnxtpg	| ^C	| Flip display to the next page. Each page is 16 lines long so pages overlap. |
| curs$rite	| ^D	| Cursor right |
| cupline	| ^E	| Up cursor |
| cmoveol	| ^F	| Move to end of current line (end of text, start of spaces) |
| cbell		| ^G	| Send out Bell tone to speaker |
| cbs		| ^H	| Cursor left |
| ctab		| ^I	| Tab over to next TAB stop (every 8 characters) |
| clf		| ^J	| Send line feed |
| cclrscrn	| ^L	| Clear ALL of the video memory |
| cnewline	| ^M	| Send Carriage Return (with optional lf) |
| chome2	| ^Q	| Home the cursor (and move start of screen to start of video mem) |
| cprpge	| ^R	| Flip display to the previous page |
| cbs		| ^S	| Cursor left |
| txbuff	| ^T	| Transmit the buffer |
| csrtatr	| ^V	| Reverse video |
| cscrdwn	| ^W	| Scroll the screen down (soft scroll) |
| clf		| ^X	| Send line feed |
| cscrup	| ^Z	| Scroll the screen up (soft scroll) |
| cesc		| ESC	| ESCape processing (set cursor positioning flag) |

## Compilation and Linking

The main Z80 assembler source code is contained in the file named
'*term - v2.6 - H-19 (VT52-ADM3A) Emulation Terminal.mac*'.

This original v2.6 code from February 12 1988 was recompiled on an emulated CP/M system (running
Yaze-AG) in December 2025 and then compared against the original ROM image named '*2764 - Terminal ROM - v2.6 - August 20 1986.rom*'.
They were identical and hence the ROM image has been verified as being the last valid version created in 1988.

If you wish to compile and link the assembler file yourself then follow these steps:

1. Rename the file to 'term26.mac' and copy that file, m80.com and l80.com to your CP/M system or emulator.

2. Compile with: *m80.com =term26.mac*

3. Link with: *l80.com term26,term26/n/e*

The compiled and linked **term26.com** file will be the image to burn to the 2764 EPROM.

## Motorola 6845 Video chip parameter calculator

The file '*Motorola 6845 Video chip parameter calculator.asc*' contains a MBASIC program
which aids in the calculation of 6845 video chip parameters.

Copy this file to your CP/M machine or emulator as 'calc6845.asc', as well as the mbasic52.com
file, and excute as "mbasic52.com calc6845.asc'. Exit Mbasic using the 'system' command.

## DIP Switch Settings

Jumper J10 sets the following default start-up options of the terminal. The left side of the switch array is bit 7.

Note: 4 bits instead of 3 bits are now being used to set the baud rate compared to the original firmware.

| Bit # | Jumpered?| Description    |
| :-----: | :---: | :---: |
| | |
| Bit 7 | No jumper | Local mode |
| | Jumper | Remote mode |
| | |
| Bit 6 | No jumper | Dont enable auto screen-save mode |
| | Jumper | Enable auto-screen save mode |
| | |
|Bit 5 | No jumper | Dont use XON/XOFF protocol (DTR still active) |
| | Jumper | Use XON/XOFF as well as DTR |
| | |
|Bits 4,3,2,1 | | Baud rate |
| | |
|Bit 0:	| | Reserved by the keyboard strobe bit |

And these 4 switches determine the baud rate:

|Bits 4321 | Baud Rate |
| :-----: | :---: |
| |
| .... | 50 |
| ...x | 75 |
| ..x. | 110 |
| ..xx | 134.5 |
| .x.. | 150 |
| .x.x | 300 |
| .xx. | 600 |
| .xxx | 1200 |
| x... | 1800 |
| x..x | 2000 |
| x.x. | 2400 |
| x.xx | 3600 |
| xx.. | 4800 |
| xx.x | 7200 |
| xxx. | 9600 |
| xxxx | 19200 (not supported by 8251 UART) |

Where 'x = jumpered' and '. = not jumpered'.

## See Also

[Multiflex Video Display Terminal Kit](https://museum.eecs.yorku.ca/items/show/335) at the York University (Toronto) Computer Museum.

Electronics Today International (Canada) [Intelligent Terminal - Part II](https://www.worldradiohistory.com/CANADA/ETI/80s/ETI-1982-11-Canada.pdf) page 40.

[Exceltronix Z80 Multiflex Computer Kit](https://museum.eecs.yorku.ca/items/show/309) at the York University (Toronto) Computer Museum.

[Exceltronix Z80 Multiflex Computer Kit](https://jeffavery.ca/computers/z80exceltronix1.html) semi-failed restoration by Jeff Avery.

[Exceltronix Z80 Multiflex Computer Kit](https://www.s100computers.com/Hardware%20Folder/MultiFlex/History/History1.htm) reference by the S-100 Computers WEB site.

[University of Toronto M6809 computer](https://museum.eecs.yorku.ca/items/show/349) at the York University (Toronto) Computer Museum.

[Review of the University of Toronto M6809 computer by Steve Rimmer](https://www.worldradiohistory.com/CANADA/ETI/80s/ETI-CA-1982-05.pdf), May 1982, page 40.
