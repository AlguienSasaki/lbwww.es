---
title: Hardware compatibility list 
x-toc-enable: true
...

This sections relates to known hardware compatibility in libreboot.
**Osboot merged with and became part of Libreboot on November 15th, 2022. As
of this day, November 16th, many new boards have been added as a result, but
some may not be documented or even listed at all (on this page). If any are
missing, please tell us on IRC.**

**If a target doesn't yet have ROM images in the release (ones present as a
result of the osboot merge), please know that the Libreboot 20220710 release
was conducted before the merge. The next release after Libreboot 20220710 shall
contain ROM images for these additional boards.**

For installation instructions, refer to [../install/](../install/).

NOTE: For T60/R60 thinkpads, make sure that it has an Intel GPU, not an ATI GPU
because coreboot lacks native video initialization for the ATI GPUs on these
machines.

(for later machines like T500, T400, ATI GPU doesn't matter, because it also
has an Intel GPU, and libreboot uses the Intel one)

Supported hardware
==================

libreboot currently supports the following systems in this release:

### Desktops (AMD, Intel, x86)

-   [Gigabyte GA-G41M-ES2L motherboard](ga-g41m-es2l.md)
-   Acer G43T-AM3
-   [Intel D510MO and D410PT motherboards](d510mo.md)
-   [Intel D945GCLF](d945gclf.md) (D945GCLF2D also reported working by a user)
-   [Apple iMac 5,2](imac52.md)

### Servers/workstations (AMD, x86)

-   [ASUS KCMA-D8 motherboard](kcma-d8.md)
-   [ASUS KGPE-D16 motherboard](kgpe-d16.md)
-   [ASUS KFSN4-DRE motherboard](kfsn4-dre.md)

### Laptops (Intel, x86)

-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md)
-   Lenovo ThinkPad X230 (documentation needs to be re-added, ever since the
    purge on 4 January 2022. It will be done asap)
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](r400.md)
-   [Lenovo ThinkPad T400 / T400S](t400.md)
-   [Lenovo ThinkPad T500](t500.md)
-   [Lenovo ThinkPad W500](t500.md)
-   [Lenovo ThinkPad R500](r500.md)
-   [Apple MacBook1,1 and MacBook2,1](macbook21.md)
-   [Lenovo ThinkPad T440p](../install/t440p_external.md)
-   [Lenovo Thinkpad X220](../install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../install/x230_external.md)
-   [Lenovo Thinkpad X230t](../install/x230_external.md)

### Laptops (ARM, u-boot, MOSTLY UNTESTED AS OF NOVEMBER 16, 2022)

-   Samsung Chromebook 2 13" (peach-pi)
-   Samsung Chromebook 2 11" (based on `peach_pit` in coreboot)
-   HP Chromebook 11 G1 (google/daisy)
-   Samsung Chromebook XE303 (google/daisy)
-   HP Chromebook 14 G3 (google/nyan\_blaze)
-   Acer Chromebook 13 (CB5-311, C810) (google/nyan\_big)
-   ASUS Chromebit CS10 (google/veyron\_mickey)
-   ASUS Chromebook Flip C100PA (google/veyron)
-   ASUS Chromebook C201PA (google/veyron\_speedy)
-   ASUS Chromebook Flip C101 (google/gru)
-   Samsung Chromebook Plus (v1) (google/gru)

NOTE: ARM hardware is currently undocumented in Libreboot, both for
installation of Libreboot and the installation of an operating system. If you
have one of these machines configured with Libreboot, you should consult
u-boot documentation to learn how it's operated. You might ping `alpernebbi` on
Libera IRC, who ported these boards to Libreboot. Extensive work was
performed, to make u-boot work correctly. **TODO: installation instructions
must be added to documentation!**

TODO: More hardware is supported. See `resources/coreboot/` in lbmk. Update
the above list!

'Supported' means that the build scripts know how to build ROM images
for these systems, and that the systems have been tested (confirmed
working). There may be exceptions; in other words, this is a list of
'officially' supported systems.

EC update on i945 (X60, T60) and GM45 (X200, X301, T400, T500, R400, W500, R500)
==============================================================

It is recommended that you update to the latest EC firmware version. The
[EC firmware](../../faq.md#ec-embedded-controller-firmware) is separate from
libreboot, so we don't actually provide that, but if you still have
Lenovo BIOS then you can just run the Lenovo BIOS update utility, which
will update both the BIOS and EC version. See:

-   [../install/#flashrom](../install/#flashrom)
-   <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

NOTE: this can only be done when you are using Lenovo BIOS. How to
update the EC firmware while running libreboot is unknown. libreboot
only replaces the BIOS firmware, not EC.

Updated EC firmware has several advantages e.g. better battery
handling.

How to find what EC version you have (i945/GM45)
------------------------------------------------

In GNU+Linux, you can try this:

    grep 'at EC' /proc/asound/cards

Sample output:

    ThinkPad Console Audio Control at EC reg 0x30, fw 7WHT19WW-3.6

7WHT19WW is the version in different notation, use search engine to find
out regular version - in this case it's a 1.06 for x200 tablet
