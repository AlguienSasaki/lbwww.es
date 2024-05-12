% Should Canoeboot become GNU Canoeboot?
% Leah Rowe
% 12 May 2024

Introduction/context
====================

As many readers know, Libreboot abandoned the GNU FSDG policy in November 2022,
instead opting for a [different policy](policy.md) allowing blobs only under
the most limited circumstances, to allow newer machines to be supported from
coreboot. Fewer/zero blobs is still preferred, and this is provided whenever
possible; for example, Libreboot recently [removed a
blob](https://browse.libreboot.org/lbmk.git/commit/?id=cc33974150d275140fced9cfa4b8e901b0552074) previously used for raminit
on Haswell machines, from now on allowing only the libre raminit that recently
became stable. This *binary blob reduction policy* of Libreboot will remain.

However, a minority of people disliked this move, and are yearning for a
project much like the old one. Libreboot used to be entirely blob-free; coreboot
is otherwise free software, but requires blobs on some mainboards, so Libreboot
previously could not support all mainboards from Coreboot.

Canoeboot: the blob-free fork
=============================

I've been maintaining my own fork of Libreboot since October 2023, called
Canoeboot. While it may have fewer supported mainboards than Libreboot as a
result, it complies fully with the GNU Free System Distribution Guidelines,
in providing an entirely de-blobbed coreboot distro, like Libreboot used to
be. See:

<https://canoeboot.org/>

Email sent to GNU Eval
====================

With the recent Canoeboot 20240510 release, I've decided: Canoeboot should
become a GNU project.

Therefore, I sent GNU Eval an email requesting that they review it, laying the
case for GNU membership. This will apply to *Canoeboot*, not Libreboot, since
Canoeboot is the only one of the two that fully complies with their policies.

That email is published here (replies will not be published):

<https://canoeboot.org/news/gnu.html>

In it, I make a strong case in support of membership.

Share your opinion!
===================

I encourage members
of the public to also voice their opinions about this topic. Therefore, I have
publicised this link in several places:

Thread on FSF LibrePlanet mailing list:
<https://lists.gnu.org/archive/html/libreplanet-discuss/2024-05/msg00001.html>

Thread on my Mastodon account:
<https://mas.to/@libreleah/112428793049670713>

Thread on Trisquel forums (FSF endorsed GNU+Linux distro, and de-facto stomping
ground for many debates within the FSF community):

<https://trisquel.info/en/forum/should-gnu-boot-become-gnu-canoeboot>

Discussion welcome!

Final thoughts
==============

I'm taking a very much hands-off approach with this; I've sent the initial
email and asked the public what they think, so it's up to them to say what
they think.

Whether Canoeboot is accepted or not, it will continue operating along its
current course, providing a viable coreboot distro for diehard purists who
adhere to the FSF and GNU project.

I've always believed absolutely in the Four Freedoms, as defined by the GNU
Free Software Definition; Libreboot merely takes a different approach these
days in implementing it, while Canoeboot takes an approach that is precisely
in line with that of the FSF. In so doing, I provide users with a choice of
which vision they prefer, and I try as best I can to faithfully serve both
communities.

That's all for now. I'm probably expecting that nothing will come of this, but
the intent is there. I'd be very happy though if they say yes!
