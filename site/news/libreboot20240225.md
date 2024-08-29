% Libreboot 20240225 released!
% Leah Rowe
% 25 February 2024

Introduction
============

Libreboot is a free/open source BIOS/UEFI replacement on x86 and ARM, providing
boot firmware that initialises the hardware in your computer, to then load an
operating system (e.g. Linux/BSD). It is specifically a *coreboot distribution*,
in the same way that Debian is a Linux distribution. It provides an automated
build system to produce coreboot ROM images with a variety of payloads such as
GNU GRUB or SeaBIOS, with regular well-tested releases to make coreboot as easy
to use as possible for non-technical users.

Libreboot provides many additional benefits such as fast boot speeds, greater
security and greater customisation, but the *primary* benefit
is [software freedom](https://writefreesoftware.org/learn).

*This* new release, Libreboot 20240225, released today 25 February 2024, is
a new *testing* release of Libreboot. The previous release was
Libreboot 20240126, released on 26 January 2024. The most careful and
iterative approach is being taken in current development, because a new stable
release is to be made available by the summer of 2024. Much of Libreboot's
current focus is purely on hardware support, since the build system is more
or less stable at this point (owing to all of last year's major code audits).

Today's release has had some testing already, on most of the boards, and should
run just fine. If you spot any issues, please report them on Libreboot's bug
tracker.

Hardware supported in this release
==================================

This release supports the following hardware:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](../docs/hardware/kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](../docs/hardware/kgpe-d16.md)

### Desktops (AMD, Intel, x86)

-   **[Dell OptiPlex 7020/9020 MT and SFF](../docs/hardware/dell9020.md) - Also [available to buy
    with Libreboot preinstalled](https://minifree.org/product/libreboot-9020/)** - Dell OptiPlex XE2 MT/SFF also known to work
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)
-   [ASUS KCMA-D8 motherboard](../docs/hardware/kcma-d8.md)
-   Dell OptiPlex 7010 **MT** (known to work, using the T1650 ROM, but more
    research is needed)
-   [Dell Precision T1650](../docs/hardware/t1650.md)
-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [HP Elite 8200 SFF/MT](../docs/hardware/hp8200sff.md) (HP 6200 Pro Business probably works too)
-   [HP Elite 8300 USDT](../docs/hardware/hp8300usdt.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Intel D945GCLF](../docs/hardware/d945gclf.md)

### Laptops (Intel, x86)

-   **[Lenovo ThinkPad T440p](../docs/install/t440p_external.md) - Also [available
    to buy with Libreboot preinstalled](https://minifree.org/product/libreboot-t440p/)**
-   **[Lenovo ThinkPad W541](../docs/install/ivy_has_common.md) - Also [available to
    buy with Libreboot preinstalled](https://minifree.org/product/libreboot-w541/)**
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)
-   [Dell Latitude E6400, E6400 XFR and E6400 ATG, all with Nvidia or Intel
    GPU](../docs/install/latitude.md)
-   [Dell Latitude E6420 (Intel GPU](../docs/install/latitude.md)
-   [Dell Latitude E6430 (Intel GPU](../docs/install/latitude.md)
-   [Dell Latitude E5520 (Intel GPU](../docs/install/latitude.md)
-   [Dell Latitude E5530 (Intel GPU](../docs/install/latitude.md)
-   [Dell Latitude E6520 (Intel GPU](../docs/install/latitude.md)
-   [Dell Latitude E6530 (Intel GPU](../docs/install/latitude.md)
-   [HP EliteBook 2170p](../docs/hardware/hp2170p.md)
-   [HP EliteBook 2560p](../docs/hardware/hp2560p.md)
-   [HP EliteBook 2570p](../docs/hardware/hp2570p.md)
-   [HP EliteBook 820 G2](../docs/hardware/hp820g2.md)
-   [HP EliteBook 8460p](../docs/hardware/hp8460p.md)
-   [HP EliteBook 8470p](../docs/hardware/hp8470p.md)
-   [HP EliteBook 8560w](../docs/hardware/hp8560w.md)
-   [HP EliteBook Folio 9470m](../docs/hardware/hp9470m.md)
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md) (no install docs yet)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad T520 / W520](../docs/install/ivy_has_common.md) (no install guide yet)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md) (no install
-   Lenovo ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   Lenovo ThinkPad X230
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   Lenovo ThinkPad X301
-   Lenovo ThinkPad X60 / X60S / X60 Tablet

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Work done since last release
============================

With the exception of pico-serprog and replacing flashrom with flashprog (more
on this later), the current upstream revisions remain unchanged, but this
release has fixed a few issues and added more mainboards, relative to last
month's release.

New mainboards
--------------

* **HP EliteBook 8560w** - added to lbmk by Riku Viitanen, using the coreboot port
  written by Iru Cai, with Riku's MXM support added on top (more on this later)
* **Dell Latitude E5520** - added to lbmk by Nicholas Chin, using his own coreboot
  port for this board (testing provided by `Minimum_Baseball_629` on reddit)
* **Dell Latitude E5530** - added to lbmk by Nicholas Chin, using his own coreboot
  port for this board (testing provided by Martin Dawson)
* **Dell Latitude E6520** - added to lbmk by Nicholas Chin, using his own coreboot
  port for this board (testing provided by Martin Dawson)
* **Dell Latitude E6420** - added to lbmk by Nicholas Chin, using his own coreboot
  port for this board (testing provided by Martin Dawson)
* **Dell OptiPlex 9020/7020 SFF** (and XE2 SFF) - added to lbmk by Leah Rowe, using
  the coreboot port implemented by Mate Kukri.
* **Dell OptiPlex 9020/7020 MT** (and XE2 MT) - added to lbmk by Leah Rowe, using
  the coreboot port implemented by Mate Kukri - uses Haswell MRC, but testing
  was also done on Broadwell MRC (not yet added to lbmk due to lack of XHCI
  support in GRUB, whereas EHCI seems to work fine in the Haswell one - a patch
  was also [submitted](https://review.coreboot.org/c/coreboot/+/80717) to
  coreboot for review, fixing VGA decode on discrete graphics cards when the
  Broadwell MRC is used - this patch too is not yet included in Libreboot).

SeaBIOS MXM INT15H interrupt
----------------------------

For Libreboot's new HP EliteBook 8560w support, MXM handling was added to
SeaBIOS. This was performed by Riku Viitanen, who also wrote this tool:

<https://codeberg.org/Riku_V/mxmdump/>

These laptops use upgradeable graphics cards in an MXM slot. The MXM
specification states that the MXM config can be provided in an i2c ROM (and
Libreboot would not have to handle that) or, in some cases, may be provided
in the main boot flash, and loaded using an INT15H interrupt.

The MXM config is used by the VGA Option ROM on the card, for configuring
things like ports / power management. Without it, running the option ROM
would result in an error on the screen, telling you that the MXM configuration
was not loaded. In some cases, you may be able to modify the ROM to bypass it,
or it would just "work" anyway - but with undefined behaviour.

With Riku's addition, this EliteBook 8560w now works perfectly. Coreboot has
had a port for some time, but without the MXM support - for now, it is only
available in SeaBIOS, so you must use *SeaBIOS* to run the VGA Option ROM (it
cannot yet be done directly from coreboot).

U-Boot release script
---------------------

The script at `script/update/release` now supports generating standalone
U-Boot source archives, like so:

	./update release -m u-boot

The usual `-d` option also works, for specifying a directory other
than `release/`. Libreboot still provides U-Boot embedded within the larger
source release archive, and does not yet actually provide U-Boot as a
standalone project, but some people may find this useful.

Pico-serprog updates
--------------------

Riku Viitanen added support for controlling multiple chip-selects. With the
new functionality, you can set unused chipselects high. This is useful when
flashing externally on an Intel IFD-based system where the two flashes are
connected, but without resistors between the shared data lines - on these
systems, the other flash's chipselect is electrically *floating* which means
it's in an undefined state; the new version allows you to control all of them,
turning *off* the other chip. This is useful when flashing the ThinkPad W541.

Documentation is provided for this, in the pico-serprog README. Riku also
fixed this issue:
<https://codeberg.org/libreboot/lbmk/issues/182>

Riku also increased the default drive strength to 12mA on all RP2024 boards,
increasing reliabily when externally flashing certain mainboards (e.g. PCH
having low/no resistance on connections to the data lines for the flash).

Flashprog now used, instead of flashrom
---------------------------------------

Nico Huber was unfairly banned from participation in the flashrom project, and
went on to create a new project called [flashprog](https://flashprog.org/). The
purpose of flashprog is to provide *stable* releases, with incremental
development over time, maintaining support for newer chips/devices, while
being careful not to break old ones. The new leadership of flashrom takes a
far less conservative approach; flashprog was initially released as a project
called *flashrom-stable*, and Nico was permitted to use the coreboot gerrit
infrastructure - permission which was later revoked, leading to flashprog.

The new leader of flashrom accused Nico of foul behaviour, while not providing
any evidence; this, at the same time that the two of them had seemingly
incompatible assumptions about the future of the project. It's pretty clear
what the real reason was for his *dismissal*, though the Libreboot project is
not taking Nico's side simply for personal reasons - no, it is because Nico has
a much better vision for the flashrom project, which he is now implementing in
the new flashprog project.

Several developers have already jumped ship and went to work on flashprog.
Nico's vision is a far more sensible one, and he has been invaluable to the
Libreboot project over many years. He has *personally* provided me with help
and advise on numerous occasions. He is also the author of the Roda RK9 port in
coreboot, upon which Vladimir Serbinenko's ThinkPad X200 port was based - work
which *defined* Libreboot in its early days, contributions for which I remain
eternally grateful. I *know* Nico and trust him as a project leader. As a result,
Libreboot withdraws its support and endorsement of flashrom - it will only
promote flashprog from now on.

It's quite possible that *flashrom* has a future, but there's no reason why
their changes cannot also be included in flashprog, if they are good changes,
moving forward. Libreboot supports Nico's work out of principle, both on
a social (community-led, anyone can participate in it) and technological
perspective. This was done because, firstly Nico Huber is a better leader,
and flashprog is very likely to become the dominant project in the future.

This is the *first* Libreboot release to include flashprog sources, instead
of flashrom. Moving forward, we will *not* be providing support for flashrom.
Only flashprog. Libreboot's distribution of flashprog *also* includes the
macronix workaround (useful when externally flashing the ThinkPad X200).

Indeed, Nico's mentality has already proven to have merit; newer revisions of
flashrom *break* internal flashing on ThinkPad X60 and T60, last time it was
tested (some time during Q4 2023, on the latest git revision). Nico's flashprog
is rock-solid on these older platforms, while having already added several major
new features and hardware functionality - for instance, it has code for handling
Riku Viitanen's recent changes on the RP2040 serprog images, for pulling unused
chipselects high (useful on machines like ThinkPad W541 for external flashing).

Context regarding flashprog vs flashrom
---------------------------------------

It was suggested by a reader, on 27 March 2024, that the lack of context made
judging this situation very difficult. Therefore, the following links have
been added, with explanation below.

More context can be gleaned from the following links:

* <https://mail.coreboot.org/hyperkitty/list/flashrom@flashrom.org/thread/47TTR4YNQV7QGPXSL2TROI3FES56XXF7/>
* <https://www.phoronix.com/news/Flashrom-Splits-Into-Two>
* <https://mail.coreboot.org/hyperkitty/list/flashrom@flashrom.org/message/ZOYX5R2R5IO3KVFBOKUSQGSL7I7WIHAL/>
* <https://mail.coreboot.org/hyperkitty/list/flashrom@flashrom.org/message/6MOOPJHTXRS5GK5JUUKL6APJQ6HIHA73/>
* <https://mail.coreboot.org/hyperkitty/list/flashrom-stable@flashrom.org/thread/RTP3UKMMGWUEVTS5XUFRCXXL24UE4FRV/>

The first link is the mailing list post, where the new flashrom leader unilaterally
announced Nico's banishment from the project, without giving actual evidence
to back up her accusations (accusations which are made in the very same post).
Several other flashrom developers weighed in, some taking the side of the new
leadership, and several others taking Nico's side. What happened to Nico was
unfair. As already stated, Libreboot will promote flashprog, *not* flashrom,
from now on.

Not only were such accusations made, but Nico was initially given his own
repository on the coreboot project, for his project, which he then
named *flashrom-stable*. See:

* <https://review.coreboot.org/admin/repos/flashrom-stable,general>

However, as you can see (at least on this date, 27 March 2024), this repo has
been hidden from view. Nico was banned from even having flashrom-stable on
the same infrastructure as flashrom, which uses the coreboot infrastructure.
This later lead to Nico establishing the Flashprog project.

Libreboot stands with Nico and his Flashprog project. Anyone contributing to
flashrom should consider working for flashprog, either in addition to or
instead of flashrom. Libreboot will continue its boycott of Flashrom until
its current leader, Anastasia Klimchuk, steps down from the leadership, and
even then only after reparations/apologies are issued to the accused.

Exact git log
-------------

The following is an exact log of commits in the Git repository, on the master
branch, relative to the previous January 2024 release:

```
* d4d25993 Libreboot 20240225 
* 35e5464a config/vendor: fix entry for 9020sff 
* 53544ff3 disable hiding peg from mrc on dell 9020 
*   7073ba3e Merge pull request 'config/ifd/dell_ivybridge: Add ifd_nogbe' (#188) from nic3-14159/lbmk:add-dell-ivb-ifd-nogbe into master 
|\  
| * 75c9a2b1 config/ifd/dell_ivybridge: Add ifd_nogbe 
|/  
* 4680d154 ./update trees -u coreboot 
* 0add5571 NEW BOARD: dell 9020 optiplex sff 
*   4641d996 Merge pull request 'Add HP EliteBook 8560w, MXM' (#187) from Riku_V/lbmk:hp8560w into master 
|\  
| * f9ed92e4 Add HP EliteBook 8560w 
| * 4a9fca57 Patch SeaBIOS: Add MXM support 
* | b7bc713b update pico-serprog to new revision 
* |   31849194 Merge pull request 'Add Dell Latitude E5520' (#184) from nic3-14159/lbmk:latitude-ports into master 
|\ \  
| * | aadfa6bb config: Add Dell Latitude E5520 
| * | 381cb119 config/coreboot/default/patches : Renumber E6420, E6520, E5530 patches 
|/ /  
* | 06933491 coreboot/dell9020mt: disable pcie rebar 
* | a8435c4f remove coreboot/dell9020mtvga_12mb 
* | 872e3b92 Merge pull request 'update revision: pico-serprog' (#185) from Riku_V/lbmk:master into master 
|\| 
| * 0e3a5759 update revision: pico-serprog 
|/  
* 91792c0c update coreboot configs 
*   667854de Merge pull request 'Add Latitude E6420, E6520, and E5530' (#183) from nic3-14159/lbmk:latitude-ports into master 
|\  
| * eee22447 config: Add Dell Latitude E5530 
| * a5bfbe4d config: Add Dell Latitude E6520 
| * 617f2b88 config: Add Dell Latitude E6420 
* | abe33ce0 support making u-boot-only tarballs in releases 
* | 8e2e9735 add vga-only 9020 config 
* | dfad11f3 NEW BOARD: Dell OptiPlex 9020 MT (and 7020 MT) 
* | b2d8e118 import dell optiplex 7020/9020 patch from gerrit 
|/  
* 0c8fa201 update pico-serprog to Riku's new revision 
*   2ad52ed3 Merge pull request 'flashprog: apply the good old MX25 workaround' (#180) from Riku_V/lbmk:master into master 
|\  
| * 112d2a4e flashprog: apply the good old MX25 workaround 
|/  
* 77770f5a remove remaining flashrom remnants (use flashprog) 
* 36ddd6f6 update parabola dependencies for flashprog 
* 182a029f update arch dependencies for flashprog 
* e8523864 update trisquel dependencies for flashprog 
* 4131981c update debian dependencies for flashprog 
* af82d671 config/git: use flashprog instead of flashrom 
```

You may find archives of this release, by looking at the Libreboot download
page. Support is available on IRC or Reddit if you need help.

NOTE: As in the previous release, HP 820 G2 images are not provided, because
the inject scripts cannot currently produce a reliable checksum when inserting
vendor files on these boards, due to the non-reproducible way in which the
refcode file is compressed during insertion. Therefore, you
must [build it from source](../docs/build/).
