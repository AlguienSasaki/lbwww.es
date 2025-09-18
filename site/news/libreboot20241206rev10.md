% Libreboot 20241206, 10th revision released! GRUB security fixes, better LVM scanning, non-root USB2 hub support
% Leah Rowe
% 18 February 2025

A corresponding [Canoeboot 20250107rev1](https://canoeboot.org/news/canoeboot20250107rev1.html)
release is also available.

Today's Libreboot 20241206 revision is the 10th revision in the Libreboot
20241206 stable release series. The changelog on this page is written, relative
to Libreboot 20241206 revision 9 which was released on 12 February 2025.
The *original* Libreboot 20241206 release came out on 6 December 2024. You
can find the full list of revisions [here](libreboot20241206.Revisions.md)
and the original release [here](libreboot20241206.md).

Open source BIOS/UEFI firmware
------------------------------

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

Libreboot is a free/open source BIOS/UEFI replacement on x86 and ARM, providing
boot firmware that initialises the hardware in your computer, to then load an
operating system (e.g. Linux/BSD). It is specifically
a *[coreboot distribution](../docs/maintain/)*,
in the same way that Debian is a Linux distribution. It provides an automated
build system to produce coreboot ROM images with a variety of payloads such as
GRUB or SeaBIOS, with regular well-tested releases to make coreboot as easy
to use as possible for non-technical users. From a project management perspective,
this works in *exactly* the same way as a Linux distro, providing a source-based
package manager (called lbmk) which patches sources and compiles coreboot images.
It makes use of [coreboot](https://www.coreboot.org/) for hardware initialisation,
and then a payload such as [SeaBIOS](https://www.seabios.org/SeaBIOS)
or GRUB to boot your operating
system; on ARM(chromebooks), we provide *U-Boot* (as a coreboot payload).

We also provide an experimental U-Boot setup on x86, as a coreboot payload for
providing a minimal UEFI implementation.

### Regarding revision 10

Normally, revisions would only be documented on
the [Libreboot 20241206 revisions page](libreboot20241206.Revisions.md), but
this revision contains *critical security fixes*, so it was decided that there
should be a full announcement, to ensure that more people see it.

Summarised list of changes
------------------------

### Revision updates

GRUB released *73 patches* to its main branch, fixing a large number of
security issues. You can read about them here:

https://lists.gnu.org/archive/html/grub-devel/2025-02/msg00024.html

This updates GRUB to revision `4dc6166571645780c459dde2cdc1b001a5ec844c`
from 18 February 2025. Several OOB heap writes, buffer overflows, use after
frees and so on, are now prevented with this update.

### Feature changes

In addition to the security fixes, several out-of-tree fixes from Libreboot's
main branch have been merged for GRUB, fixing bugs in the xHCI driver, and
adding support for non-root USB2 hubs on platforms that use the `xhci` GRUB
tree.

### Configuration changes

Changes to the GRUB configuration have been made, to make scanning of LVM
volume/group names more reliable, including on full-disk-encryption setups.
More such changes are planned for the next major release; the current changes
are very minor.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
