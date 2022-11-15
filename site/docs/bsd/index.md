---
title: BSD operating systems
x-toc-enable: true
...

Guide last updated on 24 August 2022.

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

On x86 platforms, libreboot/libreboot both provide the choice of GNU GRUB and/or
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
and libreboot/libreboot don't distribute these (instead, coreboot's own video
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

Dubious mention: Tianocore
--------------------------

Tianocore is extremely bloated, and unauditable, so it is not included
in libreboot or libreboot, but it is the reference UEFI implementation by
Intel and contributors. It can boot most BSD systems very well.

More robust ways to provide UEFI services in Libreboot are to be investigated.
Tianocore integration will not be provided officially, in any current or future
releases of Libreboot.

Desktop users
-------------

Desktop users on libreboot/libreboot should just install a graphics card,
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
