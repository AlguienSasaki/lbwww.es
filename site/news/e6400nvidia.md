% Experimental Nvidia GPU support on Dell Latitude E6400 variants, plus E6400 XFR support now confirmed
% Leah Rowe
% 9 May 2023

<img tabindex=1 class="r" style="max-width:35%" src="https://av.libreboot.org/e6400/e6400xfr-seabios.jpg" /><span class="f"><img src="https://av.libreboot.org/e6400/e6400xfr-seabios.jpg" /></span>

Introduction
============

[Dell Latitude E6400 *with Intel GMA 4500MHD* graphics](e6400.md) was added, and
included in the recent Libreboot 20230423 release. *Today*, [experimental
support](https://browse.libreboot.org/lbmk.git/log/?h=e6400nvidia_wip) is now
available for variants with GPU: Nvidia Quadro NVS 160M. The Dell Latitude 6400
XFR (rugged variant) was also tested today (Intel graphics) and confirmed
working in Libreboot 20230423.

The *Nvidia* variants are *not* supported in Libreboot 20230423. Support is
available in an experimental branch of Libreboot. 6400 XFR testing+photo
provided, courtesy Mark Cornick (`mcornick` on Libreboot IRC).

Dell Latitude E6400 with Nvidia GPU
===================================

This section *also* applies to E6400 XFS and ATG models. [Testers are
needed!](../docs/maintain/testing.md).

*Some* models of Dell Latitude E6400 have Nvidia Quadro NVS 160M graphics
device, instead of Intel GMA 4500MHD. The *initial* Libreboot port of Dell
E6400 *only* supported models with Intel graphics, but experimental support
for Nvidia graphics now exists, in a WIP branch of Libreboot.

<img tabindex=1 class="l" style="max-width:25%" src="https://av.libreboot.org/e6400/e6400-seabios.jpg" /><span class="f"><img src="https://av.libreboot.org/e6400/e6400-seabios.jpg" /></span>

The Libreboot documentation has been updated, to cover these models. Refer
to Dell Latitude E6400 documentation in Libreboot; specifically,
the [E6400 info page](../docs/hardware/e6400.md) and [E6400 flashing
guide](../docs/install/e6400.md).

Nicholas Chin, author of the original Dell E6400 port, has been helping me and
I've been working on this experimental branch of Libreboot under his
guidance:

* <https://browse.libreboot.org/lbmk.git/log/?h=e6400nvidia_wip>

*These* patches were already merged in Libreboot's master branch, which the
changes in the WIP branch rely upon:

* Import the `bios_extract` utilities in lbmk: <https://browse.libreboot.org/lbmk.git/commit/?id=6d0ff0286451dc43d32428a44a68d07bc13c058a>
* Bug fixes from Nicholas for `bios_extract`: <https://browse.libreboot.org/lbmk.git/commit/?id=2e64f6397556b7e6fff8a7a305a5eaa1095acfc1>
* blobutil: support downloading VGA ROM for Nvidia E6400: <https://browse.libreboot.org/lbmk.git/commit/?id=5a197b4ff160a348179a3350af266c6b87a3aa04>

Ongoing development discussion is available, on the Libreboot bug tracker. See:

* <https://codeberg.org/libreboot/lbmk/issues/14>

For more information about the *Nvidia GPU* variants, please review the
following pages (which have been updated, while publishing this news article):

* [Dell Latitude E6400 hardware information](../docs/hardware/e6400.md)
* [Dell Latitude E6400 flashing instructions](../docs/install/e6400.md)

Nouveau(in Linux) currently broken
----------------------------------

Nouveau is the libre driver in Linux, for Nvidia graphics. Nvidia themselves
do not provide binary drivers anymore, for these GPUs. It crashes in Linux,
when you try to start Xorg (Wayland is untested).

If you're booting an Nvidia variant in Linux, boot Linux with
the `nomodeset` kernel option at boot time. This means that graphics are
rendered in software.

Development discussion, for Nvidia variants of E6400, is available here:

<https://codeberg.org/libreboot/lbmk/issues/14>

OpenBSD's Nvidia driver works perfectly
---------------------------------------

OpenBSD 7.3 was tested, on my Nvidia-model E6400, and Xorg works OK with
the `nv` driver.

<img tabindex=1 class="l" style="max-width:35%" src="https://av.libreboot.org/openbsd.jpg" /><span class="f"><img src="https://av.libreboot.org/openbsd.jpg" /></span>

See: <https://www.openbsd.org/>

OpenBSD is a complete free 4.4BSD Unix operating system focused on portability,
security and *code correctness*. It's quite useable for most day to day tasks.

You can find information in Libreboot about BSD operating systems on the
main guide:

* [BSD Operating Systems](../docs/bsd/)

FreeBSD and newer Linux (e.g. Archlinux) untested!
--------------------------------------------------

[Testers needed! Please get in touch!](../docs/maintain/testing.html)

**At the time of writing this post, FreeBSD
and newer Linux have not yet been tested** (I plan to test *Arch Linux*), but
the older Linux/Mesa version in Debian 11.6 works just fine in the Dell BIOS,
and I've confirmed that it uses the exact same Video BIOS Option ROM.

Dell Latitude E6400 ATG model
-----------------------------

[Testers needed! Please get in touch!](../docs/maintain/testing.html)

We also found out about this model; it's another rugged design, assumed to
be the same board as regular E6400, but testing is needed. If it's anything to
go by, this model shares the same service manual as the regular E6400. If you
have this board, please [get in touch](../docs/maintain/testing.md)!

ATG is basically just a thicker chassis. It seems to use the same/similar
heatsink compared to E6400 XFR.

Were you expecting more?

Well, that's all for now.

Stay tuned for further development.
