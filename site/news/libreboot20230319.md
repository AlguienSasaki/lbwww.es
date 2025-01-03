% Libreboot 20230319 released!
% Leah Rowe
% 19 March 2023

**UPDATE: Bugs in this were fixed, and a bugfix release is now available.
See: [Libreboot 20230413 release announcement of 13
April 2023](libreboot20230413.md) - the `t440pmrc_12mb` and `w541mrc_12mb`
images have been re-added, in the new release.**

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

Introduction
============

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on ARM-based chromebooks supported (U-Boot bootloader, instead of Google's
depthcharge bootloader). On x86 machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. It provides an automated build system for the configuration and
installation of coreboot ROM images, making coreboot easier to use for
non-technical people. You can find the list of supported hardware in the
Libreboot documentation.

The last Libreboot release, version 20221214, was released on 14 December
in 2022. *This* new release, Libreboot 20230319, is released today on
March 19th, 2023.

This is marked as a *testing* release. Not all ROM image configurations have
been provided pre-compiled; specifically, `daisy` and `veyron` chromebook
boards are not available pre-compiled, but the other boards are. A few new
boards have been added, in addition to several fixes and feature additions.

Build from source
-----------------

*This* release was build-tested on Debian *Sid*, as of 18 March 2023. Your
mileage may vary, with other distros. Refer to Libreboot documentation.

Work done since last release
============================

For more detailed change logs, look at the Git repository. This is
a summary of changes.

Brief overview of changes
-----------------

I've been very busy these last couple months, so there have been less changes
in Libreboot lately. I'm expecting to have a lot more time throughout the
coming summer, which I plan to make full use of for the next Libreboot
release.

The changes can be summarised, thus:

* **LIBRE** raminit code now available, on Haswell boards (ThinkPad T440p and
  ThinkPad W541). This is using patches from Angel Pons (hell on coreboot IRC),
  that are currently still in review on coreboot master. The *old* configs
  that use `mrc.bin` for raminit are still available aswell, so this release
  contains ROMs with libre raminit *and* ROMs with vendor raminit. The reasons
  are explained below.
* **FIXED S3 suspend/resume on Haswell (T440p/W541)** - but only on configs
  that use `mrc.bin`. The images with libre MRC still have broken S3. S3
  suspend/resume is what people refer to as *sleep mode* and *wake from sleep*.
* **Force console display mode in GRUB**: This corresponds to the
  setting `GRUB_TERMINAL=console` that you might find in `/etc/default/grub`
  on a Linux machine. In Libreboot, switching VGA/text modes is unsupported,
  it's stuck in one mode until Linux/BSD Kernel Mode Setting takes over.
  With this change to GRUB, GRUB's VGA mode switching is disabled. This might
  make a few Linux distro installer menus work a bit nicer.
* **New ports:** Lenovo T530 and W530 thinkpads added.
* **Bump coreboot revision**: February 2023 revision for most x86 boards,
  including Haswell (T440p/W541) when `mrc.bin` is in use; Haswell (T440p/W541)
  setups that don't use `mrc.bin` (and instead use Angel`s libre replacement
  code), it's currently based on coreboot from mid-2022 because that's what
  Angel's as yet unmerged patches are based on (relative to coreboot's
  master branch)
* **Bump GRUB revision:** February 2023 revision now, on all boards.
* **Bump SeaBIOS revision:** February 2023 revision now, on all
  boards.
* `grub.cfg`: 5 seconds boot delay, instead of 10 seconds.
* GM45 thinkpads: default to 256MB VRAM instead, which is more stable than
  the previous setting of 352MB. Many reports from users indicated performance
  issues when the 352MB config was used.
* Stricter handling of exit status in lbmk. More errors result in a hard
  exit than before, preventing the build process from erroneously continuing.
* Fixed fault checks in the Libreboot build system, when checking which
  coreboot utilities are needed (e.g. cbfstool, ifdtool)
* util/nvmutil: Massively re-factored the codebase, making it more efficient
  and *correct*.

NOTE: T440p/W541 images with `mrc.bin` must be compiled from source. They were
removed from the release, due to a bug reported in scripts used for preparing
them post-build, when users install them. The bug was fixed in a revision
after the release. If you still have those ROM images from prior to deletion,
and want to use them, you should apply the following patch to your archive
of the release, or just use latest `lbmk` from the Git repository; see:

<https://browse.libreboot.org/lbmk.git/commit/?id=da6bf57a3f57f2625a4903cafb5cfd10ea4a1dae>

