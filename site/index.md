---
title: Libreboot project
x-toc-enable: true
...

The `libreboot` project provides
[libre](https://en.wikipedia.org/wiki/Open_source) *boot
firmware* that initializes the hardware (e.g. memory controller, CPU,
peripherals) on [specific Intel/AMD x86 and ARM targets](docs/hardware/), which
then starts a bootloader for your operating system. [GNU+Linux](docs/gnulinux/)
and [BSD](docs/bsd/) are well-supported. It replaces proprietary BIOS/UEFI
firmware. Help is available
via [\#libreboot](https://web.libera.chat/#libreboot)
on [Libera](https://libera.chat/) IRC.

**NEW RELEASE: The latest release is Libreboot 20221214, released on
14 December 2022.
See: [Libreboot 20221214 release announcement](news/libreboot20221214.md).**

Why should you use *libreboot*?
----------------------------

Libreboot gives you freedoms that you otherwise can't get with most other
boot firmware. It's extremely powerful and configurable for many use cases.

You have rights. The right to privacy, freedom of thought, freedom of speech
and the right to read. In this context, Libreboot gives you these rights.
Your freedom matters.
[Right to repair](https://vid.puffyan.us/watch?v=Npd_xDuNi9k) matters.
Many people use proprietary (non-libre)
boot firmware, even if they use [a libre OS](https://www.openbsd.org/).
Proprietary firmware often [contains](faq.html#intel) [backdoors](faq.html#amd),
and can be buggy. The libreboot project was founded in in December 2013, with the
express purpose of making coreboot firmware accessible for non-technical users.

The `libreboot` project uses [coreboot](https://www.coreboot.org/) for [hardware
initialisation](https://doc.coreboot.org/getting_started/architecture.html).
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
(Libreboot MaKe), builds these ROM images automatically, without any user input
or intervention required. Configuration has already been performed in advance.

If you were to build regular coreboot, without using libreboot's automated
build system, it would require a lot more intervention and decent technical
knowledge to produce a working configuration.

Regular binary releases of `libreboot` provide these
ROM images pre-compiled, and you can simply install them, with no special
knowledge or skill except the ability to
follow [simplified instructions, written for non-technical
users](docs/install/).

How to help
-----------

You can check bugs listed on
the [bug tracker](https://notabug.org/libreboot/lbmk/issues).

If you spot a bug and have a fix, [here are instructions for how to send
patches](git.md), and you can also report it. Also, this entire website is
written in Markdown and hosted in a [separate
repository](https://notabug.org/libreboot/lbwww) where you can send patches.

Any and all development discussion and user support are all done on the IRC
channel. More information is on the [contact page](contact.md).
