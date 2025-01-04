---
title: Побудова з джерельного коду
x-toc-enable: true
...

**TODO: This page needs to be re-translated. Much of the newer sections are
still in English, and there may be some differences aside from translation,
versus the English version.**

WARNING: eCryptfs file name limits
=================================

Do not run the build system on a eCryptfs file system, because it has
very short file name limits and Libreboot's build system deals with very
long file names. We commonly get reports from this by Linux Mint users
who encrypt their home directory with eCryptfs; regular LUKS encryption will
do nicely.

Introduction
============

Система побудови libreboot, називається `lbmk`, скорочення від `LibreBoot MaKe`, і цей
документ описує те, як використовувати її. З цим керівництвом ви можете узнати те, як побудувати
libreboot з доступного джерельного коду.
Ця версія, якщо розміщена наживо на libreboot.org, передбачає, що ви використовуєте
сховище git `lbmk`, яке
ви можете завантажити, використовуючи інструкції на [сторінці огляду коду](../../git.uk.md).

Якщо ви використовуєте архів випуску libreboot, будь ласка, зверніться до
документації, включеної до *того* випуску. Випуски libreboot розраховані тільки,
як *знімки*, не для розробки. Для належної розробки ви маєте завжди
працювати безпосередньо в сховищі git libreboot.

Наступний документ описує те, як працює `lbmk`, і як ви можете робити зміни
до нього: [керівництво обслуговування libreboot](../maintain/)

System requirements
===================

This concerns system requirements when *building* Libreboot.

Operating system
----------------

Any sensible Linux distribution will do. Libreboot's build system is regularly
testing on all the major distros. Please do report bugs if you encounter
issues.

Host CPU
--------

At least an Intel Core 2 Duo, though we recommend much faster CPUs if building
entire release archives, e.g. quad-core Haswell CPU or better.

NOTE: x86 boards require an *x86_64* host CPU with appropriate host toolchains
and libraries. We don't yet cross-compile x86 payloads.

NOTE2: ARM64 mainboards *are* cross compiled, so you can build for AArch64
machines quite easily, from x86 or ARM64 machines.

NOTE3: *32-bit* x86 (i686) machines can be used to compile Libreboot, but
MemTest86\+ is only compiled for 64-bit, and not cross compiled, so builds
are disabled when lbmk detects a 32-bit host CPU.

Memory
------

At least 2GB per CPU core, ideally 4GB; for example, 16GB RAM is recommended
if you're compiling an a quad-core CPU.

NOTE: `XBMK_THREADS` environmental variable defaults to 1 if unset. This sets
the number of build threads, which you should match to the number of cores.
For example, when you're building on a quad-core, do this prior to building:

	export XBMK_THREADS=4

Disk space
----------

About 20GB bare minimum, if only compiling for 1 board. The sources take up a
lot of space. However, Libreboot is always expanding as it's developed.

At least 50GB of free disk space is therefor recommended.

We *actually* recommend 100GB, because Libreboot will also have a Linux distro
in flash on a future release. On our testing, disk I/O does not seem to be a
major bottleneck, so any HDD or SSD will do, but we obviously recommend a
fast NVMe (PCI-E) SSD if you can.

Multi-threaded builds
=====================

Libreboot's build system defaults to a single build thread, but you can change
it by doing e.g.

	export XBMK_THREADS=4

This would make lbmk run on 4 threads.

Environmental variables
=======================

Please read about environmental variables in [the build
instructions](../maintain/), before running lbmk. You should set
your variables accordingly, though you do not technically need to; some
of them may be useful, e.g. `XBMK_THREADS` (sets the number of build threads).

Environmental variables
=======================

Please read about environmental variables in [the build
instructions](../maintain/), before running lbmk. You should set
your variables accordingly, though you do not technically need to; some
of them may be useful, e.g. `XBMK_THREADS` (sets the number of build threads).

Git
===

Система побудови Libreboot використовує Git, обширно. Ви маєте виконати кроки
знизу, *навіть, якщо ви використовуєте архів випуску*.

Перед тим, як вам використовувати систему побудови, будь ласка, знайте: система побудови, сама по собі,
використовує Git обширно, коли завантажує програмне забезпечення, таке як coreboot, та проводить застосування виправлень.

Ви маєте переконатись в тому, щоб ініціалізувати ваш Git належним чином, перед тим, як почати, або інакше
система побудови не буде працювати належно. Зробіть це:

	git config --global user.name "John Doe"
	git config --global user.email johndoe@example.com

Змініть ім'я та адресу електронної пошти на будь-яку, що забажаєте, коли робите це.

