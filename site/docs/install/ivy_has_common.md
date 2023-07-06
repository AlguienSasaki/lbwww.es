---
title: Insert binary blobs on Sandybridge/Ivybridge/Haswell
x-toc-enable: true
...

For how to use an external programmer see the [25xx NOR flashing guide](/docs/install/spi.html)

The Intel Flash Descriptor defines that the first 5MiB of the 12MiB boot flash consists of
the Intel Flash Descriptor, GbE and Intel ME regions. The final 7MiB of that
12MiB flash is the BIOS region. However, this 12MiB of flash is physically split
into an 8MiB NOR flash and a 4MiB NOR flash; the OS sees a continuous 12MiB of
flash, with the lower part being the contents of 8MiB NOR flash and the upper
contents being the 4MiB NOR flash.

Do not worry too much about which flash chip your programmer is connected to.
Flashrom will fail if you try to flash the wrong sized image for the chip you are connected
to.

The libreboot roms released or built for haswell or ivybridge boards come as 12/16MiB roms.
The size of the rom in question refers to the total size of *both* chips.
In order to flash a full rom externally, you need to split the rom into two sections to fit the size of the two chips you wish to flash.
This guide will show examples for the Thinkpad X230, but all of the information will apply to other boards.

Ivybridge boards require *at least* the intel management engine in order to boot.
Haswell boards additionally require the mrc blob.
Neither of these blobs are redistributable, so roms for these boards must be built from source or patched with the required blobs.

If you're planning to flash a release rom to your board then you need only patch the existing rom.
Alternatively, you can attempt to build a rom from source for your board.

Internal flashing
-----------------

For ivybridge specifically (e.g. thinkpad X230, T430) on Lenovo ThinkPads,
it is possible to flash from vendor firmware to Libreboot, without using a
clip, but some disassembly is still required. This can be beneficial if you
want to save money by not buying external flashing equipment. All you need is
a pain of metal tweezers or something similar that can be used to create
a short circuit between two conductors.

See: [ivybridge internal flashing](ivy_internal.md)

Obtaining Binary Blobs
----------------------

If you have built your rom from source then all of the blobs are generally downloaded automatically.
Some boards however, do not have sources for all blobs and require manual blob extraction.
If you try to build a rom from source and lbmk fails to locate the blobs, you can extract them from an existing rom backup.
To do this, start by obtaining a full backup rom from your machine.

Once you have connected your programmer and read from both flash chips, you will have to combine the two images to a single rom.
In general, the 4mb image is the top, and the 8mb image is the bottom.
To create a readable rom file, simply concatenate the two files.

	cat bottom.rom top.rom > full_backup.bin

Once you have a backup of your vendor rom, you can use lbmk to automatically extract the necessary blobs.
The blob extraction script takes a board name as the first argument and a path to a rom as the second argument.
For example, here is how you would extract the blobs from an x230 rom backup.

	./blobutil extract x230_12mb full_backup.bin

Note that the above command must be run from the root of the lbmk directory.
See [building instructions](/docs/build/index.html) for more details. 

Injecting Blobs into an Existing Rom
------------------------------------

Release roms cannot include certain blobs for legal reasons.
You therefore **cannot** directly flash a release rom to your board.
You must patch the release rom with the necessary blobs *and then* flash it to your board.

Lbmk includes a script that will automatically inject the necessary blobs into a rom file.
The script can determine the board automatically if you have not changed the name, but you can also manually set the board name with the `-b` flag.

You must determine the correct board name, for your board, based on the list
generated when running this command:

	./build boot roms list

For example, `t440pmrc_12mb` corresponds to ThinkPad T440p with MRC firmware.
Whereas `t440plibremrc_12mb` corresponds to T440p with libre MRC firmware.
Another example: `x230_12mb` corresponds to Thinkpad X230.

In order to inject the necessary blobs into a rom image, run the script from the root of lbmk and point to the rom image.

If you only wish to flash a release rom then the process of injecting the necessary blobs is quite simple.
Run the injection script pointing to the release archive you downloaded:

	./blobutil inject /path/to/libreboot-20230319-18-g9f76c92_t440pmrc_12mb.tar.xz

The script can automatically detect the board as long as you do not change the file name.
You can then find flash-ready ROMs in `/bin/release/`

Alternatively, you may patch only a single rom file.
For example:

	./blobutil inject -r x230_libreboot.rom -b x230_12mb

Optionally, you can use this script to modify the mac address of the rom with the `-m` flag.
For example:

	./blobutil inject -r x230_libreboot.rom -b x230_12mb -m 00:f6:f0:40:71:fd

**NOTE: Haswell machines come with `mrc.bin` or without, depending on the
ROM image configuration. These ROM configs have `mrc.bin`: `t440pmrc_12mb`
and `w541mrc_12mb`. These ROM configs have libre MRC: `t440p_12mb`
and `w541_12mb` - it is critical that you choose the right one, when using
the `-b` flag in the `blobutil inject` command. For example, if you
used `-b t440p_12mb` on a ROM image that actually corresponds
to `t440pmrc_12mb`, then the required `mrc.bin` file would not be added
and that ROM would not boot when flashed.**

**NOTE: In the Libreboot 20230319 src archive or git tag, the `blobutil`
insert method is broken on Haswell configs that need `mrc.bin`, because it does
not insert `mrc.bin` at the correct offset. This was fixed in revisions after
the release, and will be available in the next release after that. Read
the [Libreboot 20230319 update
announcement](../../news/libreboot20230319_update.md) for more information.**

NOTE: the MAC changer makes use of `nvmutil`, which you can read more about in
the [nvmutil documentation](nvmutil.md).

**WARNING: This is broken in Libreboot 20221214's src archive. It fails when
attempting to use cbfstool, due to a faulty check in a script. This is fixed in
recent Libreboot releases or revisions. The fix is as
follows:

Edit line 137 in `resources/scripts/blobs/inject`. The line in 20221214 says
this:

	make -C cd coreboot/default/util/cbfstool || Fail 'could not build ifdtool'

Modify it to say this:

	make -C coreboot/default/util/cbfstool || Fail 'could not build cbfstool'

ALSO:

*When generating a MAC address*, the same script tries to build `nvmutil`
from `/util/nvmutil`, in Libreboot 20221214. This was discovered on 10 January
2023, based on user report on IRC. Fix it like so (already fixed, in latest
Libreboot from Git):

Line 30, it says:

	make -C /util/nvmutil || Fail 'failed to build nvmutil'

Change it to say:

	make -C util/nvmutil || Fail 'failed to build nvmutil'

Until this is edited accordingly, the inject script will *exit* with non-zero
status, and no blobs will be injected.

This has been fixed, following the Libreboot 20221214 release, but you must
apply this fix yourself, if using *that* release.

Splitting The Rom
-----------------

You can use `dd` to easily split your rom into the two separate portions for
external flashing.
For example, here is how you would split a 12mb rom for installation: 

	dd if=libreboot.rom of=top.rom bs=1M skip=8
	dd if=libreboot.rom of=bottom.rom bs=1M count=8

You would then flash the 4MiB chip with `top.rom` and the 8MiB chip with `bottom.rom`.
For a larger rom image, the same logic would apply.

In dd `skip` means that you want the program to ignore the first n blocks, whereas
`count` means you want it to stop writing after n blocks.

Once you have your rom image split you can proceed to [flashing.](/docs/install/spi.html)
