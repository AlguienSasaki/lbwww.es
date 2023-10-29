% Canoeboot project launched (new Libreboot fork)
% Leah Rowe
% 26 October 2023

I've started a new sister project of Libreboot, that I will maintain in
parallel; whenever there is a new Libreboot release, I will then use it to
create a new release of *Canoeboot*.

You can find Canoeboot here: <https://canoeboot.org/>

The first release, Canoeboot 20231026, is here (created on 26 October 2023):
<https://canoeboot.org/news/canoeboot20231026.html> - it is based on the
recent [Libreboot 20231021 release](libreboot20231021.md).

Canoeboot is a *proof of concept* that provides a technical implementation of
Libreboot, but *without* the [Binary Blob Reduction Policy](policy.md); instead,
Canoeboot implements the [GNU Free System Distribution
Guidelines (GNU FSDG)](policy.md#problems-with-fsdg) as policy. GNU FSDG is the
policy that Libreboot *previously* implemented, until 16 November 2022 when the
new *Binary Blob Reduction Policy* was enacted.

The *reduction* policy has resulted in more hardware being supported from
coreboot, thus bringing free software to more people, and it is handled in the
manner described by Libreboot's [Freedom Status](../freedom-status.md) page.

The purpose of Canoeboot is to demonstrate the inferior state Libreboot would
be in today, if it still adhered to the *old* GNU policy. The goal of Libreboot
is to help as many people as possible achieve a level of [software
freedom](https://writefreesoftware.org/learn), so that they may rid themselves
of proprietary software. This is done, because every user deserves to have the
freedom to study, adapt, share and use software infinitely, without restrictions.

By implementing the policies that it has, Libreboot is in a position to do this
in the most optimal way, whereas Canoeboot can only support a limited subset of
hardware compared to Libreboot; in other words, Canoeboot's policies are a
liability to the adoption of free software by normal people everywhere.

Canoeboot is still a good option if your hardware supports it, but you should
know: Libreboot *also* provides the very same blob-free, entirely free software
config on all of the mainboards that Canoeboot supports. Canoeboot is a proof
of concept, but you *can* use it, if you wish.

[Patches are also welcome](https://canoeboot.org/git.html) in Canoeboot, if you
spot something wrong that ought to be fixed or improved.
