---
title: Install Libreboot on Dell OptiPlex 780 MT/USFF
x-toc-enable: true
...


**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**


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
| **Architecture**           | x86\_64                                        |
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

| ***Payloads supported***   |           |
|----------------------------|-----------|
| **GRUB (libgfxinit only)** | Works     |
| **SeaBIOS**                | Works     |
| **SeaBIOS with GRUB**      | Works     |
</div>


### Get the ROM image

The `_truncate` image is needed if you're flashing Libreboot internally from
the original Dell firmware. Otherwise, you only need the regular images that
lack `_truncate` in the file name.


**[Download](https://libreboot.org/download.html) from release**


**[Build](https://libreboot.org/docs/build/) from source**

The build target, when building from source, is thus:

```
./mk -b coreboot dell_optiplex_780_mt_truncate_8mb_uefi
./mk -b coreboot dell_optiplex_780_mt_8mb_uefi
./mk -b coreboot dell_optiplex_780_usff_truncate_8mb_uefi
./mk -b coreboot dell_optiplex_780_usff_8mb_uefi
```

### Flash a ROM image

**Software**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


Backup the previous (*working*) firmware (atleast) twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p internal -r Backup1.bin

	flashprog -p internal -r Backup2.bin

Check and compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.


Short the service jumper. It's a
2-pin header located somewhere on the board (no photos available yet).

With the service jumper set, all flash protections are disabled on the first
6MB of the flash; the latter 2MB part is restricted. Because we are using an
Intel Flash Descriptor on this board, the flash will be divided into these
parts:

* Intel Flash Descriptor (basic machine configuration)
* Intel Gigabit Ethernet region (e.g. MAC address configuration)
* BIOS region (contains coreboot)

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

If you already have Libreboot running, and you don't have flash protection
enabled, you can omit `--ifd -i REGION` from the flashprog command, and flash
the full image; the one without `_truncate` in the file name uses all of the
flash, with the BIOS region ending at the 8MB limit, so the BIOS region is
therefore 2MB larger on those images.


**Hardware**

For general information, please refer to [25xx NOR flash
instructions](../install/spi.md) - that page refers to use of socketed flash.


Backup the previous (*working*) firmware twice (to check and compare if reading it gone wrong and backups differs.)

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup1.bin

	flashprog -p serprog:dev=/dev/ttyACM0,spispeed=16M -r Backup2.bin

Check and compare those if reading from the flash were successful:

	diff Backup1.bin Backup2.bin

For more than 2:

	shasum Backup1.bin Backup2.bin Backup3.bin

If those differs, something gone wrong and proceeding further may **(DON'T) leave your device unrecoverable** (without a reliable backup) if flashing fails as well.


No photos yet available.


### Set MAC address

This platform uses an Intel Flash Descriptor, and defines an Intel GbE NVM
region. As such, release/build ROMs will contain the same MAC address. To
change the MAC address, please read [nvmutil documentation](../install/nvmutil.md).


### Custom Mods

Might apply for the 780 motherboards too.

[Guide for mods](https://libreboot.org/extras.md#dell-7010-9010-t1650-7020-9020-xe2-t1700-probably-other-dell-ones-too)


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
| **Graphics**               | [GMA 4500](https://www.techpowerup.com/gpu-specs/gma-x4500.c3116)                                       |
| **Audio**                  | Analog Devices ADI 1984A                       |
| **Memory**                 | DDR3 SODIMMs (max 16 GB, 4x4 GB)               |
| **Networking**             | Intel 82567LM                                  |
| **Expansion Slots**        | PCIe 2.0; x16, x1, PCI                       |
| **Storage connectors**     | 4x SATA II                                     |
| **Other Connectors**       | Headers; audio, dual USB 2.0                  |
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
