---
title: Hardening GRUB
x-toc-enable: true
...

**NOTE: [Encrypted /boot with LUKS2 on argon2 key derivation is now
possible](../../news/argon2.md). This is covered in
the [main Linux guide](./).**

GRUB can have password protection at boot, prevent unauthorised access to the
shell and to menuentries. You can also boot from fully encrypted distros, where
the `/boot` directory is already encrypted, containing your kernel. You can
additionally verify each file, including GRUB configuration files and Linux
kernels, using GPG; GRUB supports putting a GPG pubkey in CBFS, and using it
to verify all files that it accesses.

Let's begin.

**Disable security before flashing**
================================

**Before internal flashing, you must first disable `/dev/mem` protections. Make
sure to re-enable them after you're finished.**

**See: [Disabling /dev/mem protection](../install/devmem.md)**

This only applies if you're following these instructions via internal
flashing, from an existing installation.

Back up your flash first!
=========================

Make sure you also back up the current flash contents, before you proceed with
this guide. See: [Libreboot flashing guides](../install/) (it also says how
to read the flash, in addition to writing it)

Build dependencies
==================

You need `cbfstool` from coreboot. For whatever board you have, check which
coreboot tree it uses in Libreboot's build system, lbmk. For example, let's
say your board is `x200_8mb`, you would do:

	grep tree= config/coreboot/x200_8mb/target.cfg

In this example, the output might be:

	tree="default"

This means you should compile `cbfstool` from the `default` coreboot tree,
like so:

	./mk -d coreboot default

This will result in the following binary: `elf/cbfstool/default/cbfstool`

We won't assume the path to cbfstool, in the remainder of this guide, so
adapt accordingly.

Background information
=========================

By default, the `grub.cfg` file and `grubtest.cfg` file are not present in
CBFS, because the GRUB memdisk, contained within the GRUB binary itself, within
CBFS, contains a GRUB configuration file.

Libreboot will switch to `grub.cfg` from flash instead, if it exists,
skipping the one in memdisk.

Because we need to put a signature next to each file, that would mean
re-building GRUB if you wanted to use the one in memdisk. Therefore, we can
insert a custom one in CBFS, to mitigate that fact.

By doing it this way, you can avoid re-building GRUB, or indeed anything inside
your current Libreboot images.

References:

