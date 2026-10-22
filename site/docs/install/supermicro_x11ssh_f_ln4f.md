---
title: Install Libreboot on Supermicro X11SSH-(LN4)F
x-toc-enable: true
...

**[!!BACKUP everything!!](https://libreboot.org/docs/install/#howto-readwriteerase-the-boot-flash) just in case something fails.**

<div class="specs">
<center>
<img tabindex=1 alt="Supermicro X11SSH-(LN4)F" class="p" src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f.jpg" /><span class="f"><img src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f.jpg" /></span>
</center>

| ***Specifications***       |                                                |
|----------------------------|------------------------------------------------|
| **Manufacturer**           | Supermicro                                     |
| **Name**                   | X11SSH-(LN4)F                                  |
| **Variants**               | X11SSH-F / X11SSH-LN4F                         |
| **Released**               | 2017                                           |
| **Chipset**                | Intel C236                                     |
| **CPU**                    | LGA 1151 - Intel 6-9th gen. 80W TDP            |
| **Graphics**               | iGPU on cpu, but no display output             |
| **Memory**                 | 64GB DDR4 SODIMMs (4x16 GB), ECC               |
| **Architecture**           | x86_64                                         |
| **Original boot firmware** | Supermicro                                     |
| **Intel ME/AMD PSP**       | SPS->ME disabled with me_cleaner *1            |
| **Flash chip**             | SOIC-8 16MiB                                   |


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
| **Internal flashing with original boot firmware** | N |
| **Display (if Intel GPU)**                        | N |
| **Audio**                                         | N |
| **RAM Init**                                      | W* |

| ***Payloads supported***   |           |
|----------------------------|-----------|
| **GRUB (libgfxinit only)** | Works     |
| **SeaBIOS**                | Works     |
</div>

*1 The server Intel ME (SPS), which is the server version of the ME. One difference is that SPS does not support CPU replacement detection, so it always redoes DRAM training from scratch on every boot, which makes booting take longer. Libreboot replaces SPS with ME, which fixes this.

### Get the ROM image

**[Download](https://libreboot.org/download.html) from release**

If you're using a release ROM, please ensure that you've inserted extra firmware
required refer to the [guide](../install/ivy_has_common.md) for that (, failure
to adhere to this advice will result in a bricked machine):

- Intel ME (vendor blob)
- Intel FSP (vendor blob)


**[Build](https://libreboot.org/docs/build/) from source**

To build it from source for both X11SSH-F/LN4F:

	./mk -b coreboot supermicro_x11ssh_f_16mb_uefi


### Flash the ROM image

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


If you're already running Libreboot, and you don't have flash protection turned on, internal flashing is possible.


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


<img tabindex=1 alt="Supermicro X11SSh-(LN4)F flash chip position" class="p" src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f_soic8_1.jpg" /><span class="f"><img src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f_soic8_1.jpg" /></span>
<img tabindex=1 alt="Supermicro X11SSh-(LN4)F flash chip with programmer" class="p" src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f_soic8_2.jpg" /><span class="f"><img src="https://supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f_soic8_2.jpg" /></span>


That 16MB soic-8 flash chip is below the battery, highlighted, usually covered with a sticker and sometimes socketed,
The other one is a 32MB soic_16 BMC flash chip next to the previous one.


### 6-8 core processor support

- A pin mod is needed.

[Guide for mod](https://libreboot.org/extras.md#reversible-intel-8th-9th-gen-processors-support-on-older-6th-7th-gen-motherboards)


### Replacable Baseboard Management Controller firmware

The Aspeed AST2400 BMC vendor firmware can be replaced with [OpenBMC](https://www.openbmc.org/)

Incomplete, does not boot the system and not yet used with libreboot. [See](https://codeberg.org/libreboot/lbwww/pulls/194#issuecomment-20790506)

To disable it instead, shorting it's jumper pins would work.

Not yet used/tested with libreboot.


### Display output, graphics acceleration and gpu offload

On the motherboard the BMC is used for display output. You just won't have any hardware graphics acceleration whatsoever and will be limited to 1920x1200.

However the igpu on the processor could be used to offload lighter softwares.
For desktop environment (DE) using a dedicated gpu is advised (for display output too).

With switcheroo_control; the DE, image/video encode/decode and browser ones can be used on the integrated one, while using the more powerful one for editing,gaming,computing and/or whatever needs more horse power. (Of course, it is selectable which gpu shall be used for any program.)

(Provided the iGPU is not disabled if the dGPU added or using 2 dGPUs.)

[Guide for dual-gpu setup](https://libreboot.org/extras.html#reversible-dual-gpu-setup-do-not-use-it-on-processorsmotherboards-without-integrated-graphics.)


### Not working

- S3 resume not working (vendor and coreboot)
- [No iGPU output via vga port](https://codeberg.org/libreboot/lbwww/pulls/194#issuecomment-20790470) (Wired directly to BMC and it is used for output without graphics acceleration.)



### Expanded_Specifications

<div class="expandedspecs">
<center>
<img tabindex=1 alt="Supermicro X11SSH-(LN4)F" class="p" src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f.jpg" /><span class="f"><img src="https://av.libreboot.org/supermicro_x11ssh_f_ln4f/supermicro_x11ssh_f_ln4f.jpg" /></span>
</center>

| ***Specifications***         |                                                |
|------------------------------|------------------------------------------------|
| **Manufacturer**             | Supermicro                                          |
| **Name/Variant**             | X11SSH-F / X11SSH-LN4F                               |
| **Released**                 | 2017                                           |
| **Chipset**                  | Intel [C236](https://en.wikipedia.org/wiki/List_of_Intel_Xeon_chipsets#Skylake-based_Xeon_chipsets)   |
| **CPU with venfor fw.**      | LGA 1151; Intel 6th gen. [Core](https://en.wikipedia.org/wiki/Skylake_(microarchitecture)#Mainstream_desktop_processors) + [Xeon E3-12xx v5](https://en.wikipedia.org/wiki/List_of_Intel_Xeon_processors_(Skylake-based)#Xeon_E3-12xx_v5_(uniprocessor)) & 7th gen. [Core](https://en.wikipedia.org/wiki/Kaby_Lake#Desktop_processors) + [Xeon E3-12xx v6](https://en.wikipedia.org/wiki/Kaby_Lake#Server/workstation_Xeon_processors) processors, up to 80W TDP
| **CPU with LibreBoot fw.**   | LGA 1151; vendor fw. supported cpus included and Intel 8th gen. [Core with B0 stepping; i3, Pentium Gold and Celeron](https://en.wikipedia.org/wiki/Coffee_Lake#List_of_8th_generation_Coffee_Lake_processors) + 9th gen. [Core with B0 stepping; i3, Pentium Gold, Celeron](https://en.wikipedia.org/wiki/Coffee_Lake#List_of_9th_generation_Coffee_Lake_processors_(Coffee_Lake_Refresh)) processors, up to 80W TDP (8th & 9th gen. desktop processors support ecc memories.)|
| **Graphics**                 | iGPU on cpu, but no display output |
| **Audio**                    | None | 
| **Memory**                   | DDR4 SODIMMs (max 64 GB, 4x16 GB)  |
| **Networking**               | Intel I210-AT; 2 for X11SSH-F, 4 for X11SSH-LN4F               |
| **Expansion Slots**          | 4 PCIe 3.0; x16 wired in x8, x8, x8 wired in x4 and M.2 key M in x2 with 2280 size |
| **Storage connectors**       | 8x S-ATA III    |
| **Other connectors/headers** | USB 3.0 , Headers; TPM, dual USB 3.0 and 2x dual USB 2.0, [Custom SCOM](https://strugglers.net/~andy/mothballed-blog/2015/06/19/fun-with-supermicro-motherboard-serial-headers/).  |
| **I/O Ports on Back**        | SCOM (RS-232), 2x USB 2.0, 2x USB 3.0, 3x/5x RJ-45 1GbE (Ethernets); 1 wired to the BMC directly and 2 normal (+2 for X11SSH-LN4F)|
| **Super I/O or BMC**         | ASPEED AST2400 |

</div>

Most detailed [Here](https://doc.coreboot.org/mainboard/supermicro/x11-lga1151-series/x11ssh-f/x11ssh-f.html#technology).



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
