---
title: Code review
x-toc-enable: true
...

libreboot repositories
===================

The `libreboot` project has 3 main Git repositories:

* Build system: <https://codeberg.org/libreboot/lbmk>
* Website (+docs): <https://codeberg.org/libreboot/lbwww>
* Images (for website): <https://codeberg.org/libreboot/lbwww-img>
* Pico-serprog: <https://codeberg.org/libreboot/pico-serprog>

You can also *browse* lbmk on Libreboot's own cgit instance, though it is not
intended for development (use codeberg for that):\
<https://browse.libreboot.org/lbmk.git/>

The [tasks](tasks/) page lists some (not all) of the things we wish to
implement, if not already implemented. Patches welcome!

If the main Git repositories are down, mirrors of `lbmk` and `lbwww` are listed
further down in this page

Libreboot was previously using NotABug, but it had continued reliability
issues due to HTTP 500 errors being returned, largely in the evenings, most
likely because too many people were on it; it was decided that Libreboot
needed something more stable, so now Libreboot is hosted on codeberg. See:
[announcement of move to codeberg, 8 April 2023](news/codeberg.md)

There are also these programs, hosted by the Libreboot project, and libreboot
either recommends them or makes use of them:

The `ich9utils` project is now available under `util/ich9utils` in lbmk, and
lbmk uses *that*, but the old standalone repository is still available on
notabug (bucts is also there):

* Bucts (utility): <https://notabug.org/libreboot/bucts>
* ich9utils (utility): <https://notabug.org/libreboot/ich9utils>

You can download any of these repositories, make whatever changes you like, and
then submit your changes using the instructions below.

It is recommended that you build libreboot (all parts of it) in a Linux
distribution. For example, the build system (lbmk) is untested on BSD systems.
Install `git` in your Linux system, and download one of the repositories.

