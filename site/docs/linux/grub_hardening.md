---
title: Hardened GNU boot loader (GRUB payload)
x-toc-enable: true
...

**Modern Libreboot uses EDK2 by default now. EFI builds of GRUB are provided
in flash, accessible from the ESC boot menu in EDK2; set a password in EDK2
as well, setting GRUB as the only boot option in boot order menu. This
simulates old SeaGRUB-GRUB-only setups from previous guidance. EFI GRUB works
fine with this guide, and even reads from CBFS for example. We still recommend
use of GRUB for hardened setup, as in this guide, hence the decision to provide
EFI GRUB after making EDK2 the main payload. Our EFI GRUB build is modified to
behave superficially just like the old coreboot payload.**

What is Secure libreBoot?
-------------------------

Libreboot provides open source BIOS/UEFI firmware, replacing proprietary
BIOS/UEFI firmware, and that precisely is Libreboot's primary mission; Libreboot
is a [Free Software](https://writefreesoftware.org/learn) project, *first*.
Our *next* priority is to provide you with *highly secure* boot firmware, free
from backdoors and with well-audited code. *This* document does just that, by
telling you how to *harden* your Libreboot installation against various physical
access attacks.

NOTE: We also support SecureBoot now, if you set it up in the EDK2 payload that
we provide. It's all fairly straight-forward. This guide was written before we
had EDK2, and shall still be maintained for backward compatibility. You could
reproduce much of the same benefit as this guide, by using SecureBoot with a
Linux UKI-based setup (SecureBoot verifying your UKI bootloader). GRUB is still
recommended, as in this guide, as it offers true full disk encryption which
will prevent an attacker from doing anything other than merely corrupt your
data; even if a signing mechanism is cracked, use of encryption prevents
meaningful modification unless you have the keys for decryption, so use of
encrypted `/boot` is a useful extra layer of security, and GRUB does have
GPG support for verifying kernel signatures as shown in the guidance below.

Full disk encryption
--------------------

[Encrypted /boot with LUKS2 on argon2 key derivation is now
possible](../../news/argon2.md) - the work is based on that done by
Patrick Steinhardt, importing PHC's Argon2 implementation. This was later added
to GRUB 2.06 on the Arch Linux AUR by Ax333l, and Nicholas Johnson later rebased
it for GRUB 2.12 to use in Libreboot's GRUB payload.

This is covered in
the [main Linux guide](./#encrypted-boot-via-luks2-with-argon2), in the
section pertaining to LUKS2/argon2.

You are strongly advised to create an encrypted Linux installation, before
setting up GRUB as shown in the guide below. Adapt it for whichever distro
you're installing (documenting every distro on the Libreboot documentation
would be a futile exercise and will not be attempted).

You are advised to do this *first*, because steps below depend on certain
configuration changes to be made on your installed Linux distro.

**Dependencies (do this first)**
--------------------------------

**Please read this: [Modifying GRUB in CBFS](grub_cbfs.md)**

**Assimilate that knowledge before continuing with the instructions below,
which tells you what modifications to actually perform, whereas the guide
linked above tells you how to apply your modifications for flashing.**

Flash write protection
----------------------

Although not strictly related to GRUB, flash protection will prevent anyone
except you from overwriting the flash without permission. This is important,
because you don't want some malicious software running as root from overwriting
your flash, thus removing any of the above protections.

### Build-time write protect

For example:

	./mk -m coreboot lenovo_thinkpad_x200_8mb

Find this section: Security -> Boot media protection mechanism

In the above example, I found:

* Lock boot media using the controller (Advised, reversible/updateable with an spi programmer)
* Lock boot media using the chip (Do not try this, applies the flash chip's unreversible write-protection, only for extreme cases)

Let's pick "controller".

Now we can see: Security -> Boot media protected regions

In there, there is the option to ban writes, or to ban both reads and writes.
Banning reads may be desirable, for example if you have a salt hashed password
stored in `grub.cfg`! (as this guide told you to do)

You'll have to play around with this yourself. These options are not enabled
by default, because Libreboot images are supposed to allow writes by default,
when booted. You have to enable such security yourself, because the design of
Libreboot is to be as easy to use as possible by default, which include updates,
thus implying read-write flash permissions.

This example was for `x200_8mb`, but other boards may look different in config.
Anyway, when you're done, save the config and then build it from source in lbmk.

See: [build from source](../build/)

### IFD-based flash protection

**NOTE: This CAN cause bricks on a lot of machines. You should use this with
care. The FLILL and/or PRx based methods are more reliable - also SMM methods.
You can reconfigure coreboot and enable the chipset-based flash protection there
which accomplishes the same result, and the benefit is that it can't easily
be overridden by an evil maid attack e.g. can't simply set the service jumper
on desktops.**

The simplest way is to just do this:

	ifdtool --lock libreboot.rom -O libreboot.rom

If you did the step before, to compile `cbfstool`, you can find ifdtool in
the `elf/` directory, e.g. `elf/ifdtool/default/ifdtool`. Use the ifdtool
version matching the coreboot tree for your motherboard.

Note that this only works for Intel-based systems that use an Intel Flash
Descriptor, which is actually most Intel systems that Libreboot supports.

### Other facts

Strapping `HDA_SDO` or `HDA_DOCK_EN` requires physical access, because you have
to short a pin on the HDA chip on the motherboard, or there will be a header
for this on the board (e.g. "service mode" jumper). If you strap those pins,
it disables descriptor-based flash protections.

On *Dell Latitude* laptops specifically, the EC can unlock flash by setting
the SDO/DOCK\_EN signal as described, and this is in fact what
the `dell-flash-unlock` utility does, so you can consider IFD locking there
to be basically useless.

In addition to the above, you may also consider `/dev/mem` protection.
Enable `CONFIG_STRICT_DEVMEM` in your Linux kernel, or set `securelevel` above
zero on your BSD setup (but BSD cannot be booted with GRUB very easily so
it's a moot point).

### FLILL

On Intel Flash Descriptor, you can insert up to four (4) commands on a list
within, called *FLILL*; not yet documented, but any SPI command listed here
would no longer work during internal flash operations. For example, you could
use it to disable certain erase/write commands. You could also use it to
disable *reads*.

### PRx registers

Protected Range registers are available on Intel platforms, to disable flash
writes. This is not yet documented, and it varies per platform.

GRUB password
-------------

**The guidance below uses argon2 for password hashing. Please note that older
versions of this guide recommended PBKDF2 - users are advised that use of
argon2 offers superior protection against GPU-based bruteforce attacks due
to high memory costs. Please consider migrating to argon2, if you still
use PBKDF2.**

Please generally check [build instructions](../build/) as you will need to
compile GRUB. Compiling GRUB is required, so that you can use
the `grub-mkpasswd-argon2` utility - please make sure to use the latest version
of lbmk (from Git) for this. Get the build dependencies, as in the build guide.

Now, compile GRUB using lbmk as in the guide linked above:

	./mk -b grub

With this, you now have the requisite `grub-mkpasswd-argon2` utility located
at `elf/grub/default/default/noconfig/grub-mkpasswd-argon2` - needed for
setting a password in your GRUB configuration file.

Please note: the `grub-mkpasswd-pbkdf2` utility is also made available in
that directory, but you are advised to use the argon2 utility.

The security of this setup depends on a good GRUB password as GPG signature
checking can be disabled through the GRUB console with this command:

	set check_signatures=no

The above GRUB shell command is required when you want to live USB media,
or other Linux setups that don't yet have signatures on files e.g. linux.

We will assume that you're using the `default` GRUB tree; the GRUB CBFS guide
linked above tells you how to determine which GRUB tree to use.

The following executable will then be available under `src/grub/default/`:

	grub-mkpasswd-argon2

Run that program. It will ask you to choose a new passphrase. Its output will
be a string of the following form. The following example is what I got when
entering "password" as the password, in grub-mkpasswd-argon2:

	grub.argon2.5.131072.1.7FC84774FCD613915AA8643B82B2E356.8E159449557ADC7F3C0AB17E8BB23D64B266C75749ABD54E464332C98F7BF31A

Put this *before* the menuentries (just before) in `grub.cfg`, but note that
you should **not** literally use what is below; the hash below is not the one
you generated yourself. Make sure to adapt accordingly.

Example:

	set superusers="root"
	password_argon2 root grub.argon2.5.131072.1.7FC84774FCD613915AA8643B82B2E356.8E159449557ADC7F3C0AB17E8BB23D64B266C75749ABD54E464332C98F7BF31A

**Again**, replace it with the correct hash that you actually obtained for the
password you entered. In other words, *do not use the hash that you see above!*

If you *did* use the hash above, please note that it corresponds to
the following password: *password*

GRUB will also ask for a username in addition to the password; the "root" user
is specified above, but you can change it to whatever you want.

Unset superusers
----------------

Find this line in `grub.cfg`:

	unset superusers

Change it to this:

	# unset superusers

Commenting it, as shown above, ensures that password authentication works,
because `unset superusers` in fact disables password authentication, so it's
very important that you comment out this line.

Disable the SeaBIOS menu
------------------------

**Very important. Make sure you read this carefully.**

In releases after Libreboot 20240504, SeaBIOS is the primary payload on
all images, but GRUB is available in the boot menu.

Do this:

	cbfstool libreboot.rom add-int -i 0 -n etc/show-boot-menu

This disables the SeaBIOS menu, so that it only loads GRUB.

If your ROM image doesn't auto-start GRUB, you should also insert the
bootorder file:

	cbfstool libreboot.rom add -f config/grub/bootorder -n bootorder -t raw

This `bootorder` file has the following contents:

```
/rom@img/grub2
```

Release images with `seagrub` in the name already have this bootorder file,
so you only need to disable the menu on these images. If you have the
image with `seabios` in the name (instead of `seagrub`), you must do both.

### SeaBIOS option ROMs

SeaBIOS will also still execute PCI option ROMs. Depending on your preference,
you may wish to disable this, but please note that this will break certain
things like graphics cards. More information is available here:

<https://seabios.org/Runtime_config.html>

If you're using a graphics card, you *need* VGA option ROMs at least.

### GRUBSEA

Another option is to make it so that GRUB is the primary payload on your board.
In this setup, SeaBIOS and U-Boot are still available.

For your board's `target.cfg`, add this:

	payload_grubsea="y"

E.g. `config/module/coreboot/x200_8mb/target.cfg`

You should only do this on setups that have native graphics initialisation
e.g. Intel graphics (or FSP-based Intel graphics initialisation like on
Alderlake which sets up a similar framebuffer).

If you can't use GRUB as a primary payload, then you can use SeaGRUB as above
and disable the SeaBIOS menu, making SeaBIOS load only GRUB; SeaGRUB is useful
because GRUB will piggyback off of the VGA setup done by SeaBIOS first.

GPG keys
--------

First, generate a GPG keypair to use for signing. Option RSA (sign only)
is ok.

WARNING: GRUB does not read ASCII armored keys. When attempting to
trust ASCII armor keys, it will print `error: bad signature` on the screen.

```
mkdir --mode 0700 keys
gpg --homedir keys --gen-key
gpg --homedir keys --export-secret-keys --armor > boot.secret.key # backup
gpg --homedir keys --export > boot.key
```

Now that we have a key, we can sign some files with it. We must sign:

-   a kernel
-   (if we have one) an initramfs
-   (if we wish to transfer control to it) an on-disk `grub.cfg`
-   `grubtest.cfg` in CBFS, if it exists
-   `grub.cfg` in CBFS, if it exists

You must provide a *detached signature* alongside each file. For example, if
a file in a directory is named `foo`, and GRUB uses this file, an accompaning
file `foo.sig` must exist alongside it.

Suppose that we have a pair of `my.kernel`, `my.initramfs` and an
on-disk `grub.cfg`. We will sign them by running the following
commands:

```
gpg --homedir keys --detach-sign my.initramfs
gpg --homedir keys --detach-sign my.kernel
gpg --homedir keys --detach-sign grub.cfg
```

You must also do the above on any file that goes in CBFS, and insert it
into CBFS, using instructions already provided on the GRUB CBFS guide linked
above, earlier on in this guide.

Enforce GPG check in GRUB
-------------------------

The following must be present in `grub.cfg`, but please note that the
background image used by GRUB is in the memdisk by default, not CBFS, so you
might want to put it *after* the command that enables a background:

```
trust (cbfsdisk)/boot.key
set check_signatures=enforce
```

What remains now is to include the modifications into the libreboot image
(ROM):

Please read and follow the [GRUB configuration guide](grub_cbfs.md); this is
the GRUB CBFS guide that was also linked above, earlier on in the article
you're currently reading.

Install the new image
---------------------

Now simply flash the new image, using
the [flashing instructions](../install/).

If you did all of the above steps correctly, your system should boot
up just fine. Shut it down and wait a few seconds. If you screwed it up
and the system is now unbootable, that's OK because you can use an
external flasher; please
read [external flashing instructions](../install/spi.md)

Linux kernel hardening
----------------------

You may also wish to compile your own kernel, because distro kernels will always
have code in the same place, so attackers are more easily able to know exactly
where to attack your kernel (ROP-based attacks).

The Whonix/KickSecure Linux projects have guidance about Linux kernel
hardening:

* <https://www.kicksecure.com/wiki/System_Hardening_Checklist>
* <https://www.whonix.org/wiki/System_Hardening_Checklist>
* <https://www.kicksecure.com/wiki/Operating_System_Hardening>

There's info there about userspace too, but start with kernel first. Libreboot
is a boot firmware project, so Linux kernel hardening is beyond the scope of
the Libreboot project documentation, **for now**.

(for now, because Libreboot may in fact provide a Linux distro in the flash
at some point, and this page will definitely be updated when that happens)

References
----------

* GRUB manual: *https://www.gnu.org/software/grub/manual/html_node/Security.html#Security*
* GRUB info pages: *http://git.savannah.gnu.org/cgit/grub.git/tree/docs/grub.texi*
* Coreboot GRUB security howto: *https://www.coreboot.org/GRUB2#Security*


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/linux/grub_hardening.html>
