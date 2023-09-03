---
title: ZFSbootmenu with Full Disk Encryption Guide
x-toc-enable: true
...

This article is obsolete!
=========================

**NOTE: [Encrypted /boot with LUKS2 on argon2 key derivation is now
possible](../../news/argon2.md) but not yet documented by this guide.**

zfsbootmenu is still nice, but no longer required. Use latest Libreboot and
boot encrypted /boot (with argon2) directly from GRUB if you want to. Guides
need to be written for this, and are not yet present on the Libreboot site.

Article
=======

As described in the [general encryption guide,](encryption.md) Libreboot allows for full disk encryption including the boot partition.
Just as with the general guide, this explanation will demonstrate how to create a partition with moderate encryption for GRUB as well as a root partition with strong encryption.
The major differences between the encryption method described in the general guide and this guide are:

+ `/boot` must remain on the *root* zfs encrypted partition
+ The root partition will be encrypted with ZFS native encryption rather than LUKS
+ ZFSbootmenu will be loaded at the second boot stage (after Libreboot itself) rather than directly loading the operating system kernel/initramfs

[ZFSbootmenu](https://docs.zfsbootmenu.org/en/latest/) works by placing modified versions of the operating system kernel where they can be loaded by the system's bootloader.
ZFSbootmenu provides installation guides for various major distros in their [official docs.](https://docs.zfsbootmenu.org/en/latest/)
You should follow those docs for installation, only noting the differences necessary for full disk encryption described below.
The only differences between this guide and the docs are:

+ You need not install/configure syslinux as GRUB in Libreboot will be used to load the ZFSbootmenu kernel/initramfs
+ The ZFSbootmenu kernel/initramfs will reside on a LUKS encrypted partition you will create in this guide
+ Cryptsetup must be installed and configured to mount the LUKS encrypted partition

## Creating Encrypted Partition for GRUB

The following section is mostly identical to the main encryption guide except for the naming conventions of the partition in question.
When using ZFSbootmenu, the OS kernel/initramfs will reside on the root partion in the `/boot` directory; **not** on a separate boot partition.
The partition created in this section is only used to load the ZFSbootmenu kernel/initramfs itself and is therefore referred to as the 'pre-boot environment' *(pbe)* partition.

**Step 1:**
Create a LUKS2 formatted device with the PBKDF2 algorithm.
You can play around with the iteration count.
A higher iteration is more secure but will take GRUB a **very** long time to decrypt.
The [debian encrypted boot guide](https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html) recommends a count of 500,000 which will still take GRUB a very long time (around 25 seconds) but is faster than the default 1,000,000.
Use whatever count makes you feel comfortable.
I'll use an arbitrarily low count.
You'll also want to use a different password than you intend to use for your root partition.
We don't want someone to be able to get our root key by brute-forcing our less secure boot key.

`sudo cryptsetup luksFormat /dev/sda1 --type luks2 --pbkdf pbkdf2 --pbkdf-force-iterations 200000`

**Step 2:**
Format and mount the new LUKS2 device.

```
sudo cryptsetup luksOpen /dev/sda1 pbe
sudo mkfs.ext4 -L boot /dev/mapper/pbe
sudo mkdir -p /boot/pbe
sudo mount /dev/mapper/boot /boot/pbe
```
**Note:**
If you wish to change the passphrase for the boot partition in the future then you'll need to pass the same arguments to cryptsetup as when you created it.
If you don't pass any special arguments, the key will be changed to the distro's default encryption and grub won't be able to decrypt it.
The command to use is:

`cryptsetup luksChangeKey /dev/sda1 --type luks2 --pbkdf pbkdf2 --pbkdf-force-iterations 200000`

## Configure ZFSbootmenu

The [official ZFSbootmenu docs](https://docs.zfsbootmenu.org/en/latest/guides/general.html) will provide the most up-to-date information.
The only differences from the official documentation relevant here are that anything related to syslinux can be ignored and the configuration must be tailored to create only a single kernel/initramfs set.
Note that you should follow the *MBR/syslinux* guide for your distro if you are using the ZFSbootmenu guides.

Here is an example configuration:

```
> vim /etc/zfsbootmenu/config.yaml

Global:
  ManageImages: true
  BootMountPoint: /boot/pbe
  DracutConfDir: /etc/zfsbootmenu/dracut.conf.d
  PreHooksDir: /etc/zfsbootmenu/generate-zbm.pre.d
  PostHooksDir: /etc/zfsbootmenu/generate-zbm.post.d
  InitCPIOConfig: /etc/zfsbootmenu/mkinitcpio.conf
Components:
  ImageDir: /boot/pbe/zfsbootmenu
  Versions: false
  Enabled: true
  syslinux:
    Config: /boot/syslinux/syslinux.cfg
    Enabled: false
EFI:
  ImageDir: /boot/pbe
  Versions: false
  Enabled: false
Kernel:
  CommandLine: ro quiet loglevel=4
```

## Final Steps

Refer to the [general guide](encryption.md) on how to set up fstab/crypttab to mount the pre-boot environment on boot.
Replace references to *boot* with *pbe* if copying commands from the guide.
For example: make sure the partition is mounted at `/boot/pbe` rather than just `/boot.`

Ensure that your OS kernel/initramfs is generated with LUKS support.
LUKS support is generally automatically enabled in the kernel upon installing *cryptsetup.*

Create a simulated grub configuration to point Libreboot's GRUB to ZFSbootmenu.
Libreboot will search for and source a grub configuration file on boot/decryption automatically.
**Do not** actually install GRUB.
Simply create a file on the partition created for GRUB at `/boot/pbe/grub/grub.cfg` which points to the ZFSbootmenu kernel/initramfs.

```
mkdir -p /boot/pbe/grub
> vim /boot/pbe/grub/grub.cfg

linux /zfsbootmenu/vmlinuz-* loglevel=4
initrd /zfsbootmenu/initramfs-*
boot
```
