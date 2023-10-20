---
title: Hardware compatibility list 
x-toc-enable: true
...

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../../news/safety.md),
OR YOU MIGHT BRICK YOUR MACHINE: [SAFETY PRECAUTIONS](../../news/safety.md)**

This sections relates to known hardware compatibility in libreboot.

For installation instructions, refer to [../install/](../install/).

NOTE: For T60/R60 thinkpads, make sure that it has an Intel GPU, not an ATI GPU
because coreboot lacks native video initialization for the ATI GPUs on these
machines.

(for later machines like T500, T400, ATI GPU doesn't matter, because it also
has an Intel GPU, and libreboot uses the Intel one)

READ THIS BEFORE UPDATING LIBREBOOT, OR YOU MIGHT BRICK YOUR MACHINE
====================================================================

**On newer Intel platforms that require Intel ME and/or MRC firmware, such as
ThinkPad X230 or T440p, and/or HP laptops that require KBC1126 EC firmware,
the release ROMs of Libreboot are MISSING certain files, that you must insert
yourself. FAILURE to adhere to this warning may result in you bricking your
machine (rendering it unbootable), if you were to flash the release ROMs without
modifying them in any way. For more information, please read:**

**[Insert vendor files on Sandybridge/Ivybridge/Haswell](../install/ivy_has_common.md)**

NOTE: This warning does not apply to ROMs that you compiled yourself, using
lbmk. It only applies to release ROMs, because ME/MRC/EC firmware is *deleted*
in release ROMs. The link above says how to re-add them. When building ROM images
yourself, from source, Libreboot's build system automatically handles it. See:
[Libreboot build instructions](../build/)

Supported hardware
==================

libreboot currently supports the following systems in this release:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](kgpe-d16.md)

### Desktops (AMD, Intel, x86)

-   [Acer G43T-AM3](acer_g43t-am3.md)
-   [Apple iMac 5,2](imac52.md)
-   [ASUS KCMA-D8 motherboard](kcma-d8.md)
-   [Dell Precision T1650](t1650.md) (**easy to flash without disassembly**)
-   [Gigabyte GA-G41M-ES2L motherboard](ga-g41m-es2l.md)
-   [HP Elite 8200 SFF/MT](hp8200sff.md) (HP 6200 Pro Business probably works too)
    (**easy to flash without disassembly**)
-   [HP Elite 8300 USDT](hp8300usdt.md) (**easy to flash without disassembly**)
-   [Intel D510MO and D410PT motherboards](d510mo.md)

### Laptops (Intel, x86)

