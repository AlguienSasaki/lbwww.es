---
title: BSD operating systems
x-toc-enable: true
...

Guide last updated on 16 November 2022.

NOTE: This guide pertains to x86 hosts, and does not cover supported CrOS/ARM
chromebooks. For ARM targets, you should refer to u-boot documentation.

libreboot is capable of booting many BSD systems. This section mostly documents
the peculiarities of libreboot as it pertains to BSD; you can otherwise refer to
the official documentation for whatever BSD system you would like to use.

Kernel Mode Setting
===================

Your BSD system *must* support Kernel Mode Setting for your graphics
device (most of them do nowadays). The reasons will become apparent, as
you read this article.

Boot BSD, using SeaBIOS
=======================

On x86 platforms, Libreboot provides the choice of GRUB and/or
SeaBIOS payload. GRUB can technically boot BSD kernels, but the code is
poorly maintained and unreliable for this use-case scenario; on BIOS systems,
GRUB can chainload BSD bootloaders, but on bare metal (as coreboot payload),
GRUB can only chainload other coreboot payloads or boot Linux/BSD kernels
directly (but direct booting is only really reliable for Linux, in GRUB).

It is recommended that you boot in text mode, with SeaBIOS. You can literally
just follow the official installation guides for your BSD system, whether it
be FreeBSD, OpenBSD or others.

If you don't plan to set up Xorg/Wayland, then that's all you really need to
do. For example, you might want to run a headless server, in which case you
probably don't mind running in text mode all the time.

OpenBSD and corebootfb
----------------------

It's still recommended to use SeaBIOS in text mode, but OpenBSD specifically
can work with SeaBIOS booting in a coreboot framebuffer, with SeaVGABIOS. In
Libreboot ROM images, this would be SeaBIOS images with `corebootfb` in the
file name.

Make sure to select MBR-style partitioning on the installer, and it will
Just Work.

If you're using the GRUB payload but SeaBIOS is available in the boot menu,
you can just select SeaBIOS at said menu, and OpenBSD will work fine.

FreeBSD and corebootfb
----------------------

Assumed broken, so please ensure that you boot with SeaBIOS payload in text
mode (lbmk ROM images with `txtmode` in the file name, not `corebootfb`).

Warnings for X11 users
----------------------

One important peculiarity of most libreboot and libreboot systems is: VGA mode
support exists, if booting with corebootfb (coreboot's own framebuffer) and
the SeaVGABIOS option ROM used in the SeaBIOS payload; however, the ability
to switch modes is not present, which means you can't switch to text mode
either.

Coreboot can start in framebuffer (corebootfb) or INT10H text mode, and it
stays in whatever mode was set, unless KMS is used to change the mode. It
should be noted that the coreboot framebuffer is not a VGA mode, but instead
coreboot implements minimal drivers for hardware that it supports, providing
a framebuffer directly in memory, which software (such as GRUB) can simply
use.

The BSD bootloaders on x86, in BIOS systems, typically expect text mode
startup. It is usually possible to set the console to higher VGA modes,
on most systems, but not on most coreboot systems with native video
initialisation used, due to the quirks already described. If you see any
documentation (in BSD land) pertaining to VESA modes, ignore it entirely;
unless you're using the proprietary VGA ROM for your device, it won't work,
and Libreboot doesn't distribute these (instead, coreboot's own video
initialisation is used where possible, or a headless SeaBIOS payload setup
is provided, where you would either run it headless or install a graphics
card).

Now, this would otherwise mean: no X11/Wayland. If you start in corebootfb
mode with SeaVGABIOS, you won't get a display in BSD bootloaders, and if you
boot in text mode, you can't set VESA modes from BSD. However, you're in luck:

At least OpenBSD and FreeBSD (possibly others) all have excellent KMS
support nowadays; short for `Kernel Mode Setting`. This avoids the inefficiency
of BIOS/UEFI methods, by having the kernel set modes directly. It is based on
KMS drivers that the BSD projects ported over from the Linux kernel. With this,
you can use X11/Wayland in FreeBSD (and just X11 in OpenBSD, for now).

For example: on FreeBSD, you can install `graphics/drm-kmod` as a package
or from ports, and (for Intel GPUs) do this:

	sysrc kld_list+="i915kms"

This creates the following entry in `/etc/rc.conf`:

	kld_list="i915kms"

On FreeBSD it is also recommended that you switch to KMS on the console/TTY;
add this to `/boot/loader.conf` so that you can still use the console after
terminating Xorg:

	kern.vty=vt

You should not rely on the above instruction (for FreeBSD), because the exact
step might change, and it does not go into full detail either. Refer to the
documentation provided by your system, to know how KMS is configured.

ALWAYS READ THE MANUAL
----------------------

All of the BSDs have *excellent* documentation; it's one of the defining
characteristics, versus typical Linux distros.

Aside from this quirk in coreboot, regarding *BIOS* video modes, the BSDs
otherwise work in exactly the same way as you would expect, and you can
follow along to their official documentation without much fuss.

No specific or detailed guides will be provided here, because SeaBIOS is
fairly self-explanatory; you can otherwise refer to the SeaBIOS
documentation.

If you're flashing a ROM for a machine where `seabios_withgrub`
and `seabios_grubfirst` ROMs are available, choose `seabios_withgrub`.

DO NOT USE ROM IMAGES WITH `seabios_grubfirst` IN THE FILE NAME! These were
present in older Libreboot releases, and supported in previous revisions
of the build system, but they did not work for the intended purpose. More
info is written on the [Libreboot installation guide](../install/). ROM
images with `seabios_grubfirst` in the filename will NOT be included in
future Libreboot releases.

Dubious mention: Tianocore
--------------------------

Tianocore is extremely bloated, and unauditable, so it is not included
in Libreboot firmware, but it is the reference UEFI implementation by
Intel and contributors. It can boot most BSD systems very well.

More robust ways to provide UEFI services in Libreboot are to be investigated.
Tianocore integration will not be provided officially, in any current or future
releases of Libreboot.

Desktop users
-------------

NOTE: This section may not be full accurate; for example, the hardware page
about HP Elite 8200 SFF talks about use of graphics cards on both corebootfb
and txtmode setups, and seems to work fine with SeaBIOS in both cases.

Desktop users on Libreboot should just install a graphics card,
and again boot with SeaBIOS in text mode; however, when you do this,
SeaBIOS will execute the VGA option ROM on the card which will provide
early video initialisation instead of coreboot's initialisation, and that
VGA ROM will usually implement full INT10H modes, including the ability
to set modes in the BIOS (using interrupts), in which case you don't
need to worry about Kernel Mode Setting, but you should still use KMS
anyway.

The reason to use KMS is because it's more efficient. The INT10H service can
only be called in Real Mode or Virtual 8086 mode; v8086 is unavailable in
long mode (x86\_64) and switching into Real Mode just to set VGA modes is
extremely expensive computationally speaking. This is why modern kernels
(Linux and BSD one) do mode setting themselves.

You can learn more about INT10H text/VGA modes here:

<https://en.wikipedia.org/wiki/INT_10H>
