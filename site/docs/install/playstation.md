---
title: Sony PlayStation (PS1/PSX)
x-toc-enable: true
...

The PlayStation is a computer like any other. It ought to run all Free Software
if you want it to, and you can!

<div class="specs">
<center>
<img tabindex=1 alt="Sony PlayStation (PSOne) console" class="p" src="https://av.libreboot.org/playstation/psone.jpg" /><span class="f"><img src="https://av.libreboot.org/playstation/psone.jpg" /></span>
<img tabindex=1 alt="PCSX-Redux Open BIOS" class="p" src="https://av.libreboot.org/playstation/openbios.png" /><span class="f"><img src="https://av.libreboot.org/playstation/openbios.png" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Sony Computer Entertainment Inc.               |
| **Name**                   | PlayStation                                    |
| **Variants**               | PlayStation, PSOne, Net Yaroze                 |
| **Released**               | 1994 (Japan), 1995 (Worldwide)                 |
| **CPU**                    | MIPS R3000 @ 33.8688 MHz                       |
| **Graphics**               | Custom 3D processor by Toshiba, 1MB Video RAM  |
| **Sound**                  | 16-bit custom Sony SPU                         |
| **I/O**                    | CD-ROM, analog Audio/Video, Serial, Parallel   |
| **Memory**                 | 2MB EDO DRAM                                   |
| **Architecture**           | MIPS I instruction set (RISC)                  |
| **Original boot firmware** | Sony PS1 BIOS (USA/JPN/EU)                     |
| **Flash chip**             | 512KB Mask ROM                                 |
</div>
Introduction
============

**Unavailable in Libreboot 20240612 or earlier. You must compile from source;
first, check the [main build guide source](../build/) to get dependencies.**

You can use the following command in lbmk (on Linux) to build the BIOS:

	./mk -b pcsx-redux

This command only compiles the BIOS code found in `src/mips/openbios/`, of
PCSX-Redux, which is downloaded to `src/pcsx-redux/`, so the BIOS source code
in full would be located at `src/pcsx-redux/src/mips/openbios` within lbmk.

