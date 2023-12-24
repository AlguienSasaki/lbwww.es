---
title: Jobs that need doing
x-toc-enable: true
...

This page contains a curated list of tasks that are to be worked on, or tasks
that are being worked on. This is intended to complement
the [issue pages](https://codeberg.org/libreboot/lbmk/issues/).

General auditing
================

Libreboot's build system design is already extremely efficient. See:
[lbmk build system documentation](../docs/maintain/)

One of the reasons for this is auditing. The build system is regularly audited.
In this context, that means reading the code to check for quality, pre-emptively
fix bugs and generally think about the design of the project. Smaller is better.

Code equals bugs, so less code yields fewer bugs. For a general idea of how
audits are done in Libreboot, see:

* [Libreboot build system audit 1](../news/audit.md)
* [Libreboot build system audit 2](../news/audit2.md)
* [Libreboot build system audit 3](../news/audit3.md)

Auditing can often be pedantic, and seem petty. You might commit a patch that
reduces the sloccount by only 1 line, maybe 3, but they all add up. Audit 3
contained hundreds of changes, small changes, that together accounted for
about 1000 lines of code removed, while not affecting functionality in any way.

Linuxboot
=========

See for inspiration: [Heads project](https://osresearch.net/)
and [Ownerboot project](https://sr.ht/~amjoseph/ownerboot/), these are other
coreboot distros similar to Libreboot, but they provide Linux-based payloads.
Also see more in general, the [Linuxboot](https://www.linuxboot.org/) project.

Libreboot's build system is documented, see: [lbmk
documentation](../docs/maintain/).

It's possible to provide a Linux system that runs from the flash. Linux can
execute another Linux kernel, using the `kexec` feature. There are bootloaders
that can make use of it, for example
the [u-root](https://github.com/u-root/u-root) project.

Libreboot's current choice of coreboot payloads are:

* SeaBIOS (x86 only), provides a traditional PC BIOS implementation
* GNU GRUB (x86 only), provides a multiboot implementation, can boot Linux and
  BSD. This is the preferred default payload on x86, especially for Linux
  distros, because it provides many security features like GPG signature
  checking on Linux kernels, and password protection.
* U-Boot (ARM only), provides several boot methods, we typically use the UEFI
  implementation but it also provides many different boot methods; the one that
  is most interesting is the SPL (secondary program loader) feature, which is
  essentially the same concept as loading a coreboot payload - together with
  something like the minimal [uefistub](https://review.coreboot.org/c/coreboot/+/78913)
  payload, can provide a complete setup.

U-Root in particular (not to be confused with U-boot has parsers in it for
GRUB and Syslinux config files. GRUB also has a parser for syslinux configs.
This makes it a useful drop-in replacement for the GNU GRUB payload that
Libreboot currently uses. Linux has much better drivers than GRUB, especially
for things like LUKS2 and networking.

Ideas for how to implement in lbmk
-----------------------------------

Look at the [lbmk documentation](../docs/maintain/) for context. The most
logical way to implement Linux payloads in Libreboot's build system, lbmk,
might be:

* Re-use the current crossgcc handling in `script/update/trees`, which is used
  for coreboot and u-boot. Coreboot's cross compiler isn't very useful for
  general applications e.g. utilities, but it could compile the Linux kernel
  easily.
* Separately to crossgcc,
  use [musl-cross-make](https://github.com/richfelker/musl-cross-make) for
  the programs inside initramfs. Use this to provide musl libc, busybox and
  all of the userland applications in general. Musl-cross-make itself would not
  be used as-is, but adapted and integrated into the lbmk build system.
  The design of musl-cross-make is largely compatible with that of lbmk, because
  both build systems are written in shell scripts and with the same minimalist
  mentality. 72 source lines! At least as of musl-cross-make git
  revision `fe915821b652a7fa37b34a596f47d8e20bc72338`.
* In each package defined under `config/git/` in lbmk, use the current design
  but support specifying, for each one, whether or not to use musl-cross-make.
  The current design in lbmk already permits use of make and cmake, for simple
  projects, otherwise for more complicated setups, a dedicated script is
  written, e.g. `script/build/grub` for building the grub images (which runs
  automake in the grub build system), or `script/build/roms` which builds
  rom images.
* A script, `script/build/linuxboot` would build the entire payload with u-root
  in it, but `script/update/trees` would actually build each package.

BONUS: the musl-cross-make logic could also be used to provide static linked
utilities, so as to provide compiled utilities in Libreboot releases, reliably.
We currenty only provide source code for utilities, which is not always
convenient for users, especially for utilities needed alongside vendor scripts.

If done in the way described above, the current code size in the Libreboot
build system would not increase much. It's mainly the addition of
musl-cross-make. Most of the generic build logic already exists in lbmk, for
projects that use cmake and/or make. It could be done with minimal complexity.

Flash size limitations
----------------------

With a stripped down kernel, and sensible configuration, about 6-8MB of flash
space would be required in this setup. The Heads setup is just under 8MB.

Why Linux in flash?
-------------------

Linux has better drivers than GRUB, has netboot, and it's much more practical
when you want to control the boot process. For example, you could more easily
implement measured boot and make use of TPM-based security mechanisms.

For the everyday user, it probably doesn't make much difference if they're
already happy with SeaBIOS, GRUB or SeaBIOS.

x86 implementation
------------------

Coreboot can directly execute it as a payload, but we would also execute it
from the GRUB payload - if running from the GRUB payload, we could just provide
it as a vmlinuz and initramfs file.

ARM implementation
------------------

We already standardise on U-Boot, for ARM machines. It's debateable whether
Linuxboot is even desirable here, U-Boot is quite competent, but the SPL mode
in U-Boot could be used to provide the Linux payload setup, OR:

See: [uefistub](https://review.coreboot.org/c/coreboot/+/78913)

Although currently only under review, not yet merged anywhere, uefistub seems
like a useful way to provide just the most minimal UEFI implementation, required
on Linux distros, but all it does it then boot a Linux payload. This is probably
what should be used, on ARM platforms, instead of U-Boot, if Linux is to be
provided in flash, but the uefistub will use a lot less space than U-Boot. That
being said, uefistub does not seem to provide a complete, or even fully
correct UEFI implementation.

(then again, linux on bare metal providing kexec as main bootloader method is
also quite non-standard, at least on x86 and ARM).

Disable Libgfxinit on DGPU setups
=================================

On machines where a discrete GPU is used, and no Intel GPU is present, but there
are other variants where an Intel GPU is present, it is possible to provide a
ROM image where:

1) Libgfxinit is enabled, for Intel graphics
2) SeaBIOS payload starts first, and can execute VGA ROMs from graphics cards
3) If a graphics card works, then that VGA ROM provides initialisation

When a dGPU is used, this can cause problems. For example, VGA-based
mode setting doesn't work properly on some machines, for some reason. For
example, on the Nvidia variants of Dell Latitude E6400, no Intel graphics
exists. For some reason, enabling libgfxinit makes `nomodeset` no longer work,
and VGA modes in various bootloaders e.g. syslinux/grub no longer work - 
whereas enabling just the SeaBIOS payload to load a VGA ROM, without loading
libgfxinit, works reliable.

Look at [lbmk build system documentation](../docs/maintain/) to know the
difference between libgfxinit/normal configs. Basically, we only enable
libgfxinit when available but we provide SeaBIOS as the default payload. If
a graphics card is used, SeaBIOS scans the VGA ROM from it or one can be
provided in CBFS.

We should keep doing what we're doing, but also provide configurations where
only the VGA ROM is used, on setups that use a discrete GPU. Libreboot's
preference is to use the native initialisation, but sometimes the VGA ROM has
be to be used instead.
