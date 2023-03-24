% 20230319 W541/T440p ROMs using mrc.bin deleted
% Leah Rowe
% 23 March 2023 (UPDATED on 24 March 2023)

This was reported last night, but the issue it reports has been fixed as
of this morning, on 24 March 2023. News article ammended, and date updated
to the 24th instead of 23rd.

Introduction
============

From the rsync mirrors for Libreboot 20230319, the following images have
been *deleted* retroactively:

* `t440pmrc_12mb`
* `w541mrc_12mb`

Those images listed above use the proprietary `mrc.bin` blob for raminit and
peripherals.

Libreboot *also* has these images:

* `t440p_12mb`
* `w541_12mb`

These ones, the ones without `mrc` in the name, use experiment source code from
Angel Pons of coreboot (`hell` on IRC), removing the need for `mrc.bin`
entirely. *These* ROM images are still available, in the rsync mirrors, for the
Libreboot 20230319 release.

Why were the MRC-based images deleted?
--------------------------------------

The ROM images themselves were *correct*, but there is a bug in the `blobutil`
insert script, for inserting `mrc.bin` into these images. The `mrc.bin` file
must be inserted at a specific offset, and no offset was declared explicitly
by `blobutil`.

In the Libreboot 20230319 release source archive, the `blobutil` program runs
this command for inserting `mrc.bin` when necessary:

	./coreboot/default/util/cbfstool/cbfstool ${rom} add -f mrc/haswell/mrc.bin -n mrc.bin -t mrc

This is **wrong**, because it's missing the following arguments at the the end:

	-b 0x78fe00

This, the correct command for those images would be:

	./coreboot/default/util/cbfstool/cbfstool ${rom} add -f mrc/haswell/mrc.bin -n mrc.bin -t mrc -b 0x78fe00

This command is *unnecessary* when building the ROM images yourself, from
lbmk directly, because the same download script is used and then coreboot's
build system inserts it, at the correct offset.

However, Libreboot's build system scrubs `mrc.bin` when creating release
archives, which means they must be re-added by the user when downloading
such pre-compiled ROMs, using a script available in Libreboot; it is *this*
script that was broken, in the Libreboot 20230319 release.

The position of MRC is to be fixed, and it is hardcoded in coreboot master.
Based on that, the MRC file is always added at the same location in CBFS, and
the number above is based on that. The number is correct, on Haswell configs
supported by Libreboot.

FIX AVAILABLE
=============

This bug described on this page has been fixed, in `lbmk.git`. See patch:

<https://browse.libreboot.org/lbmk.git/commit/?id=da6bf57a3f57f2625a4903cafb5cfd10ea4a1dae>

Will not re-add images to rsync
-------------------------------

I **could** re-add the images, and add a note to the Libreboot documentation
telling the user to patch their `blobutil` according to the commit linked
above. However, such would be un-clean. As far as I'm concerned, the images
are gone now and it's a testing release anyway.

So there's little risk. If you want images using `mrc.bin`, you shall build
Libreboot from source using these instructions:

[How to build from lbmk.git](../docs/build/)

All it costs you is a bit of extra time waiting for the build process to
complete. Those wily enough to flash a T440p themselves for the first time,
will surely not mind.

That is all.

PS:

If any persons bricked their T440p due to this bug, I'm very sorry. This is
precisely *why* I'm not re-adding the images, even though they're OK - a lot
of people as I said, won't patch their blobutil even when told to, to correct
that offset, but they can't brick their machine with such ROM images if they
don't exist. So they no longer exist, on mirrors providing Libreboot releases.

The irony would be if Angel finished the libre MRC code before the next
Libreboot release, making this fix pointless (because in that situation, I
would simply exclude images using `mrc.bin` at that point).
