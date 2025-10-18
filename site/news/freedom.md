% What level of Software Freedom does Libreboot give me?
% Leah Rowe
% 20 March 2023

Quite a while ago, I [wrote a policy](policy.md) in Libreboot that defines,
precisely, the standards of what Libreboot will accept in releases as it
pertains to *[Software Freedom](https://writefreesoftware.org/)*. This policy
was written, because a *lack of
clarity* existed, so I wanted to make sure that people knew exactly what the
Libreboot project is all about, and what they can expect. It is essentially
a *manifesto*, describing the *ideology* of the Libreboot project.

Now, *ideology* is all well and good, but it must be translated into something
concrete that exists in the real world. You can't get there with thought!

Today, I published a follow-up article that defines how the policy
is *implemented* in practice. There has been some confusion among some fans of
the Libreboot project, about what the policy means in practice.

The article describes, in great detail, the current status of licensing for
various components of Libreboot. It is the *goal* of Libreboot to promote
*[software freedom](https://writefreesoftware.org/)*, helping as many people as
possible achieve a level of
sovereignty in their personal computing, reducing (or eliminating) the power
that proprietary software developers would otherwise have over them. Libreboot
makes great strides to provide *boot firmware*, based on coreboot, with as much
libre *source code* as possible. It is the goal of the Libreboot project to
help bring about a world where software freedom is the *default* for everyone.

I hope that the article clears up any confusion, and I'm open to questions if
people have any.

The article will be maintained over time, to reflect the status of Libreboot.

PS:

Also, a new version of Libreboot was *released* yesterday. See:

[Libreboot 20230319 release announcement](libreboot20230319.md)

It made several major fixes, and massively updated the revisions for each
part used in ROM images (containing coreboot, GRUB and SeaBIOS).

I have a bunch of motherboards that I'm working on, and I hope to make another
release available as soon as possible. My priority for the next Libreboot
release is to add as many new boards as possible from coreboot, with minimal
changes to the build system itself; another focus this time is on improvements
to the documentation. Several installation guides are missing, for example, on
specific motherboards.

Specifically, I have focus on some AMD platforms, Intel sandybridge/ivybridge,
Intel Haswell and (more) GM45 platforms. Several boards exist in coreboot that
are viable to be added, both under the *current policy* and adhering to the
current *freedom status* in how the policy is implemented.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
