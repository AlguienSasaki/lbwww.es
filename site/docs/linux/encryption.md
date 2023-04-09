# Fully Encrypted Boot and Root Partitions with Libreboot

The following guide will explain how to create:

+ A boot partition (/dev/sda1 in this example) that GRUB can decrypt with 'passphrase1'
+ A root partition (/dev/sda2) with stronger encryption using 'passphrase2'

This guide assumes you are working from a live disk of your preffered distro.

# Creating Encrypted Boot Partition

Grub2 currently (Oct 2021) supports luks2 encryption, which is great, but only the (not very strong) PBKDF2 algorithm.
Start by creating a boot partition of around 1GB, you don't have to format it to anything as LUKS will overwrite it anyway.

**Step 1:**
Create a LUKS2 formatted device with the PBKDF2 algorithm.
You can play around with the iteration count.
A higher iteration is more secure but will take GRUB a **very** long time to decrypt.
The [debian encrypted boot guide](https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html) recommends a count of 500,000 which will still take GRUB a very long time (around 25 seconds) but is faster than the default 1000,000.
Use whatever count makes you feel comfortable.
I'll use and arbitrarily low count.
You'll also want to use a different password than you intend to use for your root partition.
We don't want someone to be able to get our root key by brute-forcing our less secure boot key.

`sudo cryptsetup luksFormat /dev/sda1 --type luks2 --pbkdf pbkdf2 --pbkdf-force-iterations 200000`

**Step 2:**
Format and mount the new LUKS2 device.

```
sudo cryptsetup luksOpen /dev/sda1 boot
sudo mkfs.ext4 -L boot /dev/mapper/boot
sudo mount /dev/mapper/boot /boot
```
**Note:**
If you wish to change the passphrase for the boot partition in the future then you'll need to pass the same arguments to cryptsetup as when you created it.
If you don't pass any special arguments, the key will be changed to the distro's default encryption and grub won't be able to decrypt it.
The command to use is:

`cryptsetup luksChangeKey /dev/sda1 --type luks2 --pbkdf pbkdf2 --pbkdf-force-iterations 200000`

# Root Partition

Setting up the root partion is generally simple.
Use the same command without the given parametres used to make the device decryptable by GRUB.

`cryptsetup luksFormat /dev/sda2 root`

# Set Up Grub and Install

You will need to pass the correct kernel parametres to your kernel on boot to allow you to use your encryption passphrase to decrypt the root partition.
These parametres can be passed via a grub config in the boot partition by editing `/etc/default/grub.`

Add the necessary parametres to the line `GRUB_CMDLINE_LINUX_DEFAULT` as follows:

`GRUB_CMDLINE_LINUX_DEFAULT="loglevel=4 rd.auto=1 cryptdevice=/dev/sda2:root"`

*rd.auto=1* tells linux that you want to decrypt all disks.
*cryptdevice* tells linux the block device and mapped name you want to use for the root partition.
Note that the mapped name **must** match what you have it `/etc/fstab.`

From here, you can generally follow the install guide from your distro's docs.
Make sure that the generated `/boot/grub/grub.cfg` file indeed contains the necessary kernel parametres and that the `/etc/default/grub` file on the disk has the same modifications described above.

# Set up Fstab

> The device holding the kernel (and the initramfs image) is unlocked by GRUB, but the root device needs to be unlocked again at initramfs stage, regardless whether it’s the same device. This is because GRUB boots with the given vmlinuz and initramfs images, but there is currently no way to securely pass cryptographic material (or Device Mapper information) to the kernel. Hence the Device Mapper table is initially empty at initramfs stage; in other words, all devices are locked, and the root device needs to be unlocked again.
>
> \- [Debian Guide](https://cryptsetup-team.pages.debian.net/cryptsetup/encrypted-boot.html)

**Step 1:**
Here, we're not trying to store the root key as we don't want to jeopardize the integrity of our root device.
Instead, we want to store the key for the boot device on the root partition.

```
sudo mkdir -m0700 /etc/keys
su -c '( umask 0077 && dd if=/dev/urandom bs=1 count=64 of=/etc/keys/boot.key conv=excl,fsync )'
sudo cryptsetup luksAddKey /dev/sda1 /etc/keys/boot.key
```

**Step 2:**
Add your boot device to your crypttab.
You'll need to have the device's UUID.
You can obtain the UUID from `blkid` or simply use the linux block device name `/dev/sda1,` acknowleding it may lead to another device if your disk configuration changes.

```bash
lsblk -o 'PATH,LABEL,UUID' # to get UUID
sudo vim /etc/crypttab

> boot_crypt UUID=YOUR_UUID /etc/keys/boot.key luks,key-slot=1
```
**Step 3:**
Add the crypt device to your fstab.
Use 'mount -a' to test your fstab configuration.
NOTE: you will not be able to mount the device until it has been unlocked and mapped, rebooting with your new crypttab should do this automatically.

```
sudo vim /etc/fstab

> /dev/mapper/boot_crypt /boot ext4 defaults 0 1
sudo mount -a
```