* [GRUB manual](https://www.gnu.org/software/grub/manual/html_node/Security.html#Security)
* [GRUB info pages](http://git.savannah.gnu.org/cgit/grub.git/tree/docs/grub.texi)
* [Coreboot GRUB security howto](https://www.coreboot.org/GRUB2#Security)

GRUB Password
=============

The security of this setup depends on a good GRUB password as GPG signature
checking can be disabled through the interactive console:

	set check_signatures=no

Disabling signatures, using the above command, is useful when you're booting
regular media such as live distros via USB.

You are strongly advised to use an *open diceware* passphrase (look that up).
Generate a strong passphrase of completely random words, at least 20 words in
total is ideal. Passphrases are better than pass*words* containing lots
of random letters and symbols, because pass*phrases* have higher entropy and
are therefore harder to crack.

The GRUB password can be stored in one of two ways:

* plaintext
* protected with [PBKDF2](https://en.wikipedia.org/wiki/Pbkdf2)

We will *obviously* use the latter method. Generating the PBKDF2 derived key is
done using the `grub-mkpasswd-pbkdf2` utility. You can get it by
installing GRUB version 2. Generate a key by giving it a password:

NOTE: This utility is included under the `grub/` directory, when you build
GRUB using the libreboot build system. Run the following commands (assuming
you have the correct build dependencies installed) to build GRUB, from the
libreboot Git repository:

	./mk -b grub default

The following executable will then be available under `src/grub/default/`:

	grub-mkpasswd-pbkdf2

Run that program. It will ask you to choose a new passphrase. Its output will
be a string of the following form:

	grub.pbkdf2.sha512.10000.HEXDIGITS.MOREHEXDIGITS

Make sure to copy this into the correct GRUB config. The correct GRUB config
can be determined as follows. Again, let's assume that you have `x200_8mb`:

	grep grubtree= config/coreboot/x200_8mb/target.cfg

It *may* or *may not* output anything. If it outputs *nothing*, then the
GRUB tree is `default`, otherwise it might output something like:

	grubtree="nvme"

Make *sure* to use the correct GRUB tree. We will assume `default`, so you
should adapt accordingly, when doing this yourself:

	cp config/grub/default/config/payload grub.cfg

Now, your `grub.cfg` file is correct for the board, and you can insert
the salted, hashed passphrase that you get from `grub-mkpasswd-pbkdf2` earlier.

Now open my.grubtest.cfg and put the following before the menu entries
(prefered above the functions and after other directives). Of course use
the pbdkf string that you had generated yourself:

Put this *before* the menuentries (just before) in `grub.cfg`, but note that
you should **not** literally use what is below; the hash below is not the one
you generated yourself. Make sure to adapt accordingly.

Example:

	set superusers="root"
	password_pbkdf2 root grub.pbkdf2.sha512.10000.711F186347156BC105CD83A2ED7AF1EB971AA2B1EB2640172F34B0DEFFC97E654AF48E5F0C3B7622502B76458DA494270CC0EA6504411D676E6752FD1651E749.8DD11178EB8D1F633308FD8FCC64D0B243F949B9B99CCEADE2ECA11657A757D22025986B0FA116F1D5191E0A22677674C994EDBFADE62240E9D161688266A711

**Again**, replace it with the correct hash that you actually obtained for the
password you entered. In other words, *do not use the hash that you see above!*

Once this configuration is inserted, you will need to enter a passphrase every
time you boot. GRUB will also ask for a username. In the above example, we
made a username `root`, but you can set it to what you want and
adapt accordingly.

Another good thing to do, if we chose to load signed on-disk GRUB
configurations, is to remove (or comment out) `unset superusers`. Find any line
that says this, in your `grub.cfg` file:

	unset superusers

Change it to this:

	# unset superusers

The `unset superusers` command disables password authentication, which will
allow the attacker to boot an arbitrary operating system, regardless of
signature checking. The default libreboot configuration is tweaked for *ease of
use* by end users, and it is *not* done with security in mind (though security
is preferred). Thus, libreboot is less restrictive by default. What you are
doing, per this article, is making your system *more secure* but at the expense
of user-friendliness.

That just about covers it, where password setup is concerned!

SeaBIOS first?
==============

**Very important. Make sure you read this carefully.**

In releases after Libreboot 20240504, SeaBIOS is the primary payload on
all images, but GRUB is available in the boot menu. Select a ROM image
with `grubfirst` at the end, and do this to the ROM image:

	cbfstool libreboot.rom add-int -i 0 -n etc/show-boot-menu

This disables the SeaBIOS menu, so that it only loads GRUB. The `grubfirst`
image had this done to it by lbmk (Libreboot build system) during build:

	cbfstool libreboot.rom add -f config/grub/bootorder -n bootorder -t raw

This `bootorder` file has the following contents:

```
/rom@img/grub2
```

You can add it yourself if your image doesn't have it. With this, SeaBIOS
only loads GRUB first. You can still put a GRUB config in CBFS to override
the default one, as of Libreboot 20240612.

NOTE: Before disabling the boot menu, make sure GRUB works. Access it using
the `bootorder` file and/or press ESC in the SeaBIOS menu. Then disable the
SeaBIOS menu:

	cbfstool libreboot.rom add-int -i 0 -n etc/show-boot-menu

Although the `bootorder` file only specifies *GRUB*, this just means that
SeaBIOS won't automatically try to boot anything else. The SeaBIOS menu is still
accessible, by pressing ESC when prompted; the above `add-int` command disables
that menu, so that *only* the GRUB payload will be executed.

SeaBIOS option ROMs
===================

SeaBIOS will also still execute PCI option ROMs. Depending on your preference,
you may wish to disable this, but please note that this will break certain
things like graphics cards. More information is available here:

<https://www.seabios.org/Runtime_config>

On a laptop, you probably don't have to worry about option ROMs at all, but
desktops are much more easily upgradeable; though, in practise, anyone
inclined to insert a card with a malicious option ROM on it wouldn't do that
anyway, because if they have access to your hardware, they could just
externally re-flash the machine anyway, so I wouldn't worry, but it's up to you.

If you're using a graphics card, you *need* VGA option ROMs at least.

GPG keys
========

First, generate a GPG keypair to use for signing. Option RSA (sign only)
is ok.

WARNING: GRUB does not read ASCII armored keys. When attempting to
trust ... a key filename it will print `error: bad signature` on the screen.

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
-   `grubtest.cfg` (so that you can go back to `grubtest.cfg` after signature
    checking is enforced. You can always get back to `grub.cfg` by pressing ESC,
    but, afterwards, `grubtest.cfg` is not signed and it will not load. The
    GRUB config we made earlier can be copied to `grubtest.cfg` and inserted,
    but please only do this at the end of the guide when it tells you to insert
    the GRUB config, because there are still osme things you need to do.

Suppose that we have a pair of `my.kernel` and `my.initramfs` and an
on-disk `libreboot_grub.cfg`. We will sign them by running the following
commands:

```
gpg --homedir keys --detach-sign my.initramfs
gpg --homedir keys --detach-sign my.kernel
gpg --homedir keys --detach-sign libreboot_grub.cfg
gpg --homedir keys --detach-sign my.grubtest.cfg
```

Of course, some further modifications to grubtest.cfg will be required. We
need to *trust* the key and enable signature enforcement (put this before menu
entries):

```
trust (cbfsdisk)/boot.key
set check_signatures=enforce
```

What remains now is to include the modifications into the libreboot image
(ROM):

```
cbfstool libreboot.rom add -n boot.key -f boot.key -t raw

# You might consider copying `grub.cfg` to `grubtest.cfg` and adding that.
# Otherwise, adding just `grub.cfg` is also fine.

cbfstool libreboot.rom add -n grub.cfg -f my.grub.cfg -t raw
cbfstool libreboot.rom add -n grub.cfg.sig -f my.grub.cfg.sig -t raw

cbfstool libreboot.rom add -n grubtest.cfg -f my.grubtest.cfg -t raw
cbfstool libreboot.rom add -n grubtest.cfg.sig -f my.grubtest.cfg.sig -t raw
```

Congratulations!
================

With any luck, this should work perfectly. Your system will no longer boot
anything unless it's signed by your key.

If your entire system is also encrypted, including `/boot`, then that protects
everything including the privkey much more robustly, and it further prevents
tampering with your kernel (where GPG only detects tampering, encryption can
prevent it).

Encryption, combined with a GRUB passphrase, combined with a GPG check, should
make you pretty damn secure at boot time. All that's left now is one final,
optional step:

Flash write protection
======================

Although not strictly related to GNU GRUB, flash protection will prevent anyone
except you from overwriting the flash without permission. This is important,
because you don't want some malicious software running as root from overwriting
your flash, thus removing any of the above protections.

The simplest way is to just do this:

	ifdtool -x libreboot.rom -O libreboot.rom

If you did the step before, to compile `cbfstool`, you can find ifdtool in
the `elf/` directory, e.g. `elf/ifdtool/default/ifdtool`. Make sure to use
the correct version, as per `tree=` (same as before when deciding to which
cbfstool version to use based on the coreboot tree used by your board).

Note that this only works for Intel-based systems that use an Intel Flash
Descriptor, which is actually most Intel systems that Libreboot supports.

You can still flash externally, or strap `HDA_SDO` (`HDA_DOCK_EN` on older
GM45 machines) accordingly. Note that the x4x/ich10-based machines don't have
flash descriptors, and neither do the i945 machines, but the GM45 and newer
Intel platforms do.

Other facts
-----------

Strapping `HDA_SDO` or `HDA_DOCK_EN` requires physical access, because you have
to short a pin on the HDA chip on the motherboard, or there will be a header
for this on the board (e.g. "service mode" jumper).

On *Dell Latitude* laptops specifically, the EC can unlock flash by setting
the SDO/DOCK\_EN signal as described, and this is in fact what
the `dell-flash-unlock` utility does, so you can consider IFD locking there
to be basically useless.

In addition to the above, you may also consider `/dev/mem` protection.
Enable `CONFIG_STRICT_DEVMEM` in your Linux kernel, or set `securelevel` above
zero on your BSD setup (but BSD cannot be booted with GRUB very easily so
it's a moot point).

Other write-protect methods
---------------------------

The steps above do not require recompilation of the Libreboot images. However,
coreboot offers additional security at build time, which you can select if you
wish.

Let's assume your board is `x200_8mb`, do:

	./mk -m coreboot x200_8mb

Find this section: Security -> Boot media protection mechanism

In the above example, I found:

* Lock boot media using the controller
* Lock boot media using the chip

Which one to pick depends on your board. Let's pick "controller".

Now we can see: Security -> Boot media protected regions

In there, there is the option to ban writes, or to ban both reads and writes.
Banning reads may be desirable, for example if you have a salt hashed password
stored in `grub.cfg`! (as this guide told you to do)

You'll have to play around with this yourself. These options are not enabled
by default, because Libreboot images are supposed to allow writes by default,
when booted. You have to enable such security yourself, because the design of
Libreboot is to be as easy to use as possible by defalut, which include updates,
thus implying read-write flash permissions.

This example was for `x200_8mb`, but other boards may look different in config.
Anyway, when you're done, save the config and then build it from source in lbmk.

See: [build from source](../build/)

Install the new image
=====================

Now simply flash the new image, using
the [flashing instructions](../install/).

If you did all of the above steps correctly, your system should boot
up just fine. Shut it down and wait a few seconds. If you screwed it up
and the system is now unbootable, that's OK because you can use an
external flasher; please
read [external flashing instructions](../install/spi.md)
