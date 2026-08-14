---
title: Install Libreboot on Intel D945GCLF(2/D)
x-toc-enable: true
...

**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**

<div class="specs">
<center>
<img tabindex=1 alt="D945GCLF" src="https://av.libreboot.org/d945gclf/d945gclf.jpg" />
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Intel                                          |
| **Name**                   | D945GCLF(2/D)                                  |
| **Variants**               | D945GCLF, D945GCLF2, D945GCLFD                 |
| **Released**               | 2008                                           |
| **Chipset**                | 945GC                                          |
| **CPU**                    | Atom 230 (D945GCLF) / 330 (D945GCLF2/D)        | 
| **Graphics**               | GMA 950                                        |
| **Memory**                 | DDR2 SODIMM max. 2 GB, 1x2GB                   |
| **Architecture**           | x86\_64                                        |
| **Original boot firmware** | Intel BIOS                                     |
| **Intel ME/AMD PSP**       | Not present.                                   |
| **Flash chip**             | SOIC-8 512KiB                                  |

```
W+: Works without vendor firmware; 
N: Doesn't work; 
W*: Works with vendor firmware; 
U: Untested; 
P+: Partially works; 
P*: Partially works with vendor firmware
```

| ***Features*** |                                       | Notes |
|----------------|---------------------------------------|-------|
| **Internal flashing with original boot firmware** | N  |       |
| **Display**                                       | -  |       |
| **Audio**                                         | W+ |       |
| **RAM Init**                                      | W+ |       |
| **External output**                               | W+ |       |

| ***Payloads supported***  |              |
|---------------------------|--------------|
| **GRUB**              | Doesn't work |
| **SeaBIOS**           |    Works     |
| **SeaBIOS with GRUB** | Doesn't work |
</div>

### Flash a ROM image

#### Software

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

# THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)



If you're already running Libreboot, and you don't have flash protection turned on, internal flashing is possible.


#### Hardware

For general information, please refer to [25xx NOR flash
instructions](../install/spi.md) - that page refers to use of socketed flash.

The vendor bios write protects the flash so it requires external flashing to install libreboot on this device;

<img tabindex=1 alt="Intel D945GCLF flash chip position" src="https://av.libreboot.org/d945gclf/20160923_141550.jpg" />


### Build ROM image from source

The build target, when building from source, is thus:

	./mk -b coreboot d945gclf_512kb

with a 8mb flash;

	./mk -b coreboot d945gclf_8mb


### Remarks about vendor bios and small flash chip

Without coreboot/libreboot this board is completely useless, since the vendor bios is very bad. It cannot boot from any HDD whether it is connected to the SATA port or USB.

With libreboot it works fine, but the flash chip is very small, 512KiB, so grub2 does not fit, which is why libreboot has to use seabios


### How to replace thermal paste and fan

This board comes with very crappy disposable loud fan, that one has no
bearings, which can not be repaired or oiled properly. 

DO NOT waste your time trying to fix it, just buy one chinese same size fan
![](https://av.libreboot.org/d945gclf/20160923_141620.jpg)\
![](https://av.libreboot.org/d945gclf/20160923_141614.jpg)\
Make sure that new one has same wiring

![](https://av.libreboot.org/d945gclf/20160923_142618.jpg)\
This is a new one, with bearing and maintenable

![](https://av.libreboot.org/d945gclf/20160923_141738.jpg)\
![](https://av.libreboot.org/d945gclf/20160923_141814.jpg)\
Now remove the both coolers rotating them a bit, slowly, then clean both
silicons and both coolers (removing cmos battery first is recommended)

![](https://av.libreboot.org/d945gclf/20160923_141601.jpg)\
Put a little bit of non conductive thermal paste on both silicons (only
cpu silicon iis shown on that image)

![](https://av.libreboot.org/d945gclf/20160923_142031.jpg)\

Before assembling new fan, some need new longer screws, make sure having
these (on the left is original one, too short for new fan)

![](https://av.libreboot.org/d945gclf/20160923_141659.jpg)\
After that, assemble your new fan into CPU cooler

![](https://av.libreboot.org/d945gclf/20160923_141635.jpg)\
Finally assemle both coolers on both chips, do not forget put in the CPU
fan connector back, and you are done.



### Expanded_Specifications

<div class="expandedspecs">
<center>
<img tabindex=1 alt="d945gclf_io+exp" src="https://av.libreboot.org/d945gclf/d945gclf.jpg" />
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Intel                                          |
| **Name/Variant**           | D945GCLF                                       |
| **Released**               | 2008                                           |
| **Chipset**                | 945GC                                          |
| **CPU**                    | Atom 230                                       |
| **Memory**                 | DDR2 SODIMM max. 2 GB, 1x2GB                   |
| **Audio**                  | Realtek ALC662                   |
| **Networking**             | RealTek RTL8102EL                              |
| **Expansion Slots**        | PCI |
| **Storage connectors**     | 2x SATA II , IDE     |
| **Other Connectors**       | Headers; USB 2.0  |
| **I/O Ports on Back**      | PS/2 Mouse+Keyboard , SCOM , VGA , Parallel , 4x USB 2.0 , 100Mbit Ethernet  |

</div>

<div class="expandedspecs2">
<center>
<img tabindex=1 alt="d945gclf2_io+exp" src="https://av.libreboot.org/d945gclf/20160923_141521.jpg" />
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Intel                                          |
| **Name/Variant**           | D945GCLF2(D)                                   |
| **Released**               | 2008                                           |
| **Chipset**                | 945GC                                          |
| **CPU**                    | Atom 330                                       |
| **Memory**                 | DDR2 SODIMM max. 2 GB, 1x2GB                   |
| **Audio**                  | Realtek ALC662                   |
| **Networking**             | Realtek RTL8111C                               |
| **Expansion Slots**        | PCIe 2.0 x16 wired in x4 , PCI |
| **Storage connectors**     | 2x SATA II , IDE      |
| **Other Connectors**       | Headers; 2x dual USB 2.0  |
| **I/O Ports on Back**      | PS/2 Mouse+Keyboard , SCOM , VGA , Parallel , 4x USB 2.0 , 1GbE Ethernet , S-VIDEO*1  , S/PDIF |

</div>

*1 D945GCLF2D does not have it

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/d945gclf.html>
