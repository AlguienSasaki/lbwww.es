---
title: Compile Libreboot from source
x-toc-enable: true
...

If you need to build Libreboot from source, this guide is for you.

Introduction
------------

Libreboot's build system is named `lbmk`, short for `LibreBoot MaKe`, and this
document describes how to use it. With this guide, you can know how to compile
libreboot from the available source code.

The following document describes how `lbmk` works, and how you can make changes
to it: [libreboot maintenance manual](../maintain/)

### WARNING: eCryptfs file name limits

Do not run the build system on a eCryptfs file system, because it has
very short file name limits and Libreboot's build system deals with very
long file names. We commonly get reports from this by Linux Mint users
who encrypt their home directory with eCryptfs; regular LUKS encryption will
do nicely.

System requirements
-------------------

You must ensure that you have the correct operating system, CPU, RAM, disk space
and so on.

System requirements are documented in
the [lbmk maintenance manual](../maintain/#system-requirements).

Multi-threaded builds
---------------------

Libreboot's build system defaults to a single build thread, but you can change
it by doing e.g.

	export XBMK_THREADS=4

This would make lbmk run on 4 threads.

More specifically: when compiling source trees via `script/trees`, `-jTHREADS`
is passed, where THREADS is the number of threads. This is also set when running
xz commands for compression, using the `-t` option.

Environmental variables
-----------------------

Please read about environmental variables in [the build
instructions](../maintain/), before running lbmk. You should set
your variables accordingly, though you do not technically need to; some
of them may be useful, e.g. `XBMK_THREADS` (sets the number of build threads).

Sources
-------

This version, if hosted live on libreboot.org, assumes that you are using
the `lbmk` git repository, which
you can download using the instructions on [the code review page](../../git.md).

Git
---

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

How to compile Libreboot
------------------------

Actual development/testing is always done using lbmk directly, and this
includes when building from source. Here are some instructions to get you
started:

### Zero..st, check time/date

Make sure date/hwclock report the correct time and date on your system,
because parts of the build process download from HTTPS servers and wrong
time or date can cause connections to be dropped during negotiation.

### First, install build dependencies

Libreboot includes a script that automatically installs build dependencies
according to the selected linux distro. 
The currently supported distros are: Debian/Ubuntu/Linux Mint/Pop!\_OS, 
Fedora, Arch Linux or Void Linux.

Some examples (run them as root, use e.g. `sudo`, `doas`):

	./mk dependencies ubuntu

or

	./mk dependencies debian

or

	./mk dependencies fedora41

or

	./mk dependencies arch

NOTE: For versioned files, such as `fedora41`, typically other versions will
be available too, e.g. `fedora38`. Make sure to check `config/dependencies/`,
so that you know whether or not a file is available for your distro.

NOTE: In case of Ubuntu 20.04 LTS or derived distros for that specific release,
use the dedicated configuration file:

	./mk dependencies ubuntu2004

Check: `config/dependencies/` for list of supported distros.

Technically, any Linux distribution can be used to build libreboot.
However, you will have to write your own script for installing build
dependencies. 

### Debian Trixie/Sid

Debian Trixie, the testing release as of 3 January 2025, and Debian Sid,
provide `gnat` and `gcc` as you expect, but `gnat` resolves to `gnat-13` and
installs `gcc-13` as a dependency, while `gcc` resolves to `gcc-14` and other
toolchain components correspond to this version.

The GCC/GNAT versions need to match during build time, so Libreboot's build
system hacks the `PATH` environmental variable, setting up symlinks, matching
GNAT to GCC or GNAT to GCC. When you run `./mk dependencies debian`, you get
GNAT 13 and GCC 14. This seems to make most boards compile; in our testing, the
KGPE-D16 board failed to compile in this configuration. This PATH hack is only
done for compiling the coreboot crossgcc toolchain, and nothing else; after that,
coreboot's toolchain is used.

GNAT is used by coreboot, because some of the Intel graphics devices are
initialised natively, with code written in Ada spark (called `libgfxinit`).

When updating from Debian stable to Debian Trixie(testing) or Sid, you should
also check for orphaned packages, using `aptitude search '~o'`. Do this,
removing what was leftover from the old release, and make sure to re-run the
Debian dependencies script, but do it like this:

	./mk dependencies debian --reinstall

For better reliability, you should, after running the dependencies script,
remove `gnat` and install `gnat-14` instead, which is available on this day
of 3 December 2025, but currently marked experimental. When you install
GNAT 14, GNAT 13 is removed but `gnat` (in `PATH`) still won't resolve to
anything. Libreboot *still* accommodates this, detecting and matching the GCC
and GNAT versions, which would in this instance match version 14 between them,
so that `gnat` and `gcc` are both in PATH at build time, resolving to v14.x.
When we tested with this configuration, the KGPE-D16 images also compiled.

NOTE: Ubuntu 24.10 also has the issue described above. Some other distros may
also have it, if they're based on Debian Testing/Sid or Ubuntu 24.10.

### MIPS cross compiler

Libreboot has support for the Sony PlayStation (PS1/PSX), based on
the PCSX-Redux Open BIOS. If you're doing a full release build, and/or
specifically building the PSX BIOS, you need a MIPS cross compiler.

Arch-based systems have a mipsel cross compiler available from AUR, and most
Debian-based systems have a mipsel cross compiler in apt; for these, the normal
dependencies installation command will provide them. We know Void Linux and
Fedora don't have a MIPS compiler, for instance.

If your distro doesn't have the MIPS compiler available,
the [PlayStation](../install/playstation.md) page provides instructions for
manual installation; please do this in addition to the normal dependencies.

### Next, build ROM images

Libreboot MaKe (lbmk) automatically runs all necessary commands; for
example, `./mk -b coreboot` will automatically build the required payloads
if not already compiled.

As a result, you can now (after installing the correct build dependencies) run
just a single command, from a fresh Git clone, to build all ROM images:

	./mk -b coreboot

To get a list of targets, do:

	./mk -b coreboot list

Then from the target list, you can pick one, for example:

	./mk -b coreboot lenovo_thinkpad_x60_2mb_bios

### Or maybe just build payloads?

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

	./mk -m coreboot lenovo_thinkpad_x200_8mb_bios

Or perhaps add a new board! The maintenance manual will teach you how the
Libreboot build system (lbmk) works!

A note about documentation (and this page)
-------------------------------

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



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/build/>
