---
title: Gigabyte GA-G41M-ES2L desktop board 
...

<div class="specs">
<center>
![GA-G41M-ES2L]()
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Gigabyte                                       |
| **Name**                   | GA-G41M-ES2L                                   |
| **Released**               | 2009                                           |
| **Chipset**                | Intel G41                                      |
| **CPU**                    | Intel Core 2 Extreme/Quad/Duo, 
                                        Pentium Extreme/D/4 Extreme/4/Celeron |
| **Graphics**               | Integrated                                     |
| **Display**                | None.                                          |
| **Memory**                 | Up to 16GB                                     |
| **Architecture**           | x86_64                                         |
| **Original boot firmware** | AWARD BIOS                                     |
| **Intel ME/AMD PSP**       | Present. Can be disabled                       |
| **Flash chip**             | 2x8Mbit                                        |

```
W+: Works without blobs; 
N: Doesn't work; 
W*: Works with blobs; 
U: Untested; 
P+: Partially works; 
P*: Partially works with blobs
```

| ***Features*** |                                       |
|----------------|---------------------------------------|
| **Internal flashing with original boot firmware** | W+ |
| **Display**                                       | -  |
| **Audio**                                         | W+ |
| **RAM Init**                                      | P+ |
| **External output**                               | P+ |
| **Display brightness**                            | -  |

| ***Payloads supported***  |       |
|---------------------------|-------|
| **GRUB**              | Slow! |
| **SeaBIOS**               | Works |
| **SeaBIOS with GRUB** | Works |
</div>
This is a desktop board using intel hardware (circa \~2009, ICH7
southbridge, similar performance-wise to the ThinkPad X200. It can make
for quite a nifty desktop. Powered by libreboot.

As of Libreboot release 20221214, only SeaBIOS payload is provided in ROMs
for this board. According to user reports, they work quite well. GRUB was
always buggy on this board, so it was removed from lbmk.

IDE on the board is untested, but it might be possible to use a SATA HDD
using an IDE SATA adapter. The SATA ports do work, but it's IDE emulation. The
emulation is slow in DMA mode sia SeaBIOS, so SeaBIOS is configured to use PIO
mode on this board. This SeaBIOS configuration does not affect the Linux kernel.

You need to set a custom MAC address in Linux for the NIC to work.
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
under `cbutils/` after running the following command
in the build system:

	./update trees -b coreboot utils

You can learn more about using the build system, lbmk, here:\
[libreboot build instructions](../build/)

Flashing instructions can be found at
[../install/](../install/)

RAM
---

**This board is very picky with RAM. If it doesn't boot, try an EHCI debug
dongle, serial usb adapter and null modem cable, or spkmodem, to get a
coreboot log to see if it passed raminit.**

Kingston 8 GiB Kit  KVR800D2N6/8G with Elpida Chips E2108ABSE-8G-E

this is a 2x4GB setup and these work quite well, according to a user on IRC.

Nanya NT2GT64U8HD0BY-AD with 2 GiB of NT5TU128M8DE-AD chips works too.

Many other modules will probably work just fine, but raminit is very picky on
this board. Your mileage *will* fluctuate, wildly.
