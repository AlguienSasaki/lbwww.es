---
title: Install Libreboot Open Source BIOS/UEFI boot firmware
x-toc-enable: true
...

Open source BIOS/UEFI boot firmware
------------------------------

**ALWAYS remember to make a backup of the current flash, when overwriting it,
regardless of what firmware you currently have and what firmware you're
re-flashing it with; this includes updates between Libreboot releases. Use
the `-r` option in flashprog instead `-w`, to read from the flash.**

These documents will teach you how to install Libreboot, on these devices/motherboards.
Libreboot is a [Free Software](https://writefreesoftware.org/learn) project
that replaces proprietary BIOS/UEFI firmware.

SAFETY WARNING!
---------------

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!!

If you're [building from source](../build/), you can ignore the above guidance.
Please take care to back up the current firmware, before flashing!

Chromebook install/update
----------------

Be careful as certain config in `RO_VPD` must be retained during installation
and updates. Check [chromebooks-x86.md](chromebooks-x86.md) for guidance,
cross-referenced with MrChromebook documentation.

PCI-E rebar
-----------

The EDK2-based images work with modern graphics cards, which often lack VGA
ROMs these days; EDK2 does GOP ROM execution.

We do not enable PCI-E relocatable BAR by default, because it caused stability
issues when libgfxinit is used on Intel integrated graphics.

To enable it, do:

```
./mk -m coreboot TARGET
```

Enable: Devices -> Extend resource window for PCIe devices above 4G

Enable: Devices -> Support PCIe Resizable BARs

Set that last one to the number of bits corresponding to your video RAM.
Two to the power of thirty-two is 4GB; thirty-three becomes 8GB. Work it out
accordingly. E.g. 16GB would be 33 bits. 39 bits is generally the maximum limit
though some machines might not be stable past 36-bit.

Where only graphics cards are supported, we would enable this by default. On
systems with integrated graphics, we turn it off by default.

SMMSTORE settings
---------------

Modern Libreboot releases use EDK2 with SMMSTORE.

If you used an EDK2 build before Libreboot 26.10, note that SMMSTORE was set
to 256KB. It was upped back to 512KB as is default, on most platforms. For
safety, please flash your entire image when switching to a stable release
with EDK2.

If you're installing for first time, please also flash the whole image.

If updating from an existing stable Libreboot release with EDK2, note that you
can avoid overwriting your EFI setup config by doing this:

```
flashprog -p internal --fmap -i COREBOOT -w whatever.rom
```

This avoids flashing the SMMSTORE section, where your settings are. This is
because the EDK2 payload writes settings to that section in the flash.

Conversely, you may reset it thus:

```
flashprog -p internal --fmap -i SMMSTORE -w whatever.rom
```

Be very careful with this. Note that fmap exists inside the BIOS region on
IFD-based systems.

Devices purchasable from [Minifree](https://minifree.org) directly;
-------------------------------------------------------------------

**Laptops and Desktops**

-   **[Lenovo ThinkPad X280](https://minifree.org/product/libreboot-x280.html)**

-   **[Lenovo ThinkPad T480 and ThinkPad T480s](https://minifree.org/product/libreboot-t480.html)**

-   **[Lenovo ThinkPad T580](https://minifree.org/product/libreboot-t580.html)**

-   **[Dell OptiPlex 3050 Micro](https://minifree.org/product/libreboot-3050-micro/)**

Devices supported by libreboot;
-----------------------

**If you want professional Libreboot installation,
an [installation service](https://minifree.org/product/installation-service.html)
is available; it also offers other coreboot firmware besides Libreboot. For
example, Chromebook installation via MrChromebox UEFI coreboot distribution is
also available.**

*Before internal flashing, [Disable /dev/mem protection](devmem.md) first.*

*In short add this kernel parameter: iomem=relaxed(linux distro)/kernel.securelevel=-1(bsd distro)*

**THEN REMOVE THIS KERNEL PARAMETER TO PREVENT UNAUTHORIZED/MALICIOUS ACCESS TO YOUR SYSTEM AFTER FLASHING LIBREBOOT.**

(An important reminder for those who might forget to delete it, even better, replace it with lockdown=integrity / lockdown=confidentiality)

### Laptops (x86\_64 chromebooks)

See: [x86 chromebooks](chromebooks-x86.html) - also pertains to EDK2 more
generally, across all boards, not just Chromebooks.

Libreboot 26.10 and higher have full integration of all EDK2-based MrChromebox
Chromebook setups. That is to say: we support every Chromebook that MrChromebox
does, in Libreboot.

### Laptops (x86\_64)

Intel

**8th Gen.**

-   [Lenovo ThinkPad X280](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-x280.html)**
-   [Lenovo ThinkPad T480 and ThinkPad T480s](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-t480.html)**
-   [Lenovo ThinkPad T580](t480.html) (Complex, DDR4) - Also **[available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-t580.html)**

NOTE: X270 also supported and installation is basically identical to X280.

**5th Gen.**

-   [HP EliteBook 820 G2](hp820g2.md) (Complex, DDR3L)

**4th Gen.**

-   [Lenovo ThinkPad W541](w541_external.md) (Complex, DDR3L)
-   [Lenovo ThinkPad T440p](t440p_external.md) (Complex, DDR3L)

**3rd -> 2nd Gen.**

(Socketed processors are fully compatible with motherboards from different generations)

-   [HP EliteBook 2170p](hp2170p.md) (Moderate, DDR3L)
-   [HP EliteBook 2570p/2560p](hp2560p.md) (Moderate, DDR3L)
-   [HP EliteBook 8470p/8460p](hp8460p.md) (Moderate, DDR3L)
-   [HP EliteBook Folio 9470m](hp9470m.md) (Complex, DDR3L)
-   Lenovo ThinkPad T430 (**but not the T430S**) (Complex, DDR3L)
-   Lenovo ThinkPad T530 (Complex, DDR3L)
-   [Lenovo Thinkpad X230 / X230 Tablet](x230_external.md) (Complex, DDR3L)
-   [HP EliteBook 8560w](hp8560w.md) (Moderate, DDR3L)
-   [Dell Latitude E5420, E5520, E5530, E6420, E6430, E6520, E6530, E6230, E6330, E6220, E7240](latitude.md) (Simple, DDR3L)
    (Intel GPU variants only)
-   [Lenovo Thinkpad T420/T520](t420_external.md) (Complex, DDR3L)
-   Lenovo ThinkPad T420s (Complex, DDR3L)
-   [Lenovo ThinkPad X220/X220](x230_external.md) (Complex, DDR3L)

**Core 2**

-   [Dell Latitude E4300, E6400, E6400 XFR and E6400 ATG, all with Nvidia or Intel GPU](latitude.md) (Simple, DDR2L + DDR3L for E4300)
-   [Lenovo ThinkPad R400](r400.md) (Complex, DDR3L)
-   Lenovo ThinkPad R500 (Complex, DDR3L)
-   [Lenovo ThinkPad T400 / T400S](t400.md) (Complex, DDR3L)
-   [Lenovo ThinkPad T500 / W500](t500.md) (Complex, DDR3L)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md) (Complex, DDR3L)
-   [Apple MacBook1,1 and MacBook2,1](macbook21.md) (Complex for 1.1/Simple for 2.1 - 2007, DDR2L)
-   [Lenovo ThinkPad T60](t60_unbrick.md) (with Intel GPU) (Complex, DDR2L)
-   [Lenovo ThinkPad X60](x60_unbrick.md) (with Intel GPU) (Complex, DDR2L)
-   Lenovo ThinkPad X60S (with Intel GPU) (Complex, DDR2L)
-   [Lenovo ThinkPad X60 Tablet](x60tablet_unbrick.md) Tablet (with Intel GPU) (Complex, DDR2L)

**ARM, with U-Boot payload**

-   [ASUS Chromebook Flip C101 (gru-bob)](chromebooks.md) (Complex)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](chromebooks.md) (Complex)


### Games consoles

-   [Sony Playstation](playstation.md) (Complex) (PS1/PSX)

### Servers (x86_64 with ECC support)

Intel

**9th -> 6th gen** (Xeon v5-6 and Core up to the 9th gen ones)

(Different gen. CPUs compatible with older and newer motherboards too, on older ones 8th-9th Core with B0 stepping; i3, Pentium Gold, Celeron. 6/8 core processors might work with pin-mod, untested.)

-   [Supermicro X11SSH-(LN4)F](supermicro_x11ssh_f_ln4f.md) (Moderate, DDR4)

AMD

**Opteron 6300**

-   [ASUS KGPE-D16 motherboard](kgpe-d16.md) (Moderate, DDR3)

**Opteron 4300/4200**

-   [ASUS KCMA-D8 motherboard](kcma-d8.md) (Moderate, DDR3)

**Opteron 2000/8000**

-   [ASUS KFSN4-DRE motherboard](kfsn4-dre.md) (Complex, DDR2)

### Desktops/WorkStations (x86/_64)

**12th Gen**

-   [Topton X2E N150 aka BKHD H30W-N150](x2e-n150.md) (Simple, DDR5)

**9th -> 6th gen** (Xeon v5-6 and Core up to the 9th gen ones)

(Different gen. CPUs compatible with older and newer motherboards too, on older ones 8th-9th Core with B0 stepping;
i3, Pentium Gold, Celeron. 6/8 core processors might work with pin-mod, untested.)

-   [Dell OptiPlex 3050 Micro](dell3050.html) (Simple, DDR4) - **Also [available to buy with Libreboot pre-installed](https://minifree.org/product/libreboot-3050-micro/)**
- [Dell OptiPlex 5050 SFF](dell5050.html) (Simple, DDR4)

NOTE: Dell OptiPlex 3040 also supported and installation is basically identical
to the 3050 Micro.

**5th -> 4th Gen.** (Core + Xeon v3-4)

(5th gen cpus are compatible with some the 4gen boards; C222/4/6 and H/Z97 chipsets 
https://en.wikipedia.org/wiki/LGA_1150#Haswell_chipsets )

-   [Dell OptiPlex 7020/9020/XE2/T1700 MT and SFF](dell9020.md) (Simple, DDR3 + ECC for T1700, Broadwell processors does not works with this motherboard yet)

**3rd -> 2nd Gen.** (Core + Xeon v1-2)

(Processors are fully compatible with motherboards from different generations)

-   [Dell OptiPlex 7010/9010 SFF/MT +T1650](dell7010.md) (Simple, DDR3 + ECC for T1650)
-   [HP Compaq Elite 8300 USDT](hp8300usdt.md) (Simple, DDR3)
-   [HP Compaq Elite 8200/Compaq 6200 Pro SFF/MT](hp8200sff.md) (Simple, DDR3)

**Core 2**

-   [Acer G43T-AM3](acer_g43t-am3.md) (Simple, DDR3)
-   [Dell OptiPlex 780 variants e.g. MT, USFF](dell780.md) (Simple, DDR3)
-   [Gigabyte GA-G41M-ES2L motherboard](ga-g41m-es2l.md) (Simple, DDR2)
-   [Apple Apple iMac 5,2](macbook21.md) (Simple, DDR2) (Same as Macbook 2.1 to flash)

**Bonnel (Atom)**
-   [Intel D945GCLF](d945gclf.md) (Moderate, DDR2)
-   Intel D510MO and D410PT motherboards (Moderate, DDR2)

### Emulation

-   [Qemu x86 and arm64](../misc/emulation.md)

More information about the Libreboot
build system can be found in the [lbmk maintenance manual](../maintain/),
and information about porting new systems in
the [porting manual](../maintain/porting.md).

Install Libreboot via external flashing
---------------------------------------

Refer to the following article:\
[Externally rewrite 25xx NOR flash via SPI protocol](spi.md)

You are strongly advised to *have* an external flashing setup, and make sure
it works, before attempting internal flashing. This, in addition to making
a backup of the current flash contents, prior to flashing, whether you dump
externally or internally - if only external flashing is available, then it's
usually the case that only external dumping is available too.

Trouble?
----------

Inquiries can be done via [Libreboot IRC](../../contact.md) and other channels.

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.


ROM image file names
--------------------

**Modern Libreboot standardises on EDK2 these days, not SeaBIOS/GRUB which are
provided inside EDK2 for backward compatibility but recommended only for
legacy use. Please use the `edk2_` images provided by modern Libreboot, where
possible as this is much more compatible with modern operating systems.**

Libreboot ROM images are named like
this: `payload_board_inittype_displaytype_keymap.rom`

The `payload` option can be EDK2, SeaBIOS, GRUB or U-Boot. U-Boot on ARM64
and EDK2 on x86; GRUB and SeaBIOS EFI builds provided for backward compatibility
with older releases, inside of the EDK2 build.

Inittype can be `libgfxinit`, `vgarom` or `normal`. The `libgfxinit` option
means coreboot provides native video initialisation, for onboard graphics.
The `vgarom` option means coreboot executes a VGA option ROM for video
initialisation. The `normal` option means coreboot provides no video
initialisation, via VGA ROM or native code.

Displaytype can be `txtmode` or `corebootfb` - if inittype is `normal`, this
is ignored because `txtmode` is assumed.

NOTE: modern releases also support `fspgop` which, semantics aside, works
essentially just like `libgfxinit_corebootfb`.

If `payload` is `seabios` instead of `seagrub`, no keymaps are inserted into
flash and only US QWERTY is assumed, otherwise the keymap refers to what is used
in GRUB on `seagrub` payload setups.

If you use a libgfxinit image on a desktop machine, you can still insert a
graphics card and it'll work just fine; its own VGA option ROM will be
executed instead, if the primary payload is SeaBIOS, whether that be pure
SeaBIOS or a SeaGRUB setup.

EC firmware updates
-------------------

Obviously, free EC firmware would be preferable, but it is not the case on
all machine. We would like to have free EC firmware on more machines, but for
now, we must rely on the vendor in a lot of cases. The EC is usually on a
separate flash, so you wouldn't think about it unless you knew it was there;
this is exactly why it's mentioned, so that you think about it,
[because proprietary software is bad](../../news/policy.md).

In many cases, the EC firmware must be updated on a separate IC to the main
boot flash, and this can usually only be done with the vendor's own tool,
running from the vendor boot firmware, and usually only on Windows, because
they provide EC and BIOS/UEFI updates in the same utility. Find out what you
need to do for your machine before installing Libreboot.

It is recommended that you update to the latest EC firmware version. The
[EC firmware](../../faq.md#ec-embedded-controller-firmware)

Updating the EC can sometimes provide benefit depending on the vendor. For
example, they might fix power issues that could then enhance battery life.

### ThinkPads

See: <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

Otherwise, check the Lenovo website to find the update utility for your
motherboard.

### HP EliteBooks

EC firmware is required in the main boot flash, but Libreboot's build system
automatically downloads this from HP for each machine, and inserts it, so
you don't have to worry. Just make sure that [vendor files are
inserted](ivy_has_common.md) if using release images.

Except for the hp820g2.

### Other

The same wisdom applies to other laptop vendors.

Non-laptops typically do not have embedded controllers in them.

Libreboot installation instructions
-----------------------------------

In general, if Libreboot is already running, you can skip
towards the final section on this page, which provides general internal
flashing instructions. Internal flashing is when you flash the target machine
from the target machine, inside an operating system running on it.

Some boards require special steps, even if Libreboot is already running,
for example if you [locked down the flash](../linux/grub_hardening.md), or
as another example, [SureStart on HP EliteBook 820 G2](hp820g2.md)

Therefore, before following generic guides, make sure to check first whether
your board has special instructions, otherwise use the generic guide at the
end of this article.

### Intel GbE MAC address (IFD-based systems)

On all Intel platforms except X4X (e.g. Gigabyte GA-G41M-ES2L) and i945
ones (e.g. ThinkPad X60, ThinkPad T60, MacBook2,1), an Intel Flash Descriptor is
used. If the board has Intel gigabit ethernet, the MAC address is included in
flash, and can (must) be changed prior to installation.

You can use [nvmutil](nvmutil.md) to change the MAC address. You will perform
this modification to the ROM image, before flashing it.

### Flash lockdown / boot security

This is referred to informally as *Secure libreBoot*.

Full flash lockdown is possible, with cryptographic verification of your
Linux kernel and other files, using special features in the GRUB payload.

See: [GRUB hardening / Secure libreBoot](../linux/grub_hardening.md)

If you already did this, it's possible that you may no longer be able to
flash internally. If that is the case, you must [flash externally](spi.md).

### Updating an existing installation

Unless otherwise stated, in sections pertaining to each motherboard below,
an existing Libreboot installation can be updated via internal flashing,
without any special steps; simply follow the general internal flashing
guide, in the final section further down this page.

If you have an existing Libreboot installation but you *locked down the flash*,
updating it will require external flashing.

If you currently have the factory firmware, you probably need to flash
externally; on *some* machines, internal flashing is possible, usually with
special steps required that differ from updating an existing installation.

The next sections will pertain to specific motherboards, where indicated,
followed by general internal flashing instructions where applicable.

### ThinkPad X200/T400/T500/W500/R400/R500

If you're running one of these with Lenovo BIOS, you must externally flash
Libreboot, because the original firmware restricts writes to the flash.

There machines all use SOIC8/SOIC16 flash ICs. Refer to pages specifically for
each machine:

* [ThinkPad X200](x200.md)
* [ThinkPad T400](t400.md)
* [ThinkPad R400](r400.md)
* [ThinkPad T500/W500](t500.md) (R500 is similar)

NOTE: T400S, X200S and X200 Tablet require different steps, because these have
WSON8 flash ICs on them, which will require some soldering. Please read
the [external flashing guide](spi.md) in the section pertaining to WSON.

You can find WSON8 probes online, that are similar to a SOIC8/SOIC16 clip. Your
mileage may very, but WSON8 has the same pinout as SOIC8 so you might have some
luck with that.

Hot-swap the flash IC with another one while it's running, and flash it
internally.

### ThinkPad X220/X220T/T420/T420s/T520

T420/T520: [T420 flash instructions](t420_external.md)

T420s/T520 are similar, in terms of assembly/disassembly.

Now, as for X220/X220 Tablet:

X220/X220 Tablet is essentially the same as the X230, but cannot be flashed
internally; you must use [an external flasher](spi.md).

Otherwise, look at [X230 disassembly](x230_external.md). Note that the X220
has a single 8MB flash, instead of 8MB and 4MB.

### ThinkPad X230/T430/T530/W530/X230T

Internal flashing from vendor BIOS to Libreboot is possible, but not
recommended. See: [IvyBridge ThinkPad internal flashing](ivy_internal.md).

External flashing recommended, but only documented for X230.
See: [ThinkPad X230 install guide](x230_external.md).

Otherwise, refer to [external SPI flashing](spi.md).

### ThinkPad T60/X60/X60Tablet/X60S

Only the Intel GPU is compatible. Do not flash the ATI GPU models.

External flashing guides:

* [ThinkPad X60](x60_unbrick.md)
* [ThinkPad X60 Tablet](x60tablet_unbrick.md)
* [ThinkPad T60](t60_unbrick.md)

These machines can also be flashed internally, by exploiting a bug
in the original Lenovo BIOS. If there's a BIOS password at boot, you should
just flash externally.

Internal flashing instructions:

First, please ensure that your CR2032/CMOS battery is working. This is what
powers the SRAM containing BIOS settings, and it powers the real-time clock.
It also holds the BUC.TS value - this is what we need.

BUC (Backup Control) register contains a bit called Top Swap (TS). The 64KB
bootblock at the top of flash is complemented by a backup Top Swap just above
it. The one at the end can't be flashed internally while Lenovo BIOS is running,
but the rest of it can be flashed (everything above the main bootblock).

By setting the TS bit, you can make the machine boot from the backup bootblock.

Download the Libreboot 20160907 utils archive, and in there you will find
these binaries:

* `flashrom`
* `flashrom_i945_sst`
* `flashrom_i945_mx`

You'll also find the bucts tool. Run it as root:

	./bucts 1

Now run both of these as root:
	
	./flashrom_i945_sst -p internal -w coreboot.rom
	./flashrom_i945_mx -p internal -w coreboot.rom

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

The main bootblock still isn't flashed, but you can shut down, wait a few
seconds and boot up again. When you do, you'll have Libreboot. Please make
sure to flash a second time, like so:
	
	flashprog -p internal -w coreboot.rom

Libreboot recommends `flashprog` now, which is a fork of flashrom, but we used
flashrom in the 2016 release. The macronix/ssh flashrom binaries there are
specifically patched; check the Libreboot 20160907 source code for the actual
patches. The patches modify some flash chip definitions in flashrom, to exploit
the bug in Lenovo BIOS enabling internal flashing.

You must ensure that the second flash is performed, upon reboot, because
otherwise if the CR2032 battery dies, bucts will be reset and it will no
longer boot.

When you've done the second flash, which includes overwriting the main
bootblock, set bucts back to zero:

	./bucts 0

The second flash can be done by simply following the general internal flashing
guide further down on this page.

### ARM-based Chromebooks

See: [Chromebook flashing instructions](chromebooks.md)

NOTE: The generic flashing instructions (later on this page) apply only to
the x86 machines, because the Chromebooks still use flashrom with
the `-p host` argument instead of `-p internal` when flashing, and you typically
need to flash externally, due to Google's security model.

### QEMU (arm64 and x86)

Libreboot can be used on QEMU (virtual machine), which is useful for debugging
payloads and generally trying out Libreboot, without requiring real hardware.

See: [Libreboot QEMU guide](../misc/emulation.md)

Install via host CPU (internal flashing)
----------------------------------------

NOTE: This mainly applies to the x86 machines.

Please check other sections listed above, to see if there is anything
pertaining to your motherboard. Internal flashing means that you boot Linux or
BSD on the target machine, and run `flashprog` there, flashing the machine
directly.

**If you can't flash internally, you must [flash externally](spi.md).**

Internal flashing is often unavailable with the factory firmware, but it is
usually possible when Libreboot is running (barring special circumstances).

### Run flashprog on host CPU

**When using release images, always remember to
[insert vendor files](ivy_has_common.md).  When building from source,
the vendor files are added automatically at build time (they are not
present in release images).**

#### Flash chip size

Use this to find out:

	flashprog -p internal

In the output will be information pertaining to your boot flash.

#### Howto: read/write/erase the boot flash

How to read the current chip contents:

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -r dump.bin

You should still make several dumps, even if you're flashing internally, to
ensure that you get the same checksums. Check each dump using `sha1sum`

How to erase and rewrite the chip contents:

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -w libreboot.rom

NOTE: `force_I_want_a_brick` is not scary. Do not be scared! This merely disables
the safety checks in flashprog. Flashprog and coreboot change a lot, over the years,
and sometimes it's necessary to use this option. If you're scared, then just
follow the above instructions, but remove that option. So, just use `-p internal`.
If that doesn't work, next try `-p internal:boardmismatch=force`. If that doesn't
work, try `-p internal:boardmismatch=force,laptop=force_I_want_a_brick`. So long
as you *ensure* you're using the correct ROM for your machine, it will be safe
to run flashprog. These extra options just disable the safety checks in flashprog.
There is nothing to worry about.

If successful, it will either say `VERIFIED` or it will say that the chip
contents are identical to the requested image.

NOTE: there are exceptions where the above is not possible. Read about them in
the sections below:

Removed boards
--------------

These boards were in Libreboot, but have been removed with the intention of
re-adding them at a later date. They were removed due to issues. List:

-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](c201.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](chromebooks.md)
-   [Hisense Chromebook C11 and more (veyron-jerry)](chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](chromebooks.md)
-   [HP Chromebook 14 G3 (nyan-blaze)](chromebooks.md)
-   [Samsung Chromebook 2 11" (peach-pit)](chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](chromebooks.md)
-   Lenovo ThinkPad X301 (still in lbmk, but with `release="n"`)

### NOTES about removed boards

**WARNING: veyron speedy boards (e.g. C201) have non-functional video init as
of 19 February 2023, and no fix is yet available on that date. See:
<https://notabug.org/libreboot/lbmk/issues/136> - the last tested revision
from 2021.01 is known to work, for u-boot on this board. See:\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
(alpernebbi on IRC is looking into this, to bisect uboot and update the latest
revisions) - for now, ROM images deleted from the Libreboot 20221214
and 20230319 releases.**

**WARNING: daisy- and peach- boards require a BL1 bootloader firmware, but the
one from coreboot 3rdparty is a fake/placeholder file. We need logic in the
Libreboot build system for properly fetching/extracting these, plus docs to
cover it. For now, assume that these are broken - ROM images are excluded,
for now, and have been deleted from the Libreboot 20221214 and 20230319
releases. - see: <https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e>
and <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

D945GCLF: Doesn't boot at all, according to last report. D510MO is still in
lbmk but still was reported problematic; other boards should be fine (see list
above).

WARNING: Support for ARM chromebooks is at a proof-of-concept stage. Refer
to [docs/uboot/](../uboot/) for more info about the U-Boot payload.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/>
