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

Introduction
============

libreboot's build system is named `lbmk`, short for `LibreBoot MaKe`, and this
document describes how to use it. With this guide, you can know how to compile
libreboot from the available source code.

The following document describes how `lbmk` works, and how you can make changes
to it: [libreboot maintenance manual](../maintain/)

Release status
==============

Information about status will be reported during builds; if a board is
marked as stable, the build proceeds without further input. If the board is
marked anything other, a warning appears asking if you wish to proceed; to
disable these warnings, do this before building (not recommended):

	export LBMK_STATUS=n

In Libreboot, we specify: `stable`, `unstable`, `broken` or `untested`.
The "unstable" marking means that the board boots mostly/entirely reliably
annd should be safe to use, but may have a few issues, but nothing which would,
for example, cause safety issues e.g. thermal, data reliability etc.

The `broken` setting means that a given board will likely brick if flashed.
The `untested` setting means untested.

Release status is always set with regards to the current lbmk revision, on
the theory that the current revision is being used to generate a full release.
The setting is decided on a board-by-board basis, taking its various quirks
and idiosynrasies into account.

Multi-threaded builds
=====================

Libreboot's build system defaults to a single build thread, but you can change
it by doing e.g.

	export LBMK_THREADS=4

This would make lbmk run on 4 threads.

Environmental variables
=======================

Please read about environmental variables in [the build
instructions](../maintain/), before running lbmk. You should set
your variables accordingly, though you do not technically need to; some
of them may be useful, e.g. `LBMK_THREADS` (sets the number of build threads).

Sources
=======

This version, if hosted live on libreboot.org, assumes that you are using
the `lbmk` git repository, which
you can download using the instructions on [the code review page](../../git.md).

A note about documentation (and this page)
------------------------------------------

From Libreboot 20231021 onwards, *all* releases (including 20231021)
have `lbwww.git` (the website) and `lbwww-img.git` (images for the website)
archived in the *src* tar archive for that release; older releases were hit
or miss, from 20210522 to 20230625, as to whether they came with documentation;
releases older than 20210522 generally always came with documentation. Modern
Libreboot documentation is written in Markdown (pandoc variant)

If you're working with *release* documentation, you don't get the full HTML
files (such as the one you're viewing now, if you're reading *this* page in a
web browser), so either read the Markdown files directly, or compile them to
HTML using the [Untitled Static Site Generator](https://untitled.vimuser.org/)
(which is what the Libreboot project uses to generate HTML from those files).

NOTE: `av.libreboot.org` is hardcoded as the domain name where images are
pointed to, in `lbwww.git`, so you will need to replace these references in
your local version, unless you're happy to just continue using those.

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

You should ensure that the `python` command runs python 3, on your system.
Python2 is unused by lbmk or anything that it pulls down as modules. 

If building on Debian/Ubuntu based systems, you can achieve that via:

	sudo apt install python-is-python3

On Fedora, you can use the following

	sudo dnf install python-unversioned-command

How to compile Libreboot
========================

Actual development/testing is always done using lbmk directly, and this
includes when building from source. Here are some instructions to get you
started:

Zero..st, check time/date
-------------------------

Make sure date/hwclock report the correct time and date on your system,
because parts of the build process download from HTTPS servers and wrong
time or date can cause connections to be dropped during negotiation.

First, install build dependencies
---------------------------------

Libreboot includes a script that automatically installs build dependencies
according to the selected linux distro. 
The currently supported distros are: Debian/Ubuntu/Linux Mint/Pop!\_OS, 
Fedora, Arch Linux/Parabola or Void Linux.

Some examples (run them as root, use use e.g. `sudo`, `doas`):

	./build dependencies ubuntu

or

	./build dependencies debian

or

	./build dependencies fedora38

or

	./build dependencies arch

NOTE: In case of Ubuntu 20.04 LTS or derived distros for that specific release,
use the dedicated configuration file:

	./build dependencies ubuntu2004

Check: `config/dependencies/` for list of supported distros.

Technically, any Linux distribution can be used to build libreboot.
However, you will have to write your own script for installing build
dependencies. 

Next, build ROM images
----------------------

Libreboot MaKe (lbmk) automatically runs all necessary commands; for
example, `./build roms` will automatically run `./build grub`
if the required GRUB payload (under `elf/grub/`) does not exist.

As a result, you can now (after installing the correct build dependencies) run
just a single command, from a fresh Git clone, to build all ROM images:

	./build roms all

or even just build specific ROM images, e.g.:

	./build roms x60

or get a list of supported build targets:

	./build roms list

Or maybe just build payloads?
-----------------------------

If you wish to build payloads, you can also do that. For example:

	./build grub

	./update trees -b seabios

	./update trees -b u-boot

Previous steps will be performed automatically. However, you can *still* run
individual parts of the build system manually, if you choose. This may be
beneficial when you're making changes, and you wish to test a specific part of
lbmk.

Want to modify Libreboot?
-------------------------

Check the [lbmk maintenance manual](../maintain/) for guidance. You may for
example want to modify a config, e.g.:

	./update trees -m coreboot x200_8mb

Or perhaps add a new board! The maintenance manual will teach you how the
Libreboot build system (lbmk) works!
