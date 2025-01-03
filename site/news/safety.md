% Safety issues updating Libreboot on Sandybridge/Ivybridge/Haswell
% Leah Rowe
% 7 July 2023

New safety features
===============

NOTE: This page is mostly completely obsolete, as of 3 January 2025; changes
made in the vendor inject script for Libreboot 20241206 rev8 or higher (and
releases newer than the 20241206 series) make the script almost completely
safe to use, as described in the section pertaining to this on
the [main guide](../docs/install/ivy_has_common.md).

To summarise, those new safety features are as follows:

* Newer release ROMs (20241206rev8 or higher, or releases newer than 20241206
  series) have 1-byte padding on non-inject images, to trigger an error in
  flashprog due to a size mismatch versus chip size, and they have the
  words `DO_NOT_FLASH` in the image file names. (older release images don't
  have this, so watch out)
* Injects directly into the *tarballs*, and replaces the given tarball with
  one containing the injected images. Older versions left the tarball
  untouched and outputted images to `bin/release/` (directory), whereas many
  users wrongly believed they could use the tarball; the new version therefore
  adheres accordingly to the user's natural expectation, in this regard.
* Avoids replacing the tarball, where errors are observed, and prints much more
  pedantic error messages, to let the user know that they must *stop* and take
  note.

With this in mind, the original article written below is largely obsolete, and
the steps below (which would require extreme over-engineering) are probably not
going to be implemented.

Now, continue reading the article below, if you wish!

NOTE: The sections in the article below are *still* important to read, so please
do read it, and note that **you must
still [insert vendor files](../docs/install/ivy_has_common.md) regardless,
prior to Libreboot installation, if required on your board.**

Article
=======

**Please also follow this guide if using Dell Latitude laptops.**

**If unsure, just follow this guide. If you follow this guide on a board that
does not need vendor files, the resulting ROM images will be identical and
therefore nothing will have happened. The inject script is designed to insert
certain files, only if required on a given mainboard.**

**NOTE: Libreboot standardises on [flashprog](https://flashprog.org/wiki/Flashprog)
now, as of 27 January 2024, which is a fork of flashrom.
The reason why was explained, in
the [Libreboot 20240225 release](libreboot20240225.md#flashprog-now-used-instead-of-flashrom)**

Introduction
============

**UPDATE (21 August 2023): None of the proposals below have yet been
implemented, and this page is still relevant for Libreboot 20231021. It applies
to any system that requires vendor code to be inserted inside ROM images.**

(it also applies to Libreboot 20231101, 20231106, 20240126, 20240225, 20240504,
20240612, 20241008 and 20241206)

**UPDATE (16 August 2023): This also applies to the recently added Dell
Precision T1650 mainboard.**

As I write this post, [Libreboot 20230625](libreboot20230625.md) recently came
out. There's technically nothing unsafe about the release itself, but certain
users have been bricking their machines, on the following mainboards:

* Sandybridge platforms (e.g. ThinkPad X220, T420)
* Ivybridge platforms (e.g. ThinkPad X230, T430)
* Haswell platforms (e.g. ThinkPad T440p, W541, OptiPlex 9020)

Why?
----

On these platforms, the following binary vendor files are required:

* Intel ME firmware: all Sandy/Ivy/Haswell boards. Libreboot's build system
  runs `me_cleaner` to neuter the Intel ME, so that it's disabled after BringUp.
* Intel MRC firmware: broadwell (HP EliteBook 820 G2)
* KBC1126 EC firmware: HP laptops (all sandy/ivy/haswell)

When you [build Libreboot from source](../docs/build/), Libreboot's automated
build system (lbmk) automatically downloads these files directly from the
hardware vendor, and inserts them into the ROM during build time.

However, these files are not redistributable, so Libreboot's build system (lbmk)
automatically scrubs (deletes) these files, from each ROM image, prior to
archiving the ROM images for release.

What this means is exactly as implied:

If you simply flash the release ROMs as-is, *without* modification, you will
be flashing them *without* these required files. This is exactly what some
people have been doing.

Instructions are given here, for how to insert these files on release ROMs:

[Insert vendor files on Sandybridge/Ivybridge/Haswell](../docs/install/ivy_has_common.md)

The linked guide makes use of vendor scripts, that
handles *all* firmwares, automatically for each given mainboard. It can
automatically download and insert all of the following:

* Intel ME firmware
* Intel MRC firmware
* KBC1126 EC firmware
* VGA ROM for Nvidia GPU, on Nvidia variant of Dell Latitude E6400 (which is
  still, as of this post, not in lbmk's master branch, but available in a
  different branch of lbmk, though the logic for downloading the VGA ROM and
  inserting it *is* included in lbmk master)

More information is available in the guide.

What can be done to reduce the risk?
------------------------------------

Like I said, there's technically nothing wrong with recent Libreboot releases.

The main problem is that Libreboot *documentation* did not prominently warn
about this issue. Such warnings *were* available on Libreboot, but were not
prominently displayed. Such warnings are now littered all throughout the
Libreboot documentation, even mentioned in bold lettering at the top of the
downloads page, so there's no way a user can miss it.

Other mitigations considered
-----------------------------

See: <https://codeberg.org/libreboot/lbmk/issues/92>

In this issue page, I outline ways to further reduce the risk. On the platforms
affected by this, the flash is divided into the following regions:

* IFD region
* GbE region
* ME region
* BIOS region

The IFD region configures the machine, and specifies read/write capability for
host CPU when flashing all regions, including IFD.

GbE contains NIC configuration, including MAC address, for intel gigabit NIC.

ME region is Intel ME firmware.

BIOS region is coreboot.

Per the issue page, I intend to implement the following regime in future
Libreboot releases, on the affected machines:

* BIOS region doesn't need vendor files (no MRC/EC firmware needed): set IFD,
  GbE and BIOS regions read-write by default, but lock the ME region.
* If BIOS region requires vendor files: set IFD and GbE regions read-write
  by default, but lock the ME and BIOS regions.

In this configuration, internal flashing would still be possible, so that you
do not have to disassemble the machine, but *two* flashes would be needed:

* Firstly, re-flash IFD that unlocks ME/BIOS regions
* Then ensure that the ROMs are properly prepared, and re-flash the entire
  ROM with IFD once again re-flashed to set ME and/or BIOS region read-only.

Under this configuration, we would still have the reality where some people
don't read documentation, but if they don't read documentation, they will
then just run flashprog on ROM images as-is, and it won't work. This will cause
one of three possible scenarios:

* They don't bother updating, and therefore avoid bricking their machine
* They complain on IRC/reddit, and we point them to instructions for how to
  deal with it - then they update their machine, and likely don't brick it
  anymore.
* They read the documentation from the start.

Under this regime, some users may still brick their machines. For example,
they might read the instructions for how to unlock regions, and then still
flash a ROM image without running vendor scripts on it - there is nothing we
can really do to prevent this, short of simply locking *all* regions, including
the IFD region (if we did that, then users would need to externally re-flash
their machine when updating).

Libreboot's policy is to make updates as easy as possible, but these extra
precautions are required on the newer Intel platforms.

When this is implemented in Libreboot, this page will be updated, and info
about it will be added to the installation/update instructions. I'm also
considering whether to apply this change *retroactively* on older release ROMs,
for all of these releases: 20221214, 20230319, 20230413, 20230423 and 20230625.

That's all for now. Please take care when updating or installing Libreboot.
Libreboot is generally well-tested and with good release engineering, but you
must ALWAYS read the documentation. This is true of any software, but it is
*especially* true of Libreboot. Please take care not to brick your machine.
Thanks!
