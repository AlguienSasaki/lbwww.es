% GM45 no-microcode bug mitigations re-added (ThinkPad X200, T400 etc)
% Leah Rowe
% 17 April 2023

[Libreboot releases after 20230423 will provide separate ROMs with microcode
excluded, alongside default ROMs that include microcode](microcode.md).

UPDATE: This also applies to the Dell Latitude E6400 port, added on 19
April 2023 to Libreboot. See: [E6400 announcement](e6400.md)

The change described in this article is *not* present in Libreboot 20221214,
20230319 or 20230413 - **UPDATE: [Libreboot 20230423 is out, and includes
this change on all GM45 targets](libreboot20230423.md)**

If you want a no-microcode setup, either [build the
latest Libreboot from source via lbmk](../docs/build/) and remove the microcode
updates using the instructions on this page, *or* if you need pre-built ROM
images, Libreboot 20220710 had no-microcode ROMs by default and it has the
mitigation patches described by this article.

Free as in freedom
------------

Microcode updates provide stability and security fixes, using the files
provided by coreboot, which are assembled at boot time. CPUs have microcode
in them which configures the logic gates that implement an instruction set
architecture (the x86/x86\_64 architecture), and CPUs provide a *volatile*
mechanism for hot-patching it at boot time, which coreboot makes use of.

Libreboot *includes* CPU microcode updates *by default*, on all x86 hardware.
This *includes* boards that it supported prior to the new policy enacted last
year; before the policy change, Libreboot *excluded* these updates (but the
code for loading them *was retained*, so you could manually add them yourself
if you wanted to). Adding and removing them can be done by coreboot, at build
time, or you can add/remove them from the compiled ROM image by
running `cbfstool`.

You can learn *why* they're needed, and about microcode in general, by reading
the [Binary Blobs Reduction Policy](policy.md) - it is a pragmatic policy,
enacted on 15 November 2022 with a view to supporting as *much* hardware as
possible from coreboot, reducing or eliminating binary blobs in the boot
flash and mitigating any issues that arise - for example, Libreboot makes use
of `me_cleaner` on some newer Intel platforms.

Libreboot contained mitigations for broken SpeedStep/reboot, in older releases,
but it was decided that these mitigations would be *excluded* from the new
releases which include microcode updates by default; why mitigate a problem
that no longer exists in Libreboot?

Why indeed.

Mitigations patches restored!
-----------------------------

*I've restored these mitigations to Libreboot, as of today.*

The patch re-adding these mitigations can be seen here:

<https://codeberg.org/libreboot/lbmk/commit/bd4ea9a02845b22a09b73ebb015ce134234d100b>

### Why?

Free choice, that's why. We don't lecture anyone. We just help people.

One aspect of Libreboot policy is that Libreboot must be *configurable*. In
practice, some users want to remove microcode updates regardless of the
problems that it would cause. Even if I disagree with their reasoning, I
respect their decision and this change is designed to accomodate them.

in practice, such users had a choice before but they currently do not, in
recent Libreboot releases. This criticism was raised with me, and I've taken
it to heart. Therefore, users now have a choice; they always did, but remember:

Libreboot is aimed at non-technical users. Libreboot needs to be usable by
them. It needs to *work*. It *does* work, very well, when microcode updates
are included, as they are by default these days. This patch is about harm
reduction, to at least smooth things over for a small handful of users who
decide to remove microcode updates (as is their choice).

This change, today, fixes a practical violation of Libreboot policy by once
again restoring such practical choice to the user. It is already said how to
do so elsewhere, but I'll repeat it here in this news article for posterity:

### How to add/remove microcode updates from ROM

Extract to file on disk:

	cbfstool libreboot.rom extract -n cpu_microcode_blob.bin -f cpu_microcode_blob.bin

Remove:

	cbfstool libreboot.rom remove -n cpu_microcode_blob.bin

Add (if not present):

	cbfstool libreboot.rom add -f cpu_microcode_blob.bin -n cpu_microcode_blob.bin -t microcode

The `cbfstool` program can be compiled from `util/cbfstool/` in the coreboot
Git repository. In all recent Libreboot releases, you can find it
under `coreboot/default/util/cbfstool/`.

Regardless, the mitigations are largely harmless and so, I've restored them.
They are now included once again, in the Libreboot git repository and they
will be available in ROM images when the next release comes out.

I'll probably re-work the mitigation patch at some point, probably before the
next release, so that the mitigations are only applied if microcode updates
are absent. This would be checked for during machine initialisation, by
modifying the current patches accordingly.

That is all.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
