---
title: lbmk maintenance manual
x-toc-enable: true
...

Automated pragmatism
====================

This manual describes the nature of `lbmk` (LibreBoot MaKe), the automated
build system used to produce libreboot releases. It is intended as a reference
for *libreboot development*.

If you simply wish to compile libreboot from source, you should instead refer
to the [build instructions](../build/)

Generally speaking, *testing* releases of libreboot will not come with
documentation; if you're later using *old* testing releases, it is prudent to
check the `lbwww.git` repository on a revision from around the same time as
those releases. Future stable releases of libreboot will come with a snapshot of
the `lbwww.git` repository, for documentation pertaining to such releases. One
way to do this, all testing releases of libreboot, will be to simply run `git log`
on the `news/` section of `lbwww.git` and find the revision that added
the *announcement* for a given release (when available), and then you can
just reset to that revision.

As such, you should always refer to the *live* version of this page, on
libreboot.org, when working on the `lbmk.git` repository; the live version is
intended for development on the Git repository!

libreboot blob reduction policy
============================

The coreboot software is nominally free, but it requires additional binary
blobs on many supported systems. These *blobs* lack source code, and the
coreboot project does not control them, but they can be used to perform
specific initialization tasks.

The libreboot project *allows* binary blobs from coreboot, but there is *still* a
lot of nuance to precisely what is allowed. It is important that you understand
these nuances, when working on *libreboot*.

[Please read the blob reduction guidelines](../../news/policy.md)


What is lbmk?
==============

In the same way that that *Alpine Linux* is a *Linux distribution*, Libreboot 
is a **coreboot distribution**. The `lbmk` build system *is* that distro,
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
for *technical* users who want to know how libreboot is put together.

The lbmk design also helps to ease copyright licensing and compliance, because
each part of lbmk is literally its own separate program. With this design, it
means that most scripts do not directly link/embed/include each other. Because
of this, it's much easier to have different licenses in use for different
files. Generally speaking, lbmk is GPLv3+, but it's perfectly OK, for
example, to add files that are GPLv2 or other licenses. By comparison, if you
were to have a C program under GPLv3, you could not \#include C libraries that
are GPLv2, at least not directly, or there would be many pitfalls to avoid at
the very least. With lbmk's design, you can think of it as like when you have
many programs running in your operating system, and not all of those programs
are under the same license, and most of those different licenses are not
compatible with each other; this is perfectly OK there, and it's OK here too.

The purpose of this document is to (hopefully) cause you to understand the
entire build system in libreboot, so that you can contribute patches or
otherwise make whatever changes you like. As such, this is a reference guide
for libreboot development.

Libreboot is a *coreboot distro*, focusing on integration. As such, direct
development on software such as coreboot, GRUB, SeaBIOS etc should ideally
be done upstream, or if it's a project hosted by libreboot (such as ich9utils)
developed in the corresponding separate repository.

This document is written for developers and power users alike, or otherwise for
anyone who is curious enough to learn more about what *makes* libreboot!

A major planned addition to lbmk in the future is: use it to implement a small
busybox+linux distribution, with musl libc, plus u-root, and implement a
linux-based bootloader setup similar to Heads, but do it *lbmk-style*. The
lbmk build system is designed for absolute simplicity and modularity, making
it easy to understand and maintain. It intentionally avoids use of rather
complicated programs such as Autoconf; the Makefile in lbmk is just bolted
on but it not required. The `lbmk` build system is a *non-design*; it evolved
over time, into what it is today. Its modularity and simplicity of non-design
allows you to easily rewrite large parts of it, whenever you want to do so.

lbmk is written in POSIX shell scripts, and this is unlikely to change in the
future. However, lbmk integrates several projects such as coreboot, GRUB
or SeaBIOS, and these all have *their* own build systems aswell. The `lbmk`
build system is the glue that puts all of these together to produce ROM images
for users, in a completely automated fashion. The purpose of `lbmk` is to
provide an *unattended* build process, with as little user interaction as
possible. Thus, `lbmk` is an *automated build system*. It says on the libreboot
home page that libreboot is a *coreboot distribution* in much the same way that
Alpine Linux is a *Linux distribution*, and `lbmk` is what implements that!

Continue reading, and you will learn of each file contained in `lbmk`. This
document largely pertains to the version of `lbmk` as hosted in `lbmk.git`,
but this manual also covers source code archives containing the full downloaded
set of modules such as coreboot and GRUB.

