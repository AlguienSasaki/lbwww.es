---
title: Software and hardware freedom status for each mainboard supported by Libreboot
x-toc-enable: true
...

Introduction
============

This page documents how Libreboot's [binary blob reduction
policy](news/policy.md), adopted in November 2022, is implemented in practise,
especially that line which says: *"if free software can be used, it must be
used."*

Libreboot uses [coreboot](https://coreboot.org/) for hardware initialisation.
While coreboot is nominally [*free* or *open source*
software](https://writefreesoftware.org/), on *some* (not
all) platforms, coreboot *requires* certain
[vendor files](https://en.wikipedia.org/wiki/Binary_blob)
for things like raminit. *All* boards currently supported by Libreboot can be
initialised entirely with *free*, *libre* or *open source* code from *coreboot*
itself, because Libreboot currently only focuses on such mainboards. Libreboot's
goal is to eventually support *all* mainboards from coreboot.

A more *pragmatic* [binary blob reduction policy](news/policy.md) was adopted
by Libreboot during November 2022, as part of an ongoing campaign to support
more hardware (from coreboot) within Libreboot, so as to provide *many more*
people with coreboot which, regardless of freedom status, *does* provide increased
[software freedom](https://writefreesoftware.org/) compared to fully proprietary
boot firmware which is what most
people would otherwise use; Libreboot's modern policy is thus pragmatic, further
advancing the cause of *software freedom*.

Libreboot's *previous* policy was to *ban all binary blobs*. This actively
*harmed* the Free Software movement, by reducing the number of people who can
realistically use coreboot because, to this day, nothing quite like Libreboot
yet exists. Libreboot's main purpose is to make coreboot *as easy to use as
possible* for normal, non-technical users who like the idea of coreboot but
who are otherwise not competent to configure it from scratch. Such harm
was *corrected*, in November 2022.

Coreboot architecture
---------------------

For context about certain topics, please read:

<https://doc.coreboot.org/getting_started/architecture.html>

100% libre init in coreboot
===========================

The reason this distinction matters (referring specifically to coreboot's side
of the initialisation) will become clearer, in the following sections:

A universal exemption is made in Libreboot, permitting (read: requiring, per
project policy) the inclusion of CPU microcode updates if available. You can
read more about that and the reasons *why* by reading the following article:

[CPU microcode updates
are **OK**](news/policy.md#more-detailed-insight-about-microcode)

[Releases after Libreboot 20230423 will provide separate ROMs with microcode
excluded, alongside default ones with microcode included.](news/microcode.md)

Intel platforms
===============

Descriptor vs descriptorless setup
----------------------------------

Libreboot supports several mainboards using Intel platforms. Of these, there
are essentially two class of machine (for the purposes of this article):

* Descriptorless configuration
* Descriptor-based configuration

What does *descriptor* mean? Well, traditionally, the main flash IC (containing
the boot firmware, commonly referred to as *the BIOS*) on Intel machines,
contained only *x86* executable code, responsible for initialising all of
the hardware, such as the CPU, memory controller and peripherals. This is
what we will refer to as the *descriptorless* setup; in such configurations,
the Intel ME firmware is absent by default.

In a *descriptor* configuration, the flash is divided into regions such as:

* Intel flash descriptor: always the first 4KiB of the flash. Binary-encoded
  configuration data for the machine, and the regions (such as this, or
  others below) is defined in here. In some ways, you might think of this as
  analagous to the *Master Boot Record* on a hard drive.
* Intel GbE NVM (gigabit ethernet non-volatile memory): binary-encoded
  configuration data, for the onboard Intel gigabit ethernet device, if one is
  present. It contains lots of settings like MAC address, what speed the NIC
  should run at, PCI IDs, *LED blinking speed* and more.
  If a non-Intel NIC is used, this region of the flash will not be present.
* ME (Intel Management Engine): a *nasty* security liability in its default
  state, the ME provides many features such as remote management, with full
  networking, The ME is a dedicated coprocessor separate from the main CPU, and
  the initialisation firmware plus operating system for it is loaded from
  this dedicated region in the main boot flash. More info is available [in the
  FAQ](faq.md#intelme) - where ME firmware is otherwise present, Libreboot
  either [removes](docs/install/ich9utils.html) it or (with the `me_cleaner` program) [reconfigures](https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F) it in such
  a way where it is disabled during machine initialisation.
* Platform region: non-program data, usually just a bunch of strings put there
  by the hardware vendor.
* BIOS region: this contains the main boot firmware, responsible for
  initialising the CPU, memory controller and peripherals, putting the
  machine into a state where it can run programs (most likely a bootloader,
  then an operating system). The coreboot code (provided by Libreboot) goes in
  here.

*Basically*, you can think of such "regions" as analogous to *partitions* on
a hard drive. What's important is that the flash IC is *divided* into such
regions, where each region is intended for a specific purpose.

The contents of the *descriptor* and *GbE* regions are described by Intel
datasheets, but those datasheets often contain *reserved* sections where
parts are left undocumented. Reverse engineering efforts over the years have
documented some of these blank spots.

Libreboot does *not* distribute Intel ME images
-----------------------------------------------

Libreboot does *not* distribute the Intel ME firmware in any way, whether in
the Git repository or in releases. Where it is needed, Libreboot provides
scripts that automatically fetch and install it, in a *neutered* (disabled)
state by running the `me_cleaner` program. This is completely automated.
Please read:

<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

The *BringUp* code of Intel ME is all that remains, in Libreboot configurations.
ME BringUp (BUP) is analogous to coreboot, providing initialisation for the
ME itself; by that same analogy, the way Libreboot configures it is similar to
running *coreboot* without a payload. The ME is initialised, to a state where
it can run code, but then it *doesn't actually run code*. It is thus *disabled*.

In other words, a *neutered* Intel ME setup is completely benign, both from a
software freedom and security perspective. It becomes a useless, unused
processor, that most people in the real world will never want to use anyway.
With this perspective, we see that Intel ME is now entirely inconsequential
to the average user.

*Released* Libreboot ROM images, provided pre-compiled, do *not* include the
ME firmware at all; they are scrubbed, by automated release scripts when they're
preparing a release. If you're building from source, the Libreboot build system
will automatically download it (from the vendor), neuter it and then insert it;
on release ROMs, the same scripts used by the build systems can (must) be run
manually, accomplishing the same result after the fact. Please read:
[docs/install/ivy_has_common.md](docs/install/ivy_has_common.md)

The ME firmware is *required* on almost all Intel platforms, or the machine
will turn *off* after 30 minutes (or it will not boot, if the ME also controls
whether the CPU comes out of reset).

More about Intel ME removal/disabling
----------------------------------

*Libreboot* provides a way to fully remove the ME firmware, while retaining
full use of the machine, on GM45 platforms with ICH9M southbridge. These are
laptops: ThinkPad X200/T400/T500/W500 and so on of that generation. See:
[docs/install/ich9utils.md](docs/install/ich9utils.md)

The `ich9utils` software is provided by Libreboot. The `ich9gen` utility was
specifically written by Leah Rowe, in 2014 and improved incrementally since.

On newer platforms as alluded to above, `me_cleaner` is used instead.

Notes about specific types of vendor file
===================================

VGA option ROMs
---------------

*Native* video initialisation is supported and *enabled*, for all supported
Intel platforms that have it. The source code is provided by coreboot, under
free license.

In some cases, a laptop may have a graphics chip that is unsupported by
coreboot. In this situation, a vendor file called a *VGA Option ROM* must be
used. Libreboot has *experimental* support for Nvidia GPU models of the Dell
Latitude E6400, in an experimental branch where the build system automatically
downloads the VGA ROM for it. This is currently *not* present in releases, or
in the stable branch of `lbmk`.

In other instances, a machine may have *two* graphics devices, where one has
native (free/libre) initialisation provided by coreboot. In these situations,
it is possible to insert a VGA ROM for the other one; Libreboot currently lacks
documentation for this, but it has been tested. Example: Dual Intel/Nvidia
graphics on some ivybridge or haswell thinkpads.

For *add-on* GPUs, SeaBIOS (payload) can typically scan a VGA ROM present on
the card and execute it. This has been tested on certain desktop mainboards
that Libreboot supports, and works just fine; Libreboot does not need to handle
these files at all.

Libreboot's default is *always* freedom, when feasible in practise. Users who
wish to have use of these additional GPUs, on such hardware, must take stock
of the following paragraph in Libreboot policy:

*"The principles above should apply to default configurations. However,
libreboot is to be configurable, allowing the user to do whatever they like."* -
configurable, it most certainly is! See: [docs/maintain/](docs/maintain/)

Memory controller initialisation
--------------------------------

Libreboot has *fully libre* initialisation available for all Intel memory
controllers on supported platforms. This *includes* Haswell (ThinkPad T440p
and W541) as of Libreboot 20230319 or higher.

ARM platforms (chromebooks)
=============

Mostly free software, except for the requirement on `daisy` and `peach` mainboards
to include BL1 bootloader files from the vendor. These are:

* HP Chromebook 11 G1 (daisy-spring) **(board removed from Libreboot, due to
	issues (will be re-added at a later date))**
* Samsung Chromebook XE303 (daisy-snow) **(ditto)**
* Samsung Chromebook 2 13” (peach-pi) **(ditto)**
* Samsung Chromebook 2 11” (peach-pit) **(ditto)**
* nyan-* chromebooks also temporarily removed, but are 100% free software in
  Libreboot

List of vendor files, specifically for each board
=================================================

This article has thoroughly explained, in a detailed overview, the precise
nature as to *what* vendor files are accomodated for in Libreboot. Again,
fully libre init from coreboot is available *on all currently supported boards*.

*From coreboot* is the critical aspect of this, but Libreboot's full scope is
the main flash IC which (in some cases) contains software outside of coreboot.

Here is a list, *for each* board, of those files:

Intel/x86
---------

### Intel ME:

Neutered ME required on these targets: `t420_8mb`, `t420s_8mb`, `t430_12mb`,
`t440p_12mb`, `t440pmrc_12mb`, `t520_8mb`, `t530_12mb`, `w530_12mb`,
`w541_12mb`, `w541mrc_12mb`, `x220_8mb`, `x230_12mb`, `x230_16mb`,
`x230edp_12mb`, `x230t_12mb`, `x230t_16mb`, `hp8200sff`, `hp2560p_8mb`,
`hp2570p_16mb`, `hp8300usdt_16mb`, `hp2170p_16mb`, `hp9470m_16mb`,
`hp820g2_12mb`, `t1650_12mb` and the OptiPlex 9020 ports, also
Sandybridge/Ivybridge/Haswell Dell Latitude.

As stated, Libreboot provides this in a state where the ME is no longer a
threat to security. It initialises itself, but then does nothing, so it's
disabled. This is done using `me_cleaner`. See:
<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

### KBC1126 EC firmware (HP laptops):

This applies to the following targets: `hp2170p_16mb`, `hp2560p_8mb`,
`hp2570p_16mb`, `hp8470pintel_16mb`, `hp9470m_16mb`.

[EC firmware](faq.md#ec-embedded-controller-firmware)
is inserted into main boot flash, rather than being on a separate
IC. This is *better* because libre replacements would be more easy to install in
the future, and reverse engineering is made much easier by it. Libreboot's
build system handles such firmware in a script, automatically downloading
it during the build process. Libreboot 20230423 onwards does scrub EC firmware
and provide functionality in a special script, to insert them with `cbfstool`
at the correct offset as defined by coreboot config for each board.

### SMSC SCH5545 Environmental Control

This is a tiny firmware required for fan control, on Dell Precision T1650.

### CPU microcode:

[*Microcode* updates](faq.md#microcode)
for CPU provided on *all* x86 platforms, by default. Not
technically required, but highly recommended. To remove, do:

	cbfstool filename.rom remove -n cpu_microcode_blob.bin

On ASUS KFSN4-DRE, KCMA-D8 and KGPE-D16 boards, do this instead:

	cbfstool filename.rom remove -n microcode_amd.bin
	cbfstool filename.rom remove -n microcode_amd_fam15h.bin

[Releases after Libreboot 20230423 will provide separate ROMs with microcode
excluded, alongside default ones with microcode included.](news/microcode.md)

Removal of microcode updates will affect system stability in a negative way,
introducing non-standard broken behaviour and it may result in the machine
being unable to boot properly. In other cases, doing so may break features such
as S3 suspend/resume.

CPU microcode files included by default, on all x86 boards. While not needed
in most cases, their use is highly recommended. For reasons why, see:
[news/policy.md#more-detailed-insight-about-microcode](news/policy.md#more-detailed-insight-about-microcode)

### Intel Flash Descriptor (IFD):

Intel Flash Descriptors are provided as blobs on some boards, but these are
not *software* blobs. They are configurations provided in a binary format,
fully readable by libre software. For example:

* Libreboot's `ich9gen` program generates ICH9M flash descriptors from scratch.
* Coreboot's `ifdtool` program has extensive features for manipulating Intel
  flash descriptors.
* Corebot's `bincfg` program generates any sort of binary from a `.spec` file
  which can describe any binary format in a human readable format. It contains
  several flash descriptors for several platforms, but Libreboot does not use
  these.

Intel GbE NVM config (configuration data, binary-encoded, for gigabit NIC):

* Libreboot's `ich9gen` program *also* generates GbE NVM images specifically
  for Intel NICs used in GM45 thinkpads.
* Libreboot's `nvmutil` program can manipulate GbE NVM images

ARM/chromebooks
---------------

### BL1 bootloader (peach/daisy):

BL1 bootloader needed on: `daisy_snow`, `daisy_spring` and `peach_pit`.

These boards are *currently* not present. They were removed from Libreboot,
because the build system does not yet auto-insert the BL1 files. The boards
are otherwise believed to work, using Alper's port of U-Boot in Libreboot.

Conclusion
==========

From the above, you can see that Libreboot really *does* implement a *binary
blobs reduction policy*, with the emphasis on *reduction* being most critical.
It can be asserted that Libreboot does in fact provide a reasonable level of
*software freedom*, on all boards.

Libreboot *could* add a lot more such files for many platforms, to enable various
extra features, that it instead avoids adding, precisely because the *purpose*
of the Libreboot project is to promote *libre* software and *minimise* the
power that proprietary software developers have over users.

I hope this article provided food for thought.

An aside: hardware freedom
--------------------------

None of the currently supported Libreboot machines have libre *hardware*, in
the sense that ICs do not come with publicly available *verilog* files and the
like. You could not manufacture your own replacements of these machines.

Some schematics and boardview files describing the circuit boards of each
machine are available online, through various channels. You have to search for
them yourself; one day, the Right To Repair movement will hopefully bring
about universal access to such documents by the public.

Further reading
===============

This article has described code what goes in the *main boot flash*, but any
computer you buy will have *tons* of firmware elsewhere in the system. Some
insights are available in the Libreboot FAQ. See:

* [faq.md#what-level-of-software-freedom-does-libreboot-give-me](faq.md#what-level-of-software-freedom-does-libreboot-give-me)
* [faq.md#what-other-firmware-exists-outside-of-libreboot](faq.md#what-other-firmware-exists-outside-of-libreboot)

Of these, the most critical are HDD/SSD firmware and EC firmware. The problems
described in those two links apply to many different computers, including
Libreboot ones, and virtually every other computer in the world.