-   [Apple MacBook1,1 and MacBook2,1](macbook21.md) (2,1 flashable without disassembly)
-   [Dell Latitude E6400, E6400 XFR and E6400 ATG, all with Nvidia or Intel
    GPU](e6400.md) **(easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   [Dell Latitude E6430 (Intel GPU](e6430.md) **(easy to flash, no disassembly)**
-   [HP EliteBook 2170p](hp2170p.md) (**socketed flash IC**)
-   [HP EliteBook 2560p](hp2560p.md)
-   [HP EliteBook 2570p](hp2570p.md)
-   [HP EliteBook 8470p](hp8470p.md)
-   [HP EliteBook Folio 9470m](hp9470m.md)
-   [Lenovo ThinkPad R400](r400.md)
-   [Lenovo ThinkPad R500](r500.md)
-   [Lenovo ThinkPad T400 / T400S](t400.md)
-   [Lenovo Thinkpad T420](../install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T420S](../install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T430](../install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T440p](../install/t440p_external.md)
-   [Lenovo ThinkPad T500](t500.md)
-   [Lenovo ThinkPad T530 / W530](../install/ivy_has_common.md) (no install
-   Lenovo ThinkPad T60 (with Intel GPU) (**easy to flash without disassembly**)
-   [Lenovo ThinkPad W500](t500.md)
-   [Lenovo ThinkPad W541](../install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md)
-   [Lenovo Thinkpad X220](../install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../install/ivy_has_common.md)
-   Lenovo ThinkPad X230
-   [Lenovo Thinkpad X230](../install/x230_external.md)
-   [Lenovo Thinkpad X230t](../install/x230_external.md)
-   Lenovo ThinkPad X301
-   Lenovo ThinkPad X60 / X60S / X60 Tablet (**easy to flash without disassembly**)

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../install/chromebooks.md)

## Removed boards

These boards were in Libreboot, but have been removed with the intention of
re-adding them at a later date. They were removed due to issues. List:

-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](../install/chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](../install/chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](../install/c201.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](../install/chromebooks.md)
-   [Hisense Chromebook C11 and more (veyron-jerry)](../install/chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](../install/chromebooks.md)
-   [HP Chromebook 14 G3 (nyan-blaze)](../install/chromebooks.md)
-   [Intel D945GCLF](d945gclf.md) (removed from lbmk, TODO: re-add support)
-   [Samsung Chromebook 2 11" (peach-pit)](../install/chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](../install/chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](../install/chromebooks.md)

### NOTES about removed boards:

**WARNING: veyron speedy boards (e.g. C201) have non-functional video init as
of 19 February 2023, and no fix is yet available on that date. See:
<https://notabug.org/libreboot/lbmk/issues/136> - the last tested revision
from 2021.01 is known to work, for u-boot on this board. See:\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
(alpernebbi on IRC is looking into this, to bisect uboot and update the latest
revisions) - for now, ROM images deleted from the Libreboot 20221214
and 20230319 releases.**

**WARNING: daisy- and peach- boards require a BL1 bootloader firmware, but the
one from coreboot 3rdparty is a fake/placeholder file. We need logic in the
Libreboot build system for properly fetching/extracting these, plus docs to
cover it. For now, assume that these are broken - ROM images are excluded,
for now, and have been deleted from the Libreboot 20221214 and 20230319
releases. - see: <https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e>
and <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

d945gclf: Doesn't boot at all, according to last report. D510MO is still in
lbmk but still was reported problematic; other boards should be fine (see list
above).

WARNING: Support for these boards is at a proof-of-concept stage. Refer
to [docs/uboot/](../uboot/) for more info about the U-Boot payload.

### Emulation

-   [Qemu x86](../misc/emulation.md)
-   [Qemu arm64](../misc/emulation.md)


TODO: More hardware is supported. See `config/coreboot/` in lbmk. Update
the above list!

'Supported' means that the build scripts know how to build ROM images
for these systems, and that the systems have been tested (confirmed
working). There may be exceptions; in other words, this is a list of
'officially' supported systems.

EC update on i945 (X60, T60) and GM45 (X200, X301, T400, T500, R400, W500, R500)
==============================================================

It is recommended that you update to the latest EC firmware version. The
[EC firmware](../../faq.md#ec-embedded-controller-firmware) is separate from
libreboot, so we don't actually provide that, but if you still have
Lenovo BIOS then you can just run the Lenovo BIOS update utility, which
will update both the BIOS and EC version. See:

-   [../install/#flashrom](../install/#flashrom)
-   <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

NOTE: this can only be done when you are using Lenovo BIOS. How to
update the EC firmware while running libreboot is unknown. libreboot
only replaces the BIOS firmware, not EC.

Updated EC firmware has several advantages e.g. better battery
handling.

How to find what EC version you have (i945/GM45)
------------------------------------------------

In Linux, you can try this:

	grep 'at EC' /proc/asound/cards

Sample output:

	ThinkPad Console Audio Control at EC reg 0x30, fw 7WHT19WW-3.6

7WHT19WW is the version in different notation, use search engine to find
out regular version - in this case it's a 1.06 for x200 tablet

Alternatively, if `dmidecode` is available, run the following command (as `root`) to
find the currently flashed BIOS version:

	dmidecode -s bios-version

On a T400 running the latest BIOS this would give `7UET94WW (3.24 )` as result.
