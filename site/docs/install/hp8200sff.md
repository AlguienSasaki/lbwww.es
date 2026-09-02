---
title: Install Libreboot on HP Compaq Elite 8200 SFF/MT & 6200 Pro
x-toc-enable: true
...


**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**


<div class="specs">
<center>
<img tabindex=1 alt="HP Compaq 8200 Elite SFF" class="p" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span> <img tabindex=1 title="From left to right: SFF and MT" class="p" src="https://av.libreboot.org/hp8200sff/sff+mt.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/sff+mt.jpg" /></span>
</center>

| ***Specifications***      |                                             |
|---------------------------|---------------------------------------------|
| **Manufacturer**          | HP                                          |
| **Name**                  | Compaq Elite 8200 & 6200 Pro MT/SFF         |
| **Released**              | 2011                                        |
| **Chipset**               | Intel Q67                                   |
| **CPU**                   | LGA 1155, Intel 2nd gen. (Core+Xeon e3 v1) and with Libreboot 3rd gen. (Core+Xeon e3 v2)                      |
| **Graphics**              | Intel HD 3000                               |
| **Memory**                | DDR3 SODIMMs (max. 32GB, 4x8GB)             |
| **Architecture**          | x86\_64                                     |
| **Intel ME/AMD PSP**      | Present. Can be disabled with me_cleaner.  |
| **Flash chip**            | SOIC-8 8MiB/64Mbit                          |

```
W+: Works without vendor firmware; 
N: Doesn't work; 
W*: Works with vendor firmware; 
U: Untested; 
P+: Partially works; 
P*: Partially works with vendor firmware
```

| ***Features***                                    |    |
|---------------------------------------------------|----|
| **Internal flashing with original boot firmware** | W* |
| **Display (Intel GPU)**                           | W+ |
| **Display (PCIe graphics card)**                  | W+ |
| **Audio**                                         | W+ |
| **RAM Init**                                      | W+ |

| ***Payloads supported***  |       |
|---------------------------|-------|
| **GRUB**                  | Works |
| **SeaBIOS**               | Works |
| **SeaBIOS with GRUB**     | Works |
</div>


*These 3 devices uses the same motherboard, regardles of the case sizes.*


### Get the ROM image

