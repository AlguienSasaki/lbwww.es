---
title: Libreboot project
x-toc-enable: true
...

The *Libreboot* project provides
[free, open source](freedom-status.md) (*libre*) boot
firmware based on coreboot, replacing proprietary BIOS/UEFI firmware
on [specific Intel/AMD x86 and ARM based motherboards](docs/hardware/),
including laptop and desktop computers. It initialises the hardware (e.g. memory
controller, CPU, peripherals) and starts a bootloader for your operating
system. [Linux](docs/linux/) and [BSD](docs/bsd/) are well-supported. Help is
available via [\#libreboot](https://web.libera.chat/#libreboot)
on [Libera](https://libera.chat/) IRC.

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

**NEW RELEASE: The latest release is Libreboot 20231101, released on
1 November 2023.
See: [Libreboot 20231101 release announcement](news/libreboot20231101.md).**

Why should you use *Libreboot*?
----------------------------

Libreboot gives you [freedoms](https://writefreesoftware.org/) that
you otherwise can't get with most other boot firmware, plus faster boot speeds
and [better security](docs/linux/grub_hardening.md). It's extremely powerful
and [configurable](docs/maintain/) for many use cases.

*We* believe the freedom to [study, share, modify and use
software](https://writefreesoftware.org/), without any
restriction, is one of the fundamental human rights that everyone must have.
In this context, *software freedom* matters. Your freedom matters. Education
matters.
[Right to repair](https://yewtu.be/watch?v=Npd_xDuNi9k) matters.
Many people use proprietary (non-libre)
boot firmware, even if they use [a libre OS](https://www.openbsd.org/).
Proprietary firmware often [contains](faq.html#intel) [backdoors](faq.html#amd),
and can be buggy. The Libreboot project was founded in December 2013, with the
express purpose of making coreboot firmware accessible for non-technical users.

The Libreboot project uses [coreboot](https://www.coreboot.org/) for [hardware
initialisation](https://doc.coreboot.org/getting_started/architecture.html).
Coreboot is notoriously difficult to install for most non-technical users; it
handles only basic initialization and jumps to a separate
[payload](https://doc.coreboot.org/payloads.html) program (e.g.
[GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), which must also be configured.
*Libreboot solves this problem*; it is a *coreboot distribution* with
an [automated build system](docs/build/) that builds complete *ROM images*, for
more robust installation. Documentation is provided.

Libreboot is not a fork of coreboot
-----------------------------------

<img tabindex=1 class="l" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

In fact, Libreboot tries to stay as close to *stock* coreboot as possible,
for each board, but with many different types of configuration provided
automatically by the Libreboot build system.

In the same way that *Alpine Linux* is a *Linux distribution*, Libreboot is
a *coreboot distribution*. If you want to build a ROM image from scratch, you
otherwise have to perform expert-level configuration of coreboot, GRUB and
whatever other software you need, to prepare the ROM image. With *Libreboot*,
you can literally download from Git or a source archive, and run `make`, and it
will build entire ROM images. An automated build system, named `lbmk`
(Libreboot MaKe), builds these ROM images automatically, without any user input
or intervention required. Configuration has already been performed in advance.

If you were to build regular coreboot, without using Libreboot's automated
build system, it would require a lot more intervention and decent technical
knowledge to produce a working configuration.

Regular binary releases of Libreboot provide these
ROM images pre-compiled, and you can simply install them, with no special
knowledge or skill except the ability to
follow [simplified instructions, written for non-technical
users](docs/install/).

How to help
-----------

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

The *single* biggest way you can help is to *add* new mainboards in Libreboot,
by submitting a config. Anything coreboot supports can be integrated in
Libreboot, with ROM images provided in releases. See:

* [Apply to become a board maintainer/tester](docs/maintain/testing.md)
* [Porting guide for new mainboards](docs/maintain/porting.md)
* [Libreboot build system documentation](docs/maintain/)

After that, there is build system maintenance (see above), and *documentation*
which we take seriously. Documentation is critical, in any project.

*User support* is also critical. Stick around on IRC, and if you're competent
to help someone with their issue (or wily enough to learn with them), that is
a great service to the project. A lot of people also ask for user support
on the `r/libreboot` subreddit.

You can check bugs listed on
the [bug tracker](https://codeberg.org/libreboot/lbmk/issues).

If you spot a bug and have a fix, [here are instructions for how to send
patches](git.md), and you can also report it. Also, this entire website is
written in Markdown and hosted in a [separate
repository](https://codeberg.org/libreboot/lbwww) where you can send patches.

Any and all development discussion and user support are all done on the IRC
channel. More information is on the [contact page](contact.md).

Translations needed, for libreboot.org
--------------------------------------

Libreboot currently has translated Web pages in Ukrainian and French (but not
for all pages, yet, on either language).

If you want to help with translations, you can translate pages, update existing
translations and submit your translated versions. For instructions, please
read:

[How to submit translations for libreboot.org](news/translations.md)

Even if someone is already working on translations in a given language, we can
always use multiple people. The more the merrier!
