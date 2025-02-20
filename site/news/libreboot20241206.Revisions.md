---
title: Libreboot 20241206 release revisions
x-toc-enable: true
...

This article concerns Libreboot 20241206. For general information about that
release, please read
the [Libreboot 20241206 release announcement](libreboot20241206.md).

Occasionally, stable releases such as this one may contain minor (or critical)
issues that went unnoticed during testing. When this occurs, critical or
otherwise desirable fixes are implemented, while not fundamentally altering
the substance of the original release.

When this occurs, the ROM image and source code archives are entirely re-built,
and re-uploaded, replacing the old one. Patch files are provided alongside the
updated source archive, so that you can revert (from it) back to the older
revisions if you wish; by doing this, you can therefore also re-create the
original release archive, for reference.

Revision 1 (8 December 2024)
----------

A bug was found whereby `seabios_` images are created twice, This was fixed in
the revision `e3b77b132e6b5981c09bc1ce282afaae64058ab3`. This bug caused vendor
file insertion to fail on release images, because the `vendorhashes` file would
list the file twice, but one of the hashes would be wrong.

This is because the build system wrongly created U-Boot-only images first with
that name, because the `add_uboot` function in `rom.sh` unconditionally created
images with U-Boot in them; the function also creates ARM64 images, where U-Boot
is the primary payload.

On x86 machines, SeaBIOS should be the main payload, chainloading U-Boot.

The build system would then create the actual `seabios_` image, replacing the
other one, on x86 machines.

So, the command to create the first image was removed. However, just before
uploading `rev1` images, it was discovered that this would cause no U-Boot
images to be built for ARM64 devices, which lead to Revision 2:

Revision 2 (8 December 2024)
----------

See commit ID `b910424b5df8ed7c931a7b8f5cc8e34eacf0ca3e`.

Revision 1 was reverted, and replaced with the following logic instead:

In `add_uboot`, the instruction to create a ROM image was changed so that it
only creates one where U-Boot is the primary payload (which is the case for
ARM64).

The source archive is now `20241206rev2` instead of `20241206`, and the same
is true of affected images on x86, where `vcfg` is set in `target.cfg`.

Revision 3 (11 December 2024)
----------

See commit ID `3b6b283eabe7a655c861d1543aeae49d27566f53` and the two commits
before that.

This revision disables PCI-E Re-plug (hotplug feature) for the NVMe SSD on
Dell OptiPlex 3050 Micro, to prevent the device from being renamed randomly;
such renaming will cause a system crash if booting Linux from the NVMe.

In my case, I was running RAID1 (SATA+NVMe) and every few days, the RAID1 got
unsynced; I simply re-synced and it was fine, but what if that was a RAID0? It
would possibly corrupt the entire array.

This revision should prevent the issue from occuring. Without this patch,
the `nvme0n1` device (as an example) might randomly rename to `nvme0n2`, because
Linux would see it as a new device.

This same fix was made to the ThinkPad T480/T480s to fix the same issue there,
which manifested during S3 resume, but that bug never made it into the release
because it was fixed *before* the initial release of Libreboot 20241206.

The ROM images were all re-uploaded, compiled from the rev3 tarball, because it was discovered that the rev2 tarballs had a GRUB built showing the old Libreboot 20241008 version number; the actual code in GRUB matched the code for 20241206, but it was a cached GRUB build from just before updating the version number for release. This is because the rev2 ROM image tarballs were done manually, to avoid a full re-build of every target in lbmk. To avoid all doubt, all ROM images have been re-compiled with the version number corrected, from the rev3 tag.

Revision 4 (17 December 2024)
----------

Rev4 fixed a bug: GRUB was not allowing the background image to be changed,
despite rules in `grub.cfg` that made one in CBFS load before memdisk. This fix
was implemented by no longer inserting background images into GRUB's memdisk,
instead inserting them into CBFS.

This way, you can remove what's in CBFS and replace it with your own, if that's
what you want to do.

To celebrate this fix, the default background logo was also changed. The old
one was a white silhouette of the Libreboot logo, whereas the new one is of
the same shape but shows a rainbow-coloured gradient instead of all-white. This
rainbow logo was also used in U-Boot on the very initial Libreboot 20241206
release, and it's also used for the main website logo at the time of this
revision.

