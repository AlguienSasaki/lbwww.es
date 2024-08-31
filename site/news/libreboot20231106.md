% Libreboot 20231106 released!
% Leah Rowe
% 6 November 2023

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](safety.md).**

Introduction
============

*This* new release, Libreboot 20231106, released today 6 November 2023, is
a new *testing* release of Libreboot. The previous release was
Libreboot 20231101, released on 1 November 2023. Today's release has focused
on minor bug fixes, plus tweaks to the GRUB payload.

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on [ARM-based chromebooks](../docs/install/chromebooks.html) supported
(U-Boot bootloader, instead of Google's depthcharge bootloader). On x86
machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. It provides an [automated build system](../docs/maintain/) for the
[configuration](../docs/build/) and [installation](../docs/install/) of coreboot
ROM images, making coreboot easier to use for non-technical people. You can find
the [list of supported hardware](../docs/install/) in Libreboot documentation.

Libreboot's main benefit is *higher boot speed*,
[better](../docs/linux/encryption.md) 
[security](../docs/linux/grub_hardening.md) and more
customisation options compared to most proprietary firmware. As a
[libre](policy.md) software project, the code can be audited, and coreboot does
regularly audit code. The other main benefit is [*freedom* to study, adapt and
share the code](https://writefreesoftware.org/), a freedom denied by most boot
firmware, but not Libreboot! Booting Linux/BSD is also [well](../docs/linux/) 
[supported](../docs/bsd/).

Work done since last release
============================

This is largely a bugfix release. Most notably, boot issues on GM45 thinkpads
present in the 20231101 release have been resolved.

Dell E6400 on its own tree
-----------------------------

Libreboot contains a DDR2 raminit patch for Dell Latitude E6400, that increases
reliability on coldboot, but it negatively affects other GM45 machines that use
DDR3 RAM instead.

This board is no longer provided by `coreboot/default`. Instead, it is provided
by `coreboot/dell`, and the offending patch has been moved there, along with
other required patches.

This means that the Dell Latitude E6400 still works, and quite reliably, but
the patch for it will not impact other boards. Some users reported that
Libreboot 20231101 randomly crashed or rebooted with certain memory modules,
when using on GM45 ThinkPads (ROM images for those machines were then deleted
from the 20231021 and 20231101 release archives). Today's Libreboot release
solves that problem, so these machines can be used reliably once again.

Coreboot, GRUB, U-Boot and SeaBIOS revisions
------------------------------------

Libreboot 20231106 and 20231101 are both based on these revisions:

* Coreboot (default): commit ID `d862695f5f432b5c78dada5f16c293a4c3f9fce6`, 12 October 2023
* Coreboot (dell): commit ID `d862695f5f432b5c78dada5f16c293a4c3f9fce6`, 12 October 2023
  (the `dell` tree does not exist in Libreboot 20231101, only 20231106)
* Coreboot (fam15h\_udimm): commit ID `1c13f8d85c7306213cd525308ee8973e5663a3f8`, 16 June 2021
* GRUB: commit ID `e58b870ff926415e23fc386af41ff81b2f588763`, 3 October 2023
* SeaBIOS: commit ID `1e1da7a963007d03a4e0e9a9e0ff17990bb1608d`, 24 August 2023
* U-Boot: commit ID `4459ed60cb1e0562bc5b40405e2b4b9bbf766d57`, 2 October 2023

Several other fixes and tweaks have been made, in addition to this and the
E6400 patch mentioned above.

Build system tweaks
===================

These changes were made:

* Documentation now included under `docs/`, not `src/docs/`, in releases,
  including this one.
* nvmutil: Support `make install`, so now nvmutil can be *installed* to the host.
  Patch courtesy of Riku Viitanen.
* Use the UK Mirror Service (University of Kent) mirror for GCC downloads and
  other GNU toolchain components, when building coreboot. The GNU HTTP server
  returns 302 status, redirecting to mirrors, but this 302 redirect often fails.
* Enable VBT on Dell Latitude E6430 (patch courtesy Nicholas Chin)
* Dell Latitude E6400: Set VRAM to maximum (256MB), rather than 32MB.
* GRUB: Don't print an error message about missing modules. We remove a lot of
  modules in GRUB, because we only need a few, but many GRUB configuration files
  provided by distros will just load GRUB modules frivilously, that we don't
  have. In almost all cases, the user can still boot even in such situations.
* GRUB: Don't print the missing prefix error on screen (normally seen when
  running GRUB in text mode). The prefix is actually being set, and these
  messages are benign. The message just annoys/confuses some people, so it has
  been hidden by modifying GRUB not to display it at all.
* GM45 and i945 coreboot configurations were re-made from scratch. In testing,
  certain defaults set by upstream were being overridden unnecessarily by lbmk,
  so they were re-made from scratch. When Libreboot updates the coreboot
  revision, `make oldconfig` is used on each configuration file, which is
  usually OK, but every now and then we re-do them again. (the alternative is
  to use defconfigs, not full configs, but that has other drawbacks)
* Dell Latitude E6430: Enable ME Soft Temporary Disable. This is a disable mode
  that works even if the ME region is unmodified, but we also use `me_cleaner`
  to neuter the Intel ME.
* Untitled static site generator *documentation* also included in the release.
* Untitled static site generator now included in releases, alongside the
  included Libreboot documentation (website files, and images).

FULL list of changes (git log)
------------------------------

The log is as follows, relative to Libreboot 20231101:

```
* d5a3abdb put docs under docs/ in releases (not src/docs/) 
*   139815f9 Merge pull request 'nvmutil: make install' (#149) from Riku_V/lbmk:nvmins into master 
|\  
| * 9184940f nvmutil: make install 
* | 9be589ef include untitledssg docs in releases 
* | 188b4f0d put images in the proper place, in releases 
* | 1c6add41 roll back untitled revision 
* | b295fd40 config/git: merge img files with docs 
* | 4bdaf39c use mirrorservice.org for gcc downloads 
|/  
*   5a315470 Merge pull request 'Enable VBT for E6430' (#147) from nic3-14159/lbmk:enable-e6430-vbt into master 
|\  
| * c5fd0069 Enable VBT for E6430 
* | 80b70d89 grub: avoid printing empty error messages 
* | 6e60bfe0 Recreate i945 coreboot configs from scratch 
* | 42392f6f dell/e6400: set vram to max (256mb) 
* | ada4de5f further silence grub prefix errors 
* | dd03a87b grub: don't print messages if a module isn't found 
* | ab57e7a4 grub: don't print prefix errors on the screen 
* | 72e7d090 coreboot: re-configure gm45 thinkpads from scratch 
* | 742c0033 coreboot/dell: move e6400 to new tree, dell 
* | f870a2fe Dell E6430: use ME Soft Temporary Disable 
|/  
* dd1e15fd merge untitled with docs, in releases 
* 922bccf9 include untitled ssg in releases 
```

Yet another extremely conservative changelog. Libreboot's ten-year anniversary
is on 12 December 2023, which is not long away. Regular testing of Libreboot
is in progress, and rapid development towards a new stable release; it is very
much planned that 12 December 2023 will have a new stable release of Libreboot.

Hardware supported in this release
==================================

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](../docs/install/kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](../docs/install/kgpe-d16.md)

