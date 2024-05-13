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

The benefit of both Canoeboot and Libreboot is that they provide a completely
automated build process and installation procedure, with well-tested builds
released on a regular basis that the user can simply install, with minimal
fuss. You can think of it as a *coreboot distro*. Coreboot provides snapshot
source code archives every few months, but that's it. Libreboot/Canoeboot gives
you the ROM images pre-compiled, with source code, and with payloads
already pre-configured. In other words: Canoeboot and Libreboot make coreboot
extremely easy to use.

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

And reddit: <https://www.reddit.com/r/linux/comments/1cqe924/should_canoeboot_become_gnu_canoeboot/>

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

Follow-up
=========

In addition to the [original message](https://canoeboot.org/news/gnu.html#email-sent-to-gnu-eval), I also sent this message to GNU Eval:

some people have actually asked me if i should contribute to GNU Boot, instead of trying to replace it with GNU Canoeboot. They have asked this outside of this email discussion, but some here may be wondering that.

I'd like now to address it. Here goes:

I actually submitted extensive patches to gnuboot in January 2024:

<https://lists.gnu.org/archive/html/gnuboot-patches/2024-01/index.html>

<https://web.archive.org/web/20240511074211/https://lists.gnu.org/archive/html/gnuboot-patches/2024-01/index.html>

The patches were never reviewed, let alone merged, but they:

* Replace "Libreboot" with "GNU Boot" in several places on the documentation (**THIS IS THE ONLY PATCH THEY MERGED**)
* Fix major build issues, allowing GNU Boot to build on modern distros (GNU Boot only builds on Trisquel 10, my patches make it build on 12. also Gentoo-libre and Arch/Parabola)
* Add Dell Latitude E6400 support
* Fix hang in GRUB caused when there is a stuck key, by disabling the "Unknown key" spew message
* ESP and btrfs subvol support in grub.cfg
* Fixes building KGPE-D16 on newer distros, by skipping GNAT which isn't needed
* Add gru bob support (rk3399 chromebooks, with free EC and *no* microcode) - ditto gru kevin
* Keyboard fix for GRUB: force it to use scancode set 2 translated, instead of untranslated set 2 to work around buggy ECs such as Dell Latitude E6400
* Avoid spewing the Unknown prefix message in GRUB
* Adds the dell-flash-unlock tool from Nicholas Chin, allowing internal flashing from factory BIOS to GNU Boot, on the E6400
* cache cbfstool/ifdtool builds to speed up build time
* better caching of coreboot rom images during build, to speed up build time
* Prevent future GRUB build errors by disabling -Werror
* Support for *building* U-Boot as a coreboot payload, on gru bob/kevin chromebooks (GNU Boot only archives it, doesn't build it)

A second patch set that I sent does the above, and also:

* Updates GRUB, coreboot and SeaBIOS to newer revisions from late 2023 (GNU Boot uses late 2021 revisions)
* Adds Argon2 KDF support, for booting from LUKS2-encrypted /boot (GNU Boot can't boot from encrypted LUKS2 /boot without this patch)
* Reduced the number of modules in GRUB to only those needed, saving 100KB of space in flash
* Update memtest86+ to v6.x instead of 5.x

I sent all of these patches to GNU Boot while bored, and it only took me 1 day to implement all of them, re-using what I had done in Canoeboot months beforehand

My patches fixed all of the fundamental issues with GNU Boot, without rewriting the build system; they use an older version of the Libreboot build system, prior to my re-write of the latter half of 2023 (my re-write makes the build system much smaller and more efficient.

All of the above improvements *and much more* is in Canoeboot. In terms of development, Canoeboot is about 2 years ahead of Canoeboot; Canoeboot has since far surpassed the improvements sent to GNU Boot in January, so even if they did merge them now, they'd still be behind.

I may be missing a thing or two, above, but one thing I'm not missing is my strong and stable commitment to the free software movement, even when I'm dealing with hostile project maintainers who won't even consider my patches.

This is why I will no longer assist the GNU Boot project. Because I tried to help them; and this wasn't my first attempt to help, either.

Whether GNU accepts Canoeboot or not, Canoeboot will continue to press full speed ahead. I say now it's 2 years ahead of GNU Boot;

Next year it'll be 3 years ahead.
