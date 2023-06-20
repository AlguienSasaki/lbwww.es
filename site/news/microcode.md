% No-microcode ROMs available in next Libreboot release (new stable release soon!)
% Leah Rowe
% 20 June 2023

As I write this, I'm quite close to providing a new stable release of Libreboot.
The final push to get it out the door is underway, with round-the-clock build
testing and general polishing.

Introduction
============

Firstly, what is microcode? In this context, CPU microcode is what configures
logic gates in the CPU to implement an instruction set. You can learn more
about microcode on the [FAQ](../faq.md#microcode) and
[here](policy.md#more-detailed-insight-about-microcode).

In the next Libreboot releases, ROM images that *exclude* CPU microcode updates
will once again be available. Libreboot's [Binary Blob Reduction
Policy](policy.md) dictates that each mainboard must be provided with as few
binary blobs as possible, ideally none. *At present*, *all* x86 and ARM
mainboards supported in Libreboot (in `lbmk.git` and in releases) can boot
with entirely free software, requiring *zero* blobs of any kind from *coreboot*.

The nuance of how Libreboot *implements* this policy is described in
the [Freedom Status](../freedom-status.md) page. Although coreboot (which
Libreboot uses for hardware initialisation) *is* a free software project,
certain binary blobs are needed on some platforms, for things like raminit
(memory controller initialisation). Libreboot tries to reduce or eliminate
these, or mitigate their existence (for example, `me_cleaner` is used on newer
Intel platforms).

One exception made in that policy is that CPU microcode updates *must* be
provided by default, on all x86 mainboards. The ARM platforms do not use
microcode at all. This is a correct policy, because these updates fix critical
security and stability issues in the silicon; more information about that
can be found [here](policy.md#more-detailed-insight-about-microcode). Since
the CPU already has older microcode burned into mask ROM, it makes logical
sense to install these updates to get the latest bug fixes.

In a previous news post,
titled [GM45 no-microcode bug mitigations re-added](gm45microcode.md), I
announced that Libreboot had re-added certain mitigations, working around bugs
caused when microcode is removed on certain Intel GM45 platforms (e.g. X200 or
T400 ThinkPads).

Why?
----

Freedom of choice, that's why. Libreboot's policy explicitly
[states](policy.md#configuration), in the context of *adding* binary blobs:

*It’s natural that the user may want to create a setup that is less libre than
the default one in libreboot. This is perfectly acceptable; freedom is superior,
and should be encouraged, but the user’s freedom to choose should also be
respected, and accomodated.*

Well, this change simply applies that *principle* in reverse. Roughly speaking:

It's natural that some people may wish to cause random kernel panics, raminit
failures, thermal safety issues, random data corruption in memory, and other
similar issues commonly caused by lack of microcode updates. Such folly should
be discouraged, but the user's *freedom to choose* should also be respected,
and accomodated.

It is the official view of the Libreboot project that microcode updates do not
even quality as *software*. The burned-in microcode *is* software, but the
updates only provide hotpatching, essentially turning on or off certain
features. The CPU already has older, buggier microcode burned into mask ROM,
so the choice is to either update it, or encounter more bugs. Regardless,
this is a point of contention for some people.

How?
----

The change was implemented by [this
patch](https://browse.libreboot.org/lbmk.git/commit/?id=f338697b96757977d2a14da00a91236595704fed)
on 19 June 2023, in the Libreboot build system.

In `board.cfg` for each mainboard defined (in Libreboot's build system, lbmk),
the following entries are available:

* `microcode_required="n" or "y"` (it's "n" on ALL boards)
* `blobs_required="n" or "y"`("n" on MOST boards)

If `blobs_required="n"`, the given ROM image `filename.rom`
becomes `filename_noblobs.bin` for all versions of it. In this
context, *noblobs* means *zero* blobs in the entire boot flash when the
final ROM image is flashed to it, regardless of whether coreboot is
blob-free, irrespective of microcode updates. ROM images
containing `noblobs` in the filename *may* still contain microcode
updates, distinguishing *microcode* as a special kind of blob distinct
from all others.

With or without `_noblobs` in the name:

If `microcode_required="n"`, the given ROM image `filename.rom`
is either:

* If no microcode blob exists within it already, such as on ARM
  mainboards, the ROM is simply copied to: `filename_nomicrocode.rom`
* If the ROM contains microcode (default on most x86 boards, except Qemu
  or in rare cases where none are advised), `filename.rom` is retained and
  is copied to `filename_nomicrocode.rom`, and the CPU microcode update blob
  shall be removed from `filename_nomicrocode.rom`.

What this means is that ROMs *with* OR *without* microcode will be present,
where applicable, on ROM images for each given mainboard. This will be the case
by default, for ROM images provided in the next release of Libreboot and all
releases that follow.

Example:

* `seabios_e6400_4mb_libgfxinit_txtmode_noblobs.rom`
* `seabios_e6400_4mb_libgfxinit_txtmode_noblobs_nomicrocode.rom`

It is *strongly* recommended, by the Libreboot project, that you
do *not* use the `_nomicrocode` ROMs on x86 mainboards. These updates
are *required* to implement stable x86 ISA, otherwise your CPU will behave
in strange, unpredictable ways, which could cause severe bugs in software
that cause *real* issues. Issues such as data loss.

A small but vocal minority of users are unhappy with the presence of these
microcode files, so it has been decided that the Libreboot project will once
again accomodate such users. This change has been implemented in the most
unintrusive way possible, to keep the build system logic clean, contrary to the
bloat that existed in many older Libreboot releases.

In previous releases of Libreboot, no-microcode was the default (microcode
updates were excluded entirely, from all releases). This policy was changed
during November 2022, as part of an ongoing campaign to support more hardware
(from coreboot) within Libreboot, so as to provide many more people with
coreboot which, regardless of blob status on each platform, *does* provide
increased software freedom compared to fully proprietary boot firmware, which
is what people would otherwise use; thus, Libreboot's modern policy is
pragmatic, advancing further the cause of *software freedom*.

By contrast, Libreboot's previous policy was to *ban all binary blobs*, which
meant that many mainboards from coreboot were excluded. This resulted in less
people achieving a level of software freedom, because to this day, nothing
quite like Libreboot exists with the scope and ambition that it has. Libreboot
makes coreboot as easy to use as possible for normal, non-technical people who
like the idea of coreboot, but are not competent to configure it from scratch.

Accordingly, the old Libreboot policy, prior to November 2022, *harmed* the
Free Software movement. Such harm was *corrected* in November 2022 and, going
forward, it is the intention of the Libreboot project to eventually have
build targets *for every mainboard that coreboot supports!*

ARM platforms
-------------

On ARM platforms, microcode is not used at all, so the `nomicrocode`
ROM images there are named simply according to what is already
the case.

Removing microcode also possible on older releases
==================================================

Libreboot releases *before* 20221214 excluded microcode by default, and did
not provide ROMs *with* microcode.

Libreboot 20221214, 20230319, 20230413 and 20230423 *include* microcode by
default, and do not provide no-microcode ROM images; however, removal is quite
simple:

	cbfstool libreboot.rom remove -n cpu_microcode_blob.bin

Flash the resulting image. Again, expect *bugs*.

That is all.
