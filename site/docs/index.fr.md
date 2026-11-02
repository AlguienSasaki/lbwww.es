---
title: Installer le firmware BIOS/UEFI libre et open source Libreboot
...

Consultez toujours [libreboot.org](https://libreboot.org/) pour les dernières mises à jour de
Libreboot. Les actualités, y compris les annonces de versions, se trouvent dans
la [section principale des actualités](../news/).

[Réponses aux questions fréquemment posées sur Libreboot](../faq.md).

Un problème ?
----------

Vous pouvez poser vos questions sur Libreboot via l'[IRC de Libreboot](../contact.md) et
d'autres canaux.

Si vous souhaitez une installation professionnelle, Minifree Ltd vend
[Libreboot préinstallé](https://minifree.org/) sur du matériel sélectionné, et propose aussi
un [service de préinstallation de Libreboot](https://minifree.org/product/installation-service/)
si vous voulez envoyer votre machine pour qu'on y installe Libreboot à votre place ; d'autres
firmwares coreboot sont également disponibles. Par exemple, vous pouvez faire installer le firmware
UEFI coreboot de MrChromebox sur des Chromebooks.

Leah Rowe, la fondatrice et développeuse principale de Libreboot, est aussi propriétaire
et gérante de Minifree Ltd ; les ventes financent le projet Libreboot.

Installer Libreboot
--------------------

-   [Comment installer Libreboot](install/)

Installer des systèmes d'exploitation
----------------------------

-   [Installer des systèmes d'exploitation BSD sur Libreboot](bsd/)
-   [Installer Linux sur un système Libreboot](linux/)

Informations pour les développeurs
--------------------------

-   [Comment compiler le code source de Libreboot](build/)
-   [Documentation du système de compilation pour les développeurs](maintain/)
-   [Charge utile GRUB](grub/)
-   [Charge utile U-Boot](uboot/)

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

Autres informations
-----------------

-   [Générateur de site statique de Libreboot](sitegen/)
-   [Divers](misc/)
-   [Liste des noms de code](misc/codenames.md)



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/>