Pre-compiled ROM images will for the `t440pmrc_12mb` and `w541mrc_12mb` targets
will be available in the next release; the `t440p_12mb` and `w541_12mb` images
are still available in this release, pre-compiled.

REMARK: libre raminit on Haswell
--------------------------------

Upon testing, I've discovered that the libre code has the following problems:

* I haven't gotten S3 suspend/resume working properly on that yet.
* Broken USB device detection in GRUB.
* SeaBIOS still works, for USB devices.

Therefore, the *libre* MRC setup in this release, for T440p and W541 thinkpads,
only provides SeaBIOS payload (booting in text mode, implying use of a
bootloader that supports this, and if wanting xorg/wayland, a kernel that does
kernel mode setting, which is most BSD/Linux setups these days).

In this release, and in the build system, the following targets are defined:

* `t440p_12mb`: libre raminit code used (reverse engineered replacement
  of `mrc.bin`)
* `w541_12mb`: ditto (libre raminit code)
* `t440pmrc_12mb`: vendor `mrc.bin` used for raminit. GRUB and SeaBIOS payloads
  both supported.
* `w541mrc_12mb`: vendor `mrc.bin` used for raminit. GRUB and SeaBIOS payloads
  both supported.

The libre raminit comes from this patchset:

<https://review.coreboot.org/c/coreboot/+/64198/5>

