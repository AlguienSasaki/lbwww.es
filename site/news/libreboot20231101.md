% Libreboot 20231101 released!
% Leah Rowe
% 1 November 2023

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

Free Software Boot
------------------

*This* new release, Libreboot 20231101, released today 1 November 2023, is
a new *testing* release of Libreboot. The previous release was
Libreboot 20231021, released on 21 October 2023.

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
----------------------------

### ThinkPad X201 support added

**UPDATE: SUPPORT WAS REMOVED on 12 January 2024, because it was found that fans
fail when using a neutered ME, on this platform - this bug only affects the
older arrandales like X201, but newer platforms are not affected.**

Intel D945GCLF was also re-added, in this release.

### Coreboot, GRUB, U-Boot and SeaBIOS revisions

Libreboot 20231021 and 20231101 are both based on these revisions:

* Coreboot (default): commit ID `d862695f5f432b5c78dada5f16c293a4c3f9fce6`, 12 October 2023
* Coreboot (cros): MERGED WITH `coreboot/default` (see above)
* Coreboot (fam15h\_udimm): commit ID `1c13f8d85c7306213cd525308ee8973e5663a3f8`, 16 June 2021
* GRUB: commit ID `e58b870ff926415e23fc386af41ff81b2f588763`, 3 October 2023
* SeaBIOS: commit ID `1e1da7a963007d03a4e0e9a9e0ff17990bb1608d`, 24 August 2023
* U-Boot: commit ID `4459ed60cb1e0562bc5b40405e2b4b9bbf766d57`, 2 October 2023

However, Libreboot 20231101 has added several new patches on top of several
of these, that fix certain bugs or improve certain functionalities. More
information is available elsewhere in this page.

Build system tweaks
-------------------

These changes were made:

* i945 bootblock copy: during ROM building, the bootblock must be copied from
  the main 64KB block, to the backup block, for use with `bucts`, but this
  wasn't done in the last release; this regression has now been fixed.
* Re-add SeaGRUB support - enabled on Dell Latitude E6400 and Dell Precision
  T1650. This is useful in cases where a VGA BIOS might be used. In that case,
  start SeaBIOS payload first but only ever load GRUB and disable the SeaBIOS
  menu. This is available in ROM images with `_grubonly` in the file name.
  This is user-equivalent to running GRUB as first payload, because SeaBIOS
  will never allow user input, only switching instead to GRUB. GRUB offers many
  security features like password locks and GPG checking.
* Export `LC_COLLATE` and `LC_ALL`, setting it to `C`, to ensure
  consistent sorting; the Libreboot build system heavily relies on sorting
  by alphanumerical order with numbers and capital letters first, e.g. when
  applying patches.
* GRUB config: Re-wrote the text on some entries to make them easier to
  understand for novice users.
* Add HECI timeout for Ibex Peak, courtesy of a patch written by
  Denis `GNUtoo` Carikli in 2017 - on neutered ME setups (using `me_cleaner`),
  the coreboot code checking HECI can cause a hang, so a timeout is introduced.
* Crank up VRAM to max settings on all boards with Intel graphics (shared system
  memory) - e.g. some ivybridge boards had it set to 32MB, now it's 224MB.
* GRUB: Don't spew the "Unknown key" message on unknown/invalid key presses -
  otherwise, a faulty keyboard with stuck keys will make the message spew
  repeatedly, making GRUB unusable.
* Fix S3 suspend/resume on Sandybridge/Ivybridge/Haswell Intel platforms, by
  disabling TSEG Stage Cache in coreboot configs.
* Revert a heapsize patch in coreboot that broke S3 suspend/resume in some
  boards - though we still needed to make another tweak. See above.
* Dell Latitude E6400 and E6430: fixed keyboard initialisation in GRUB. GRUB
  was using scancode set 2 without translation. The EC on these Dell laptops
  allows to set the scancodes and reports back what you set, but only ever
  actually uses scancode set 1 (XT) - so, the fix makes GRUB always use set 2
  with translation (treated as 1). This is the same behaviour used in SeaBIOS.
  This fixes E6400/E6430 GRUB, and doesn't break other boards as per testing.
