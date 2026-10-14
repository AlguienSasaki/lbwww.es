% Fixed release schedule scrapped (old schedule restored)
% Leah Rowe
% 17 September 2026

See previous 6-month-release-schedule: [revisions](revisions.md). - This is
now scrapped. We will continue to make regular releases, but no longer on a
fixed schedule. Thus, we have restored the old schedule in which we still
usually did at least one (often more than two) releases each year.

Libreboot attempted to have a fixed release schedule; one release in January
and one release in December, each year. The problem with a fixed schedule is
that there's sometimes not a lot of work to do, or I don't feel like doing as
much, but then I'll have burst periods in which lots of work gets done. Thus,
the older and more flexible release schedule was much better.

~~The next release of Libreboot is expected at some time
during 2027 (today's date is 17 September 2026), after having already skipped
the scheduled June 2026 release; December 2026 is also being skipped.~~
**[Libreboot 26.10 was released in October 2026!](libreboot2610.md) - and
another one probably planned for December 2026. Libreboot is rapidly expanding
and moderning in recent development; we added EDK2 payload support for example,
which can boot modern operating systems (including Windows).**

~~That being said, I am considering whether or not to implement a more automated
build-release test. I regularly run `./mk release` in lbmk to test changes,
because I try to ensure that the master branch is always stable. I could perhaps
implement a CI, and do nightly or weekly snapshot builds. In other words,
Libreboot could become a rolling release distro with intermittent stable
releases.~~ - too much work. The nature of Libreboot is such that use of a CI
is ill-advised; the `./mk test` feature stress-tests Libreboot's build process
just fine, and I run this regularly.

Yes. No more six-month schedule. Libreboot's new schedule is: release whenever
I feel like it.

What hasn't changed?
--------------------

Release codenames will still be used! That was another feature of the six
month schedule; the current release is codenamed *Magnificent Max*. This was
inspired by my time back in the day when I used Ubuntu releases, or laughed
at some of Fedora's whacky release codenames (Beefy Miracle!)

The *YY.MM* release version numbers will also stay in place, for the next
release. For example, a July 2027 release would become Libreboot 27.07
Magnificent Max, and that codename is definitely not going to be the next
release codename, but it would probably be a good one.

In summary
----------

Nothing has changed. This new release schedule is exactly what Libreboot has
always used since it started. A release is out when it's ready, which will be
on average every 6-24 months (usually about 8-16 months).

The fixed schedule was a temporary, now-failed experiment. We once again use
our previous, much more flexible schedule of release-when-ready, the way it
should be.
