---
title: Binary blob policy
x-toc-enable: true
...

This article was written by Leah Rowe, the founder and current lead developer
of Libreboot.

Introduction
============

Libreboot intentionally *de-blobs* coreboot, which is to say that it does not
include binary blobs. The coreboot software otherwise requires binary blobs on
most systems that it has support for. Libreboot's version of coreboot is
entirely *free*, on its consequently reduced set of supported mainboards.

Libreboot is designed to comply with the Free Software Foundation's
[Respects Your Freedom criteria](https://ryf.fsf.org/about/criteria) and
the [GNU Free System Distribution Guidelines (GNU FSDG)](https://www.gnu.org/distros/free-system-distribution-guidelines.en.html),
ensuring that it is entirely [Free Software](https://www.gnu.org/philosophy/free-sw.html).

It was decided that a formal policy should be written, because there is quite
a bit of nuance that would otherwise not be covered. Libreboot's policies in
this regard were previously ill defined.

Background information
======================

Libreboot concerns itself only with what goes in the main boot flash IC, but
there are other pieces of firmware to take into consideration, as covered
in the [Libreboot FAQ](faq.md#what-other-firmware-exists-outside-of-libreboot).

Most critical of these are:

* Embedded controller firmware
* HDD/SSD firmware
* Intel Management Engine / AMD PSP firmware

Specific binary blobs are also problematic, on most coreboot systems, but they
differ per machine. Libreboot *excludes* binary blobs in releases, so it only
supports a handful of machines from coreboot.

For information about Intel Management Engine and AMD PSP, refer to the FAQ.

So what *is* Libreboot's policy?
================================

Libreboot follows a very conservative and *light touch* approach, when it comes
to deblobbing coreboot. It is a very *correct* approach.

Libreboot only excludes *software* binary blobs, plus CPU microcode updates,
completely in line with FSF policy. *In practise, it is mostly microcode
updates that Libreboot's build system deletes, along with coreboot Git history
so that no traces remain of old revisions; older revisions had many blobs in
the main repository, but modern coreboot moved almost all of them to third
party submodule repositories.*.

*Non-software* blobs are permitted, so long as they are in an easily understood
and/or well-documented format. For example, DDR training data is permitted
(patterns used during memory controller initialization, specifically training,
where the precise timings for the RAM are brute-forced); this is not software.

SPD data stored in the coreboot Git repository is in all cases some format
that's simply more efficient to store as a binary, in a format that is in fact
known/understood (see: coreboot source code and data sheets); in many cases,
there's only *one* correct way to write such data, making even the question of
copyright a moot point. Data is data, and code is code; the two are *separate*.

Non-software blobs must be redistributable under a free license, and must not
be encumbered by DRM, or they will not be included in Libreboot.

Logic (in coreboot) for *loading or executing* binary blobs should not
be removed/disabled. Libreboot merely *excludes* the blobs themselves. Most
of the blobs that Libreboot removes (when downloading coreboot, in the build
system) are CPU microcode updates; Libreboot leaves the code for loading
microcode updates intact, and you can in fact insert microcode updates into
your ROM image. This behaviour is intentional, and must not be removed. The
only job Libreboot has is to not *distribute* those blobs itself!

*That's all*. Furthermore, Libreboot must only support systems where *all* of
the main boot flash can be free. For example, ivybridge and sandybridge intel
platforms are completely libre in coreboot, but you still need neutered Intel
ME firmware in the flash, making those machines unsuitable for Libreboot.

Other firmware, such as Embedded Controller firmware, is currently outside the
scope of the Libreboot project, but not due to lack of desire; rather, these
are not yet possible on most supported or otherwise capable platforms, at least
not with free software. Other examples of firmware outside of the main boot
flash is covered in the Libreboot FAQ.

Problems with RYF criteria
==========================

You can read those guidelines by following these hyperlinks:

* [GNU Free System Distribution Guidelines (GNU FSDG)](https://www.gnu.org/distros/free-system-distribution-guidelines.en.html)
* [FSF Respects Your Freedom (RYF) guidelines](https://ryf.fsf.org/about/criteria)

The FSF RYF guidelines state the following:

*"However, there is one exception for secondary embedded processors. The exception applies to software delivered inside auxiliary and low-level processors and FPGAs, within which software installation is not intended after the user obtains the product. This can include, for instance, microcode inside a processor, firmware built into an I/O device, or the gate pattern of an FPGA. The software in such secondary processors does not count as product software."*

This is absolute pure nonsense, and should be rejected on ideological grounds.
The rest of libreboot's policy and overall ideology expressed, in this article,
will be based largely on that rejection. The term *product software* is
completely asinine; software is software, and software should always be *free*.
Instead of making such exceptions, more hardware should be encouraged, with
help given to provide as much freedom as possible, while providing education
to users about any pitfalls they may encounter, and encourage freedom at all
levels. When an organisation like the FSF makes such bold exceptions as above,
it sends the wrong message, by telling people essentially to sweep these other
problems under the rug, just because they involve software that happens to run
on a "secondary processor". If the software is possible to update by the user,
then it should be free, regardless of whether the manufacturer *intended* for
it to be upgraded or not. Where it really *isn't* possible to update such
software, proprietary or not, advice should be given to that effect. Education
is important, and the FSF's criteria actively discourages such education; it
creates a false hope that everything is great and wonderful, just because the
software on one arbitrary level is all free.

This view of the FSF's, as expressed in the quoted paragraph, assumes that
there is primarily *one* main processor controlling your system. On many
modern computers, this is *no longer true*.

Free *software* does not exist in a vacuum, but we had less freedom in the
past, especially when it came to hardware, so *software* was our primary focus.

[The four freedoms are absolute](https://www.gnu.org/philosophy/free-sw.html),
but there is a lot of nuance when it comes to *boot firmware*, nuance which is
largely non-existent outside of firmware development, or kernel development.
Most typical application/system software is high level and portable, but boot
firmware has to be written for each specific machine, and due to the way
hardware works, there are many trade-offs made, including by the FSF when
defining what standards should apply *in practise*. Different lines in different
sands. Different *battle grounds*.

The fact that almost nobody talks about the EC firmware is *because* of the
Respects Your Freedom certification. In reality, the EC firmware is crucial
to user freedom, and ought to be free, but it is completely disregarded by
the FSF as *part of the hardware*. This is wrong, and the FSF should actively
actively encourage people to free it, on every laptop!

Other firmware currently outside the reach of the Libreboot project are covered
in the Libreboot FAQ. For example, HDD/SSD firmware is covered in the FAQ.
Again, completely disregarded and shrugged off by the FSF.

The Libreboot project will not hide or overlook these issues, because they are
indeed critical, but again, currently outside the scope of what lbmk does.
At the moment, lbmk concerns itself just with coreboot, but this ought to
change in the future.

More detailed insight about microcode
=====================================

To be clear: it is preferable that microcode be free. The microcode on Intel
and AMD systems *are* non-free. Facts and feelings rarely coincide; the
purpose of this section is to spread *facts*.

Not including CPU microcode updates is an absolute disaster for system
stability and security, and yet, this is one of Libreboot's key policies, to
comply with FSF criteria.

Making matters worse, that very same text quoted from the FSF RYF criteria in
fact specifically mentions microcode. Quoted again for posterity:

*"However, there is one exception for secondary embedded processors. The
exception applies to software delivered inside auxiliary and low-level
processors and FPGAs, within which software installation is not intended after
the user obtains the product. This can include, for instance, microcode inside
a processor, firmware built into an I/O device, or the gate pattern of an FPGA.
The software in such secondary processors does not count as product software."*

Here, it is discussing the microcode that is burned into *mask ROM* on the CPU
itself. It is simultaneously not giving the OK for microcode *updates* supplied
by either coreboot or the Linux kernel; according to the FSF, these are an
attack on your freedom, but the older, buggier microcode burned into ROM is OK.
This is absolutely inconsistent.

The CPU already has microcode burned into mask ROM. The microcode configures
logic gates in the CPU, to implement an instruction set, via special *decoders*
which are fixed-function; it is not possible, for example, to implement a RISCV
ISA on an otherwise x86 processor. It is only possible for the microcode to
implement x86, or *broken* x86, and the default microcode is almost always
*broken x86* on Intel/AMD CPUs; it is inevitable, due to the complexity of
these processors.

The basis of the FSF's disagreement about microcode *updates* is that they do
believe otherwise; Stallman himself expressed such ignorance to me, in a recent
email conversation I had with him, as of January 2nd, 2022. The FSF believes
that these x86 microcode updates (on Intel/AMD) allow you to completely create
a new CPU that is fundamentally different than x86. This is not true. It is also
not true that *all* instructions in x86 ISA are implemented with microcode. In
some cases, hardcoded circuitry is used! The microcode updates are more like
tiny one liner patches here and there in a git repository, by way of analogy.
To once again get in the head-space of the FSF: these updates cannot do the CPU
equivalent of re-factoring an entire codebase. They are *hot fixes*, nothing
more!

These processors provide a way to supply microcode *updates*. These updates
are volatile, and consequently must be applied during every boot cycle. The
updates fix stability/reliability/security bugs, and their *absence*
is *technically incorrect*, but Libreboot excludes them anyway, because that is
FSF policy. Examples of where these updates fix bugs: on ASUS KCMA-D8/KGPE-D16
and ThinkPad X200/T400/T500/W500/X200T/X200/R500/X301, the updates make
hardware-based virtualization (via `kvm`) completely stable, where it would
otherwise lead to a kernel panic. They allow those same thinkpads to be run with
high CPU usage and I/O (RAM usage), without crashing (otherwise, it's very
likely to encounter a kernel panic caused by a
[Machine Check Exception](faq.html#machine-check-exceptions-on-some-montevina-penryn-cpu-laptops)).

Not including these updates will result in an unstable/undefined state. Intel
themselves define which bugs affect which CPUs, and they define workarounds, or
provide fixes in microcode. Based on this, software such as the Linux kernel
can work around those bugs/quirks. Also, upstream versions of the Linux kernel
can update the microcode at boot time (however, it is recommend still to do it
from coreboot, for more stable memory controller initialization or “raminit”).
Similar can be said about AMD CPUs.

Here are some examples of where lack of microcode updates affected Libreboot,
forcing Libreboot to work around changes made upstream in coreboot, changes
that were *good* and made coreboot behave in a more standards-compliant manner
as per Intel specifications. Libreboot had to *break* coreboot to retain
certain other functionalities, on some GM45/ICH9M thinkpads:

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0012-fix-speedstep-on-x200-t400-Revert-cpu-intel-model_10.patch?id=9938fa14b1bf54db37c0c18bdfec051cae41448e>

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0018-Revert-cpu-intel-Configure-IA32_FEATURE_CONTROL-for-.patch?id=4b7be665968b67463ec36b9afc7e8736be0c9b51>

These patches revert *bug fixes* in coreboot, fixes that happen to break other
functionality but only when microcode updates are excluded. The most
technically correct solution is to *not* apply the above patches, and instead
supply microcode updates!

Pick your poison. Libreboot does not disable the mechanism in coreboot to load
these updates. At boot time, coreboot can supply such updates to the CPU, if
present in CBFS. Libreboot merely excludes them, but you can add them to your
Libreboot ROM image. A fork of Libreboot, named osboot, includes them by
default; it does this, even on libreboot-compatible hardware. Not adding the
updates is *irresponsible*, but a promise was made to the FSF back in 2013
when the Libreboot project started, precisely that it would not add microcode
to ROM images by default. It is Libreboot's policy to keep that promise,
despite the *obvious* flaw of that policy.

More info about osboot is available on <https://osboot.org/> - osboot's policy
is the same as Libreboot, except that it does *not* delete blobs; the goal is
still software freedom, but it provides those users who are not willing/able
to use libreboot hardware to otherwise still have some freedoms compared to
otherwise fully proprietary *vendor* firmware. osboot and libreboot are two
sides of a coin; libreboot is the "light", and osboot is the dark side. Both
projects are maintained and were founded by Leah Rowe.

The osboot firmware is far superior to Libreboot, in terms of reliability, due
to the presence of microcode updates in the firmware, and with zero practical
change to your freedom, on libreboot-compatible hardware. However, I will say:

People have been using Libreboot for years, on these machines, and most people
don't really have *that* many issues, most of the time. My opposition to FSF's
microcode policy is out of principle. *Logical*, common sense principle. I
simply cannot compute that microcode updates are an attack on your freedom,
because:

Microcode updates are not an attack on your freedom. The FSF's opposition to
these updates is both symbolic and *ignorant*; it is ultimately futile, but I
digress.

**I will continue to develop Libreboot and osboot, in parallel.**

Other considerations
====================

Also not covered strictly by Libreboot: OSHW and Right To Repair. Freedom at
the silicon level would however be amazing, and efforts already exist; for
example, look at the RISCV ISA (in practise, actual fabrication is still
proprietary and not under your control, but RISCV is a completely free CPU
design that companies can use, instead of having to use proprietary ARM/x86 and
so on). Similarly, Right To Repair (ability to repair your own device, which
implies free access to schematics and diagrams) is critical, for the same
reason that Free Software (Right To Hack) is critical!

OSHW and Right To Repair are not covered at all by RYF (FSF's Respects Your
Freedom criteria), the criteria which Libreboot was created to comply with.
RYF also makes several concessions that are ultimately damaging, such as
the *software as circuitry* policy which is, frankly, nonsensical. ROM is still
software. There was a time when the FSF didn't consider BIOS software a freedom
issue, just because it was burned onto a mask ROM instead of *flashed*; those
FSF policies ignore the fact that, with adequate soldering skills, it is trivial
to replace stand-alone mask ROM ICs with compatible flash memory.

Conclusion
==========

Compromise and nuance is the name of the game, even if you're the FSF. It is
completely unavoidable, but there are some who try to deny this fact and
pretend like things are as they'd prefer them to be, rather than how they
actually are in the real world.

Facts and *feelings* are usually very different things, and contradictory.

RYF isn't *wrong* per se, just flawed. It is correct in some ways and if
complied with, the result *does* give many freedoms to the user, but RYF
completely disregards many things that are now possible, including freedoms at
the hardware level (the RYF criteria only covers *software*). Those guidelines
are written with assumptions that were still true in the 1990s, but the world
has since evolved. As of 2 January 2022, Libreboot still complies strictly with
RYF, and will continue to do so, at least for the time being.

The conclusion that should be drawn from all of this is as follows:

*Following* FSF criteria does not damage anything, but that criteria is very
conservative. Its exemptions should be *disregarded* and entirely ignored.
RYF is no longer fit for purpose, and should be rewritten to create
a *more strict* set of guidelines, without all the loopholes or exemptions.
As has always been the case, Libreboot tries to always go above and beyond, but
the Libreboot project does not see RYF as a *gold standard*. There are levels
of freedom possible now that the RYF guidelines do not cover at all, and in
some cases even actively discourage/dis-incentivize because it makes compromises
based on assumptions that are no longer true.

Sad truth: RYF actively encourages *less* freedom, by not being bold enough.
It pitches a victory flag and says *mission accomplished*, despite the fact
that the work is *far* from complete!

If followed *with exemptions unchallenged*, RYF may in some cases encourage
companies to *sweep under the rug* any freedom issues that exist, where it
concerns non-free firmware not running on the host CPU (such as the
Embedded Controller firmware).

I propose that new guidelines be written, to replace RYF. These new guidelines
will do away with all exemptions/loopholes, and demand that *all* software be
free on the machine, or as much as possible. Instead of only promoting products
that meet some arbitrary standard, simply catalog all systems on a grand
*database* of sorts (like h-node.org, but better). Include Right to Repair and
OSHW (including things like RISCV) in the most "ideal" standard machine.

Don't call it "Respects Your Freedom" or something similar. Instead, call it
something like: the freedom catalog. And actually focus on hardware, not just
software!

In the year 2022 onwards, we can do better. The RYF program should be cancelled.
It is no longer fit for purpose.
