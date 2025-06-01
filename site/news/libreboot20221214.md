% Libreboot 20221214 released!
% Leah Rowe
% 14 December 2022

Free as in freedom!
--------------------

The last Libreboot release, version 20220710, was released on 10 July
in 2022. *This* new release, Libreboot 20221214, is released today on December
14th, 2022. This is intended to be a *testing* release.

This is marked as a *testing* release, but most/all boards should be fairly
stable.

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

### Build from source

*This* release was build-tested on Debian 11. Your mileage may vary, with
other distros.

Work done since last release
--------------------

For more detailed change logs, look at the Git repository. This is
a summary of changes.

### New boards, x86

This list *may not be complete*, but it is as follows:

* QEMU (arm64 and x86\_64) images added, for testing payloads and so on.
* lenovo/t430 (thinkpad)
* lenovo/x230 (thinkpad)
* lenovo/x230edp (e.g. nitrocaster FHD mod kit)
* lenovo/t440p (thinkpad)
* lenovo/w541 (thinkpad)
* lenovo/x220 (thinkpad) (also works with x220 tablet)
* lenovo/t420 (thinkpad)
* lenovo/x230 tablet (thinkpad)

### GA-G41M-ES2L ROMs

GA-G41M-ES2L ROM images are back, but your mileage may vary. Only the
SeaBIOS payload is enabled, on this board.

No lbmk changes were done for this, because the ROMs were simply excluded
in the previous release, but this board was not deleted from lbmk.

**UPDATE ON 20 December 2022: per many user reports, these ROMs work very
well. GA-G41M-ES2L support is therefore stable, for all intents and purposes.
This section of the release announcement previously alluding to issues, but
those speculations were premature, based on limited prior testing.**

### New boards, ARM

NOTE: These boards use u-boot payload, instead of depthcharge.

* Samsung Chromebook 2 13" (peach-pi)
* Samsung Chromebook 2 11" (peach-pit)
* HP Chromebook 11 G1 (daisy-spring)
* Samsung Chromebook XE303 (daisy-snow)
* HP Chromebook 14 G3 (nyan-blaze)
* Acer Chromebook 13 (CB5-311, C810) (nyan-big)
* ASUS Chromebit CS10 (veyron-mickey)
* ASUS Chromebook Flip C100PA (veyron-minnie)
* ASUS Chromebook C201PA (veyron-speedy)
* Hisense Chromebook C11 and more (veyron-jerry)
* ASUS Chromebook Flip C101 (gru-bob)
* Samsung Chromebook Plus (v1) (gru-kevin)

These ARM boards are all Chromebooks.

This work on the Chromebooks and u-boot integration is courtesy
of Alper Nebi Yasak (`alpernebbi` on libreboot IRC).

In these configurations, u-boot is a *payload of coreboot*.

### Removed boards

ASUS KGPE-D16, KCMA-D8 and KFSN4-DRE are removed, because raminit
never worked reliably on these boards to begin with and they were
barely maintained anymore. Use an older release of Libreboot, if
you still want to use these boards.

Known issues
------------

### Intel ME firmware missing in ROMs that need it

If you compile ROM images with lbmk directly, the build system
automatically fetches ME images from the internet and neuters
plus truncated them, using `me_cleaner`. This downloading is done
to avoid distributing them directly in Libreboot, and they get
scrubbed by the release build scripts.

To re-insert neutered/truncated ME into your image, look at
the [guide](../docs/install/ivy_has_common.md).

This applies to sandybridge, ivybridge and haswell Intel platforms,
e.g. X220, T420, X230, T430, T440p. On *older* Intel platforms such
as GM45+ICH9M (X200, T400, etc) the Intel ME image isn't needed and
Libreboot ships with ME-disabled configuration.

### MRC image missing in Haswell ROMs

Ditto ME images. To re-insert, follow
the [guide](../docs/install/ivy_has_common.md).

### Internal flashing on 16MB X230 images

The X230 has two ways of upgrading the default 12MB (8MB and 4MB chips)
flash to 16MB: remove a bunch of resistors and capacitors on the board
(a guide is yet to be written for this) and both flash ICs, and replace
SPI1 with a 16MB chip e.g. Winbond W25Q128FVSIG.

The other method: replace just SPI2 which is a 4MB flash, with an 8MB
flash e.g. Winbond W25Q64FVSIG. This latter method was tested, and it
is made to boot by changing component2density in the IFD to 8MB, but
this is non-standard per Intel specifications.

It boots, but then internal flashing still only lets you flash the first
12MB, even though the bootblock is at the end of that 16MB flash and
does boot perfectly!

So, 16MB images on X230 are experimental. When these ROM images are built,
the required IFD modification is already done in Libreboot's IFD for this
setup.

### S3 suspend/resume on Haswell (T440p/W541)

Totally broken. The suspected cause is component2density setting in IFD
being wrong: ifdtool reports it as being too big for what's actually
flashed.

No fix has been found yet that doesn't brick the machine, so this bug
is still present in the Libreboot 20221214 release.

The issue is that the MRC cache is in the wrong place in memory, because
of that bug in the IFD, but when ifdtool is used to correct component2density,
it bricks, which leads to wonder: is ifdtool's knowledge of these bits on
haswell faulty?

When X230 16MB was experimented with, Haswell was also looked at, and what
seemed to be the same component density bits were set, also resulting in
a brick (it's why only 12MB images are available for haswell in libreboot).

### Chromebook boards mostly untested

Alper has tested the `gru_kevin` ROM images produced by lbmk, but
the other images are untested as of this day.
