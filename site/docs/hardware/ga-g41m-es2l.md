---
title: Gigabyte GA-G41M-ES2L desktop board 
...

This is a desktop board using intel hardware (circa \~2009, ICH7
southbridge, similar performance-wise to the ThinkPad X200. It can make
for quite a nifty desktop. Powered by libreboot.

NOTE: As of January 4th, 2021, video initialization is broken on this machine.
It is advisable to use Libreboot 20160907, for the time being. You can build a
ROM image from libreboot, and extract the CPU microcode updates to then insert in
the Libreboot 20160907 ROM image, like so (using cbfstool):

    cbfstool libreboot.rom extract -n cpu_microcode_blob.bin -f cpu_microcode_blob.bin
    cbfstool libreboot.rom add -n cpu_microcode_blob.bin -f cpu_microcode_blob.bin -t microcode

With this, you will then have a Libreboot ROM image, but with improved stability
due to microcode updates. The code in coreboot that checks for this file, in
CBFS, is present in every Libreboot release; Libreboot merely excludes the blob
itself, but does not delete the code for loading it. The Libreboot 20160907
release is reliable, on this board (but has a few issues, for example the PCI
express slots don't work).

The advice above is only useful for the onboard graphics chipset (the Intel
one). If you're using an add-on graphics card (PCI express), you can simply
use libreboot, and it will work. If you're doing *that*, please use one of the
ROM images with the *SeaBIOS* payload, booting in text mode. SeaBIOS will
automatically execute the option ROM on your graphics card, implementing VBE
(Video BIOS extension).

IDE on the board is untested, but it might be possible to use a SATA HDD
using an IDE SATA adapter. The SATA ports do work, but it's IDE emulation. The
emulation is slow in DMA mode sia SeaBIOS, so SeaBIOS is configured to use PIO
mode on this board. This SeaBIOS configuration does not affect the Linux kernel.

You need to set a custom MAC address in GNU+Linux for the NIC to work.
In /etc/network/interfaces on debian-based systems like Debian or
Devuan, this would be in the entry for your NIC:\
hwaddress ether macaddressgoeshere

Alternatively:

    cbfstool libreboot.rom extract -n rt8168-macaddress -f rt8168-macaddress

Modify the MAC address in the file `rt8168-macaddress` and then:

    cbfstool libreboot.rom remove -n rt8168-macaddress
    cbfstool libreboot.rom add -f rt8168-macaddress -n rt8168-macaddress -t raw

Now you have a different MAC address hardcoded. In the above example, the ROM
image is named `libreboot.rom` for your board. You can find cbfstool
under `coreboot/default/util/cbfstool/` after running the following command
in the build system:

    ./build module cbutils

You can learn more about using the build system, lbmk, here:\
[libreboot build instructions](../build/)

Flashing instructions can be found at
[../install/](../install/)

RAM
---

Kingston 8 GiB Kit  KVR800D2N6/8G with Elpida Chips E2108ABSE-8G-E

this is a 2x4GB setup and these work quite well, according to a user on IRC.

Many other modules will probably work just fine, but raminit is very picky on
this board.
