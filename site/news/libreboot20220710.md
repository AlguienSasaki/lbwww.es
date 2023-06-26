% Libreboot 20220710 released!
% Leah Rowe
% 10 July 2022

Free your BIOS today!
=====================

Libreboot is [free](https://writefreesoftware.org/) (as in freedom) boot
firmware, which initializes the hardware
(e.g. memory controller, CPU, peripherals) in your computer so that software
can run. Libreboot then starts a bootloader to load your operating system. It
replaces the proprietary BIOS/UEFI firmware typically found on a computer.
Libreboot is compatible with specifical computer models that use the Intel/AMD
x86 architecture. Libreboot works well with Linux and BSD operating systems.

The last Libreboot release, version 20211122, was released on November 22nd
in 2021. *This* new release, Libreboot 20220710, is released today on July
10th, 2022. This is intended to be a *stable* release, with some caveats.

This is a *bug fix* release, relative to 20211122. No new boards or major
features have been added, but several problems that existed in the previous
release have now been fixed.

Build from source
-----------------

*This* release was build-tested on Debian 11. Your mileage may vary, with
other distros.

Work done since the 20211122 release:
-------------------------------------

* Lots and lots of improvements to the documentation. Previous 2021 testing
  releases did not include snapshots of the documentation (which is actually
  the Markdown source files for the website), but this release *does* include
  now a snapshot of the current Libreboot documentation, as per the time of
  release.
* grub.cfg: Many performance improvements, improving the boot speeds
  when using the GRUB payload (courtesy Ferass 'Vitali64' EL HAFIDI with
  additional improvements made by Leah Rowe)
* GM45/ICH9M laptops: Disable PECI in coreboot, to work around a microcode bug
  causing SpeedStep (and possibly other CPU features) to fail.
* Do not treat warnings as errors when building flashrom (fixes building on
  newer versions of GCC).
* Macbook2,1: 16MB configurations now available (you must first upgrade the
  SPI flash)
* Build system improvement: automated scripts for modifying coreboot configs.
* Disable (by default) serial output on all boards, to prevent boot speed
  issues.
* grub.cfg: Actually enable USB keyboards, explicitly (works around bug seen
  on some laptops, when using the GRUB payload).
* Coreboot configs: Do not enable wifi during early init (security liability)
* Preliminary u-boot integration; not used in any boards yet, but future
  full integration is planned, for several ARM platforms. U-boot is not
  included in the release archives, but logic does exist in the build system.
  Courtesy of Denis 'GNUtoo' Carikli.
* Scripts in lbmk: improved help output, courtesy of Denis 'GNUtoo' Carikli.
* scripts: process git versions when lbmk is a worktree or submodule. Courtesy
  John Doe (cool guy)
* Updated to newer flashrom, in the build system
* Perform silentoldconfig in seabios before full make. This fixes a race
  condition when rebuilding SeaBIOS with a high CPU count, resulting in failure
  with the error message (fix courtesy of John Doe):

	cc1: fatal error: can't open 'out/src/asm-offsets.s' for writing: No such file or directory

* lbmk: Specifically call python3, when python3 is to be used instead of 2.
* lbmk: Preliminary fix for git credentials check. Set a placeholder name/email
  if one is not set.

Caveats
-------

Due to reported issues by users, these boards do not have ROM images
available in the Libreboot 20220710 release:

* KGPE-D16 ROM images not included
* ditto KCMA-D8
* ditto GA-G41M-ES2L

The boards listed above can still be compiled, from the source code archive
in this release and from the Libreboot git repository; additionally, ROM images
are provided for these in the previous release. D8/D16 continue to have raminit
issues; for now, use the 2021 releases. The next Libreboot release will
merge newer patches that are available for this board, improving raminit
reliability (among other things); that new release will, when available, have
D16 ROMs included.

All other boards are reasonably stable, and shouldn't provide any issues (no
major issues reported, and/or non-blocking issues only).

Planned future work
===================

In general, you should also check the issue tracker to find other notes.
There is always more work to do, to improve Libreboot.

Support for non-x86 platforms
-----------------------------

This is still on hold, but will be done as part of a future release.
The coreboot firmware does support other platforms.

Linux distro in flash
---------------------

This is another project that has been on hold for a while. The issue
has been that I need a decent userland project. I've looked at many
different userlands and recently (as of late June) decided to make
my own. I want a BusyBox-like solution, but based on code from OpenBSD,
ported to run under Linux with musl libc.

I want this distro to provide a kexec bootloader in flash, similar to Heads,
and I also want it to use apk-tools, pointing at Alpine Linux repositories
so as to allow any number of packages to be downloaded. It could also provide
lots of utils in general, to be a live *rescue system* of sorts. Linux system
in flash, that can bootstrap other systems.

Re-factor and optimize GRUB
---------------------------

GRUB is full of unused bloat that almost nobody uses, yet is in the current
Libreboot builds. It's been on TODO for some time, but work has not yet
begun on this project. My efforts are currently focused on the Linux distro.

What I want is a fork of GRUB, optimized to run on bare metal as a coreboot
payload, on x86 and ARM platforms.