The MRC vendor file (and Angel's replacement code) don't just do raminit, they
handle a few other init tasks aswell, including USB host controller.

New boards, x86
----------

* Lenovo ThinkPad W530
* Lenovo ThinkPad T530

I bought these machines, which I've not added to the release but plan to
add for the next release:

* HP EliteBook 8560w (<https://review.coreboot.org/c/coreboot/+/39398>)
* Lenovo G505S (<http://dangerousprototypes.com/docs/Lenovo_G505S_hacking>)
* Dell Latitude E6400 (now merged in coreboot master. It's an ICH9M machine
  but with DDR2 raminit)

^ I would have put these in today's release, but didn't have time, and wanted
to get this release done today.

Removed boards
--------------

* asus p2b\_ls/p3b\_f - they didn't boot properly in pcbox, and the real
  hardware is basically useless / impossible to find

lbmk Git changes
------------

The precise list of commits in `lbmk.git` since the last release, is as
follows:

```
* 07b6bb3d - build/release: handle nvmutil (12 hours ago) <Leah Rowe>
* 653810b8 - fix bug: me not being downloaded on some boards (12 hours ago) <Leah Rowe>
* 2bb63d85 - new board: lenovo/w530 (12 hours ago) <Leah Rowe>
* 896e9065 - new board: lenovo/t530 (13 hours ago) <Leah Rowe>
* cffa5679 - haswell (lenovo t440p/w541): fix S3 suspend/resume (14 hours ago) <Leah Rowe>
* be3d7b7e - haswell: re-add mrc.bin in separate board configs (22 hours ago) <Leah Rowe>
* bdc39ffc - haswell: only use txtmod seabios configuration (25 hours ago) <Leah Rowe>
* df6b9e28 - remove t440p_12mb_cbfs4mb (retain t440_12mb) (25 hours ago) <Leah Rowe>
* 04f1fe17 - remove x220_16mb (x220 with 16MB flash) (29 hours ago) <Leah Rowe>
* 548872ce - haswell boards: use libre mrc.bin replacement (2 days ago) <Leah Rowe>
* a942bd65 - move download/gitmodule script to root directory (2 days ago) <Leah Rowe>
* 59540530 - nuke p2b_ls/p3b_f boards (2 days ago) <Leah Rowe>
* ebd9ec96 - debian/ubuntu dependencies scripts: add gettext (3 days ago) <Leah Rowe>
* f9e20b8a - util/nvmutil: optimise rhex() further (13 days ago) <Leah Rowe>
* f04855c2 - fix flashprog download error (13 days ago) <Leah Rowe>
* e2945f02 - payload/grub: force terminal_output to console (2 weeks ago) <Leah Rowe>
* 909d3b31 - grub.cfg: set default timeout to 5 seconds (2 weeks ago) <Leah Rowe>
* 544737c8 - scripts: build cbutils, not specific utils (2 weeks ago) <Leah Rowe>
* 9398ad08 - also fix data.vbt path for lenovo/w541 (2 weeks ago) <Leah Rowe>
* d2465e82 - Fix CONFIG_INTEL_GMA_VBT_FILE for the t440p_12mb config (2 weeks ago) <Konstantinos Koukopoulos>
* 0e34d199 - update debian dependencies (for sid) (2 weeks ago) <Leah Rowe>
* a5aa5bca - ICH9M: default to 256MB VRAM, not 352MB (2 weeks ago) <Leah Rowe>
* 6421af5d - bump seabios revision (4 weeks ago) <Leah Rowe>
* aba6307d - bump grub revision (4 weeks ago) <Leah Rowe>
* 36982ab5 - fix bad ifdtool patch from earlier commit (4 weeks ago) <Leah Rowe>
* 3857b4b6 - build/dependencies/debian: add python3 dependency (4 weeks ago) <Leah Rowe>
* dac9ea86 - build/boot/roms: fail when build cbutils fails (4 weeks ago) <Leah Rowe>
* 0d0f6cf3 - coreboot: update revision of cbtree "default" (4 weeks ago) <Leah Rowe>
*   dc1fedf9 - Merge branch 'uboot-v2023.01' of alpernebbi/lbmk into master (4 weeks ago) <Leah Rowe>
|\  
| * 7932d5fa - u-boot: Disable environment storage (5 weeks ago) <Alper Nebi Yasak>
| * 8d57468e - u-boot: Update to v2023.01 (5 weeks ago) <Alper Nebi Yasak>
|/  
* 6b4a14ce - util/nvmutil: tidy up variable declarations (7 weeks ago) <Leah Rowe>
* 031a0b55 - util/nvmutil: setWord(): declare variables first (7 weeks ago) <Leah Rowe>
* 257eedca - util/nvmutil: reset errno if any write attempted (7 weeks ago) <Leah Rowe>
* adc76e38 - util/nvmutil: do not write non-changes to disk (7 weeks ago) <Leah Rowe>
* 3e150bf3 - util/nvmutil: cmd_swap(): write sequentually (7 weeks ago) <Leah Rowe>
* 7e3a7355 - util/nvmutil: don't use malloc() (7 weeks ago) <Leah Rowe>
* a924d43b - util/nvmutil: fix clang build errors (7 weeks ago) <Leah Rowe>
* c822033b - util/nvmutil: simplify rhex() (7 weeks ago) <Leah Rowe>
* 0f485245 - util/nvmutil: use gbe[] in word() and setword() (7 weeks ago) <Leah Rowe>
* b1186968 - util/nvmutil: code cleanup (7 weeks ago) <Leah Rowe>
* 7a986497 - util/nvmutil: call pledge() earlier, in main() (7 weeks ago) <Leah Rowe>
* bb6fe263 - util/nvmutil: remove unused #define (7 weeks ago) <Leah Rowe>
* 5a5a8662 - util/nvmutil: optimised disk reads (7 weeks ago) <Leah Rowe>
* 24d56456 - util/nvmutil: optimise cmd_swap() (7 weeks ago) <Leah Rowe>
* ef84329a - util/nvmutil: optimise rhex() for speed (7 weeks ago) <Leah Rowe>
* 88a51531 - util/nvmutil: code cleanup in rhex() (7 weeks ago) <Leah Rowe>
* ac1cab28 - x230edp_12mb: Correct the path to data.vbt (7 weeks ago) <Alexei Sorokin>
* afc80b89 - util/nvmutil: update copyright years (9 weeks ago) <Leah Rowe>
* 8242dca5 - util/nvmutil: limit bytes written per command (9 weeks ago) <Leah Rowe>
* e398331b - util/nvmutil: make writeGbeFile more readable (9 weeks ago) <Leah Rowe>
* 8dea350a - util/nvmutil: only write parts that are modified (9 weeks ago) <Leah Rowe>
* d0fa08d5 - blobs/inject: fix wrong nvmutil path for make (10 weeks ago) <Leah Rowe>
*   e8072934 - Merge branch 'veyron-uboot-dmreset' of alpernebbi/lbmk into master (10 weeks ago) <Leah Rowe>
|\  
| * e11650c3 - u-boot: Enable DM_RESET for veyron boards (3 months ago) <Alper Nebi Yasak>
* |   6b104542 - Merge branch 'peach-uboot-usbehci' of alpernebbi/lbmk into master (10 weeks ago) <Leah Rowe>
|\ \  
| |/  
|/|   
| * 80bf54b2 - u-boot: Enable USB_EHCI_EXYNOS on peach boards (3 months ago) <Alper Nebi Yasak>
|/  
* 7f5dfebf - Do not rely on bashisms and behaviour undefined by the POSIX specification. Part 2 (3 months ago) <Ferass 'Vitali64' EL HAFIDI>
* f7870446 - Do not rely on bashisms and behaviour undefined by the POSIX specification. (3 months ago) <Ferass 'Vitali64' EL HAFIDI>
* d45b2e70 - util/nvmutil: use err() more consistently (3 months ago) <Leah Rowe>
* d726b16f - util/nvmutil: more robust pointer handling (3 months ago) <Leah Rowe>
* 448ee510 - util/nvmutil: optimise cmd_swap() further (3 months ago) <Leah Rowe>
* effcb942 - util/nvmutil: greatly optimise cmd_copy() (3 months ago) <Leah Rowe>
* 6e5828e4 - util/nvmutil: greatly optimise cmd_swap() (3 months ago) <Leah Rowe>
* 7aafc62b - scripts/blobs/inject: fix bad cbfstool build check (3 months ago) <Leah Rowe>
* 6ebd178f - util/nvmutil: simplified error handling in rhex() (3 months ago) <Leah Rowe>
* 04da953c - util/nvmutil: return errno when calling err() (3 months ago) <Leah Rowe>
* 00187811 - util/nvmutil: exit non-zero if close() fails (3 months ago) <Leah Rowe>
```

Major works planned
===================

In general, you should also check the issue tracker to find other notes.
There is always more work to do, to improve Libreboot.

Linux distro in flash
---------------------

STILL ON TODO SINCE LAST RELEASE.

This is another project that has been on hold for a while. The issue
has been that I need a decent userland project. I've looked at many
different userlands and since late June in 2022, decided to make
my own. I want a BusyBox-like solution, but based on code from OpenBSD,
ported to run under Linux with musl libc.

I want this distro to provide a kexec bootloader in flash, similar to Heads,
and I also want it to use apk-tools, pointing at Alpine Linux repositories
so as to allow any number of packages to be downloaded. It could also provide
lots of utils in general, to be a live *rescue system* of sorts. Linux system
in flash, that can bootstrap other systems.

About a week before this release, I actually started on the project for real,
after having many false starts. I've forked a project called `lobase` which
already ports OpenBSD's userland utilities *to glibc* under Linux, and it's
as of today about 5 years outdated based on OpenBSD 6.3.

I've ported these utilities directly from OpenBSD 7.2, in my local fork of
lobase, superimposing the new code on top of the old and adapting according
to musl libc (lobase is full of hacks for glibc that I don't need):

`mail`, `cat`, `ls`, `head`, `rcs`, `hexdump`, `whois` and `time`

I've been working on this in a dedicated Alpine Linux virtual machine,
currently on release 3.17 of Alpine Linux. Alpine is an ideal test distro for
such development, because it already uses musl libc and has *libressl*
available in aports.

I don't have enough to release yet, but when I have a release ready, I will
upload it to a Git repository. When the userland port is fully complete,
I shall then base off of Alpine Linux abuild+aports build system
infrastructure to provide small base images. It will be similar to the Heads
project, but built separately and not specifically targeted at Libreboot,
but in general to any coreboot setup, on supported hardware. It won't be a
general purpose distro, but I would at that point submit my userland port to
Alpine, proposing it as a replacement of their busybox package in base.

Unlike Heads, I don't plan yet to make this a direct coreboot payload.
Instead, it'll be a standalone image loaded into CBFS, and chainloaded via
the GRUB or SeaBIOS payloads, which are both capable of executing ELF binaries
from the flash.

Lobase, which my development is forked from, can be found here (archived):
<https://github.com/Duncaen/lobase>

I've been re-using lobase's build system, adapting newer code from OpenBSD.
It's a lot of work, but I'm hopeful I can have this ready before the next
Libreboot release.

Re-factor and optimize GRUB
---------------------------

STILL ON TODO SINCE LAST RELEASE.

GRUB is full of unused bloat that almost nobody uses, yet is in the current
Libreboot builds. It's been on TODO for some time, but work has not yet
begun on this project. My efforts are currently focused on the Linux distro.

What I want is a fork of GRUB, optimized to run on bare metal as a coreboot
payload, on x86 and ARM platforms.

I have an update since the last release. Paul Menzel of coreboot *has* made
GRUB modules more configurable, making it possible to reduce the size of the
payload. His patch is not yet used in Libreboot (not in this release, anyway),
but the patch in GRUB is:

```
commit 6c5ee45548fcb65d7921c9fca5867b256f9a48ad
Author: Paul Menzel <pmenzel@molgen.mpg.de>
Date:   Thu Mar 7 12:16:06 2019 +0100
    Makefile: Allow to set file systems modules for default_payload.elf
```

I'm going to play around with this when I get the time. Even with this
modification, GRUB is still full of code that Libreboot will never use.
A *GRUB Valhalla Rampage* is still in order!
