% Formalised Libreboot 2025 release schedule
% Leah Rowe
% 17 January 2025

**THIS RELEASE SCHEDULE IS OBSOLETE. PLEASE READ
THE [NEW SCHEDULE](revisions.md) instead - there will instead be just TWO
releases annually, and a Release Candidate stage will be used. Therefore,
the Libreboot 25.04 release, which did come out, might instead be
considered Libreboot 25.06 RC1 - except an actual RC1 is planned, so just
think of it as RC0 instead!.**

TL DR, the new schedule is basically the same as below, but only a June and
December release will be done; releases in-between will be either a revision
release to the previous release, or an RC to the next release.

This policy change was enacted on 9 May 2025.

And now, the original article:

Libreboot releases will now be on a much stricter timeline than in the past.

Testing/Stable release cycle
----------------------------

### April/June and October/December

As alluded to in the Libreboot 20241206 release, a new release schedule is
planned for 2025 onward:

* April/October will be for testing releases.
* June/December will be for stable releases.

In other words, the following Libreboot releases will come out in 2025:

* Libreboot 25.04 (testing release)
* Libreboot 25.06 (stable release)
* Libreboot 25.10 (testing release)
* Libreboot 25.12 (stable release)

So, two testing releases and two stable releases. Each release can have any
number of revisions, whereby the tarballs are re-generated and re-uploaded,
replacing old ones, as was done on the 20241206 and 20240612
releases.

Releases will have codenames too; please read further down this article, for
information about that. The April 2025 release will
be: Libreboot 25.04 "Corny Calamity".

This .4, .6, .10 and .12 scheme will continue in 2026 and beyond.

### Rules for revisions

Testing releases come out two months before stable; any number of non-breaking
or otherwise relatively safe changes can be made, and testing releases will
stop receiving new revisions about 3-4 weeks before the stable release. Any
number of improvements can be made, including massive updating of upstream
code revisions.

Stable release revisions must not fundamentally alter the substance of a given
stable release, relative to first release. Essentially, any revisions to stable
releases post-release will be critical bug fixes; in a few cases, special
additions will be made when desirable and safe (e.g. Pico 2 support was added
to Libreboot 20241206 post-release, in the revision 8 update from 2025-01-06).

### Why?

In the past, a problem Libreboot has had was that we'd do testing releases,
but not do revisions on them; then by the time a stable release came around,
some upstream revisions would be about 4-6 months out of date (typically).

With this new formalised structure, we can be as close to upstream as possible
by the time of each stable release, for each given upstream e.g. coreboot.

This release schedule will also provide greater opportunity for coverage of
Libreboot releases, since people know then what to expect and what dates to
put in their calendars.

This decision is part of a much larger initiative to boost Libreboot's
popularity and therefore use within the free software world.

Release version numbers
-----------------------

### Libreboot YY.MM releases

Libreboot YYYYMMDD was the previous version number scheme.

The new scheme is: Libreboot YY.MM

For example, the April 2025 release will be Libreboot 25.04.

The new release scheme is better, because we presently need to start release
builds early in the morning to ensure (given build time and possible errors
to be fixed) that they are released on the day. For example, a 20250404 release
would have to come out on April 4th, so if it finished compiling on April 5th,
it would become Libreboot 20250405 under the previous scheme.

With this new scheme, and given Libreboot's expansion plans, it won't matter
even if a release build takes 2 days to complete, because the day of the month
will no longer be included in a given release number.

### Y2.1k compliance

If Libreboot still exists in the year 2100, then those releases will be
e.g. Libreboot 100.04 for April 2100 release.

It could happen. Even if I'm no longer around by then, Libreboot might still be.

### Release codenames

The release codename for Libreboot 25.04 will be "Corny Calamity", which is a
nod of respect to an equally gutsy release codename that *Fedora* used in
one of their releases (Beefy Miracle!).

The rules for Libreboot release codenames are: two words, each starting with
the same letter.

Credit
------

[Jane Arkanian](https://janethemotherfucker.github.io/) was the one who suggested
to me that I use this new release number and codename scheme, and I previously
came up with the plan to use an April/June and October/December testing/stable
release schedule.

Jane also came up with the codename that I've decided to use for
the June 2025 stable release - stay tuned!


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
