---
title: Ivybridge internal flashing
x-toc-enable: true
...

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../../news/safety.md),
OR YOU MIGHT BRICK YOUR MACHINE: [SAFETY PRECAUTIONS](../../news/safety.md)**

External flashing still recommended
-----------------------------------

Internal flashing is quite complex, from a software and hardware
perspective, when switching from Lenovo firmware to Libreboot.
If you already have Libreboot, then you likely have the entire
flash unlocked so you can refer to generic flashing instructions.

Internal flashing from Lenovo firmware is more time consuming, but basically
costs less money, because there's less equipment that you need for it.

This method is also more risky, because one of the steps involves shorting
two pins on the HDA (audio) chip, and if you do it wrong you could short
the wrong thing by mistake; consequences could be blown fuses and/or fire,
or just a dead ThinkPad. Proceed at your own risk!

If you prefer external flashing, see: [external flashing](x230_external.md)

Internal flashing from vendor firmware (ThinkPads only)
----------------------------------------

IVYBRIDGE ONLY:

Refer to this coreboot guide:
<https://doc.coreboot.org/mainboard/lenovo/ivb_internal_flashing.html?highlight=x230>

With this guide, you can exploit a vulnerability in Lenovo firmware, to flash
just the BIOS region without disassembling your machine.

You will have to flash just the BIOS region, on upstream coreboot. Just compile
upstream coreboot from scratch. Coreboot instructions here:
<https://doc.coreboot.org/tutorial/index.html>

You can then strap HDA\_SDO (soft descriptor override), which will disable
flash protections set in the Intel Flash Descriptor. More info about that is
available here:
<https://winraid.level1techs.com/t/guide-unlock-intel-flash-descriptor-read-write-access-permissions-for-spi-servicing/32449>

On ivybridge platforms specifically, coreboot supports what's called
the *ME Soft Temporary Disable*, which disables the ME after BringUp, similar
to `me_cleaner`. You can do this by setting `me_state=Disabled` in nvramtool.

Now boot with HDA\_SDO strapped. On the HDA (audio) chip, there is a pin that
you can short, to disable IFD-based flash protections. You simply HOLD it in
the shorted state, while booting up the machine, and if successful, you will
then have the flash unlocked. This, combined with ME Soft Temporary Disable,
and the internal flashing guide linked above (from coreboot.org), you'll get
to a state where you can simply flash all regions. You can then flash the
truncated+neutered images from Libreboot, after building the ROMs in lbmk; see
[Libreboot build instructions](../build/).

The following photo shows what to short on the ThinkPad X230. You'll have to
look for it. X230 is easy because you don't have to directly touch the pins,
instead you can use alt points that have continuity. They are marked on this
photo:

<img tabindex=1 src="https://av.libreboot.org/x230/hda_sdo.jpg" /><span class="f"><img src="https://av.libreboot.org/x230/hda_sdo.jpg" /></span>

TODO: Document other ivybridge thinkpads (just show pics)
