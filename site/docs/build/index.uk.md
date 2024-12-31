---
title: Побудова з джерельного коду
x-toc-enable: true
...

WARNING: eCryptfs file name limits
=================================

Do not run the build system on a eCryptfs file system, because it has
very short file name limits and Libreboot's build system deals with very
long file names. We commonly get reports from this by Linux Mint users
who encrypt their home directory with cryptfs; use a serious distro like
Debian or Arch please. And use a decent file system; ZFS or perhaps LUKS
with btrfs will do nicely.

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

Release status
==============

Information about status will be reported during builds; if a board is
marked as stable, the build proceeds without further input. If the board is
marked anything other, a warning appears asking if you wish to proceed; to
disable these warnings, do this before building (not recommended):

	export XBMK_STATUS=n

In Libreboot, we specify: `stable`, `unstable`, `broken` or `untested`.
The "unstable" marking means that the board boots mostly/entirely reliably
annd should be safe to use, but may have a few issues, but nothing which would,
for example, cause safety issues e.g. thermal, data reliability etc.

The `broken` setting means that a given board will likely brick if flashed.
The `untested` setting means untested.

Release status is always set with regards to the current lbmk revision, on
the theory that the current revision is being used to generate a full release.
The setting is decided on a board-by-board basis, taking its various quirks
and idiosynrasies into account.

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