Basically, this fix was done as an excuse just to do another revision update,
to change the logo! The actual bug was actually quite minor and irrelevant.

Revision 5 and 6 (17 December 2024)
----------------

All current ROM/src archives in this release match changes up to rev6.

I decided afterall to keep using the boring all-grey silhouette logo in GRUB.
I also made the same decision for U-Boot.

The plain logo is less eye catching and looks less out of place, and it's
uncontroversial in every way. This revision still contains the fix allowing
GRUB backgrounds to be changed. Rev5 made this change to GRUB and Rev6 made
it to U-Boot. All ROM images were re-compiled to rev6, and re-uploaded.

A minor change, to be sure. I need Libreboot to be as trouble-free as possible
to everyone, and some countries are culturally hostile to the particular
colour gradient used by the old logo (from rev4); even if I preferred that
logo, I want those users to have no trouble at all using Libreboot in public.
Libreboot's only purpose is to provide free boot firmware.

Revision 7 (18 December 2024)
----------

Added support for fetching ThunderBolt firmware and padding it, on ThinkPad
T480 and T480s motherboards. This can be used for externally flashing the newer
firmware update, without the need to boot Windows to use Lenovo's own updater.

Revision 8 (6 January 2025)
----------

There are *so many* changes, in this revision, that it is being treated like
a brand new release, though it is still merely a revision to the existing
Libreboot 20241206 release.

Please read the [Libreboot 20241206 8th
revision release announcement](libreboot20241206rev8.md).

Here's a short summary of the revision 8 changes, but you should also read
the full announcement, linked above:

* HP EliteBook 820 G2 images are now *provided*, whereas this port was
  previously *source-only*; this is due to fixes that have been made to
  the build system, that mean the images now have correct checksums after
  running the `./mk inject` commands.
* Several upstream code revisions have been updated, with minor fixes.
* ThinkPad T480/T480s backlight keys now work perfectly, whereas you could
  only change the brightness with software before.
* Thinkpad T480/T480s and OptiPlex 3050 Micro no longer automatically turn
  on by merely plugging in a power supply; you must now actually press the
  power button. This is especially desirable on the T480.
* Safer vendorfile insertion, on `include/vendor.sh` - much stricter error
  handling, and the way it works now reduces the number of possible bricks.
* Intel FSP now inserted via `./mk inject` commands, instead of being
  included directly inside release images. This is to mitigate technicalities
  of Intel's licensing, pertaining to modifications, since coreboot has
  to specifically split up the various modules and adjust various pointers
  inside the M module.
* General bug fixes in the build system.
* Fix build errors on Debian Trixie/Sid and Arch Linux, as of January 2025.
* Pico 2 support added for the `pico-serprog` images, courtesy Riku Viitanen.
* Disabled TPM2 on T480/3050micro to mitigate SeaBIOS hanging, due to buggy
  TPM drivers in SeaBIOS.
* Better handling of dependencies, for various Linux distros.
* Various code quality improvements for `util/nvmutil`, especially the Makefile.

Revision 9 (12 February 2025)
-----------

Merged a fix from Riku Viitanen, fixing an uninitialised variable in
pico-serprog, which caused sync issues on pico2 devices or other dongles
that used RP2530. See:

<https://codeberg.org/Riku_V/pico-serprog/issues/3>

This bug page on the upstream project describes the bug, and the fix as
mentioned there was merged in Libreboot's mirror, here in this commit:

<https://codeberg.org/libreboot/pico-serprog/commit/f6c0e3b15ca5de98ad6c46b11813ca7c7a93b796>

People who previously downloaded the tarball for pico-serprog images, should
download again, choosing 20241206rev9.

The coreboot tarballs have not been replaced. The source tarball *has* been
replaced.

Revision 10 (18 February 2025)
-----------

Several critical security fixes made to GRUB, and a few improvements to LVM
scanning in `grub.cfg` were made. Other minor fixes. A full announcement is
available in the [20241206rev10 announcement](libreboot20241206rev10.md).
