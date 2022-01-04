---
title: lbmk maintenance manual
x-toc-enable: true
...

Automated freedom
=================

This manual describes the nature of `lbmk` (LibreBoot MaKe), the automated
build system used to produce Libreboot releases. It is intended as a reference
for *libreboot development*.

If you simply wish to compile Libreboot from source, you should instead refer
to the [build instructions](../build/)

This documentation covers *modern* Libreboot; version 20160907 and below use a
much older, less polished version, from before it was actually
called `lbmk` (it was simply called "the libreboot build system" back then).
Information about those build systems are provided in the documentation
provided with those releases.

Generally speaking, *testing* releases of Libreboot do not come with
documentation; if you're using *old* testing releases, it is prudent to check
the `lbwww.git` repository on a revision from around the same time as those
releases. Future stable releases of Libreboot will come with a snapshot of
the `lbwww.git` repository, for documentation pertaining to such releases. One
way to do this, for releases from 2021 onwards, is to simply run `git log` on
the `news/` section of `lbwww.git` and find the revision that added
the *announcement* for a given release (again, 2021 onwards), and then you can
just reset to that revision.

As such, you should always refer to the *live* version of this page, on
libreboot.org, when working on the `lbmk.git` repository; the live version is
intended for development on the Git repository!

Libreboot blob policy
=====================

Libreboot has a strict policy of *excluding* non-free software. It is to only
distribute *free software*. Please keep this in mind, when you work on the
Libreboot build system, if you will later submit patches to the project.

[Learn more about Libreboot's policies](../../news/policy.md)

osbmk
=====

Another project, named osboot, is also maintained by Leah Rowe, forked from
Libreboot: <https://osboot.org/> - this project is just the same as Libreboot,
with the same build system, except for some tweaks: *all* blobs are allowed,
and CPU microcode updates are enabled by default, and the build system is
modified accordingly, but mostly the same.

The libreboot build system is `lbmk`. The osboot build system is `osbmk`.
These two build systems are almost identical, except for a few differences.
They are both actively maintained, in parallel, and lead/founded by Leah Rowe.

Why bring up osboot? Because it's relevant for licensing and compliance, in
case of audit in the future.

The `osboot` project was forked from Libreboot 20160907's build system (which
did not have a name back then), but with Libreboot documentation from December
in 2020. It was then expanded upon, fixing many flaws in the 20160907 build
system. At that time, a failed experimental re-write of Libreboot's build
system had to either be revived and succeed (but that build system was very
badly designed), or scrapped; the latter was decided, and osboot-libre was
born, which was used to create `lbmk`. With this act, the Libreboot project,
formerly a *dead project* for all intents and purposes, was completely restored.
All of these acts took place during the early months of the year 2021; the
failed Libreboot re-write took place after the Libreboot 20160907 release, and
was scrapped during March of 2021, in favour of lbmk which is a fork of osbmk.
(and lbmk is now, as of 2 January 2021, being re-forked to bring osbmk/lbmk
back into feature parity. so you can fork your forks of your forks, and
maybe fork the fork of your fork of your fork)

