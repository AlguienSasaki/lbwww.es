% Binary Blob Reduction Policy
% Leah Rowe
% 4 January 2022 (updated 15 November 2022)

Free as in freedom!
-----------------

Libreboot's policy is to provide as much
[software freedom](https://writefreesoftware.org/) as possible to each
user, on each and every bit of hardware supported, and to *support as much
hardware from coreboot as is feasible*; what this means is that you should
have the potential to study, modify and *share* all source code, documentation
or other such resources that make Libreboot what it is. Put simply, you should
have *control* of your own computing.

The *goal* of Libreboot is
to do exactly this, and help as many people as possible by automating the
configuration, compilation and installation of *coreboot* for *non-technical*
users, easing it further for the average user by providing user-friendly
instructions for everything. Essentially, Libreboot is a *coreboot
distribution*, in much the same way *Alpine Linux* is a Linux distribution!

The purpose of this document it to outline how that is brought about, and how
the project operates along this basis. *This* document is largely about the
ideology and it is therefore (mostly) non-technical; for technical information,
you can refer to the [Libreboot build system documentation](../docs/maintain/).

Current project scope
---------------------

The libreboot project is concerned with what goes in the main boot flash IC, but
there are other pieces of firmware to take into consideration, as covered
in the [libreboot FAQ](../faq.md#what-other-firmware-exists-outside-of-libreboot).

Most critical of these are:

* Embedded controller firmware
* HDD/SSD firmware
* Intel Management Engine / AMD PSP firmware

### What is a binary blob?

A binary blob, in this context, is any executable for which no source code
exists, that you cannot study and modify in a reasonable manner. By definition,
all such blobs are *proprietary* in nature, and should be avoided if possible.

For information about Intel Management Engine and AMD PSP, refer to the FAQ.

Blob *reduction* policy
---------------------

### Default configurations

Coreboot, upon which Libreboot is based, is mostly libre software but does
require certain vendor code on some platforms. A most common example might be raminit
(memory controller initialisation) or video framebuffer initialisation. The
coreboot firmware uses certain vendor code for some of these tasks, on some motherboards,
but some motherboards from coreboot can be initialised with 100% libre source
code, which you can inspect, and compile for your use.

Libreboot deals with this situation in a *strict* and *principled* way. The
nature of this is what you're about to read.

The libreboot project has the following policy:

* If free software *can* be used, it *should* be used. For example, if VGA ROM
  initialization otherwise does a better job but coreboot has *libre* init code
  for a given graphics device, that code should be used in libreboot, when
  building a ROM image. Similarly, if *memory controller initialization* is
  possible with vendor code *or* libre code in coreboot, the *libre* code
  should be used in ROMs built by the Libreboot build system, and the *vendor*
  raminit code should not be used; however, if no libre init code is available
  for said raminit, it is permitted and Libreboot build system will use the
  *vendor* code.
* Some nuance is to be observed: on some laptop or desktop configurations, it's
  common that there will be *two* graphics devices (for example, an nvidia and
  an intel chip, using nvidia optimus technology, on a laptop). It may be that
  one of them has libre init code in coreboot, but the other one does not. It's
  perfectly acceptable, and desirable, for libreboot to support both devices,
  and accomodate the required vendor code on the one that lacks native
  initialization.
* An exception is made for CPU microcode updates: they are permitted, and in
  fact *required* as per libreboot policy. These updates fix CPU bugs, including
  security bugs, and since the CPU already has non-libre microcode burned into
  ROM anyway, the only choice is either *x86* or *broken x86*. Thus, libreboot
  will only allow coreboot motherboard configurations where microcode updates
  are *enabled*, if available for the CPU on that motherboard.
  [Releases after 20230423 will provide separate ROM images with microcode
  excluded, alongside the default ones that include microcode.](microcode.md)
* Intel Management Engine: in the libreboot documentation, words *must* be written
  to tell people how to *neuter* the ME, if possible on a given board.
  The `me_cleaner` program is very useful, and provides a much more secure ME
  configuration.
* Such vendor files should *never* be deleted, even if they are unused. In the
  coreboot project, a set of `3rdparty` submodules are available, with vendor
  code for init tasks on many boards. These must *all* be included in libreboot
  releases, even if unused. That way, even if the Libreboot build system does
  not yet integrate support for a given board, someone who downloads libreboot
  can still make changes to their local version of the build system, if they
  wish, to provide a configuration for their hardware.

Generally speaking, common sense is applied. For example, an exception to the
minimalization might be if *vendor* raminit and *libre* raminit are available, but
the *libre* one is so broken so as to be unusable. In that situation, the vendor
one should be used instead, because otherwise the user might switch back to an
otherwise fully proprietary system, instead of using coreboot (via libreboot).
*Some* freedom is *better than none*.

Libreboot's pragmatic policies will inevitably result in more people becoming
coreboot developers in the future, by acting as that crucial *bridge* between
*it* and non-technical people who just need a bit of help to get started.

### Configuration

The principles above should apply to *default* configurations. However, libreboot
is to be *configurable*, allowing the user to do whatever they like.

It's natural that the user may want to create a setup that is *less* libre than
the default one in libreboot. This is perfectly acceptable;
[freedom](https://writefreesoftware.org/) is superior,
and should be encouraged, but the user's *freedom to choose* should also be
respected, and accomodated.

In other words, do not lecture the user. Just try to help them with their
problem! The goal of the libreboot project is simply to make coreboot more
accessible for otherwise non-technical users.

Examples of Libreboot policy in action
-----------------------------------

Context is everything, so to make the above picture more clear, here are a few
examples of how this very policy is implemented in Libreboot:

* On many laptops, it is possible to have both Intel graphics and a dedicated
  graphics card enabled; more about Intel graphics is covered below. We could
  provide the VGA ROM for those extra chips, e.g. Nvidia graphics chips, but
  we don't. We configure the machines in such a way where the Intel graphics
  is used, exclusively, so the dedicated GPU may as well be turned off, and
  it *is* turned off in such circumstances. The Intel graphics provide good
  enough performance and reliable operation for most people, and native
  initialisation code is available from coreboot to make use of the Intel GPU
  at boot time.
* Newer Intel platforms have Intel ME, which is a coprocessor inside the
  machine that handles many functions including certain power management, and
  it can provides useful remote management features with Intel AMT for example.
  This is a giant 5MB+ section in the flash, and the ME has full access to your
  RAM and other peripherals; more is documented about this in the Libreboot
  FAQ. On older ME4-based platforms such as Intel GM45 platforms, e.g.
  ThinkPad X200/T400, we remove the ME firmware entirely from the flash, leaving
  the Intel ME in a permanent reset loop. On Intel Sandybridge and up, we
  include ME firmware when building, or provide a script to insert it via
  vendor download on release images, but Libreboot's build system automatically
  runs the [me cleaner](https://github.com/corna/me_cleaner) software, which
  removes most of the features from the ME, including its kernel and networking
  stack, leaving only a rump (dare we say, ROMP) of initialisation code in
  the ME; essentially, the configuration is equivalent to running coreboot
  without a payload, where it does nothing other than initialise itself, because
  that is all the ME does in this configuration, initialise itself but not
  actually run anything. ME Cleaner is required, instead of full removal,
  because the machines don't boot reliably without at least the BUP/ROMP
  modules present in flash.
* On Intel Haswell platform (Intel 4th gen), vendor firmware is available for
  memory controller initialisation, called the Intel *MRC* or System Agent. We
  previously included this in Libreboot; nowadays, we have free initialisation
  code written by Angel Pons. The libre code, called NRI (Native RAM
  Initialisation) still has a few bugs, such as lack of ECC support, and the
  MRC also does other things such as initialise certain peripherals. The
  Haswell machines can be configured in EHCI or XHCI mode for USB ports at
  startup, and the latter means that you need xHCI support in whatever you're
  using such as Linux, but also payloads such as U-Boot or GRUB. SeaBIOS and
  GRUB have stable xHCI support (we have to patch GRUB to have it because
  otherwise upstream doesn't have it). U-Boot's xHCI support is quite buggy,
  at least as of November 2024; when tested, it looks like the EHCI mode
  doesn't work on NRI, only MRC. Now, to get more stable U-Boot support when
  using USB devices, we could use the MRC and set the machine in EHCI mode
  at startup; an option exists for it in coreboot. Ditto, we could have ECC
  support (useful on the Dell Precision T1700, which Libreboot supports!) - it
  was decided that these issues are quite minor so the free initialisation code
  is used, and we no longer provide the MRC.
* Libreboot provides native graphics initialisation on most Intel hardware,
  provided by coreboot. This replaces the Intel VGA ROM; called libgfxinit,
  coreboot's native code is far from perfect because many legacy VGA functions
  simply do not work so it can be a challenge to get old, legacy operating
  systems working, even if they had full driver support on a given machine
  when running the vendor BIOS. Libgfxinit otherwise provides a sensible
  framebuffer implementation from coreboot, which Linux and BSD systems know
  how to use, and you can use KMS on these systems instead of calls to VGA
  in the BIOS, to mitigate this issue. It was decided a long time ago that,
  when available, the native initialisation should be used, when it is stable
  and Linux/BSD systems work reliably. There is only one exception: the
  ThinkPad T480 currently doesn't work with libgfxinit in Libreboot, at least
  as of November 2024, so the Intel VGA ROM is used instead.
* On RK3399-based Chromebooks, it is possible to load a HDCP blob, which could
  be convenient for easy access to DRM-based media. We simply do not use this
  blob on any machines, because it is completeley unnecessary for everyday
  operating of a free operating system.

Free hardware
--------------

It is desirable to see a world where all hardware and software is libre, under
the same ideology as the Libreboot project. Hardware!?

Yes, hardware. RISC-V is a great example of a modern attempt at libre hardware,
often called *Open Source Hardware*.
It is a an ISA for the manufacture of a microprocessor. Many real-world
implementations of it already exist, that can be used, and there will only be
more.

Such *hardware* is still in its infancy. We should start a project that will
catalog the status of various efforts, including at the hardware level (even
the silicon level). Movements like OSHW and Right To Repair are extremely
important, including to our own movement which otherwise will
typically think less about hardware freedoms (even though it really, really
should!)

One day, we will live in a world where anyone can get their own chips made,
including CPUs but also every other type of IC. Efforts to make homemade
chip fabrication a reality are now in their infancy, but such efforts do
exist, for example, the work done by Sam Zeloof and the Libre Silicon project:

* <https://www.youtube.com/channel/UC7E8-0Ou69hwScPW1_fQApA>
* <http://sam.zeloof.xyz/>
* <https://libresilicon.com/>

(Sam literally makes CPUs in his garage)

More detailed insight about microcode
---------------------------------

To be clear: it is preferable that microcode be free.
Not including CPU microcode updates is an absolute disaster for system
stability and security, so Libreboot *includes microcode updates by default, in
all modern release images, where possible to do so*.

The CPU already has microcode burned into mask ROM. The microcode configures
logic gates in the CPU, to implement an instruction set, via special *decoders*
which are fixed-function; it is not possible, for example, to implement a RISCV
ISA on an otherwise x86 processor. It is only possible for the microcode to
implement x86, or *broken* x86, and the default microcode is almost always
*broken x86* on Intel/AMD CPUs; it is inevitable, due to the complexity of
these processors.

These processors provide a way to supply microcode *updates*. These updates
are volatile, and consequently must be applied during every boot cycle. The
updates fix stability/reliability/security bugs, and their *absence*
is *technically incorrect*, so you are strongly advised to install them.
Examples of where these updates fix bugs: on ASUS KCMA-D8/KGPE-D16
and ThinkPad X200/T400/T500/W500/X200T/X200/R500/X301, the updates make
hardware-based virtualization (via `kvm`) completely stable, where it would
otherwise lead to a kernel panic. They allow those same thinkpads to be run with
high CPU usage and I/O (RAM usage), without crashing (otherwise, it's very
likely to encounter a kernel panic caused by a *Machine Check Exception*).

Not including these updates will result in an unstable/undefined state. Intel
themselves define which bugs affect which CPUs, and they define workarounds, or
provide fixes in microcode. Based on this, software such as the Linux kernel
can work around those bugs/quirks. Also, upstream versions of the Linux kernel
can update the microcode at boot time (however, it is recommend still to do it
from coreboot, for more stable memory controller initialization or “raminit”).
Similar can be said about AMD CPUs.

Once upon a time, Libreboot *excluded* microcode updates by default, but this
lead to broken behaviour. Here are some examples:

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0012-fix-speedstep-on-x200-t400-Revert-cpu-intel-model_10.patch?id=9938fa14b1bf54db37c0c18bdfec051cae41448e>

<https://browse.libreboot.org/lbmk.git/plain/resources/coreboot/default/patches/0018-Revert-cpu-intel-Configure-IA32_FEATURE_CONTROL-for-.patch?id=4b7be665968b67463ec36b9afc7e8736be0c9b51>

These patches revert *bug fixes* in coreboot, fixes that happen to break other
functionality but only when microcode updates are excluded. The most
technically correct solution is to *not* apply the above patches, and instead
supply microcode updates!

You *need* microcode updates, or you will have a broken CPU; broken, because
it literally behaves differently than it's supposed to, so software will have
unpredictable bugs that could even cause data corruption - or worse.

[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/news/policy.html>
