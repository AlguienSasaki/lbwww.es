% Osboot is now part of Libreboot
% Leah Rowe
% 15 November 2022

Long live Libreboot!
====================

The former `osboot` project, started by the same founder as Libreboot, Leah
Rowe, has today shut down; the osboot website now automatically redirects, via
HTTP 301, to the libreboot website. Everything osboot had, has now been added
to Libreboot, and development will now occur exclusively in Libreboot from now
on.

The result of this decision is that Libreboot does (and can) now support newer
hardware, such as the ThinkPad X230 or T440p.

Such was alluded to in the previous Libreboot release announcement:

<https://libreboot.org/news/libreboot20220710.html#planned-osbootlibreboot-merger>

The exact method proposed in that previous announcement has not been adopted.
Instead, Libreboot essentially now *is* osboot, under the Libreboot name.

The *binary blob minimalisation* policy of osboot has replaced the previous
policy used by Libreboot. It is a pragmatic policy revolved around providing
as much freedom as possible per platform, while no longer sticking only to
those *"pure"* platforms; all platforms from coreboot can now be supported, if
someone wishes to maintain the board in `lbmk`. The new policy is here:

<https://libreboot.org/news/policy.html> (same URL as the old one, but the
text has been updated in line with osboot policy)

To understand the full nature of this merge, you should read the policy
document linked above. Both osboot and Libreboot were largely identical, and
basically the same project. In order to make management of the projects much
easier, they were simply merged into one.

How the merge was conducted
---------------------------

Since 2020, Libreboot (and osboot) have both been through several reboots,
forking off each other other to keep in sync. It was starting to become too
much of a burden, which is the main reason the merger took place.

On November 14th, I (Leah Rowe) literally diffed all of Libreboot since the
last reboot (of the Libreboot repository) and checked each one against osboot,
adding whatever was missing from that *in osboot*. This then put the osboot
project in sync with Libreboot, but in line with osboot and with the additional
boards (plus scripts and so on) in osboot, compared to Libreboot pre-merge.

The same was *then* applied *in reverse*: differences in osboot were ported
back to Libreboot, carefully and methodically. All of this took place in a the
period of 1 day, because the overall structure of both projects was largely the
same and the build systems in both projects are extremely modular, making this
sort of work very easy indeed.

A similar process was performed with the Libreboot website and the osboot
website, but osboot already contained all the same documentation as Libreboot,
plus extra. The differences were therefore ported over to Libreboot.

Porting of documentation (from osboot over to Libreboot) was done by
Caleb La Grange (IRC nick `shmalebx9`), and reviewed by myself prior to going
live.

As of writing this post, one utility from osboot (for changing MAC addresses
inside Gbe regions on Intel machines that have an IFD) hasn't been moved over
yet, but I'm considering merging all outside utilities maintained by us (bucts,
ich9gen and nvmutil) into lbmk, making lbmk a BSD-like *monorepo* of sorts. We
shall see.

That's it really. Osboot and Libreboot are now operating as a single, unified
project, the way it should have been from the beginning. Quite boring news,
actually.

New Libreboot release coming soon!!!
====================================

CrOS devices re-added to Libreboot
----------------------------------

Documentation needs to be updated, to reflect this and many other things, but
ARM-based chromebooks are now once again supported in Libreboot, using the
u-boot payload inside coreboot.

This work is courtesy of Alper Nebi Yasak (`alpernebbi` on libera IRC) and it's
also part of the merge.	
