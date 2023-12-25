---
title: Jobs that need doing
x-toc-enable: true
...

This page contains a curated list of tasks that are to be worked on, or tasks
that are being worked on. This is intended to complement
the [issue pages](https://codeberg.org/libreboot/lbmk/issues/).

Many of these entries will pertain to *lbmk*, which is Libreboot's build
system, but some entries may relate to documentation, or organisational
changes.

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

Interesting board ports
=======================

Libreboot can support any board from coreboot, in principle. It would also be
feasible to integrate other (libre) boot firmware, if desirable. The list below
is not exhaustive, it just lists boards that are interesting to us at this time:

Boards
------

* HP EliteBook 2760p
* HP ProBook 6360b
* HP Revolve 810 G1
* HP EliteBook Folio 9480m
* HP EliteBook 8770w
* HP EliteBook 820 G2
* HP EliteBook 840 G2 (not in coreboot yet, but should be similar to 820 G2)
* HP Z220 CMI and SFF mainboards
* Dell OptiPlex 7010 and 9010
* Dell OptiPlex 7020 and 9020
* MSI PRO Z690-A mainboard (supported by Dasharo, not sure about coreboot) -
  also, Dasharo supports several more mainboards that aren't in coreboot
  proper.
* KGPE-D16 and KCMA-D8: use the Dasharo fork of coreboot, instead
  of coreboot `4.11_branch`, because Dasharo's version is much more up to
  date and more reliable with raminit. D8 isn't supported by Dasharo, but it's
  not much different code-wise to the D16 mainboard, so differences
  in coreboot `4.11_branch` could be adapted to provide a Dasharo port.

Blobless boards
-----------

Not yet supported, but interesting for the project. Separated thus:

already supported by coreboot:

* [ASUS P5Q mainboard](https://doc.coreboot.org/mainboard/asus/p5q.html) (ICH10 / i82801jx),
  known variants, e.g.: Pro, C, L-Pro, SE
* Scan coreboot code for ICH9/ICH10 systems, or boards with x4x/gm45 based
  northbridges. Many of these can boot blobless.

Dell Latitude/Precision:

* Dell Latitude laptops: E4200, E4300, E5400, E5500, E6500, Precision M4400,

These typically use MEC5045 or compatible EC. Some may use MEC5035.

SuperIO: at least M6500 is known to use ECE5028. I have a bunch of these
Dells at my lab, they are high priority for porting because they would be
easily flashable, and blob-free configs (Canoeboot could also support them).

Other Dells (Ivybridge and Sandybridge)
------------------------

Nicholas Chin is interested in these:

* E6520
* E6420
* E6320 and E6330
* E6220 and E6230
* E5520 and E5530
* E5420 and E5430
* 6430u
* E5520m
* E5420m

Most/all of these should be easily flashable, with the `dell-flash-unlock`
utility, and many could be ported using autoport as a guide. Nicholas is
working on these. They are left here for reference. If you have one of these,
please contact `nic3-14159` on the [Libreboot IRC channel](../contact.md).

ARM-based CrOS devices
----------------------

Alper Nebi Yasak ported several of these to Libreboot, but only
the `gru_bob` and `gru_kevin` machines are known to be stable.

It would be nice to re-add veyron-based platforms, e.g. `veyron_speedy` - old,
but still very useful.

The `nyan`, `peach` and `daisy` platforms were initially added to lbmk, but
prematurely. They are talked about here:

<https://libreboot.org/docs/hardware/#removed-boards>

It would be nice in general to support more ARM platforms in Libreboot. None
of these machines are as decent as the Apple silicon machines (m1/m2/m3 etc),
but they're still decent enough for most computing tasks (and the Apple machines
do not currently have coreboot support).

The actual coreboot code for these machines is thought to be reliable. The
problem is that the U-Boot port is not yet stable across all these machines.
Libreboot has Alper's proof of concept which works well
on `gru` chromebooks.

Caleb is interested in the `krane` chromebooks, but has had problems with vboot,
getting it to boot reliably on custom firmware builds.

OpenSIL and AMD Ryzen
---------------------

Of interest: coreboot has started imported AMD's *OpenSIL*, to support the
Ryzen-based chromebooks, and there is interest in adapting that code for
Ryzen-based desktops. AMD Ryzen CPUs are quite powerful, currently among the
best available at least on consumer-grade hardware.

AMD Family16 boards
-------------------

See: <https://review.coreboot.org/c/coreboot/+/71607>

This is part of a patch series, from 9 September 2023 onward, re-adding
AMD Family 16 platform to coreboot, most notably enabling use of the new
allocator and other things in coreboot.

The linked patch is for mainboard: HP T620 - of note, it may also be suitable
for Canoeboot. Worth looking into.

AMD AGESA-based platforms were removed from coreboot, because they weren't
being maintained anymore, so they were dropped. Some of those boards are
still quite decent today. Various efforts here and there have revived some
of them, e.g. the Dasharo project.

Also referenced there: Biostar A68N-5200 mainboard. Check
coreboot `4.18_branch` for these boards. Coreboot started removing the
AGESA boards after release 4.11.

Lenovo G505s
------------

Old board, removed from coreboot ages ago, but one of the fastest pre-PSP
AMD laptops, has full init in coreboot - it does require a VGA ROM for
graphics. Anyway:
<http://dangerousprototypes.com/docs/Lenovo_G505S_hacking>

This page was linked to me ages ago by Mike Banon. It contains instructions
for how to configure the machine. It might be worth integrating into lbmk.

UEFI payload
============

A UEFI payload in Libreboot is highly desirable, because it would basically
enable any distro or BSD to Just Work.

MrChromebox distribution
------------------------

MrChromebox is another coreboot distro, similar in spirit to Libreboot.

Of interest: Mrchromebox provides Tianocore-based UEFI setups on chromebooks,
and we could probably integrate some of that in Libreboot. Tianocore is
essentially bloatware, and really a liability for the Libreboot project due
to its complexity, though MrChromebox targets a very different audience.

U-Boot SPL and UEFI on x86
--------------------------

Simon Glass has been working extensively on x86 support for U-Boot, to be used
as a coreboot payload. This work is of interest to the Libreboot project,
because we provide UEFI on ARM but not on x86.

U-Boot also provides SPL which can be used to execute other software in the
flash, and it's often used to boot a Linux kernel; since U-Boot provides a
UEFI implementation, it's perfect.

U-Boot is the preferred choice of UEFI implementation on x86, for Libreboot
purposes, because U-Boot uses a coding style similar to Linux and can more
easily import Linux drivers which are high quality, and Linux functionality
in general, for anything that we need.

Since we already provide U-Boot on ARM (thanks to the continued work done by
Alper Nebi Yasak), U-Boot on x86 would then create a situation whereby Libreboot
is consistent across platforms, at least for UEFI-based setups.

uefistub
--------

Currently [under review](https://review.coreboot.org/c/coreboot/+/78913) in
the coreboot project, this provides an *incomplete* UEFI implementation, but
much more minimalist than the U-Boot one. It doesn't really *do* anything
except provide the most minimal code possible, and then you can jump to a
Linux payload in the flash.

For UEFI purposes, U-Boot seems more mature, and it offers other features
like SPL. As already stated, this is the preferred UEFI implementation for
Libreboot, but uefistub is listed too because it's interesting.

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

Seek QUBES endorsement
======================

Libreboot is compatible with Qubes, on several supported mainboards. This could
be audited, to provide a complete list. Qubes has a page on their website which
lists compatible devices.

It would be a nice way to promote the Libreboot project, and promote Qubes at
the same time, which is an excellent project. We could host a page specifically
for it, saying what works on our end, and basically copy that to their wiki.

GRUB VGA modes
==============

VGA support is not universal in Libreboot. We typically rely on GRUB to start
in console mode (`GRUB_TERMINAL=console`), which means GRUB won't change
modes, it'll just use whatever mode we started in.

We do not currently modify GRUB's video handling, so some distro setups will
try to use VGA modes, or some syslinux configs (that GRUB can parse) will,
causing weird behaviour on many Libreboot systems.

TODO: modify GRUB to only have behaviour matching `GRUB_TERMINAL=console`.
See: <https://www.gnu.org/software/grub/manual/grub/html_node/Simple-configuration.html>

This will prevent the need for modification. In some cases, it is necessary
to modify `GRUB_TERMINAL` in distro grub configs. The way Libreboot's GRUB
menu works is, it scans for GRUB and Syslinux/Extlinux configs on the user's
HDD/SSD, switching to the first one found.

GRUB configs menu
================

Libreboot systematically scans for GRUB/Syslinux/Extlinux configs provided by
the user's operating system, by scanning partitions. It can also scan
encrypted partitions (asking for the user to type their LUKS passphrase).

However, Libreboot switches to the first one found. In some cases, a user may
have multiple configurations.

TODO: Keep the current behaviour, for performance reasons, but offer a mode
where instead a new menu appears, with menuentries generated, where each one
just switches to one of the detected configurations.

This would enable Libreboot to work more seemlessly on dualboot setups, where
it is currently assumed that the user would modify `grub.cfg` in the flash.

This pertains to the GRUB *payload* provided in the flash, by Libreboot. It is
currently the preferred payload in Libreboot, at least for x86 machines.

Document flash write protection
============================

IFD-based method
----------------

Already covered, but could be documented more prominently.
Use `ifdtool --lock libreboot.rom` to lock the IFD.

This method is easily circumvented, by enabling the Flash Descriptor Override,
which varies from trivial to physically difficult depending on the board.

On some platforms, such as the Dell Latitude E6400, this method is entirely
useless; on the E6400, the EC firmware can be instructed to override the
IFD settings, by enabling the Flash Descriptor Override (in fact, this is part
of what the `dell-flash-unlock` utility does).

FLILL-based method
------------------

We already vaguely mention Intel Flash Descriptor settings ta enable
write protection. This documentation should be expanded on.

See:
<https://opensecuritytraining.info/IntroBIOS_files/Day2_02_Advanced%20x86%20-%20BIOS%20and%20SMM%20Internals%20-%20Flash%20Descriptor.pdf>

Actually, look at that site in general:

* <https://web.archive.org/web/20190104155418/http://opensecuritytraining.info/IntroBIOS.html>
* <https://opensecuritytraining.info/IntroBIOS.html>
* <https://p.ost2.fyi/courses/course-v1:OpenSecurityTraining2+Arch4001_x86-64_RVF+2021_v1/course/>

Anyway:

Universal across all currently known IFD versions, the FLILL section can be
used to define *invalid* opcodes when the flash is used, and this could be used
to define *write* and/or *erase* opcodes. Up to 4 can be defined.

This could be used to complement existing flash-based write protection. Of
particular interest is the fact that the FLILL config *cannot* be overridden.
Setting `HDA_SDO` (newer platforms) or `HDA_DOCK_EN` (GPIO33) to enable
Flash Descriptor Override, will not affect FLILL entries.

We could document this on the Libreboot website.

SMM write protection
--------------------

system management mode can also be used, to implement flash write protection.

PR (Protected Range) registers
------------------------------

Differing per platform but defined by Intel datasheets, the Protected Range
registers can be set, to enable flash write protection. Once written, these
cannot be changed until a reboot. Anything can set them.

This is the preferred method and should be the default (enabled by default),
because it can be done from GRUB. So, it could be provided on GRUB setups.

We could make it so that all menuentries in the default Libreboot GRUB menu
enable this, when possible on a given mainboard. The GRUB *shell* would not
enable it, and special menuentries that don't enable it could be provided (or
an entirely separate GRUB config, e.g. `grub_unprotected.cfg`).

With the PRx-based method, the user can easily circumvent it when they want to
update their firmware. Combined with a passphrase in GRUB, for menuentries
and the shell, this would prevent an unauthorised user from updating the
system; boot password alone cannot protect against malicious code in the user's
operating system, but this method would *require* a boot password.

It could also be done earlier, in coreboot, but then there's no way to turn
it off. Doing it from GRUB (or Linux, when a payload for that is added) seems
wiser.

In practise, this should probably not be the default. Libreboot's current
default is *no write protection*, though most Linux distros and BSDs enable
protecting `/dev/mem` by default, that the user can turn off at boot time when
they want to flash (e.g. cmdline option `iomem=relaxed` in Linux,
or `kern.securelevel=-1` in OpenBSD).

Chip-specific
-------------

Some flash chips support their own write protection scheme, covered in their
datasheets, but this is usually unreliable or inconsistent. This method is
not to be relied upon.

Layers!
-------

Security is all about layers. When you want to lock down the flash, use every
method available to you.

lbwww: Document MXM graphics
============================

MXM graphics modules are present, on some laptops that we do not yet support,
because certain functionality is needed on them that we do not implement yet.

See: <https://codeberg.org/libreboot/lbmk/issues/112>

Unlike on several other setups, many of these modules require certain data
tables to be present, provided by a BIOS interrupt, which the VGA ROMs then
use. These tables essentially contain config for things like ports, and power
management. More information, including links to PDF files containing the
specs for it, are provided for in the above linked issue page.

Several more high-end HP EliteBook machines use MXM graphics modules,
e.g. HP EliteBook 8560w.

John lane GRUB patches
======================

The GRUB project itself has started merging some of this functionality, for
things like detached LUKS headers. We had these for GRUB in Libreboot 2016
releases, but they were since removed.

Current functionality works on most setups, and we merge [Argon2
KDF support](../news/argon2.md) into GRUB, out-of-tree.

See: <https://grub.johnlane.ie/>

Interesting, the Arch Linux AUR has several of these patches for its
GRUB 2.06 package. See:
<https://aur.archlinux.org/packages/grub-luks-keyfile>

These could be re-based for GRUB 2.12, which is what Libreboot uses.

Use crossgcc for SeaBIOS and GRUB
=================================

We currently use hostcc for the SeaBIOS and GRUB payloads. This, among other
things, means lbmk is currently only supported for amd64 machines.

See other notes on this page about Linuxboot. When that work is done, we will
have better infrastructure for cross compilation, which could also be used for
this purpose.

In particular, GRUB's build system requires you to build certain utilities
first. We use `grub-mkstandalone` to then provide the coreboot payload. For
GRUB specifically, we should therefore use musl-cross-make. SeaBIOS can be
built using crossgcc.

Port lbmk to BSD systems
========================

In particular, FreeBSD is of interest.

We probably don't need to natively port it, because FreeBSD has Linux ABI
compatibility in its kernel, using *linuxlator*, and you can bootstrap a
Debian system under it.

See: <https://docs.freebsd.org/en/books/handbook/linuxemu/>

See: <https://docs.freebsd.org/en/books/handbook/linuxemu/#linuxemu-debootstrap>

We may still need certain build system modifications anyway, but this would
probably be mostly just documenting how to use lbmk that way.

FreeBSD specifically offers many advantages, such as really good OpenZfs
integration (better than ZFS-On-Linux setups), which it can do natively because
the licensing in BSD is compatible; Linux can't merge ZFS due to CDDL licensing.

An actual native port to FreeBSD is also feasible, and coreboot itself already
has some support for that, as does GRUB. If using crossgcc to build all payloads,
this could be even easier.

Building a Linux kernel might be slightly more challenging, but again: crossgcc.

Adapting musl-cross-make for use in FreeBSD could be interesting. Other notes
on this TODO page talk about using musl-cross-make to provide static linked
utilities in releases, but this has only Linux in mind. Doing them for
FreeBSD may also be desirable.

Libreboot already has excellent support for booting all of the BSDs. Having the
build system be compatible would just be another great boon.

Package lbmk in distros
=======================

Providing binaries of Libreboot in distros wouldn't make sense, because we
do that anyway, on Libreboot RSYNC, but having ports of the build system on
various Linux distros and BSDs might be desirable.

Distro package managers could check when changes are made to a given board,
and if the system you're on matches that given board, the package manager could
provide you with an option to *flash* it.

This would probably only be provided on systems where that is extremely safe,
specifically that those systems have been well-tested. Some ports in Libreboot
are a bit flaky and would require extra work.

It's unlikely that this job will ever be done, but it's on the TODO page
anyway. Distro package managers concern themselves with OS applications, kernel,
libc, bootloaders and so on; Libreboot is a step below them, earlier on in
the boot process.

But then again, there are things
like [fwupd](https://github.com/fwupd/fwupd) that provide firmware updates in
distros, so there's no reason Libreboot couldn't do something equivalent - we
could even do binaries, though I'm mostly thinking of the Libreboot build
system itself. A distro could package lbmk to build for a specific Libreboot
version, and handle all of the dependencies and everything.

Vendor scripts
==============

Bruteforce more files
---------------------

We bruteforce extract IME but some other firmwares are more or less
hardcoded in config.

In particular, VGA ROM extraction could be improved. We could modify
the `romheaders` utility to return zero status or non-zero status based on
a given PCI vendor/device ID; non-zero if it's not a match, for a given file,
or it isn't a VGA ROM. We currently extract an nvidia ROM for certain models
of Dell Latitude E6400, but the logic is more or less hardcoded.

The script at `script/vendor/download` auto-downloads vendor firmwares needed
on certain mainboards, during build time. Libreboot's build system
uses the script at `script/vendor/inject` to add or remove such files after
the fact, on release ROMs, because those firmwares are *deleted* at release
time. This work began mostly after mid-2022, and has since been expanded to
cover many types of firmwares, used on various mainboards.

Investigate 16MB flash setups
=============================

On some ivybridge and sandybridge boards, where flash is 8MB or 12MB,
it is feasible (with some soldering) to upgrade it to 16MB setups.

The IFD is configured accordingly, but some board modification besides
that may be required. For example, on the ThinkPad T440p, SPI2 is easily
accessible but SPI1 requires full disassembly. One could re-wire the board,
removing the Chip Select resistor for SPI1, and the SPI2 CS resistor, then
re-wiring CS1 to CS2 via a resistor, so that only SPI2 is used (thanks go
to Nicholas Chin for describing this idea) - then you stick one big 16MB flash
on SPI2, which is easily flashable.

These upgrades are really only recommended for advanced users. We do already
provide images for them; 16MB ROM images on many GM45 thinkpads, and also
the ThinkPad X230.

A 16MB setup was attempted on the ThinkPad T440p, but didn't boot, and I now
believe it was because I didn't insert the MRC firmware at the correct offset
during that test. Libreboot's build system now handles that correctly, in
the vendorfile inject script at `script/vendor/inject`.
