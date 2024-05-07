% Binary Blob Reduction Policy
% Leah Rowe
% 4 January 2022 (updated 15 November 2022)

Introduction
============

This article describes the *principles* that govern the Libreboot project. For
information about *how those principles are applied in practise*, please read
this article instead: [Software and hardware freedom status for each mainboard
supported by Libreboot](../freedom-status.md)

Libreboot's policy is to provide as much
[software freedom](https://writefreesoftware.org/) as possible to each
user, on each and every bit of hardware supported, and to *support as much
hardware from coreboot as is feasible*; what this means is that you should
have the potential to study, modify and *share* all source code, documentation
or other such resources that make Libreboot what it is. Put simply, you should
have *control* of your own computing.

The *goal* of Libreboot is
to do exactly this, and help as many people as possible by automating the
configuration, compilation and installation of *coreboot* for *non-technical*
users, easing it further for the average user by providing user-friendly
instructions for everything. Essentially, Libreboot is a *coreboot
distribution*, in much the same way *Alpine Linux* is a Linux distribution!

The purpose of this document it to outline how that is brought about, and how
the project operates along this basis. *This* document is largely about the
ideology and it is therefore (mostly) non-technical; for technical information,
you can refer to the [Libreboot build system documentation](../docs/maintain/).

Current project scope
=====================

The libreboot project is concerned with what goes in the main boot flash IC, but
there are other pieces of firmware to take into consideration, as covered
in the [libreboot FAQ](../faq.md#what-other-firmware-exists-outside-of-libreboot).

Most critical of these are:

* Embedded controller firmware
* HDD/SSD firmware
* Intel Management Engine / AMD PSP firmware

What is a binary blob?
----------------------

A binary blob, in this context, is any executable for which no source code
exists, that you cannot study and modify in a reasonable manner. By definition,
all such blobs are *proprietary* in nature, and should be avoided if possible.

For information about Intel Management Engine and AMD PSP, refer to the FAQ.

Blob *reduction* policy
=======================

Default configurations
----------------------

Coreboot, upon which Libreboot is based, is mostly libre software but does
require certain vendor code on some platforms. A most common example might be raminit
(memory controller initialisation) or video framebuffer initialisation. The
coreboot firmware uses certain vendor code for some of these tasks, on some mainboards,
but some mainboards from coreboot can be initialised with 100% libre source
code, which you can inspect, and compile for your use.

Libreboot deals with this situation in a *strict* and *principled* way. The
nature of this is what you're about to read.

The libreboot project has the following policy:

* If free software *can* be used, it *should* be used. For example, if VGA ROM
  initialization otherwise does a better job but coreboot has *libre* init code
  for a given graphics device, that code should be used in libreboot, when
  building a ROM image. Similarly, if *memory controller initialization* is
  possible with vendor code *or* libre code in coreboot, the *libre* code
  should be used in ROMs built by the Libreboot build system, and the *vendor*
  raminit code should not be used; however, if no libre init code is available
  for said raminit, it is permitted and Libreboot build system will use the
  *vendor* code.
* Some nuance is to be observed: on some laptop or desktop configurations, it's
  common that there will be *two* graphics devices (for example, an nvidia and
  an intel chip, using nvidia optimus technology, on a laptop). It may be that
  one of them has libre init code in coreboot, but the other one does not. It's
  perfectly acceptable, and desirable, for libreboot to support both devices,
  and accomodate the required vendor code on the one that lacks native
  initialization.
* An exception is made for CPU microcode updates: they are permitted, and in
  fact *required* as per libreboot policy. These updates fix CPU bugs, including
  security bugs, and since the CPU already has non-libre microcode burned into
  ROM anyway, the only choice is either *x86* or *broken x86*. Thus, libreboot
  will only allow coreboot mainboard configurations where microcode updates
  are *enabled*, if available for the CPU on that mainboard.
  [Releases after 20230423 will provide separate ROM images with microcode
  excluded, alongside the default ones that include microcode.](microcode.md)
* Intel Management Engine: in the libreboot documentation, words *must* be written
  to tell people how to *neuter* the ME, if possible on a given board.
  The `me_cleaner` program is very useful, and provides a much more secure ME
  configuration.
* Vendor blobs should *never* be deleted, even if they are unused. In the
  coreboot project, a set of `3rdparty` submodules are available, with vendor
  code for init tasks on many boards. These must *all* be included in libreboot
  releases, even if unused. That way, even if the Libreboot build system does
  not yet integrate support for a given board, someone who downloads libreboot
  can still make changes to their local version of the build system, if they
  wish, to provide a configuration for their hardware.

Generally speaking, common sense is applied. For example, an exception to the
minimalization might be if *vendor* raminit and *libre* raminit are available, but
the *libre* one is so broken so as to be unusable. In that situation, the vendor
one should be used instead, because otherwise the user might switch back to an
otherwise fully proprietary system, instead of using coreboot (via libreboot).
*Some* freedom is *better than none*.

Libreboot's pragmatic policies will inevitably result in more people becoming
coreboot developers in the future, by acting as that crucial *bridge* between
*it* and non-technical people who just need a bit of help to get started.

Configuration
-------------

The principles above should apply to *default* configurations. However, libreboot
is to be *configurable*, allowing the user to do whatever they like.

It's natural that the user may want to create a setup that is *less* libre than
the default one in libreboot. This is perfectly acceptable;
[freedom](https://writefreesoftware.org/) is superior,
and should be encouraged, but the user's *freedom to choose* should also be
respected, and accomodated.

In other words, do not lecture the user. Just try to help them with their
problem! The goal of the libreboot project is simply to make coreboot more
accessible for otherwise non-technical users.

FREEDOM CATALOG
===============

A *[freedom status](../freedom-status.md)* page should also be made available,
educating people about the software freedom status on each machine supported by
the Libreboot build system. Please read:
[Software and hardware freedom status for each mainboard supported by
Libreboot](../freedom-status.md).

It is desirable to see a world where all hardware and software is libre, under
the same ideology as the Libreboot project. Hardware!?

Yes, hardware. RISC-V is a great example of a modern attempt at libre hardware,
often called *Open Source Hardware*.
It is a an ISA for the manufacture of a microprocessor. Many real-world
implementations of it already exist, that can be used, and there will only be
more.

Such *hardware* is still in its infancy. We should start a project that will
catalog the status of various efforts, including at the hardware level (even
the silicon level). Movements like OSHW and Right To Repair are extremely
important, including to our own movement which otherwise will
typically think less about hardware freedoms (even though it really, really
should!)

One day, we will live in a world where anyone can get their own chips made,
including CPUs but also every other type of IC. Efforts to make homemade
chip fabrication a reality are now in their infancy, but such efforts do
exist, for example, the work done by Sam Zeloof and the Libre Silicon project:

* <https://www.youtube.com/channel/UC7E8-0Ou69hwScPW1_fQApA>
* <http://sam.zeloof.xyz/>
* <https://libresilicon.com/>

(Sam literally makes CPUs in his garage)
