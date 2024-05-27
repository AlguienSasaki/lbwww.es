---
title: Installation instructions
x-toc-enable: true
...

**GRUB payload warning**
====================

Firstly, it should be stated: in almost all cases, GRUB works just fine, on
all of the machines that we test, but as of 26 May 2024 we got the error
report:

See: <https://codeberg.org/libreboot/lbmk/issues/216>

Although we've only seen this thus far (as per user reports) on Intel
SandyBridge based Dell Latitude laptops, we advise:

**DO NOT use a ROM image where GRUB is the first payload. If you want to
use the GRUB payload, please use a ROM image with `seabios_` at the start
of the file name. Avoid images with `grub_` at the start of the file name.**

ROM images with `grubonly` in them should also be avoided; if you want GRUB
to be the first thing you see (without interruption), use a ROM image
with `seabios_` at the start of the file name, and `grubfirst` at the end;
these place a bootorder file in CBFS, so that SeaBIOS loads GRUB first, but
you can still press ESC to bring up the SeaBIOS boot select menu.

The error above, in the linked bug report, occurs on `grub_free()`
or `grub_realloc()` and could be caused by anything in GRUB; the user tested
on memtest86plus and got no errors, and SeaBIOS works; BIOS GRUB (from a distro)
also works, where GRUB doesn't need as many drivers; we suspect this may be
a use-after-free in a GRUB driver, or otherwise something writing to heap memory
wrongly. The bug is not yet known, only the symptom (the fatal error message).

Libreboot releases after 20240504 will *only* (on x86) contain ROM images where
SeaBIOS is the first payload, without disabling the SeaBIOS menu (no `grubonly`). You'll still be able to use GRUB, either by pressing ESC for the boot
select menu, and/or using an image with `grubfirst` in the file name so that
SeaBIOS loads it first (while still permitting boot select via ESC keypress).

GRUB's code is vast, and complicated, so this policy change is permanent,
until GRUB can be well-audited (likely forked, with dead/legacy code removed).
SeaBIOS code is much smaller and more robust. Remember always: code equals bugs.

Need help?
==========

Help is available on [Libreboot IRC](../../contact.md) and other channels.

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.

FLASH ERRORS (and workarounds)
=======================

