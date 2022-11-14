---
title: Build from source
x-toc-enable: true
...

libreboot's build system is named `lbmk`, short for `Libreboot Make`, and this
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

GNU Make
========

libreboot Make includes a file called `Makefile`. You can still use
the `lbmk` build system directly, or you can use GNU Make. The `Makefile`
simply runs `lbmk` commands. However, using `osbmk` directly will offer you
much more flexibility; for example, the Makefile currently cannot build single
ROM images (it just builds all of them, for all boards).

You must ensure that all build dependencies are installed. If you're running
Ubuntu or similar distribution (Debian, Trisquel, etc) you can do this:

    sudo make install-dependencies-ubuntu

One exists specifically for Debian:

    sudo make install-dependencies-debian

Another exists for Arch:

    sudo make install-dependencies-arch

Now, simply build the coreboot images like so:

    make

This single command will build ROM images for *every* board integrated in
libreboot. If you only wish to build a limited set, you can use `lbmk` directly:

    ./build boot roms x200_8mb

You can specify more than one argument:

    ./build boot roms x200_8mb x60

ROM images appear under the newly created `bin/` directory in the build system.

For other commands, simply read the `Makefile` in your favourite text editor.
The `Makefile` is simple, because it merely runs `lbmk` commands, so it's very
easy to know what commands are available by simply reading it.

Standard `clean` command available (cleans all modules except `crossgcc`):

    make clean

To clean your `crossgcc` builds:

    make crossgcc-clean

To build release archives:

    make release

Build without using GNU Make
============================

The `Makefile` is included just for *compatibility*, so that someone who
instictively types `make` will get a result.

Actual development/testing is always done using `lbmk` directly, and this
includes when building from source. Here are some instructions to get you
started:

First, install build dependencies
---------------------------------

libreboot includes a script that automatically installs apt-get dependencies
in Ubuntu 20.04. It works well in other apt-get distros (such as Trisquel and
Debian):
    
    sudo ./build dependencies ubuntu2004

Separate scripts also exist:

    sudo ./build dependencies debian

    sudo ./build dependencies arch

    sudo ./build dependencies void

Technically, any GNU+Linux distribution can be used to build libreboot.
However, you will have to write your own script for installing build
dependencies. 

libreboot Make (lbmk) automatically runs all necessary commands; for example
`./build payload grub` will automatically run `./build module grub` if the
required utilities for GRUB are not built, to produce payloads.

As a result, you can now (after installing the correct build dependencies) run
just a single command, from a fresh Git clone, to build the ROM images:

    ./build boot roms

or even just build specific ROM images, e.g.:

    ./build boot roms x60

If you wish to build payloads, you can also do that. For example:

    ./build payload grub

    ./build payload seabios

Previous steps will be performed automatically. However, you can *still* run
individual parts of the build system manually, if you choose. This may be
beneficial when you're making changes, and you wish to test a specific part of
lbmk.

Therefore, if you only want to build ROM images, just do the above. Otherwise,
please continue reading!

Optional: extract binary blobs
------------------------------

Some boards, including all sandy/ivybridge boards require nonfree blobs which cannot be included in libreboot.
For boards requiring these blobs, libreboot will attempt to download the blobs itself.
If your board does not have blob sources available, then you must extract them from a backup of you vendor rom.
You must point libreboot to the backup rom and tell the build system which board you want to extract blobs for.
For example, to extract blobs for the t440p you must run:

	./blobutil extract t440p_12mb /path/to/12mb_backup.rom

You can then build the rom for this board as normal:

	./build boot roms t440p_12mb


Second, download all of the required software components
--------------------------------------------------------

If you didn't simply run `./build boot roms` (with or without extra
arguments), you can still perform the rest of the build process manually. Read
on! You can read about all available scripts in `lbmk` by reading
the [libreboot maintenance manual](../maintain/); lbmk is designed to be modular
which means that each script *can* be used on its own (if that's not true, for
any script, it's a bug that should be fixed).

It's as simple as that:

    ./download all

The above command downloads all modules defined in the libreboot build system.
However, you can download modules individually.

This command shows you the list of available modules:

    ./download list

Example of downloading an individual module:

    ./download coreboot

    ./download seabios

    ./download grub

    ./download flashrom

Third, build all of the modules:
--------------------------------

Building a module means that it needs to have already been downloaded.
Currently, the build system does not automatically do pre-requisite steps
such as this, so you must verify this yourself.

Again, very simple:

    ./build module all

This builds every module defined in the libreboot build system, but you can
build modules individually.

The following command lists available modules:

    ./build module list

Example of building specific modules:

    ./build module grub

    ./build module seabios

    ./build module flashrom

Commands are available to *clean* a module, which basically runs make-clean.
You can list these commands:

    ./build clean list

Clean all modules like so:

    ./build clean all

Example of cleaning specific modules:

    ./build clean grub

    ./build clean cbutils

Fourth, build all of the payloads:
---------------------------------

Very straight forward:

    ./build payload all

You can list available payloads like so:

    ./build payload list

Example of building specific payloads:

    ./build payload grub

    ./build payload seabios

The build-payload command is is a prerequsite for building ROM images.

Fifth, build the ROMs!
----------------------

Run this command:

    ./build boot roms

Each board has its own configuration in `lbmk` under `resources/coreboot/`
which specifies which payloads are supported.

By default, all ROM images are built, for all boards. If you wish to build just
a specific board, you can specify the board name based on the directory name
for it under `resources/coreboot/`. For example:

    ./build boot roms x60

Board names, like above, are the same as the directory names for each board,
under `resources/coreboot/` in the build system.

That's it!

If all went well, ROM images should be available to you under bin/
