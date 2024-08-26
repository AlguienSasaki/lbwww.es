---
title: Internally flash Dell Latitude laptops
x-toc-enable: true
...

Make sure SecureBoot is disabled, if you're running a UEFI-based Latitude.
Other steps also required, described on this page and in the `README.md`
file included with `dell-flash-unlock`.

Internal flashing
=================

You can simply boot Linux/BSD, on the Dell Latitude you wish to flash, and
run `flashprog` from there, for Libreboot installation. Certain other steps
are also required, documented in the steps below:

Flashing from BSD
-----------------

Basically identical to Linux, except that you must
set `kern.securelevel` to -1 on boot, which is equivalent to `iomem=relaxed`
under Linux, for this purpose.

The `dell-flash-unlock` utility will compile and run just fine. It has been
tested on OpenBSD, NetBSD and FreeBSD. With this in mind, simply follow the
Linux instrucctions but adapt accordingly.

If unsure, just boot a live Linux distro via USB and run it all there.

Flashing from Linux
-------------------

MAKE SURE you boot with this Linux kernel parameter: `iomem=relaxed` - this
disables memory protections, permitting `/dev/mem` access needed by flashprog.
The flash is memory mapped and flashprog accesses it via `/dev/mem`.

You can flash Libreboot directly from the vendor (Dell) BIOS, without taking
the machine apart. It can be done entirely from Linux. It will probably also
work on BSD systems, but it has only been testing on Linux thus far.

**NOTE (15 October 2023): The util is now called `dell-flash-unlock`, but it
was previously called `e6400-flash-unlock`. Links have been updated.**

Check `util/dell-flash-unlock` in the `lbmk.git` repository, or in release
archives for Libreboot releases from 20230423 onwards.

Go in there:

	cd util/dell-flash-unlock
	make

**With this program, you can unlock the flash in such a way where everything
is writeable. Information about how to use it is in the `README.md` file which
is included in that program's directory, or you can read it online here:**

**<https://browse.libreboot.org/lbmk.git/plain/util/dell-flash-unlock/README.md>**

**Please make sure that you do fully read the README, because it contains
useful information.**

Literally just run that program, and do what it says. You run it once, and shut
down, and when you do, the system brings itself back up automatically; on some
systems, you have to boot the machine back up manually, after power down.  Then
you run it and flash it unlocked. Then you run it again. The source code is
intuitive enough that you can easily get the gist of it; it's writing some EC
commands and changing some chipset config bits. The EC on this machine is
hooked up to the `GPIO33` signal, sometimes called `HDA_DOCK_EN`, which sets
the flash descriptor override thus disabling any flash protection by the IFD.
It also bypasses the SMM BIOS lock protection by disabling SMIs, and Dell's
BIOS doesn't set any other type of protection either such as writing to
Protected Range registers.

MAKE SURE to back up the original firmware image first:

	flashprog -p internal -r factory.rom

When you flash it, you can use this command:

	flashprog -p internal -w libreboot.rom

Where `libreboot.rom` is your Dell Latitude ROM. *Make sure* it's the right
one. If flashprog complains about multiple flash chips detected, just pick one of them (doesn't matter which one). On *most* Dell machines, the most correct
would probably be this option in flashprog: `-c MX25L3205D/MX25L3208D`.

So:

	flashprog -p internal -w libreboot.rom -c MX25L3205D/MX25L3208D

When you see flashprog say `VERIFIED` at the end, that means the flash was
successful. If you don't see that, or you're unsure, please [contact the
Libreboot project via IRC](../../contact.md).