Desktops (AMD, Intel, x86)
-----------------------

-   [Gigabyte GA-G41M-ES2L motherboard](../docs/install/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/install/acer_g43t-am3.md)
-   Intel D510MO and D410PT motherboards
-   Apple iMac 5,2
-   [ASUS KCMA-D8 motherboard](../docs/install/kcma-d8.md)
-   [Dell Precision T1650](../docs/install/t1650.md) (**easy to flash without disassembly**)
-   [HP Elite 8200 SFF/MT](../docs/install/hp8200sff.md) (HP 6200 Pro Business probably works too)
-   [HP Elite 8300 USDT](../docs/install/hp8300usdt.md)
-   [Intel D945GCLF](../docs/install/d945gclf.md)

### Laptops (Intel, x86)

-   **[Dell Latitude E6400](../docs/install/latitude.md) (easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   [Dell Latitude E6430 (Intel GPU](../docs/install/latitude.md) **(easy to flash, no disassembly)**
-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/install/x200.md)
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/install/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/install/t400.md)
-   [Lenovo ThinkPad T500](../docs/install/t500.md)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad W500](../docs/install/t500.md)
-   Lenovo ThinkPad R500
-   [Apple MacBook1,1 and MacBook2,1](../docs/install/macbook21.md)
-   [Lenovo ThinkPad T440p](../docs/install/t440p_external.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   [Lenovo ThinkPad W541](../docs/install/ivy_has_common.md)
-   [HP EliteBook 2170p](../docs/install/hp2170p.md) (**socketed flash IC**)
-   [HP EliteBook 2560p](../docs/install/hp2560p.md)
-   [HP EliteBook 2570p](../docs/install/hp2570p.md)
-   [HP EliteBook 8470p](../docs/install/hp8470p.md)
-   [HP EliteBook Folio 9470m](../docs/install/hp9470m.md)

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Downloads
=========

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.

Post-release errata
===================

S3 on GM45/i945
---------------

GM45 ThinkPads still have buggy S3 behaviour, ditto i945. x4x and Pineview
untested, other Intel platforms work; AMD untested but should work.

If you're using a GM45 ThinkPad (e.g. X200, T400), don't try putting your
laptop to sleep. S3 suspend/resume is broken. The intention is that this will
be fixed before the next stable release.

E6430 VBT config
----------------

See: <https://browse.libreboot.org/lbmk.git/commit/?id=a02c7e316370409e985fa634fec5a59326e3b4fd>

This patch, linked, enables the VBT setup on Dell Latitude E6430, but this
patch was not merged before the Libreboot 20231106 release. VBT wasn't actually
enabled, on the release, so images for E6430 have been removed from the release
archives; E6430 ROM images will instead be provided in the next release. For
now, you can build Libreboot from `lbmk.git`. See:
[Building Libreboot from source](../docs/build/)

Errata
======

Many modules were removed from GRUB, to save space in flash, but the ones that
are actually needed were carefully retained.

However, FAT/NTFS support in GRUB was removed in recent Libreboot releases,
including Libreboot 20231106.

Apparently, some distros use FAT-based file systems for `/boot`, and this meant
that a user with such a distro could not boot their kernel on the latest release.

This has been fixed, in the following patch:
<https://browse.libreboot.org/lbmk.git/commit/?id=39aad57873b9e6265d089ef7dbb4dc5add5c659f>

This was merged *after* the Libreboot 20231106 release, and so it will be
present in the next Libreboot release after 20231106.
