---
title: Insert binary blobs on Sandybridge/Ivybridge/Haswell
x-toc-enable: true
...

**TODO: Re-translate this again to Ukrainian. It was translated before, but
the english page got heavily re-written.**

**NOTE: This page also applies to PM45 machine Dell Latitude E6400 if it
contains an Nvidia GPU (Libreboot blob scripts can insert the VGA ROM)**

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

Coreboot is nominally free software, but requires binary blobs for certain
boards, for certain functionalities; it differs per board, and some boards do
not require blobs of any kind in the flash. We cover this more thoroughly in
the [Freedom Status](../../freedom-status.md) page and in the [Binary Blob
Reduction Policy](../../news/policy.md).

Well, not all of these blobs are freely redistributable. Coreboot does provide
binary blobs in some cases, if the vendor has allowed it. In other cases,
extraction from factory firmware is required, or you can extract them from
vendor-supplied updates - Libreboot's build system does the latter.

When you [compile Libreboot ROM images from source](../build/), Libreboot will
automatically download any given blobs that are required, for any given board
target. This is done without user intervention, and only when absolutely needed
to make the machine boot properly.

The problem?
------------

Well, if the blobs cannot be freely redistributed, then we can't provide them.
So how do we handle *that*, in the context of Libreboot releases?

The solution
------------

The answer is very simple:  these blobs are **NOT** provided, at all! However,
the very same logic used by the build system can be run standalone, to re-insert
these binary blobs on release ROMs. The `inject` script detects what blobs are
needed for your ROM image.

The script will detect what board you're inserting on, or you can manually tell
it what board, and it will fetch them for you, inserting them, so that your
board is ready to flash - flashing it without these required blobs may result in
a brick.

Blob locations
--------------

During auto-download of blobs, they are saved to these locations within the
Libreboot build system:

* ME firmware: `blobs/*/me.bin` - the `*` can be any given directory. Different ones will
  be used by given boards, but the directory name may not match the board
  target name.
* SMSC SCH5545 fan control firmware (for Dell T1650): `blobs/t1650/sch5545ec.bin`
* SMSC KBC1126 embedded controller firmware, on HP EliteBooks: `ec/`
* Intel MRC firmware, used for ram/peripheral init on Haswell machines such as
  thinkpad t440p/w541: `mrc/`

The above list refers to the *non-redistributable blobs*, and these are not
directly included in releases. These are what `blobutil` auto-downloads.
The `me.bin` files are produced by extracting them from vendor updates and
neutering them with `me_cleaner` so that Intel ME is disabled during early boot.

Injecting Blobs into an Existing Rom
------------------------------------

You must determine the correct board name, for your board, based on the list
generated when running this command:

	./build boot roms list

For example, `t440pmrc_12mb` corresponds to ThinkPad T440p with MRC firmware.
Whereas `t440plibremrc_12mb` corresponds to T440p with libre MRC firmware.
Another example: `x230_12mb` corresponds to Thinkpad X230.

In order to inject the necessary blobs into a rom image, run the script from the root of lbmk and point to the rom image.

If you only wish to flash a release rom then the process of injecting the necessary blobs is quite simple.
Run the injection script pointing to the release archive you downloaded:

	./update blobs inject /path/to/libreboot-20230319-18-g9f76c92_t440pmrc_12mb.tar.xz

The script can automatically detect the board as long as you do not change the file name.
You can then find flash-ready ROMs in `/bin/release/`

Alternatively, you may patch only a single rom file, but you must supply the
correct board target name as alluded to above.
For example:

	./update blobs inject -r x230_libreboot.rom -b x230_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./update blobs inject -r x230_libreboot.rom -b x230_12mb -m 00:f6:f0:40:71:fd

Check that the blobs were inserted
==================================

You *must* ensure that the blobs were inserted.

Some examples of how to do that in lbmk:

	./build module cbutils

Now you find `cbutitls/default`, which is a directory containing `cbfstool`
and `ifdtool`. Do this on your ROM image (`libreboot.rom` in the example
below):

	./cbutils/default/cbfstool libreboot.rom print

You should check that the blobs were inserted in cbfs, if needed; for example,
EC firmware or MRC firmware.

Next:

	./cbutils/default/ifdtool -x libreboot.rom

This creates several `.bin` files, one of which says `me` in it (Intel ME).
Run hexdump on it:

	hexdump flashregion_2_intel_me.bin

Check the output. If it's all `0xFF` (all ones) or otherwise isn't a bunch
of code, then the Intel ME firmware wasn't inserted.

You'll note the small size of the Intel ME, e.g. 84KB on sandybridge platforms.
This is because blobutil *automatically* neuters it, disabling it during
early boot. This is done using `me_cleaner`, which lbmk imports.

Errata
======

**NOTE: Haswell machines come with `mrc.bin` or without, depending on the
ROM image configuration. These ROM configs have `mrc.bin`: `t440pmrc_12mb`
and `w541mrc_12mb`. These ROM configs have libre MRC: `t440p_12mb`
and `w541_12mb` - it is critical that you choose the right one, when using
the `-b` flag in the `blobutil inject` command. For example, if you
used `-b t440p_12mb` on a ROM image that actually corresponds
to `t440pmrc_12mb`, then the required `mrc.bin` file would not be added
and that ROM would not boot when flashed.**

**NOTE: In the Libreboot 20230319 src archive or git tag, the `blobutil`
insert method is broken on Haswell configs that need `mrc.bin`, because it does
not insert `mrc.bin` at the correct offset. This was fixed in revisions after
the release, and will be available in the next release after that. Read
the [Libreboot 20230319 update
announcement](../../news/libreboot20230319_update.md) for more information.**

NOTE: the MAC changer makes use of `nvmutil`, which you can read more about in
the [nvmutil documentation](nvmutil.md).

**WARNING: This is broken in Libreboot 20221214's src archive. It fails when
attempting to use cbfstool, due to a faulty check in a script. This is fixed in
recent Libreboot releases or revisions. The fix is as
follows:

Edit line 137 in `resources/scripts/blobs/inject`. The line in 20221214 says
this:

	make -C cd coreboot/default/util/cbfstool || Fail 'could not build ifdtool'

Modify it to say this:

	make -C coreboot/default/util/cbfstool || Fail 'could not build cbfstool'

ALSO:

*When generating a MAC address*, the same script tries to build `nvmutil`
from `/util/nvmutil`, in Libreboot 20221214. This was discovered on 10 January
2023, based on user report on IRC. Fix it like so (already fixed, in latest
Libreboot from Git):

Line 30, it says:

	make -C /util/nvmutil || Fail 'failed to build nvmutil'

Change it to say:

	make -C util/nvmutil || Fail 'failed to build nvmutil'

Until this is edited accordingly, the inject script will *exit* with non-zero
status, and no blobs will be injected.

This has been fixed, following the Libreboot 20221214 release, but you must
apply this fix yourself, if using *that* release.
