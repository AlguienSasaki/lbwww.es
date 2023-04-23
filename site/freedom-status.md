---
title: Software and hardware freedom status for each mainboard supported by Libreboot
x-toc-enable: true
...

Introduction
============

The short version of the story is: *all* boards currently supported by Libreboot
can be initialised in coreboot with *free*, *libre* or *open source* code, that
the user can study in great depth and adapt for their purposes, but there are
certain caveats and pitfalls that the user must know about for certain machines.

As many readers may know, coreboot (which Libreboot uses for hardware
initialisation) is nominally *Free As In Freedom*, *libre* or *open source*
software; albeit, coreboot attempts to provide free code to initialise each
machine. The coreboot developers work very hard to reverse engineer as much
functionality as possible, so as to provide freely licensed source code.
These people are to be celebrated, for Libreboot could not exist without such
efforts by them.

On *some* boards that coreboot supports, certain binary blobs are required.
This may be for things like video framebuffer initialisation, setting up the
memory controllers and so on. A binary blob is, in this context, any software
that only comes as executable binary code *with source code unavailable*. This
makes it harder (but not impossible) to study how such programs work, and those
blobs often come with restrictions on either their usage and/or distribution.

The purpose of this document is to clearly describe the presence (or lack) of
binary blobs, on each given hardware platform or, within each platform,
specific mainboard variations. Such information was either absent or poorly
worded on the Libreboot website, so it was decided that formal documentation
should be written. The reason for such absence was that Libreboot previously
provided support *only* for those boards that do *not* require binary blobs in
the main flash IC for boot firmware, so no such documentation was required.

A more *pragmatic* policy was published during November 2022, with a view to
helping as many people as possible install coreboot, even on less-than-ideal
hardware (while continuing to also support the more freedom-friendly hardware).
Libreboot still has strict standards about precisely *what* blobs are allowed,
which you can read in the following document, thus:

[Binary blob reduction policy](news/policy.md)

In this article, you will learn of all the ways (in practise) that Libreboot
implements this *blob reduction policy*, especially that line in it which says,
quote, "if a blob can be avoided, it must be avoided".

Why does this matter?
---------------------

The *practical goal* of the Libreboot project is to support as much hardware of
coreboot compatibility as possible, fully tested with pre-compiled ROM images
and easy flashing instructions, aimed at non-technical users so as to bring in
as many users as possible into the coreboot community. With more users, many
more people are exposed to coreboot and this will inevitably lead to more
people developing coreboot, which is a critical project in the libre software
movement. With more developers, more *users* will be able to achieve a level of
freedom or *sovereignty* in their computing and, thus, the cycle shall continue.

This goal exists because the *ideological goal* of Libreboot is to spread
software freedom by whatever means necessary, to as many people as possible.
Universal access to source code, the ability to study and change the code or
redistribute it, and run it infinitely for any purpose is extremely important.
Such freedom should be the *default* for all software. This makes coreboot
extremely useful, even in cases where a binary blob is needed for certain
functionality. Freedom should be the *default* for all software, and it is the
purpose of *Libreboot* to help establish such a reality.

The *policy* of the Libreboot project, within that goal, is to provide such
hardware support with as *few* binary blobs as possible, ideally *none* as is
the case with many configurations that Libreboot supports. Libreboot *will*
attempt to support any given piece of hardware, even in cases where *full*
software freedom is not possible; for example, if coreboot requires a blob for
raminit on a given board, the blob would be provided by Libreboot.

*You can read Libreboot's blob reduction/minimalisation policy in more detail.
Please read the document, titled: [Binary Blob Reduction
Policy](news/policy.md).*

Coreboot architecture
---------------------

Although not *strictly* necessary for non-developers, you may find it useful
to gain a high-level understanding of *how* coreboot works, to gain some
some context about some of the topics discussed in this article. See:

<https://doc.coreboot.org/getting_started/architecture.html>

100% libre init in coreboot
===========================

*All* mainboards currently supported by Libreboot can have 100%
libre initialisation *from the coreboot side*. In this context, that refers to
any initialisation handled on the main CPU, to bring up the machine for code
execution.

The boot firmware is not the *only* software present in any machine, and there
are some contexts that must be understood to see the bigger picture.

The reason this distinction matters (referring specifically to coreboot's side
of the initialisation) will become clearer, in the following sections:

A universal exemption is made in Libreboot, permitting (read: requiring, per
project policy) the inclusion of CPU microcode updates if available. You can
read more about that and the reasons *why* by reading the following article:

[CPU microcode updates
are **OK**](news/policy.md#more-detailed-insight-about-microcode)

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
  either removes it or (with the `me_cleaner` program) reconfigures it in such
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

On some newer descriptor-based Intel platforms, there are more regions than
this. In some cases, the *[Embedded
Controller](faq.md#ec-embedded-controller-firmware)* (EC) firmware may also
be in its own region of the boot flash; at least for the time being, this is
not the case on any hardware that Libreboot supports (instead, it is stored
on a separate IC).

The contents of the *descriptor* and *GbE* regions are described by Intel
datasheets, but those datasheets often contain *reserved* sections where
parts are left undocumented. Reverse engineering efforts over the years have
documented some of these blank spots.

Libreboot does *not* distribute Intel ME images
-----------------------------------------------

The ME contains many modules within itself, and one of these modules is the
BringUp code. This BringUp code is the ME's *own* initialisation firmware,
analogous to coreboot. By that very same analogy, the other modules of
Intel ME (such as the ME kernel) are similar (conceptually) to
a *coreboot payload*.

Thus, a *neutered ME* setup is, on the intel ME coprocessor, analogous to
running coreboot without a payload. In this state, the ME initialises itself
ready to run code, but then *doesn't actually run code*. It is thus benign,
both from a security- and freedom-minded point of view. In other words, the ME
is *disabled*.

Libreboot does *not* distribute the Intel ME firmware in any way, whether in
the Git repository or in releases. Where it is needed, Libreboot provides
scripts that automatically fetch and install it, in a *neutered* state by
running the `me_cleaner` program, which you can learn about here:

<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

This is completely automated. If you're compiling ROM images from `lbmk.git`,
libreboot's automated build system, the ME firmware will be downloaded,
neutered with `me_cleaner` and inserted into the final ROM image automatically.

*Released* ROM images, provided pre-compiled, omit the Intel ME firmware. On
platforms that require it, the *same* scripts that insert it during the build
process can *also* run post-build, reinserting Intel ME into the boot ROM. This
is due to licensing issues surrounding the distribution of Intel ME images.

The Libreboot build system fetches it directly from the vendor for a given
machine or set of machines (as an example, Lenovo provides images for several
ThinkPad machines). This ensures that each user gets the exactly same
configuration (the other alternative is to extract Intel ME firmware from the
original vendor image, in the ME region of the flash IC).

You can learn more about this on the following page:
[docs/install/ivy_has_common.md](docs/install/ivy_has_common.md)

The ME firmware is *required* on almost all Intel platforms, or the machine
will turn *off* after 30 minutes. In the neutered setup, the BringUp code of
Intel's ME will disable that 30 minute reset time, allowing you to use your
computer normally even though the ME is *not* running anything after that.

Neutered ME really is disabled
------------------------------

Consider this: if the ME is only doing it's own BringUp, but then not
running anything, is it really anything more than a slight drain on your
battery life? In the neutered state, Intel ME is just an inactive computer
on your mainboard, one that does nothing, that you don't need and that you
will never use. Therefore, it might be considered benign from both a software
freedom perspective and a security perspective, and such is the view taken by
the Libreboot project.

If these assumptions are held, and you agreed with Libreboot's article about
microcode as linked above, and you considered the *fact* that (at least as of
now) Libreboot is capable of purely libre initialisation within coreboot, then
we can reasonably assert that Libreboot provides a decent level of software
freedom. This, in spite of how some may otherwise feel if they *don't* have
such perspective.

So, even though the remaining BringUp code for Intel ME *is* proprietary,
and cannot be modified due to cryptographic signature verification by the
hardware, it's software for a device you'll never want to actually use in
the real world, so if it's *not doing anything* in the neutered state, then
it can be ignored in practise. This depends on your point of view, and some
people may take a more dogmatic approach than this. The Libreboot project
considers neutered ME setups to be acceptable, both from a security perspective
and software freedom perspective.

More about Intel ME removal/disabling
----------------------------------

*Libreboot* provides a way to fully remove the ME firmware, while retaining
full use of the machine, on GM45 platforms with ICH9M southbridge. These are
laptops: ThinkPad X200/T400/T500/W500 and so on of that generation. See:
[docs/install/ich9utils.md](docs/install/ich9utils.md)

On newer platforms, `me_cleaner` is used. You can read about there here:

<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

VGA option ROMs
============

*Native* video initialisation is supported and *enabled*, for all supported
Intel platforms that have it. The source code is provided by coreboot, under
free license.

On *some* machines, dual graphics is possible. For example, certain ThinkPad
T440p mainboards come with both an Intel and an Nvidia GPU, where you can
either use *both* or just the Intel one; to use the Nvidia GPU, a binary blob
is required that Libreboot does not provide (nor will provide), instead opting
to enable only the *Intel* GPU (where libre initialisation code is available
in coreboot). In most T440p mainboards, *only* the Intel GPU is physically
present.

On some ThinkPad T400, W500 and T500 mainboards, an ATI and Intel GPU is
present, and you can use either or. Once again, Libreboot *only* supports the
Intel GPU, where coreboot has libre initialisation code.

This is an example of the nuanced nature in Libreboot's policy. Libreboot
*could* provide such blobs, with the justification that they are *needed* to
use those extra processors. However, in practise, the machines are completely
useable with just Intel graphics for which source code is available.

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

AMD platforms
=============

Libreboot currently *lacks* support for AMD platforms, but information about
them will be written when this fact changes.

ARM platforms (chromebooks)
=============

Mostly blobless, except for the requirement on `daisy` and `peach` mainboards
to include BL1 bootloader blobs. These are:

* HP Chromebook 11 G1 (daisy-spring)
* Samsung Chromebook XE303 (daisy-snow)
* Samsung Chromebook 2 13” (peach-pi)
* Samsung Chromebook 2 11” (peach-pit)

Libreboot *currently* does not accomodate these blobs at all, and this is
considered to be a *bug*; such is documented on the hardware compatibility
page. They can be added manually by the user, but documentation for this is
currently lacking in the Libreboot website.

List of blobs needed, specifically for each board
=================================================

This article has thoroughly explained, in a detailed overview, the precise
nature as to *what* binary blobs are accomodated for in Libreboot. Again,
fully libre init from coreboot is available *on all currently supported boards*.

*From coreboot* is the critical aspect of this, but Libreboot's full scope is
the main flash IC which (in some cases) contains software outside of coreboot.

Here is a list, *for each* board, of those blobs:

Intel/x86
---------

Neutered ME required on these targets: `t420_8mb`, `t420s_8mb`, `t430_12mb`,
`t440p_12mb`, `t440pmrc_12mb`, `t520_8mb`, `t530_12mb`, `w530_12mb`,
`w541_12mb`, `w541mrc_12mb`, `x220_8mb`, `x230_12mb`, `x230_16mb`,
`x230edp_12mb`, `x230t_12mb`, `x230t_16mb`, `hp8200sff`, `hp2560p_8mb`
and `hp9470m_16mb`.

KBC1126 EC firmware on `hp9470m_16mb` and `hp9470m_16mb` - on HP laptops, EC
firmware is inserted into main boot flash, rather than being on a separate IC.
This is *better* because libre replacements would be more easy to install in
the future, and reverse engineering is made much easier by it. Libreboot's
build system handles such firmware in `blobutil`, automatically downloading
it during the build process - releases after Libreboot 20230413, which lacks
such EC integration for HPs, will scrub EC firmware and provide functionality
in `blobutil` insert, to insert them with `cbfstool` at the correct offset as
defined by coreboot config for each board.

As stated, Libreboot provides this in a state where the ME is no longer a
threat to security. It initialises itself, but then does nothing, so it's
disabled. This is done using `me_cleaner`. See:
<https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F>

*Microcode* updates for CPU provided on *all* x86 platforms, by default. Not
technically required, but highly recommended. To remove, do:

	cbfstool filename.rom remove -n cpu_microcode_blob.bin

Removal of microcode updates will affect system stability in a negative way,
introducing non-standard broken behaviour and it may result in the machine
being unable to boot properly. In other cases, doing so may break features such
as S3 suspend/resume.

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

CPU microcode blobs included by default, on all x86 boards. While not needed
in most cases, their use is highly recommended. For reasons why, see:
[news/policy.md#more-detailed-insight-about-microcode](news/policy.md#more-detailed-insight-about-microcode)

ARM/chromebooks
---------------

BL1 bootloader needed on: `daisy_snow`, `daisy_spring` and `peach_pit`.

Conclusion
==========

From the above, you can see that Libreboot really *does* implement a *binary
blobs reduction policy*, with the emphasis on *reduction* being most critical.

Libreboot *could* add a lot of blobs for various platforms, to enable various
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
