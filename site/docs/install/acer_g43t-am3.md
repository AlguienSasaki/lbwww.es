---
title: Install Libreboot on Acer G43T-AM3
x-toc-enable: true
...

**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**

<div class="specs">
<center>
<img tabindex=1 alt="acer_g43t-am3" class="p" src="https://av.libreboot.org/acer_g43t-am3/picture.jpg" /><span class="f"><img src="https://av.libreboot.org/acer_g43t-am3/picture.jpg" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Acer                                          |
| **Name**                   | G43T-AM3                               |
| **Released**               | 2009                                           |
| **Chipset**                | G43                                     |
| **CPU**                    | LGA 775, Core 2 (Duo & Quad)                   |
| **Graphics**               | GMA X4500 |
| **Memory**                 | DDR3 SODIMMs (max 8 GB, 4x2GB)    |
| **Architecture**           | x86_64                                         |
| **Original boot firmware** | Acer BIOS                             |
| **Intel ME/AMD PSP**       | Present, Fully removable      |
| **Flash chip**             | SOP-8 2MB/16Mbit MX25L1605D           |


```
W+: Works without vendor firmware; 
N: Doesn't work; 
W*: Works with vendor firmware; 
U: Untested; 
P+: Partially works; 
P*: Partially works with vendor firmware
?: UNKNOWN AT THIS TIME
```

| ***Features***                                    |    |
|---------------------------------------------------|----|
| **Internal flashing with original boot firmware** | W+ |
| **Display (if Intel GPU)**                        | W+ |
| **Audio**                                         | W+ |
| **RAM Init**                                      | W+ |

| ***Payloads supported***   |           |
|----------------------------|-----------|
| **GRUB (libgfxinit only)** | Works    |
| **SeaBIOS**                | Works    |
</div>



### Get the ROM image

**[Download](https://libreboot.org/download.html) from release**


**[Build](https://libreboot.org/docs/build/) from source**

For one only:

	./mk -b coreboot g43t_am3

To build it to use both flash chips:

	./mk -b coreboot g43t_am3_16mb


### Flash a ROM image

**Software**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)

External header and internal flashing works. 

It might be necessary to specify the chip type; MX25L1605D/MX25L1608D/MX25L1673E is the correct one, not MX25L1605.


**Hardware**

To flash it externally, turn the board off and leave it plugged in with the psu on and then use a programmer WITHOUT the *3.3v connected* to flash. 

Another way is through the SPI_ROM1 header while the board is off and disconnected from power, HOLD# and WP# must be pulled high (to VCC) to be able to flash the chip

It might be necessary to specify the chip type; MX25L1605D/MX25L1608D/MX25L1673E is the correct one, not MX25L1605.


### Networking (might be Outdated)

This is similar to Gigabyte GA-G41M-ES2L but uses an Intel NIC rather than
Realtek. Some problems with Linux on this NIC, on this board, with Libreboot,
were observed; see:

<https://notabug.org/libreboot/lbmk/issues/125>

That page (on notabug) has some notes about workarounds. It links to this:

<https://superuser.com/questions/1104537/how-to-repair-the-checksum-of-the-non-volatile-memory-nvm-of-intel-ethernet-co/1106641#1106641>

This page has some guidance on how to either correct the checksum (in GbE
config) or skip checksum validation in Linux, to get the onboard NIC working.
Although it's talking about different hardware, the steps should be the same.

TODO: factory BIOS on this board works fine with the onboard NIC. study what
that is doing


### Memory Modules ([might be Outdated](https://doc.coreboot.org/mainboard/acer/g43t-am3.html#not-working))

DDR3 memory with 512Mx8 chips (G43 limitation), 4x4GB sticks (works, but showed a single bit error within one pass of Memtest86+ 5.01)


### Expanded_Specifications

<div class="expandedspecs">
<center>
<img tabindex=1 alt="acer_g43t-am3" class="p" src="https://av.libreboot.org/acer_g43t-am3/io+exp.jpg" /><span class="f"><img src="https://av.libreboot.org/acer_g43t-am3/io+exp.jpg" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Acer                                          |
| **Name/Variant**           | G43t-AM3                               |
| **Released**               | 2009                                           |
| **Chipset**                | Intel [G43](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#Core_2_chipsets) |
| **CPU**                    | LGA775, Intel [Core 2](https://en.wikipedia.org/wiki/List_of_Intel_processors#Intel_Core_2)        |
| **Graphics**               | [GMA X4500](https://www.techpowerup.com/gpu-specs/gma-x4500.c3116)                                       |
| **Audio**                  | Realtek ALC888S |
| **Memory**                 | DDR3 SODIMMs (max 8 GB, 4x2GB)                      |
| **Networking**             | Intel 82567V-2                                          |
| **Expansion Slots**        | PCIe gen. 2; x16, x1, 2x PCI |
| **Storage connectors**     | 6x SATA II       |
| **Other Connectors**       | Headers; Audio, 4x dual USB 2.0, SCOM |
| **I/O Ports on Back**      | PS/2 Mouse+Keyboard, VGA, HDMI, 4x USB 2.0, IEEE 1394 (Firewire), RJ-45 1GbE (Ethernet) |

</div>


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/acer_g43t-am3.html>
