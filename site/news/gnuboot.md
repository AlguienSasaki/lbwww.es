% GNU Boot
% Leah Rowe
% 17 July 2023

People have been waiting for me to break the silence about this. I go on about
it on IRC. This article is intended to address it once and for all, offically.

I waited so long, because until recently there really wasn't anything tangible
to talk about; why talk about vaporware? Why indeed.

Introduction!
============

This doesn't need to be an overly long post, so it won't be. There is a *fork*
of Libreboot, named GNU Boot, which you can find here:
<https://savannah.gnu.org/projects/gnuboot/>

Unofficial GNU Boot 20230717 release
------------------------------------

If you want to skip the lecture, just read these first and re-visit this
page (the one you're reading now) afterwards for more context:

* **GNU Boot 20230717, unofficial release (produced by *me*):
  <https://gnuboot.vimuser.org/news/gnuboot20230717.html> - based on the
  recent [Libreboot 20230625](libreboot20230625.md) release**, but modified to
  comply with their policy, as best as I could approximate.
  I *encourage them* to re-use this work. It's roughly *8 months* ahead
  of their current work.

Or generally: **<https://gnuboot.vimuser.org/> - Unofficial GNU Boot website**

I call this unofficial fork *GNU Boot*, specifically because I want the work
to be used *by* the real GNU Boot project. It is also clearly marked unofficial,
on that website, so people don't get confused about that.

These links, above, are for an *unofficial* fork of Libreboot that *I* have
done myself, proposed for re-use by the new GNU Boot project. I am *not* a
member of the GNU Boot project, but I do want to see it succeed.

GNU Boot? What is that, you ask me? It is a fork of Libreboot by the GNU
project, but it currently does not have a website and does not have any
releases of its own. My intent is to *help them*, and they are free - encouraged -
to re-use my work, linked above.

GNU forked Libreboot?
=====================

Why?
----

They forked Libreboot, due to disagreement with Libreboot's [Binary Blob
Reduction Policy](policy.md). This is a pragmatic policy, enacted in November
2022, to increase the number of coreboot users by increasing the amount of
hardware supported in Libreboot. Libreboot's [Freedom
Status](../freedom-status.md) page describes in great detail, how that policy
is implemented - the last few Libreboot releases have *vastly* expanded the
list of hardware supported, which you can read [here](../docs/hardware/).

I wish GNU Boot all the best success. Truly. Although I think their project is
entirely misguided (for reasons explained by modern Libreboot policy), I do
think there is value in it. It provides continuity for those who wish to use
something resembling the old Libreboot project; some context:

osboot
------

Previously, another project
started by me named [osboot](https://web.archive.org/web/20220714144846/https://osboot.org/)
existed - osboot, created in December 2020, ran for just under two years as
a separate project, and it very much resembled what Libreboot is today.

osboot was a fork of Libreboot, that I created *myself*, and maintained in
parallel to Libreboot. The old osboot Git repositories are *still available*
here, archived for historical purposes: <https://notabug.org/osboot>

osboot/libreboot merge
----------------------

In November 2022, I *shut down* osboot's website and redirected it to the
Libreboot website, merging all of its documentation and additional code into
Libreboot. Libreboot *adopted* OSBoot policy, verbatim. The [Binary Blob
Reduction Policy](policy.md) *is* that policy - the [old Libreboot
policy](https://web.archive.org/web/20221107235850/https://libreboot.org/news/policy.html)
was declared obsolete, and abandoned - the main problem with it, and the problem
with GNU Boot today which is based on it, is that it limited the amount of
hardware that Libreboot could support.

OSBoot was always the superior project, and Libreboot was practically dead,
so I saw nothing to lose and just did it. I merged them together.

So why talk about GNU Boot?
===========================

Ordinarily, I would ignore other projects; it's not that I'm bothered by them,
it's just that I have Libreboot, which pleases me, and therefore I have no need
to worry about the others. They can sort themselves out. I work collaboratively
with a few other coreboot distros; for example, I sometimes provide advice or
ideas to the [Heads](https://osresearch.net/) project (a very interesting
project, superior to Libreboot in many ways). I recently helped them by offering
to host tarballs for them, that they use in their build system.

But that's just the problem: when GNU Boot first launched, as a failed *hostile
fork* of Libreboot (at domain name `libreboot.at`), I observed: their code repository
was based on Libreboot from late 2022, and their website based on Libreboot in
late 2021. Their same-named Libreboot site was announced during LibrePlanet
2023, by this video:
<https://media.libreplanet.org/u/libreplanet/m/taking-control-over-the-means-of-production-free-software-boot/> - 
their speaker is Denis Carikli, an early contributor to Libreboot, who you can
read about here: <https://libreplanet.org/2023/speakers/#6197>. Denis is one
of the founders of that project.

Well, now they are calling themselves *GNU Boot*, and it is indeed GNU, but it
still has the same problem as of *today*: still based on very old Libreboot,
and they don't *even* have a website. According to Savannah, GNU Boot was
created on 11 June 2023. Yet no real development, in over a month since then.

I have this itch in the back of my mind, that says: if you're going to do
something, you should *do it*. When someone expresses disagreement with what
I say, I can respect it if the it's more than just words. Which is precisely
what they have been.

I value *technical excellence*.

So *why talk about it??*
---------------------

Simple: I've decided that I want to **help them**. Refer to the links above, in
the early section of this article. I decided recently that I'd simply make a
release *for them*, exactly to their specifications (GNU Free System
Distribution Guidelines), talking favourably about FSF/GNU, and so on. I'm in
a position to *do it* (thus scratching the itch), so why not?

**I did this release for them:
<https://gnuboot.vimuser.org/news/gnuboot20230717.html>** - it's designated *GNU
Boot 20230717*, and I encourage them to re-use this in their project, to get
off the ground. This completely leapfrogs their current development; it's
months ahead. *Months*. **It's 8 months ahead**, since their current revision
is based upon Libreboot from around ~October 2022.

The most remarkable thing of all is this: in December 2022 is when I first
learned of their supposed effort. They tried to poach several Libreboot developers
behind my back,
but none of them were interested it seems, and one of them leaked the existence
of their effort to me. I knew *three months* before they announced that they
were going to announce something, and I reliably predicted it'd be at LibrePlanet.

The most absurd thing of that is: why did they not contact *me*?

The GNU people should have simply contacted me from the start. I *would* have
helped them. I did Libreboot releases under their policies for *years*, and I
know what I'm doing. Ideology aside, I enjoy fun technical challenges; I have a
wide depth of knowledge and expertise. *I offer it now*, as I have today, and
will continue to do so. I offer my *support*, in service to it, even if I would
personally never use nor recommend their project. One of the purposes of today's
article is simply to tell people they exist, because I hope maybe they'll get
more devs. They use the same build system as Libreboot, so Libreboot could even
merge a lot of any actual code/ideas that they produce (and they can merge our
work - *and I want them to do that*).

There were/are more things to talk about, but I'm not really interested in
writing more. Free as in freedom? Libreboot is a free software project, yet
GNU propaganda says otherwise.

GNU Boot is an [inferior](../policy.md#problems-with-fsdg) free software
project, and Libreboot still provides the same blob-free configurations on
mainboards when that is possible, so GNU Boot is also a *superfluous* project,
just as Libreboot was before I merged osboot with it, but I digress.

What more is there to say?

Happy hacking!
