% Binary blob minimalisation policy
% Leah Rowe
% 4 January 2022 (updated 23 January 2022)

Introduction
============

The `osboot` project is a *fork* of the Libreboot project.
Libreboot is designed to comply with the Free Software Foundation's
[Respects Your Freedom criteria](https://ryf.fsf.org/about/criteria) and
the [GNU Free System Distribution Guidelines (GNU FSDG)](https://www.gnu.org/distros/free-system-distribution-guidelines.en.html),
ensuring that it is entirely [Free Software](https://www.gnu.org/philosophy/free-sw.html).

It was decided that a formal policy should be written, because there is quite
a bit of nuance that would otherwise not be covered. The policies of `osboot`
are fundamentally different than those of Libreboot.

Libreboot intentionally *de-blobs* coreboot, which is to say that it does not
include binary blobs. The coreboot software otherwise requires binary blobs on
most systems that it has support for. Libreboot's version of coreboot is
entirely *free*, on its consequently reduced set of supported mainboards.

The `osboot` project is *fundamentally* different:
Libreboot's [zero blobs policy](https://libreboot.org/news/policy.html) has
been scrapped, entirely. The goal of *osboot* is simply to support every single
system from coreboot, to provide pre-configured, automated compiling of ROM
images for *all* of them. This is quite a lot more ambitious in terms of sheer
workload, and maintenance. It is expected that the project will *grow* in the
future, to accomodate *board maintainers*, just like you have *package
maintainers* in Debian; the analogy is highly appropriate, given the nature
of the osboot build system, which you can learn more about on the [osbmk
maintenance manual](../docs/maintain/).

**Freedom is very much preferable and a world where everyone can use Free
Software, exclusively, is to be welcomed. However, we do not yet live in that
world. The osboot project wishes to see more hardware become suitable for
entry into Libreboot, in the future. Both osboot and Libreboot are lead by
Leah Rowe, who is also the founder of both projects.**

The osboot position is more like an opinion, as opposed to an actual policy.
That opinion is this: *some* freedom is better than *zero* freedom. There are
plenty of people with coreboot-compatible hardware, who wish to move away from
otherwise fully proprietary boot firmware (usually supplied by the manufacturer
of the hardware). The *osboot* project is here to help! It provides a fully
automated build system, making coreboot much easier to use, and it provides
user-friendly [installation guides](../docs/install/) to help you get started.

The position of the osboot project is basically the same as that of Libreboot,
except that it takes a far more *pragmatic* approach, as opposed to Libreboot's
*dogmatic* approach. Supporting more hardware, even if the hardware is less
friendly to software freedom, will provide a path towards coreboot for more
people, and it may lead to more coreboot development in the future.

Freedom is still the ultimate goal, and *coreboot* provides a lot more freedom
to the user compared to fully proprietary vendor firmware. Making coreboot
easier to use is a noble goal, and the result is that more people can achieve
a level of computing freedom that they would otherwise not have.

Current project scope
=====================

The osboot project is concerned with what goes in the main boot flash IC, but
there are other pieces of firmware to take into consideration, as covered
in the [osboot FAQ](../faq.md#what-other-firmware-exists-outside-of-osboot).

Most critical of these are:

* Embedded controller firmware
* HDD/SSD firmware
* Intel Management Engine / AMD PSP firmware

Specific binary blobs are also problematic, on most coreboot systems, but they
differ per machine. The *osboot* project has a *blob minimalization* policy
(as opposed to Libreboot's *blob deletion policy*), which you can read about
in the following section.

For information about Intel Management Engine and AMD PSP, refer to the FAQ.

Blob *minimalization* policy
============================

Default configurations
----------------------

*Libreboot* has a blob *deletion* policy; it contains no binary blobs, not even
CPU microcode updates.

The *osboot* project has the following policy, by contrast:

* If a blob *can* be avoided, it should be avoided. For example, if VGA ROM
  initialization otherwise does a better job but coreboot has *free* init code
  for a given graphics device, that code should be used in osboot, when
  building a ROM image. Similarly, if *memory controller initialization* is
  possible with a binary blob *or* free code in coreboot, the *free* code
  should be used in ROMs built by `osbmk`, and the *blob* for raminit should
  not be used; however, if no free init code is available for said raminit,
  it is permitted and osbmk will use the *blob*.
* Some nuance is to be observed: on some laptop or desktop configurations, it's
  common that there will be *two* graphics devices (for example, an nvidia and
  an intel chip, using nvidia optimus technology, on a laptop). It may be that
  one of them has free init code in coreboot, but the other one does not. It's
  perfectly acceptable, and desirable, for osboot to support both devices,
  and accomodate the required binary blob on the one that lacks native
  initialization.
* An exception is made for CPU microcode updates: they are permitted, and in
  fact *required* as per osboot policy. These updates fix CPU bugs, including
  security bugs, and since the CPU already has non-free microcode burned into
  ROM anyway, the only choice is either *x86* or *broken x86*. Thus, osboot
  will only allow coreboot mainboard configurations where microcode updates
  are *enabled*, if available for the CPU on that mainboard.
* Intel Management Engine: in the osboot documentation, words *must* be written
  to tell people how to *neuter* the ME, if possible on a given board.
  The `me_cleaner` program is very useful, and provides a much more secure ME
  configuration.
* Binary blobs should *never* be deleted, even if they are unused. In the
  coreboot project, a set of `3rdparty` submodules are available, with binary
  blobs for init tasks on many boards. These must *all* be included in osboot
  releases, even if unused. That way, even if `osbmk` does not yet integrate
  support for a given board, someone who downloads osboot can still make
  changes to their local version of the build system, if they wish, to provide
  a configuration for their hardware.

Generally speaking, common sense is applied. For example, an exception to the
minimalization might be if *blob* raminit and *free* raminit are available, but
the *free* one is so broken so as to be unusable. In that situation, the blob
one should be used instead, because otherwise the user might switch back to an
otherwise fully proprietary system, instead of using coreboot (via osboot).

Configuration
-------------

The principles above should apply to *default* configurations. However, osboot
is to be *configurable*, allowing the user to do whatever they like.

It's natural that the user may want to create a setup that is *less* free than
the default one in osboot. This is perfectly acceptable; freedom is superior,
and should be encouraged, but the user's freedom to choose should also be
respected, and accomodated.

In other words, do not lecture the user. Just try to help them with their
problem! The goal of the osboot project is simply to make coreboot more
accessible for otherwise non-technical users.

FREEDOM CATALOG
===============

A *blob status* page should also be made available, educating people about the
status of binary blobs on each machine supported by `osbmk`.

It is desirable to see a world where all hardware and software is free.
Hardware!?

Yes, hardware. RISC-V is a great example of a modern attempt at free hardware.
It is a free ISA for the manufacture of a microprocessor. Many real-world
implementations of it already exist, that can be used, and there will only be
more.

Free *hardware* is still in its infancy. We should start a project that will
catalog the status of various efforts, including at the hardware level (even
the silicon level). Movements like OSHW and Right To Repair are extremely
important, including to the Free Software movement which otherwise will
typically think less about hardware freedom (even though it really, really
should!)

Problems with RYF criteria
==========================

You can read those guidelines by following these hyperlinks:

* [FSF Respects Your Freedom (RYF) guidelines](https://ryf.fsf.org/about/criteria)

The FSF RYF guidelines state the following:

*"However, there is one exception for secondary embedded processors. The exception applies to software delivered inside auxiliary and low-level processors and FPGAs, within which software installation is not intended after the user obtains the product. This can include, for instance, microcode inside a processor, firmware built into an I/O device, or the gate pattern of an FPGA. The software in such secondary processors does not count as product software."*

This is a violation of every principle the FSF stands for, *and it should be
rejected on ideological grounds*. The rest of osboot's policy and overall
ideology expressed, in this article, will be based largely on that rejection.
The definition of *product software* is completely arbitrary; software is
software, and software should always be *free*. Instead of making such
exceptions, more hardware should be encouraged, with help given to provide as
much freedom as possible, while providing education to users about any pitfalls
they may encounter, and encourage freedom at all levels. When an organisation
like the FSF makes such bold exceptions as above, it sends the wrong message,
by telling people essentially to sweep these other problems under the rug, just
because they involve software that happens to run on a "secondary processor".
If the software is possible to update by the user, then it should be free,
regardless of whether the manufacturer *intended* for it to be upgraded or not.
Where it really *isn't* possible to update such software, proprietary or not,
advice should be given to that effect. Education is important, and the FSF's
criteria actively discourages such education; it creates a false hope that
everything is great and wonderful, just because the software on one arbitrary
level is all free.

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
defining what standards should apply *in practise*.

The fact that almost nobody talks about the EC firmware is *because* of the
Respects Your Freedom certification. In reality, the EC firmware is crucial
to user freedom, and ought to be free, but it is completely disregarded by
the FSF as *part of the hardware*. This is wrong, and the FSF should actively
actively encourage people to free it, on every laptop!

Other firmware currently outside the reach of the *osboot* project are covered
in the osboot FAQ page. For example, HDD/SSD firmware is covered in the FAQ.
Again, completely disregarded and shrugged off by the FSF.

The *osboot* project will not hide or overlook these issues, because they are
indeed critical, but again, currently outside the scope of what osbmk does.
At the moment, osbmk concerns itself just with coreboot, but this ought to
change in the future.

Examples of FSF *sweeping blobs under the rug*
----------------------------------------------

Over the years, there have been numerous cases where the FSF actively fails to
provide incentive for levels of software freedom, such as:

* TALOS II "OpenPOWER" workstation from Raptor Engineering USA. It contains a
  broadcom NIC inside it which requires firmware, and that firmware was non-free.
  The FSF were willing to ignore it, and certify the TALOS II product under RYF,
  but Timothy Pearson of Raptor Engineering had it freed anyway, without being
  told to. Hugo Landau reverse engineered the specification and Evan Lojewski
  wrote free firmware. See:
  See: <https://www.devever.net/~hl/ortega> and <https://github.com/meklort/bcm5719-fw>
* FSF once endorsed the ThinkPad X200, as sold by [Minifree Ltd](https://minifree.org),
  which contains the Intel ME; the bootrom is still there, as is the ME
  coprocessor, but the ME is put into a disabled state via the Intel Flash
  Descriptor, and the ME firmware in flash is removed. However, the ME is an
  entire coprocessor which, with free firmware, could be used for a great many
  things. In the Libreboot and coreboot projects, there has always been interest
  in this but the FSF disregards it entirely. The X200 product they certified
  came with Libreboot pre-installed.
* Libreboot has a utility written by I, Leah Rowe, that generates ICH9M flash
  descriptors and GbE NVM images from scratch. This is necessary to configure
  the machine, but these are binary blobs otherwise; the FSF would have been
  quite content to certify the hardware anyway since these were non-software
  blobs in a format fully documented by Intel (they are just binary configuration
  files), but I went ahead and wrote ich9gen anyway. With ich9gen, you can
  more easily modify the descriptor/gbe regions for your firmware image. See:
  <https://libreboot.org/docs/install/ich9utils.html> - osboot also has this
* FSF once endorsed the ThinkPad T400 with Libreboot, as sold by Minifree. This
  machine comes in two versions: with ATI+Intel GPU, or only Intel GPU. If ATI
  GPU, it's possible to configure the machine so that either GPU is used. If the
  ATI GPU is to be used, a firmware blob is needed for initialization, though the
  driver for it is completely free. FSF ignored this fact and endorsed the
  hardware, so long as Libreboot does not enable the ATI GPU or tell people how
  to enable it. The *Intel* GPU on that machine has free initialization code by
  the coreboot project, and a fully free driver in both Linux and BSD kernels.
  In the configuration provided by Libreboot, the ATI GPU is completely disabled
  and powered down.
* All Libreboot-compatible ThinkPads contain non-free Embedded Controller
  firmware, which is user-flashable (*and intended for update by the
  manufacturer*). The FSF chose to ignore the EC firmware, under
  their *secondary processor* exemption. See:
  <http://libreboot.org/faq.html#ec-embedded-controller-firmware>

In all of the above cases, the FSF could have been stricter, and bolder, by
insisting that these *additional* problems for freedom were solved. They did
not. There are many other examples of this, but the purpose of this article is
not to list all of them (otherwise, a book could be written on the subject).

Problems with FSDG
------------------

The FSDG criteria is separate from RYF, but has similar problems. FSDG is
what the FSF-endorsed GNU+Linux distros comply with. Thoughts:

* Excluding firmware blobs in the linux kernel is *bad*. Non-free firmware
  is *also bad*. Including them is a wiser choice, if strong education is also
  provided about *why they are bad* (lack of freedom). If you expose them to
  the user, and tell them about it, there is greater incentive (by simple
  reminder of their existence) to reverse engineer and replace them.
* Firmware *in your OS kernel* is *good*. The FSF simultaneously gives the OK
  for hardware *with those same firmware blobs* if the firmware is embedded
  into a ROM/flash chip on the device, or embedded in some processor. If the
  firmware is on separate ROM/flash, it could still be replaced by the user via
  reverse engineering, but then you would probably have to do some soldering
  (replace the chip on the card/device). *If the firmware is loaded by the
  OS kernel, then the firmware is exposed to the user and it can be more
  easily replaced. FSF criteria in this regard encourages hardware designers
  to hide the firmware instead, making actual freedom less likely!*

Besides this, FSDG seems OK. Any free operating system should ideally not
have non-free *drivers* or *applications*.

Hardware manufacturers like to shove everything into firmware because their
product is often poorly designed, so they later want to provide workarounds in
firmware to fix issues. In many cases, a device will already have firmware on it
but require an update supplied to it by your OS kernel.

The most common example of non-free firmware in Linux is for wifi devices.
This is an interesting use-case scenario, if freed, because it could be used
for owner-controlled *software defined radio*.

The *Debian* way is ideal. The Debian GNU+Linux distribution is entirely free
by default, and lacks any of the non-free firmware, but they have a separate
repository containing non-free software. If you only want firmware, it is
trivial to get installer images with it included, or add that to your installed
system.

*Banning* linux-firmware specifically is a threat to freedom in the long term,
because new users of GNU+Linux might be discouraged from using the OS if their
hardware doesn't work. You might say: just buy new hardware! This is often not
possible for users, and the user might not have the skill to reverse engineer
it either.

More detailed insight about microcode
=====================================

To be clear: it is preferable that microcode be free. The microcode on Intel
and AMD systems *are* non-free. Facts and feelings rarely coincide; the
purpose of this section is to spread *facts*.

The *osboot* build system enables microcode updates *by default*, even on
boards that Libreboot supports. Libreboot *excludes* microcode updates.

Not including CPU microcode updates is an absolute disaster for system
stability and security.

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

Not including these updates will result in an unstable/undefined state. Intel
themselves define which bugs affect which CPUs, and they define workarounds, or
provide fixes in microcode. Based on this, software such as the Linux kernel
can work around those bugs/quirks. Also, upstream versions of the Linux kernel
can update the microcode at boot time (however, it is recommend still to do it
from coreboot, for more stable memory controller initialization or “raminit”).
Similar can be said about AMD CPUs.

Here are some examples of where lack of microcode updates affected *Libreboot*,
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

Pick your poison. Not adding the updates is *irresponsible*, and ultimately
futile, because you still end up with non-free microcode anyway, just you get
an older, buggier version instead!

The *osboot* build system does not apply the two patches linked above! Instead,
CPU microcode updates are enabled by default, on the affected boards. The
result is superior IA32 feature control and added PECI support.

The *osboot* project rejects the FSF's narrow, dogmatic view entirely.

The osboot firmware is far superior to Libreboot, in terms of reliability, due
to the presence of microcode updates in the firmware, and with zero practical
change to your freedom, on libreboot-compatible hardware.

However:

**I will continue to develop Libreboot and osboot, in parallel.**

There are some people who still want Libreboot, who believe in FSF principles
on this subject, and I believe it would ultimately be damaging if I were to
just *shut down* the Libreboot project. I do not agree with the policies of
the Libreboot project; I was only following the FSF's instructions when I made
it, all those years ago.

If I were to cancel Libreboot, my fear is that such people would be stubborn
and end up, ironically, being less likely to use coreboot-based firmware. This
is for a very human reason: such people might try (fail) to make their *own*
Libreboot instead. I believe I'm in the best position to run such a project, to
ensure that a good job is being performed. I *want* to help people, even those
people, *because I want everyone to have freedom*. I also believe in freedom of
choice, and Libreboot is an excellent choice for those who wish to use something
that complies with FSF criteria.

Thus, I continue developing Libreboot in parallel with osboot, even though
Libreboot is technically *inferior* to osboot. The *osboot* project is where
my heart truly lies. I'm completely in it, whereas Libreboot is something I
also maintain on the side. I try to do my best, when working on both projects.
I really don't mind maintaining both of them, because they are actually very
similar anyway, on relevant hardware.

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
RYF, and will continue to do so, at least for the time being. The *osboot*
project rejects those policies in their entirety, and instead takes a pragmatic
approach.

The conclusion that should be drawn from all of this is as follows:

*Following* FSF criteria does not damage anything, but that criteria is very
conservative. Its exemptions should be *disregarded* and entirely ignored.
RYF is no longer fit for purpose, and should be rewritten to create
a *more strict* set of guidelines, without all the loopholes or exemptions.
As has always been the case, Libreboot tries to always go above and beyond, but
the Libreboot project does not see RYF as a *gold standard*. There are levels
of freedom possible now that the RYF guidelines do not cover at all, and in
some cases even actively discourage/dis-incentivize because it makes compromises
based on assumptions that are no longer true. The *osboot* project, again,
takes a pragmatic approach, rejecting Libreboot's dogmatic approach entirely.

Sad truth: RYF actively encourages *less* freedom, by not being bold enough.
It sets a victory flag and says *mission accomplished*, despite the fact
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

Other resources
===============

Ariadne Conill's RYF blog post
------------------------------

Ariadne Conill, security team chair of *Alpine Linux*, posted a very robust
article about RYF, with similar points made when compared to *this* article.
However, Ariadne goes into detail on several other examples of problems with
the FSF RYF criteria; for example, it talks about the *Novena* product by
Bunnie.

It's worth a read! Link:

<https://ariadne.space/2022/01/22/the-fsfs-relationship-with-firmware-is-harmful-to-free-software-users/>

Hector Martin's RYF thread
--------------------------

Hector Martin, leader of the *Asahi Linux* project (for booting linux kernels
on M1 macbooks) wrote a very robust twitter thread criticizing the RYF criteria
and much of what he wrote inspired *this* article that you are reading. See:

<https://twitter.com/marcan42/status/1040626210999431168>

If you wish to avoid non-free javascript, you can read this thread using
nitter:

<https://nitter.net/marcan42/status/1040626210999431168>

Article updates
===============

23 January 2022
---------------

Added link to Ariadne Conill's article.

21 January 2022
---------------

This article was updated on 21 January 2022, to add the section with examples
in the real world of FSF sweeping blobs under the rug (ATI T400 thinkpads,
ICH9M descriptors and TALOS II NIC firmware).

Also on 21 January 2022: added section about FSDG (criticisms of it).

Also on 21 January 2022: added link to Hector Martin's twitter thread.