Libreboot and osboot are two sides of a coin. Libreboot is the "light", and
osboot is the *dark side*; osboot scrapts Libreboot's
[zero blobs policy](../../policy.md) and it is targeted at those who don't
want to (or can't) use Libreboot, but who still want some freedoms compared
to otherwise fully non-free vendor firmware.

What is lbmk?
=============

In the same way that Trisquel and Debian are GNU+Linux distributions, Libreboot
is a **coreboot distribution**. The `lbmk` build system *is* that distribution,
providing the glue necessary to integrate coreboot plus anything else that's
needed, unifying everything in a completely automated and pre-configured
fashion, so as to provide a distribution that is ease to install and use by
non-technical users.

In the past, installation of coreboot **required** extensive amounts of
configuration by the user, because there was no automation available. It was a
problem, and one that `lbmk` has *solved*; it is a problem, because most users
simply want to *install* coreboot without giving it much thought. The `lbmk`
build system is written for *those* people, while also providing some
flexibility for those who do want to tinker and get their hands dirty.

The `lbmk` build system is designed to be simple. Each part of it is its own
separate program, which is to run independently. *Write one program that does
one thing well*.

Technically, `lbmk` isn't necessarily a build system, but rather, a handful of
small scripts that run other scripts, or even C programs if you wish. What
makes `lbmk` *be* `lbmk` is what each individual script does, and how scripts
interact with or call each other to produce working ROM images. It takes
a *light touch* approach, providing only the most minimal glue necessary to
build working ROM images that the user can install, with sane defaults, while
also providing some ability to customize the firmware, with documentation
describing how to do just that. User-friendly documentation is provided, with
simple installation steps, automating as much of it as possible.

*This* document is different. The document you're reading right now is written
for *technical* users who want to know how Libreboot is put together.

The lbmk design also helps to ease copyright licensing and compliance, because
each part of lbmk is literally its own separate program. With this design, it
means that most scripts do not directly link/embed/include each other. Because
of this, it's much easier to have different licenses in use for different
files. Generally speaking, lbmk is GNU GPLv3+, but it's perfectly OK, for
example, to add files that are GPLv2 or other licenses. By comparison, if you
were to have a C program under GPLv3, you could not \#include C libraries that
are GPLv2, at least not directly, or there would be many pitfalls to avoid at
the very least. With lbmk's design, you can think of it as like when you have
many programs running in your operating system, and not all of those programs
are under the same license, and most of those different licenses are not
compatible with each other; this is perfectly OK there, and it's OK here too.

The purpose of this document is to (hopefully) cause you to understand the
entire build system in Libreboot, so that you can contribute patches or
otherwise make whatever changes you like. As such, this is a reference guide
for Libreboot development.

Libreboot is a *coreboot distro*, focusing on integration. As such, direct
development on software such as coreboot, GNU GRUB, SeaBIOS etc should ideally
be done upstream, or if it's a project hosted by Libreboot (such as ich9utils)
developed in the corresponding separate repository.

This document is written for developers and power users alike, or otherwise for
anyone who is curious enough to learn more about what *makes* Libreboot!

A major planned addition to lbmk in the future is: use it to implement a small
busybox+linux distribution, with musl libc, plus u-root, and implement a
linux-based bootloader setup similar to Heads, but do it *lbmk-style*. The
lbmk build system is designed for absolute simplicity and modularity, making
it easy to understand and maintain. It intentionally avoids use of rather
complicated programs such as GNU Autoconf; the Makefile in lbmk is just bolted
on but it not required. The `lbmk` build system is a *non-design*; it evolved
over time, into what it is today. Its modularity and simplicity of non-design
allows you to easily rewrite large parts of it, whenever you want to do so.

lbmk is largely written in GNU BASH, and this is unlikely to change in the
future. However, lbmk integrates several projects such as coreboot, GNU GRUB
or SeaBIOS, and these all have *their* own build systems aswell. The `lbmk`
build system is the glue that puts all of these together to produce ROM images
for users, in a completely automated fashion. The purpose of `lbmk` is to
provide an *unattended* build process, with as little user interaction as
possible. Thus, `lbmk` is an *automated build system*. It says on the Libreboot
home page that Libreboot is a *coreboot distribution* in much the same way that
Trisquel is a *GNU+Linux distribution*, and `lbmk` is what implements that!

Continue reading, and you will learn of each file contained in `lbmk`. This
document largely pertains to the version of `lbmk` as hosted in `lbmk.git`, but
this manual also covers source code archives containing the full downloaded
set of modules such as coreboot and GRUB.

In general, it is advisable to open *every* file in lbmk, after you downloaded
it (from the Git repository), and study the logic in great detail. This manual
attempts to explain all of it, and provide a general idea, but nothing beats
simply *studying* the logic directly.

AUTOMATED automation
====================

Every part of lbmk checks if the prerequisite steps are done, and performs them
automatically if not. The `roms_helper` script is no different; for example, it
automatically downloads coreboot if not present, aswell as GRUB and everything
else. You can run each and every part of lbmk without having to worry about
running something before it, because it is handled automatically; if that is
ever not the case, it's a bug that should be fixed immediately (in Libreboot
20160907, such fine tuned automation did not exist and you did have to run
specific parts of the build system manually, in a precise order, but this is
no longer the case in `lbmk` for Libreboot from 2021 onwards).

Another example: if you run `./build payload grub` but `./build module grub` is
not completed, it will automatically run that first, to produce
the `grub-mkstandalone` binary which is then used by `./build payload grub`

Another example: if you run `./build boot roms` and crossgcc isn't yet built
for the revision used on each given board, it will automatically compile that
version of it, using *that* coreboot tree's own build system to do it.

This level of automation means that `lbmk` from 2021 onwards is much easier to
use, compared to the build system present in Libreboot 20160907. Massive
improvements to that build system were made, during most of 2021, when
implementing both the `osbmk` *and* `lbmk` build systems.

All sections below pertain to actual files in lbmk:

COPYING
=======

This file contains a copy of the GNU General Public License, version 3.0. It is
the license that most parts of `lbmk` are released under.

Makefile
========

For use with GNU Make, this is a frontend to `lbmk`, which can be used to run
various commands in `lbmk`.

Use of this file is purely optional, and largely beneficial if you simply want
to build all of `lbmk` (just run `make` when the current work directory is the
root directory of `lbmk`).

README.md
=========

This file contains a brief description of Libreboot, along with information
about the project

build
=====

This is the main BASH script, part of `lbmk`, used for running most `lbmk`
commands. You could say that this file *is* `lbmk`. Run `./build help` for
usage instructions.

