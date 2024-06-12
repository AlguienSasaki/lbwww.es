---
title: Downloads
x-toc-enable: true
...

New releases are announced in the [main news section](news/).

If you're more interested in libreboot development, go to the
[libreboot development page](../git.md), which also includes links to the
Git repositories. The page on [/docs/maintain/](docs/maintain/) describes how
Libreboot is put together, and how to maintain it. If you wish to build
Libreboot from source, [read this page](docs/build/).

Buy Libreboot pre-installed
-----------------

If you want professional installation, Minifree Ltd sells [Libreboot
pre-installed](https://minifree.org/) on select hardware, and it also provides
a [Libreboot preinstall service](https://minifree.org/product/installation-service/)
if you want to send your machine in to have Libreboot installed for you.

Leah Rowe, the founder and lead developer of Libreboot, also owns and
operates Minifree Ltd; sales provide funding for the Libreboot project.

Safety warning
--------------

**IMPORTANT ADVICE: [PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING/UPDATING
LIBREBOOT](news/safety.md).**

GPG signing key
---------------

**The latest release is Libreboot 20240612, under the `stable` directory.**

### NEW KEY

Full key fingerprint: `8BB1 F7D2 8CF7 696D BF4F  7192 5C65 4067 D383 B1FF`

This key is for Libreboot releases *after* the 20240126 release. It applies to
all Libreboot releases from the year 2024, and it will expire (unless revoked
early) on 26 December 2028.

Download the key here: [lbkey.asc](lbkey.asc)

Libreboot releases are signed using GPG.

### OLD KEY

Full key fingerprint: `98CC DDF8 E560 47F4 75C0  44BD D0C6 2464 FA8B 4856`

This key is for Libreboot releases *after* the 20160907 release, and up
to the Libreboot 20240126 release. This key *expired* during December 2023,
so you should use the *newer* key (see above) for the releases after
Libreboot 20240126.

Download the key here: [lbkey.asc](lbkeyold.asc)

Libreboot releases are signed using GPG.

### OLD OLD KEY:

This key is for Libreboot 20160907, and releases older than 20160907:

Full key fingerprint: CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2

The GPG key can also be downloaded with this exported dump of the
pubkey: [lbkeyold.asc](lbkeyoldold.asc).

	sha512sum -c sha512sum.txt
	gpg --verify sha512sum.txt.sig

Git repository
--------------

Links to regular release archives are listed on this page.

However, for the absolute most bleeding edge up-to-date version of Libreboot,
there is a Git repository that you can download from. Go here:

[How to download Libreboot from Git](git.md)

HTTPS mirrors {#https}
-------------

**The latest release is Libreboot 20240612, under the `stable` directory.**

These mirrors are recommended, since they use TLS (https://) encryption.

You can download Libreboot from these mirrors:

* <https://mirror.math.princeton.edu/pub/libreboot/> (Princeton
university, USA)
* <https://mirror.shapovalov.website/libreboot/> (shapovalov.website, Ukraine)
* <https://www.mirrorservice.org/sites/libreboot.org/release/> (University
of Kent, UK)
* <https://mirrors.mit.edu/libreboot/> (MIT university, USA)
* <https://mirror.koddos.net/libreboot/> (koddos.net, Netherlands)
* <https://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)
* <https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, France)

RSYNC mirrors {#rsync}
-------------

The following rsync mirrors are available publicly:

* <rsync://rsync.mirrorservice.org/libreboot.org/release/> (University of Kent,
UK)
* <rsync://mirror.math.princeton.edu/pub/libreboot/> (Princeton university, USA)
* <rsync://rsync.shapovalov.website/libreboot/> (shapovalov.website, Ukraine)
* <rsync://ftp.linux.ro/libreboot/> (linux.ro, Romania)
* <rsync://mirror.koddos.net/libreboot/> (koddos.net, Netherlands)
* <rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)

Are you running a mirror? Contact the libreboot project, and the link will be
added to this page!

You can make your rsync mirror available via your web server, and also configure
your *own* mirror to be accessible via rsync. There are many resources online
that show you how to set up an rsync server.

How to create your own rsync mirror:

Useful for mirroring Libreboot's entire set of release archives. You can put
an rsync command into crontab and pull the files into a directory on your
web server.

If you are going to mirror the entire set, it is recommended that you allocate
at least 25GiB. Libreboot's rsync is currently about 12GiB, so allocating 25GiB
will afford you plenty of space for the future. At minimum, you should ensure
that at least 15-20GiB of space is available, for your Libreboot mirror.

*It is highly recommended that you use the libreboot.org mirror*, if you wish
to host an official mirror. Otherwise, if you simply want to create your own
local mirror, you should use one of the other mirrors, which sync from
libreboot.org.

Before you create the mirror, make a directory on your web server. For 
example:

	mkdir /var/www/html/libreboot/

Now you can run rsync, for instance:

	rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

You might put this in an hourly crontab. For example:

	crontab -e

Then in crontab, add this line and save/exit (hourly crontab):

	0 * * * * rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

**It's extremely important to have the final forward slash (/) at the end of each path,
in the above rsync command. Otherwise, rsync will behave very strangely.**

**NOTE: `rsync.libreboot.org` is not directly accessible by the public, except
those whose IPs are whitelisted. For bandwidth reasons, the firewall running
on libreboot.org blocks incoming rsync requests, except by specific IPs.**

**If you wish to run an rsync mirror, sync from one of the third party mirrors
above and set up your mirror. You can then contact Leah Rowe, to have your IP
addresses whitelisted for rsync usage - if the IP addresses match DNS A/AAAA
records for your rsync host, this can be used. A script runs in an hourly
crontab on libreboot.org, that fetches the A/AAAA records of whitelisted
rsync mirrors, automatically adding rules permitting them to get through the
firewall.**

If you wish to regularly keep your rsync mirror updated, you can add it to a
crontab. This page tells you how to use crontab:
<https://man7.org/linux/man-pages/man5/crontab.5.html>

HTTP mirrors {#http}
------------

**The latest release is Libreboot 20240612, under the `stable` directory.**

WARNING: these mirrors are non-HTTPS which means that they are
unencrypted. Your traffic could be subject to interference by
adversaries. Make especially sure to check the GPG signatures, assuming
that you have the right key. Of course, you should do this anyway, even
if using HTTPS.

* <http://mirror.linux.ro/libreboot/> (linux.ro, Romania)
* <http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Germany)

FTP mirrors {#ftp}
-----------

**The latest release is Libreboot 20240612, under the `stable` directory.**

WARNING: FTP is also unencrypted, like HTTP. The same risks are present.

* <ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (University
of Kent, UK)
* <ftp://ftp.linux.ro/libreboot/> (linux.ro, Romania)

Statically linked
------------------

Libreboot includes statically linked executables in some releases, built from
the available source code. Those executables have certain libraries built into
them, so that the executables will work on many Linux distros.

To comply with GPL v2, source ISOs are supplied by the
Libreboot project. You can find these source ISOs in the `ccsource` directory
on the `rsync` mirrors.

Libreboot releases past version 20160907 do not distribute statically linked
binaries. Instead, these releases are source-only, besides pre-compiled ROM
images for which the regular Libreboot source code archives suffice. These newer
releases instead automate the installation of build dependencies, with instructions
in the documentation for building various utilities from source.

These executables are utilities such as `flashprog`.
