---
title: Libreboot project
x-toc-enable: true
...

*Libreboot* is a *[coreboot distribution](docs/maintain)* (coreboot distro),
in the same way that Debian is a *Linux distribution*. Libreboot provides
[free, open source](https://writefreesoftware.org/) (*libre*) boot
firmware based on coreboot, replacing proprietary BIOS/UEFI firmware
on [specific Intel/AMD x86 and ARM based motherboards](docs/install/#which-systems-are-supported-by-libreboot),
including laptop and desktop computers. It initialises the hardware (e.g. memory
controller, CPU, peripherals) and starts a bootloader for your operating
system. [Linux](docs/linux/) and [BSD](docs/bsd/) are well-supported. Help is
available via [\#libreboot](https://web.libera.chat/#libreboot)
on [Libera](https://libera.chat/) IRC. The [tasks page](tasks/) lists what
we are working on (contributions are welcome, if you want to work on some of
them yourself).

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

You can also [buy Libreboot preinstalled](https://minifree.org/) from Minifree Ltd,
on select hardware, aswell as send your compatible hardware
for [Libreboot preinstallation](https://minifree.org/product/installation-service/).
The founder and lead developer of Libreboot, Leah Rowe, also owns and operates
Minifree; sales provide funding for Libreboot.

**NEW RELEASE: The latest release is Libreboot 20241206, released on
6 December 2024.
See: [Libreboot 20241206 release announcement](news/libreboot20241206.md) - the
newest revision release, Libreboot 20241206 revision 8, is from 6
January 2025.**

*We* believe the freedom to [study, share, modify and use
software](https://writefreesoftware.org/), without any
restriction, is one of the fundamental human rights that everyone must have.
In this context, *software freedom* matters. Your freedom matters. Education
matters.
[Right to repair](https://en.wikipedia.org/wiki/Right_to_repair) matters; Libreboot lets
you continue to use your hardware, with continued firmware updates. All of this
is *why* Libreboot exists.

Overview of Libreboot design
============================

<img tabindex=1 class="l" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot provides [coreboot](https://coreboot.org/) for [machine
initialisation](https://doc.coreboot.org/getting_started/architecture.html),
which then jumps to a [payload](https://doc.coreboot.org/payloads.html) in
the boot flash; coreboot works with many payloads, but Libreboot specifically
provides SeaBIOS, GNU GRUB and U-Boot as options. Memtest86+ is also provided
in flash, on some mainboards. The payload is the program in flash that provides
the early user interface, for booting an operating system. This *payload*
infrastructure means you can run whatever you want (even Linux!) from the
boot flash.

Libreboot makes coreboot easy to use for
non-technical users, by providing a [fully automated build
system](docs/maintain/), [automated build process](docs/build/) and
[user-friendly installation instructions](docs/install/), in addition to
regular binary releases that provide pre-compiled ROM images for installation
on supported hardware. Without automation such as that provided by Libreboot,
coreboot would be inaccessible for most users; you can also
still [reconfigure](docs/maintain/) Libreboot however you wish.

Why use Libreboot?
==================

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

If you're already inclined towards free software, maybe already a coreboot user,
Libreboot makes it easier to either get started or otherwise maintain coreboot
on your machine, via build automation. It provides regular tested releases,
pre-assembled, often with certain patches on top of coreboot (and other code)
to ensure stability. By comparison, coreboot uses a rolling-release model, with
a snapshot of the codebase every few months; it is very much developer-oriented,
whereas Libreboot is specifically crafted for end users. In other words, the
purpose of Libreboot is to *Just Work*. Direct configuration and installation
of coreboot is also possible, but Libreboot makes it *much* easier.

Libreboot gives you [freedoms](https://writefreesoftware.org/) that
you otherwise can't get with most other boot firmware, plus faster boot speeds
and [better security](docs/linux/grub_hardening.md). It's extremely powerful
and [configurable](docs/maintain/) for many use cases. If you're unhappy with
the restrictions (not to mention, security issues) imposed on you by proprietary
BIOS vendors, then Libreboot is one possible choice for you. Since it inherits
coreboot, it doesn't have any known backdoors in the code, nor does it contain
legacy cruft from the 1980s. Libreboot provides a sleek, fast boot experience
for Linux/BSD systems, based on coreboot which is regularly audited and improved.

Libreboot is more reliable than proprietary firmware. Many people use proprietary
(non-libre) boot firmware, even if they use [a libre OS](https://www.openbsd.org/).
Proprietary firmware often [contains](faq.html#intel) [backdoors](faq.html#amd),
and can be buggy. The Libreboot project was founded in December 2013, with the
express purpose of making coreboot firmware accessible for non-technical users.

Libreboot is a community-oriented project, with a focus on helping users escape
proprietary boot firmware; we ourselves want to live in a world where all software
is [free](https://writefreesoftware.org/), and so, Libreboot is an effort to
help get closer to that world. Unlike the big vendors, we don't try to stifle
you in any way, nor do we see you as a security threat; we regard the ability
to use, study, modify and redistribute software freely to be a human right that
everyone must have. Extended to computers, these are products that you purchased,
and so you should have the freedom to change them in any way you like. When you
see Intel talk about their *Boot Guard* (which prevents coreboot by only letting
firmware signed by them be executed) or other vendors imposing similar
restrictions, and you hear them talk about "security", they are only talking
about *their* security, not yours. In the Libreboot project, it is reversed; we
see Intel Boot Guard and similar such technologies as an attack on your freedom
over your own property (your computer), and so, we make it our mission to help
you [wrest](https://trmm.net/TOCTOU/) back such control.

Libreboot is not a fork of coreboot
===================================

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/t1650/t1650_grub.jpg" /><span class="f"><img src="https://av.libreboot.org/t1650/t1650_grub.jpg" /></span>

In fact, Libreboot tries to stay as close to *stock* coreboot as possible,
for each board, but with many different types of configuration provided
automatically by the Libreboot build system.

In the same way that *Alpine Linux* is a *Linux distribution*, Libreboot is
a *coreboot distribution*. If you want to build a ROM image from scratch, you
otherwise have to perform expert-level configuration of coreboot, GRUB and
whatever other software you need, to prepare the ROM image. With *Libreboot*,
you can literally download from Git or a source archive, and run a simple script,
and it will build entire ROM images. An automated build system, named `lbmk`
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

The [tasks page](tasks/) lists tasks that could (will) be worked on. It will
be updated over time as more tasks are added/completede. If you want to help,
you could pick one of these tasks and work on it.

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