It calls scripts in `resources/scripts/build/`. For example, the
command `./build boot roms` will execute `resources/scripts/build/boot/roms`.
When running such commands, additional parameters can be given, which will
be passed along to the corresponding script. For example, try:

    ./build boot roms x60 x200_8mb w500_16mb

This will run:

    ./resources/scripts/build/boot/roms x60 x200_8mb w500_16mB

The `list` function is very helpful. For example:

    ./build boot list

At the time of writing this section, this would have outputted:

    Available options for mode 'boot':
    
    roms
    roms_helper

Another use of `list` would be:

    ./build boot roms list

However, the `roms` script merely happens to implement a `list` command. For
example, `./build payload grub list` does nothing differently
than `./build payload grub`.

You may also refer to the [build instructions](../build)

download
========

This is the main BASH script for downloading various components used by `lbmk`.
For example, this script downloads coreboot. Scripts called by `download` may
also apply patches and such, to the corresponding project; for example, it will
apply custom patches to GNU GRUB.

This runs scripts in `resources/scripts/download`. For example:

    ./download coreboot

This would run:

    ./resources/scripts/download/coreboot

Additional parameters can be given, for example:

    ./download coreboot default

This would run:

    ./resources/scripts/download/coreboot default

For a full list of all `download` commands, run:

    ./download help

modify
======

This can be used to modify SeaBIOS and coreboot configs. It calls scripts
in `resources/scripts/modify/`, for example:

    ./modify coreboot configs

This runs:

    ./resources/scripts/modify/coreboot/configs

Additional parameters can be given, for example:

    ./modify coreboot configs x200_8mb x60

This would run:

    ./resources/scripts/modify/coreboot/configs x200_8mb x60

projectname
===========

This file contains a single line of text, with the string "libreboot".

