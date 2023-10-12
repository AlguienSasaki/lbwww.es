---
title: Build from source
x-toc-enable: true
...

WARNING: Flash from bin/, NOT elf/
==================================

**WARNING: When you build a ROM image from the Libreboot build system, please
ensure that you flash the appropriate ROM image from `bin/`, NOT `elf/`.
The `elf/` coreboot ROMs do not contain payloads. Libreboot's build system
builds no-payload ROMs under `elf/`, and payloads separately under `elf/`. Then
it copies from `elf/` and inserts payloads from `elf/`, and puts the final ROM
images (containing payloads) in `bin/`. This design is more efficient, and
permits many configurations without needless duplication of work. More info
is available in the [lbmk maintenance manual](../maintain/)**

Also, this page currently only refers to the build system as it exists
in `lbmk.git`. A massive re-design of lbmk has been in progress, since the
Libreboot 20230625 release. When the next version after 20230625 comes out,
this page will once again match the current tarball release.

Introduction
============

libreboot's build system is named `lbmk`, short for `LibreBoot MaKe`, and this
document describes how to use it. With this guide, you can know how to compile
libreboot from the available source code.
This version, if hosted live on libreboot.org, assumes that you are using
the `lbmk` git repository, which
you can download using the instructions on [the code review page](../../git.md).

If you're using a release archive of libreboot, please refer to the
documentation included with *that* release. libreboot releases are only intended
as *snapshots*, not for development. For proper development, you should always
be working directly in the libreboot git repository.

The following document describes how `lbmk` works, and how you can make changes
to it: [libreboot maintenance manual](../maintain/)

Git
===

Libreboot's build system uses Git, extensively. You should perform the steps
below, *even if you're using a release archive*.

Before you use the build system, please know: the build system itself uses
Git extensively, when downloading software like coreboot and patching it.

You should make sure to initialize your Git properly, before you begin or else
the build system will not work properly. Do this:

	git config --global user.name "John Doe"
	git config --global user.email johndoe@example.com

Change the name and email address to whatever you want, when doing this.

You may also want to follow more of the steps here:
<https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>

Python
======

Python2 is unused by lbmk or anything that it pulls down as modules. You
should ensure that the `python` command runs python 3, on your system.

Building Libreboot
==================

Actual development/testing is always done using `lbmk` directly, and this
includes when building from source. Here are some instructions to get you
started:

libreboot includes a script that automatically installs apt-get dependencies
in Ubuntu 20.04 and distros based upon it:

	sudo ./build dependencies ubuntu2004

Separate scripts also exist:

	sudo ./build dependencies debian

	sudo ./build dependencies arch

	sudo ./build dependencies void

Check: `config/dependencies/` for list of supported distros.

Technically, any Linux distribution can be used to build libreboot.
However, you will have to write your own script for installing build
dependencies. 

libreboot Make (lbmk) automatically runs all necessary commands; for
example, `./build fw coreboot` will automatically run `./build fw grub`
if the required GRUB payload (under `elf/grub/`) does not exist.

As a result, you can now (after installing the correct build dependencies) run
just a single command, from a fresh Git clone, to build all ROM images:

	./build fw coreboot all

or even just build specific ROM images, e.g.:

	./build fw coreboot x60

or get a list of supported build targets:

	./build fw coreboot list

If you wish to build payloads, you can also do that. For example:

	./build fw grub

	./update project trees -b seabios

	./update project trees -b u-boot

Previous steps will be performed automatically. However, you can *still* run
individual parts of the build system manually, if you choose. This may be
beneficial when you're making changes, and you wish to test a specific part of
lbmk.

Post-compilation steps
======================

So you compiled your Libreboot image? Congratulations!

Before you flash, please make sure that you *dumped* two copies of the original
firmware just in case (verifying the hashes of each dump, to ensure that they
match), using the `-r` option in flashrom.

There are some additional considerations:

Regarding binary blobs
----------------------

Firstly, you should read the Libreboot *Binary Blob Reduction Policy*:
<https://libreboot.org/news/policy.html>

A lot of mainboards in Libreboot will boot entirely with free software,
but there are certain newer mainboards supported in Libreboot
that need binary blobs added to them, mostly for minor initialisation tasks.

They are *automatically* inserted during the build process, but then deleted
during the *release* process; images built directly from lbmk can be flashed
without further modification, but release images need to have these files
re-inserted (e.g. KBC1126 EC firmware on HP EliteBooks). Please read this page
for context:
<https://libreboot.org/docs/install/ivy_has_common.html>

