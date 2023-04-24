---
title: HP Elite 8200 SFF and 6200 Pro Business
x-toc-enable: true
...

<div class="specs">
<center>
<img tabindex=1 alt="HP Compaq 8200 Elite SFF" class="p" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>
</center>

| ***Specifications***      |                                             |
|---------------------------|---------------------------------------------|
| **Manufacturer**          | HP                                          |
| **Name**                  | Compaq 8200 Elite SFF                       |
| **Released**              | 2011                                        |
| **Chipset**               | Intel Q67                                   |
| **CPU**                   | Intel Sandy/Ivy Bridge                      |
| **Graphics**              | Intel HD Graphics or PCI-e low profile card |
| **Memory**                | Up to 32GB (4x8GB)                          |
| **Architecture**          | x86_64                                      |
| **Intel ME/AMD PSP**      | Present, neutered                           |
| **Flash chip**            | SOIC-8 8MiB                                 |


| ***Payloads supported***  |       |
|---------------------------|-------|
| **GRUB**                  | Works |
| **SeaBIOS**               | Works |
| **SeaBIOS with GRUB**     | Works |
</div>

Introduction
============

Libreboot has support for this, in the Git repository and release versions
from 20230423 onwards.

Brief board info
----------------

HP Elite 8200 SFF is a small-form-factor desktop of Intel Sandybridge platform
which you can read more about here:

<https://support.hp.com/gb-en/product/hp-compaq-8200-elite-small-form-factor-pc/5037931>

Here's the [Technical Reference Manual](https://web.archive.org/web/20160109143257/https://h10032.www1.hp.com/ctg/Manual/c02778024.pdf)

Installation of Libreboot
-------------------------

TODO: this patch is now in coreboot gerrit:\
<https://review.coreboot.org/c/coreboot/+/74248>\
(more notes about internal flashing)\
also:\
<https://www.reddit.com/r/coreboot/comments/12nux51/hp_elite_8200_sff_coreboot_port_current_state/> - see notes about 8300

You can actually just compile the Libreboot ROM for this, and flash the
entire ROM, then flash it. The *coreboot* project proper, has information
about this:

<https://doc.coreboot.org/mainboard/hp/compaq_8200_sff.html>

Refer to the coreboot guide for flashing instructions, and you can build the
images for it in Libreboot like so:

	./build boot roms hp8200sff_8mb

More information about building ROM images can be found in
the [build guide](../build/).

If you plan on using a graphics card (other than the integrated graphics of
your CPU), choose one of the files which name contains both "seabios" and
"txtmode".

This is a *Sandybridge* board which means that a neutered ME image is required
if you wish to flash the ME region. Libreboot's build system automatically
downloads, neuters (using `me_cleaner`) and inserts this if compiling from
source.

If you're using *Libreboot release* ROM images, the ME image has been scrubbed
and you must re-insert it. Use the information on this guide to know how
to do that:

[Insert binary blobs on Intel Sandybridge/Ivybridge/Haswell
platforms](../install/ivy_has_common.md)

You may also wish to change the *default MAC address* if you're planning to
use the onboard Intel Gigabit Ethernet. You can do this using the information
in the same guide linked above, or read the nvmutil manual:

[Modify MAC addresses with nvmutil](../install/nvmutil.md).

HP 6200 Pro Business PC
-----------------------

According to this page from the vendor, HP BIOS updates are the same on both
the 8200 SFF Elite *and* 6200 Pro Business desktop PCs; therefore, we believe
that the Libreboot config for 8200 SFF will *also* work on 6200 Pro Business
PCs. That page is here:
<https://support.hp.com/fi-fi/drivers/selfservice/swdetails/hp-compaq-8200-elite-small-form-factor-pc/5037931/swItemId/vc-229778-2>

The config for this board is courtesy of *Riku Viitanen* (`Riku_V` on Libreboot
IRC), who tested and confirmed the following functionality:

* Sandy Bridge (i5-2400) and Ivy Bridge (i5-3570S) CPUs
* 4x8 GB RAM (Sandy Bridge: 1333MHz, Ivy Bridge: 1600MHz)
* PS/2 and USB keyboards
* Boot from USB and DVD
* Gigabit ethernet
* VGA and DisplayPort (Intel graphics), with libgfxinit (native video init)
* Headphone output, PC speaker
* S3 suspend, wake on USB keyboard
* lm\_sensors outputs CPU core temperatures only
* Both PCIe x16 slots, external GPU works with SeaBIOS
* PCI
* SATA

At the time of adding this board to Libreboot, the following is untested:

* Serial port and parallel port (internal header on the board) and PS/2 mouse-
  NOTE: Serial port believed to work, according to initial coreboot commit from
  revision `6308e0e92f624cb18a875ed04e41e1d15fc91054` in 2018
* Wake on LAN
* Internal flashing from OEM BIOS - TODO: Riku spoke to someone on IRC who said
  it might be possible, so this should be investigated. - NOTE: coreboot git
  logs also suggest that this is possible.
* Does the OEM BIOS support Ivy Bridge chips? People on various forums say no,
  but of them say the chipset is the reason. No one seems to have actually
  tested, or at least haven't told about it.
* Floppy drive. The case has a spot for it, but I can't find the header (P10).

According to the initial coreboot port from 2018, the following also works:

* EHCI debug (not enabled by Libreboot configs)
* USB ports
* Native (libre) raminit with up to four DIMM modules (also tested by Riku and
  confirmed working, with 32GB RAM installed as 4x8GB)

TPM
---

According to git logs, TPM should work, and a commit from 2018 at revision
ID `39d0e2a2cf45e28cdddd0fe0c88f94ce527ab1ef` in coreboot makes the TPM visible
to operating systems.

PSU Fan control
---------------

See coreboot commit `9bd601584350f51f112b15a7369f9aa82f1d0919` - labelled
by commit message `superio/nuvoton/npcd378: Add PSU fan control`.

Per this commit, SuperIO-based fan control is supported on HP Elite 8200 SFF.

TODO for testing the above is here:\
<https://codeberg.org/libreboot/lbmk/issues/9>

This is controlled via `nvramtool` to modify the value in sram. See:

* `psu_fan_lvl=3` <-- default setting in coreboot, and Libreboot.

Other values possible: from reading the source code, it is implied that the
number can be between 0 and 7. If the value is set higher than 7, it will
default back to 3.

Libreboot locks CMOS/NVRAM settings, but you can change the default setting in
the *ROM* by using the `-C` option in nvramtool. You can find this under the
directory `coreboot/default/util/nvramtool` when downloading coreboot inside
of lbmk by running the command:

	./download coreboot default

Go in there and type `make` to build nvramtool. Simply run nvramtool without
arguments, and it will show a list of options.
