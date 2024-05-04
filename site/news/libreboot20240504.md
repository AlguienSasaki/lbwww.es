% Libreboot 20240504 released!
% Leah Rowe
% 4 May 2024

Introduction
============

Libreboot is a free/open source BIOS/UEFI replacement on x86 and ARM, providing
boot firmware that initialises the hardware in your computer, to then load an
operating system (e.g. Linux/BSD). It is specifically a *coreboot distribution*,
in the same way that Debian is a Linux distribution. It provides an automated
build system to produce coreboot ROM images with a variety of payloads such as
GNU GRUB or SeaBIOS, with regular well-tested releases to make coreboot as easy
to use as possible for non-technical users. From a project management perspective,
this works in *exactly* the same way as a Linux distro, providing the same type
of infrastructure, but for your boot firmware instead of your operating system.
It makes use of [coreboot](https://www.coreboot.org/) for hardware initialisation,
and then a payload such as [SeaBIOS](https://www.seabios.org/SeaBIOS)
or [GNU GRUB](https://www.gnu.org/software/grub/) to boot your operating
system; on ARM(chromebooks), we provide *U-Boot* (as a coreboot payload).

Libreboot provides many additional benefits such as fast boot speeds, greater
security and greater customisation, but the *primary* benefit
is [software freedom](https://writefreesoftware.org/learn). With use of GRUB
in the flash, you can make use of many advanced features such as the ability
to [boot from an encrypted /boot partition](../docs/linux/)
and [verify kernel GPG signature at boot time](../docs/linux/grub_hardening.md).
Libreboot's GRUB payload is *heavily* patched; for example, today's release
uses GRUB based on version 2.12, but Libreboot adds argon2 KDF support (for
LUKS2) and xHCI support - you can use USB 3.0 devices natively, in GRUB,
including distro install media via USB3. This means that with the right
coreboot config, you could use Libreboot's GRUB even on modern machines that lack
PS/2 keyboard controllers or EHCI support; many modern machines only have USB3.

Another example of the type of benefit you could get from Libreboot: you can
boot from NVMe SSDs in the SeaBIOS payload, if your board can take them (e.g.
desktop board with an NVMe adapter in the PCI-E slot). If your vendor's BIOS/UEFI
firmware only supports SATA, then this is a nice bonus for you. With Libreboot,
you get continued firmware updates over time, adding new features on both older
and newer hardware. Libreboot still provides updates for machines that are
nearly 20 years old, while also supporting newer machines. More hardware support
is being added all the time!

These and other examples are just the start. Libreboot provides a *superior* boot
experience compared to proprietary BIOS/UEFI, giving you the same power and level of
control that running a Linux or BSD operating system does. It's *your* computer
to boot however you wish. Libreboot lets you get more out of the hardware. All
your favourite Linux distros and BSDs are compatible, even Qubes(on most machines).

If you're fed up of the control that proprietary UEFI vendors have over you,
then Libreboot is *for you*. Although many would agree that it is a major step
forward for most users, it's actually a conservative idea socially. It used to
be that computers were much more open for learning, and tinkering. Libreboot
implements this old idea in spirit and in practise, helping you wrest back control.
Unlike the hardware vendors, Libreboot does not see *you* as a *security threat*;
we regard the ability to use, study, modify and redistribute software freely to
be a human right that everyone *must* have, and the same is true of hardware.
Your computer is *your property* to use as you wish. Free Software protects you,
by ensuring that you always have control of the machine.

*This* new release, Libreboot 20240504, released today 4 May 2024, is
a new *stable* release of Libreboot. The previous stable release was
Libreboot 20230625 released on 25 June 2023, and the previous *testing* release
was Libreboot 20240225 released on 25 February 2024. Extreme care has been
taken with this release, but it adds a host of new features such as USB3
support in the GRUB payload, and a slew of mainboard fixes. *Read on* to learn
more.

The main purpose of this release has been to fix bugs. A lot more work will now
go into Libreboot for a planned *testing* release in the summer of 2024.

Hardware supported in this release
==================================

This release supports the following hardware:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](../docs/hardware/kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](../docs/hardware/kgpe-d16.md)

### Desktops (AMD, Intel, x86)

-   **[Dell OptiPlex 7020/9020 MT and SFF](../docs/hardware/dell9020.md) - Also [available to buy
    with Libreboot preinstalled](https://minifree.org/product/libreboot-9020/)** - Dell OptiPlex XE2 MT/SFF also known to work
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)
-   [ASUS KCMA-D8 motherboard](../docs/hardware/kcma-d8.md)
-   Dell OptiPlex 7010 **MT** (known to work, using the T1650 ROM, but more
    research is needed)
-   [Dell Precision T1650](../docs/hardware/t1650.md)
-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [HP Elite 8200 SFF/MT](../docs/hardware/hp8200sff.md) (HP 6200 Pro Business probably works too)
-   [HP Elite 8300 USDT](../docs/hardware/hp8300usdt.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Intel D945GCLF](../docs/hardware/d945gclf.md)

### Laptops (Intel, x86)

-   **[Lenovo ThinkPad T440p](../docs/install/t440p_external.md) - Also [available
    to buy with Libreboot preinstalled](https://minifree.org/product/libreboot-t440p/)**
-   **[Lenovo ThinkPad W541](../docs/install/ivy_has_common.md) - Also [available to
    buy with Libreboot preinstalled](https://minifree.org/product/libreboot-w541/)**
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)
-   [Dell Latitude E6400, E6400 XFR and E6400 ATG, all with Nvidia or Intel
    GPU](../docs/hardware/e6400.md)
-   [Dell Latitude E6420 (Intel GPU](../docs/hardware/e6420.md)
-   [Dell Latitude E6430 (Intel GPU](../docs/hardware/e6430.md)
-   [Dell Latitude E5520 (Intel GPU](../docs/hardware/e5520.md)
-   [Dell Latitude E5530 (Intel GPU](../docs/hardware/e5530.md)
-   [Dell Latitude E6520 (Intel GPU](../docs/hardware/e6520.md)
-   [Dell Latitude E6530 (Intel GPU](../docs/hardware/e6530.md)
-   Dell Latitude E5420
-   [HP EliteBook 2170p](../docs/hardware/hp2170p.md)
-   [HP EliteBook 2560p](../docs/hardware/hp2560p.md)
-   [HP EliteBook 2570p](../docs/hardware/hp2570p.md)
-   [HP EliteBook 820 G2](../docs/hardware/hp820g2.md)
-   [HP EliteBook 8460p](../docs/hardware/hp8460p.md)
-   [HP EliteBook 8470p](../docs/hardware/hp8470p.md)
-   [HP EliteBook 8560w](../docs/hardware/hp8560w.md)
-   [HP EliteBook Folio 9470m](../docs/hardware/hp9470m.md)
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad T520 / W520](../docs/install/ivy_has_common.md) (no install guide yet)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md) (no install
-   Lenovo ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   Lenovo ThinkPad X230
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   Lenovo ThinkPad X301
-   Lenovo ThinkPad X60 / X60S / X60 Tablet

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Canoeboot 20240504 also released!
=========================

I've done simultaneous Canoeboot and Libreboot releases, today. I strongly
recommend that everyone uses Libreboot, which adheres to Libreboot's
own [Binary Blob Reduction Policy](policy.md) and therefore supports more
boards. Canoeboot is a parallel effort, that I also maintain, but Canoeboot
adheres to GNU FSDG as policy and therefore supports far fewer mainboards. See:

[Canoeboot 20240504 release](https://canoeboot.org/news/canoeboot20240504.html)

New mainboard added
====================

This release adds support for the following mainboard:

* Dell Latitude E5420, courtesy of Nicholas Chin

Dell Latitude laptops: S3 resume fixed
================================

Nicholas Chin sent in a patch just before the release, fixing suspend/resume
on sandy bridge and ivy bridge Dell laptops. According to him, resume on open
is still broken and therefore disabled, but pressing the power button works.

Work done since Libreboot 20230625
============================

Today's release, Libreboot 20240504, is a stable release. The *previous* stable release
was Libreboot 20230625, but there have been a number of *testing* releases
between that and today's release.

To know the full set of differences between Libreboot 20230625
and Libreboot 20240405, first you must read the changelogs of those interim
testing releases. They are, in order: Libreboot [20231021](libreboot20231021.md),
[20231101](libreboot20231101.md), [20231106](libreboot20231106.md),
[20240126](libreboot20240126.md) and [20240225](libreboot20240225.md).

The following log will now acount for changes since Libreboot 20240225, from
most recent descending to very earliest commits. The most interesting changes
are highlighted in bold:

* **Fix S3 suspend/resume on Ivybridge/Sandybridge-era Dell Latitude laptops.**
  Patch courtesy of Nicholas Chin.
* **Fixed WiFi on HP EliteBook 8560w via GPIO config**. Patch by Leah Rowe,
  but using advice from Angels Pons; thank you, Angel! There is a GPIO
  that sets `WLAN_TRN_OFF#` low or high; coreboot was setting it low, but it
  needs to be set high otherwise hardware rfkill would be set. WiFi now
  works perfectly, but NOTE: the WiFi enable/disable button doesn't currently
  do anything; it sends a scancode which is picked up in dmesg due to being
  non-standard. You can still enable or disable WiFi from your OS.
* `sript/update/release`: Report on the terminal when a tarball is being
  created, to avoid giving the impression that the script has crashed when
  making very large tar archives.
* dell-flash-unlock: Use a portable Makefile (GNU Make no longer required).
  Patch courtesy of Nicholas Chin.
* dell-flash-unlock README updated for the BSDs (patch courtesy Nicholas Chin)
* **`dell_flash_unlock`: Add support for FreeBSD** (patch courtesy Nicholas Chin)
* `dell_flash_unlock`: Set iopl level back to 0 when done (patch by Nicholas Chin)
* `dell_flash_unlock`: Fix ec_set_fdo() signature (patch courtesy Nicholas Chin)
* QEMU: Corrected SMBIOS information in the coreboot config. Patch courtesy
  of *livio*. (libreboot provides coreboot images to run on QEMU)
* GRUB ATA boot: Fixed it so that it actually scans `ata` devices in GRUB,
  rather than `ahci`. Patch courtesy of *livio*.
* GRUB hotkeys added: at boot, hold *shift* to disable gfxterm. Hold *CTRL* to
  enable serial output. Hold *ALT* to enable spkmodem. Patch courtesy of *livio*.
* General code cleanup / simplification in lbmk.
* **Support SeaGRUB with SeaBIOS menu enabled**. This is when GRUB is the first
  thing that SeaBIOS starts (GRUB from flash). We already supported it, but
  we disabled the SeaBIOS menu when doing this. Now we provide options with
  the menu retained. This is useful on desktops where you use a graphics card,
  but you still mainly want to use the GRUB payload, because we don't initialise
  VGA from coreboot, only from SeaBIOS (we provide a framebuffer from coreboot
  for Intel graphics, but graphics cards are handled by SeaBIOS).
* `update/trees`: Simplified handling of defconfig files. The new code is
  more reliable, and will not have to be modified as much when adding
  new options for changing configs.
* Don't use `nproc` to decide build threads; set it to 1 instead. The user
  can manually specify build threads when running lbmk. This is because nproc
  is not available on all systems.
* eDP-based X230/X220 configs have been removed. Reasoning is provided at
  the end of this article (please scroll down).
* IASL/acpica: Libreboot now hosts these releases on the mirrors, and uses
  them in lbmk. This is because Intel's own links often expire, or have
  unstable hashes. Coreboot's build system is patched to use Libreboot's
  mirror, when downloading these tarballs.
* Allow disabling status checks during build. Do: `export LBMK_STATUS=n`
* `./build roms list`: Allow filtering based on status. E.g.
  command: `./build roms list stable`
* Allow setting *status* on coreboot targets, and warn about it during builds.
  Set it in target.cfg; e.g. `status="stable"` or `status="untested"`. If it's
  not marked stable, a given board will provide a y/n confirmation screen on
  build, asking if you want to skip the build (this dialog is disabled
  in release builds) - there is another: `release` variable in target.cfg
  can be set to n, to always skip building that target, but only skip on
  release builds. This is better than documenting board status on the website,
  because it's automated in lbmk. A `warn.txt` file can be provided in
  the board directory inside lbmk, with a message that will be printed when
  building; you can use this to warn the user about specific issues.
* i945 targets (ThinkPad X60/T60, Macbook2,1): switch back to the February 2023
  coreboot revision used in Libreboot 20230625 for now, to work around an issue
  that was reported by some users; it will be updated again in a future release.
* Export environmental variables from `err.sh`, to ensure that they are always
  exported and therefore universally consistent in all parts of lbmk, once
  set (unless overridden by a given script).
* **GRUB:** Update to newer revision just after version 2.12. The new revision
  has a fix bug fixes for file systems, most notably XFS.
* Dell OptiPlex 9020 SFF/MT: Add TPM support and enable the TPM by default.
* lbmk: Better handling of `/tmp` files. Fewer/no files are left behind now,
  after a build has completed.
* HP EliteBook 820 G2: Retain the target, allow it to be built from source, but
  do not include ROM images in releases. This is because the *inject* script
  cannot yet reliably and reproducibly insert the *refcode* file without
  the hash changing, due to the native of *xz* (compression utility).
* **Haswell targets: MRC configs disabled. Only NRI ROMs provided now.** The
  libre raminit (NRI) is now stable enough in testing, that it's the default,
  and the only one provided in releases. This affects: ThinkPad W541/T440p
  and Dell OptiPlex 9020 SFF/MT. This is done, in application of Libreboot's
  Binary Blob Reduction Policy which states: if a blob can be avoided, it
  should be avoided.
* **Dell OptiPlex 9020 SFF/MT: Added configs using NRI (native RAM
  initialisation / libre raminit).** Angel Pons fixed S3 suspend/resume also,
  which works perfectly now, on NRI.
* **Dell OptiPlex 9020 SFF/MT: Fan control now works perfectly.** Before, the
  fans would only run at a very low speed even in stress conditions, leading
  to higher temperatures. The result is you can now use faster, hotter CPUs
  and the fans will spin up just right. Patch courtesy of Mate Kukri.
* **ThinkPad W541/T440p NRI:** GRUB payload has been enabled on setups that use
  NRI (native RAM initialisation / libre raminit). It was previously only
  enabled on the MRC-based setup.
* ThinkPad T440p/W541: Added targets that use *Broadwell* MRC code (same as
  below regarding Dell OptiPlex 9020 SFF/MT). Again: MRC targets disabled in
  release. NRI-based images are provided exclusively now (libre raminit).
* Dell OptiPlex 9020 SFF/MT: Added targets that use the *Broadwell* MRC code,
  for memory controller initialisation. Use of it works around a lot of issues
  in the Haswell one. NOTE: Libreboot no longer provides MRC-based images, so
  you have to build this from source in lbmk. See notes above, about S3 fix
  on NRI (native RAM initialisation / libre raminit).
* **GRUB: Added xHCI (USB 3.0) native driver.** Patches courtesy Patrick Rudolph,
  rebased by Leah Rowe for GRUB 2.12 series. GRUB keyboard input can now work,
  on boards that only have xHCI (some newer boards lack EHCI and/or PS/2)
* **Fixed 3rd SATA slot on Dell OptiPlex 9020 SFF**, and 3rd and 4th slots
  on 9020 MT; they previously did not work. Patch courtesy of Mate Kukri.
* Allow specifying the number of threads to build on, via environmental
  variable `LBMK_THREADS`. This is useful if you're on a host with limited RAM.
* Simpler, safer error handling in the build system
* **util/autoport:** New utility, imported from coreboot's version but with extra
  patches merged for Haswell platforms. This can be used in future, tied heavily
  to Libreboot's own coreboot trees, to more easily add new mainboards.
* **util/dell-flash-unlock: NetBSD support added**, courtesy of the developer
  who goes by the name *Linear Cannon*. Here is that person's website as of
  today: <https://linear.network/>
* NEW MAINBOARD: Dell Latitude E5420, courtesy of Nicholas Chin
* **OptiPlex 9020 SFF/MT: Graphics card now work perfectly.** Disable IOMMU by
  default, to work around an issue so that graphics cards can be used. It is a
  toggle option that you can change; IOMMU recommended if using Intel graphics,
  otherwise leave it turned off.
* coreboot/haswell: Make IOMMU a runtime option (on/off toggle)
* Enable the serial console by default, on AMD boards (kgpe-d16, kcma-d8)

Exact git log (versus Libreboot 20240225)
======================================

The following is an exact log of commits in the Git repository, on the master
branch, relative to the previous January 2024 release. There are 99 changes:

```
* ae9e7389 Libreboot 20240504 release 
* d3aeb2c7 config/git: importer newer documentation 
* 5bf25eac coreboot: update latitude release status 
* 7a955a4c d510mo and d945gclf: disable for release 
* 7e799e1f nb/haswell: lock policy regs when disabling IOMMU 
* d9c0346a build/roms: more useful status warnings 
* 98587029 deprecate MRC 9020MT/SFF (NRI 9020 is default now) 
* d839bfa1 mark 9020 sff/mt stable for release 
* a9bc6b25 mark lenovo x301 as stable for release 
*   6e61052a Merge pull request 'coreboot/default: Add patches to fix S3 on SNB/IVB Latitudes' (#208) from nic3-14159/lbmk:latitude-fix-s3 into master 
|\  
| * 67ddd3f2 coreboot/default: Add patches to fix S3 on SNB/IVB Latitudes 
|/  
* 780e03fe remove x220edp/x230edp (keep regular x220/x230) 
* b379186a update hp machines to status=stable for release 
* 6e7b5c0b Enable WiFi on HP EliteBook 8560w (GPIO config) 
*   99617796 Merge pull request 'Implemented failsafe options at boot and inside menus for enabling/disabling serial, spkmodem and gfxterm' (#203) from livio/lbmk:failsafe into master 
|\  
| * 3e86b3ab Implemented failsafe options at boot and inside menus for enabling/disabling serial, spkmodem and gfxterm 
* | 2d207c54 coreboot/x301: set release=n (will re-test) 
* | 64ae2ddd update/release: purge test/lib/strlcat.c in u-boot 
* | 748b2072 mark x4x boards ready for release 
* | 9caff263 err.sh: update copyright info 
* | 7db2ae0b update/release: say when an archive is being made 
* |   cd9685d1 Merge pull request 'dell-flash-unlock: Remove dependency on GNU Make' (#207) from nic3-14159/lbmk:dell-flash-unlock-updates into master 
|\ \  
| * | a5cb6376 dell-flash-unlock: Remove dependency on GNU Make 
|/ /  
* |   4bf3da31 Merge pull request 'Fixed QEMU x86 target's SMBIOS informations' (#205) from livio/lbmk:qemux86_fix into master 
|\ \  
| * | 707d7ce7 Fixed QEMU x86 target's SMBIOS informations 
| * | d654a3e5 Fixed QEMU x86 target's SMBIOS informations 
| |/  
* |   a18cd7f1 Merge pull request 'Fixed boot selection menu' (#204) from livio/lbmk:livio_290424 into master 
|\ \  
| * | b4d27d0c Fixed boot selection menu 
| |/  
* |   05c3f493 Merge pull request 'dell-flash-unlock-updates' (#206) from nic3-14159/lbmk:dell-flash-unlock-updates into master 
|\ \  
| * | 61f66a46 dell-flash-unlock: Update README for BSD 
| * | 5e2e7611 dell_flash_unlock: Add support for FreeBSD 
| * | 61dbaf94 dell_flash_unlock: Set iopl level back to 0 when done 
| * | 355dffb7 dell_flash_unlock: Fix ec_set_fdo() signature 
| * | 6fe2482f dell-flash-unlock: Remove unnecessary includes for NetBSD 
| * | b737a24c dell-flash-unlock: Remove memory clobber from inline assembly 
* | | 5c3d81ff correct dell latitude status for release 
* | | 6dfd8c70 update release status for HP machines 
* | | 50f6943c set gru bob/kevin stable for release 
* | | df5e3216 set dell latitudes stable for release 
* | | 7e7c3c23 mark i945 machines as stable for release 
* | | 310378c9 build/roms: simplified list handling 
* | | 5003e02b build/roms: if release, allow all non-broken roms 
* | | dbe259ef build/roms: always display warnings 
* | | 0e2c56be build/roms: reduce indentation in skip_board() 
* | | 91927760 build/roms: simplified status handling 
* | | 230f68fd build/roms: simplified seagrub handling 
|/ /  
* | 515185a7 build/roms: support SeaGRUB *with menu enabled* 
* | a88a8281 update/trees: simplified defconfig copying 
* | 55204dc4 option.sh: don't use nproc (not portable) 
* | 71f8e653 eDP configs (x230/x220): don't release 
* | a5c7cc1a fix target.cfg files on dell latitudes 
* | d923d314 use mirrorservice.org for iasl downloads 
* | 714d4b3e update/release: disable status checking 
* | e614f906 build/roms: tell the user how to ignore status 
* | f22305fb update macbook21/x60/t60 status 
* | 6c4f07b3 allow disabling status checks during builds 
* | ad7e3966 update 9020 sff/mt release status 
* | 3ace925e update more board statuses before release 
* | e7619225 Set status=unstable on dell latitudes 
* | 1fd9ba9a declare ivy/sandy thinkpads stable for release 
* | 5218bfb0 declare gm45 thinkpads stable for release 
* | b99ebe05 kcma-d8/kgpe-d16: mark as tested(unstable) 
* |   e5cc3e55 Merge pull request 'dell-flash-unlock: add NetBSD support' (#194) from linear/lbmk:master into master 
|\ \  
| * | e119ffa5 dell-flash-unlock: add NetBSD support 
* | | c0b4ba2e build/roms: update help, pertaining to status 
* | | d88783b7 build/roms: let "list" specify status types 
* | | b6014a65 erroneous return 
* | | ce7fd754 build/roms: report status when building images 
* | | a2f42353 i945: switch boards to 20230625 coreboot revision 
* | | 64177dbb exports variables from err.sh, not build 
* | | a5082de4 GRUB: bump to today's latest revision 
* | | ddfe71a3 9020 sff/mt: actually enable the TPM (by default) 
* | | 2d7debd3 9020 sff/mt: add tpm enable patch from mate kukri 
* | | 08859bb4 lbmk: export TMPDIR from err.sh, not build 
* | | f5f2c58a build/roms: add missing deletion of tmp file 
* | | 02e4c0b2 hp820g2: allow building, but don't do release ROMs 
* | | ed0678ae haswell: only provide NRI-based ROMs in releases 
* | | f5035e32 9020 sff/mt: fix bad gpio read on hwm patch 
* | | 523f1df9 w541 libremrc: disable tseg stage cache 
* | | c557e9e0 haswell nri: set 8MB CBFS on thinkpads (fix S3) 
* | | ac7ce930 add 9020sff/mt configs using haswell NRI 
* | | 9e3b217c update coreboot/haswell (NRI) 
* | | 6da91df6 add mate's patch for 9020 sff/mt fan controls 
* | | 83195489 enable grub payload on libremrc w541/t440p 
* | | e9c591a5 add t440p/w541 configs using broadwell mrc 
* | | 4134a883 add 9020 sff/mt targets that use broadwell mrc 
* | | f7283fa1 grub xhci support 
* | | 5cb17795 fix sata slots on dell 9020 sff and mt 
* | | 33277897 allow users to specify number of build threads 
* | | 6ebab10c safer, simpler error handling in lbmk 
| |/  
|/|   
* |   6b11f1b0 Merge pull request 'config: Add Dell Latitude E5420' (#191) from nic3-14159/lbmk:latitude-ports into master 
|\ \  
| * | 036bf2c6 config: Add Dell Latitude E5420 
* | |   457a7037 Merge pull request 'util: Import autoport with Haswell patches' (#195) from nic3-14159/lbmk:autoport-fork into master 
|\ \ \  
| |_|/  
|/| |   
| * | 8cba2370 util: Import autoport with Haswell patches 
|/ /  
* |   c578fe56 Merge pull request 'Use proper autolink' (#192) from eo/lbmk:master into master 
|\ \  
| |/  
|/|   
| * 98caceb1 Use proper autolink 
|/  
* 665840b2 coreboot/dell9020*_12mb: Disable IOMMU by default 
* 944cafa2 coreboot/haswell: make IOMMU a runtime option 
* db074b78 enable serial console on fam15h boards 
```

You may find archives of this release, by looking at the Libreboot download
page. Support is available on IRC or Reddit if you need help.

Disabled boards
===============

Libreboot's build system can be configured to exclude certain boards in
release archives, while still permitting them to be re-built.

All of the following boards have been disabled in the build system:

HP EliteBook 820 G2, because refcode cannot be inserted reproducibly yet. This
is what enables the gigabit ethernet on the machine (it's a Broadwell machine
so still needs MRC). A future release will fix this, and there are three
viable ways: execute an uncompresed refcode instead, or use tar
reproducibly (impossible to guarantoo on the host so tar and xz would have
to be compiled by lbmk), or: replace the blob. None of the possible solutions
are fully viable, so lbmk will support this board but ROM images for it will
be excluded in releases (at least for the time being)

D510MO and D945 images not included either, due to lack of testing. (820 G2
is believed to be stable and has been tested repeatedly)

*All other boards have ROM images in this release.*

eDP mods (ThinkPad X230/X220)
==========================

The `x230edp_12mb` and `x220edp_8mb` targets were removed, but
the `x230_12mb` and `x220_8mb` targets were retained. Only the original
nitrocaster mod (for eDP) is reliable in my experience, and it's unknown what
you get with the various knockoffs available on aliexpress. Delete the board
from Libreboot, to reduce the maintenance burden. Use an older Libreboot
revision if you want these boards. They will probably not be re-added to
Libreboot, unless Nitrocaster re-opens and/or a professional/reliable
alternative appears(the alternatives as of today are all assumed to be rubbish).

The nitrocaster store seems to be out of business at this time of writing,
and these modded boards are uncommon enough as it is, making testing extremely
challenging; testing on multiple machines is desirable, but most people who
do these mods don't want to then mess with their hardware afterward.

The good news is that coreboot has mainlined X230 eDP support, so you will
always have that option readily available. The other bad news with this mod
is the knockoff gear generally has poor documentation (Nitrocaster has very
good documentation), and people frequently have problems, either by their own
fault or by virtue of the product; the eDP-based targets are therefore a liability
to the Libreboot project.

That is all.
