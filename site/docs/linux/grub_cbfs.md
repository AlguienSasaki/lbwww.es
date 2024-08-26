---
title: Modifying grub.cfg in CBFS
x-toc-enable: true
...

NOTE: Libreboot standardises on [flashprog](https://flashprog.org/wiki/Flashprog)
now, as of 27 January 2024, which is a fork of flashrom.

Before you follow this guide, it is advisable that you have the ability to
flash externally, just in case something goes wrong.

Libreboot's own GRUB configuration automatically scans for one provided by
your distro, and this automation will usually work. Sometimes, you might wish
to override it with your own custom menuentry or additional logic in the GRUB
config. You can configure GRUB however you like, and this topic is vast so what
to actually *put in the config* will not be covered here.

This guide will simply teach you how to modify the config, but not what to put.

Compile flashprog and cbfstool
=============================

libreboot does not currently distribute utilities pre-compiled. It only
provides ROM images pre-compiled, where feasible. Therefore, you have to build
the utilities from source.

As for the ROM, there are mainly three methods for obtaining a libreboot ROM
image:

1. Dump the contents of the the main *boot flash* on your system, which already
   has libreboot installed (with GRUB as the default payload). Extract the
   GRUB configuration from *that* ROM image.
2. Extract it from a libreboot ROM image supplied by the libreboot project, on
   the libreboot website or mirrors of the libreboot website.
3. Build the ROM yourself, using the libreboot build system. Instructions for
   how to do this are covered in the following article:
   [How to build libreboot from source](../build/)

In either case, you will use the `cbfstool` supplied in the libreboot build
system.
This can be found under `coreboot/*/util/cbfstool/` as source code,
where `*` can be any coreboot source code directory for a given mainboard.
The directory named `default` should suffice.

Install the build dependencies. For Debian and similar, you can run
the following command in the libreboot build system, from the root directory
of the libreboot Git repository.

	./mk dependencies debian

Determine what coreboot tree you need for your board. For example, if building
for `x200_8mb`, check `config/coreboot/x200_8mb/target.cfg` and it might
say `tree="default"` - in this case, the coreboot tree is named `default`.

Then, download coreboot (we'll assume the `default` tree is correct):

	./mk -f coreboot default

Finally, compile the `cbutils` payload (and you will then have the utils):

	./mk -b grub

GRUB is multi-tree, but for GRUB utilities that's fine because we don't patch
those in any tree; we only patch the GRUB kernel to add various drivers and
extra crypto such as argon2.

Among other things, this will produce a `cbfstool` executable under any of the
subdirectories in `src/coreboot/` under `util/cbfstool/cbfstool`.

For example: `src/coreboot/default/util/cbfstool/cbfstool`

The `cbfstool` utility is what you shall use. It is used to manipulate CBFS
(coreboot file system) which is a file system contained within the coreboot
ROM image; as a *coreboot distribution*, libreboot inherits this technology.

You can compile cbfstool and ifdtool for the given coreboot tree, e.g.:

	./mk -d coreboot default

This will create `elf/cbfstool/default/cbfbstool`
and `elf/ifdtool/default/ifdtool`.

You will also want to build `flashprog` which libreboot recommends for reading
from and/or writing to the boot flash. In the libreboot build system, you can
build it by running this command:

	./mk -b flashprog

An executable will be available at `elf/flashprog/flashprog` after you have done
this.

Dump the boot flash
===================

[Learn how to externally reprogram these chips](../install/spi.md) and use
the `-r` option in flashprog; alternatively, for internal flash access,
look at the [main flashing guide](../install/).

Those guides show how to dump the flash contents, which you are advised to do.

Insert new grub.cfg
===================

If you already have a `grub.cfg` in cbfstool, you can extract and modify that
one, e.g.:

	cbfstool libreboot.rom extract -n grub.cfg -f grub.cfg

Now remove it:

	cbfstool libreboot.rom remove -n grub.cfg

It's important that you re-add `grub.cfg` before flashing:

	cbfstool libreboot.rom add -f grub.cfg -n grub.cfg

Repeat this for `grubtest.cfg` if you wish.

If you're using a default Libreboot image, there *is no `grub.cfg` in flash*.
This is because there is also a default one embedded inside the GRUB binary,
which is inside CBFS. So:

The coreboot image has its own filesystem, CBFS, and within CBFS is the GRUB
binary, and within the GRUB binary is another filesystem called memdisk, where
the default GRUB configuration is located.

You can insert a `grub.cfg` into CBFS and it will override the one in memdisk.

Check your board, e.g. `x200_8mb`, look at the file: `config/coreboot/x200_8mb/target.cfg`
and check for `grubtree` - if it's not sot, then it's `default`, otherwise check
what it's set to.

We'll assume it's `default`, so the
file `config/grub/default/config/payload` is your GRUB config; this will be the
same as what yo uhave in memdisk.

Modify *that* file, or the one you extracted if you already inserted a custom
one before, and you will re-insert it when you're done.

Flash the modified ROM image
============================

Check the [Libreboot flashing guide](../install/) which says how to flash the
new image.
