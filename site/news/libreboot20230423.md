% Libreboot 20230423 released!
% Leah Rowe
% 23 April 2023

Introduction
============

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on ARM-based chromebooks supported (U-Boot bootloader, instead of Google's
depthcharge bootloader). On x86 machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. You can find the list of supported hardware in the Libreboot
documentation.

The last Libreboot release, version 20230413, was released on 13 April
in 2023. *This* new release, Libreboot 20230423, is released today on
April 23rd, 2023.

This is marked as a *testing* release, though it is *basically stable*.
We've been going at it like crazy, on a big spree adding more mainboards from
coreboot. Some fixes to the build system were also made, since the last release
only *10 days ago*.

The *priority* for Libreboot is to add as many new boards as possible, from now
to the next stable release (ETA Q3 2023), with many testing releases in
between. Release early, release often. Rigorious testing ensues.

Build from source
-----------------

*This* release was build-tested on Debian *Sid*, as of 23 April 2023. Your
mileage may vary, with other distros. Refer to Libreboot documentation.

KCMA-D8 and KGPE-D16 wanted!
----------------------------

[ASUS KGPE-D16 and KCMA-D8 needed for testing!](kgpe-d16.md)

These boards still haven't made it back to Libreboot, but I wish to re-add
them in a future release. If you can give/loan me a fully assembled workstation
with one (or both) of these, I would appreciate it. Please
[get in touch](../contact.md)!

Work done since last release
============================

This is in the last *10 days*, since the previous release was 10 days ago!
Ergo, this is a very conservative changelog. It seems Libreboot has been
releasing almost fortnightly, as of late; perhaps this could continue from
now on.

New mainboards now supported:
-----------------------------

* **Dell Latitude E6400 (laptop)** (GM45, blob-free, flashable entirely in
  software, no disassembly required!) - courtesy Nicholas Chin, `nic3-14159` on
  Libreboot IRC.
* HP Compaq 8200 Elite SFF (desktop), courtesy Riku Viitanen (`Riku_V` on
  Libreboot IRC) - *Sandybridge* hardware generation, really nice machine,
  cheap, easy to flash, supports 32GB RAM, multiple HDDs etc.
* HP EliteBook Folio 9470m (laptop), courtesy Riku Viitanen (IvyBridge gen)
* HP EliteBook 2560p (laptop), courtesy Riku Viitanen (*seriously* cool guy) -
  Sandybridge hardware gen

Build system changes:
---------------------

* **GM45 no-microcode bug mitigations re-added: revert to old SMRR handling
  and disable PECI (for e.g. X200/T400 users who want to [remove microcode
  updates](gm45microcode.md), using `cbfstool`) - fixes broken reboot/speedstep
  CPU scaling in such configuration.** - Patch:
  <https://browse.libreboot.org/lbmk.git/commit/?id=bd4ea9a02845b22a09b73ebb015ce134234d100b>
  (patch by Leah Rowe) - this also affects Dell Latitude E6400, and it can be
  used there on that board. We recommend *keeping* microcode updates, but these
  mitigations were re-added to satisfy users of older releases that excluded
  them, who want to still have the option to feasibly run without them.
  [This is ill advised, due to bugs that the microcode updates
  fix](gm45microcode.md)
* `blobutil/inject`: Fixed bad variable expansion pattern
* `build/release/roms`: HP KBC1126 EC firmware scrubbed from release ROMs, for
  re-insertion later via `./blobutil download` and `./blobutil inject` like
  with ME images via `me_cleaner` - for HP laptops.
* `build/dependencies/parabola`: New script for installing build dependencies
  in Parabola GNU+Linux, courtesy of Riku Viitanen (`Riku_V` on Libreboot IRC)
* `util/nvmutil`: sorted includes alphabetically; `sys/` first (puffy!)
* `util/e6400-flash-unlock`: New utility for Dell Latitude E6400 added, written
  by Nicholas Chin (`nic3-14159` on Libreboot IRC). It sends EC commands to
  pull a GPIO connected to `GPIO33`/`HDA_DOCK_EN` in the chipset to a low logic
  state, disabling IFD-based flash protections. Additionally, it bypasses the
  SMM BIOS lock protection by disabling SMIs, and since Dell's own BIOS offers
  no other protections, the machine can be flashed *entirely with software on
  the host CPU*, from Dell BIOS to Libreboot! See:
  <https://browse.libreboot.org/lbmk.git/tree/util/e6400-flash-unlock>
* GRUB payload: `grub.cfg` menu timeout now 30s, not 5s
* `blobutil/download`: support downloading KBC1126-based EC firmware for HP
  laptops. (patch by Leah Rowe)
* `blobutil/download: Support extracting `me.bin` from full archives, when
  running `./blobutil download` - this is done, using the `-M` option
  in `me_cleaner` (some vendors put whole ROM images with IFD, GBE, ME and BIOS
  regions in them, inside their BIOS update archives - we only need to get ME
  from them, to run through `me_cleaner`) in `me_cleaner`. Ninja'd into lbmk by
  Leah Rowe.

Hardware supported in this release
==================================

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

Desktops (AMD, Intel, x86)
-----------------------

-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)
-   [HP Elite 8200 SFF](../docs/hardware/hp8200sff.md) (HP 6200 Pro Business probably works too)

### Laptops (Intel, x86)

-   **[Dell Latitute E6400](../docs/hardware/e6400.md) (easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   Lenovo ThinkPad X230
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad T530](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)
-   [Lenovo ThinkPad T440p](../docs/install/t440p_external.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   [Lenovo ThinkPad W541](../docs/install/ivy_has_common.md)
-   [HP EliteBook 2560p](../docs/hardware/hp2560p.md)
-   [HP EliteBook Folio 9470m](../docs/hardware/hp9470m.md)

### Laptops (ARM, with U-Boot payload)

-   [HP Chromebook 14 G3 (nyan-blaze)](../docs/install/chromebooks.md)
-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](../docs/install/chromebooks.md)
-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

More boards soon!
=================

I've purchased about ~10 HP mainboards, all of the viable sandybridge,
ivybridge and haswell ones from coreboot. I'm going to add them all.

I also have Dell Optiplex 7020 and 9020; these are on coreboot gerrit and
will also be added, in the next Libreboot release (Haswell gen).

I'm going to re-work a lot of the merged Haswell boards, so that they can
also make use of Angel's experimental libre MRC raminit and such, currently
available on ThinkPad T440p and W541 as an option in Libreboot (including in
this release!)

Downloads
=========

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.
