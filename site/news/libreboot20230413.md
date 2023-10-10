% Libreboot 20230413 released!
% Leah Rowe
% 13 April 2023

**PLEASE READ THIS BEFORE INSTALLING:
[Safety issues updating Libreboot on Sandybridge/Ivybridge/Haswell](safety.md)**

Introduction
============

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on ARM-based chromebooks supported (U-Boot bootloader, instead of Google's
depthcharge bootloader). On x86 machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. It provides an automated build system for the configuration and
installation of coreboot ROM images, making coreboot easier to use for
non-technical people. You can find the list of supported hardware in the
Libreboot documentation.

The last Libreboot release, version 20230319, was released on 19 March
in 2023. *This* new release, Libreboot 20230413, is released today on
April 13th, 2023.

This is marked as a *testing* release, though it is *basically stable*.
It is a bugfix release, relative to Libreboot 20230319. In addition to this,
massive code cleanup has been performed on parts of the build system.

The priority of this release has been build system fixes/improvements. For the
time being, no more code changes will be made unless needed, in coreboot, for
existing supported hardware; the focus is going to be on adding *more* boards
to Libreboot, to support more hardware. I've been on a spree, buying lots of
mainboards that coreboot supports, that would be interesting in Libreboot.

READ THIS BEFORE FLASHING LIBREBOOT, OR YOU MIGHT BRICK YOUR MACHINE
====================================================================

**On newer Intel platforms that require Intel ME and/or MRC firmware, such as
ThinkPad X230 or T440p, and/or HP laptops that require KBC1126 EC firmware,
the release ROMs of Libreboot are MISSING certain files, that you must insert
yourself. FAILURE to adhere to this warning may result in you bricking your
machine (rendering it unbootable), if you were to flash the release ROMs without
modifying them in any way. For more information, please read:**

**[Insert vendor files on Sandybridge/Ivybridge/Haswell](../docs/install/ivy_has_common.md)**

This isn't required on *all* Libreboot-supported boards, but if in doubt, follow
these instructions anyway. If you run the vendor scripts on a board that doesn't
need any vendor files, then nothing will happen.

Build from source
-----------------

*This* release was build-tested on Debian *Sid*, as of 13 April 2023. Your
mileage may vary, with other distros. Refer to Libreboot documentation.

KCMA-D8 and KGPE-D16 wanted!
----------------------------

[ASUS KGPE-D16 and KCMA-D8 needed for testing!](kgpe-d16.md)

These boards still haven't made it back to Libreboot, but I wish to re-add
them in a future release. If you can give/loan me a fully assembled workstation
with one (or both) of these, I would appreciate it. Please
[get in touch](../contact.md)!

Work done since last release
============================

For more detailed change logs, look at the Git repository. This is
a summary of changes.

Summary of changes:
-------------------

Zero code changes within coreboot on any boards, but the build system went
through a mild overhaul:

* Massive code cleanup of `util/nvmutil`, and improved error handling.
* Insert scripts for post-release sandybridge/ivybridge/haswell ROMs are now
  much easier to use, and less error-prone.
* A few problematic boards, for which ROM images were excluded in the previous
  release, have now been *removed* from the Libreboot build system. They will
  be re-added in a future release, and you can find a list of them in the
  detailed changelog below.
* Haswell (T440/W541): `mrc.bin` insertion is now *correct*, on the blobutil
  script. In the previous release, Libreboot's build system inserted this at
  the wrong location in the ROM, after downloading it; coreboot's own build
  system does it correctly, and that is used when compiling, but post-release
  ROM images have it inserted with equivalent logic in the Libreboot build
  system, instead. - NOTE: libre raminit is also available, if you choose
  the ROM images with `t440p_12mb` in the filenames, whereas `t440pmrc_12mb`
  images need `mrc.bin` for raminit. More info about this is in the previous
  release.

MRC W541/T440p ROM images re-added
----------------------------------