The scripts under directory `script/update/vendor/` are the ones that do this.
Specifically, `script/update/vendor/download`. The benefit of this is that you
do not need to extract anything from the original vendor image (e.g. Lenovo
UEFI image).

To know if this was performed on your board, you can read the Freedom Status
page which documents which files are used (if any), on which boards:
<https://libreboot.org/freedom-status.html>

There are specific files that you must know about, so they will be covered
next:

### Regarding CPU microcode

Libreboot release archives provide ROMs with or without CPU microcode updates,
but the newest revisions of lbmk (from `lbmk.git`) *only* build ROM images that
contain microcode updates, by default. Libreboot's build system contains a script
that will generate release archives, and this is used to provide Libreboot
releases; this script is what provides the images with microcode removed,
alongside the default ones generated by lbmk during the build.

Please read:
<https://libreboot.org/freedom-status.html#cpu-microcode>

The linked page has info, and includes instructions about how to manually remove
them, if you wish to do so. Removal of CPU microcode updates (on Intel/AMD
processors) will result in unpredictable security/stability issues, so beware.

Further context is provided in the Libreboot *Binary Blob Reduction Policy*:
<https://libreboot.org/news/policy.html>

We *recommend* that you do not delete these updates, but you have the freedom
to choose. Additionally: we have only tested certain mainboards under this
configuration. To know which ones they are, do this in `lbmk.git`:

	cd config/coreboot/
	git grep microcode_required

Example entry:

	e6400_4mb/target.cfg:microcode_required="n"

If it says `="n"`, that means it has been tested. That *does not* mean it will
be stable, it's just that we're reasonably sure it will at least *boot* most
of the time, but you should expect random instability e.g. kernel panics.

If a given mainboard *excludes* `microcode_required` in `target.cfg`, or if
it says `microcode_required="y"`, then the release ROMs will only contain
microcode updates. Manual removal (as above) is still possible, but it has
not been tested and is not recommended.

NOTE: ARM-based chromebooks (that Libreboot supports) don't use microcode at
all. This section is only revelant for x86 (Intel/AMD).

### Regarding Nvidia GPU ROM (Dell Latitude E6400)

Dell Latitude E6400 comes in two variants: Nvidia graphics, or Intel graphics.
More information is available on the page:
<https://libreboot.org/docs/hardware/e6400.html>

The intel graphics model can boot with coreboot's native video initialisation,
which is free software. The *Nvidia* model (comes with Nvidia GPU) requires
proprietary code called a *VGA Option ROM* to initialise the video display in
early boot.

Libreboot automatically fetches this during the build process, inserting it into
the very same ROM image that can be flashed on either model, but the VGA ROM
will only be *executed* if you actually have the Nvidia ROM.

ALSO: Libreboot 20230625 did not support Nvidia models at all (only Intel).

Pre-built ROM images from release archives after 20230625 will *not* contain
this file by default, but it will be present if you compiled directly from lbmk.
To remove it, do this:

	./cbutils/default/cbfstool libreboot.rom remove -n pci10de,06eb.rom

The cbfstool binary was compiled when your image (target `e6400_4mb`) was
compiled, which can alter the file system within your coreboot image.

Obviously, removing this VGA ROM will mean that the ROM image will only work
correctly on Intel GPU variants of E6400, but you probably knew that already!

(If you're using a release archive instead, you can re-insert the VGA ROM by
following [these instructions](../install/ivy_has_common.md))

20230625 build error (release archive)
======================================

When building ROM images from the release archives, the following error
is observed in some cases, depending on distro:

```
In file included from src/lib/version.c:4:
build/build.h:10:32: error: 'libreboot' undeclared here (not in a function)
   10 | #define COREBOOT_MAJOR_VERSION libreboot-20230625
      |                                ^~~~~~~~~
src/lib/version.c:35:46: note: in expansion of macro 'COREBOOT_MAJOR_VERSION'
   35 | const unsigned int coreboot_major_revision = COREBOOT_MAJOR_VERSION;
      |                                              ^~~~~~~~~~~~~~~~~~~~~~
```

This happened when a user tried to build for ThinkPad W541 on an Arch Linux
system. The fix is available here:

<https://browse.libreboot.org/lbmk.git/patch/?id=f34e07ae27e3e6e8508cdebcbd09fdf73fca302d>

Apply this patch to your local release archive, and it should fix the issue.
