---
title: Документація
...

Завжди перевіряйте [libreboot.org](../index.uk.html) для останніх оновлень
libreboot. Новини, включаючи оголошення про випуски, може бути знайдено
в [основній секції новин](../news/).

[Відповіді на поширені запитання про libreboot](../faq.md).

Trouble?
----------

Inquiries can be made via [Libreboot IRC](../contact.md) and other channels.

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you; other
coreboot firmware is also available. For example, you can have MrChromebox UEFI
coreboot firmware installed on Chromebooks.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.

Встановлення libreboot
----------------------

-   [Як встановити libreboot](install/)

Документація, яка має відношення до операційних систем
-----------------------------------------------------

-   [Як встановити BSD на x86 хостову систему](bsd/)
-   [Керівництва Linux](linux/)

Інформація для розробників
--------------------------

-   [Як зібрати джерельний код libreboot](build/)
-   [Документація розробника системи побудови](maintain/)
-   [Корисне навантаження GRUB](grub/)
-   [Корисне навантаження U-Boot](uboot/)

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

Інша інформація
---------------

-   [Libreboot Static Site Generator](sitegen/)
-   [Різне](misc/)
-   [Список кодових назв](misc/codenames.md)



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/index.uk.html>