As alluded to and applied by the above text, T440p/W541 thinkpad images that
use `mrc.bin` have been re-added. Libreboot supports experimental raminit on
these boards, where the MRC file is not required, but it also provides ones
that use it, so that users can choose which one they want.

More information this is available, in the *previous*
[Libreboot 20230319 release announcement](libreboot20230319.md).

Detailed descriptions of changes
--------------------------------

Changelog:

* `blobutil/inject`: Relative to the fix below (courtesy `shmalebx9`), the
  ROM image archives in releases now contain lists of SHA1 hashes, matching
  what each file should be when running `blobutil/inject`
* `blobutil/inject`: It is now possible to insert MRC and neutered ME images,
  where required on specific mainboards, into *all* ROM images of a given
  tar archive, in addition to single ROM images. The patch for this is
  courtesy of Caleb La Grange (`shmalebx9` on libreboot IRC)
* Removed daisy/peach chromebooks: The machines are believed to boot properly,
  with the coreboot and u-boot code being correct, but lbmk currently does not
  handle the BL1 bootloaders on these machines, and this was overlooked before;
  the images for these machines have also been deleted, from previous releases.
  These will be re-added in a future Libreboot release.
* Removed veyron chromebooks for now: u-boot does not work at all on those
  boards (video issues), last known revision working on veyron was 2021.01 so
  a git-bisect can probably done. These boards will be re-added in a future
  Libreboot release.
* `util/ich9utils`: Merged it back into lbmk, rather than keeping it as a
  separate repository. It is now directly part of the Libreboot build system,
  once again.
* `util/nvmutil`: Major code cleanup; reduced SLOC count to 315 source lines,
  whereas it was 386 code lines in the previous release. The compiled binary
  sizes have been reduced by 7%, as tested with TCC on an x86\_64 host. This
  code size reduction is provided, *without* reducing any functionality.
* `util/nvmutil`: Fixed faulty check for MAC address `00:00:00:00:00:00` - the
  total was being reset for each word, wrongly. This has been corrected.
* `blobutil/download`: now supports extracting `me.bin` from LZMA archives, in
  addition to inno archives; in practise, lbmk still currently only supports
  machines where inno archives are extracted from, but experimental new ports
  exist outside of `master` that will be present in future releases.
* `blobutil/download`: no longer hardcodes the `me.bin` path, when extracting
  from updates during auto-download. When compiling ROM images, lbmk now does
  this by bruteforce, automatically finding the correct location of the ME
  image inside vendor archives; this works well on inno/lzma archives.
  The script then runs `me_cleaner` as usual, inserting that into the ROM
  image - this same logic is also used when inserting into release ROMs.
  This is in preparation for *non-Lenovo* sandybridge, ivybridge and haswell
  machines being added in future releases, e.g. Dell and HP laptops that
  coreboot supports.
* `blobutil/download`: heavily re-factored the logic, switching to top-down
  order of functions, split more operations into functions, generally made
  the script easier to read/work on.
* `blobutil/inject`: use correct offset for insertion of `mrc.bin` (Haswell
  platform, e.g. ThinkPad T440p) - as written above.
* Removed mainboard: `d945gclf` - known to be problematic at boot. I have one,
  and I'm going to test it for re-entry in a future release.
* Added missing dependency in Arch Linux dependencies installation script,
  patch courtesy of Andreas Hartmann.

Hardware supported in this release
==================================

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

Desktops (AMD, Intel, x86)
-----------------------

-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)

### Laptops (Intel, x86)

-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   Lenovo ThinkPad X230
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)
-   [Lenovo ThinkPad T440p](../docs/install/t440p_external.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   [Lenovo ThinkPad W541](../docs/install/ivy_has_common.md)

### Laptops (ARM, with U-Boot payload)

-   [HP Chromebook 14 G3 (nyan-blaze)](../docs/install/chromebooks.md)
-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](../docs/install/chromebooks.md)
-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Downloads
=========

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.
