% March 2023 Libreboot release expected soon
% Leah Rowe
% 16 March 2023

Introduction
============

Development on Libreboot has largely focused on fixing issues since the
December 2022 release, and I very recently updated coreboot revisions (for
x86 boards) to a newer February 2023 revision. The GRUB and SeaBIOS payloads
have also been updated to newer revisions from upstream. A few minor fixes
here and there (e.g. VRAM size on GM45 machines causing instability, heavy
code re-factoring of nvmutil, minor build system tweaks).

The build system in Libreboot has matured quite a bit since the reboot of the
project back in 2021, to the point where much less work on the build system is
needed these days. The focus from now on is going to be on new hardware support
and hardware testing (an ambitious agenda, for which board maintainers will be
needed).

Saturday, 18 March 2023
-----------------------

I'll have more to say on the day of release, via Change Log, but
I'm basically confident that I'll have a new Libreboot release out
the door on ~Saturday, 18 March 2023. This release will be marked *testing*,
because there are still many blockers preventing the declaration of a stable
release, but in practise all of the boards that have pre-built ROMs will
at least boot and be usable.

The current state of Libreboot is that it's ready for a new testing
release, without many major issues, but for one major problem:

Haswell S3 issue (and fix)
==========================

The problem
-----------

The S3 suspend/resume feature on Haswell (ThinkPad T440p/W541) is still
broken. Resuming from suspend results in a cold boot. This is due quirks in
how Intel's MRC blob works (for raminit), coupled with how Libreboot configures
both the IFD and CBFS size in coreboot.

I now know of two possible ways to fix it, and I'm confident that one or both
will be effective:

Fix
---

The Heads project, which is another coreboot distro similar to Libreboot,
recently added T440p support, but they fixed the S3 bug. The relevant
differences in their firmware, compared to Libreboot, is as follows:

* IFD changes, related to read/write bits. Most likely irrelevant for the
purpose of this
* CBFS size on theirs is set to 8MB, and the reasoning is explained here:
  <https://github.com/osresearch/heads/pull/1282#issuecomment-1400634600>

Basically, the `mrc.bin` blob provided for raminit on Haswell (T440p/W541 etc)
caches the coreboot code in ways that Libreboot currently screws up. At the
time of this post, I'm currently adapting the changes from Heads and conducting
tests. I believe their changes will fix it.

However, 8MB CBFS is far from ideal, as Libreboot currently provides much
larger size of ~11.7MB CBFS size (same size as that of the BIOS region, per
rules set in the intel flash descriptor).

Libre MRC replacement
---------------------

Angel Pons (`hell` on coreboot IRC) has been reverse engineering `mrc.bin` on
Haswell platforms. I've learned that they have in fact released a WIP of their
work, and it apparently works well. You can find the patch set here:

<https://review.coreboot.org/c/coreboot/+/64176/5/>

When you follow up on the chain of commits, it ends here:

<https://review.coreboot.org/c/coreboot/+/64198/5>

The MRC blob provides initialisation of the memory controller. Hell's work
implements a libre replacement of it, with source code.

Plan
----

Unrelated to the S3 issue, I'm testing the differing IFD setup from Heads,
but inserted into Libreboot.

Then I'm going to *ignore* the 8MB CBFS config option in Heads, and retain
the current larger CBFS size set in Libreboot. I shall do this, while
merging Angel's patches into Libreboot (coreboot has not yet merged them,
so the patches are under review, but Libreboot has integrated unmerged patches
before, from coreboot).

If the libre raminit works, then I'll re-test the S3 suspend/resume function.
Whether or not that fixes it, I will start building the Libreboot release.
(my first priority, per Libreboot's binary blob reduction policy, is to provide
libre code wherever possible - broken S3 suspend/resume would not be enough to
make me retain the `mrc.bin` blob).

I was hoping to have the release out *today*, but instead I've decided to
announce that today that I'm working on it.

This business with Haswell is the only major issue I've yet to fix in
Libreboot, at least for x86.

*ETA Saturday, 18 March 2023 for release. Libreboot 20230318.*
