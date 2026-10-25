---
title: Install Libreboot HP Compaq Elite 8300 USDT
x-toc-enable: true
...


<div class="specs">
<center>
<img tabindex=1 alt="HP Compaq Elite 8300 USDT" class="p" src="https://av.libreboot.org/hp8300usdt/hp8300usdt.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8300usdt/hp8300usdt.jpg" /></span>
</center>

| ***Specifications***      |                                             |
|---------------------------|---------------------------------------------|
| **Manufacturer**          | HP                                          |
| **Name**                  | Compaq 8300 Elite USDT                      |
| **Released**              | 2012                                        |
| **Chipset**               | Intel Q77                                   |
| **CPU**                   | LGA 1155, Intel Sandy/Ivy Bridge            |
| **Graphics**              | Intel HD 3000/4000 + MXM gpu                |
| **Memory**                | 16GB DDR3L SODIMMs (2x8GB)                  |
| **Architecture**          | x86_64                                      |
| **Intel ME/AMD PSP**      | Present. Disabled with me_cleaner.          |
| **Flash chip**            | SOIC-16 16MiB                               |


### Get the ROM image

**[Download](../../download.html) from release**

If you're using a release ROM, please ensure that you've inserted extra firmware
required refer to the [guide](../install/ivy_has_common.md) for that (, failure
to adhere to this advice will result in a bricked machine)

- Intel ME (vendor blob)


**[Build](../build/) from source**

	./mk -b coreboot hp_elite_8300_usdt_16mb


### Internal/Software Flashing

Internal flashing is possible. OEM BIOS versions 2.87, 2.90, 2.99, and 3.08 are confirmed
compatible with this guide. BIOS 2.05 is confirmed **not** to work.

If you have some other BIOS version, please do tell about it on
[IRC](../../contact.md) or comment on this
[issue](https://codeberg.org/libreboot/lbwww/issues/73),
so this guide can be updated.

The jumper labelled "FDO" (for Flash Descriptor Override) needs to be shorted.
That removes all write protections on this board.

We can borrow a shunt from another header on the board: PSWD. It is right
next to the SO-DIMM RAM slots. Move it to the FDO header between the quartz
crystal (small metal cylinder) and the power cable for the optical drive.

![](https://av.libreboot.org/hp8300usdt/jumper_to_fdo.jpg)

The BIOS should no longer impose any write-protections.

**After the jumper set, follow the [guide](../install/simplified-sw-flashing.md) to backup and flash.**


### External/Hardware Flashing


Unbricking is possible by external flashing. You first need to remove
the optical disk drive and 2.5" HDD/SSD and the metal bracket that
supports them. This requires you to open one torx (T15) screw in total.

The SOIC-16 flash chip is located on the edge of the board
near the group of yellow cubes. Follow the
[general SPI flashing guide](../install/spi.html).

![](https://av.libreboot.org/hp8300usdt/chip+header.jpg)



If you don't have a suitable clip, you can also use the ROM_RCVRY header
right next to the flash chip. By default only the footprint is present,
so you have to solder a pin header of your own. End result can be seen
and the pinout can be seen in the photo earlier. Consult the HP service
manual (page 241) on how to remove the motherboard from the chassis.

<http://web.archive.org/web/20210305234331/https://h10032.www1.hp.com/ctg/Manual/c03612798.pdf>

If you do this, you have to reapply thermal paste.
That might be a good idea anyway, considering how old these are getting

**You might need to power the board by plugging it in. In that case,
do not connect the Vcc (3v3) pin of the flash chip.
Also make sure the board doesn't fully power on (that is, boot).**

**Check the [hw-flashing guide](../install/simplified-hw-flashing.md) before moving onto the next step below.**

Flash it: (With rpi-pico as an example, edit the line if needed.)

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -w filename.rom
    

### Set MAC address

This platform uses an Intel Flash Descriptor, and defines an Intel GbE NVM
region. As such, release/build ROMs will contain the same MAC address. To
change the MAC address, please read [nvmutil documentation](../install/nvmutil.md).


### Upgradable (socketed) GPU

So far, only Quadro 1000M and 2000M have been tested with Libreboot (hp8560w). [Guide](../../extras.md#reversible-upgradabale-socketed-gpus-in-older-laptops)

If a MXM gpu is installed, all three monitor ports should be active. The igpu will use the top DisplayPort and the MXM gpu will
operate the bottom DisplayPort & VGA. (Untested)

It should be possible to use both gpus in a [dual-gpu setup](../../extras.md#reversible-dual-gpu-setup-do-not-use-it-on-processorsmotherboards-without-integrated-graphics.).


### Expanded_Specifications

<div class="expandedspecs">

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | HP                                          |
| **Name/Variant**           | Compaq 8300 Elite USDT                                |
| **Released**               | 2012                                           |
| **Chipset**                | Intel [Q77](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#LGA_1155)                                     |
| **CPU**                    | LGA 1155, Intel Ivy Bridge [Core](https://en.wikipedia.org/wiki/Ivy_Bridge_(microarchitecture)#Desktop_processors)+[Xeon E3 v2](https://en.wikipedia.org/wiki/Ivy_Bridge_(microarchitecture)#Server_processors) & Sandy Bridge [Core](https://en.wikipedia.org/wiki/Sandy_Bridge#Desktop_platform)+[Xeon E3 v1](https://en.wikipedia.org/wiki/Sandy_Bridge#Server_platform) |
| **Memory**                 | DDR3L SODIMMs (max 15GB, 2x8GB)   |
| **Graphics**               | Intel [HD 4000](https://www.techpowerup.com/gpu-specs/hd-graphics-4000.c1282) (Ivy Bridge) & [HD 3000 1.35Ghz i7+Xeon E3](https://www.techpowerup.com/gpu-specs/hd-graphics-3000.c3196)/[HD 3000 1.1Ghz Core i5 and lower](https://www.techpowerup.com/gpu-specs/hd-graphics-3000.c1257) (Sandy Bridge) |
| **Audio**                  | Realtek ALC221 HD |
| **Networking**             | Intel 82579LM                                         |
| **Expansion Slots**        | PCIe 3.0; x16 in MXM, mPCIe  |
| **Storage connectors**     | SATA III, mSATA II     |
| **I/O Ports on Front**     | 4x USB 2.0, 2x Jack (headphone+microphone/stereo) |
| **I/O Ports on Back**      | 2x jack (in/out), 2x DisplayPort, PS/2 Mouse+Keyboard, 4x USB 3.0, 2x USB 2.0, RJ-45 1GbE (Ethernet), VGA  |

</div>

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/hp8300usdt.html>
