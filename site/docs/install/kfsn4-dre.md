---
title: Install Libreboot on ASUS KFSN4-DRE
x-toc-enable: true
...

<div class="specs">

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | ASUS                                           |
| **Name**                   | KFSN4-DRE                                      |
| **Released**               | 2007                                           |
| **Chipset**                | nVIDIA nForce Pro 2200                         |
| **CPU**                    | 2x Socket F; Opteron 2300/8300                 |
| **Graphics**               | XGI Z9s                                        |
| **Memory (max/cpu)**       | 32GB DDR2 SODIMMs (8x4 GB) ECC                 |
| **Architecture**           | x86_64                                         |
| **Original boot firmware** | AMI BIOS                                       |
| **Intel ME/AMD PSP**       | None                                           |
| **Flash chip**             | PLCC 1MiB                                      |

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
| **Display (No 3D accel.)**                        | -  | 
| **Audio**                                         | W+ |
| **RAM Init**                                      | W+ |
| **External output**                               | W+ |

</div>

### Get the ROM image

**[Download](../../download.md) from release**


**[Build](../build/) from source**

	./mk -b coreboot asus_kfsn4_dre_1mb

*Replacable and upgradeble flash chip sizes possible, if done, adjust the 2mb to it*


### Internal/Software Flashing

The flash might not be protected, possibly installable from vendor firmware.

**Backup and install according to the [guide](../install/simplified-sw-flashing.md)**


### External/Hardware Flashing

If it is protected, follow the instructions below.

The basic [guide](../install/simplified-hw-flashing.md) is not enough for this board,
read the [full guide](../install/spi.md) as well.


### Device Unique Properties

**Form factor**

-   SSI EEB 3.61 (12"x13")


**Flash chips**

These boards use LPC flash (not SPI), in a PLCC socket. The default
flash size 1MiB (8Mbits), and can be upgraded to 2MiB (16Mbits).
SST49LF080A is the default that the board uses. SST49LF016C is an
example of a 2MiB (16Mbits) chip, which might work. It is believed that
2MiB (16Mbits) is the maximum size available for the flash chip.

*DO NOT hot-swap the chip with your bare hands. Use a PLCC chip
extractor. These can be found online. See
<http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>*


**Hex-core CPUs support**

PCB revision 1.05G is the latest version of this board and the best one
(the revision number is be printed on the board), if you want to use
dual hex-core CPUs (Opteron 2400/8400 series), though only two board
configurations are believed to support them. Other revisions are
believed to only support dual quad-core CPUs.

To be sure your board supports a CPU check the official ASUS website here:
<https://www.asus.com/support/cpu_support>. Note: not all CPUs are listed.

If you are running a Hex-Core CPU on any board version, please contact us.


**Different Board configurations**

There are 7 different configurations of this board: "standard", 2S, iKVM,
iKVM/IST, SAS, SAS/iKVM and SAS/iKVM/IST.

The 2S boards have two PCI-E slots with the numbers of lanes shared,
making each slot have 8 lanes.

The iKVM boards are so called because they offer a remote real-time access
to the machine through a removable PCI management card, their hardware is
the same as the non-iKVM ones.

The SAS versions have a 4-port SAS controller and a four 7-pin SAS connectors
instead of the PCI-E 8x slot which is present in all the other board configurations.
Note: the SAS functionality is **not supported** by libreboot.

The IST versions with PCB revision 1.05G are the ones who are believed to
support the six core Opteron Istanbul processors (2400 and 8400 series).


**Issues/Untested**

-   There seems to be a 30 second bootblock delay (observed by
    tpearson); the system otherwise boots and works as expected. See
    [text/kfsn4-dre/bootlog.txt](text/kfsn4-dre/bootlog.txt) - this uses
    the 'simple' bootblock, while tpearson uses the 'normal'
    bootblock, which tpearson suspects may be a possible cause. This
    person says that they will look into it. [This
    config](http://review.coreboot.org/gitweb?p=board-status.git;a=blob;f=asus/kfsn4-dre/4.0-10101-g039edeb/2015-06-27T03:59:16Z/config.txt;h=4742905c185a93fbda8eb14322dd82c70641aef0;hb=055f5df4e000a97453dfad6c91c2d06ea22b8545)
    doesn't have the issue.

-   Text-mode is jittery and it may not be usable, so it's recommended
    to flash the BIOS with the coreboot frame-buffer image (kfsn4-dre_corebootfb.rom).
    The jitter disappears if using KMS once the kernel starts, but it will 
    remain, if booting the kernel in text-mode.

-   Booting from USB mass storage devices is not possible; neither GRUB
    nor SeaBIOS detect USB drives when present. USB keyboards function
    under both GRUB and SeaBIOS, albeit slowly under GRUB (several seconds per
    character typed).

-   To install an operating system you will need a hard disk
    with a pre-installed OS otherwise you have to plug in another hard disk or
    a CD/DVD reader in order to boot a copy of the installer of your OS, since
    the USB booting doesn't work.


### Expanded_Specifications

<div class="expandedspecs">

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | ASUS                                           |
| **Name**                   | [KFSN4-DRE](https://web.archive.org/web/20181212180051/http://ftp.tekwind.co.jp/pub/asustw/mb/Socket%20F/KFSN4-DRE/Manual/e3335_kfsn4-dre.pdf)                                      |
| **Released**               | 2007                                           |
| **Chipset**                | [nVIDIA nForce Pro 2200](https://en.wikipedia.org/wiki/Comparison_of_Nvidia_nForce_chipsets#nForce4) |
| **CPU**                    | 2x Socket F; [Opteron 2300/8300](https://en.wikipedia.org/wiki/List_of_AMD_Opteron_processors#8300_series_%22Shanghai%22) |
| **Memory (max/cpu)**       | 32GB DDR2 SODIMMs (8x4 GB) ECC                  |
| **Graphics**               | [XGI Z9s](https://www.vgamuseum.info/index.php/cpu/item/1124-xgi-volari-z9s) |
| **Networking**             | 2x Broadcom BCM5721                             |
| **Expansion Slots**        | PCI-E 1a x16: 1x full (DRE) or 2x half wired in x8 (DRE/2s) or 1x half wired in x8 (DRE/SAS) |
| **Storage connectors**     | 4x SATA II, 2x SAS (DRE/SAS only)               |
| **Other Connectors**       | Headers: SCOM, BMC, dual USB 2.0, 2x SMBus, PS/2 |
| **I/O Ports on Back**      | 2x PS/2, 2x USB 2.0, SCOM, VGA, 2x RJ-45 1GbE (Ethernets) |

</div>


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/kfsn4-dre.html>
