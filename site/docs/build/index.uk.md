---
title: Побудова з джерельного коду
x-toc-enable: true
...

WARNING: Flash from bin/, NOT elf/
==================================

TODO: translate this section into ukrainian language

**WARNING: When you build a ROM image from the Libreboot build system, please
ensure that you flash the appropriate ROM image from `bin/`, NOT `elf/`.
The `elf/` coreboot ROMs do not contain payloads. Libreboot's build system
builds no-payload ROMs under `elf/`, and payloads separately under `elf/`. Then
it copies from `elf/` and inserts payloads from `elf/`, and puts the final ROM
images (containing payloads) in `bin/`. This design is more efficient, and
permits many configurations without needless duplication of work. More info
is available in the [lbmk maintenance manual](../maintain/)**

Also, this page currently only refers to the build system as it exists
in `lbmk.git`. A massive re-design of lbmk has been in progress, since the
Libreboot 20230625 release. When the next version after 20230625 comes out,
this page will once again match the current tarball release.

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

Побудова з джерельного коду
============================

Фактична розробка/тестування завжди виконується безпосередньо за допомогою `lbmk`, і це також
стосується збирання з джерельного коду. Ось кілька інструкцій, щоб
почати:

libreboot включає сценарій, який автоматично встановлює apt-get залежності
в Ubuntu 20.04:

	sudo ./build dependencies ubuntu2004

Окремі сценарії також існують:

	sudo ./build dependencies debian

	sudo ./build dependencies arch

	sudo ./build dependencies void

Check: `config/dependencies/` for list of supported distros.

Технічно, будь-який дистрибутив Linux може бути використано для побудови libreboot.
Однак, вам потрібно буде написано свій власний сценарій для встановлення залежностей
побудови. 

libreboot Make (lbmk) автоматично виконує всі необхідні команди; наприклад,
`./build grub payload` автоматично виконає `./build grub utils`,
якщо затребувані утиліти для GRUB не збудовано, для виготовлення корисних навантажень.

В якості результату, ви тепер можете (після встановлення правильних залежностей побудови) виконати
лише одну команду, з свіжого Git clone, для побудови образів ROM:

	./build fw coreboot all

або навіть побудувати конкретні образи ROM, такі як:

	./build fw coreboot x60

or get a list of supported build targets:

	./build fw coreboot list

Якщо ви бажаєте побудувати корисні навантаження, можете зробити це. Наприклад:

	./build fw grub

	./update project trees -b seabios

	./update project trees -b u-boot

Попередні кроки буде виконано автоматично. Однак, ви можете *досі* виконати
окремі частини системи побудови власноруч, якщо виберете. Це може бути
вигідно, коли ви робите зміни, та бажаєте протестувати конкретну частину
lbmk.

20230625 build error (release archive)
======================================

When building ROM images from the release archives, the following error
is observed in some cases, depending on distro:

```
In file included from src/lib/version.c:4:
build/build.h:10:32: error: 'libreboot' undeclared here (not in a function)
   10 | #define COREBOOT_MAJOR_VERSION libreboot-20230625
      |                                ^~~~~~~~~
src/lib/version.c:35:46: note: in expansion of macro 'COREBOOT_MAJOR_VERSION'
   35 | const unsigned int coreboot_major_revision = COREBOOT_MAJOR_VERSION;
      |                                              ^~~~~~~~~~~~~~~~~~~~~~
```

This happened when a user tried to build for ThinkPad W541 on an Arch Linux
system. The fix is available here:

<https://browse.libreboot.org/lbmk.git/patch/?id=f34e07ae27e3e6e8508cdebcbd09fdf73fca302d>

Apply this patch to your local release archive, and it should fix the issue.