In general, it is advisable to open *every* file in lbmk, after you downloaded
it (from the Git repository), and study the logic in great detail. This manual
attempts to explain all of it, and provide a general idea, but nothing beats
simply *studying* the logic directly.

AUTOMATED automation
====================

Every part of lbmk checks if the prerequisite steps are done, and does them
automatically if not. The `roms_helper` script is no different; for example, it
automatically downloads coreboot if not present, aswell as GRUB and everything
else. You can run each and every part of lbmk without having to worry about
running something before it, because it is handled automatically; if that is
ever not the case, it's a bug that should be fixed immediately (in Libreboot
20160907, such fine tuned automation did not exist and you did have to run
specific parts of the build system manually, in a precise order, but this is
no longer the case in modern `lbmk` or `lbmk`).

Another example: if you run `./build payload grub` but `./build module grub` is
not completed, it will automatically run that first, to produce
the `grub-mkstandalone` binary which is then used by `./build payload grub`

Another example: if you run `./build boot roms` and crossgcc isn't yet built
for the revision used on each given board, it will automatically compile that
version of it, using *that* coreboot tree's own build system to do it.

This level of automation means that modern `lbmk` is much
easier to use, compared to the build system present in Libreboot 20160907.
Massive improvements to that build system were made, during most of 2021, when
implementing the `lbmk` build system.

All sections below pertain to actual files in lbmk:

.gitcheck
=========

This checks whether Git credentials are set, and sets placeholder credentials
if required, locally for the given project.

If something went wrong during build, these placeholder credentials will still
be in effect, but only within `lbmk`. You can unset them like so:

	./.gitcheck Clean

This is important, when working on Libreboot. Ordinarily, temporary credentials
are cleared after running lbmk, but may not be cleared in error conditions.

A bit of a hack, but it avoids build issues when the user hasn't set a name
and email address in Git. If you've set a *global* one, then this script is
irrelevant.

NOTE: In a git repository, the directory `.git` and files like `.gitignore`
or `.gitmodules` are used by the Git software. The name of this script begins
with `.git`, but the Git software does not make use of this file. It is
a *shell script*, executed by lbmk when you run commands in it.

More context about Git name/email can be found in
the [Libreboot build instructions](../build/).

.git
====

Metadata used by git-scm, the version control system that Libreboot uses for
development. This directory will be present in the Git repository. It is not
provided in Libreboot *releases*. Learn more about Git here:

<https://git-scm.com/>

.gitignore
==========

This file is used by Git. It tells Git to *ignore* certain files, so that they
do not get added accidentally to commits for the Libreboot Git repository,
named `lbmk.git`.

You can learn more about `.gitignore` files here:

<https://git-scm.com/docs/gitignore>

COPYING
=======

This file contains a copy of the GNU General Public License, version 3.0. It is
the license that most parts of `lbmk` are released under.

NOTE: Not all of Libreboot is released under this license, but it is heavily
used in the Libreboot *build system*. Much of coreboot is GPL version 2 and
in some cases, other licenses (such as BSD-style licenses) are used. This is
inevitable, with Libreboot being an *aggregate distribution* of software,
namely coreboot and *payloads*, plus utilities.

Including a `COPYING` file is a good, conservative first step in adhering
to *good practise* when it comes to software, and it is *mostly* technically
correct in the context of *lbmk*, because *most* of lbmk is under GPLv3. This
is a legacy from when Libreboot started, where that license was chosen, and
it has just been *de facto* standard for Libreboot (build system) ever since.

You should perform an audit, to learn more about other licenses. This can be
done by inspecting the various projects that lbmk makes use of, like coreboot.
For aggregate distributions such as coreboot distributions or Linux distros,
it's not trivial to keep track of every license in a simple way, so such an
audit is inevitable if you want to know more.

Makefile
========

For use with *GNU* Make, this is a frontend to `lbmk`, which can be used to run
various commands in `lbmk`.

Use of this file is purely optional, and largely beneficial if you simply want
to build all of `lbmk` (just run `make` when the current work directory is the
root directory of `lbmk`).

README*.*md
===========

This file contains a brief description of libreboot, along with information
about the project

It's basically a copy of the homepage text, relative to libreboot.org.

blobs/
======

This directory contains binary blobs, presently only GbE and IFD files which
are non-software blobs; they are binary-encoded configuration files.

