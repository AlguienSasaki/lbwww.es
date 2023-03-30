% ASUS KGPE-D16 hardware donation needed
% Leah Rowe
% 30 March 2023

This page will be updated after I've acquired the hardware.

Please donate a D16 machine if you can
======================================

If someone can donate one to me, that would be a great service to the Libreboot
project. Preferably assembled, with CPU, cooler, working RAM (in coreboot), in
a case with PSU... throw in a graphics card if you can.

ASUS KGPE-D16 support was removed from Libreboot a while ago, because I didn't
have enough testers to be confident in providing ROM images for it.

I would like to re-add support for ASUS KGPE-D16 in a future Libreboot release,
but this time I'd like to be able to test it myself.

I don't currently have a KGPE-D16 set up at my lab, because finding parts
and (especially) the coolers is a challenge, to say the least.

If you would like to help, and have a machine to spare, please can you contact
me at my email address: [info@minifree.org](mailto:info@minifree.org)

KCMA-D8 also needed
-------------------

I'm also arranging for an assembled machine with KCMA-D8 in it to be sent to
me - though I'm not yet sure if that will go through, so if you have one of
those aswell, I'd be interested too.

How I plan to re-add
====================

Dasharo produces updated coreboot images for KGPE-D16, with source code. They
took coreboot from release 4.11 and updated the code. I plan to add support in
lbmk (Libreboot's build system) for using other coreboot repositories besides
the official one, when downloading, patching and compiling for each board.

In other words, I would integrate Dasharo's coreboot repository in Libreboot,
alongside the default one on coreboot.org.

As far as I know, Dasharo does not yet work on KCMA-D8 (that was the case,
last time I checked), but I could inspect code differences between D8/D16 in
coreboot's branch `4.11_branch` and try to port those to Dasharo, to then put
in Libreboot.

Failing that (for KCMA-D8), I would just use `4.11_branch` from coreboot.
D8/D16 support was dropped in coreboot after release 4.11, but updated code
mostly fixing compiler issues and such, is available in a branch off
of 4.11 called `4.11_branch`.

When Libreboot dropped support for D8/D16, it wasn't using `4.11_branch`.
Instead, it was using the normal 4.11 tag in coreboot.git, with some extra
patches on top provided by Libreboot.
