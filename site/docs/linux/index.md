---
title: Boot GNU and Linux on a Libreboot system
x-toc-enable: true
...

GNU boot loader "GRUB"
--------

This page is useful for those who wish to use the GNU boot loader called GRUB.
If you're using SeaBIOS, the boot process will work similarly to traditional
BIOS systems; refer to the SeaBIOS documentation
on <https://seabios.org/SeaBIOS>

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

The easiest way to use it is like this: in Linux, set up your partitions like
you would, but use LVM volume groups, with group name `grubcrypt` and either:

* `/` as volume name `rootvol` and `/boot` as volume name `bootvol`
* `/` as volume name `rootvol` and `/boot` exists within it (no `bootvol`)

If your distro then installs GRUB, and provides a `grub.cfg` file
under `/boot/grub` (within the distro, on your SSD/HDD file system), it should
work. Libreboot's GRUB will automatically give you a passphrase prompt, where
you type your passphrase and it unlocks the volume. Then it will find your
LVMs and it'll boot from that.

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

NOTE: You should also read the instructions about about `GRUB_TERMINAL`.

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
[fedora website](https://docs.fedoraproject.org/en-US/quick-docs/bootloading-with-grub2/#restoring-bootloader-using-live-disk)

### linux16 issue

Libreboot's default GRUB config sources fedora's grub config
`grub.cfg` (in `/boot/grub2/grub.cfg`), fedora by default makes use of the
`linux16` command, where it should be saying `linux`

Do this in fedora:

Open `/etc/grub.d/10_linux`

Set the `sixteenbit` variable to an empty string, then run:

	grub2-mkconfig -o /boot/grub2/grub.cfg

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
