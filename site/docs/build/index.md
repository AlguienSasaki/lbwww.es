---
title: Build from source
x-toc-enable: true
...

WARNING: cryptfs file name limits
=================================

Do not run the build system on a cryptfs file system, because it has
very short file name limits and Libreboot's build system deals with very
long file names. We commonly get reports from this by Linux Mint users
who encrypt their home directory with cryptfs; use a serious distro like
Debian or Arch please. And use a decent file system; zfs or perhaps mdcrypt
with btrfs will do nicely.

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

Multi-threaded builds
=====================

Libreboot's build system defaults to a single build thread, but you can change
it by doing e.g.

	export XBMK_THREADS=4

This would make lbmk run on 4 threads.

More specifically: when compiling source trees via `script/trees`, `-jTHREADS`
is passed, where THREADS is the number of threads. This is also set when running
xz commands for compression, using the `-t` option.

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

On most modern distros, Python 2 is no longer included and Python 3 will be
the only one available on the `python`.

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

	./mk dependencies ubuntu

or

	./mk dependencies debian

or

	./mk dependencies fedora38

or

	./mk dependencies arch

NOTE: In case of Ubuntu 20.04 LTS or derived distros for that specific release,
use the dedicated configuration file:

	./mk dependencies ubuntu2004

Check: `config/dependencies/` for list of supported distros.

Technically, any Linux distribution can be used to build libreboot.
However, you will have to write your own script for installing build
dependencies. 

Next, build ROM images
----------------------

Libreboot MaKe (lbmk) automatically runs all necessary commands; for
example, `./mk -b coreboot` will automatically build the required payloads
if not already compiled.

As a result, you can now (after installing the correct build dependencies) run
just a single command, from a fresh Git clone, to build all ROM images:

	./mk -b coreboot

or even just build specific ROM images, e.g.:

	./mk -b coreboot x60

or get a list of supported build targets:

	./mk -b coreboot list

Or maybe just build payloads?
-----------------------------

If you wish to build payloads, you can also do that. For example:

	./mk -b grub

	./mk -b seabios

	./mk -b u-boot

Previous steps will be performed automatically. However, you can *still* run
individual parts of the build system manually, if you choose. This may be
beneficial when you're making changes, and you wish to test a specific part of
lbmk.

Want to modify Libreboot?
-------------------------

Check the [lbmk maintenance manual](../maintain/) for guidance. You may for
example want to modify a config, e.g.:

	./mk -m coreboot x200_8mb

Or perhaps add a new board! The maintenance manual will teach you how the
Libreboot build system (lbmk) works!