**NOTE: Libreboot standardises on [flashprog](https://flashprog.org/wiki/Flashprog)
now, as of 27 January 2024, which is a fork of flashrom.**

This section relates to installing libreboot on supported targets.

Right out of the gate, some users may experience errors with flashprog when
using the internal programmer. They are:

/dev/mem access error
---------------------

NOTE: if running `flashprog -p internal` for software based flashing, and you
get an error related to `/dev/mem` access, you should reboot with
`iomem=relaxed` kernel parameter before running flashprog, or use a kernel that
has `CONFIG_STRICT_DEVMEM` not enabled.

On NetBSD and OpenBSD systems, the equivalent to `iomem=relaxed` in this case
is `kernel.securelevel=-1`; see [NetBSD securelevel
manual](https://wiki.netbsd.org/tutorials/kernel_secure_levels/)
and [OpenBSD securelevel manual](https://man.openbsd.org/securelevel).

ERROR: Could not get I/O privileges
------------------------------------

Error message: `ERROR: Could not get I/O privileges (Function not implemented)`

If you get this while running `flashprog -p internal -w filename.rom` (or any
internal flash operation), note: flashprog heavily uses ioperm/iopl functions
to operate the internal flasher, at least on x86 machines.

See: <https://lwn.net/Articles/804143/>

Yeah, just enable `CONFIG_X86_IOPL_IOPERM` in your Linux kernel. This is
a *build-time* option, so you must re-compile your kernel, or find a build that
has this option enabled (IOPL emulation). Many default kernel configurations
now disable this option.

Here's a handy-dandy guide for building a kernel from source:
<https://www.cyberciti.biz/tips/compiling-linux-kernel-26.html>

**BSD users:** On OpenBSD and NetBSD if you get similar errors, note that it
should work here, but you need to boot with `kern.securelevel=-1`.
See: [NetBSD securelevel manual](https://wiki.netbsd.org/tutorials/kernel_secure_levels/)
and [OpenBSD securelevel manpage](https://man.openbsd.org/securelevel).

Otherwise, if you get such errors, it may just be that you're not root. You
must run flashprog as root, at least to use the internal flasher (using external
USB flashing dongles doesn't normally require root).

SAFETY WARNING!
====================================================================

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

PRECAUTIONS
===========

libreboot flashing can be risky business. Please ensure that you have external
flashing equipment, in case anything goes wrong. The general rule of thumb with
firmware is this: if it's non-free, replace it, but if you're already running
free firmware and it works nicely for you, you do not need to update it.
However, you might want to tweak it or try out newer releases of libreboot if
they have bug fixes for your board, and/or new security fixes.

If you're already running libre firmware on your board, you should decide for
sure whether you wish to risk it. See changelogs on
the [release announcements via the news page](/news/) and decide for yourself.

Haswell/Ivybridge/Sandybridge machines
======================================

BLOBS MISSING IN RELEASE ROMs
-----------------------------

E.g. ThinkPad X220, X230, T440p, W541. - also desktop boards such as HP
Elite 8200 SFF.

The lbmk build system automatically fetches required vendor code for these
boards, when building, and sets them up properly, e.g. `me_cleaner`
is used. The same process is also available in a script, which can
insert them into ROM images.

**If you're using release ROMs, these files are missing, and must be
added. See: [ivy_has_common.md](ivy_has_common.md).**

About ROM image file names
==========================

Init types and display mode
---------------------------

NOTE: regardless of init type, on desktops, an external/add-on GPU can always
be used. On laptop hardware in libreboot, libgfxinit will always be used. On
desktop/server hardware, if available, libgfxinit will also always be used by
default (but in that setup, SeaBIOS can be used if you want to use an add-on
graphics card, e.g. on KCMA-D8, KGPE-D16, GA-G41M-ES2L)

**This means that on desktop hardware such as KCMA-D8, KGPE-D16, G43T-AM3,
GA-G41M-ES2L and others, you can use either the internal GPU or an add-on
PCI-E graphics card. Simply use a ROM image that starts with SeaBIOS, and you
can use both. On desktop/server hardware, libgfxinit simply means that you
CAN use the internal graphics chip, but you don't have to; external add-on
GPUs will also still work! However, if libgfxinit is enabled, that disables
coreboot from loading/executing PCI option ROMs which means you MUST use SeaBIOS
if you wish to use the add-on cards!**

### libgfxinit

In this setup, on supported systems, coreboot's own native video initialization
code is used. This is referred to generically as libgfxinit, which is coreboot's
library in `3rdparty/libgfxinit` but not all boards with native video
initialization use libgfxinit; some of them are using coreboot's older style
of video initialization method, written purely in C.

#### corebootfb (libgfxinit)

high resolution coreboot framebuffer used on startup

#### txtmode (libgfxinit)

int10h text mode is used on startup.

### vgarom

NOTE: no configs in libreboot are currently available that use this method.

With this method, coreboot is finding, loading and executing a VGA option ROM
for your graphics hardware. This would not be done on laptops, because that
implies needless supply of non-free software in libreboot, so this setup would only
ever be provided on desktop hardware where no GPU exists or where it is
desirable for you to use an external/add-on graphics card

#### vesafb (vgarom)

high resolution VESA framebuffer used on startup. This is equivalent
to `corebootfb` (high resolution framebuffer), but for setups where a VGA
Option ROM is used.

#### txtmode (vgarom)

int10h text mode is used on startup

### normal

NOTE: Dell Latitude E6400 Nvidia variant uses this.

int10h text mode startup is implied here. The `vesafb` mode is unavailable here.
For `vesafb` mode, please use init type `vgarom`; most useful for GRUB payloads
or perhaps Tianocore.

In this setup, coreboot is neither implementing libgfxinit / native graphics
initialization nor is it finding/loading/executing VGA option ROMs. In this
setup, SeaBIOS would most likely be used for that.

The `normal` setup is supported in the libreboot build system, but not
currently used. It is there for desktop hardware that will be added in the
future, where those desktop boards do not have an onboard GPU and therefore an
add-on GPU is always used..

Payload names
-------------

### grub

ROM images with just `grub` in the file name will start first with the GRUB
payload. They may or may not also provide other payloads in the menu, such as
memtest86+, SeaBIOS, Tianacore and so on.

### seabios

ROM images with just `seabios` in the file name will start first with the
SeaBIOS payload. They will only contain SeaBIOS, but may also contain memtest as
an option in the boot menu.

### seabios\_withgrub

ROM images that have `seabios_withgrub` in the file name start with SeaBIOS
first, but also have GRUB available in the boot menu when you press ESC.

ROM images with this and `grubonly` in the image start SeaBIOS, but only load
GRUB from SeaBIOS and the SeaBIOS menu is disabled. Use these images if you
only want GRUB; they are provided on systems that only have VGA ROM-based
initialisation, usually discrete graphics cards on desktop machines.

Which systems are supported?
============================

[Refer to the hardware compatibility page](../hardware/)

Intel GbE MAC address (IFD-based systems)
=====================================================================

You can change the MAC address in flash, on these machines. See:
[nvmutil documentation](nvmutil.md)

The MAC address is stored in a region of the boot flashed called *GbE NVM*
which is short for *gigabit ethernet non-volatile memory*. Refer to the
following article:

For GM45/ICH9M systems (e.g. ThinkPad X200/T400, Dell Latitude E6400), see:
[ich9utils documentation](ich9utils.md) (you can also use nvmutil, see link
above)

libreboot puts a default MAC address in the available ROM images, but this is
a generic MAC address and it's identical on every ROM image. Technically, you
can use it but if you encounter other libreboot users on the same ethernet
switch, using the same physical network as you, you will encounter a MAC
address conflict.

NOTE: R500 thinkpads do not have an Intel gigabit ethernet NIC, so on that
laptop you can just flash the default ROM and you do not have to worry.

There are also some Intel X4X platforms that use an ICH10 southbridge,
supported in libreboot, but these are flashed in a *descriptorless* setup,
which means that the MAC address is irrelevant (either there will be an Intel
PHY module that is now unusable, and you use an add-on card, or it doesn't use
an Intel PHY module and the onboard NIC is usable).

HP EliteBook 820 G2
===================

Please refer to the [820 G2 page](../hardware/hp820g2.md) for specific
flashing instructions, both internal and external.

HP EliteBook 8560w
===================

Please refer to the [8560w page](../hardware/hp8560w.md) for specific
flashing instructions, both internal and external.

Install via host CPU (internal flashing)
========================================

On all mainboards is a built-in programmer, which can read, erase and rewrite
the boot flash. However, it is not always usable by default. For example, it
may be configured to restrict write privileges by the host CPU.

In some situations, the host CPU can rewrite/erase/dump the boot flash.
This is called *internal flashing*. This means that you will run software,
namely `flashprog`, to read/erase/write the contents of the boot flash from a
running operating system on the target device.

NOTE: please also read the sections further down this page. On some systems,
external flashing is required. This means that you power the system down and
use a special tool that connects to and reprograms the boot flash.

NOTE: in some cases, external flashing is possible but special steps are
required. This depends on your mainboard. Again, please read this page
carefully.

Run flashprog on host CPU
------------------------

You can simply take any ROM image from the libreboot project, and flash it.
Boot a Linux distribution on the target device, and install flashprog.

In some cases, this is not possible or there are other considerations. Please
read this section *carefully*.

### Flash chip size

Use this to find out:

	flashprog -p internal

In the output will be information pertaining to your boot flash.

### Howto: read/write/erase the boot flash

How to read the current chip contents:

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -r dump.bin

You should still make several dumps, even if you're flashing internally, to
ensure that you get the same checksums. Check each dump using `sha1sum`

How to erase and rewrite the chip contents:

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -w libreboot.rom

If you are re-flashing a GM45+ICH9M laptop (e.g. ThinkPad X200/X200S/X200T,
T400, T500, R400, W500 etc - but not R500), you should run the ich9gen utility
to preserve your mac address.
Please read the ich9utils documentation:
[/docs/install/ich9utils.html](/docs/install/ich9utils.html)

NOTE: `force_I_want_a_brick` is not scary. Do not be scared! This merely disables
the safety checks in flashprog. Flashrom and coreboot change a lot, over the years,
and sometimes it's necessary to use this option. If you're scared, then just
follow the above instructions, but remove that option. So, just use `-p internal`.
If that doesn't work, next try `-p internal:boardmismatch=force`. If that doesn't
work, try `-p internal:boardmismatch=force,laptop=force_I_want_a_brick`. So long
as you *ensure* you're using the correct ROM for your machine, it will be safe
to run flashprog. These extra options just disable the safetyl checks in flashprog.
There is nothing to worry about.

If successful, it will either say `VERIFIED` or it will say that the chip
contents are identical to the requested image.

NOTE: there are exceptions where the above is not possible. Read about them in
the sections below:

### Exceptions

#### If your boot flash is currently write-protected

[You must flash it externally](spi.md)

#### DELL Latitude E6400 laptop

See: [Dell Latitude E6400 Libreboot Installation Guide](e6400.md)

#### DELL Latitude E6420 laptop

See: [Dell Latitude E6420 Libreboot Installation Guide](e6430.md)

#### DELL Latitude E6430 laptop

See: [Dell Latitude E6430 Libreboot Installation Guide](e6430.md)

#### DELL Latitude E6520 laptop

See: [Dell Latitude E6520 Libreboot Installation Guide](e6420.md)

#### DELL Latitude E6530 laptop

See: [Dell Latitude E6530 Libreboot Installation Guide](e6430.md)

#### DELL Latitude E5520 laptop

See: [Dell Latitude E5520 Libreboot Installation Guide](e5520.md)

#### DELL Latitude E5530 laptop

See: [Dell Latitude E5530 Libreboot Installation Guide](e6430.md)

#### ThinkPad X200/T400/T500/W500/R400/R500 vendor BIOS

If you're running one of these, it cannot be flashed internally if you're still
running the original Lenovo BIOS firmware.

[You must flash it externally](spi.md)

See notes further down on this page. We have guides for specific thinkpads,
related to disassembly and reassembly so that you can access the flash.

Please also see notes about the built-in MAC address inside the boot flash, for
the onboard NIC (ethernet one); not relevant on R500, which doesn't use an
Intel NIC.

TARGET: ThinkPad X201/X201S/X201 Tablet
--------------------------------

**SUPPORT WAS REMOVED, because fan controls fail when the ME is neutered on
this platform. This only affects the arrandale machines, and does not affect
newer Intel platforms.**

#### Intel D510MO and D410PT running original Intel BIOS

[You must flash it externally](spi.md)

D410PT is more or less the same board as D510MO, but we would like more info
about this board. If you have a D410PT mainboard, please contact the libreboot
project via IRC and ping `leah` before you flash it. When you do so, please
reference this paragraph on this web page.

#### Gigabyte GA-G41M-ES2l (any firmware)

Ignore this section. Internal flashing *is* possible, but there are two chips
and you must flash both chips. Refer to the guide:\
[Gigabyte GA-G41M-ES2L installation guide](ga-g41m-es2l.html)

#### Macbook1,1 running original Apple EFI firmware

This laptop requires external flashing. Remove the mainboard and refer to
the [external flashing guide](spi.md); if libreboot is already running, you
can flash internally.

MacBook2,1 can be flashed internally.

#### ASUS KFSN4-DRE?

Simply boot Linux with the default vendor firmware, and flash it internally,
but before you do: take a push pin, remove the metal pin, and superglue the
plastic part to the chip. Then remove the chip after you booting your
Linux system. Install a new chip, and flash *that*.

This board uses LPC flash in a PLCC32 socket. This coreboot page shows an
example of the push pin as a proof of concept:
<http://www.coreboot.org/Developer_Manual/Tools#Chip_removal_tools>

#### ASUS KGPE-D16 running original ASUS BIOS

[You must flash it externally](spi.md)

#### ASUS KCMA-D8 running original ASUS BIOS

[You must flash it externally](spi.md)

#### ASUS D945GCLF running original Intel BIOS

[You must flash it externally](spi.md)

#### ThinkPad X60/X60S/X60T/T60 with Lenovo BIOS {#flashrom_lenovobios}

**WARNING: Libreboot 20231021 and likely older 2023 releases do not have the
bootblock copied in release ROMs, so the bucts trick below will actually cause
a brick. This is fixed in lbmk as of 25 October 2023 onward, but you should
do this on 20231021 release ROMs before following the instructions below:**

	dd if=libreboot.rom of=bootblock.bin bs=1 skip=$(($(stat -c %s libreboot.rom) - 0x10000)) count=64k

And then do this:

	dd if=bootblock.bin of=libreboot.rom bs=1 seek=$(($(stat -c %s libreboot.rom) - 0x20000)) count=64k conv=notrunc

(This was fixed in Libreboot 20231101)

**NOTE: the section below pertaining to 20160907 static binaries references
flashrom. Libreboot recommends flashprog nowadays, but if you're using that
utils archive, please note that it is from a time when Libreboot used
flashrom. Use flashrom there as that's what included in those binaries.
Libreboot does not currently document how to patch flashprog for sst/macronix
on X60/T60, when going (in software) from lenovobios to libreboot.**

**NOTE: This section partially relates to `utils` release archive in
Libreboot 20160907, which contains static compiled binaries for things like
bucts and flashrom. It will *still* work on modern distros, and thus is
still referenced here. The `flash` script in that release can be used, with
modern Libreboot ROMs. Current Libreboot releases do not include pre-compiled
utilities, only ROMs.**

NOTE: If BIOS password auth is enabled, you can clear it by shorting pins on
an EEPROM and then resetting the password in Lenovo BIOS, prior to flashing
Libreboot. For T60, see:
<https://ounapuu.ee/posts/2022/10/13/recovering-password-locked-thinkpad-t60/>
(TODO: link something here for X60)

X60 BIOS password (Lenovo): you might find info here:
<https://bios-pw.org/>

You can just get bucts from the libreboot project, same thing for the patched
flashrom. In the Libreboot 20160907 release, there is a *utility* archive, which
has statically compiled executables. They still work just fine on modern
systems, and they can be used for this purpose.

Here are a list of targets:

* ThinkPad X60/X60S/X60T: flash the X60 ROM
* ThinkPad T60 with Intel GPU: flash the T60 ROM
* ThinkPad T60 with ATI GPU: flash the Headless T60 ROM (no video init, but you
  can get a serial console on the RS232 port if you use the Advanced Dock or
  Advanced Mini Dock. Connect to it from another machine, using null modem
  cable and USB serial adapter; *Screen* can connect to the serial console
  and you will run it at 115200 baud rate. agetty/fgetty in Linux can give
  you a serial console in your OS)

You can replace Lenovo BIOS with libreboot, using flashrom running on the host
CPU. However, there are some considerations. NOTE: needs patching for SST
and macronix chips, but libreboot doesn't yet do this for flashprog. You can
use the old Libreboot 20160907 sources to get the modified flashrom instead,
which contains this patch - and static binaries are provided, for convenience;
they will still work, due to libs being statically linked.

Firstly, make sure that the yellow CMOS battery is installed, and functioning
correctly. You could check the voltage. The battery is a CR2032
coin cell and it *should* be providing a 3V signal. You should check this while
it is connected to the board, because this will give a more accurate reading
(if the battery is weak, it will have severe voltage drop when there is any
load on it, which there will be. This coincell powers the real-time clock and
CMOS memory).

Lenovo BIOS restricts write access, but there is a weakness in it. With a
specially patched flashrom binary, you can easily flash it but the top 64KiB
region of the boot flash, containing your bootblock, cannot be flashed just
yet. However, there is a register called the *Backup Control* or *BUC* register
and in that register is a status bit called *Top Swap* or *TS*.

There are *2* bootblocks possible. The *other* bootblock is below the upper
64KiB one, which can't be flashed, but the lower one can. By using bucts, you
can set the machine to boot using that lower 64KiB bootblock, which is
read-write. You do this by setting the BUC.TS register to 1, using the `bucts`
program referenced below.

The libreboot ROM images already have the upper 64KiB bootblock copied to the lower
one, so you don't have to worry about copying it yourself.

If you use the Libreboot 20160907 utils archive, there will be three
binaries:

* `flashrom`
* `flashrom_i945_sst`
* `flashrom_i945_mx`

It's these last two binaries that you should use. Now compile bucts (just
run `make` in the bucts source directory).

Run the bucts tool:

	sudo ./bucts 1

Ensure that your CMOS battery is connected too. Now you must determine whether
you have Macronix or SST. An X60/T60 thinkpad will have either an SST or a
Macronix chip. The Macronix chip will have "MX" written on the chip. You will
use `flashrom_i945_sst` for the SST chip, and `flashrom_i945_mx` for the
Macronix chip.

Now run flashrom from the Libreboot 20160907 utils archive (for SST):

	sudo ./flashrom_i945_sst -p internal -w coreboot.rom

Or Macronix:

	sudo ./flashrom_i945_mx -p internal -w coreboot.rom

NOTE: you *can* just run both. One of them will succeed. It is perfectly
harmless to run both versions of flashrom. In fact, you should do so!

You'll see a lot of errors. This is normal. You should see something like:

```
Reading old flash chip contents... done.
Erasing and writing flash chip... spi_block_erase_20 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
spi_block_erase_52 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
Transaction error!
spi_block_erase_d8 failed during command execution at address 0x1f0000
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_60 failed during command execution
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_c7 failed during command execution
Looking for another erase function.
No usable erase functions left.
FAILED!
Uh oh. Erase/write failed. Checking if anything has changed.
Reading current flash chip contents... done.
Apparently at least some data has changed.
Your flash chip is in an unknown state.
```

If you see this, rejoice! It means that the flash was successful. Please do not
panic. Shut down now, and wait a few seconds, then turn back on again.

**WARNING: if flashrom (from Libreboot 20160907 utils) complains
about `/dev/mem` access, please
run `sudo ./bucts 0`. If flashrom is complaining about `/dev/mem`, it means
that you have `CONFIG_STRICT_DEVMEM` enabled in your kernel. Reboot with the
following kernel parameter added in your bootloader: `iomem=relaxed` and try
again with the above instructions. DO NOT continue until the above works, and
you see the expected flashrom output as indicated above.**

If you *did* run flashrom and it failed to flash, but you set bucts to 1 and
shut down, don't worry. Just remove the yellow coin-cell battery (it's underneath
the keyboard, connected to the mainboard), wait a minute or two, reconnect the
coin-cell and try again from scratch. In this instance, if flashrom didn't do
anything, and didn't flash anything, it means you still have Lenovo BIOS but
if bucts is set to 1, you can flush it and set it back to 0. BUC.TS is stored in
volatile memory, powered by that CR2032 coin-cell battery.

Assuming that everything went well:

Switch to flashprog now! (avoid flashrom)
---------------------------------------

Flash the ROM for a second time. For this second flashing attempt, the upper
64KiB bootblock is now read-write. Use the *unpatched* flashprog binary:

	sudo ./flashprog -p internal -w libreboot.rom

NOTE: At this point, we recommend use of flashprog instead of flashrom, for
the reasons mentioned in the [Libreboot 20240225
release](../../news/libreboot20240225.md).

To reset bucts, do this:

	sudo ./bucts 0

ONLY set bucts back to 0 if you're sure that the upper 64KiB bootblock is
flashed. It is flashed if flashprog said VERIFIED when running the above
command.

If it said VERIFIED, shut down. If it didn't say VERIFIED, make sure bucts is
still set to 1, and consult the libreboot project on IRC for advice, and avoid
shutting down your system until you get help.

If all went well, libreboot should now be booting and you should be able to
boot into your operating system.

If you messed up, there are external flashing instructions. See main navigation
menu on this page. These "external" instructions teach you how to flash
externally, using special equipment (requires disassembling your laptop and
removing the mainboard).

Install using external flashing equipment
=========================================

In many situations, the host CPU is restricted from rewriting/erasing/dumping
the boot flash. In this situations, you must re-flash the chip (containing the
boot firmware) externally. This is called *external flashing*.

DO NOT buy CH341A! Read the above link, which explains why you shouldn't use it.
CH341A will damage your flash chip, and other components on your mainboard.

How to use external flashing equipment
--------------------------------------

Refer to the following article:\
[Externally rewrite 25xx NOR flash via SPI protocol](spi.md)

DELL Latitude E6400 laptop (easy to flash, similar to X200/T400)
-------------------------

See: [Dell Latitude E6400 Libreboot Installation Instructions](e6400.md)

DELL Latitude E6430 laptop (easy to flash, no disassembly)
-------------------------

See: [Dell Latitude E6430 Libreboot Installation Instructions](e6430.md)

ASUS KFSN4-DRE
--------------

The KFSN4-DRE has an LPC chip.  Most people have been flashing these
internally, hot-swapping the chip out after boot, preserving the original chip,
and using flashprog on a new chip as described above.

TODO: Document PLCC32 (LPC) flashing.
The [FlexyICE](https://www.coreboot.org/FlexyICE) has been used to flash these
chips, but it is hard to find now.  A custom flasher may be made such as
[flashprog serprog stm32](https://github.com/wosk/stm32-vserprog-lpc) or
[teensy flasher](https://www.flashprog.org/Teensy_3.1_SPI_%2B_LPC/FWH_Flasher)

TARGET: DELL Precision T1650 desktop
------------------------------------

Refer to the [T1650 hardware page](../hardware/t1650.md).

TARGET: DELL OptiPlex 7020/9020 SFF and MT desktops
------------------------------------

Refer to the [7020/9020 hardware page](../hardware/dell9020.md).

TARGET: Apple Macbook2,1, Macbook1,1 and iMac5,2 (i945 platform)
----------------------------------------------------------------

iMac5,2 is essentially the same board as Macbook2,1, and it is compatible with
libreboot.

Refer to the following article:\
[Macbook2,1 and MacBook1,1 installation guide](../hardware/macbook21.md)

iMac5,2 isn't documented but you can find the flash chip on that board quite
easily. See the generic flashing guide:\
[Externally rewrite 25xx NOR flash via SPI protocol](spi.md)

TARGET: HP EliteBook laptops
----------------------------

Refer to the [hardware page](../hardware/) for info about HP laptops.

Links to specific HP laptop pages:

* [HP EliteBook 2170p](../hardware/hp2170p.md)
* [HP EliteBook 2560p](../hardware/hp2560p.md)
* [HP EliteBook 2570p](../hardware/hp2570p.md)
* [HP EliteBook 820 G2](../hardware/hp820g2.md)
* [HP EliteBook 8460p](../hardware/hp8460p.md)
* [HP EliteBook 8470p](../hardware/hp8470p.md)
* [HP EliteBook Folio 9470m](../hardware/hp9470m.md)

TARGET: HP Elite 8200 SFF
-------------------------

See: [HP Elite 8200 SFF hardware information](../hardware/hp8200sff.md)

TARGET: Gigabyte GA-G41M-ES2L mainboard
---------------------------------------

Refer to the following article:\
[Gigabyte GA-G41M-ES2L](ga-g41m-es2l.md)

TARGET: Intel D510MO and D410PT mainboards
------------------------------------------

Refer to the following article:\
[Intel D510MO and D410PT boards](d510mo.md)

TARGET: Intel D945GCLF mainboard
--------------------------------

Refer to the following article:\
[Intel D945GCLF](d945gclf.md)

TARGET: ASUS KGPE-D16 mainboard
-------------------------------

Refer to the following article:\
[ASUS KGPE-D16](kgpe-d16.md)

TARGET: ASUS KCMA-D8 mainboard
------------------------------

Refer to the following article:\
[ASUS KCMA-D8](../hardware/kcma-d8.md)

TARGET: ASUS Chromebook C201 laptop
----------------------------

Refer to the following article:\
[ASUS Chromebook C201](c201.md)

TARGET: Lenovo ThinkPad X60 laptop
----------------------------------

Refer to the following article:\
[ThinkPad X60](x60_unbrick.md)

TARGET: Lenovo ThinkPad X60 Tablet laptop
-----------------------------------------

Refer to the following article:\
[ThinkPad X60 Tablet](x60tablet_unbrick.md)

TARGET: Lenovo ThinkPad T60 laptop
----------------------------------

Refer to the following article:\
[ThinkPad T60](t60_unbrick.md)

TARGET: Lenovo ThinkPad X200 laptop
-----------------------------------

Refer to the following article:\
[ThinkPad X200](x200_external.md)

TARGET: Lenovo ThinkPad X200S or X200 Tablet laptop
---------------------------------------------------

Software-wise, identical to regular X200 but SMD rework skills are required.
You must de-solder the default flash chip, and replace it with another one.

Refer to the following article:\
[25xx NOR flashing guide](spi.md)

That guide, linked above, has instructions for how to deal with these machines.

TARGET: Lenovo ThinkPad T400 laptop
-----------------------------------

Refer to the following article:\
[ThinkPad T400](t400_external.md)

TARGET: Lenovo ThinkPad T400S laptop
------------------------------------

Software-wise, identical to regular T400 but SMD rework skills are required.
You must de-solder the default flash chip, and replace it with another one.

Refer to the following article:\
[25xx NOR flashing guide](spi.md)

TARGET: Lenovo ThinkPad R400 laptop
-----------------------------------

Refer to the following article:\
[ThinkPad R400](r400_external.md)

TARGET: Lenovo ThinkPad T500 or W500 laptop
-------------------------------------------

These two laptops have identical mainboard, except for a few minor changes.

Refer to the following article:\
[ThinkPad T500/W500](t500_external.md)

TARGET: Lenovo ThinkPad R500 laptop
-----------------------------------

Refer to the following laptop:\
[ThinkPad R500](../hardware/r500.md)

sandybridge/ivybridge/haswell
=================

**If using release ROMs, neutered ME must be inserted. Refer to the info
below.**

On currently supported HP laptops AND desktops
-------------------------------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

Refer to links about and the [hardware page](../hardware/) for installation
instructions on each HP mainboard.

TARGET: ThinkPad X220/T420/T420s
--------------------------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

Similar to X230 but there's only 1 flash. Intel ME image must be inserted
if using release ROMs. See: [guide](ivy_has_common.md) (says ivy/haswell but
the insert script for ME works with sandybridge aswell).

Refer to assembly/disassembly guide for T420:

[ThinkPad T420 external flashing](t420_external.md) (T420s is very similar)

X220/X220i: disassembly/reassembly very similar to X230. Please refer to
X230 instructions, but note: X220/X220i has *one* flash chip, not two.

**If using release ROMs, neutered ME must be inserted. Refer to the above
guide.**

TARGET: Thinkpad X230/T430/T530/W530
---------------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

NOTE: no install docs for T430/T530/W530 yet, but check coreboot wiki.

NOTE: Internal flashing is also possible, on this machine, from vendor firmware,
but it's still recommended to use a clip and a SPI flasher. However, follow
[internal X230 flashing from Lenovo firmware](ivy_internal.md) if you wish.

Refer to the [ivybridge/haswell common guide.](ivy_has_common.md) for how to
make the rom image usable for external flashing (with a clip). **If using
release ROMs, you must insert the neutered ME. Look at the info on that page.**

Read [board documentation](/docs/install/x230_external.html) for disassembly.

TARGET: Thinkpad X230t
---------------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

Refer to the [ivybridge/haswell common guide.](ivy_has_common.md) for how to
make the rom image usable for external flashing.

Read [board documentation](/docs/install/x230_external.html) for disassembly.
(same instructions as X230, for this purpose of external flashing, but
full disassembly will differ slightly)

**If using release ROMs, neutered ME must be inserted. Refer to the above
guide.**

TARGET: Thinkpad t440p/w541
---------------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](../../news/safety.md).**

Refer to the [ivybridge/haswell common guide.](ivy_has_common.md) for how to
make the rom image usable for external flashing.

Read [board documentation](/docs/install/t440p_external.html) for disassembly.

**If using release ROMs, neutered ME must be inserted. Refer to the above
guide.**
