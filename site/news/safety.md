% Safety issues updating Libreboot on Sandybridge/Ivybridge/Haswell
% Leah Rowe
% 7 July 2023

Introduction
============

**UPDATE (16 August 2023): This also applies to the recently added Dell
Precision T1650 mainboard.**

As I write this post, [Libreboot 20230625](libreboot20230625.md) recently came
out. There's technically nothing unsafe about the release itself, but certain
users have been bricking their machines, on the following mainboards:

* Sandybridge platforms (e.g. ThinkPad X220, T420)
* Ivybridge platforms (e.g. ThinkPad X230, T430)
* Haswell platforms (e.g. ThinkPad T440p, W541)

Why?
----

On these platforms, the following binary blobs are required:

* Intel ME firmware: all Sandy/Ivy/Haswell boards. Libreboot's build system
  runs `me_cleaner` to neuter the Intel ME, so that it's disabled after BringUp.
* Intel MRC firmware: Haswell platforms (W541, T440p) - a libre MRC replacement
  is available, but experimental, and the blob version is still recommended.
* KBC1126 EC firmware: HP laptops (all sandy/ivy/haswell)

When you [build Libreboot from source](../docs/build/), Libreboot's automated
build system (lbmk) automatically downloads these blobs directly from the
hardware vendor, and inserts them into the ROM during build time.

However, these blobs are not redistributable, so Libreboot's build system (lbmk)
automatically scrubs (deletes) these blobs, from each ROM image, prior to
archiving the ROM images for release.

What this means is exactly as implied:

If you simply flash the release ROMs as-is, *without* modification, you will
be flashing them *without* these required blobs. This is exactly what some
people have been doing.

Instructions are given here, for how to insert these blobs on release ROMs:

[Insert binary blobs on Sandybridge/Ivybridge/Haswell](../docs/install/ivy_has_common.md)

The linked guide makes use of `blobutil`, lbmk's single centralised utility that
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

* If BIOS region blob-free (no MRC/EC firmware needed): set IFD, GbE and BIOS
  regions read-write by default, but lock the ME region.
* If BIOS region requires blobs inserted: set IFD and GbE regions read-write
  by default, but lock the ME and BIOS regions.

In this configuration, internal flashing would still be possible, so that you
do not have to disassemble the machine, but *two* flashes would be needed:

* Firstly, re-flash IFD that unlocks ME/BIOS regions
* Then ensure that the ROMs are properly prepared, and re-flash the entire
  ROM with IFD once again re-flashed to set ME and/or BIOS region read-only.

Under this configuration, we would still have the reality where some people
don't read documentation, but if they don't read documentation, they will
then just run flashrom on ROM images as-is, and it won't work. This will cause
one of three possible scenarios:

* They don't bother updating, and therefore avoid bricking their machine
* They complain on IRC/reddit, and we point them to instructions for how to
  deal with it - then they update their machine, and likely don't brick it
  anymore.
* They read the documentation from the start.

Under this regime, some users may still brick their machines. For example,
they might read the instructions for how to unlock regions, and then still
flash a ROM image without running `blobutil` on it - there is nothing we
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
