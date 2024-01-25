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

The scripts under directory `script/vendor/` are the ones that do this.
Specifically, `script/vendor/download`. The benefit of this is that you
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
will only be *executed* if you actually have the Nvidia *GPU* on your board.

**ALSO: Libreboot 20230625 did not support Nvidia models at all (only Intel).
In releases, only Libreboot 20231021 and newer will support it.**

Pre-built ROM images from release archives since 20231021 will *not* contain
this file by default, but it will be present if you compiled directly from lbmk.
To remove it, do this:

	./cbutils/default/cbfstool libreboot.rom remove -n pci10de,06eb.rom

The cbfstool binary was compiled when your image (target `e6400_4mb`) was
compiled, which can alter the file system within your coreboot image.

Obviously, removing this VGA ROM will mean that the ROM image will only work
correctly on Intel GPU variants of E6400, but you probably knew that already!

(If you're using a release archive instead, you can re-insert the VGA ROM by
following [these instructions](../install/ivy_has_common.md))
