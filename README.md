# TokuROM
**A unified ROM displacement cable system**

## Overview
TokuROM is a collection of adapter PCBs with a unified cable pinout for flexible ROM displacement setups.
It allows ROM chips to be relocated or replaced with an equivalent device at the other end of a cable.
A typical use case is to put an EPROM socket somewhere you can access it on an assembled device. 

The majority of TokuROM parts are entirely passive, serving only to move signals from one place to another.
The cabling has a unified pinout which carries 16 data bits with up to 23 address bits,
or 8 data bits with up to 24 address bits (A-1 shared with D15).
The cable pinout is based on the popular 16-bit mask ROMs / EPROMs with a /BYTE input,
but may be passively adapted to almost any kind of ROM pinout.
There is also a provision for a few additional control signals such as a remote reset signal. 

In TokuROM terminology, the "target" is any system with a position for a ROM chip,
and the "remote" is a ROM chip or equivalent that has been relocated.
The boards are named accordingly, with additional "converter" boards for special purposes. 

## Cabling
There are two kinds of cabling used in TokuROM, with a simple mapping from one to the other. 

The first uses one or two IDC cables: 40-pin + 10-pin, or 40-pin alone.
IDC allows cheap readily-availble cables and connectors to be used,
and the choice of a split setup allows connecting smaller ROMs with fewer address lines via a more compact cable setup.
The 40-pin section is directly compatible with **non-JEDEC** 40-pin chips, like the 27C400 EPROM and similar mask ROMs,
and can be used with IDC DIP displacement plugs.
The 10-pin section carries additional address lines, extra power, a reset line, and some currently reserved pins. 

The second kind of cabling is 50-pin 0.5mm **same-side** (aka forward or type A) FFC.
These are much more compact than IDC so only use a single cable, and are suitable for smaller target packages.
The pinout is the same as the 40+10 IDC arrangement combined, with the 10-pin section at the "top" to keep
address lines grouped together for 42-pin or 44-pin targets, and the 40-pin section is below that.
FFC connectors are not rated for many insertion cycles, so these should be used for semi-permanent connections.
If a cable will be inserted and removed often, consider using two cables and a replaceable FFC coupler in the middle. 

IDC cables are keyed and will always be the correct orientation.
FFC cables are also not keyed and heavily orientation-dependent.
TokuROM uses same-side cables and appropriately mirrored pinouts between target and remote.

*TODO: pinout diagrams*

## Included Boards
The following list will grow over time as I make more boards and connection types for a variety of uses.
Boards marked with `*` may be **untested** currently, assemble at your own risk.
I will of course aim to test all the boards as the project evolves.
Generally `I` suffix for IDC and `F` suffix for FFC connections.
Boards with neither of those may have both, or be too small for IDC in which case FFC is implied.

Target boards:
- `Target-DIP40MI`\*: DIP40 to IDC40, mask-ROM pinout **not JEDEC**. Equivalent to IDC-DIP transition plug but GND pins shorted.
- `Target-DIP42I`\*: DIP42 to IDC40+10. Defaults to 16Mbit pinout (like 27C160), jumper for 32Mbit (like 27C322). A20-A22 and reset are brought out to pin headers.
- `Target-DIP42IH`\*: DIP42 to IDC40+header. Minimal pin header with extra address lines for compactness. Same jumper as Target-DIP42.
- `Target-SOP44`: SOP44 to FFC50, SMD mask-ROM pinout. Castellated board edges for soldering directly. Has a pad for reset signal.

Remote boards:
- `Remote-ZIF42`\*: ZIF48 to IDC40+10 and FFC50, for 40P/42P EPROMs. Has 16M/32M jumper, manual reset button, A20-A22 and reset on a pin header.
- `Remote-TSOP48`: 50P FFC to TSOP48 NOR flash, 29F/29LV series pinout. Useful for semi-permanent setups. Program with the below converter. Some pins repurposed.

Converter boards:
- `Convert-Flash-TL866`: Programmer for NOR flash remote boards. Use with Xgecu TL866x "minipro" programmer and SN001-1 adapter board.

## Currently Planned
These boards don't exist or aren't ready for use yet but are planned to be added at some point.
You may politely nag me or donate if you'd like any particular one to happen a little faster.

Target planned:
- `Target-DIP40J(I/F)`: JEDEC pinout version of DIP40 target, like 27C4096. Haven't routed yet as I have no devices that use this.
- `Target-DIP28(I/F)`: DIP28 to IDC40 or FFC50 for 8-bit ROMs.
- `Target-DIP40MF`: FFC50 version of DIP40M target.
- `Target-DIP42F`: FFC50 version of DIP42 target.
- `Target-SOP40`: smaller version of SOP44 target for narrower SOP40 footprint. SOP44 target can manage it for now but fiddly to solder.

Remote planned:
- A flash-based Romulator, separate project but will use TokuROM interface.

Converter planned:
- IDC40+10 to FFC50 in a compact form. Not sure how to manage FFC orientation differences.
- Inline voltage translation for NOR flash remotes with modern LV flash. Not urgent as they can survive running at 5V.
- Combiners for 2 to 4 DIP40 or DIP28 targets into a single TokuROM cable, for devices with split ROMs.

## Notes on Assembling
Some of the boards, especially those with FFC connectors, require some very fine SMD soldering.
The boards don't currently use full part numbers for the BOM but I recommend using a PCB assembly service if you can.
FFC connectors are bottom-contact type, Hirose FH12 or clones, they are a very common type.
