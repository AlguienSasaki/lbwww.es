% HP EliteBook 8470p and Dell Precision T1650 support added to Libreboot
% Leah Rowe
% 14 August 2023

**UPDATE, 16 August 2023:
[installation instructions are now available for
HP EliteBook 8470p](../docs/install/hp8470p.md) and also
[Dell Precision T1650](../docs/install/t1650.md).**

Free your BIOS today!
-------------------

I've ported and tested two new boards, and merged them into the `lbmk` master
branch. They will be available in the next regular release:

<img tabindex=1 class="r" style="max-width:25%" src="https://av.libreboot.org/hp8470p/hp8470p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8470p/hp8470p.jpg" /></span>

### HP EliteBook 8470p

HP EliteBook 8470p is another IvyBridge platform, similar to other EliteBooks
that Libreboot already supports, such as the HP EliteBook 9470m that Riku
added.

It was added in this patch:
<https://browse.libreboot.org/lbmk.git/commit/?id=6075fed84912be82cb03a99ad5a5a82547872463>

It should be noted: there are *two* variants of this machine. One variant has
Intel graphics, and this is what was tested. The other variant has discrete
AMD graphics, and is not supported, but could be added - if you have one of
these AMD variants, please [contact Leah Rowe](../docs/maintain/testing.html).

Another quirk: SOIC16 flash IC, but it's 16MB in size (Winbond W25Q128).

There is currently no documentation on the coreboot website, but installation
instructions will be written for the Libreboot website.

### Dell Precision T1650

This machine is truly unique, among most desktop boards in coreboot. It's yet
another IvyBridge desktop, same platform as [HP 8300
USDT](../docs/install/hp8300usdt.html), but it supports *ECC* memory, if you
use a Xeon E3 model CPU (ivybridge variant).

It was added in this patch:
<https://browse.libreboot.org/lbmk.git/commit/?id=f9afeb6feb289003359790e820d7e7d918937fea>

<img tabindex=1 class="l" style="max-width:25%" src="https://av.libreboot.org/t1650/t1650.jpg" /><span class="f"><img src="https://av.libreboot.org/t1650/t1650.jpg" /></span>

You can find a list of IvyBridge CPUs here:
<https://en.wikipedia.org/wiki/Ivy_Bridge_(microarchitecture)#List_of_Ivy_Bridge_processors>

*My* one, that I tested, has Intel *Xeon E3 1270v2* - this model does not have
an Intel graphics chip built in, so I could not test coreboot's libgfxinit for
this board, but it should work. I've since ordered a 1275v2 which does have
graphics, but I didn't have it at the time of this news post.

<img tabindex=1 class="r" style="max-width:25%" src="https://av.libreboot.org/t1650/t1650_grub.jpg" /><span class="f"><img src="https://av.libreboot.org/t1650/t1650_grub.jpg" /></span>

Native video init for Intel graphics *is* enabled, in this Libreboot port, but
because I haven't tested it yet, I've done *this* in the port that was merged:
SeaBIOS-only at startup, but GRUB is available in the SeaBIOS menu. SeaBIOS
is relied upon to execute the VGA Option ROM on an add-on graphics card (the
one I had was Nvidia Quadro K420, tested with the `nouveau` driver under
Debian Linux).

If you want GRUB-only setup, you could configure SeaBIOS bootorder to only
load the GRUB payload, and disable the SeaBIOS menu. This can be done by
only booting `img/grub2` from CBFS. More info available here:
<https://www.seabios.org/Runtime_config> - just insert the appropriate files
into CBFS, as per the instructions on that page. Make sure you have external
flashing equipment to recover in case you brick it!

<img tabindex=1 class="r" style="max-width:25%" src="https://av.libreboot.org/t1650/t1650_libreboot_website.jpg" /><span class="f"><img src="https://av.libreboot.org/t1650/t1650_libreboot_website.jpg" /></span>

With ECC memory, this board is easily suitable for running a server of some kind,
in cases where data integrity matters (many userspace programs will use `read()`
and `write()` to manipulate data on the HDD/SSD, so it's best to ensure that the
memory is as reliable as possible) - the board also has *four* SATA slots, so
you could easily do RAID10 or RAID6 - a filesystem like ZFS running under FreeBSD
would be a boon. Stick 32GB of ECC RAM with RAID10 HDDs on FreeBSD and run
OpenBSD virtual servers in bhyve!

Other works currently in progress
-------------------------------

A similar report was made in the [HP EliteBook 2170p news
page](hp2170p.md). Some of that is now done, so the list is updated (and more
work is also being done):

### More boards coming soon

I'm *still* on a spree, adding more boards to Libreboot, especially HP
EliteBooks. These boards are of interest, to the Libreboot project, at this
present time:

* Dell OptiPlex 9010 - similar to the T1650 (same platform, same vendor)
* HP ProBook 6360b - it has been added today to Libreboot, but it's not yet
  tested.
* HP Revolve 810 G1
* HP EliteBook Folio 9480m - it has been added already to Libreboot (WIP), but
  currently does not boot (more testing needed - not sure what my problem is
  yet, but it was added today).
* HP EliteBook 820 G2 (broadwell generation)
* HP EliteBook *840* G2 - not supported in coreboot yet, but should be similar
  to 820 G2 and I've ordered one for testing. It's probably an easy port.
* HP Z220 workstation
* Dell Precision T1650
* Dell Optiplex 7020 and 9020 - not mentioned in the last post, but I also have
  these.
* Dell Latitude E6320 - not supported in coreboot yet, but there's a patch
  under review on gerrit for Latitude E6220, and the E6320 should be similar
  enough for an easy port. This is the gerrit patch for E6220:
  <https://review.coreboot.org/c/coreboot/+/74287>

Another build system audit
--------------------------

I've also started another audit of the Libreboot build system; the [last
one](audit.md) was quite intense and a lot more work is going in for the
next release.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
