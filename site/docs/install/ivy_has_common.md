---
title: Insert vendor files not included in release images
x-toc-enable: true
...

WARNING: eCryptfs file name limits
=================================

Do not run the build system on a eCryptfs file system, because it has
very short file name limits and Libreboot's build system deals with very
long file names. We commonly get reports from this by Linux Mint users
who encrypt their home directory with cryptfs; use a serious distro like
Debian or Arch please. And use a decent file system; ZFS or perhaps LUKS
with btrfs will do nicely.

**Install build dependencies first**
================================

**You will be compiling several small utilities from source code. This means
you need the compilers and various libraries.**

**Please make sure to install [build dependencies](../build/)** before using this
guide, and note that this guide assumes you use [lbmk.git](../../git.md).

**Failure to adhere to this warning will result in vendor file insertion not
working. The insertion must work correctly, prior to Libreboot installation,
if your board requires it, otherwise your board simply will not boot.**

Introduction
============

Coreboot is nominally free software, but requires certain vendor code on some
boards, for certain functionalities; we cover this more thoroughly in
the [Freedom Status](../../freedom-status.md) page and in the [Binary Blob
Reduction Policy](../../news/policy.md).

Libreboot can't directly distribute *all* of these blobs, so some of them are
downloaded at build-time, and processed for insertion into the firmware images.
**On pre-compiled ROM images in releases, these files are removed, and can be
re-added using the same automation that was applied during the build process.**

Examples of vendor files can include, for example: Intel ME (disabled after
boot with me\_cleaner), Embedded Controller firmware (e.g. KBC1126 EC firmware
on HP EliteBooks), VGA ROMs (e.g. Nvidia GPU ROM for Dell Latitude E6400),
and so on. Without these, your machine may not boot correctly, or not boot at
all!

The same logic can be used after the fact, to re-download and re-insert these
files; the page that you're reading now will tell you how to do so.

*If in doubt, just follow these instructions anyway; if your board doesn't need
vendor files inserted, nothing will happen. You only need to follow this guide
if you use release ROMs; if you're building directly from source, using the
Libreboot build system, then you can just flash the result.*

Injecting vendor files ROMs tarball
------------------------------------

You must determine the correct board name, for your board, based on the list
generated when running this command:

	./mk -b coreboot list

In order to inject the necessary files into a rom image, run the script from the root of lbmk and point to the rom image.

If you only wish to flash a release rom then the process of injecting the necessary files is quite simple.
Run the injection script pointing to the release archive you downloaded:

	./mk inject libreboot-RELEASE_targetname.tar.xz

The script can automatically detect the board as long as you do not change the file name.
You can then find flash-ready ROMs in `/bin/release/`

*This is the recommended way to do it, injecting into the tarball.*

Injecting vendor files into single ROMs
---------------------------------------

**You are strongly advised only to insert it on the tarball, because then
checksums are verified to ensure that the vendor files were inserted correctly.
Otherwise, you can do it manually on each individual image, specifying the
board name with the instructions provided below:**

**However, when injecting into the tarball in bulk like that, lbmk currently
cannot change the MAC addresses automatically, using the `-m` option mentioned
below.**

**Therefore, if you want to rely on insertion into the tarball, you can just
copy the ROM you want and [change the MAC address manually](nvmutil.md).**

Alternatively, you may patch only a single rom file, but you must supply the
correct board target name as alluded to above.
For example:

	./mk inject -r x230_libreboot.rom -b x230_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./mk inject -r x230_libreboot.rom -b x230_12mb -m 00:f6:f0:40:71:fd

You are *strongly* advised to inject the tarballs instead. However, so long as
you're careful, injecting into single ROM images is perfectly safe. Just know
once more that the checksum verification is unavailable in the latter, so you
must absolutely ensure that you specified the correct board with the `-b`
option.

Check that the files were inserted
==================================

You *must* ensure that the files were inserted. The inject command automatically
verifies checksums of the complete images, when you run it directly on a
release tarball, but not when running it manually on an individual image;
checking it manually is useful for the latter, but you should probably just
insert it into the tarball.

Some examples of how to do that in lbmk:

	./mk -d coreboot TREENAME

TREENAME should be the coreboot tree corresponding to your board. Check
this in `config/coreboot/BOARD/target.cfg` for your board, and `tree` will be
set to e.g. `default`, or some other tree name.

Now you find `elf/cbfstool`, which is a directory containing `cbfstool`
and `ifdtool`. Do this on your ROM image (`libreboot.rom` in the example
below):

	./elf/cbfstool/TREENAME/cbfstool libreboot.rom print

You should check that the files were inserted in cbfs, if needed; for example,
EC firmware or MRC firmware.

Next:

	./elf/ifdtool/TREENAME/ifdtool -x libreboot.rom

This creates several `.bin` files, one of which says `me` in it (Intel ME).
Run hexdump on it:

	hexdump flashregion_2_intel_me.bin

Check the output. If it's all `0xFF` (all ones) or otherwise isn't a bunch
of code, then the Intel ME firmware wasn't inserted.

You'll note the small size of the Intel ME, e.g. 84KB on sandybridge platforms.
This is because lbmk *automatically* neuters it, disabling it during
early boot. This is done using `me_cleaner`, which lbmk imports.

NOTE: the MAC changer makes use of `nvmutil`, which you can read more about in
the [nvmutil documentation](nvmutil.md).

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