This file exists because of [osboot](https://osboot.org/) existing, which uses
a modified version of `lbmk`. Leah Rowe, the founder of Libreboot, is also the
founder of osboot and actively maintains both projects. A lot of scripts
in `lbmk` make use of the `projectname` file.

If you were to fork Libreboot, you could very easily just modify this file, so
as to rename your fork in a largely automated way.

resources/coreboot/
===================

This directory contains configuration, patches and so on, for each mainboard
supported in the `lbmk` build system. These directories contain such
configuration, so that `lbmk` can build working ROM images.

The scripts in `resources/scripts/build/boot/` make heavy use of this
directory.

resources/coreboot/BOARDNAME/
=============================

Each `BOARDNAME` directory defines configuration for a corresponding mainboard.
It doesn't actually have to be for a board; it can also be used to just define
a coreboot revision, with patches and so on.

resources/coreboot/BOARDNAME/board.cfg
======================================

This file can contain several configuration lines, each being a string, such
as:

* `cbtree="default"` (example entry)
* `romtype="normal"` (example entry)
* `cbrevision="ad983eeec76ecdb2aff4fb47baeee95ade012225"` (example entry)
* `arch="x86_64"` (example entry)
* `payload_grub="y"` (example entry)
* `payload_grub_withseabios="y"` (example entry)
* `payload_seabios="y"` (example entry)
* `payload_memtest="y"` (example entry)
* `payload_seabios_withgrub="y"` (example entry)
* `grub_scan_disk="ata"`

More information about these and other variables will be provided throughout
this document.

The `cbtree` entry is actually a link, where its value is a directory name
under `resources/coreboot`. For example, `cbtree="default"` would refer
to `resources/coreboot/default` and the corresponding coreboot source tree
created (when running `./download coreboot`, which makes use of `board.cfg`)
would be `coreboot/default/`. In other words: a `board.cfg` file
in `resources/coreboot/foo` might refer to `resources/coreboot/bar` by
specifying `cbtree="bar"`, and the created coreboot source tree would
be `coreboot/bar/`. ALSO:

FUN FACT: such references are infinitely checked until resolved. For
example, `foo` can refer to `bar` and `bar` can refer to `baz` but if there is
an infinite loop, this is detected and handled by `lbmk`. For example,
if `bar` refers to `foo` which refers back to `bar`, this is not permitted
and will throw an error in `lbmk`.

The `romtype` entry largely defines what `./build boot roms` does once the ROM
is built; for example, `romtype="4MiB ICH9 IFD NOR flash"` would specify that
an Intel Flash Descriptor for ICH9M, generated by `ich9gen`, would have to be
inserted.

The `cbrevision` entry defines which coreboot revision to use, from the
coreboot Git repository. *At present, lbmk only supports use of the official
repository from the upstream coreboot project*.

The `arch` entry specifies which CPU architecture is to be used: currently
recognized entries are `x86_32`, `x86_64` and `ARMv7`. *At present, setting it
to ARMv7 only means that crossgcc-arm will be compiled, but no support for
actually building ROMs exists in lbmk exists yet, except for 32-bit and 64-bit
x86 machines.*

The `payload_grub` entry specifies whether or not GNU GRUB is to be included in
ROM images.

The `payload_grub_withseabios` entry specifies whether or not SeaBIOS is to
be included *with* GRUB, in ROM images. Turning this on also turns
on `payload_seabios_withgrub`, unless that option is explicitly turned off.

The `payload_seabios` entry specifies whether or not SeaBIOS is to be included
in ROM images. This option is *automatically* enabled
if `payload_grub_withseabios` and/or `payload_seabios_withgrub` are also turned
on.

The `payload_memtest` entry specifies whether or not MemTest86+ is to be
included in ROM images; it will only be included in ROM images for *text mode*
startup, on x86 machines.

The `grub_scan_disk` option specifies can be `ahci`, `ata` or `both`, and it
determines which types of disks are to be scanned, when the `grub.cfg` file in
GRUB payloads tries to automatically find other `grub.cfg` files supplied by
your GNU+Linux distribution. On some machines, setting it to `ata` or `ahci`
can improve boot speed by reducing delays; for example, trying to scan `ata0`
on a ThinkPad X60 with the optical drive may cause GRUB to hang, so on that
machine it is advisable to set this option to `ahci` (becuse the default HDD
slot is AHCI).

resources/coreboot/BOARDNAME/config/\*
======================================

Files in this directory are *coreboot* configuration files. 

Configuration file names can be as follows:

* `libgfxinit_corebootfb`
* `libgfxinit_txtmode`
* `vgarom_vesafb`
* `vgarom_txtmode`
* `normal`

Information pertaining to this can be found on
the [installation manual](../install/)

In `lbmk`, a board-specific directory under `resources/coreboot/` should never
specify a coreboot revision. Rather, a directory *without* coreboot configs
should be created, specifying a coreboot revision. For example, the
directory `resources/coreboot/default/` specifies a coreboot revision. In the
board-specific directory, your `board.cfg` could then
specify `cbtree="default"` but without specifying a coreboot revision (this
is specified by `resources/coreboot/default/board.cfg`).

When you create a coreboot configuration, you should set the payload to *none*
because `lbmk` itself will assume that is the case, and insert payloads itself.

Configurations with `libgfxinit` will use coreboot's native graphics init code
if available on that board. If the file name has `txtmode` in it, coreboot
will be configured to start in *text mode*, when setting up the display. If
the file name has `corebootfb` in it, coreboot will be configured to set up a
high resolution *frame buffer*, when initializing the display.

NOTE: If the configuration file is `libgfxinit_txtmode`, the SeaBIOS payload
can still run *external* VGA option ROMs on graphics cards, and this is the
recommended setup (SeaBIOS in text mode) if you have a board with both onboard
and an add-on graphics card (e.g. PCI express slot) installed.

Configuration files with `vgarom` in the name have coreboot itself configured
to run VGA option ROMs (and perhaps other option ROMs). *This* setup is not
strictly recommended for *SeaBIOS*, and it is recommended that you only run
GRUB in this setup. As such, if you wish for a board to have coreboot initialize
the VGA ROM (on an add-on graphics card, as opposed to onboard chipset), you
should have a *separate* directory just for that, under `resources/coreboot/`;
another directory for that board will have configs with `libgfxinit`. HOWEVER:

It *is* supported in lbmk to have SeaBIOS used, on either setup. In the
directory `resources/seabios/` there are SeaBIOS configs for both; the vgarom
one sets VGA hardware type to *none* while the libgfxinit one sets it
to *coreboot linear framebuffer*. However, if you use SeaBIOS on a setup with
coreboot also doing option ROM initialization, such initialization is being
performed *twice*. As such, if you want to use an add-on graphics card in
SeaBIOS, but the board has libgfxinit, it is recommended that you do it from
a `libgfxinit` ROM.

HOWEVER: there's no hard and fast rule. For example, you could make a vgarom
configuration, on a board in lbmk, but in its coreboot configuration, don't
enable native init *or* oproms, and do SeaBIOS-only on that board.

On `vgarom` setups, coreboot can be configured to start with a high resolution
VESA frame buffer (NOT to be confused with the coreboot frame buffer), or just
normal text mode. Text mode startup is always recommended, and in that setup,
GRUB (including coreboot GRUB, but also PC GRUB) can use VGA modes.

The name `libgfxinit` is simply what `./build boot roms` uses, but it may be
that a board uses the old-school native video init code written in C. On some
platforms, coreboot implemented a 3rd party library called `libgfxinit`, which
is written in Ada and handles video initialization. In this setup, coreboot
*itself* should *never* be configured to run any option ROMs, whether you
start in text mode or with the coreboot framebuffer initialization.

The `normal` config type is for desktop boards that lack onboard graphics
chipsets, where you would always use an add-on graphics card (or *no* graphics
card, which would be perfectly OK on servers).

Even if your board doesn't actually use `libgfxinit`, the config for it should
still be named as such. From a user's perspective, it really makes no
difference.

COREBOOT build system
---------------------

If you wish to know about coreboot, refer here:\
<https://doc.coreboot.org/tutorial/part1.html>

This and other documents from coreboot shall help you to understand *coreboot*.

You create a config, for `resources/coreboot/BOARDNAME/configs`, by running
the `make menuconfig` command in the *coreboot* build system. You should do
this after running `./download coreboot` in lbmk.

You can simply clone coreboot upstream, add whatever patches you want, and
then you can make your config. It will appear afterwards in a file
named `.config` which is your config for inside `resources/coreboot/BOARDNAME/`.

You can then use `git format-patch -nX` where `X` is however many patches you
added to that coreboot tree. You can put them in the patches directory
under `resources/coreboot/BOARDNAME`.

The *base* revision, upon which any custom patches you wrote are applied,
shall be the `cbrevision` entry.

REMINDER: Do not enable a payload in coreboot's build system. Set it
to *none*, and enable whatever payload you want in lbmk.

If a payload is not supported in lbmk, patches are very much welcome! It is
the policy of Libreboot, to only ever use the *coreboot* build system inside
coreboot, but not use any of *coreboot's* own integration for payloads. It is
far more flexible and *robust* to handle payloads externally, relative to the
coreboot build system.

Scripts exist in `lbmk` for automating the modification/updating of *existing*
configs, but not for adding them. Adding them is to be done manually, based on
the above guidance.

ALSO:

If the option exists, for a given board, please configure coreboot to clear
all DRAM upon boot. This is for security reasons. An exception is made when
such functionality is not available, on the specific board/revision that you're
configuring in coreboot.

resources/coreboot/DEFAULT/blobs.list
=====================================

For directories in `resources/coreboot/` that specify `cbrevision`,
a `blobs.list` file can be included. When running `./download coreboot`, lbmk
will delete whatever files are listed in `blobs.list` for that coreboot tree.

When downloading coreboot, lbmk checks out coreboot 3rdparty submodules, but
only does `git submodule update --init`; on coreboot's side, it is set up so
that this doesn't download most of the non-free software that coreboot
distributes (for that, you run `git submodule --init --checkout` (you'll note
that the `--checkout` option is included).

However, some binary blobs still remain even when only doing `--init`. These
are discovered, whenever a new coreboot revision is added to lbmk, by running
the *linux-libre* deblob script on the coreboot source tree, *after* doing
the `git submodule update --init` command.

See `deblob-check` from the fsfla website:\
<https://www.fsfla.org/ikiwiki/selibre/linux-libre/>

The `deblob-check` script in fact *does* work quite well on the coreboot
source tree! However, coreboot is far simpler than the Linux kernel, and much
more conservative in its general scope, that the script was never actually
forked specifically for Libreboot. Simply speaking, the way deblobbing is
handled in Libreboot is as follows:

* Copy the `blobs.list` from the *last* deblobbed coreboot revision
* Run `deblob-check` on the *new* coreboot revision
* Run `deblob-check` on the *last* deblobbed coreboot revision
* Diff the results
* Any file that was deleted on coreboot side, remove from the new `blobs.list`
* Any new files get added to the new `blobs.list`

Doing it manually, and in such a crude fashion as this, is perfectly acceptable
because coreboot makes a good habit of always separating binary code blobs into
completely *separate* files.

There is some nuance in exactly how Libreboot handles binary blobs. As far as
Libreboot is concerned, only *software* is deleted if a blob. Non-software
blobs are retained, so long as they are in a well-understood format or are
otherwise trivial. Of course, such data must not be under a non-free license!
On the other hand, blobs such as CPU microcode are always to be deleted.

For example: DDR training data is retained. These are data patterns used for
memory controller initialization, specifically during *training* (bruteforcing
the precise timings required at boot time).

More nuance: lbmk does *not* disable any code for *loading* blobs, but rather,
it only deletes the actual blobs. For example, you can still add CPU microcode
updates to Libreboot ROM images, and libreboot's version of coreboot will still
use them, if present. This has always been the case. Libreboot will never try
to prevent you from running blobs; it merely does not *include* them. This is
for the sake of efficiency, because deblobbing is actually only a very minor
aspect of what Libreboot is, and time is better spent on other areas of
development. Deblobbing is done in the most low-effort way possible, just so as
to comply with the *GNU Free System Distribution Guidelines*.

Of course, deleting blobs from coreboot *breaks* coreboot, in situations where
you actually want to build for a board where those blobs are used, but since
those boards are not to be supported in lbmk anyway, it's moot (the boards that
lbmk does support will all boot just fine, because all of the required files
exist, and are free).

resources/coreboot/BOARDNAME/patches/\*
=======================================

In cases where `cbrevision` is specified, where the given directory
under `resources/coreboot/` does in fact define a version of coreboot to
download, you can add custom *patches* on top of that revision. When you run
the command `./download coreboot`, those patches will be applied chronologically
in alphanumerical order as per patch file names.

The patch files should be named with `.patch` file extensions. All other files
will be ignored. By having `lbmk` do it this way, you could add a `README` file
for instance, and `lbmk` will not erroneously try to apply `README` as though
it were a patch file. This might be useful if you have a *lot* of patches, and
you want to provide some explanations about specific files.

FUN FACT: If you run `NODELETE= ./download coreboot`, lbmk will *skip* deleting
blobs, and also skip deleting the `.git` files and directories in those
coreboot clones. By default, the Git history is deleted, because it contains
blobs. However, you may want to make changes and then create a patch
using `git format-patch`, and you can do just that! Afterwards, you would
simply delete the blobs manually and delete the Git history (or you could just
run `./download coreboot` again, without `NODELETE`).

resources/grub/background/
==========================

Splash screen images applied duing startup when using the GNU GRUB payload.

resources/grub/config/grub.cfg
==============================

This is a configuration file. It is used to program GRUB's shell.

This is inserted (as `grub.cfg`) into the root of CBFS, in the ROM image. It
contains a lot of logic in it, for booting various system configurations, when
the GRUB payload is in use.

resources/grub/config/grub\_memdisk.cfg
========================================

This is a configuration file. It is used to program GRUB's shell.

This file is inserted (as `grub.cfg`) into the GRUB *memdisk*, when building
the GRUB payload (for coreboot), using GRUB's `grub-mkstandalone` utility. It
simply loads the `grub.cfg` file from CBFS (see above).

resources/grub/keymap/
======================

This directory contains keymaps for GRUB. They allow for different keyboard
layouts to be used. The `lbmk` build system uses these to produce ROM images
with various keyboard layouts used by default, when the GRUB payload is to be
used.

They are stored here, directly in GRUB's own `.gkb` file format, which is a
binary format defining which scancodes correspond to which character input.

This binary format is documented by GRUB; the code for it is easy to
understand. Please read `grub-core/commands/keylayouts.c` in the GRUB source
code.

resources/grub/modules.list
===========================

This file defines all modules that are to be included in builds of GNU GRUB.
They are standalone builds, created using the `grub-mkstandalone` utility.

resources/grub/patches/
=======================

This directory contains custom patches for GNU GRUB.

resources/memtest86plus/patch/
==============================

This directory contains custom patches for Memtest86+.

resources/scripts/build/boot/roms
=================================

This script builds coreboot ROM images. It is largely a shim, which calls
the `roms_helper` script, which does most of the legwork.

Command: `./build boot roms`

Additional parameters can be provided. This lists all boards available:

    ./build boot roms list

Pass several board names if you wish to build only for specific targets. For
example:

    ./build boot roms x60 x200_8mb

resources/scripts/build/boot/roms\_helper
=========================================

This script builds coreboot ROM images. It is not to be executed directory;
user interaction must be done via the main `roms` script.

It heavily makes use of the `board.cfg` file, for a given board. This script
will *only* operate on a single target, from a directory
in `resources/coreboot/`.

If `grub_scan_disk` is set, it sets that in the `grub.cfg` file that is to be
inserted into a ROM image, when `payload_grub` is turned on.

It automatically detects if `crossgcc` is to be compiled, on a given coreboot
tree (in cases where it has not yet been compiled), and compiles it for a
target based on the `arch` entry in `board.cfg`.

It creates ROM images with GRUB and/or SeaBIOS, optionally with Memtest86+
also included, in various separate configurations in many different ROM images
for user installation.

The `romtype` entry in `board.cfg` tells this script what to do with the ROM,
after it has been built. Currently, it operates based on these possible values
for `romtype`:

* `4MiB IFD BIOS region` will cause only the upper 4MB section of the ROM
  to be included in a release. This option is largely deprecated, a hangover
  from osboot, which also no longer uses this option on any boards, and it is
  thus subject for removal.
* `d8d16sas` will cause *fake* (empty) files named `pci1000,0072.rom`
  and `pci1000,3050.rom` to be inserted in CBFS. This prevents SeaBIOS from
  loading or executing the option ROM stored on PIKE2008 modules, present on
  certain configurations with the ASUS KCMA-D8 or KGPE-D16 mainboards. Those
  option ROMs cause the system to hang, so they should never be executed (this
  means however that booting Linux kernels from SAS devices is impossible on
  those boards, unless a Linux payload is used; Linux can use those SAS drives,
  without relying on the PIKE2008 option ROMs). When SeaBIOS runs, it will
  default to loading the corresponding option ROM from CBFS, if it exists, for
  a given PCI device, overriding whatever option ROM is present on the device
  itself, but if the option ROM is invalid/empty, SeaBIOS will not attempt to
  load another one, until the empty/invalid one (in CBFS) is deleted.
* `4MiB ICH9 IFD NOR flash`: the `ich9gen` program will be used to insert an
  Intel Flash Descriptor and Gigabit Ethernet Non-volatile memory file into
  the ROM image. This is used on GM45/ICH9M based laptops, such as: ThinkPad
  X200, T400, T500, W500, X200 Tablet, X200S, T400S, X301
* `8MiB ICH9 IFD NOR flash`: Same as the 4MB one as described above, but for
  ROM images with 8MB (64Mbit) of boot flash. The one above is for systems
  with 4MB (32Mbit) of flash.
* `16MiB ICH9 IFD NOR flash`: ditto, but for 16MB (128Mbit) flash. In this and
  the other two cases as described above, the first 4KB is the Intel Flash
  Descriptor, the next 8KB is GbE NVM and the rest is BIOS (for the coreboot
  part). In all cases, the default *ME* (Intel Management Engine) region is
  disabled, as is the ME itself, based on bits set to disable it in the Intel
  Flash Descriptor. The descriptor is used in such a setup, because on all
  such boards in Libreboot, GbE NVM is needed to get gigabit ethernet working
  correctly; it is the sole reason `ich9gen` was written, because it is
  otherwise possible to boot these machines in a *descriptorless* setup, where
  ICH9M behaves similarly to ICH7: all one region of flash, for the boot
  firmware (coreboot), but it results in a non-functional gigabit enternet
  device.
* `4MiB ICH9 IFD NOGBE NOR flash`: Intel Flash Descriptor *on its own*, without
  ME or GbE NVM. Just IFD and BIOS. This is used on the ThinkPad R500.
* `8MiB ICH9 IFD NOGBE NOR flash`: Same as above, but for 8MB (64Mbit) ROMs
* `16MiB ICH9 IFD NOGBE NOR flash`: Same as above, but for 16MB (128Mbit) ROMs
* `i945 laptop`: in this configuration, the upper 64KB section of the ROM is
  copied into the 64KB section below that. This results in there being two
  bootblocks in the ROM, and you can decide which one is used by setting `bucts`

If no payload is defined in `board.cfg`, the `roms_helper` script will exit
with error status.

If SeaBIOS is to be used, on `libgfxinit` setups, SeaVGABIOS will also be
inserted. This provides a minimal VGA compatibility layer on top of the
coreboot framebuffer, but does not allow for *switching* the VGA mode. It is
currently most useful for directly executing ISOLINUX/SYSLINUX bootloaders,
and certain OS software (some Windows setups might work, poorly, depending on
the board configuration, but don't hold your breath; it is far from complete).

If SeaBIOS is to be used, in `vgarom` setups or `normal` setups, SeaVGABIOS
is not inserted and you rely on either coreboot and/or SeaBIOS to execute VGA
option ROMs.

In all cases, this script automatically inserts several SeaBIOS runtime
configurations, such as: `etc/ps2-keyboard-spinup` set to 3000 (PS/2 spinup
wait time), `etc/pci-optionrom-exec` set to 2 (despite that already being
the default anyway) to enable *all* option ROMs, unless `vgarom` setups are
used, in which case the option is set to *0* (disabled) because coreboot is
then expected to handle option ROMs, and SeaBIOS should not do it.

Essentially, the `roms_helper` script makes use of each and every part of lbmk.
It is the heart of Libreboot.

When the ROM is finished compiling, it will appear under a directory in `bin/`

resources/scripts/build/clean/cbutils
=====================================

This simply runs `make clean` on various utilities from coreboot, which lbmk
makes use of.

Command: `./build clean cbutils`

resources/scripts/build/clean/crossgcc
======================================

This runs `make crossgcc-clean` on all of the coreboot revisions present in
lbmk.

Command: `./build clean crossgcc`

resources/scripts/build/clean/flashrom
======================================

This runs `make clean` in the `flashrom/` directory.

Command: `./build clean flashrom`

resources/scripts/build/clean/grub
==================================

This runs `make clean` in the `grub/` directory.

It does not delete anything in `payload/grub/`.

Command: `./build clean grub`

resources/scripts/build/clean/ich9utils
=======================================

This runs `make clean` in the `ich9utils/` directory.

Command: `./build clean ich9utils`

resources/scripts/build/clean/memtest86plus
===========================================

This runs `make clean` in the `memtest86plus/` directory.

Command: `./build clean memtest86plus`

resources/scripts/build/clean/payloads
======================================

This deletes the `payload/` directory.

Command: `./build clean payloads`

resources/scripts/build/clean/rom\_images
=========================================

This deletes the `bin/` directory.

Command: `./build clean rom_images`

resources/scripts/build/clean/seabios
=====================================

This runs `make clean` in the `seabios/` directory.

Command: `./build clean seabios`

resources/scripts/build/dependencies/arch
=========================================

Using `pacman`, this installs build dependencies in Arch. It may also work on
similar distros like Manjaro or Artix.

Command: `./build dependencies arch`

resources/scripts/build/dependencies/debian
===========================================

Using `apt-get`, this installs build dependencies in Debian. It may work on
other `apt-get` distros.

Command: `./build dependencies debian`

resources/scripts/build/dependencies/fedora35
=============================================

Using `dnf`, this installs build dependencies in Fedora 35.

Command: `./build dependencies fedora35`

resources/scripts/build/dependencies/ubuntu2004
===============================================

Using `apt-get`, this installs build dependencies for Ubuntu 20.04 (for later
versions, you might use the Debian script).

This script should also work with Trisquel 9 and 10.

Command: `./build dependencies ubuntu2004`

resources/scripts/build/dependencies/void
=========================================

Using `xbps`, this installs build dependencies for Void.

Command: `./build dependencies void`

resources/scripts/build/descriptors/ich9m
=========================================

This runs `ich9gen` to generate descriptors for ICH9M platforms. These are
then stored in `descriptors/ich9m/`

Command: `./build descriptors ich9m`

resources/scripts/build/module/cbutils
======================================

This compiles various coreboot utilities (such as cbfstool).

Command: `./build module cbutils`

resources/scripts/build/module/flashrom
=======================================

This compiles flashrom.

Command `./build module flashrom`

resources/scripts/build/module/grub
===================================

This compiles GRUB utilities. It does not build the actual payloads.

Command: `./build module grub`

resources/scripts/build/module/ich9utils
========================================

This compiles `ich9utils`, which includes the `ich9gen` utility.

Command: `./build module ich9utils`

resources/scripts/build/module/memtest86plus
============================================

This compiles Memtest86+.

Command: `./build module memtest86plus`

resources/scripts/build/payload/grub
====================================

This builds the GRUB payloads.

Command: `./build payload grub`

resources/scripts/build/payload/seabios
=======================================

This builds the SeaBIOS payloads.

Command: `./build payload seabios`

resources/scripts/build/release/roms
====================================

This builds release archives, containing ROM images. You must only run this
after you've built all of the ROM images that you wish to release.

Command: `./build release roms`

resources/scripts/build/release/src
===================================

This builds source archives. You must only run this after compiling crossgcc
on all coreboot source trees.

Command: `./build release src`

resources/scripts/download/coreboot
===================================

This downloads, patches and deblobs coreboot, as per `board.cfg` files
in `resources/coreboot/`.

Command: `./download coreboot`

resources/scripts/download/flashrom
===================================

This downloads and patches flashrom.

Command: `./download flashrom`

resources/scripts/download/grub
===============================

This downloads and patches GNU GRUB.

Command: `./download grub`

resources/scripts/download/ich9utils
====================================

This downloads `ich9utils`, which includes `ich9gen`.

Command: `./download ich9utils`

resources/scripts/download/memtest86plus
========================================

This downloads and patches Memtest86+.

Command: `./download memtest86plus`

resources/scripts/download/seabios
==================================

This downloads and patches SeaBIOS.

Command: `./download seabios`

resources/scripts/misc/versioncheck
===================================

This updates the text file containing version information. It is used by many
other build scripts. It also updates the files containing the version date.

You need not run this yourself, directly.

resources/scripts/modify/coreboot/configs
=========================================

Loads coreboot configs into coreboot trees, and runs `make menuconfig`, so
that you can easily modify them in an ncurses interface. Additional parameters
are accepted, for example:

    ./modify coreboot configs x60 x200_8mb

With no additional parameters given, it simply cycles through all configs
under `resources/coreboot/`.

Command: `./modify coreboot configs`

resources/scripts/modify/seabios/configs
========================================

This lets you modify SeaBIOS configs.

Command: `./modify seabios configs`

resources/scripts/update/coreboot/configs
=========================================

This runs `make oldconfig` on coreboot configs under `resources/coreboot/`.
It is most useful when updating a coreboot revision, per `board.cfg`. It allows
additional parameters, for example:

    ./update coreboot configs x60 x200_8mb

With no additional parameters given, it simply cycles through all configs
under `resources/coreboot/`.

Command: `./update coreboot configs`

resources/scripts/update/seabios/configs
========================================

This runs `make oldconfig` on SeaBIOS configs. It is most useful when updating
the version of SeaBIOS used by lbmk.

Command: `./update seabios configs`

resources/seabios/config/libgfxinit
===================================

SeaBIOS configuration file, when `libgfxinit` is to be used. It enables
the `coreboot linear framebuffer` option in the SeaBIOS `make menuconfig`
configuration interface.

resources/seabios/config/vgarom
===============================

This version is for normal SeaBIOS configurations, where `libgfxinit` is not
to be used.

update
======

This can be used to update SeaBIOS and coreboot configs. It calls scripts
in `resources/scripts/update/`, for example:

    ./update coreboot configs

This runs:

    ./resources/scripts/update/coreboot/configs

Additional parameters can be given, for example:

    ./update coreboot configs x200_8mb x60

This would run:

    ./resources/scripts/update/coreboot/configs x200_8mb x60

