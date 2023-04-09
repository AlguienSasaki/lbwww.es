% Libreboot 20221214 released!
% Leah Rowe
% 14 December 2022

The last Libreboot release, version 20220710, was released on 10 July
in 2022. *This* new release, Libreboot 20221214, is released today on December
14th, 2022. This is intended to be a *testing* release.

This is marked as a *testing* release, but most/all boards should be fairly
stable.

Build from source
-----------------

*This* release was build-tested on Debian 11. Your mileage may vary, with
other distros.

Work done since last release
============================

For more detailed change logs, look at the Git repository. This is
a summary of changes.

New boards, x86
----------

This list *may not be complete*, but it is as follows:

* ASUS p2b\_ls/p3b\_f added, for testing with [PCBox](https://pcbox-emu.xyz) -
  these ROM images achieve perfect raminit and jump to payload in the emulator,
  but VGA ROM initialisation does not yet work.
* QEMU (arm64 and x86\_64) images added, for testing payloads and so on.
* lenovo/t430 (thinkpad)
* lenovo/x230 (thinkpad)
* lenovo/x230edp (e.g. nitrocaster FHD mod kit)
* lenovo/t440p (thinkpad)
* lenovo/w541 (thinkpad)
* lenovo/x220 (thinkpad) (also works with x220 tablet)
* lenovo/t420 (thinkpad)
* lenovo/x230 tablet (thinkpad)

GA-G41M-ES2L ROMs
------------

GA-G41M-ES2L ROM images are back, but your mileage may vary. Only the
SeaBIOS payload is enabled, on this board.

No lbmk changes were done for this, because the ROMs were simply excluded
in the previous release, but this board was not deleted from lbmk.

**UPDATE ON 20 December 2022: per many user reports, these ROMs work very
well. GA-G41M-ES2L support is therefore stable, for all intents and purposes.
This section of the release announcement previously alluding to issues, but
those speculations were premature, based on limited prior testing.**

New boards, ARM
----------

NOTE: These boards use u-boot payload, instead of depthcharge.

* Samsung Chromebook 2 13" (peach-pi)
* Samsung Chromebook 2 11" (peach-pit)
* HP Chromebook 11 G1 (daisy-spring)
* Samsung Chromebook XE303 (daisy-snow)
* HP Chromebook 14 G3 (nyan-blaze)
* Acer Chromebook 13 (CB5-311, C810) (nyan-big)
* ASUS Chromebit CS10 (veyron-mickey)
* ASUS Chromebook Flip C100PA (veyron-minnie)
* ASUS Chromebook C201PA (veyron-speedy)
* Hisense Chromebook C11 and more (veyron-jerry)
* ASUS Chromebook Flip C101 (gru-bob)
* Samsung Chromebook Plus (v1) (gru-kevin)

These ARM boards are all Chromebooks.

This work on the Chromebooks and u-boot integration is courtesy
of Alper Nebi Yasak (`alpernebbi` on libreboot IRC).

In these configurations, u-boot is a *payload of coreboot*.

Removed boards
--------------

ASUS KGPE-D16, KCMA-D8 and KFSN4-DRE are removed, because raminit
never worked reliably on these boards to begin with and they were
barely maintained anymore. Use an older release of Libreboot, if
you still want to use these boards.

lbmk Git changes
------------

These are takes from the git log of `lbmk.git`:

```
* c6bb4d25 (HEAD -> master, tag: 20221214, srht/master, origin/master, notabug/master) build/release/src: don't delete .gitcheck
* 0fbf3325 correct a faulty if statement in build/release/src
* ab2cfb86 util/nvmutil: only mask random unicast/local macs
* fea3e51c update the readme
* 664cdcfb fix ./build boot roms all
* 48c73186 p2b_ls/p3b_f boards: Disable memtest payload
* 31111c64 build/boot roms: add exits for failing commands
* 4eba525b p2b_ls/p3b_f boards: no payload and no vga init
*   c931b40e Merge branch 'master' of qeeg/lbmk into master
|\  
| * 6351a4a4 Add P2B-LS and P3B-F configs
* |   34a56281 Merge branch 'cros-postmerge-fixes' of alpernebbi/lbmk into master
|\ \  
| * | f079b83d build/release/src: Include U-Boot sources in source archive
| * | 70435784 build/clean: Add helper script to clean U-Boot builds
| * | 0bd4fdbe dependencies/debian: Install dependencies for U-Boot
| * | 3d5bd034 coreboot: Add qemu_arm64_12mb board
| * | d14731be u-boot: Add qemu_arm64_12mb board
| * | b5a5801f coreboot: qemu_x86_12mb: Enable DRIVERS_UART_8250IO
| * | 737573ce u-boot: Add qemu_x86_12mb build
| * | 1c62b003 build/roms: Support using "u-boot" ELF file as U-Boot payload
| * | 6cabcec5 u-boot: Add video damage tracking patch series
| * | 38328b93 u-boot: Set default revision to v2022.10
| * | c798975d u-boot: Use a common tree
| * | 5b6bf2a8 build/roms: Don't rebuild crossgcc if it was already built
| * | bee50540 build/roms: Make coreboot crossgcc usable for payloads and modules
| * | a5863561 build/roms: Build 32-bit crossgcc for AArch64 as well
| * | 9fb4ecec build/roms: Don't build Memtest86+ when not specified by cmdline
| * | 4e3097b5 build/roms: Disable U-Boot when not in payloads specified by cmdline
| * | 584210bd download/u-boot: Change to download target before running extra.sh
| * | 2b761f2f download/u-boot: Re-add usage text for no-argument form
| * | 71cf7f9d download/u-boot: Remove support for deleting git folders
| |/  
* | b495aa09 util/nvmutil: consistent parentheses on comparison
* | 17fa25e5 util/nvmutil file reads: skip reading if errno!=0
* | 27876c64 util/nvmutil: return error when fstat() is -1
|/  
* 960af2d6 util/nvmutil: rhex(): fail if errno not zero
* 3d01cf28 util/nvmutil: minor code formatting cleanup
* a7ea70c7 build/release/roms: delete ME/MRC firmware in ROMs
* 0c334380 build/boot/roms: remove errant code
* 33bbb36d remove errant detail from comment
* 55869474 delete build/release/u-boot-libre
* 137b5434 remove logic for avoiding nonredistributable blobs
* 7679c8e0 coreboot/default: add --nuke flag to ifdtool
* a5e4416a util/nvmutil: remove errant line break
* c100dd1f util/nvmutil: missing paretheses on if statement
* 036d7107 util/nvmutil: don't initialise rbuf unless needed
* 851892b4 util/nvmutil: rename variable in hextonum
* 0bf3f1ed util/nvmutil: don't reallocate memory in hextonum
* e5a46b46 util/nvmutil: dont report bad size if /dev/urandom
* ededa5dd util/nvmutil: rename variables in hextonum
* e2e321fc util/nvmutil: use BUFSIZ for rmac size in hextonum
* a6d0112d util/nvtutil: fix out of bounds error
* 04ced693 update the README
* 85937f3f util/nvmutil: reset errno on cmd_swap
* ec082429 scripts: avoid relying on spaces from sha1sum output
*   7c5334ca Merge branch 'hide-mei' of XRevan86/lbmk into master
|\  
| * 69eaca2c coreboot: hide MEI on neutered-ME targets
|/  
*   cf052220 Merge branch 'master' of Arsen/lbmk into master
|\  
| * a40ba4ad t430_12mb: Add, based on x230_12mb
* |   0c5dfddd Merge branch 'x230edp' of XRevan86/lbmk into master
|\ \  
| |/  
|/|   
| * a33e8429 coreboot: add x230edp_12mb, remove x230fhd_12mb
|/  
* e8eee6dd util/nvmutil: mild refactoring
* 342e5abe util/nvmutil: improved errno handling in main
* d7465efb util/nvmutil: put hextonum in its own function
* 9e5ff5e4 util/nvmutil: move ENOTDIR check to function
* ff88cb1a util/nvmutil: further improved errno handling
* b81b51f9 util/nvmutil: remove errant code
* a94bac81 util/nvmutil: improved error handling
* 55a951a7 util/nvmutil: fix off by one bug
* 0108615f nvmutil copy/swap: actually set nvmPartModified
* 82300f4f util/nvmutil: move cmd copy to own function
* ddf3b76c util/nvmutil: move cmd swap to own function
* c2ed251c util/nvmutil: move cmd brick to own function
* eaad16ed util/nvmutil: cmd setchecksum in own function
* cea1beea util/nvmutil: split "dump" into smaller functions
*   59e4f560 Merge branch 'dev' of shmalebx9/lbmk into master
|\  
| * 99652baa fix injection script
| * 175b48a4 added more checks and optimised extraction script
| * b2c71747 make gitcheck verify coreboot subdir
| * 1246c3ad add smort failures to blob download script
* | 0ae00e88 util/nvmutil: re-factor to reduce code indentation
* | 0bbd4f1f util/nvmutil: write gbe files in a function
* | b0f9f47e util/nvmutil: human-friendly exit messages, part 2
* |   e35a33d5 Merge branch 'qemu' of shmalebx9/lbmk into master
|\ \  
| * | da155b3d added x86 qemu board based on x230 coreboot config
* | | e1bbdadc build/roms: remove seabios_grubfirst logic
| |/  
|/|   
* | 7629dfb8 remove duplicate patch causing build error
|/  
* ca45a60f bump grub revision to latest upstream
* c1c76a05 dependencies/arch: notice about unifont dependency
* 43196abc also fix crossgcc on cros/fhd coreboot trees
* f0631908 cros devices: use a common coreboot tree
* 24a866ba remove kfsn4-dre, kcma-d8 and kgpe-d16
* f5b4eb3f update gitignore
* 60793c55 fix gnat build issue on coreboot repositories
* 6114c349 add innoextract to federa dependency script
* 5ec5d0ea ditto others
* 551e845e ditto debian script
* f896bb84 remove stupid flags from arch dependency script
* 5a01e98d build/dependencies/*: remove python2
* 6c12afa9 util/nvmutil: more human-friendly exit messages
* 50174563 fix part 1 checksum in t440p gbe.bin
* a7b8d0cf update .gitignore
* b3b3642f assimilate nvmutil
* 8740404e make background splash screen purple
* 3f12ef85 bonerfix
* cf945dda blobs/inject: use nvmutil, not nvmutils
* 2589d367 update the README
* 7af99534 (tag: psdg) pragmatic system distribution guideline compliance
*   b5c25efe Merge branch 'u-boot-chromebooks' of alpernebbi/lbmk into master
|\  
| * 61ac6c3f u-boot: Add peach pi chromebook configs
| * f848eb81 coreboot: Add peach pit chromebook configs
| * e08e3da2 u-boot: Add peach pit chromebook configs
| * 8584fcc1 coreboot: Add spring chromebook configs
| * f9f5d5fc u-boot: Add spring chromebook configs
| * 2dcb7cab coreboot: Add snow chromebook configs
| * be8bebaa u-boot: Add snow chromebook configs
| * c97f8e5c coreboot: Add nyan blaze chromebook configs
| * 330f985d u-boot: Add nyan blaze chromebook configs
| * ddc695a2 coreboot: Add nyan big chromebook configs
| * 0d696ee3 u-boot: Add nyan big chromebook configs
| * 2e0f13d9 coreboot: Add veyron mickey chromebit configs
| * 330c62ae u-boot: Add veyron mickey chromebit configs
| * f84209ce coreboot: Add veyron jerry chromebook configs
| * fc7794a1 u-boot: Add veyron jerry chromebook configs
| * bbba94ed coreboot: Add veyron minnie chromebook configs
| * bc47f8cc u-boot: Add veyron minnie chromebook configs
| * 2ed1111d coreboot: Add veyron speedy chromebook configs
| * fa553566 u-boot: Add veyron speedy chromebook configs
| * 0ae23980 coreboot: Add bob chromebook configs
| * ff39bba2 u-boot: Add bob chromebook configs
| * af46cbff coreboot: Add kevin chromebook configs
| * 38655635 u-boot: Add kevin chromebook configs
| * 6d6bd5ee build/roms: Rebuild cbutils module before starting coreboot build
| * 61ede998 build/roms: Support using U-Boot as a coreboot payload
| * a69855f7 build/roms: Build 32-bit crossgcc for AArch64 as well
| * 769f18f2 build/roms: Fix building for ARMv7 and AArch64 boards
| * 9bfbdb59 scripts: Add helpers to modify and update U-Boot configs
| * 1dc05e40 build/payload: Add helper script to build U-Boot as payload
| * cf295741 download: Use shallow clones for big projects
| * ef39e05b download: Allow keeping .git dirs with NODELETE=git
| * 764a439a u-boot-libre: Add support for deblobbing U-Boot v2022.07
| * 270272eb download/u-boot: Remove .git folders as well
| * 820b8e70 download/u-boot: Support running extra commands from board dirs
| * eae6b35d download/u-boot: Support applying patches from board dirs
| * 454364cc download/u-boot: Try to update submodules as in coreboot script
| * 0aeb69b5 download/u-boot: Use GitHub mirror as fallback
| * 7b552bd2 download/u-boot: Support reading tree and revision from board.cfg
| * 8dd1a245 download/u-boot: Prepare files per board instead of per revision
| * d8da9b51 .gitignore: Ignore u-boot directory
| * 22b1db69 u-boot-libre: Set tar mtime to SOURCE_DATE_EPOCH or @0
| * 01f61263 u-boot-libre: Fix releasing blob list as deblob script
| * 89a4c2c6 u-boot-libre: remove nonfree firmware in drivers/dma/MCD_tasks.c
| * f679fbd3 u-boot-libre: Fix reproducability issue due to timezone
```

osbmk Git changes
-------------

It's important to show the osboot changes aswell. Osboot only became part of
Libreboot last month, but the "reboot" of the osboot project happened around
the start of 2022, when it was put back in sync with Libreboot at the time,
so changes from then to now will be showed. The *last* change in osboot as
shown below is the revision that got merged with lbmk:

To be more clear: on 19 December 2021, osboot got nuked and re-forked from
scratch, relative to Libreboot, so the earliest revisions in the log below
are Libreboot from that time (forked from lbmk
revision `c3a66c32750fa4a9a90ddb6383b09fdfb6ff77f5`), so this is actually
a full list of all osboot-related changes that became part of Libreboot
in the osboot/libreboot merge last month:

(some of them are identical changes to lbmk, because efforts *were* made at
the time to keep osboot/libreboot in sync, before it was decided that they
would simply merge)

```
* c8c030b (HEAD -> master, origin/master, origin/HEAD) fork lbmk 61ac6c3f0b26deadc2fb8355a8dd0d25b29baacd
* eef6b71 clean up the download script
*   1d2f9e8 Merge branch 'finetune' of shmalebx9/osbmk into master
|\  
| * bd4f1b4 added fine-tuned control for building roms
* |   55e5d92 Merge branch 'gitframework' of shmalebx9/osbmk into master
|\ \  
| * | a6e5c87 added internal git package management
* | |   4b424a8 Merge branch 'master' of anjan/osbmk into master
|\ \ \  
| * | | 7a444dc debian dependencies: add python-is-python3
| |/ /  
* | | 7728a98 lenovo/w541: new board added
* | | d05ab33 re-do x220 configs
* | | ad68c8c add lenovo/t520
* | | 7979263 add lenovo/t420s
* | | 3e20a41 build/roms: remove unnecessary bloat
* | | a18e962 build/roms: fix missing cbfstool bug
* | | 74ef993 re-do x230 fhd configs
* | | b1b481d fix build issues with x230 fhd
* | | 3472940 actually add the x230 fhd patch
* | | 3106501 script/blobs: check to download *default* coreboot
* | | 81b4ff7 Re-add X230 FHD patches
| |/  
|/|   
* | 83c230b add t440p config with 4mb cbfs
|/  
* 31fbee4 make only the logo darker, in grub backgrounds
* 5e2da8f update build/release/src based on osbmk changes
* 00707ea say the name osboot, in the grub menu
* 56bb8a8 add bootsplash with new logo
*   675db3d Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * 2098cfa initialize git if it isn't already
| * d4690d0 updated gitignore for new dependencies and blobs
* | e43a28c Merge branch 'dev' of shmalebx9/osbmk into master
|\| 
| * 5139ad4 added myself as a license holder to changes in last commit
| * 327a39e added workaround for git credentials
|/  
* b079a19 patch me_cleaner to specifically use python3
* 5cc10a7 specifically call python3, in scripts
* 91b6542 fixed b0rked descriptor
* ca8d824 added myself as a license holder to various scripts
* aaeba81 removed obselete entries from blob sources
* 2a11133 hardcoded paths to redistributable blobs
* 4e2bd46 updated blobutil scripts to deal with hardcoded paths
* 4ca4801 Perform the silentoldconfig step of seabios before full make
* 1c921b5 new board: lenovo/x230t_16mb
* 335f95c add missing board.cfg for x230_16mb
* c4a705e set regions read-write on xx30/ifd.bin
*   cafb408 Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * 7c7d96e Download script can tell whether to pull 16mb ifd
| * 999331d added x230_16mb
* | c437778 x230/x220: don't set CONFIG_HAVE_EM100_SUPPORT=y
* | dfeb26c fix txtmode configs: me/ifd/gbe insertion not enabled
|/  
* 6390a90 nuke boards/x230_truncated_16mb for now
* 174f6c2 disable CONFIG_HAVE_EM100_SUPPORT on boards
* 8b698a4 new board: lenovo/x230t
* ac790ee update nvmutils
*   79e94d7 Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * 55d44bc added licenses just in case
|/  
*   3171b91 Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * cba24e8 fix txtmode config for t440p
| * 55d6503 changed build system for new blobutil
| * 1462a3c move all blobs scripts to one directory
* | 84a9d53 update flashrom
* | cda2d70 reset nvmutils to known good revision
* | 65b62c0 exit if can't download nvmutils
* | 12c9fd2 coreboot: set me_state=Disabled on all boards
* | 7ef3b88 Merge branch 'dev' of shmalebx9/osbmk into master
|\| 
| * d78c65e added t440p blobs
* | 8e7b3c3 Merge branch 'dev' of shmalebx9/osbmk into master
|\| 
| * bc91ff2 fixed breaking bug in blobs downloader
|/  
* 95ae701 enable CONFIG_PCIEXP_HOTPLUG on all boards that support it
*   75a9f00 Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * ae0b95a added t420
* | 8971e0d Merge branch 'dev' of shmalebx9/osbmk into master
|\| 
| * b3081fc better error handling
| * e59a546 updated blob injector to give option to change mac
| * 563d0de made blob downloader save blobs under board_short no matter what
|/  
*   5d5746f Merge branch 'dev' of shmalebx9/osbmk into master
|\  
| * 68533c6 removed hardcoded tmp files
| * 7fc071b added blob injector for binary releases
| * 3457579 added release infrastructure
|/  
*   71f6936 Merge branch 'master' of shmalebx9/osbmk into master
|\  
| * 938bd04 switch x230 config back to 12mb cbfs size
| * b69d4fa Added dependencies for automatic blob extraction
| * 0829f5b added x220 support
| * 1f4e6d7 added ifd and gbe for xx20 and xx30 boards
| * b933bff Added the scripts for automatically downloading blobs
|/  
* 489f2ee memtest86+: fix build error (patch from Félicien Pillot)
* 240779a lenovo/x230: fix build
*   f6cffa0 Merge branch 'master' of shmalebx9/osbmk into master
|\  
| * fa8e5fa switched back to the old way of downloading the mrc
| * 831d8f3 added t440p rom as an example of a rom needing the mrc
| * cf479fd added a simpler version of the old mrc download script. This one just uses the default coreboot way of extracting it using the included script, so it will always be up to date
| * 7fdfb07 added ability to detect if the board needs the mrc and download it
| * b8bd895 added mrc download script from old osbmk but changed to agnostic shebang
| * d80bcfb added x230 coreboot config as an example of a config using the blobs extracted with the extraction script
| * d40b01c add script to extract blobs from the vendor rom image for ivy bridge lenovo boards. Could possible work/be extended for other mainboards
| * c3dfcf4 re-add me_cleaner and change to agnostic shebang
* | f70c5cc lenovo/x230: set me_state=Disabled in cmos.default
* | 9b4afd1 x230_12mb: set cbfs back to 7mb. i will add special truncated configs instead
* | d39da96 rename x230_16 to x230_truncate_16mb
|/  
* baf4fd4 Revert "lenovo/x230: set me_state=Disable in cmos.default"
* 4828eb0 new cfg: lenovo/x230_16mb: 16MB-128KB CBFS size for truncated ME
* 1334dd5 lenovo/x230: set config 12MB-128KB cbfs size for truncated ME
* ecb98cc lenovo/x230: set me_state=Disable in cmos.default
* 3a5d399 1MB coreboot config: don't enable grub_withseabios
* 19d2cda optimize grub modules: pre-load ones that will likely be used
* d7e5a08 build/boot/roms: fix wrong variable name
* 81330c6 coreboot/*: set grub_scan_disk to ahci on most boards
* 99f58d8 apple/macbook21: set grub_scan_disk to ahci
* 17830a4 build/boot/roms: substitute grub_scan_disk according to board.cfg
* f222519 grub.cfg: skip ata/ahci according to grub_scan_disk
* defe338 grub.cfg: clean up messages, be less verbose
* 1fa0e53 grub.cfg: add isolinux menuentry for ata* (replace broken cd/dvd menuentry)
* e33645a grub.cfg: delete option to boot from CD/DVD
* b370cd1 grub.cfg: clean up comments
* 81f3755 grub.cfg: don't use */? wildcards. they slow down the boot
* 6a315b6 grub.cfg: optimize search_isolinux
* ccc7aed remove entry in .gitignore from the last commit
* f84d09b Fix grub's slow boot
* 6ffbcee rename README to README.md
* 47155b3 lenovo/x230: re-add support from coreboot
* 82f381b do full coreboot checkout. enable microcode updates. don't delete blobs
* 6afa56e rename project back to osboot and delete grub background
* 8614ee7 assimilate lbmk c3a66c32750fa4a9a90ddb6383b09fdfb6ff77f5
```

Known issues
============

Intel ME firmware missing in ROMs that need it
----------------------------------------------

If you compile ROM images with lbmk directly, the build system
automatically fetches ME images from the internet and neuters
plus truncated them, using `me_cleaner`. This downloading is done
to avoid distributing them directly in Libreboot, and they get
scrubbed by the release build scripts.

To re-insert neutered/truncated ME into your image, look at
the [guide](../docs/install/ivy_has_common.md).

This applies to sandybridge, ivybridge and haswell Intel platforms,
e.g. X220, T420, X230, T430, T440p. On *older* Intel platforms such
as GM45+ICH9M (X200, T400, etc) the Intel ME image isn't needed and
Libreboot ships with ME-disabled configuration.

MRC image missing in Haswell ROMs
---------------------------------

Ditto ME images. To re-insert, follow
the [guide](../docs/install/ivy_has_common.md).

Internal flashing on 16MB X230 images
-------------------------------------

The X230 has two ways of upgrading the default 12MB (8MB and 4MB chips)
flash to 16MB: remove a bunch of resistors and capacitors on the board
(a guide is yet to be written for this) and both flash ICs, and replace
SPI1 with a 16MB chip e.g. Winbond W25Q128FVSIG.

The other method: replace just SPI2 which is a 4MB flash, with an 8MB
flash e.g. Winbond W25Q64FVSIG. This latter method was tested, and it
is made to boot by changing component2density in the IFD to 8MB, but
this is non-standard per Intel specifications.

It boots, but then internal flashing still only lets you flash the first
12MB, even though the bootblock is at the end of that 16MB flash and
does boot perfectly!

So, 16MB images on X230 are experimental. When these ROM images are built,
the required IFD modification is already done in Libreboot's IFD for this
setup.

S3 suspend/resume on Haswell (T440p/W541)
-----------------------------------------

Totally broken. The suspected cause is component2density setting in IFD
being wrong: ifdtool reports it as being too big for what's actually
flashed.

No fix has been found yet that doesn't brick the machine, so this bug
is still present in the Libreboot 20221214 release.

The issue is that the MRC cache is in the wrong place in memory, because
of that bug in the IFD, but when ifdtool is used to correct component2density,
it bricks, which leads to wonder: is ifdtool's knowledge of these bits on
haswell faulty?

When X230 16MB was experimented with, Haswell was also looked at, and what
seemed to be the same component density bits were set, also resulting in
a brick (it's why only 12MB images are available for haswell in libreboot).

Chromebook boards mostly untested
---------------------------------

Alper has tested the `gru_kevin` ROM images produced by lbmk, but
the other images are untested as of this day.

Planned future work
===================

In general, you should also check the issue tracker to find other notes.
There is always more work to do, to improve Libreboot.

Linux distro in flash
---------------------

STILL ON TODO SINCE LAST RELEASE.

This is another project that has been on hold for a while. The issue
has been that I need a decent userland project. I've looked at many
different userlands and recently (as of late June) decided to make
my own. I want a BusyBox-like solution, but based on code from OpenBSD,
ported to run under Linux with musl libc.

I want this distro to provide a kexec bootloader in flash, similar to Heads,
and I also want it to use apk-tools, pointing at Alpine Linux repositories
so as to allow any number of packages to be downloaded. It could also provide
lots of utils in general, to be a live *rescue system* of sorts. Linux system
in flash, that can bootstrap other systems.

Re-factor and optimize GRUB
---------------------------

STILL ON TODO SINCE LAST RELEASE.

GRUB is full of unused bloat that almost nobody uses, yet is in the current
Libreboot builds. It's been on TODO for some time, but work has not yet
begun on this project. My efforts are currently focused on the Linux distro.

What I want is a fork of GRUB, optimized to run on bare metal as a coreboot
payload, on x86 and ARM platforms.
