% Censored Libreboot c20230710 released!
% Leah Rowe
% 10 July 2023

Warning
=======

**This release is *not* recommended for general use. You should still use the
recent [Libreboot 20230625](libreboot20230625.md) release, which is the
current stable release. Please also read the [Binary Blob Reduction
Policy](policy.md).**

The exact changes that created this Censored Libreboot release can be found here:

* lbmk (Libreboot build system): <https://codeberg.org/libreboot/lbmk/commits/branch/fsdg20230625>
* lbwww (Libreboot website): <https://codeberg.org/libreboot/lbwww/commits/branch/c20230710>

There is an entire version of the Libreboot site, made specifically for this
release: <https://censored.libreboot.org/>

Introduction
============

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on [ARM-based chromebooks](../docs/install/chromebooks.html) supported
(U-Boot bootloader, instead of Google's depthcharge bootloader). On x86
machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. It provides an [automated build system](../docs/maintain/) for the
[configuration](../docs/build/) and [installation](../docs/install/) of coreboot
ROM images, making coreboot easier to use for non-technical people. You can find
the [list of supported hardware](../docs/hardware/) in Libreboot documentation.

Libreboot's main benefit is *higher boot speed*,
[better](../docs/linux/encryption.md) 
[security](../docs/linux/grub_hardening.md) and more
customisation options compared to most proprietary firmware. As a
[libre](policy.md) software project, the code can be audited, and coreboot does
regularly audit code. The other main benefit is [*freedom* to study, adapt and
share the code](https://writefreesoftware.org/), a freedom denied by most boot
firmware, but not Libreboot! Booting Linux/BSD is also [well](../docs/linux/) 
[supported](../docs/bsd/).

Context
-------

Libreboot previously complied with [GNU FSDG](policy.md#problems-with-fsdg)
policy, banning (removing) all binary blobs from coreboot. Coreboot *requires*
binary blobs on a lot of boards, though it does provide something very close
to full freedom on a lot of them, so the old Libreboot policy resulted in very
weak hardware support.

Libreboot, in regular releases, adopted a more pragmatic [Binary Blob Reduction
Policy](policy.md) in November 2022, with the aim of providing support for a
lot more hardware (the goal is to support everything coreboot supports), while
reducing the impact (in terms of security and reliability) that certain binary
blobs have; for example, it automatically uses `me_cleaner` during build time,
to [disable Intel ME](https://github.com/corna/me_cleaner/wiki/How-does-it-work%3F)
after bringup, on newer Intel platforms that require Intel ME.

*This* new release, Censored Libreboot c20230710, released today 10 July 2023,
is a special spin-off of Libreboot based on the [20230625
release](libreboot20230625.md), provided as a proof of concept; it shows what
state the Libreboot project would likely be in, if it never adopted the new
[Binary Blob Reduction Policy](policy.md). A lot of mainboards and documentation
has been *removed* (censored), in this version, hence the name: *Censored
Libreboot*. More information available here:
<https://censored.libreboot.org/censorship.html>

You can find out about the current status of binary blobs, on the [Freedom
Status](../freedom-status.md) page. It describes how Libreboot policy is
implemented, in great detail.

A note about the changelog
--------------------------

There are going to be *two* changelogs written in this page: one in reference
to the recent [Libreboot 20230625 release](libreboot20230625.md), showing what
was removed (censored).

Then, after that, a  separate changelog will be provided in this article, in
reference to the [Libreboot 20220710 release](libreboot20220710.md), while
*ignoring* any changes since then that do not comply with the *old*
Libreboot policy, which you can read [here](https://web.archive.org/web/20221107235850/https://libreboot.org/news/policy.html).
In other words, this will be the *censored* changelog.

This release announcement is mirrored on the Censored Libreboot website, but
heavily censored to reflect only the latter changelog, written as though Libreboot
never changed its policies; in other words, it's a view into a *parallel
universe*, another reality. You can read that censored announcement here:

<https://censored.libreboot.org/news/censored-libreboot20230710.html>

Build from source
-----------------

*This* release was build-tested on Debian *Sid*, as of 9 July 2023. Your
mileage may vary, with other distros. Refer to Libreboot documentation.

KFSN4-DRE, KCMA-D8, KGPE-D16 re-added
-------------------------------------

FUN FACT: This includes building of ASUS KFSN4-DRE, KCMA-D8 and KGPE-D16
boards, which were re-added based on coreboot `4.11_branch`. ROM images are
provided for these boards, in this Libreboot release. The toolchain in
this coreboot version would not build on modern Linux, so I spent time patching
it. I want to use coreboot `4.11_branch` to study code differences between the
D8 and D16 boards, which are mostly otherwise identical code-wise, so that I
can port KCMA-D8 to Dasharo, and then use that for D8/D16 in Libreboot. Dasharo
is based on a much newer coreboot version, with many new fixes/features.

I won't be adding this release's D8/D16/DRE support to the `master` branch of
Libreboot, because coreboot `4.11_branch` is horribly out of date; I will add
these boards there, *after* I've integrated the Dasharo version of coreboot.

Uncensored changelog, relative to Libreboot 20230625
====================================================

**NOTE: this page lists *code changes* in Censored Libreboot. For *website* and
*documentation* changes, please read the following document:
<https://censored.libreboot.org/censorship.html>**

You can actually view the changes yourself, in great detail, by looking at
these special branches of `lbmk.git` (build system) and `lbwww.git` (Libreboot
website files, markdown):

* <https://codeberg.org/libreboot/lbmk/src/branch/fsdg20230625>
* <https://codeberg.org/libreboot/lbwww/src/branch/c20230710>

I've implemented this *Censored Libreboot* release, in these special branches.
These changes are not (and will not be) merged in the `master` branches.

Removed mainboard support
-------------------------

These mainboards are not supported in *Censored Libreboot*, and have
been removed (regular Libreboot *does* support them):

* HP EliteBook 2560p (laptop)
* HP EliteBook 2570p (laptop)
* HP 8200 SFF (desktop)
* HP 8300 USDT (desktop)
* HP EliteBook 9470m
* Lenovo ThinkPad T420
* Lenovo ThinkPad T420S
* Lenovo ThinkPad T430
* Lenovo ThinkPad T440p
* Lenovo ThinkPad T520
* Lenovo ThinkPad T530
* Lenovo ThinkPad W530
* Lenovo ThinkPad W541
* Lenovo ThinkPad X220

**All** of the above mainboards have fully libre, zero-blob initialisation
code available in coreboot, and that code *is used* by Libreboot. However,
the flash is divided into regions (partitions), namely: IFD(config), GBE(config),
ME(Intel ME firmware), BIOS(coreboot firmware).

The Ifd/GbE regions are not software, and their format is well-documented.
Libreboot even includes utilities that can re-configure them!

The ME is configured via `me_cleaner`, automatically by Libreboot's build
system, in such a way that the Intel ME initialises itself, and then does
*nothing*. In other words, it is *disabled*. More information about all of this
is explained in the [Freedom Status](../freedom-status.md) page.

Removed/modified code, in the build system
-------------------------------------------

Here is an overview of the code changes in lbmk:

* **coreboot and u-boot download scripts:** Binary blobs are now removed during
  download. A list of blobs is programmed into the build system, based on
  scanning of each tree with the linux-libre `deblob-check` script. (yes, it
  works on other code bases, besides Linux). **This means that most mainboards
  no longer compile, in coreboot, and many u-boot targets no longer compile.**
* **`build/boot/roms`:** These scripts build ROM images. For **zero-blob boards**,
  in other words boards that do not require binary blobs, *regular* Libreboot
  inserts **CPU microcode** by default, but copies each ROM to produce a
  corresponding, parallel zero-blobs version **without** CPU microcode. **This**
  censored version of Libreboot modifies the script in the following way: since
  the coreboot and uboot download scripts **remove blobs** anyway, including CPU
  microcode, the default compiled ROMs exclude microcode. Therefore, *this*
  version simply removes that logic, because it's not needed.
* **`blobutil`:** Anything pertaining to [blobutil](../docs/install/ivy_has_common.md)
  has been removed. This includes `me_cleaner`, `ME7 Update Parser` and the like.
  It is not needed, in this version of Libreboot. Directories such
  as `resources/blobs/` (containing code and config data) has been removed.
  In regular Libreboot, there are certain required binary blobs that we cannot
  legally distribute on certain mainboards, so `blobutil` auto-downloads them
  from the vendor while compiling ROM images, then it processes them (if needed)
  and inserts them; the scripts that produce release archives will *delete*
  these blobs, for the release, and those same scripts can be re-run on release
  ROMs, to re-insert binary blobs. It is *completely automated*, removing any
  need for manual intervention by the user, thus saving hours of time in some
  cases. Blobutil snaps them up like *that* and everything *Just Works*.
  It does this for *many* different types of blobs, including: Intel ME, Intel
  MRC, HP KBC1126 EC firmware, VGA ROMs - you just run 1 command on 1 ROM (or
  an entire collection of ROMs) and it does it, automatically detecting what
  is needed for the given ROM image, per mainboard definition. Very easy to use.
  This *highly innovative* technology does not exist in Censored Libreboot.
* Blobs: Removed Intel Flash Descriptors and GbE configuration files. These are
  non-copyrightable, non-software blobs, just binary-encoded config. They are
  not needed, in this Libreboot version.
* Blobs: Anything downloaded and inserted by `blobutil`, during the build
  process or [post-release](../docs/install/ivy_has_common.md). This includes:
  Intel ME firmware, Intel MRC firmware, HP KBC1126 EC firmware and VGA option
  ROM for Nvidia GPU variant of Dell Latitude E6400.
* `lbmk`: Code that executes `blobutil` has been removed.
* Patches: Any custom coreboot patches, for mainboards that require binary
  blobs, have been removed. They are not needed in this Libreboot version.
* `build/release/roms` and `build/release/src`: correspondingly deleted files
  are no longer copied by these scripts (they are the scripts that generate
  tar archives for Libreboot releases, after everything is compiled). The `roms`
  script no longer bothers to scrub non-redistributable inserted binary blobs
  from certain ROM images, because 1) those corresponding mainboards are no
  longer supported anyway and 2) the logic for downloading/inserting those
  blobs no longer exists. So there's nothing to do.

It's not actually a lot of code that was removed. The actual diff that did this
is very large, because it also removed the coreboot configs for the removed
boards, and those configs are very large. The diff is about 40,000 deleted
lines. **Fourty thousand.**

Censored changelog, relative to Libreboot 20220710
==================================================

Libreboot 20220710 was the *last* regular Libreboot release to comply
with the old *Binary Blob Extermination Policy* adhering to GNU FSDG
ideology. Between then and now, there have been these releases of Libreboot
that follow the new *Binary Blob Reduction Policy*: [20221214](libreboot20221214.md),
[20230319](libreboot20230319.md), [20230413](libreboot20230413.md),
[20230423](libreboot20230423.md) and [20230625](libreboot20230625.md).

However, the purpose of *Censored Libreboot* is to provide a glimpse of what
Libreboot would be like, had it kept the old policy. The website for Censored
Libreboot has its *own* version of this release announcement, with only this
censored version of the changelog present. You can view that here:

<https://censored.libreboot.org/news/censored-libreboot20230710.html>

The following changelogs cherry-pick only the old-policy-compliant changes
from the above listed Libreboot release announcements:

New mainboards supported
------------------------

These laptops would have been compatible with Libreboot, under the old
policy, and they were added in recent regular releases of Libreboot:

* [Dell Latitude E6400](../docs/hardware/e6400.md)
* [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
* [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Build system changes
--------------------

This is not intended to be an exhaustive list. It is a high-level overview. For
more details, you should always check the log in `lbmk.git`.

*All* of these changes are present in regular Libreboot releases, but these
are the changes from regular Libreboot that would have complied with the *old*
Libreboot policy:

* [MASSIVE build system audit](audit.md) - the entire build system was
  re-written in a much cleaner coding style, with much stricter error handling
  and clear separation of logic. A *lot* of bugs were fixed. A *LOT* of bugs.
  Build system auditing has been the *main* focus, in these past 12 months.
* `cros`: Disable coreboot-related BL31 features. This fixes poweroff on gru
  chromebooks. Patch courtesy of Alper Nebi Yasak.
* `u-boot`: Increase EFI variable buffer size. This fixes an error where
  Debian's signed shim allocates too many EFI variables to fit in the space
  provided, breaking the boot process in Debian. Patch courtesy Alper Nebi Yasak
* Coreboot build system: don't warn about no-payload configuration. Libreboot
  compiles ROM images *without* using coreboot's payload support, instead it
  builds most payloads by itself and inserts them (via cbfstool) afterwards.
  This is more flexible, allowing greater configuration; even U-Boot is
  handled this way, though U-Boot at least still uses coreboot's crossgcc
  toolchain collection to compile it. Patch courtesy Nicholas Chin.
* `util/spkmodem-recv`: New utility, forked from GNU's implementation, then
  re-written to use OpenBSD style(9) programming style instead of the
  originally used GNU programming style, and it is uses
  OpenBSD `pledge()` when compiled on OpenBSD. Generally much cleaner coding
  style, with better error handling than the original GNU version (it is forked
  from coreboot, who forked it from GNU GRUB, with few changes made). This
  is a receiving client for spkmodem, which is a method coreboot provides to
  get a serial console via pulses on the PC speaker.
* download/coreboot: Run `extra.sh` directly from given coreboot tree. Unused
  by any boards, but could allow expanding upon patching capabilities in lbmk
  for specific mainboards, e.g. apply coreboot gerrit patches in a specific
  order that is not easy to otherwise guarantee in more generalised logic of
  the Libreboot build system.
* `util/e6400-flash-unlock`: New utility, that disables flashing protections
  on Dell's own BIOS firmware, for Dell Latitude E6400. This enables Libreboot
  installation *without* disassembling the machine (external flashing equipment
  is *not required*). Courtesy Nicholas Chin.
* Build dependencies scripts updated for more modern distros. As of this day's
  release, Libreboot compiles perfectly in bleeding edge distros e.g. Arch
  Linux, whereas the previous 20220710 required using old distros e.g.
  Debian 10.
* `cbutils`: New concept, which implements: build coreboot utilities like
  cbfstool and include the binaries in a directory inside lbmk, to be re-used.
  Previously, they would be compiled in-place within the coreboot build system,
  often re-compiled needlessly, and the checks for whether a given util are
  needed were very ad-hoc: now these checks are much more robust.
  Very centralised approach, per coreboot tree, rather than selectively
  compiling specific coreboot utilities, and makes the build system logic in
  Libreboot much cleaner.
* GRUB config: 30s timeout by default, which is friendlier on some desktops
  that have delayed keyboard input in GRUB.
* ICH9M/GM45 laptops: 256MB VRAM by default, instead of 352MB. This fixes
  certain performance issues, for some people, as 352MB can be very unstable.
* U-Boot patches: for `gru_bob` and `gru_kevin` chromebooks, U-Boot is used
  instead of Google's own *depthcharge* bootloader. It has been heavily
  modified to avoid certain initialisation that is replaced by coreboot, in
  such a way that U-Boot is mainly used as a bootloader providing UEFI for
  compliant Linux distros and BSDs. Courtesy Alper Nebi Yasak.
* lbmk: The entire Libreboot build system has, for the most part, been made
  portable; a lot of scripts now work perfectly, on POSIX-only implementations
  of `sh` (though, many dependencies still use GNU extensions, such as GNU
  Make, so this portability is not directly useful yet, but a stepping stone.
  Libreboot eventually wants to be buildable on non-GNU, non-Linux systems,
  e.g. BSD systems)
* nvmutil: Lots of improvements to code quality, features, error handling. This
  utility was originally its own project, started by Leah Rowe, and later
  imported into the Libreboot build system.
* build/boot/roms: Support cross-compiling coreboot toolchains for ARM platforms,
  in addition to regular x86 that was already supported. This is used for
  compiling U-boot as a payload, on mainboards.
* U-boot integration: at first, it was just downloading U-Boot. Board integration
  for ARM platforms (from coreboot) came later, e.g. ASUS Chromebook Flip C101
  as mentioned above. The logic for this is forked largely from the handling
  of coreboot, because the interface for dealing with their build systems is
  largely similar, and they are largely similar projects. Courtesy Denis Carikli
  and Alper Nebi Yasak.
* New utility: `nvmutil` - can randomise the MAC address on Intel GbE NICs, for
  systems that use an Intel Flash Descriptor
* General build system fixes: better (and stricter) error handling
* Fixed race condition when building SeaBIOS in some setups. 
* GRUB configs: only scan ATA, AHCI or both, depending on config per board.
  This mitigates performance issues in GRUB on certain mainboards, when
  scanning for `grub.cfg` files on the HDD/SSD.
* GRUB configs: speed optimisations by avoiding slow device enumeration in
  GRUB.

The number of changes are vast, too big to be readable on a release
announcement. Again, I say: check log in `lbmk.git`.

Hardware supported in Censored Libreboot c20230710
==================================================

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

Servers (AMD, x86)
------------------

-   [ASUS KGPE-D16 motherboard](../docs/hardware/kgpe-d16.md)
-   [ASUS KFSN4-DRE motherboard](../docs/hardware/kfsn4-dre.md)

Desktops (AMD, Intel, x86)
-----------------------

-   [ASUS KCMA-D8 motherboard](../docs/hardware/kcma-d8.md)
-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)

### Laptops (Intel, x86)

-   **[Dell Latitude E6400](../docs/hardware/e6400.md) (easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Downloads
=========

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.

This censored version is in the directory named `censored`, on Librbeoot rsync
and https mirrors. For example:

<https://www.mirrorservice.org/sites/libreboot.org/release/censored/c20230710/>

tl;dr yes, I made this special release of Libreboot specifically so that I could
crap all over it. Any project that tries (whether or not they succeed) to
replicate the old Libreboot project (as illustrated by this special release of
Libreboot) are doing themselves, and their users, a major disservice by
providing completely inferior firmware, and mostly on very outdated hardware
that normal people don't want to use.

Ideological purity is all well and good, but you have to meet people where
they're at. If someone approaches you with hardware that *can* have certain
proprietary code replaced (thus increasing software freedoms), they *should* be
accomodated, and Libreboot's mission is to do exactly that. We believe
passionately in free software, and we want everyone to use it!

Coreboot is one of humanity's greatest achievements. It should be respected,
not shunned. All coreboot ports are valid, and Libreboot will eventually
assimilate all of them.
