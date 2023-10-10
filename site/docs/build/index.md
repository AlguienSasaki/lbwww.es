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
example, `./build grub payload` will automatically run `./build grub utils`
if the required utilities for GRUB are not built, to produce payloads.

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
