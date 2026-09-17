% Fixed release schedule scrapped (old schedule restored)
% Leah Rowe
% 17 September 2026

See previous 6-month-release-schedule: [revisions](revisions.md). - This is
now scrapped.

Libreboot attempted to have a fixed release schedule; one release in January
and one release in December, each year.

This has been scrapped. The next release of Libreboot is expected at some time
during 2027 (today's date is 17 September 2026), after having already skipped
the scheduled June 2026 release; December 2026 is also being skipped.

That being said, I am considering whether or not to implement a more automated
build-release test. I regularly run `./mk release` in lbmk to test changes,
because I try to ensure that the master branch is always stable. I could perhaps
implement a CI, and do nightly or weekly snapshot builds. In other words,
Libreboot could become a rolling release distro with intermittent stable
releases.

I will now provide some context for this decision:

Context
-------

Extensive work is done on Libreboot, and in fact another major audit of the
Libreboot build system is underway as I write this. I already removed nearly
a thousand lines of dead code, or optimised what's left. This is done on a
semi-regular basis, similar to how an OpenBSD audit is conducted. The object
is simple: identify bugs, even ones that are not yet triggered, and fix them
preemptively. One of the best ways to prevent bugs is to to, well, prevent
them, and since fewer lines of code are easier to audit and less likely to be
buggy, one of the main focuses is to make everything as small as possible,
doing more with less. The overall design of the build system is also audited,
fixing and/or implementing safety features in the build system, to make it
more robust. Libreboot's entire infrastructure has
a [unique and highly opinionated design](../docs/maintain/) which you will
probably never find anywhere else. The reason the project can be maintained
by such a small team, usually by just one person alone, is precisely because
of this auditing process. Everything is kept simple and maintainable.

These audits are constant, and the entire design of Libreboot's infrastructure
has been improved incrementally over time. It has improved by leaps and
bounds over the years, and the build system is where all of the magic happens.
Build systems are often overlooked in projects like this, but they are the most
important part; they make or break your Linux distro, or indeed coreboot distro
as Libreboot is. A good design is not only easy to understand, but easier to
add features to over time, and to modernise over time. Libreboot's build system
focuses on reducing the maintenance burden, eliminate technical debt (and
prevent it), strict, clean coding style and on minimal dependencies. The design
can be found here: [../docs/maintain](../docs/maintain/). The very design of
lbmk is a direct result of this ongoing audit; the design of the build system
has evolved over time, to reflect how the project is developed. Libreboot does
not abstract most things, even toolchains. The goal of Libreboot's design is
to literally reduce the amount of code that is running on the user's machine,
so that fewer bugs code occur, because code equals bugs. It doesn't use fancy
technologies like Docker (there is no containment at all), doesn't use complex
frameworks like Nix and... well, the build system is literally written in
POSIX shell scripts, for example.

Auditing is not the only aspect of Libreboot. The ultimate goal of the project
has always been to provide something that is robust and easy to use for normal
non-technical people. That's why the build system is audited as intensely as
it is, with efforts made to reduce the amount of required configuration by
the user. Sane defaults for everything, and automate everything - Just Work,
in other words.

More generally, I'm looking to add a lot more newer mainboards to Libreboot,
and this will mean that several modernisation efforts also need to be conducted;
edk2 payload for example, if we want newer x86 machines as UEFI is all but
required nowadays (GOP support for graphics cards. SeaBIOS often doesn't work
anymore with modern cards because modern cards no longer have VGA images).

What's important to understand is that this work is intense, and sporadic;
it happens in bursts. Sometimes nothing is done for months, and that is likely
to always be the case with a project like this. It's just that the stars so
happened to align when the 6-month-release schedule was first announced, such
that it was just about kept.

However, every release I did under that schedule felt rushed. I felt that each
release was not as good as it could be, nor as polished as it could be, and
doing releases on specific months felt arbitrary. I would get to my own
self-imposed release dates - January and June - feeling like I had a million
different changes I wanted to make, that would just be set aside.

The reason the schedule is scrapped is because although a lot of work is done
on Libreboot, there are sometimes periods where very minimal amounts of work
are done to the project. This means that the project will sometimes do four
releases in a year, other years one, and other years none.

When is the next release?
-------------------------

I'm expecting to have at least something RC-worthy ready by about December
2026, and then work will be done throughout 2027 until a stable release is
done. Note that this is not an ETA, just a prediction, and it's likely to
be correct at this time.

The current stable release is Libreboot 26.01 released in January 2026. This is
likely to be the only stable release in 2026. Next one is expected by the
summer of 2027.

That is all. Not really much else to say. I decided that since I failed to
do a June 2026 release (Libreboot 26.06), and that a December release is just
as unlikely, then it makes sense to acknowledge reality.

Libreboot therefore assumes its old release schedule. The old (and now
current) schedule is: release when ready. This schedule is better, because it
means that lots of polishing can be done at a comfortable pace, and releases
will not be rushed.

I considered that it would be prudent to confirm all of this in an article,
because some people who might have seen the previous schedule might have
otherwise worried.

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

Just build it yourself
----------------------

Even if the next release is out yet, you can always just download Libreboot's
git repository (lbmk) and [build it yourself](../docs/build/).

Any decent machine will build an image for one machine in about 5-60 minutes,
depending on what hardware you use to build it on. (about 5 minutes on
e.g. alderlake/ryzen machine from early 2020s, whereas a ThinkPad T60 would
take roughly one hour to build. A ThinkPad T480 will take about 20 minutes
to build).

Libreboot's build system is regularly tested for bugs, and patches are
always welcome.

In summary
----------

Nothing has changed. This new release schedule is exactly what Libreboot has
always used since it started. A release is out when it's ready, which will be
on average every 6-24 months (usually about 8-16 months).

Yes. Nothing to see here.
