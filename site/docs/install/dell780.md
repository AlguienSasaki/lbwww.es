---
title: Install Libreboot on Dell OptiPlex 780 MT/USFF
x-toc-enable: true
...


<div class="specs">
<center>
Dell OptiPlex 780
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Dell                                           |
| **Name**                   | OptiPlex 780                                   |
| **Variants**               | OptiPlex 780 MT, USFF                          |
| **Released**               | 2009                                           |
| **Chipset**                | Intel Q45                                      |
| **CPU**                    | LGA 775, Intel Core 2 Duo+Quad on MT           |
| **Graphics**               | GMA 4500                                       |
| **Memory**                 | 16/8GB DDR3 SODIMMs (4x4GB,2x4GB USSF)         |
| **Architecture**           | x86-64-v1                                      |
| **Original boot firmware** | Dell UEFI firmware                             |
| **Intel ME/AMD PSP**       | Present, Fully removable firmware.             |
| **Flash chip**             | SOIC-16 or SOIC-8 8MiB/64Mbit                  |


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
| **Display (discrete GPU, SeaBIOS payload only)**  | W* |
| **Audio**                                         | W+ |
| **RAM Init**                                      | W+ |

</div>


### Get the ROM image

The `_truncate` image is needed if you're flashing Libreboot internally from
the original Dell firmware. Otherwise, you only need the regular images that
lack `_truncate` in the file name.


**[Download](../../download.html) from release**


**[Build](../build/) from source**

The build target, when building from source, is thus:

```
./mk -b coreboot dell_optiplex_780_mt_truncate_8mb
./mk -b coreboot dell_optiplex_780_mt_8mb
./mk -b coreboot dell_optiplex_780_usff_truncate_8mb
./mk -b coreboot dell_optiplex_780_usff_8mb
```

### Internal/Software Flashing

Short the service jumper
(it's a 2-pin header located somewhere on the board (no photos available yet),
to disable flash protections on the first 6MB of the flash; the latter 2MB part is restricted. 
Because we are using an Intel Flash Descriptor on this board, 
the flash will be divided into these parts:

* Intel Flash Descriptor (basic machine configuration)
* Intel Gigabit Ethernet region (e.g. MAC address configuration)
* BIOS region (contains coreboot)

**After the jumper set, [Backup](../install/simplified-sw-flashing.md) the vendor firmware before,
but DO NOT flash it directly like at the end of the referred guide.**

Before you can use the flash commands, please note that the `_truncate` images
are 6MB in size, but they must be padded to 8MB. Normally a blank part of flash
should ideally be all 0xFF, but here we do:

	dd if=/dev/zero of=2mb.bin bs=2M count=1
	cat libreboot6mb.rom 2mb.bin > libreboot.rom

If you're not using the truncate images, you can ignore the above steps.

The Flash Descriptor can be thought of more like a *partition table* for the
flash, at least in our use-case scenario. The `_truncate` images configure
the flash such that only the first 6MB is used, with the BIOS region ending
at that limit; the upper 2MB is specified as the platform data region, in
Libreboot's configuration. To flash it, do this:

	./flashprog -p internal --ifd -i fd -w libreboot.rom
	./flashprog -p internal --ifd -i gbe -w libreboot.rom
	./flashprog -p internal --ifd -i bios -w libreboot.rom

It's very important to do this in the *exact* order as above, because
the `--ifd` option references the *flashed* IFD, not the one in your image,
so you must flash the IFD first.

**Then skip the backup sections and [flash](../install/simplified-sw-flashing.md) the complete libreboot.rom.**

If you already have Libreboot running, and you don't have flash protection
enabled, you can omit `--ifd -i REGION` from the flashprog command, and flash
the full image; the one without `_truncate` in the file name uses all of the
flash, with the BIOS region ending at the 8MB limit, so the BIOS region is
therefore 2MB larger on those images.


### External/Hardware Flashing

No photos yet available.

**REMOVE all power sources and connectors from the machine, before doing this.
This is to prevent short circuiting and power surges while flashing.**

**Check the [hw-flashing guide](../install/simplified-hw-flashing.md),
before moving onto the next step below.**

Flash it: (With rpi-pico as an example, edit the line if needed.)

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -w filename.rom


### Set MAC address

This platform uses an Intel Flash Descriptor, and defines an Intel GbE NVM
region. As such, release/build ROMs will contain the same MAC address. To
change the MAC address, please read [nvmutil documentation](../install/nvmutil.md).


### Custom Mods

Might apply for the 780 motherboards too.

[Guide for mods](../../extras.md#dell-7010-9010-t1650-7020-9020-xe2-t1700-probably-other-dell-ones-too)


### Expanded_Specifications

<div class="expandedspecs">
<center>
<img tabindex=1 alt="dell780mt" src="https://av.libreboot.org/*devicename/io+exp.jpg*" />
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Dell                                           |
| **Name/Variant**           | OptiPlex 780 MT                                |
| **Released**               | 2009                                           |
| **Chipset**                | Intel [Q45](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#Core_2_chipsets) |
| **CPU**                    | LGA 775, Intel [Core 2](https://en.wikipedia.org/wiki/List_of_Intel_processors#Intel_Core_2) |
| **Graphics**               | [GMA 4500](https://www.techpowerup.com/gpu-specs/gma-x4500.c3116)    |
| **Audio**                  | Analog Devices ADI 1984A                       |
| **Memory**                 | DDR3 SODIMMs (max 16 GB, 4x4 GB)               |
| **Networking**             | Intel 82567LM                                  |
| **Expansion Slots**        | PCIe 2.0; x16, x1, PCI                         |
| **Storage connectors**     | 4x SATA II                                     |
| **Other Connectors**       | Headers; audio, dual USB 2.0                   |
| **I/O Ports on Front**     | 2x USB 2.0 , 2x Line-in (stereo/microphone)    |
| **I/O Ports on Back**      | 2x Line-out (headphone/speaker) , 6x USB 2.0 , RJ-45 1GbE (Ethernet) , SCOM , VGA , Parallel , eSATA  |

</div>

<div class="expandedspecs2">
<center>
<img tabindex=1 alt="dell780ussf" src="https://av.libreboot.org/*devicename/io+exp.jpg*" />
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Dell                                           |
| **Name/Variant**           | OptiPlex 780 USSF                              |
| **Released**               | 2009                                           |
| **Chipset**                | Intel [Q45](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#Core_2_chipsets) |
| **CPU**                    | LGA 775, Intel [Core 2+Quad](https://en.wikipedia.org/wiki/List_of_Intel_processors#Intel_Core_2) |
| **Graphics**               | [GMA 4500](https://www.techpowerup.com/gpu-specs/gma-x4500.c3116)                                       |
| **Audio**                  | Analog Devices ADI 1984A                       |
| **Memory**                 | DDR3 SODIMMs (max 16 GB, 4x4 GB)               |
| **Networking**             | Intel 82567LM                                  |
| **Expansion Slots**        | PCIe 2.0 Minicard                              |
| **Storage connectors**     | 2x SATA II                                     |
| **Other Connectors**       | Frontpanel header; audio, dual USB 2.0                  |
| **I/O Ports on Front**     | 2x USB 2.0, 2x Line-in (stereo/microphone) |
| **I/O Ports on Back**      | 2x Line-out (headphone/speaker), 5x USB 2.0, RJ-45 1GbE (Ethernet), SCOM ,VGA ,DisplayPort, eSATA  |

</div>

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/dell780.html>
