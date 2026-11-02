---
title: Установка Libreboot — свободная/открытая прошивка BIOS/UEFI  
...

Всегда проверяйте [libreboot.org](https://libreboot.org/) на наличие последних обновлений Libreboot. Новости, включая анонсы релизов, можно найти в [основном разделе новостей](../news/).

[Ответы на часто задаваемые вопросы о Libreboot](../faq.md).

Нужна помощь?
-------------

Помощь доступна в [IRC Libreboot](../contact.md) и других каналах.

Если вы хотите профессиональную установку, компания Minifree Ltd продаёт [Libreboot с предустановкой](https://minifree.org/) на определённом оборудовании, а также предоставляет [услугу предварительной установки Libreboot](https://minifree.org/product/installation-service/), если вы хотите отправить свой компьютер для установки Libreboot.

Лия Роу, основатель и ведущий разработчик Libreboot, также владеет и управляет Minifree Ltd; продажи поддерживают финансирование проекта Libreboot.

Установка Libreboot
--------------------

-   [Как установить Libreboot](install/)

Установка операционных систем
-----------------------------

-   [Установка BSD-систем на Libreboot](bsd/)
-   [Установка Linux на систему с Libreboot](linux/)

Информация для разработчиков
-----------------------------

-   [Как скомпилировать исходный код Libreboot](build/)
-   [Документация по сборочной системе для разработчиков](maintain/)
-   [Загрузчик GRUB](grub/)
-   [Загрузчик U-Boot](uboot/)

Windows OS
--------

Windows 10 and 11 were tested on recent releases, using the available
EDK2 UEFI payload (available on most boards) and it works perfectly.
Note that to use Windows 11, you might need to bypass SecureBoot and
TPM requirement on certain boards - ditto CPU requirement, as Windows 11
enforces minimum system requirements. You can bypass Windows 11 requirements
to use it on older machines, if installing it with Rufus. We do not recommend
using Windows at all, as it is proprietary software, but this minimal note
is included here for completeness. This notice is the maximum extent to which
this project is willing to, in any way, support such a non-free operating
system as Windows. More completeness: EDK2 UEFI support means you could
probably also set up a Hackintosh in Libreboot.

Yes, that's all you get. We recommend using a *free* operating system e.g.
Linux distro or a BSD. Please note that this notice applies only to 64-bit
Intel/AMD hardware with EDK2 UEFI payload; older Windows versions are not
compatible; versions that are compatible must be booted in UEFI mode, not
legacy BIOS mode. So for example, Windows XP/Vista/7 won't work at all.

Другая информация
-----------------

-   [Генератор статических сайтов Libreboot](sitegen/)
-   [Разное](misc/)
-   [Список кодовых имён](misc/codenames.md)

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"