The IFD files are *Intel Flash Descriptors* configuring the machine, on Intel
machines that use flash descriptors, and the GbE files are configs for Intel
gigabit ethernet. You can read more about these on
the [freedom status page](../../freedom-status.md) - the `ifdtool`
and `nvmutil` programs interact with these (nvmutil is provided by Libreboot,
and ifdtool is supplied by the upstream coreboot project).

When `blobutil` is running, it will place temporary files here, extracting
binary blobs such as Intel ME firmware, for running through `me_cleaner`.

At present, only the GbE and IFD files are included here for Libreboot
releases, but other files such as `me.bin` are stored here during the build
process (auto-downloaded and processed through `me_cleaner` on boards that
need them).

NOTE: Other blobs such as EC firmware and Intel MRC are *not* placed here, by
lbmk.

blobutil
========

This script is responsible for downloading, extracting and inserting binary
blobs that are required on specific machines for coreboot. This is done
automatically, during the build process, but `blobutil` can also be used as
a standalone program, for *release* ROM images (many of which will have certain
blobs like Intel ME *scrubbed*, where the user is expected to insert them).

You can read more about this on the page: [Inserting binary blobs
on Sandybridge/Ivybridge/Haswell](../install/ivy_has_common.md)

NOTE: This utility may be expanded in future Libreboot revisions, to handle
things such as EC firmware, and it may expand to other platforms. It is, at
present, only utilised for handling ROM images on Intel Sandybridge, Ivybridge
and Haswell platforms.

build
=====

This is the main shell script, part of `lbmk`, used for running most `lbmk`
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

At the time of writing this section, this would output something like:

````
Available options for mode 'boot':