* Fix coldboot on Dell Latitude E6400, courtesy of a patch by Angel Pons (IRC
  inick `hell` on coreboot IRC) - previously, this machine booted unreliably,
  though reboot was fine. Angel wrote a patch for another board, that also
  works on the E6400 quite nicely.
* Copy base ROM a second time in `build/roms` - fixes U-Boot building for x86
  QEMU which also has GRUB and SeaBIOS support.
* U-Boot x86 (QEMU): builds again, so re-added
* QEMU ARM64: video console now enabled in U-Boot (courtesy Alper Nebi Yasak)
* Dependencies scripts: updated a few of them, for newer versions of distros.

FULL list of changes (git log)
------------------------------

The log is as follows, relative to Libreboot 20231021:

```
* 82bd87fa build/roms: re-add SeaGRUB build support 
* 971f6517 add 512kb d945gclf config 
* dfc5423c export LC_COLLATE=C and LC_ALL=C 
* f9993495 d945gclf: add noblobs/nomicrocode label 
* ed27ab8a grub.cfg: use better description in menu entries 
*   2c7f83bc Merge pull request 'add intel d945gclf_8mb support based on previous libreboot configs' (#144) from fbraghiroli/lbmk:master into master 
|\  
| * 00dd3e4a add intel d945gclf_8mb support based on previous libreboot configs 
* | 65675a20 build/roms: properly print noblobs rom names 
* | 42fde8e5 update/release: insert fake x201 me.bin 
* | 1306c9d2 Revert "coreboot/default: use alternative heap size fix" 
* | d218088d coreboot/all: disable TSEG stage cache 
* | 91faeb8d crank up vram allocation on more intel boards 
* | 026d57ff GRUB: don't spew "Unknown key 0xff" in error 
* | 29e9c32e coreboot/default: use alternative heap size fix 
* | 9606c68c fix grub keyboard init on dell e6400 and e6430 
|/  
* 34f56853 fix raminit/coldboot on dell e6400 
* 27efbc6f add heci timeout for ibex peak 
* 7e6fd7e5 add lenovo x201 support 
* 93458de7 revert coreboot heap size patch 
* 83bf2376 coreboot/fam15h: don't set microcode_required 
* 5f6ba01d include/option.sh: fix i945 bootblock copy 
* df031d42 use mirrorservice.org for acpica downloads 
* 85bc9156 build/roms: copy base rom again for u-boot 
*   f1785c3f Merge pull request 'u-boot: Add qemu_x86_12mb build again' (#143) from alpernebbi/lbmk:uboot-qemu-x86 into master 
|\  
| * 03c830b2 u-boot: Add qemu_x86_12mb build again 
* |   b353b0c7 Merge pull request 'u-boot: qemu_arm64_12mb: Enable video console' (#142) from alpernebbi/lbmk:uboot-qemu-arm64-video into master 
|\ \  
| |/  
|/|   
| * 444f2899 u-boot: qemu_arm64_12mb: Enable video console 
|/  
* 280bcceb Merge pull request 'arch, fedora38, parabola, void: install python-setuptools' (#141) from Riku_V/lbmk:master into master 
* 3b92ac97 arch, fedora38, parabola, void: install python-setuptools 
```

This is much more conservative than the previous release, because not much
time has passed since then. The focus of this Libreboot 20231101 release has
been to fix bugs.

Hardware supported in this release
----------------------------------

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](../docs/install/kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](../docs/install/kgpe-d16.md)

### Desktops (AMD, Intel, x86)

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
---------

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.

Post-release errata
-------------------

### S3 suspend/resume

The previous release broke suspend/resume on all Intel platforms, but it's
fixed in this release on Sandybridge (e.g. ThinkPad X220),
Ivybridge (e.g. ThinkPad X230, Dell Precision T1650) and
Haswell (e.g. ThinkPad T440p, W541).

However, GM45 (e.g. ThinkPad X200 / T400) is still confirmed broken - though
it may work for some. S3 suspend/resume has always been flaky on GM45.

i945 testing is inconclusive. (e.g. ThinkPad X60, T60)

x4x (e.g. GA-G41M-ES2L) remains untested.

Pineview (D945GCLF) is untested for S3.

The AMD boards should work fine with suspend.