Ви також можете захотіти прослідувати більшій кількості етапів тут:
<https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup>

Python
======

Python2 не використовується lbmk або будь-чим, що завантажується в якості модулів. Ви
маєте переконатись, що команда `python` виконує python 3 на вашій системі.

Zero..st, check time/date
-------------------------

Make sure date/hwclock report the correct time and date on your system,
because parts of the build process download from HTTPS servers and wrong
time or date can cause connections to be dropped during negotiation.

Побудова з джерельного коду
============================

Фактична розробка/тестування завжди виконується безпосередньо за допомогою `lbmk`, і це також
стосується збирання з джерельного коду. Ось кілька інструкцій, щоб
почати:

libreboot включає сценарій, який автоматично встановлює apt-get залежності
в Ubuntu 20.04:

	sudo ./mk dependencies ubuntu2004

Окремі сценарії також існують:

	sudo ./mk dependencies debian

	sudo ./mk dependencies arch

	sudo ./mk dependencies void

Check: `config/dependencies/` for list of supported distros.

Технічно, будь-який дистрибутив Linux може бути використано для побудови libreboot.
Однак, вам потрібно буде написано свій власний сценарій для встановлення залежностей
побудови. 

Debian Trixie/Sid
-----------------

Debian Trixie, the testing release as of 3 January 2025, and Debian Sid,
provide `gnat` and `gcc` as you expect, but `gnat` resolves to `gnat-13` and
installs `gcc-13` as a dependency, while `gcc` resolves to `gcc-14` and other
toolchain components correspond to this version.

The GCC/GNAT versions need to match during build time, so Libreboot's build
system hacks the `PATH` environmental variable, setting up symlinks, matching
GNAT to GCC or GNAT to GCC. When you run `./mk dependencies debian`, you get
GNAT 13 and GCC 14. This seems to make most boards compile; in our testing, the
KGPE-D16 board failed to compile in this configuration. This PATH hack is only
done for compiling the coreboot crossgcc toolchain, and nothing else; after that,
coreboot's toolchain is used.

GNAT is used by coreboot, because some of the Intel graphics devices are
initialised natively, with code written in Ada spark (called `libgfxinit`).

When updating from Debian stable to Debian Trixie(testing) or Sid, you should
also check for orphaned packages, using `aptitude search '~o'`. Do this,
removing what was leftover from the old release, and make sure to re-run the
Debian dependencies script, but do it like this:

	./mk dependencies debian --reinstall

For better reliability, you should, after running the dependencies script,
remove `gnat` and install `gnat-14` instead, which is available on this day
of 3 December 2025, but currently marked experimental. When you install
GNAT 14, GNAT 13 is removed but `gnat` (in `PATH`) still won't resolve to
anything. Libreboot *still* accomodates this, detecting and matching the GCC
and GNAT versions, which would in this instance match version 14 between them,
so that `gnat` and `gcc` are both in PATH at build time, resolving to v14.x.
When we tested with this configuration, the KGPE-D16 images also compiled.

NOTE: Ubuntu 24.10 also has the issue described above. Some other distros may
also have it, if they're based on Debian Testing/Sid or Ubuntu 24.10.

MIPS cross compiler
-------------------

Libreboot has support for the Sony PlayStation (PS1/PSX), based on
the PCSX-Redux Open BIOS. If you're doing a full release build, and/or
specifically building the PSX BIOS, you need a MIPS cross compiler.

Arch-based systems have a mipsel cross compiler available from AUR, and most
Debian-based systems have a mipsel cross compiler in apt; for these, the normal
dependencies installation command will provide them. We know Void Linux and
Fedora don't have a MIPS compiler, for instance.

If your distro doesn't have the MIPS compiler available,
the [PlayStation](../install/playstation.md) page provides instructions for
manual installation; please do this in addition to the normal dependencies.

Next, build ROM images
----------------------

В якості результату, ви тепер можете (після встановлення правильних залежностей побудови) виконати
лише одну команду, з свіжого Git clone, для побудови образів ROM:

	./mk -b coreboot

або навіть побудувати конкретні образи ROM, такі як:

	./mk -b coreboot x60

or get a list of supported build targets:

	./mk -b coreboot list

Якщо ви бажаєте побудувати корисні навантаження, можете зробити це. Наприклад:

	./mk -b grub

	./mk -b seabios

	./mk -b u-boot

Попередні кроки буде виконано автоматично. Однак, ви можете *досі* виконати
окремі частини системи побудови власноруч, якщо виберете. Це може бути
вигідно, коли ви робите зміни, та бажаєте протестувати конкретну частину
lbmk.
