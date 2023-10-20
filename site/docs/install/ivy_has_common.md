---
title: Insert vendor files on Sandybridge/Ivybridge/Haswell
x-toc-enable: true
...

**NOTE: This page also applies to PM45 machine Dell Latitude E6400 if it
contains an Nvidia GPU (Libreboot vendor scripts can insert the VGA ROM)**

This is not a general purpose installation guide, but you *should read it*
before installing Libreboot! *This* guide *must* be followed, no matter what
method you use to install Libreboot; even if you compile from source, you should
still read this page for later reference.

For how to use an external programmer see the [25xx NOR flashing guide](/docs/install/spi.html)

Introduction
============

**PLEASE MAKE SURE you have build dependencies installed first. Although you
do not need to re-compile release ROMs, you should follow these instructions
before reading the instructions below:
<https://libreboot.org/docs/build/#first-install-build-dependencies>**

Coreboot is nominally free software, but requires certain vendor code on some
boards, for certain functionalities; we cover this more thoroughly in
the [Freedom Status](../../freedom-status.md) page and in the [Binary Blob
Reduction Policy](../../news/policy.md).

Well, not all of these files are freely redistributable. Coreboot does provide
vendor files in some cases, if the vendor has allowed it. In other cases,
extraction from factory firmware is required, or you can extract them from
vendor-supplied updates - Libreboot's build system does the latter.

When you [compile Libreboot ROM images from source](../build/), Libreboot will
automatically download any given files that are required, for any given board
target. This is done without user intervention, and only when absolutely needed
to make the machine boot properly.

The problem?
------------

Well, if the files cannot be freely redistributed, then we can't provide them.
So how do we handle *that*, in the context of Libreboot releases?

The solution
------------

The answer is very simple:  these files are **NOT** provided, at all! However,
the very same logic used by the build system can be run standalone, to re-insert
these vendor files on release ROMs. The `inject` script detects what files are
needed for your ROM image.

The script will detect what board you're inserting on, or you can manually tell
it what board, and it will fetch them for you, inserting them, so that your
board is ready to flash - flashing it without these required files may result in
a brick.

Vendor file locations
--------------

During auto-download of files, they are saved to these locations within the
Libreboot build system:

* ME firmware: `vendor/*/me.bin` - the `*` can be any given directory. Different ones will
  be used by given boards, but the directory name may not match the board
  target name.
* SMSC SCH5545 fan control firmware (for Dell T1650): `vendor/t1650/sch5545ec.bin`
* SMSC KBC1126 embedded controller firmware, on HP EliteBooks: `ec/`
* Intel MRC firmware, used for ram/peripheral init on Haswell machines such as
  thinkpad t440p/w541: `mrc/`

The above list refers to the *non-redistributable files*, and these are not
directly included in releases. These are auto-downloaded during the build.
The `me.bin` files are produced by extracting them from vendor updates and
neutering them with `me_cleaner` so that Intel ME is disabled during early boot.

Injecting vendor files into ROM
------------------------------------

You must determine the correct board name, for your board, based on the list
generated when running this command:

	./build roms list

For example, `t440pmrc_12mb` corresponds to ThinkPad T440p with MRC firmware.
Whereas `t440plibremrc_12mb` corresponds to T440p with libre MRC firmware.
Another example: `x230_12mb` corresponds to Thinkpad X230.

In order to inject the necessary files into a rom image, run the script from the root of lbmk and point to the rom image.

If you only wish to flash a release rom then the process of injecting the necessary files is quite simple.
Run the injection script pointing to the release archive you downloaded:

	./vendor inject /path/to/libreboot-20230319-18-g9f76c92_t440pmrc_12mb.tar.xz

The script can automatically detect the board as long as you do not change the file name.
You can then find flash-ready ROMs in `/bin/release/`

Alternatively, you may patch only a single rom file, but you must supply the
correct board target name as alluded to above.
For example:

	./vendor inject -r x230_libreboot.rom -b x230_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./vendor inject -r x230_libreboot.rom -b x230_12mb -m 00:f6:f0:40:71:fd

Check that the files were inserted
==================================

You *must* ensure that the files were inserted.

Some examples of how to do that in lbmk:

	./update trees -b coreboot utils

Now you find `cbutitls/default`, which is a directory containing `cbfstool`
and `ifdtool`. Do this on your ROM image (`libreboot.rom` in the example
below):

	./cbutils/default/cbfstool libreboot.rom print

You should check that the files were inserted in cbfs, if needed; for example,
EC firmware or MRC firmware.

Next:

	./cbutils/default/ifdtool -x libreboot.rom

This creates several `.bin` files, one of which says `me` in it (Intel ME).
Run hexdump on it:

	hexdump flashregion_2_intel_me.bin

Check the output. If it's all `0xFF` (all ones) or otherwise isn't a bunch
of code, then the Intel ME firmware wasn't inserted.

You'll note the small size of the Intel ME, e.g. 84KB on sandybridge platforms.
This is because lbmk *automatically* neuters it, disabling it during
early boot. This is done using `me_cleaner`, which lbmk imports.

Errata
======

**NOTE: Haswell machines come with `mrc.bin` or without, depending on the
ROM image configuration. These ROM configs have `mrc.bin`: `t440pmrc_12mb`
and `w541mrc_12mb`. These ROM configs have libre MRC: `t440p_12mb`
and `w541_12mb` - it is critical that you choose the right one, when using
the `-b` flag in the `./vendor inject` command. For example, if you
used `-b t440p_12mb` on a ROM image that actually corresponds
to `t440pmrc_12mb`, then the required `mrc.bin` file would not be added
and that ROM would not boot when flashed.**
