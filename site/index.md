---
title: libreboot project
x-toc-enable: true
...

The `libreboot` project provides
[freedom-respecting](https://www.gnu.org/philosophy/free-sw.html) *boot
firmware* that initializes the hardware (e.g. memory controller, CPU,
peripherals) on [specific Intel/AMD x86 computers](docs/hardware/) and starts
a bootloader for your operating system. [GNU+Linux](docs/gnulinux/)
and [BSD](docs/bsd/) are well-supported. It replaces proprietary BIOS/UEFI
firmware. Help is available
via [\#libreboot](https://web.libera.chat/#libreboot)
on [Libera](https://libera.chat/) IRC.

Why should you use *libreboot*?
----------------------------

You have rights. The right to privacy, freedom of thought, freedom of speech
and the right to read. [Free
software](https://www.gnu.org/philosophy/free-sw.html) gives you these rights.
Your freedom matters.
[Right to repair](https://vid.puffyan.us/watch?v=Npd_xDuNi9k) matters.
Many people use [proprietary](https://www.gnu.org/proprietary/proprietary.html)
boot firmware, even if they use [GNU+Linux](https://www.gnu.org/distros/).
Non-free firmware often [contains](faq.html#intel) [backdoors](faq.html#amd),
and can be buggy. The libreboot project was founded in in December 2020, with the
express purpose of making Free Software accessible for non-technical users at
the firmware level. It's true that `libreboot` can be called Open Source, [but you
should call it Free
Software](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html).

The `libreboot` project uses [coreboot](https://www.coreboot.org/) for [hardware
initialization](https://doc.coreboot.org/getting_started/architecture.html).
Coreboot is notoriously difficult to install for most non-technical users; it
handles only basic initialization and jumps to a separate
[payload](https://doc.coreboot.org/payloads.html) program (e.g.
[GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), which must also be configured.
*The libreboot software solves this problem*; it is a *coreboot distribution* with
an [automated build system](docs/build/) that builds complete *ROM images*, for
more robust installation. Documentation is provided.

How does libreboot differ from regular coreboot?
---------------------------------------------

In the same way that *Debian* is a GNU+Linux distribution, `libreboot` is
a *coreboot distribution*. If you want to build a ROM image from scratch, you
otherwise have to perform expert-level configuration of coreboot, GRUB and
whatever other software you need, to prepare the ROM image. With *libreboot*,
you can literally download from Git or a source archive, and run `make`, and it
will build entire ROM images. An automated build system, named `lbmk`
(OSBoot MaKe), builds these ROM images automatically, without any user input
or intervention required. Configuration has already been performed in advance.

If you were to build regular coreboot, without using libreboot's automated
build system, it would require a lot more intervention and decent technical
knowledge to produce a working configuration.

Reguar binary releases of `libreboot` provide these
ROM images pre-compiled, and you can simply install them, with no special
knowledge or skill except the ability to
follow [simplified instructions, written for non-technical
users](docs/install/).

How does libreboot differ from *Libreboot*?
----------------------------------------

Libreboot and libreboot are both developed in parallel. Both projects were founded
by Leah Rowe, who leads both projects.

**The libreboot project is a fork of Libreboot, but it has scrapped the [Libreboot
zero-blob policy](news/policy.md). It comes with CPU microcode updates turned
on by default, even on libreboot-compatible hardware (on libreboot-compatible
hardware, that is the only difference). The libreboot build system automatically
downloads the entire set of `3rdparty` submodules from coreboot. The coreboot
software is nominally free, but does require some binary blobs on certain
machines, and those are included in the `3rdparty` submodules.**

[CPU microcode updates do not hurt your freedom, because your CPU already has
older, buggier microcode in mask ROM anyway. You should choose libreboot, not
Libreboot, even on Libreboot-compatible hardware, because the microcode updates
improve system stability and reliability.](news/policy.md) Out of principle,
`libreboot` will always enable microcode updates. Libreboot is inferior to libreboot,
in every way, but it will continue to be developed and polished, alongside
libreboot development.

The purpose of `libreboot` is to provide as
much freedom as possible, to those who wish to move away from their otherwise
fully proprietary firmware. The `libreboot` build system does not delete binary
blobs like Libreboot's one does, because it *wants* to provide help for all
those who wish to have some freedoms over their hardware, even if that hardware
isn't supported by Libreboot yet. Libreboot compatibility is still very much
desirable, on all hardware, and work to this end is highly encouraged!

You can learn more by reading libreboot's enlightened [binary blobs
policy](news/policy.md) which is in stark contrast to Libreboot's policy.
The libreboot project removes all restrictions in its fork of the Libreboot
build system, allowing any board from coreboot to be supported (the goal
is literally to support all of them).

How to help
-----------

You can check bugs listed on
the [bug tracker](https://notabug.org/libreboot/lbmk/issues).

If you spot a bug and have a fix, [here are instructions for how to send
patches](git.md), and you can also report it. Also, this entire website is
written in Markdown and hosted in a [separate
repository](https://notabug.org/libreboot/osbwww) where you can send patches.

Any and all development discussion and user support are all done on the IRC
channel. More information is on the [contact page](contact.md).