Development of libreboot is done using the Git version control system.
Refer to the [official Git documentation](https://git-scm.com/doc) if you don't
know how to use Git.

The `bucts` repository is hosted by the libreboot project, because the original
repository on `stuge.se` is no longer available, last time we checked. The
`bucts` program was written by Peter Stuge. You need `bucts` if you're flashing
internally an libreboot ROM onto a ThinkPad X60 or T60 that is currently running
the original Lenovo BIOS. Instructions for that are available here:\
[libreboot installation guides](docs/install/)

The `ich9utils` repository is used heavily, by the `lbmk` build system. However,
you can also download `ich9utils` on its own and use it. It generates ICH9M
descriptor+GbE images for GM45 ThinkPads that use the ICH9M southbridge. It may
also work for other systems using the same platform/chipset.
Documentation for `ich9utils` is available here:\
[ich9utils documentation](docs/install/ich9utils.md)

lbmk (libreboot-make)
---------------------

This is the core build system in libreboot. You could say that `lbmk` *is*
libreboot! Download the Git repository:

	git clone https://codeberg.org/libreboot/lbmk

The `git` command, seen above, will download the libreboot build system `lbmk`.
You can then go into it like so:

	cd lbmk

Make whatever changes you like, or simply build it. For instructions on how to
build `lbmk`, refer to the [build instructions](docs/build/).

Information about the build system itself, and how it works, is available in
the [lbmk maintenance guide](docs/maintain/).

lbwww and lbwww-img
-------------------

The *entire* libreboot website and documentation is hosted in a Git repository.
Download it like so:

	git clone https://codeberg.org/libreboot/lbwww

Images are hosted on <https://av.libreboot.org/> and available in a separate
repository:

	git clone https://codeberg.org/libreboot/lbwww-img

Make whatever changes you like. See notes below about how to send patches.

The entire website is written in Markdown, specifically the Pandoc version of
it. The static HTML pages are generated with [Untitled](https://untitled.vimuser.org/).
Leah Rowe, the founder of libreboot, is also the founder of the Untitled static
site generator project.

If you like, you can set up a local HTTP server and build your own local
version of the website. Please note that images will still link to the ones
hosted on <https://av.libreboot.org/>, so any images that you add to `lbwww-img`
will not show up on your local `lbwww` site if you make the image links (for
images that you add) link to `av.libreboot.org`. However, it is required that such
images be hosted on av.libreboot.org.

Therefore, if you wish to add images to the website, please also submit to the
`lbwww-img` repository, with the links to them being
<https://av.libreboot.org/path/to/your/new/image/in/lbwww-img> for each one.
When it is merged on the libreboot website, your images will appear live.

If adding a photo, compress it for web distribution. Images should be about
800px wide, and usually under 100KiB in size:

First, scale your image down to approximately 800px width, using your favourite
image manipulation program. For example, with `imagemagick` you can do the
following (make sure the image isn't already smaller or equal than preferred).

	convert original.jpg -resize 600000@ -quality 70% web.jpg

You should always run `jpegoptim` on jpg images before submitting them.
It strips useless metadata and *losslessly* optimises them further by cleverly
rearranging the huffman tables used in them.

	jpegoptim -s --all-progressive web.jpg

If the image is a (line) drawing, vector graphics are preferable to bitmaps.
Therefore, if possible, save them as SVGs. Those are easy to modify,
and will surely make translators' work easier as well.

PNG images should be optimised with `zopfli` (this is lossless as well).
For example, this reduced the Libreboot boot logo from around 11k to 3k:

	zopflipng -ym image.png image.png

For development purposes, you might make your images local links first, and
then adjust the URLs when you submit your documentation/website patches.

Instructions are on the Untitled website, for how to set up your local version
of the website. Download untitled, and inside your `untitled` directory, create
a directory named `www/` then go inside the www directory, and clone the `lbwww`
repository there. Configure your local HTTP server accordingly.

Again, instructions are available on the Untitled website for this purpose.

Name not required
-----------------

Contributions that you make are publicly recorded, in a Git repository which
everyone can access. This includes the name and email address of the
contributor.

In Git, for author name and email address, you do not have to use identifying
data. You can use `libreboot Contributor` and your email address could be
specified as contributor@libreboot.org. You are permitted to do this, if
you wish to maintain privacy. We believe in privacy. If you choose to remain
anonymous, we will honour this.

Of course, you can use whichever name and/or email address you like.

Legally speaking, all copyright is automatic under the Berne Convention of
international copyright law. It does not matter which name, or indeed whether
you even declare a copyright (but we do require that certain copyright
licenses are used - read more about that on this same page).

If you use a different name and email address on your commits/patches, then you
should be fairly anonymous. Use
[git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
and [git show](https://git-scm.com/docs/git-show) to confirm that before you
push changes to a public Git repository.

Licenses (for contributors)
--------

Make sure to freely license your work, under a libre license. Libreboot no
longer sets arbitrary restrictions on what licenses are accepted, and many
licenses out there already exist. We will audit your contribution and tell
you if there are problems with it (e.g. no license).

*Always* declare a license on your work! Not declaring a license means that
the default, restrictive copyright laws apply, which would make your work
proprietary, subject to all of the same restrictions.

The MIT license is a good one to start with, and it is the preferred license
for all new works in Libreboot, but we're not picky. Libreboot has historically
used GNU licensing such as GPL; much of that remains, and is likely to remain.
It's your work; obviously, if you're deriving from an existing work,
it may make sense to use the same license on your contribution, for license
compatibility.

You can find common examples of licenses
[here](https://opensource.org/licenses).

If you *are* deriving from an existing work, it's important that your license
(for your contribution) be compatible with the licensing of the work from which
yours was derived. The MIT license is good because it's widely compatible
with many other licenses, and permits many freedoms (such as the freedom to
sublicense) that other licenses do not:

<https://opensource.org/licenses/MIT>

Send patches
------------

Make an account on <https://codeberg.org/> and navigate (while logged in) to the
repository that you wish to work on. Click *Fork* and in your account,
you will have your own repository of libreboot. Clone your repository, make
whatever changes you like to it and then push to your repository, in your
account on Codeberg. You can also do this on a new branch, if you wish.

In your Codeberg account, you can then navigate to the official libreboot
repository and submit a Pull Request. The way it works is similar to other
popular web-based Git platforms that people use these days.

You can submit your patches there. Alternative, you can log onto the libreboot
IRC channel and notify the channel of which patches you want reviewed, if you
have your own Git repository with the patches.

Once you have issued a Pull Request, the libreboot maintainers will be notified
via email. If you do not receive a fast enough response from the project, then
you could also notify the project via the `#libreboot` channel on Libera Chat.

Another way to submit patches is to email Leah Rowe directly:
[info@minifree.org](mailto:info@minifree.org) is Leah's project email address.

However, for transparency of the code review process, it's recommended that you
use Codeberg, for the time being.

Mailing list
------------

Libreboot has this mailing list:
<https://lists.sr.ht/~libreboot/libreboot>

The email address is [~libreboot/libreboot@lists.sr.ht](mailto:~libreboot/libreboot@lists.sr.ht)

Mirrors of lbmk.git
===================

The `lbmk` repository contains Libreboot's automated build system, which
produces Libreboot releases (including compiled ROM images).

You can run `git clone` on any of these links (the links are also clickable,
to view changes in your Web browser):

* <https://git.sr.ht/~libreboot/lbmk>
* <https://git.disroot.org/libreboot/lbmk>
* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://git.fosscommunity.in/libreboot/lbmk>
* <https://0xacab.org/libreboot/lbmk/>
* <https://framagit.org/libreboot/libreboot>
* <https://gitlab.com/libreboot/lbmk>
* <https://pagure.io/libreboot>
* <https://rocketgit.com/libreboot/libreboot>

Mirrors of pico-serprog.git
=========================

* <https://notabug.org/libreboot/pico-serprog>

lbwww.git mirror
----------------

The `lbwww` repository contains Markdown files (pandoc variant), for use
with the [Untitled Static Site Generator](https://untitled.vimuser.org/); this
is what Libreboot uses to provide HTML web pages, *including* the page that
you are reading right now!

You can run `git clone` on these links, and/or click to view changes in your
Web browser. See:

* <https://git.sr.ht/~libreboot/lbwww>
* <https://git.disroot.org/libreboot/lbwww>
* <https://gitea.treehouse.systems/libreboot/lbwww>
* <https://git.fosscommunity.in/libreboot/lbwww>
* <https://0xacab.org/libreboot/lbwww>
* <https://framagit.org/libreboot/lbwww/>
* <https://gitlab.com/libreboot/lbwww>
* <https://rocketgit.com/libreboot/lbwww>

NOTE: The `lbwww-img` repository is not generally provided, on mirrors, as
those are just image files which you can find on <https://av.libreboot.org>
and it is not the intention of the Libreboot project to bog down *mirrors*
with additional traffic by hosting images.

Notabug repositories
====================

Commits that go to codeberg are also still pushed to notabug, in addition to
the other mirrors. Notabug is considered a *mirror* since 8 April 2023, when
Libreboot's main development site moved to *Codeberg*.

OLD notabug repos are still pushed to as backup, but the codeberg mirror is
considered to be main/official now, as of the [announcement on 8
April 2023](news/codeberg.md). See:

* Build system: <https://notabug.org/libreboot/lbmk>
* Website (+docs): <https://notabug.org/libreboot/lbwww>
* Images (for website): <https://notabug.org/libreboot/lbwww-img>

For sending patches, it is now preferred that you use *codeberg*. Technically,
pull requests are still possible via Notabug. While Notabug still exists,
Libreboot patches will continue be pushed there, mirroring what gets pushed
on Notabug.

Because pull requests and issues were available on notabug in the past, it
makes sense to keep them open, though we ask that you send to codeberg. If
they were to be closed on notabug, existing PRs and issues won't be visible
anymore either, so they have to stay open.
