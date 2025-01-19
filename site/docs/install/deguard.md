---
title: Disabling Intel Boot Guard on MEv11
x-toc-enable: true
...

Deguard enables open source BIOS/UEFI firmware
-------------------------------------------

On *some* (not all) motherboards, the vendor chooses to fuse a key during
manufacturing, which ensures that you can *only* boot firmware cryptographically
signed and verified by *them*. This is a form of DRM that otherwise prevents
use of coreboot-based firmware, such as Libreboot. We are *against* DRM in the
Libreboot project. *Intel* markets the Boot Guard as a security feature, but
we regard it simply as an *attack*
on [Free Software](https://writefreesoftware.org/learn).

This document will teach you about *deguard*, which is a utility that modifies
the Intel ME (which implements Boot Guard) in such a way as to *disable* the
Intel Boot Guard, by exploiting a known security vulnerability on MEv11. The
Libreboot project *uses deguard* for *several* coreboot ports, such as
the [Lenovo ThinkPad T480 with Libreboot](t480.md) or
the [Dell OptiPlex 3050 Micro with Libreboot](dell3050.md).

This covers Intel Skylake, Kaby Lake and Kaby Lake Refresh / Coffeelake
machines; note that Coffeelake includes KabyLake Refresh and may have MEv12.
This page concerns only those platforms that have Intel MEv11, not MEv12. The
facts on this page are applicable to both *mobile* and *desktop* platforms.

The Intel Boot Guard is a "security" mechanism implemented by intel, but not all
vendors enable it. If enabled, the bootblock in the flash is protected at boot
time by cryptographic signature verification; this means only the vendor can
update the flash.

On systems with MEv11, a bug exists in older versions (of MEv11) that allows
for unsigned code execution, at a very early stage in the boot process, to the
point that almost all of the ME firmware in flash can be fully replaced. The
ME is also what implements Boot Guard, and the hack is possible *before* Boot
Guard is enforced, allowing for it to be disabled. This does *not* unfuse the
keys set by the manufacturer, but rather, it bypasses all checks against them.

See: [CVE-2017-5705](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00086.html)

Deguard utility
---------------

Mate Kukri, who authored the ThinkPad T480/T480 and OptiPlex 3050 Micro ports,
wrote a tool called *deguard*, which Libreboot uses on these boards. This tool
reconfigures the ME, exploiting it so as to disable the Boot Guard.

You can find it hosted by the coreboot project:

<https://review.coreboot.org/deguard> (git repository)

and Libreboot provides a mirror of this repository:

<https://codeberg.org/libreboot/deguard>

More information is available there.

How it works
------------

This is done by externally flashing an older version. Libreboot's build system
automatically downloads this older version, runs `me_cleaner` on it, and applies
the deguard hack; this includes machine-specific ME configuration, which is
added per machine by extracting it from a dump of the original flash. The
resulting configuration (for the MFS partition in the ME) is then inserted into
the generic ME image.

Note that the deguard utility can *also* be used on MEv11 setups that *don't*
have Boot Guard, if you simply want to auto-download and neuter a generic ME
image, and then provide machine configuration. It is essentially doing the same
thing that Intel's own *FITC* utility does (Intel Flash Image Tool), which is
normally only available to vendors; the *deguard* utility written by Mate Kukri
is available under a free software license, and included by default in Libreboot
releases. It can be used for *any* MEv11-based system.

How to use deguard
------------------

To download deguard in lbmk (Libreboot's build system), do this:

	./mk -f deguard

Then go in `src/deguard/` and the `README.md` file in there tells you more
information about how it works, and how to use it. You do not need to run
this tool yourself, unless you're adding a new board, because Libreboot is
programmed to use it automatically, during the build process (or during
insertion of vendor files after the fact, on release images).

With deguard, the machine is operating in a state as though the Boot Guard keys
were never fused, even if they were. [Previous work](https://trmm.net/TOCTOU/)
has been done by others, related to the Boot Guard, but nothing quite so
thorough and easy to use as deguard existed previously!

T480/T480s MFS data
-------------------

As of Libreboot 20241206, the upstream deguard project did not contain an MFS
config for Lenovo ThinkPad T480 and T480s, because Libreboot added them in an
out-of-tree patch (under Mate Kukri's direct guidance).

Therefore, if you're using deguard on a standalone/custom coreboot setup without
using Libreboot/lbmk, please do ensure that they are there. The tool itself
provides a README, or you could simply import the config data from lbmk if
you wish.

How does deguard work?
----------------------

Mate Kukri was able to figure this out and implement deguard, using existing
work done by PT Research and Youness El Alaoui, exploiting the Intel SA 00086
bug which you can read more about here:

<https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00086.html>

<https://www.intel.com/content/www/us/en/support/articles/000025619/software.html>

Note that *Intel* refers to this as a means of a so-called *attacker* running
so-called *malicious* code; while this may also be possible in the strictest
sense, flash write protection is possible on these machines, which you can
read about on the [GRUB hardening](../linux/grub_hardening.md) page. Intel made
the Boot Guard without giving users control of it, so people have worked for
years to try to hack around it, as a matter of user freedom. So remember: when
Intel is talking about security, they mean *their* security, not yours. To them,
you are simply flashing malicious code. But they are the ones with malice.

Thanks
------

Mate Kukri and others who work on such hacks are heroes, and they have done a
great service to the Libreboot project.

Many more machines are now possible to port to coreboot, thanks to this hack.

The Libreboot project owes Mate Kukri a debt of gratitude, many times over.