roms
roms_helper
```

Another use of `list` would be:

	./build boot roms list

However, the `roms` script merely happens to implement a `list` command. For
example, `./build payload grub list` does nothing differently
than `./build payload grub`.

You may also refer to the [build instructions](../build)

download
========

This is the main shell script for downloading various components used by `lbmk`.
For example, this script downloads coreboot. Scripts called by `download` may
also apply patches and such, to the corresponding project; for example, it will
apply custom patches to GRUB.

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

*Most* download modules are defined in `resources/git/revisions`, showing the
link to git repositories and info about revisions, for each given project.
More on this is available in the page you're reading now (keep reading).

gitclone
========

This script is used by the `download` script, specifically for cloning of
Git repositories, for certain projects as defined in the file at location,
path `resources/git/revisions` within lbmk.

It downloads a project, from a main Git repository or a backup if defined and
the main one is down. It then resets to a defined revision (commit ID). If
patches are supplied for that project, by lbmk, then those patches are applied.
The patches are applied as per: ascending, alphanumerical order of patch file
name.

modify
======

This can be used to modify SeaBIOS, coreboot and U-Boot configs. It calls
scripts in `resources/scripts/modify/`, for example:

	./modify coreboot configs

This runs:

	./resources/scripts/modify/coreboot/configs

Additional parameters can be given, for example:

	./modify coreboot configs x200_8mb x60

This would run:

	./resources/scripts/modify/coreboot/configs x200_8mb x60

If you run it without arguments, help text is shown.

projectname
===========

This file contains a single line of text, with the string "libreboot".

If you were to fork libreboot, you could very easily just modify this file, so
as to rename your fork in a largely automated way. Many parts of lbmk use this
file.

resources/blobs/
================

This directory contains ME7 Update Parser, and a file defining links to vendor
update files, from which Intel ME images are extracted, to be neutered
via `me_cleaner`.

resources/blobs/me7\_update\_parser.py
======================================

This is a special fork of `me_cleaner`, specifically for parsing and neutering
Intel ME images provided by Lenovo for ThinkPad X220 and other Lenovo
ThinkPads of Intel SandyBridge platform. You can find information about this
on the original repository:

<https://github.com/Thrilleratplay/me7_update_parser>

ME7 Update Parser was originally written for *Heads*, another coreboot distro
very similar to Libreboot that provides coreboot build automation with Linux
based payload configurations. *Their* build system auto-downloads and
auto-neuters Intel ME images, during build, so that the user does not have to
manually extract such images from dumps of the original vendor firmware (in
the flash) on a given machine.

Such logic was ported to Libreboot, courtesy of `shmalebx9` as mentioned
on the [who page](../../who.md) - Caleb is a core developer in the Libreboot
project.

resources/blobs/sources
=======================

URLs and hashes for vendor files containing Intel ME images within them. Where
feasible, backup URLs are also provided. SHA1 checksums are defined, so that
lbmk can verify the integrity of downloaded files.

When building for sandybridge, ivybridge and haswell machines, Libreboot's
bulid system automatically downloads such updates from the vendor, to extract
the Intel ME image and neuter it with `me_cleaner` or `me7_update_parser.py`.

Such auto-download logic was ported from the *Heads* build system, to be used
in the *Libreboot* build system. It is the bee's knees, because it prevents
the need for manual extraction of Intel ME images from vendor dumps. It means
that you can just *build Libreboot* (from `lbmk`) and then just flash the
resulting image, without having to worry.

Of course, backing up the original firmware is still a good idea, before
installing Libreboot or any other spin of coreboot.

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
* `payload_uboot="y"` (example entry)
* `payload_seabios_withgrub="y"` (example entry)
* `grub_scan_disk="ata"`
* `uboot_config=default`

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
recognized entries are `x86_32`, `x86_64`, `ARMv7` and `AArch64`. *Setting it
to a non-native arch means that necessary crossgcc-arch will be compiled and be
available when building roms, but not necessarily built or discovered when
individual scripts are called manually.*

The `payload_grub` entry specifies whether or not GRUB is to be included in
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

The `payload_uboot` entry specifies whether or not U-Boot is to be included in
ROM images.

The `uboot_config` option specifies which U-Boot board configuration file
variant should be used. It currently doesn't make sense for this to be anything
other than `default`, which is the default if the option is missing.

The `grub_scan_disk` option specifies can be `ahci`, `ata` or `both`, and it
determines which types of disks are to be scanned, when the `grub.cfg` file in
GRUB payloads tries to automatically find other `grub.cfg` files supplied by
your Linux distribution. On some machines, setting it to `ata` or `ahci`
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
the policy of libreboot, to only ever use the *coreboot* build system inside
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

resources/git/revisions
=======================

This defines git repositories and commit IDs (revisions) to reset to, for
various projects used by Libreboot. *This* file is used, for projects that
are relatively simple to handle when downloading (coreboot is not defined
here).

In the past, Libreboot had bespoke logic for *each* program, to download it.
This was repetitive, so much of the download logic was centralised with the
use of the `gitclone` script, which references this file.

resources/grub/background/
==========================

Splash screen images applied duing startup when using the GRUB payload.

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

This file defines all modules that are to be included in builds of GRUB.
They are standalone builds, created using the `grub-mkstandalone` utility.

resources/grub/patches/
=======================

This directory contains custom patches for GRUB.

resources/me\_cleaner/patches
=============================

Patches applied to `me_cleaner` when downloading it.

resources/memtest86plus/patches/
==============================

Patches applied to Memtest86+ when downloading it. Libreboot includes
memtest86+ as a secondary payload, loaded from SeaBIOS *or* GRUB when booted
via int10h text mode on x86 hosts.

(using it with corebootfb mode is also possible, if your machine has a viable
serial output on it with memtest86+ configured accordingly, and this was done
on some older Libreboot releases in the past, but current Libreboot releases
only provide memtest86+ in text mode, for use directly on the machine)

resources/scripts/
==================

These scripts implement the *core* logic of Libreboot's *automated build
system*, to produce coreboot ROM images with payloads.

resources/scripts/blobs/download
================================

Where certain binary blobs like Intel ME or MRC are needed on a given board,
this script is called automatically by the build system, to download the files
for insertion.

resources/scripts/blobs/extract
===============================

Where auto-download is not viable, this script can provide a somewhat automated
experience for *extracting* blobs. You will supply a *dump* of the original
vendor firmware, dumped from the flash IC on whichever target machine you wish
to flash.

Dumping the original firmware is *always* recommended, regardless of what you
want to do.

resources/scripts/blobs/inject
==============================

Where a blob is provided, via the `extract` or `download` method, *this*
script *inserts* a blob (ME, MRC etc) into a given target image.

resources/scripts/build/
========================

This directory contains shell scripts for compiling various binaries from
available sources.

resources/scripts/build/boot/
=============================

This directory contains shell scripts for compiling ROM images. Many other
scripts in lbmk are called by these scripts; for example, GRUB payload scripts.

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

Since November 2022, this script can build images for x86 *and* ARM targets.
The *ARM* targets are ChromeOS devices (chromebooks and such); Libreboot uses
the *U-Boot* payload, rather than Google's *depthcharge* bootloader. In this
setup, U-Boot is running on the bare metal, as enabled by *coreboot*.

For x86 targets, these scripts build with the GRUB and/or SeaBIOS payloads
inserted into the ROM images; secondary payloads like Memtest86+ are also
handled and inserted here.

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

It creates ROM images with GRUB, SeaBIOS, U-Boot, optionally with Memtest86+
also included, in various separate configurations in many different ROM images
for user installation.

The `romtype` entry in `board.cfg` tells this script what to do with the ROM,
after it has been built. Currently, it operates based on these possible values
for `romtype`:

* `4MiB IFD BIOS region` will cause only the upper 4MB section of the ROM
  to be included in a release. This option is largely deprecated, a hangover
  from libreboot, which also no longer uses this option on any boards, and it is
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
  such boards in libreboot, GbE NVM is needed to get gigabit ethernet working
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

This script handles U-Boot separately, for ARM-based chromeos devices.

Essentially, the `roms_helper` script makes use of each and every part of
lbmk. It is the heart of libreboot.

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

This deletes the `bin/` directory, containing compiled coreboot ROM images.

Command: `./build clean rom_images`

resources/scripts/build/clean/seabios
=====================================

This runs `make clean` in the `seabios/` directory.

Command: `./build clean seabios`

resources/scripts/build/clean/u-boot
======================================

This runs `make distclean` and `git clean -fdx` on all of the U-Boot revisions
present in lbmk.

Command: `./build clean u-boot`

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

resources/scripts/build/payload/u-boot
======================================

This builds the U-Boot payloads. Usually a target board and a cross-compiler
appropriate for the board must be specified for it to work, because trying to
build for all boards of varying architectures using only the host compiler will
not work.

Command: `CROSS_COMPILE=aarch64-gnu-linux- ./build payload u-boot qemu_arm64_12mb`


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

NOTE: This script *scrubs* certain binary blobs from release ROMs, such as
Intel ME or MRC firmware. The release ROMs shall then exclude these blobs
within them, requiring manual insertion by the user post-release. See:

[Insert binary blobs
on Sandybridge/Ivybridge/Haswell](../install/ivy_has_common.md)

resources/scripts/download/coreboot
===================================

This downloads, and patches coreboot, as per `board.cfg` files
in `resources/coreboot/`.

Command: `./download coreboot`

NOTE: This version of the script also performs the full git checkout in each
coreboot tree, like so:

	git submodule update --init --checkout

The coreboot project sets up its Git repository, in such a way where most blobs
are skipped if you omit `--checkout`. Since lbmk's policy is to *include*
these in its distribution, it makes sense to use `--checkout`.

resources/scripts/download/flashrom
===================================

This downloads and patches flashrom.

Command: `./download flashrom`

resources/scripts/download/grub
===============================

This downloads and patches GRUB.

Command: `./download grub`

resources/scripts/download/me\_cleaner
======================================

This downloads the `me_cleaner` program, for neutering Intel ME images. You
can read more about it here:

<https://github.com/corna/me_cleaner/wiki/>

resources/scripts/download/memtest86plus
========================================

This downloads and patches Memtest86+.

Command: `./download memtest86plus`

resources/scripts/download/mrc
==============================

Where required, this will download Intel MRC images. This is called
automatically by lbmk, on platforms that require it (currently only
Intel Haswell, where Libreboot-covered hardware is concerned, and a
libre replacement of `mrc.bin` exists on that platform, provided as
an option in Libreboot 20230319 or newer releases).

This is a fork of *coreboot's* MRC image download script, which does
not guarantee specific versions of the file nor does it check SHA1
hashes and such. It was forked for Libreboot purpose, because the
Libreboot build system *enforces* such verification during the build
process.

resources/scripts/download/seabios
==================================

This downloads and patches SeaBIOS.

Command: `./download seabios`

resources/scripts/download/u-boot
=================================

This downloads, and patches U-Boot, as per `board.cfg` files
in `resources/u-boot/`.

Command: `./download u-boot`

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

resources/scripts/modify/u-boot/configs
=======================================

Loads U-Boot configs into U-Boot trees, and runs `make menuconfig`, so
that you can easily modify them in an ncurses interface. Additional parameters
are accepted, for example:

	./modify u-boot configs gru_kevin gru_bob

With no additional parameters given, it simply cycles through all configs
under `resources/u-boot/`.

Command: `./modify u-boot configs`

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

resources/scripts/update/u-boot/configs
=========================================

This runs `make oldconfig` on U-Boot configs under `resources/u-boot/`.
It is most useful when updating a U-Boot revision, per `board.cfg`. It allows
additional parameters, for example:

	./update u-boot configs gru_kevin gru_bob

With no additional parameters given, it simply cycles through all configs
under `resources/u-boot/`.

However, using `make oldconfig` is not optimal for U-Boot, as their Kconfig
dependencies/defaults are not as well specified as coreboot's is. When updating
configs for an upstream board, it's usually better (but not automated) to:

- Turn `lbmk` config into a defconfig in the old version
- Compare it with the old version's upstream defconfig
- Apply the difference to the new version's upstream defconfig
- Create an updated config in the new version

Command: `./update u-boot configs`

resources/seabios/config/libgfxinit
===================================

SeaBIOS configuration file, when `libgfxinit` is to be used. It enables
the `coreboot linear framebuffer` option in the SeaBIOS `make menuconfig`
configuration interface.

resources/seabios/config/vgarom
===============================

This version is for normal SeaBIOS configurations, where `libgfxinit` is not
to be used.

resources/seabios/patches/
==========================

This directory contains patch files, automatically applied to SeaBIOS after
downloading it.

update
======

This can be used to update SeaBIOS, coreboot and U-Boot configs. It calls
scripts in `resources/scripts/update/`, for example:

	./update coreboot configs

This runs:

	./resources/scripts/update/coreboot/configs

Additional parameters can be given, for example:

	./update coreboot configs x200_8mb x60

This would run:

	./resources/scripts/update/coreboot/configs x200_8mb x60

resources/u-boot/
=================

This directory contains configuration, patches and so on, for each mainboard
that can use U-Boot as a payload in the `lbmk` build system. U-Boot doesn't yet
have reliable generic configurations that can work across all coreboot boards
(per-architecture), so these are used to build it per-board.

resources/u-boot/BOARDNAME/
===========================

Each `BOARDNAME` directory defines configuration for a corresponding mainboard.
It doesn't actually have to be for a board; it can also be used to just define
a U-Boot revision, with patches and so on. To enable use as a payload in ROM
images, this must have the same name as its `resources/coreboot/BOARDNAME/`
counterpart.

resources/u-boot/BOARDNAME/board.cfg
====================================

This file can contain several configuration lines, each being a string, such
as:

* `ubtree="default"` (example entry)
* `ubrevision="4debc57a3da6c3f4d3f89a637e99206f4cea0a96"` (example entry)
* `arch="AArch64"` (example entry)

These are similar in meaning to their coreboot counterparts.

The `ubtree` entry is actually a link, where its value is a directory name
under `resources/u-boot`. For example, `ubtree="default"` would refer to
`resources/u-boot/default` and the corresponding U-Boot source tree created
(when running `./download u-boot`, which makes use of `board.cfg`) would be
`u-boot/default/`. In other words: a `board.cfg` file in `resources/u-boot/foo`
might refer to `resources/u-boot/bar` by specifying `ubtree="bar"`, and the
created u-boot source tree would be `u-boot/bar/`. ALSO:

FUN FACT: such references are infinitely checked until resolved. For
example, `foo` can refer to `bar` and `bar` can refer to `baz` but if there is
an infinite loop, this is detected and handled by `lbmk`. For example,
if `bar` refers to `foo` which refers back to `bar`, this is not permitted
and will throw an error in `lbmk`.

The `ubrevision` entry defines which U-Boot revision to use, from the U-Boot
Git repository. *At present, lbmk only supports use of the official repository
from the upstream U-Boot project*.

The `arch` entry specifies which CPU architecture is to be used: currently
recognized entries are `x86_32`, `x86_64`, `ARMv7` and `AArch64`. *Setting it
to a non-native arch means that necessary crossgcc-arch will be compiled and be
available when building roms, but not necessarily built or discovered when
individual scripts are called manually.*

resources/u-boot/BOARDNAME/config/\*
====================================

Files in this directory are *U-Boot* configuration files. Configuration file
names can be anything, but for now `default` is the only one used.

In `lbmk`, a board-specific directory under `resources/u-boot/` should never
specify a U-Boot revision. Rather, a directory *without* U-Boot configs should
be created, specifying a U-Boot revision. For example, the directory
`resources/u-boot/default/` specifies a U-Boot revision. In the board-specific
directory, your `board.cfg` could then specify `ubtree="default"` but without
specifying a U-Boot revision (this is specified by
`resources/u-boot/default/board.cfg`).

Normally, the U-Boot build process results in the U-Boot executable and a
device-tree file for the target board, which must further be packaged together
to make things work. When you create a U-Boot configuration, you should enable
`CONFIG_REMAKE_ELF` or `CONFIG_OF_EMBED` that handles this. The former option
enables creation of a `u-boot.elf` that bundles them together after the build,
and the latter option embeds it into the `u-boot` executable.

When making a U-Boot configuration, you should also pay special attention to
the `CONFIG_SYS_TEXT_BASE` (`CONFIG_TEXT_BASE` in later versions), whose defaults
may cause it to overlap coreboot, in which case it won't boot. Normally, the
upstream coreboot build system checks for this when given `CONFIG_PAYLOAD_ELF`,
but `lbmk` injects the payload itself and doesn't check for this yet.

Another interesting config option is `CONFIG_POSITION_INDEPENDENT` for ARM
boards, which has been so far enabled in the ones `lbmk` supports, just to be
safe.

U-Boot build system
-------------------

If you wish to know about U-Boot, refer here:\
<https://u-boot.readthedocs.io/en/latest/>

This and other documents from U-Boot shall help you to understand *U-Boot*.

You create a config, for `resources/u-boot/BOARDNAME/configs`, by finding the
corresponding board name in the upstream U-Boot `configs` directory, and
running `make BOARDNAME_defconfig` and `make menuconfig` commands in the
*U-Boot* build system. You should do this after running `./download u-boot` in
`lbmk`.

You might want to consider basing your config on the upstream `coreboot` boards
when possible, but such a board is not available upstream for ARM yet.

You can simply clone U-Boot upstream, add whatever patches you want, and
then you can make your config. It will appear afterwards in a file
named `.config` which is your config for inside `resources/u-boot/BOARDNAME/`.

You can then use `git format-patch -nX` where `X` is however many patches you
added to that U-Boot tree. You can put them in the patches directory
under `resources/u-boot/BOARDNAME`.

The *base* revision, upon which any custom patches you wrote are applied,
shall be the `ubrevision` entry.

Scripts exist in `lbmk` for automating the modification/updating of *existing*
configs, but not for adding them. Adding them is to be done manually, based on
the above guidance.

resources/u-boot/BOARDNAME/patches/\*
=====================================

In cases where `ubrevision` is specified, where the given directory
under `resources/u-boot/` does in fact define a version of U-Boot to
download, you can add custom *patches* on top of that revision. When you run
the command `./download u-boot`, those patches will be applied chronologically
in alphanumerical order as per patch file names.

The patch files should be named with `.patch` file extensions. All other files
will be ignored. By having `lbmk` do it this way, you could add a `README` file
for instance, and `lbmk` will not erroneously try to apply `README` as though
it were a patch file. This might be useful if you have a *lot* of patches, and
you want to provide some explanations about specific files.

util/
=====

This directory contains utilities that `lbmk` makes use of.

util/nvmutil/
=============

The `nvmutil` software allows you to set the MAC address on Intel GbE NVM
files. It also allows you to set *random* MAC addresses, in addition to
arbitrary ones.

This directory contains the source code for `nvmutil`, which you can read
about here:

[nvmutil manual](../install/nvmutil.md)

util/ich9utils/
===============

The `ich9utils` utilities handle ICH9M Flash Descriptors, and GbE NVM configs
for Intel Gigabit Ethernet chipsets used on certain laptops of Intel GM45
platform, combined with ICH9M southbridge.

This directory contains the source code for `ich9utils`, which you can read
about here:

[ich9utils manual](../install/ich9utils.html)

This source code also pertains to `ich9gen`, which is what GM45 laptops in
Libreboot use in order to generate a config that *excludes* Intel ME firmware.

Patches welcome.

If you read this manual from start to finish, you've been assimilated. Welcome
to the team!
