---
title: GNU+Linux guides
x-toc-enable: true
...

NOTE: This guide pertains to x86 hosts, and does not cover supported CrOS/ARM
chromebooks. For ARM targets, you should refer to u-boot documentation.

This page is useful for those who wish to use the GRUB GRUB payload directly.
If you're using SeaBIOS, the boot process will work similarly to traditional
BIOS systems; refer to the SeaBIOS documentation
on <https://seabios.org/SeaBIOS>

Linux is generally assumed, especially for Libreboot development, but Libreboot
also works quite nicely with [BSD systems](../bsd/).

Useful links
============

Refer to the following pages:

* [How to Prepare and Boot a USB Installer in libreboot Systems](grub_boot_installer.md)
* [Modifying the GRUB Configuration in libreboot Systems](grub_cbfs.md)
* [How to Harden Your GRUB Configuration, for Security](grub_hardening.md)

Encrypted (LUKS/dm-crypt) installations
=======================================

A better solution for encryption would be a Linux payload in flash, handling the
encryption, at least if you want to use Linux, because then it'll have
perfect LUKS support.

GRUB otherwise has good filesystem support, so if you have a valid `grub.cfg`
in `/boot/grub` on your installed system, Libreboot's GRUB configuration has
logic in it that will try to automatically use whatever you have installed,
by switching to it. In this way, most installations Just Work, so long as
the `/boot` partition is accessible.

Full encryption for basic LUKS2 is supported in libreboot.
See [the guide](encryption.md) for more detail.

[The ZFSbootmenu guide](zfsbootmenu.md) builds upon the main encryption guide but describes a setup with ZFS native encryption and ZFSbootmenu.

Rebooting system in case of freeze
===================================

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
==================

This may also apply to CentOS or Redhat. Chroot guide can be found on
[fedora website](https://docs.fedoraproject.org/en-US/quick-docs/bootloading-with-grub2/#restoring-bootloader-using-live-disk)

linux16 issue
-------------

Libreboot's default GRUB config sources fedora's grub config
`grub.cfg` (in `/boot/grub2/grub.cfg`), fedora by default makes use of the
`linux16` command, where it should be saying `linux`

Do this in fedora:

Open `/etc/grub.d/10_linux`

Set the `sixteenbit` variable to an empty string, then run:

	grub2-mkconfig -o /boot/grub2/grub.cfg

BLS issue
---------

With [newer versions of fedora](https://fedoraproject.org/wiki/Changes/BootLoaderSpecByDefault),
scripts from grub package default to generating [BLS](https://www.freedesktop.org/wiki/Specifications/BootLoaderSpec/)
instead of `grub.cfg`. To change that behaviour add following line
to `/etc/default/grub` (or modify existing one if it already exists):

	GRUB_ENABLE_BLSCFG=false

Then generate `grub.cfg` with:

	grub2-mkconfig -o /boot/grub2/grub.cfg
