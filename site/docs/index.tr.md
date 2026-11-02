---
title: Libreboot Özgür/Açık Kaynak BIOS/UEFI Yazılımını Yükleme
...

Libreboot ile ilgili en son güncellemeler için her zaman [libreboot.org](https://libreboot.org/) adresini kontrol edin. Haberler ve sürüm duyuruları [ana haberler bölümünde](../news/) bulunabilir.

[Libreboot hakkında Sıkça Sorulan Sorular](../faq.md).

Yardıma mı ihtiyacınız var?
--------------------------

[Libreboot IRC](../contact.md) ve diğer kanallarda yardım alabilirsiniz.

Profesyonel kurulum istiyorsanız, Minifree Ltd seçili donanımlarda [Libreboot ön yüklü](https://minifree.org/) sistemler satmaktadır ve ayrıca cihazınızı göndererek Libreboot kurulumu yaptırabileceğiniz bir [Libreboot ön yükleme hizmeti](https://minifree.org/product/installation-service/) sunmaktadır.

Libreboot'un kurucusu ve baş geliştiricisi Leah Rowe aynı zamanda Minifree Ltd'nin sahibi ve işletmecisidir; satışlar Libreboot projesi için finansman sağlamaktadır.

Libreboot'u yükleme
------------------

-   [Libreboot nasıl yüklenir](install/)

İşletim sistemlerini yükleme
---------------------------

-   [Libreboot üzerine BSD işletim sistemleri kurulumu](bsd/)
-   [Libreboot sistemine Linux kurulumu](linux/)

Geliştiriciler için bilgiler
---------------------------

-   [Libreboot kaynak kodunu nasıl derleyebilirsiniz](build/)
-   [Yapı sistemi geliştirici dokümantasyonu](maintain/)
-   [GRUB yükleyici](grub/)
-   [U-Boot yükleyici](uboot/)

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

Diğer bilgiler
-------------

-   [Libreboot Statik Site Oluşturucu](sitegen/)
-   [Çeşitli](misc/)
-   [Kod adları listesi](misc/codenames.md) 

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
