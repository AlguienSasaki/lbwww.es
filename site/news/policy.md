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

Examples of Libreboot policy in action
======================================

More information about this is also available on
the [freedom status](../freedom-status.md) page.

Context is everything, so to make the above picture more clear, here are a few
examples of how this very policy is implemented in Libreboot:

* On many laptops, it is possible to have both Intel graphics and a dedicated
  graphics card enabled; more about Intel graphics is covered below. We could
  provide the VGA ROM for those extra chips, e.g. Nvidia graphics chips, but
  we don't. We configure the machines in such a way where the Intel graphics
  is used, exclusively, so the dedicated GPU may aswell be turned off, and
  it *is* turned off in such circumstances. The Intel graphics provide good
  enough performance and reliable operation for most people, and native
  initialisation code is available from coreboot to make use of the Intel GPU
  at boot time.
* Newer Intel platforms have Intel ME, which is a coprocessor inside the
  machine that handles many functions including certain power management, and
  it can provides useful remote management features with Intel AMT for example.
  This is a giant 5MB+ blob in the flash, and the ME has full access to your
  RAM and other peripherals; more is documented about this in the Libreboot
  FAQ. On older ME4-based platforms such as Intel GM45 platforms, e.g.
  ThinkPad X200/T400, we remove the ME firmware entirely from the flash, leaving
  the Intel ME in a permanent reset loop. On Intel Sandybridge and up, we
  include ME firmware when building, or provide a script to insert it via
  vendor download on release images, but Libreboot's build system automatically
  runs the [me cleaner](https://github.com/corna/me_cleaner) software, which
  removes most of the features from the ME, including its kernel and networking
  stack, leaving only a rump (dare we say, ROMP) of initialisation code in
  the ME; essentially, the configuration is equivalent to running coreboot
  without a payload, where it does nothing other than initialise itself, because
  that is all the ME does in this configuration, initialise itself but not
  actually run anything. ME Cleaner is required, instead of full removal,
  because the machines don't boot reliably without at least the BUP/ROMP
  modules present in flash.
* On Intel Haswell platform (Intel 4th gen), a binary blob is available for
  memory controller initialisation, called the Intel *MRC* or System Agent. We
  previously included this in Libreboot; nowadays, we have free initialisation
  code written by Angel Pons. The libre code, called NRI (Native RAM
  Initialisation) still has a few bugs, such as lack of ECC support, and the
  MRC also does other things such as initialise certain peripherals. The
  Haswell machines can be configured in EHCI or XHCI mode for USB ports at
  startup, and the latter means that you need xHCI support in whatever you're
  using such as Linux, but also payloads such as U-Boot or GRUB. SeaBIOS and
  GRUB have stable xHCI support (we have to patch GRUB to have it because
  otherwise upstream doesn't have it). U-Boot's xHCI support is quite buggy,
  at least as of November 2024; when tested, it looks like the EHCI mode
  doesn't work on NRI, only MRC. Now, to get more stable U-Boot support when
  using USB devices, we could use the MRC and set the machine in EHCI mode
  at startup; an option exists for it in coreboot. Ditto, we could have ECC
  support (useful on the Dell Precision T1700, which Libreboot supports!) - it
  was decided that these issues are quite minor so the free initialisation code
  is used, and we no longer provide the MRC.
* Libreboot provides native graphics initialisation on most Intel hardware,
  provided by coreboot. This replaces the Intel VGA ROM; called libgfxinit,
  coreboot's native code is far from perfect because many legacy VGA functions
  simply do not work so it can be a challenge to get old, legacy operating
  systems working, even if they had full driver support on a given machine
  when running the vendor BIOS. Libgfxinit otherwise provides a sensible
  framebuffer implementation from coreboot, which Linux and BSD systems know
  how to use, and you can use KMS on these systems instead of calls to VGA
  in the BIOS, to mitigate this issue. It was decided a long time ago that,
  when available, the native initialisation should be used, when it is stable
  and Linux/BSD systems work reliably. There is only one exception: the
  ThinkPad T480 currently doesn't work with libgfxinit in Libreboot, at least
  as of November 2024, so the Intel VGA ROM is used instead.
* On RK3399-based Chromebooks, it is possible to load a HDCP blob, which could
  be convenient for easy access to DRM-based media. We simply do not use this
  blob on any machines, because it is completeley unnecessary for everyday
  operating of a free operating system.

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

Why?
====

This next section previously existed in a less than diplomatic manner. It
has been restored, as of August 2024, because the wisdom that it provides is
important, yet being respectful of our friends in Massachussets is also
a good thing to do, where feasible. This section was previously deleted, as
a gesture of good will to those people, but it can't not be here, so without
further ado:

Firstly, observe the following graphic:

![](https://av.libreboot.org/firmware.png)

Why does this policy page need to be written? Isn't it just describing basic
common sense? The common sense that free software activism must demand all
software to be free; why even talk about it?

This page has talked about Libreboot's *blob reduction policy*, but more
context is needed. We need to talk about it, because there are many different
interpretations for the exact same argument, depending on your point of view.

If you use a piece of hardware in Linux, and it works, you might see that it has
free drivers and think nothing else. You will run free application software
such as Firefox, Vim, KDE Plasma desktop, and everything is wonderful, right?

Where drivers and applications (and your operating system) are concerned, this
is much clearer, because it's software that you're running on your main CPU,
that you installed yourself. What of firmware?

Libreboot is not the only firmware that exists on your machine, when you have
Libreboot. Look at these articles, which cover other firmwares:

* <https://libreboot.org/faq.html#what-other-firmware-exists-outside-of-libreboot>
* <https://libreboot.org/faq.html#what-level-of-software-freedom-does-libreboot-give-me>

You may ask: should the other firmwares be free too? The answer is **yes**, but
it's complicated: it's not always practical to even study those firmwares. For
example, there are so many webcams out there, so many SSDs, so many devices
all doing the same thing, but implemented differently. Coreboot is already
hard enough, and there are so many mainboards out there.

For example: every SSD has its own controller, and it has to do a lot of
error correction at great speed, to mitigate the inherent unreliability of
NAND flash. This firmware is highly specialised, and tailored to *that* SSD;
not merely that SSD product line, but *that* SSD, because it often has to be
tweaked per SSD; ditto SD cards, which fundamentally use the same technology.
Would it be practical for something like Linux to provide firmware for
absolutely every SSD? No. Absolutely not; and this is actually an example of
where it makes more sense to bake the firmware into the hardware, rather than
supply it as a firmware in Linux (even if the firmware is updateable, which it
is on some SSDs).

Another example: your wireless card implements a software defined radio, to
implement all of the various WiFi protocols, which is what your WiFi drivers
make use of. The drivers themselves are also quite complicated. However, the
same driver might be able to operate multiple wireless cards, if there is
some standard interface (regardless of whether it's documented), that the
same driver can use between all the cards, even if those cards are all very
different; this is where firmware comes in.

Coreboot only covers the main boot firmware, but you will have other firmware
running on your machine. It's simply a fact.

Historically, a lot of hardware has firmware baked into it, which does whatever
it does on that piece of hardware (e.g. software defined radio on a wifi
device, firmware implementing an AHCI interface for your SATA SSD).

In some cases, you will find that this firmware is *not* baked into the device.
Instead, a firmware is provided in Linux, uploaded to the device at boot
time, and this must be performed every time you boot or every time you plug
in that device.

Having firmware in Linux is *good*. Proprietary software is also *bad*, so why
is having *more* proprietary firmware in Linux *good*? Surely, free firmware
would be better, but this firmware has never been free; typically, most
firmware has been non-free, but baked into the hardware so you just didn't
see it. We can demand that the vendors release source code, and we do; in some
cases, we even succeed (for example `ath9k_htc` WiFi dongles have free firmware
available in Linux).

The reason vendors put more firmware in Linux nowadays is it's cheaper. If the
device itself has firmware baked in, then more money is spent on the EEPROM
that stores it, and it makes research/development more expensive; having an
easy software update mechanism allows bugs to be fixed more quickly, during
development and post-release, thus reducing costs. This saves the
industry *billions*, and it is actually of benefit to the free software
community, because it makes reverse engineering easier, and it makes 
actually updating the firmware easier, so more proprietary software can
actually be *replaced with free software*. If some standard interface exists,
for the firmware, then that makes reverse engineering easier *across many
devices*, instead of just one.

Hardware is also very complex, more so now than in the past; having the
hardware be flexible, configured by *firmware*, makes it easier to work
around defects in the hardware. For example, if a circuit for a new feature
is quite buggy on a bit of hardware, but could be turned off without ill
consequence, a firmware update might do exactly that.

The existence of such firmware also reminds more people of that fact, so more
people are likely to demand free software. If the firmware is *hidden in the
hardware*, fewer people are likely to raise a stink about it. We in the
Libreboot project want all firmware to be free, and we've known of this
problem for years.

Some people take what we call the *head in the sand* approach, where any and
all software in Linux must be excluded; certain distros out there do this, and
it is an entirely misguided approach. It is misguided, precisely because it
tells people that *compatible* hardware is more free, when it isn't; more
likely, any hardware that works (without firmware in Linux) likely just has
that same firmware baked into it; in other words, hidden from the user. Hence
the *head in the sand approach* - and this approach would result in far less
hardware being supported.

Libreboot previously had its head in the sand, before November 2022. Since
November 2022, Libreboot has been much more pragmatic, implementing the
policy that you read now, instead of simply banning all proprietary firmware;
the result is increased hardware support, and in practise many of the newer
machines we support are still entirely free in coreboot (including memory
controller initialisation), right up to Intel Haswell generation.

You are advised not to put your head in the sand. Better to see the world as
it is, and here is the actual world as it is:

These firmwares are *required*. In some cases, hardware might have firmware
baked in but provide an update mechanism, e.g. CPU microcode update
mechanism. These firmware updates fix security bugs, reliability issues,
and in some cases even *safety issues* (e.g. thermal safety on a CPU fixed by a
microcode update).

Baking firmware into the device means that the firmware is less likely to be
seen by the user, so fewer people are likely to raise a fuss about it; if
the main boot firmware for example was baked into the PCH on your Intel
system, completely non-replaceable or even inaccessible, fewer people would
demand free boot firmware and a project like coreboot (and by extension
Libreboot) may not even exist!

Such is the paradox of free firmware development. Libreboot previously took
a much more hardline approach, banning absolutely all proprietary firmware
whatsoever; the result was that far fewer machines could be supported. A more
pragmatic policy, the one you've just read, was introduced in November 2022,
in an effort to support more hardware and therefore increase the number of
coreboot users; by extension, this will lead to more coreboot development,
and more proprietary firmware being replaced with free software.

Facts are facts; how you handle them is where the magic happens, and Libreboot
has made its choice. The result since November 2022 has indeed been more
coreboot users, and a lot more hardware supported; more hardware has been
ported to coreboot, that might not have even been ported in the first place,
e.g. more Dell Latitude laptops are supported now (basically all of the
IvyBridge and SandyBridge ones).

The four freedoms are absolute, but the road to freedom is never a straight
line. Libreboot's policies are laser-focused on getting to that final goal,
but without being dogmatic. By being flexible, while pushing for more firmware
to be freed, more firmware is freed. It's as simple as that. We don't want
proprietary software at all, but in order to have less of it, we have to
have more - for now.

Let's take an extreme example: what if coreboot was entirely binary blobs
for a given mainboard? Coreboot itself only initialises the hardware, and
jumps to a payload in the flash; in this case, the payload (e.g. GNU GRUB)
would still be free software. Surely, all free firmware would be better,
but this is still an improvement over the original vendor firmware. The
original vendor firmware will have non-free boot firmware *and* its analog
of a coreboot payload (typically a UEFI implementation running various
applications via DXEs) would be non-free. *Coreboot does* in fact do this
on many newer Intel and AMD platforms, all of which Libreboot intends to
accomodate in the future, and doing so would absolutely comply with this
very policy that you are reading now, namely the Binary Blob Reduction Policy.

You can bet we'll tell everyone that Intel FSP is bad and should be replaced
with free software, and we do; many Intel blobs have in fact been replaced
with Free Software. For example, Libreboot previously provided Intel MRC
which is a raminit blob, on Intel Haswell machines. Angel Pons reverse
engineered the MRC and wrote native memory controller initialisation (raminit)
on this platform, which Libreboot now uses instead of MRC.

This is a delicate balance, that a lot of projects get wrong - they will
accept blobs, and *not* talk about them. In Libreboot, it's the exact
opposite: we make sure you know about them, and tell you that they are bad,
and we say that they should be fully replaced.

Unlike some in the community, we even advocate for free software in cases
where the software can't actually be replaced. For example: the RP2040 Boot ROM
is free software, with public source code:

<https://github.com/raspberrypi/pico-bootrom-rp2040>

This is the boot ROM source code for RP2040 devices such as Raspberry Pi Pico.
It is a reprogrammable device, and we even use it as a
cheap [SPI flasher](../docs/install/spi.md) running `pico-serprog`. The
main firmware is replaceable, but the *boot ROM* is read-only on this machine;
there are some people would would not insist on free software at that level,
despite being free software activists, because they would regard the boot
ROM as "part of the hardware" - in Libreboot, we insist that all such
software, including this, be free. Freedom merely to study the source code
is still an important freedom, and someone might make a replica of the
hardware at some point; if they do, that boot ROM source code is there for
them to use, without having to re-implement it themselves. Isn't that great?

I hope that these examples might inspire some people to take more action in
demanding free software everywhere, and to enlighten more people on the road
to software freedom. The road Libreboot takes is the one less traveled, the
one of pragmatism without compromise; we will not lose sight of our original
goals, namely absolute computer user freedom.

The article will end here, because anything else would be more rambling.