You will then find the BIOS image under `bin/playstation/openbios.bin`. This
can be used on PlayStation emulators, and it can also be used on the real
hardware. It has a relatively [high
compatibility](https://docs.google.com/spreadsheets/d/1UNGs7uYb8viAbm7YJaf1CR4dkgX7ZzntUdcowGsjcVc/edit?pli=1&gid=772799649#gid=772799649) according to the developers.

This uses the free/opensource BIOS developed by the PCSX-Redux team, which you
can learn more about here:

<https://github.com/grumpycoders/pcsx-redux/tree/main/src/mips/openbios>

NOTE: As of 26 September 2024, lbmk for PlayStation has only been build-tested
on Debian 12 (x86\_64). The build dependencies script provides a `mipsel` cross
compiler; otherwise, PCSX-Redux's page has general instructions for how to get
a compatible toolchain:\
<https://github.com/grumpycoders/pcsx-redux/blob/main/src/mips/psyqo/GETTING_STARTED.md>

Installation
============

Emulators
---------

Most PlayStation emulators rely on low-level emulation to execute the real BIOS.
The Open BIOS by PCSX-Redux (as distributed by Libreboot) can also be used, and
boots many commercial games, plus homebrew.

These emulators can boot many commercial games, with varying degrees of
compatibility, and they can also boot any homebrew/opensource games that you
might develop yourself, or that others have written.

Hardware
--------

Not yet tested by the Libreboot project, but the PCSX-Redux developers have
stated that it will work on the real console. It should be noted that the Open
BIOS does not seem to implement a memory card save handler at present. If
someone can recommend a boot disc that could replace this, that'd be nice;
otherwise, read on.

Game compatibility
==================

The upstream maintains a compatibility list, here:

<https://docs.google.com/spreadsheets/d/1UNGs7uYb8viAbm7YJaf1CR4dkgX7ZzntUdcowGsjcVc/edit?pli=1&gid=772799649#gid=772799649>

NOTE: Google Docs, but an option exists on there to export it for LibreOffice
Calc. The list is provided as a spreadsheet.

Memory card handling
====================

Basically, the Open BIOS from PCSX-Redux does not yet support handling the
memory cards. The game compatibility is also not complete, though it is
excellent. Therefore, you might consider a *dual BIOS* setup - and this has
not been tested, but should work just fine (it will be tested at a later date).

Using it on hardware
====================

The dual BIOS works like so: manipulate the OE/chip select pins on the BIOS
chip (original one), between that and the one on the replacement chip. You
would have both chips present, one of them containing your Open BIOS. You would
control the OE/CS pins via a toggle switch. When the switch is set one way, it
boots the original Sony BIOS, and the other position boots the Open BIOS. You
would take care not to switch it while the machine is in operation, but it could
be set before turning the machine on.

Modern NOR flash can be used. You specifically want a TSOP-32 SMD/SMT type device,
one that operates at 3.3v (tolerance 2.7 to 3.6v), organised into 512KB blocks,
for example 512KB x8 is a common part, for example [SST39VF040](https://eu.mouser.com/ProductDetail/Microchip-Technology/SST39VF040-70-4C-WHE?qs=I6DYTe5pEPUDs7BrdNtR9w%3D%3D).

Look here:
<https://oshpark.com/profiles/mi213>

This person has designed PCBs that can be used to add a NOR flash, adapting to
the pinout of the playstation BIOS. Some early launch model PlayStations used
a 40-pin chip but most later ones soon after and to end of life were 32-pin.
This page only sells adapters for use with 32-bit boards.

For most boards, the one you want is probably the `PS1_Flash_Bios_(A)` one.
You must flash the chip first, before soldering it onto this adapter board.
You can then de-solder the BIOS chip; look for a 512K mask ROM on the board.

Make sure the adapter is soldered properly, and solder it into place. You can
find some videos online for how to do this. Libreboot cannot verify the quality
of all of them and may provide such guidance itself in the future.

**Do not** assume that the dual-BIOS idea will even work, because to our
knowledge, it has not been tested. It is highly theoretical at this point, and
recommended (assuming it would work) instead of simply replacing the original
BIOS outright. Even if game compatibility were 100%, it helps to have a backup
solution in case of bugs.

There are a few edge cases: PsNee (referenced below) might not work well on
some Japanese playstations, or PAL PSOne Slim, due to extra restrictions in
the Sony BIOS; therefore, if doing Dual BIOS, it might be wise to use two of
those adapter PCBs stacker, using the same OE/CE switch idea. On the one
containing Sony BIOS, you might use a dump for US NTSC console for example, if
setting it up on a PAL PSOne or Japanese console. Japanese consoles otherwise
have additional region checks at startup, and PAL PSOne slims check what video
mode a game starts in.

This page may or may not be updated at some point, to document installation on
real hardware. For now, the Open BIOS is only intended for use with emulators.

Video timings
-------------

The purpose of these kinds of mods *on hardware* is typically that the user
wants to remove all DRM. Well, there's something many people overlook: video
timings out-of-region are often wrong on these machines. The GPUs have two
clock inputs (early launch models only have one clock input): one for PAL and
one for NTSC. More info could be written here at a later date but these consoles
typically don't have a PAL GPU clock on NTSC consoles, and vice versa. You can
correct this.
Look up the PS1 "dual frequency oscillator" mod.

Other mods
==========

If using hardware, you will probably still want a modchip. Many proprietary
modchip firmwares exist, such as Old crow, MultiMode3 and Mayumi; these run on
PIC chips e.g. PIC12C508A. A special *OneChip* firmware is often used on PAL
PSOne Slims.

There is a *free/opensource* modchip type called PsNee, which is what Libreboot
recommends:
<https://github.com/kalymos/PsNee>

Unlike the other modchips, PsNee runs mostly on Arduino-type boards. You can
find more info on its GitHub page.

The way copy protection works on a playstation is: the commercial discs have
a special wobble in them, that can't be reproduced on consumer hardware. This
wobble encodes region data for the disc, and it's checked for by the CD controller;
if no wobble is present, or the region is incorrect, the disc stops booting,
though it will still play music CDs regardless (in the Sony BIOS).

The Open BIOS will contain no restrictions whatsoever, but you are still advised
to have a modchip.

The [PicoStation](https://github.com/paulocode/picostation) project provides
free firmware for RP2040 devices, which you can solder into a modboard which
then emulates the PS1 CD drive. It then lets you boot software (including
many games) from an SD card instead of a CD, using disc image files.

Combined with PsNee and PicoStation, the Open BIOS from PCSX-Redux team will
turn your 90s PlayStation into a very hackable machine. There is
also PSXSDX (which is also free/opensource) allowing for libre game
development, also known (somewhat ambiguously) as "homebrew" development. The
fact that these old consoles were designed to boot proprietary games, they are
simply computers, fully reprogrammable and as such, Libreboot is happy to provide
this support, for the Sony PlayStation

This page is very new, as of 26 September 2024, because the PS1 Open BIOS was
only added on this day (26 September). It'll be fleshed out over time, especially
to talk about hardware. The Open BIOS does boot a lot of software, but it is
mostly only recommended for use in emulators at this time.

Credit
======

Thanks go to the PCSX-Redux team for their excellent work reverse engineering
the Sony PS1 BIOS.

The two photos shown are as follows:

* PSOne Slim photo was taken from here (the one provided by Libreboot is
  scaled down and cropped, with color levels enhanced for better contrast):
  <https://commons.wikimedia.org/wiki/File:PS_One_with_LCD.JPG> - 
  released under the terms of Creative Commons Attribution-Share Alike 3.0 Unported license
* The cube screenshot is of the PCSX-Redux Open BIOS in operation, on an emulator.

PlayStation, PS1, PSOne and other terms are the trademark of Sony Interactive
Entertainment. Libreboot is in no way affiliated to Sony Interactive
Entertainment; this page is not an endorsement of Sony Interactive Entertainment,
nor do they endorse Libreboot.

PCSX-Redux developers are not affiliated with Libreboot in any way. We simply
integrate their Open BIOS into Libreboot because their work is awesome.
