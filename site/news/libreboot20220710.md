% Libreboot 20220710 released!
% Leah Rowe
% 10 July 2022

Free your BIOS today!
=====================

Libreboot is free (as in freedom) boot firmware, which initializes the hardware
(e.g. memory controller, CPU, peripherals) in your computer so that software
can run. Libreboot then starts a bootloader to load your operating system. It
replaces the proprietary BIOS/UEFI firmware typically found on a computer.
Libreboot is compatible with specifical computer models that use the Intel/AMD
x86 architecture. Libreboot works well with GNU+Linux and BSD operating systems.

The last Libreboot release, version 20211122, was released on November 22nd
in 2021. *This* new release, Libreboot 20220710, is released today on July
10th, 2022. This is intended to be a *stable* release, with some caveats.

This is a *bug fix* release, relative to 20211122. No new boards or major
features have been added, but several problems that existed in the previous
release have now been fixed.

Build from source
-----------------

*This* release was build-tested on Debian 11. Your mileage may vary, with
other distros. Portability is very much a goal for a future release; in
particular, I want to port the Libreboot build system and everything it uses
to build properly on OpenBSD, but I'm also interested in non-GNU Linux distros
such as Alpine Linux.

Much of the Libreboot build system relies on GNU-specific features, in the
BASH implementation of `sh`.

Work done since the 20211122 release:
-------------------------------------

* Lots and lots of improvements to the documentation. Previous 2021 testing
  releases did not include snapshots of the documentation (which is actually
  the Markdown source files for the website), but this release *does* include
  now a snapshot of the current Libreboot documentation, as per the time of
  release.
* grub.cfg: Many performance improvements, improving the boot speeds
  when using the GNU GRUB payload (courtesy Ferass 'Vitali64' EL HAFIDI with
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

GA-G41M-ES2L works *for me* but jxself on FSF IRC reported video init issues.
If you have this board, and the 2021 releases don't work either, you might
consider using upstream coreboot or the September 2016 Libreboot release.
I will investigate this, and re-include ROMs for this board on the next
release of Libreboot.

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

Planned osboot/Libreboot merger
-------------------------------

*The plans below are a guiding principle, but the details may change, when
or if (most likely when) this decision is implemented.*

In general, more hardware support is always a focus of the Libreboot project.
With this in mind, a fundamental policy change in planned in the next release.

Read the policies of Libreboot and osboot. They differ, but the guiding
philosophy behind them is exactly the same:

* <https://libreboot.org/news/policy.html>
* <https://osboot.org/news/policy.html> (this will redirect to _newpolicy.html_
  on libreboot.org, and the current _policy.html_ will redirect
  to _oldpolicy.html_, on the libreboot site, when the decision is implemented) 

The differences are clear, but they are not entirely irreconcilable. I had
initially started *osboot* to be its own project, but I have concluded for some
time now that this level of separation is inefficient. I've thought of a better
way to run both projects. I initially planned to do an osboot release at the
same time as a new Libreboot release, but this will no longer be done.

*This is the last Libreboot release*, under the current policy. The next
Libreboot release will be conducted under a new policy, that accomodates both
the current Libreboot policy and current osboot policy.

Basically, the differences between lbmk and osbmk are quite minor and osboot
merely adds a few new features for platforms it supports that Libreboot does
(can)not under current policy. This is not to say that the differences are
not substantial, for those parts of osboot that do differ, but the overall
structure and design of both build systems (libreboot and osboot) is exactly
the same, and they're both easily adaptable.

What I want to do is refactor parts of the osboot build system so that you
can pass an option (e.g. environmental variable) at build-time, which will
dictate that any modules downloaded/built, and any ROMs built, will be created
under current Libreboot policy.

Example, Libreboot-style, blobless:

    FSDG= ./build boot roms all

Example, Osboot-style:

    ./build boot roms all

An option in `board.cfg` for each board would specify whether the given board
can actually be built and booted this way, per current Libreboot policy.
Therefore, a version of the current guidelines will still be made available.
The *new* osboot-derived guidelines would be a separate document.

Where `board.cfg` does specify that FSDG is possible, non-FSDG configs can
still be made available (for example: include microcode updates and don't
provide microcode-related mitigations), while also providing FSDG compliant
configs (no microcode updates, and related issues mitigated via patches if
possible, e.g. PECI disable patch to fix SpeedStep on GM45/ICH9M machines).

This would then become the Libreboot build system, and the documentation on
libreboot would integrate everything from osboot too, accomodating this new
policy change. The Libreboot project would therefore have two policies:

* Current one, if building with FSDG option
* Osboot one, if building without FSDG option

FSDG is the FSF guideline that Libreboot currently complies with, and which
this release (Libreboot 20220710) adheres to.

Under this planned change, *two* sets of ROM images would be provided in
the next Libreboot release:

* Limited subset, built based on current Libreboot policy. These sets would
  be similar to what you currently see in Libreboot releases.
* Expanded set, based on current osboot policy

Under that next release, with the change made, both sets of ROM images would
be built from the same source archive.

When this merger is conducted, the <https://osboot.org/> site will shut down
and redirect (HTTP 301) to <https://libreboot.org/>. A new fusion of Libreboot
and osboot will be born, continuing on *libreboot.org*.

This would then open up the Libreboot project to support more hardware, far
more than it currently supports. The documentation would also be greatly
improved, to more thoroughly specify what issues exist (if any) on a given
board, as per *current Libreboot blob policy* and from an OSHW perspective.

The reason for this planned merger is pragmatic: I want to help more people
to increase the amount of freedom they have, and most hardware currently
supported by Libreboot is nearly impossible to find these days. In other words,
it's a choice between abandoning Libreboot and focusing only on osboot, which
itself is a new project that has to completely establish itself again, or to
instead continue using the Libreboot name, and implementing this newly
pragmatic decision as a means of *continuity*.

Even if more hardware is added to Libreboot under the current policy, I think
this new change of direction is fundamentally *good*, because Libreboot is
mainly about making coreboot as easy to use as possible. My feelings about
this are already written in the current osboot policy.

I believe the Libreboot project is in a position to help people regardless, by
focusing on the wider set of supported coreboot hardware while still catering
to the existing Libreboot users (precisely the reason why the merger is
planned, in exactly the manner as described above).