**[Download](https://libreboot.org/download.html) from release**

If you're using a release ROM, please ensure that you've inserted extra firmware
required refer to the [guide](../install/ivy_has_common.md) for that (, failure
to adhere to this advice will result in a bricked machine):

- Intel ME


**[Build](https://libreboot.org/docs/build/) from source**

Use the first one to upgrade from the vendor firmware (MT/SFF): 

	./mk -b coreboot hp8200sff_4mb

To update the Libreboot firmware on a librebooted one (MT/SFF):

	./mk -b coreboot hp8200sff_8mb


### Flash a ROM image

**Software**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)


Internal flashing from OEM BIOS is possible by setting a jumper
on the board.

The vendor BIOS imposes write-protections in the Flash Descriptor and
runtime. However, the flash descriptor can be bypassed by bridging a
jumper and the runtime protections only apply to a fixed address block.
Since neutering the Management Engine frees up a lot of space, we can
just install an intermediate Libreboot image there. This removes all
write-protections so has the same end result as external flashing:
a completely unlocked system.

Power off the computer. Remove the side panel.
Near the back USB ports find the jumper labelled **FDO**.

![Location of the FDO jumper](https://av.libreboot.org/hp8200sff/fdo.jpg)

You need to short the two pins circled. Use a
[jumper block](https://en.wikipedia.org/wiki/Jumper_(computing)) if you
have one but a screwdriver will do the job fine too. Hold the tip
between the pins until you can see the normal BIOS boot screen.

![](https://av.libreboot.org/hp8200sff/fdo\_screwdriver.jpg)


Now, run this command:

	flashprog -p internal

NOTE: If flashprog complains, detecting multiple chips, just pick one, and
specify it using the `-c` option as indicated by flashprog.

The output should contain the text "The Flash Descriptor Override
Pin-Strap is set". If it doesn't, start again from the beginning.

Now build the **4** MiB Libreboot image (MT/SFF).

	./mk -b coreboot hp8200sff_4mb

Also build `ifdtool`. It will be needed soon.

	cd src/coreboot/default/util/ifdtool
	make
	sudo make install

Now choose the image you want from `bin/hp8200sff_4mb`.
We'll refer to it as `libreboot4.rom`. We need to pad it to 8 MiB:

	dd if=/dev/zero bs=4M count=1 >> libreboot4.rom

Flash the Libreboot image with a tweaked layout:

	ifdtool libreboot4.rom -f layout
	flashprog -p internal -w libreboot4.rom -l layout -i fd -i gbe -i bios -i me

Power off the computer. Make sure to power off, rebooting is not enough!

Power on the computer.

If you plan on using a graphics card (other than the integrated graphics of
your CPU), choose one of the files which name contains both "seabios" and
"txtmode".

Now we can flash the full 8 MiB image. Boot to an OS with flashprog
again.

	flashprog -p internal -w libreboot8.rom

Power cycle the computer again.

**Hardware**

For general information, please refer to [25xx NOR flash
instructions](../install/spi.md) - that page refers to use of socketed flash.

[Instructions](https://doc.coreboot.org/mainboard/hp/compaq_8200_sff.html) (works for the MT too) if you need
to recover from an unbootable BIOS.


### Set MAC address

This platform uses an Intel Flash Descriptor, and defines an Intel GbE NVM
region. As such, release/build ROMs will contain the same MAC address. To
change the MAC address, please read [nvmutil documentation](/nvmutil.md).


### Expanded_Specifications

<div class="expandedspecs">

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | HP                                          |
| **Name/Variant**           | Compaq Elite 8200 & 6200 Pro MT/SFF                               |
| **Released**               | 2011                                           |
| **Chipset**                | Intel [Q67](https://en.wikipedia.org/wiki/List_of_Intel_chipsets#LGA_1155)                                     |
| **CPU with venfor fw.**    | LGA 1155; Intel Sandy Bridge [Core](https://en.wikipedia.org/wiki/Sandy_Bridge#Desktop_platform)+[Xeon E3 v1](https://en.wikipedia.org/wiki/Sandy_Bridge#Server_platform) |
| **CPU with LibreBoot fw.** | LGA 1151; vendor fw. supported cpus included and Intel Ivy Bridge [Core](https://en.wikipedia.org/wiki/Ivy_Bridge_(microarchitecture)#Desktop_processors)+[Xeon E3 v2](https://en.wikipedia.org/wiki/Ivy_Bridge_(microarchitecture)#Server_processors) |
| **Memory**                 | DDR3 SODIMMs (max 32GB, 4x8GB)   |
| **Graphics**               | Intel [HD 3000 1.35Ghz i7+Xeon E3](https://www.techpowerup.com/gpu-specs/hd-graphics-3000.c3196)/[HD 3000 1.1Ghz Core i5 and lower](https://www.techpowerup.com/gpu-specs/hd-graphics-3000.c1257) (Sandy Bridge) and with libreboot [HD 4000](https://www.techpowerup.com/gpu-specs/hd-graphics-4000.c1282) (Ivy Bridge) |
| **Audio**                  | Realtek ALC261 HD |
| **Networking**             | Intel 82579LM                                         |
| **Expansion Slots**        | PCIe; 2.0 x16, x16 wired in x4, x1, PCI |
| **Storage connectors**     | 2x SATA III, SATA II     |
| **Other Connectors**       | Headers; 2x dual usb 2.0  |
| **I/O Ports on Front**     | 2x Line-in (stereo/microphone), 4x USB 2.0 |
| **I/O Ports on Back**      | 2x Line-in (stereo/microphone), PS/2 Mouse+Keyboard, VGA, SCOM, DisplayPort, 6x USB 2.0, RJ-45 1GbE (Ethernet)  |

</div>

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/hp8200sff.html>
