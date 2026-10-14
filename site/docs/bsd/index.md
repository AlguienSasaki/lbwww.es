---
title: Install a BSD operating system on Libreboot
x-toc-enable: true
...

**This document is obsolete on modern Libreboot, which now uses EDK2 by
default on most boards. When using EDK2, you can use normal UEFI-based BSD
bootloaders and installation, which works perfectly without intervention. The
rest of this document pertains to the old, now-deprecated setup of standalone
SeaBIOS(as coreboot payload). We recommend UEFI nowadays, for BSD. Please use
the EDK2 payload provided by Libreboot.**

*Please disregard this entire document, if you're doing UEFI-based BSD setup.
The notes about KMS are still useful though, for more efficient graphical
operation but this is true of all operating systems.*

*Please also know: FreeBSD installed in BIOS mode still expects text mode
startup with SeaBIOS; this is not compatible with the framebuffer-based
CSMWrap payload provided inside Libreboot EDK2. Please convert your FreeBSD
installation to EFI, reinstall it or re-flash with SeaBIOS text-mode; the
latter is documented in the Libreboot 26.10 release page.*

It is assumed here that you are using the *SeaBIOS* payload, *not* the GRUB
payload; the U-Boot payload may also work, but that is not covered here. The
SeaBIOS payload must ideally run in text mode (`txtmode` images from Libreboot
releases).

This guide pertains to x86 hosts, and does not cover supported CrOS/ARM
chromebooks. For ARM targets, you should refer to [u-boot
documentation](../uboot/) - and [U-Boot x86](../uboot/uboot-x86.md) is also
available. The U-Boot x86 payload is interesting, because it can in fact boot
OpenBSD via UEFI method (U-Boot provides a lightweight UEFI implementation
independently of, say, EDK2).

What is BSD?
------------

In our context, we are referring to those descendents of 4.4BSD-Lite starting
in the early 1990s. On balance, they are about equal to Linux in many ways,
and some would argue that they are *better* (higher code quality). It can be
said that the BSDs are the closest we have to *true* open source Unix systems,
since they ultimately descend from that code lineage. For example, the
FreeBSD project briefly covers its own history in the Hand Book:
<https://docs.freebsd.org/en/books/handbook/introduction/#history>

Chief among them are:

* [FreeBSD](https://www.freebsd.org/) (HardenedBSD probably also works)
* [NetBSD](https://netbsd.org/)
* [OpenBSD](https://www.openbsd.org/)
* [DragonFlyBSD](https://www.dragonflybsd.org/) (UNTESTED)

**TODO: DragonFlyBSD is untested, as of January 2025. It ought to be tested.**

Many other BSD systems exist, that are largely derived from these.

Common issues with BSD+Libreboot
--------------------------------

This page will not tell you how to install BSD systems; that is best left to
the documentation for your BSD system. Instead, these next sections cover only
the idiosyncrasies of Libreboot as they relate to BSD:

### Kernel Mode Setting

Your BSD system *must* support Kernel Mode Setting for your graphics
device (most of them do nowadays). The reasons will become apparent, as
you read this article.

### Boot BSD, using SeaBIOS

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

#### GRUB payload

GRUB *can* directly boot many BSD systems, but this is ill advisable. You are
advised to use either SeaBIOS, and boot a BIOS-based BSD bootloader, or use
Libreboot's [U-Boot payload](../uboot/) and use it to boot via UEFI; U-Boot's
bootflow menu can achieve this.

The U-Boot coreboot payload is still experimental, on ARM64 *and* x86/x86\_64,
so you should probably use SeaBIOS for now (on x86). U-Boot is the *only*
coreboot payload for Libreboot on ARM64 motherboards.

### OpenBSD and corebootfb

It's still recommended to use SeaBIOS in text mode, but OpenBSD specifically
can work with SeaBIOS booting in a coreboot framebuffer, with SeaVGABIOS. In
Libreboot ROM images, this would be SeaBIOS images with `corebootfb` in the
file name.

Make sure to select MBR-style partitioning on the installer, and it will
Just Work. **GPT partitioning won't work in OpenBSD, if you use the SeaBIOS
payload, but will work if you boot/install it via UEFI boot method with
Libreboot's [U-Boot UEFI payload](../uboot/uboot-x86.md) instead.**

If you're using the GRUB payload but SeaBIOS is available in the boot menu,
you can just select SeaBIOS at said menu, and OpenBSD will work fine.

### FreeBSD and corebootfb

Assumed broken, so please ensure that you boot with SeaBIOS payload in text
mode (lbmk ROM images with `txtmode` in the file name, not `corebootfb`).

Please boot in *text mode*. FreeBSD can be configured to use KMS, if you need
Xorg or wayland.

### Warnings for X11 users

One important peculiarity of most libreboot systems is: VGA mode
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

### Desktop users

NOTE: This section may not be fully accurate; for example, the hardware page
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

If you use the *U-Boot* payload, INT10H is irrelevant because you will rely on
an EFI framebuffer instead, which U-Boot does provide (piggybacking off of the
coreboot framebuffer where one is available).

Regardless of whether you have an EFI framebuffer or INT10H VGA interrupts,
the various BSD systems all support KMS so you should be able to use Xorg or
Wayland just fine.

### Speeding up suspend and resume in FreeBSD

Sometimes, the suspend/resume times in FreeBSD on Librebooted laptop
and on the laptop with proprietary BIOS may vary. The Librebooted
laptop will show longer suspend and resume times. This may not be due
to Libreboot but rather due to configuration quirks in FreeBSD
configuration. Some tweaks should be performed on FreeBSD to decrease
the suspend/resume time. [This article](suspend-resume-speedup.md)
describes how to do that.

ALWAYS READ THE MANUAL
----------------------

All of the BSDs have *excellent* documentation; it's one of the defining
characteristics, versus typical Linux distros. This is precisely *because*
the BSDs develop everything in-house, so the various components of a BSD
system are much more heavily integrated, and this means that they can provide
much more reliable documentation; reliable from both the user's perspective
and from the perspective of technical correctness.

Aside from these and other quirks when installing BSD *on Libreboot*, the BSDs
otherwise work in exactly the same way as you would expect, and you can
follow along to their official documentation without much fuss.

No specific or detailed guides will be provided here, because SeaBIOS is
fairly self-explanatory; you can otherwise refer to the SeaBIOS
documentation.

DO NOT USE ROM IMAGES WITH `seabios_grubfirst` IN THE FILE NAME! These were
present in older Libreboot releases, and supported in previous revisions
of the build system, but they did not work for the intended purpose. More
info is written on the [Libreboot installation guide](../install/). ROM
images with `seabios_grubfirst` in the filename will NOT be included in
future Libreboot releases.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/bsd/>
