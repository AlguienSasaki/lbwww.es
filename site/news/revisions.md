% NEW Libreboot 2025 release schedule
% Leah Rowe
% 9 May 2025

**THIS REPLACES THE [original 2025 release schedule](schedule.md).**

This policy change was enacted on 9 May 2025. The original plan was to
have April/June and October/December releases each for, for testing and
stable releases respectively, but this is pointless.

Instead, we will have stable releases in June and December each year, and
any releases in-between will either be a revision to the current stable
release, or an RC to the next release.

RC (release candidate) releases, when they are made, will later be deleted
when a new RC comes out, or the next main stable release comes out. This is
more logical, and essentially the same as the original plan, except that
we will then no longer have old testing releases lingering on the rsync
server, moving forward.

To make everything clear, the next sections will clarify this in detail
anyway, as the original release schedule article did:

RC/Stable release cycle
----------------------------

These rules shall govern Libreboot release cycles, in the year 2025 and in
years beyond 2025.

### June and December

In other words, the following Libreboot releases will come out in 2025:

* Libreboot 25.06 (stable release)
* Libreboot 25.12 (stable release)

A [Libreboot 25.04](libreboot2504.md) release was also made, under the
previous plan. It won't actually be renamed, but you might now therefore
regard that as an RC to the 25.06 release.

A general rule of thumb is that an RC should be made no later than about two
to three months before the stable release, with many RC revisions made
throughout, until it is replaced by the real stable release.

For example, an RC release might come out in March of a given year, and that
would continue to be refined until it got replaced by the June release. The
same logic applies for September to December.

Aggressive development periods can continue throughout.

Each release series will get its own branch; the 25.06 release will have
the `25.06_branch` branch on the Git repository of lbmk. We currently, at
the time of this article, have a `25.04_branch` branch; this will later branch
off to `25.06_branch`, as soon as the next commits go into it.

Releases will have codenames too; please read further down this article, for
information about that. The April 2025 release, Libreboot 25.04, was:
Libreboot 25.04 "Corny Calamity".

Corny Calamity won't be the codename for Libreboot 25.06; the June 2025
release will be named *Luminous Lemon*.

### Rules for revisions

RC releases come out two to three months before stable; any number of
non-breaking or otherwise relatively safe changes can be made. In practise,
the master branch of lbmk mostly uses a branchless development model anyway.

In practise, what this means is that there will be a feature freeze a little
while before a given stable release, but only in that release's own branch;
the master branch can continue to receive aggressive development.

For example, if a major new feature is being done in the master branch that
won't be ready for the next release, that's OK, it can continue in master,
because the release will have its own branch anyway.

### Post-stable revisions

Revision releases can be made *after* a stable release comes out. A pre-release
RC (release candidate) shall have `rc` in it, for example `25.06rc1` commit
tag. If `25.06` then came out, a stable release, and later a new update to it
would be made, the first update release would be tagged as `25.06rev1`.

Stable release revisions must not fundamentally alter the substance of a given
stable release, relative to first release. Essentially, any revisions to stable
releases post-release will be critical bug fixes; in a few cases, special
additions will be made when desirable and safe (e.g. Pico 2 support was added
to Libreboot 20241206 post-release, in the revision 8 update from 2025-01-06).

Revisions made to a stable release, after the RC phase, must basically only
be critical bug fixes and especially security fixes. Any change of substance
should go into the master branch and/or the branch for the next release.

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

For example, the April 2025 release was Libreboot 25.04. This was a one-off
release, under the [previous 2025 schedule](schedule.md).

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

### TL DR

This is basically the same as the original 2025 release schedule, except
that we won't have testing releases anymore; we will have RC releases, and RC
releases will not stay on rsync forever. Therefore, each year, Libreboot
will only have two new sets of release tarballs uploaded to its servers,
instead of four. This is more efficient.

So in substance, this is the same but re-fashioned to make the project cheaper
to run.
