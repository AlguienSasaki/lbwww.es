---
title: Install Libreboot on Gigabyte GA-G41M-ES2L
...


**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**


<div class="specs">
<center>
GA-G41M-ES2L
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Gigabyte                                       |
| **Name**                   | GA-G41M-ES2L                                   |
| **Released**               | 2009                                           |
| **Chipset**                | G41                                            |
| **CPU**                    | LGA775, Core 2 (Duo+Quad)                     |
| **Graphics**               | GMA X4500                                      |
| **Memory**                 | DDR2 SODIMMs max. 8GB (2 x 4GB)                |
| **Architecture**           | x86_64                                         |
| **Original boot firmware** | AWARD BIOS                                     |
| **Intel ME/AMD PSP**       | Present, Fully removable                       |
| **Flash chip**             | SOIC-8 2x 1MB/8Mbit SST25VF080B  (Dual-BIOS)   |

```
W+: Works without vendor firmware; 
N: Doesn't work; 
W*: Works with vendor firmware; 
U: Untested; 
P+: Partially works; 
P*: Partially works with vendor firmware
```

| ***Features*** |                                       |
|----------------|---------------------------------------|
| **Internal flashing with original boot firmware** | W+ |
| **Display**                                       | W+ |
| **Audio**                                         | W+ |
| **RAM Init**                                      | P+ |
| **External output**                               | W+ |

| ***Payloads supported***  |       |
|---------------------------|-------|
| **GRUB**              | Slow! |
| **SeaBIOS**           | Works |
| **SeaBIOS with GRUB** | Works |
</div>


### Get the ROM image

**[Download](https://libreboot.org/download.html) from release**


**[Build](https://libreboot.org/docs/build/) from source**

The build target, when building from source, is thus:

	./mk -b coreboot ga_g41m_es2l


### Flash a ROM image

**Software**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


There are 2 flash chips (one is backup).

Flash the first chip:

	./flashprog -p internal:dualbiosindex=0 -w libreboot.rom

Flash the second chip:

	./flashprog -p internal:dualbiosindex=1 -w libreboot.rom

**Warning**: If you don't flash both chips, the recovery program from the default
factory BIOS will kick in and your board will be soft bricked. Make sure that
you **flash both** chips!

Otherwise you can still boot the system with just the main flash chip
connected, after desoldering the backup chip. This has been tested while
libreboot was already installed onto the main chip.


**Hardware**

Refer to [spi.md](spi.md) for how to set up an SPI programmer for
external flashing. *You can only externally reprogram one of the chips
at a time, and you need to disable the chip that you're not flashing,
by connecting 3v3 to /CS of that chip, so you will actually need second test
clip or IC pin mini grabber.*

The flash shares a common voltage plane with the
southbridge, which draws a lot of current. This will cause under-voltage on
most SPI flashers, so do not use the 3.3V rail from your flasher. Do not
connect +3.3V to the chip. Instead, turn the board on and then turn it off by
holding the power button. With the board powered down, but plugged in, there
will be a 3.3V supply from the ATX PSU. You can then flash, but DO NOT connect
the +3.3V supply from your SPI flasher!

NOTE: You should use a resistor in series, between 1K to 10K ohms, for the 3.3v
connection to the CS pin. This is to protect from over-current.

Here is an image of the flash chip:\
![](https://av.libreboot.org/ga-g41m-es2l/ga-g41m-es2l.jpg)


### Networking (might be Outdated)

Due to a bug in the hardware, the MAC address is hardcoded in
coreboot. Therefore, you must set your own MAC address in your
operating system.

Use macchanger in your
distro, to set a valid MAC address. By doing this, your NIC should
work nicely.
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

	./mk -d coreboot TREENAME

You can learn more about using the build system, lbmk, here:\
[libreboot build instructions](../build/)


### Memory Modules ([might be Outdated](https://doc.coreboot.org/mainboard/gigabyte/ga-g41m-es2l.html#not-working))

**This board is very picky with RAM. If it doesn't boot, try an EHCI debug
dongle, serial usb adapter and null modem cable, or spkmodem, to get a
coreboot log to see if it passed raminit.**

Kingston 8 GiB Kit  KVR800D2N6/8G with Elpida Chips E2108ABSE-8G-E

this is a 2x4GB setup and these work quite well, according to a user on IRC.

Nanya NT2GT64U8HD0BY-AD with 2 GiB of NT5TU128M8DE-AD chips works too.

Many other modules will probably work just fine, but raminit is very picky on
this board. Your mileage *will* fluctuate, wildly.


### Removed GRUB (might be Outdated)

As of Libreboot release 20221214, only SeaBIOS payload is provided in ROMs
for this board. According to user reports, they work quite well. GRUB was
always buggy on this board, so it was removed from lbmk.


### SATA in IDE emulation (might be Outdated)

IDE on the board is untested, but it might be possible to use a SATA HDD
using an IDE SATA adapter. The SATA ports do work, but it's IDE emulation. The
emulation is slow in DMA mode sia SeaBIOS, so SeaBIOS is configured to use PIO
mode on this board. This SeaBIOS configuration does not affect the Linux kernel.


### Fan control issue (might be Outdated)

PWM fan speed is not changing in depend of CPU temperature based on SuperIO fan control


### Expanded_Specifications

<div class="expandedspecs">
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Gigabyte                                          |
| **Name/Variant**           | GA-G41M-ES2L                               |
| **Released**               | 2009                                           |
| **Chipset**                | Intel [G41](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#Core_2_chipsets) |
| **CPU**                    | LGA775, Intel [Core 2](https://en.wikipedia.org/wiki/List_of_Intel_processors#Intel_Core_2)        |
| **Graphics**               | [GMA X4500](https://www.techpowerup.com/gpu-specs/gma-x4500.c3116)                                       |
| **Audio**                  | Realtek ALC888B |
| **Memory**                 | DDR2 SODIMMs max. 8GB (2 x 4GB)   |
| **Networking**             | Realtek RTL8111C                                        |
| **Expansion Slots**        | PCIe 2.0; x16, x1, PCI |
| **Storage connectors**     | 2x SATA II, IDE, Floppy (iTE IT8718 chip)      |
| **Other Connectors**       | Headers; Audio, 2x dual USB 2.0, SCOM,  S/PDIF |
| **I/O Ports on Back**      | PS/2 Mouse+Keyboard, VGA, Parallel, 4x USB 2.0, D-SUB, RJ-45 1GbE (Ethernet), 3 x audio jacks |

</div>


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/ga-g41m-es2l.html>
