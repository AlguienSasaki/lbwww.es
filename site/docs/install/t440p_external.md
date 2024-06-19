---
title: ThinkPad T440p external flashing
x-toc-enable: true
...

Buy Libreboot preinstalled
==========================

This laptop is available to buy with Libreboot pre-installed:
<https://minifree.org/product/libreboot-t440p/>

Introduction
============

NOTE: Libreboot standardises on [flashprog](https://flashprog.org/wiki/Flashprog)
now, as of 27 January 2024, which is a fork of flashrom.

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../../news/safety.md),
OR YOU MIGHT BRICK YOUR MACHINE: [SAFETY PRECAUTIONS](../../news/safety.md)**

Read the [Ivybridge/Haswell common guide](/docs/install/ivy_has_common.html) if you want more information.
All of the following instructions assume that you've cloned lbmk and are operating from the
root of that project. To do so, run

	git clone https://codeberg.org/libreboot/lbmk
	cd lbmk

You can now follow the rest of the instructions.

Preparing a release Rom
-----------------------

You must patch the release rom with the necessary vendor files *and then* flash it to your board.

Lbmk includes a script that will automatically inject the necessary files into a rom file.
The script can determine the board automatically if you have not changed the name, but you can also manually set the board name with the `-b` flag.

In order to inject the necessary files into a rom image, run the script from the root of lbmk and point to the rom image.

If you only wish to flash a release rom then the process of injecting the necessary files is quite simple.
Run the injection script pointing to the release archive you downloaded:

	./vendor inject /path/to/libreboot-RELEASE_targetname.tar.xz

The script can automatically detect the board as long as you do not change the file name.
You can then find flash-ready ROMs in `/bin/release/`

Alternatively, you may patch only a single rom file.
For example:

	./vendor inject -r t440p_libreboot.rom -b t440plibremrc_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./vendor inject -r t440p_libreboot.rom -b t440plibremrc_12mb -m 00:f6:f0:40:71:fd

NOTE: this makes use of `nvmutil`, which you can read more about in
the [nvmutil documentation](nvmutil.md).

Splitting The Rom
-----------------

You can use `dd` to easily split your rom into the two separate portions for
external flashing.

	dd if=libreboot.rom of=top.rom bs=1M skip=8
	dd if=libreboot.rom of=bottom.rom bs=1M count=8

Flash the top chip with top.rom, and tho bottom chip with bottom.rom.
Don't worry about knowing which chip is which on a standard setup; flashprog will let you know if the 
image size is incorrect for the chip you're flashing.


Disassembly
-----------

Start by removing the back cover screws and the main battery.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_back.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_back_orig.jpg" /></span>

You can then remove the back cover by sliding it off.
Next you need to:

+ Unplug the cmos battery
+ Unplug and unroute the fan cable
+ Unplug and unroute the black LED cable
+ Remove all visible screws

*Note: the ultrabay screw will loosen, but not come out of the assembly*\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_nocover.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_nocover_orig.jpg" /></span>

Now you can pull up around the sides of the bottom assembly to release it.
Pull it upwards and lift it open to the front of the machine like a clamshell.
Make sure not to break the wires connecting the assembly to the rest of the machine.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_open.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_open_orig.jpg" /></span>

You should now be able to see the two flash chips near the RAM.\
<img tabindex=1 src="https://av.libreboot.org/board/t440p/t440p_chipLocation.jpg" /><span class="f"><img src="https://av.libreboot.org/board/t440p/t440p_chipLocation_orig.jpg" /></span>

You can now proceed to [flashing](/docs/install/spi.html) this machine.

Errata
======

NOTE: As of Libreboot releases from May 2024 onward, the Intel MRC is no longer
included for Haswell; MRC is a blob for raminit, but we now provide libre
raminit. The following targets no longer exist in the build system:

* `t440pmrc_12mb` (use `t440plibremrc_12mb` instead)
* `t440pbmrc_12mb` (use `t440plibremrc_12mb` instead)
* `w541mrc_12mb` (use `w541_12mb` instead)
* `w541bmrc_12mb` (use `w541_12mb` instead)
* `dell9020sff_12mb` (use `dell9020sff_nri_12mb` instead)
* `dell9020sffbmrc` (use `dell9020sff_nri_12mb` instead)
* `dell9020mt_12mb` (use `dell9020mt_nri_12mb` instead)
* `dell9020mtbmrc` (use `dell9020mt_nri_12mb` instead)

This is written as errata because some users may still be using older release
images but on the newer build system from May 2024 onward; you must use the
Libreboot 20240225 release if you want to inject MRC and so on, for these older
targets.

Libreboot's [binary blob reduction policy](../../news/policy.md) is very strict,
and states: if a blob can be avoided, it must be avoided. Therefore, the MRC
is removed on Haswell and Libreboot will only use the libre raminit (called
NRI, short for Native Ram Initialisation).
