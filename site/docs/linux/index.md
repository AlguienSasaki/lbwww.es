---
title: Boot GNU and Linux on a Libreboot system
x-toc-enable: true
...

Libreboot 26.10 and up now default to EDK2 on 64-bit-capable Intel/AMD
hardware. This makes booting and maintaining Linux/BSD systems much
easier. The first section of this page was added accordingly, and the
section about GRUB pertains to the *old* pre-EDK2 Libreboot design, which
is still accessible via GRUB EFI provided on Libreboot EDK2 releases, when
you press ESC to access the EDK2 boot menu.

EDK2 is Intel's reference implementation of the UEFI spec, and we use the
MrChromebox version of it.

EDK2 UEFI payload
--------

**Preferred boot method since Libreboot 26.10. Please refer to
the [Intel/AMD EDK2 documentation](../install/chromebooks-x86.md), written ostensibly
for Chromebooks but also applies to non-Chromebook mainboards; we also
provide a generic EDK2 build used on non-Chromebooks.**

EDK2 works pretty much like any normal UEFI firmware and should be fairly
self-explanatory. However, you can refer to [MrChromebox](https://docs.mrchromebox.tech/)
our upstream for EDK2, or Intel's [tianocore](https://www.tianocore.org/)
website for general information about EDK2.

EDK2 is the preferred boot method, booting directly via UEFI. You can use
modern EFI-based bootloaders e.g. systemd-boot, with full Linux UKI and you
could even enrol your own keys to use SecureBoot(not enabled by default).

Users who have existing Linux installations booting via SeaBIOS or GRUB can
use the GRUB EFI and SeaBIOS CSM compatibility modules. Press ESC in the
EDK menu to get the boot menu; you could also set the boot order in EDK2
to boot one of these first.

The next sections pertain to legacy setups. Please note however that the
hardened GRUB setup is still useful even in an EFI context (support for
encrypted /boot partition).

NOTE: When booting via GRUB EFI payload, Linux installers will still detect
that you're UEFI-based and will often default to UEFI-centric setup e.g.
systemd-boot and UKI turned on in Arch Linux `archinstall`. In this context,
of GRUB EFI booting, you are in fact still using UEFI but bypassing the use
of an EFI System Partition (ESP). This may actually be useful, because it
means that you could also have an *encrypted* ESP (LUKS-encrypted) and chain
load EFI executables from the EFI GRUB payload.

EFI GRUB in-flash is a Libreboot innovation, provided ostensibly for backward
compatibility but its use is going to be heavily expanded in future Libreboot
releases. It offers many advanced features otherwise normally only available
in e.g. Linuxboot setups, but with much lower flash space usage (Linuxboot
is also planned, for a future Libreboot release).

GNU boot loader "GRUB"
--------

This page is useful for those who wish to use the boot loader called GRUB.
If you're using SeaBIOS, the boot process will work similarly to traditional
BIOS systems; refer to the SeaBIOS documentation
on <https://seabios.org/Developer_Documentation.html>

Linux is generally assumed, especially for Libreboot development, but Libreboot
also works quite nicely with [BSD systems](../bsd/).

### Booting Linux from GRUB payload

NOTE: This guide pertains to x86 hosts, and does not cover supported CrOS/ARM
chromebooks. For ARM targets, you should refer
to [u-boot documentation](../uboot/) - separate [U-Boot x86
documentation](../uboot/uboot-x86.md) is also available.

### Useful links

Refer to these pages, hosted by the Libreboot project:

* [How to Prepare and Boot a USB Installer in libreboot Systems](grub_boot_installer.md)
* [Modifying the GRUB Configuration in libreboot Systems](grub_cbfs.md)
* [How to Harden Your GRUB Configuration, for Security](grub_hardening.md)

They will provide specific information; the information below pertains mostly
to troubleshooting, and there are some notes about Full Disk Encryption:

Encrypted /boot via LUKS2 with argon2
-------------------------------------

Full encryption for basic LUKS2 (with PBKDF or argon2 key derivation) is
supported in libreboot. Legacy LUKS1 is also supported. On *most* other
systems, `/boot` must be unencrypted, but Libreboot supports use of the
GRUB bootloader as a coreboot payload, directly in the boot flash.

GRUB has code in it that can be used to unlock LUKS1 and LUKS2 dm-crypt,
using the `cryptomount` command. With this, you can boot with *true* full
disk encryption, by encrypting `/boot`.

This is a boon for security, because it's harder
to tamper with, and you could potentially write-protect plus maybe provide
a [password](grub_hardening.md) in GRUB at boot time.

### Automatic LUKS/LVM boot

LVM is optional, but commonly used on LUKS setups. Libreboot's `grub.cfg`
automatically unlocks any encrypted block devices by running `cryptomount -a`
when you select the default menuentry.

Once unlocked, it will scan every unlocked partition, including logical volumes.

If a `grub.cfg` exists at a standard location, Libreboot's GRUB will pick it
up and switch to it automatically.

Standard locations? E.g. `/boot/grub/grub.cfg` where /boot is on the /
partition, or `/grub/grub.cfg` if it's a separate /boot partition. Libreboot
GRUB also detects `grub.cfg` in EFI system partition directories on common
distros, though the latter is irrelevant in this context; ESP is going to
be unencrypted anyway.

If you had a UEFI-based GRUB setup before Librebooting, or perhaps after (if
you installed it with e.g. U-Boot UEFI payload), it's best to convert it to
a BIOS-based setup (e.g. `/boot/grub/grub.cfg` location).

An easy way to set up encrypted `/boot` is to do unencrypted /boot first, and
then convert it. On a Debian system for example, let's say /boot was on a
separate partition to your encrypted rootfs. Well now, you could do this:

```
cp -R /boot /boot2
rm -Rf /boot/*
umount /boot
rmdir /boot
```

Then edit your `/etc/fstab`, commenting out the section for `/boot`, and now
rename `/boot2` to `/boot`, then re-generate `grub.cfg`; in Debian you can
do this:

```
export PATH="$PATH:/sbin"
update-grub
```

Or more generically, instead of `update-grub` which is a Debian thing:

```
grub-mkconfig -o /boot/grub/grub.cfg
```

The `update-grub` command in Debian is a script, that also runs `grub-mkconfig`
but does some other Debian-specific things. The `grub-mkconfig` program
reads several common configs e.g. `/etc/fstab` and `/etc/default/grub` to
guess locations e.g. where Linux is.

Be very careful to check the final `grub.cfg` at the end, to make sure that
it's configured correctly after the fact, or you could end up with a system
that doesn't boot afterward, requiring manual intervention; if that happened,
GRUB (Libreboot) is still likely to load your distro's GRUB config, and you
could press `E` on the entry that boots your kernel, and do some manual
editing, than press `F10` to temporarily boot with your changes, and fix it
up.

Otherwise, you could do encrypted /boot as part of an actual installation setup,
but note that some distros will prevent you. For example, the Debian installer
will not let you proceed when `/boot` is unencrypted, because it doesn't account
for this special capability of Libreboot.

### Manual booting

Otherwise, to manually unlock it, you drop to the GRUB shell with C and do:

	cryptomount -a

Or on a specific device, e.g.

	cryptomount (ahci0,1)

This is similar to `cryptsetup luksOpen` in Linux.

Libreboot GRUB merges the PHC argon2 implementation, so it has full support
for LUKS2 installations in addition to LUKS1. Libreboot 20231021 and higher
has argon2 support, but older releases only supported PBKDF2 which would make
LUKS2 dysfunctional unless you swapped it to use PBKDF2 (not argon2) and/or
downgraded to LUKS1.

With modern Libreboot, you can just use LUKS2 as-is, on most/all Linux distros.
At the time of the Libreboot 20231021 release, the GRUB upstream (on gnu.org)
did not have these argon2 patches in its source tree, but Libreboot merges and
maintains them out of tree.

### argon2id

You should *specifically* use argon2id. Please ensure this, because some
older LUKS2 setups defaulted to the weaker *argon2i*. This post by Matthew
Garret contains information about that:

<https://mjg59.dreamwidth.org/66429.html>

Libreboot's GRUB Argon2id implementation was created by Patrick Steinhardt,
who adapted PHC's Argon2 implementation for use in GRUB; Ax33l later added it
to Arch Linux AUR for GRUB 2.06, and Nicholas Johnson rebased *that* for
GRUB 2.12 so that Libreboot could use it.

NOTE: You should also read the instructions about `GRUB_TERMINAL`.

NOTE ABOUT VGA MODES and GRUB
-----------------------------

Libreboot does not support switching VGA modes, when coreboot's libgfxinit is
used on Intel GPUs. Many distros will install GRUB, which Libreboot then finds
and executes, if running SeaBIOS payload; if using GRUB, just the distro's
grub.cfg file is loaded instead, by Libreboot's own GRUB in flash.

Libreboot GRUB boots in text mode or uses the coreboot framebuffer. Anyway,
set `GRUB_TERMINAL=console` in GRUB and you should be fine. This avoids GRUB,
the one provided by your distro, switching video modes.

In Debian for example (steps largely the same on other distros):

Edit `/etc/default/grub` as root, and uncomment or add the line:

	GRUB_TERMINAL=console

Then still as root, do these commands:

	export PATH="$PATH:/sbin"
	update-grub

NOTE: `update-grub` is very much Debian-centric. Not all distros will have it.
On Arch-based distros for instance, you might do:

	grub-mkconfig -o /boot/grub/grub.cfg

Now your distro's GRUB menu should work, when your distro's GRUB bootloader is
executed from Libreboot's SeaBIOS payload.

Rebooting system in case of freeze
----------------------------------

Linux kernel has a feature to do actions to the system any time, even
with it freezes, this is called a
[Magic SysRq keys](https://en.wikipedia.org/wiki/Reisub). You can do these
actions with Alt + Sysrq + Command. These are the actions:

* Alt + SysRq + B: Reboot the system
* Alt + SysRq + I: Send SIGKILL to every process except PID 1
* Alt + SysRq + O: Shut off the system

If some of them don't work, you have to enable it in the kernel
command line paramter. So append `sysrq_always_enabled=1` to your
`GRUB_CMDLINE_LINUX_DEFAULT` in `/etc/default/grub`

You can also run `# sysctl kernel.sysrq=1` to enable them.

Fedora won't boot?
------------------

This may also apply to CentOS or Redhat. Chroot guide can be found on
[fedora website](https://docs.fedoraproject.org/en-US/quick-docs/grub2-bootloader/#_restoring_the_bootloader_using_the_live_disk)

### linux16 issue

Libreboot's default GRUB config sources fedora's grub config
`grub.cfg` (in `/boot/grub2/grub.cfg`), fedora by default makes use of the
`linux16` command, where it should be saying `linux`

Do this in fedora:

Open `/etc/grub.d/10_linux`

Set the `sixteenbit` variable to an empty string, then run:

	grub2-mkconfig -o /boot/grub2/grub.cfg

**NOTE: On newer Fedora versions, setting the sixteenbit variable is
no longer required and it is no longer present. However, you should still
deal with the BLS issue, mentioned below:**

### BLS issue

With [newer versions of fedora](https://fedoraproject.org/wiki/Changes/BootLoaderSpecByDefault),
scripts from grub package default to generating [BLS](https://www.freedesktop.org/wiki/Specifications/BootLoaderSpec/)
instead of `grub.cfg`. To change that behaviour add following line
to `/etc/default/grub` (or modify existing one if it already exists):

	GRUB_ENABLE_BLSCFG=false

Then generate `grub.cfg` with:

	grub2-mkconfig -o /boot/grub2/grub.cfg

These idiosyncrasies aside, Fedora is a great distro. It's well-tested with the
Libreboot build system, and it boots up just fine.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/linux/>
