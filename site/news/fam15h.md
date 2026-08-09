% ASUS KGPE-D16, KCMA-D8 and KFSN4-DRE re-added to Libreboot
% Leah Rowe
% 16 July 2023

Freedom as in... again
-------------------------

[Libreboot 20211122](libreboot20211122.md) was the last release to officially
support these boards: ASUS KFSN4-DRE, KCMA-D8 and KGPE-D16; they were removed
much later, in subsequent releases.

I'm pleased to announce that they have, today, been re-added to `lbmk.git`.
This is the [automated build system](../docs/maintain/) that builds Libreboot
ROM images, and Libreboot releases, ready for installation.

These boards are available *now*, if you're willing to [build Libreboot from
source](../docs/build/); otherwise, you will find them in the next regular
release of Libreboot.

It was implemented with this patch:
<https://codeberg.org/libreboot/lbmk/commit/af084014f04602f570a23b0cc23a112401348faf>

### Differences now, versus Libreboot 20211122

The following changes have been made, relative to Libreboot 20211122:

* Coreboot `4.11_branch` (branch) used, instead of regular `4.11` (release tag).
  This contains many fixes on top of the original Coreboot 4.11 release that
  Libreboot used.
* I fixed several errors, enabling coreboot's crossgcc toolchain compilers
  from `4.11_branch` to *build* properly, on modern distros; Debian Sid was
  tested, and Arch Linux should work too. Without these patches, it would not
  build at all.
* Fixed downloading of `acpica` (for use of `iasl`) in the coreboot crossgcc
  build script; the upstream link for tarballs of `acpica` were no longer
  online. Coreboot switched to github in newer revisions, but this older one
  needed patching. I ended up hosting the relevant tarball *myself*, on
  Libreboot rsync. The mirrors now have it, and Libreboot's modified
  coreboot `4.11_branch` uses that.
* Libreboot's build system (`lbmk.git`) has been modified, to make it avoid
  building GCC Gnat, in crossgcc from coreboot `4.11_branch`. Coreboot
  implements video initialisation in *Ada* on many platforms, but these AMD
  boards use AST (Aspeed) framebuffer chips, where coreboot video initialisation
  is written in normal C. This avoids having to deal with build issues, when
  building older Gnat with newer host
  Gnat: https://gcc.gnu.org/install/prerequisites.html

The PIKE2008 fix from Libreboot 20211122 was retained, and it is included in
today's change. This inserts an empty option ROM in CBFS, without which SeaBIOS
would hang; the empty option ROM prevents SeaBIOS from loading the real one.

### Dasharo firmware

**Libreboot does not yet integrate Dasharo.**

Dasharo has support for ASUS KGPE-D16 on a much newer version of coreboot,
where *upstream* coreboot had deleted KFSN4-DRE/KCMA-D8/KGPE-D16 support just
after the 4.11 release; however, Dasharo *only* supports KGPE-D16 (out of these
boards).

I intend to study code differences between D8/D16 in time, and port those to
Dasharo so that it can have a KCMA-D8 port. I then wish to use this in
Libreboot. I *could* use Dasharo now, for D16, regardless. For now, the main
priority is to have support for these boards again in Libreboot.

The previous news announcement said that I would only add these boards after
doing that, but I felt this work is worthy enough for entry in the main branch
of Libreboot. Any issues that it has will be similar to what was present in the
Libreboot 20211122 release (Dasharo made a lot of improvements, especially to
raminit).

I would like it if people can provide testing for all of these boards, in
today's Libreboot's revisions. [Learn here how to become a board tester for
the Libreboot project](../docs/maintain/testing.md).


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
