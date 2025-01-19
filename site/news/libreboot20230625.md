% Libreboot 20230625 released!
% Leah Rowe
% 25 June 2023

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

Free software BIOS/UEFI
------------------------

Libreboot provides boot firmware for supported x86/ARM machines, starting a
bootloader that then loads your operating system. It replaces proprietary
BIOS/UEFI firmware on x86 machines, and provides an *improved* configuration
on [ARM-based chromebooks](../docs/install/chromebooks.html) supported
(U-Boot bootloader, instead of Google's depthcharge bootloader). On x86
machines, the GRUB and SeaBIOS coreboot
payloads are officially supported, provided in varying configurations per
machine. It provides an [automated build system](../docs/maintain/) for the
[configuration](../docs/build/) and [installation](../docs/install/) of coreboot
ROM images, making coreboot easier to use for non-technical people. You can find
the [list of supported hardware](../docs/install/) in Libreboot documentation.

Libreboot's main benefit is *higher boot speed*,
[better](../docs/linux/encryption.md) 
[security](../docs/linux/grub_hardening.md) and more
customisation options compared to most proprietary firmware. As a
[libre](policy.md) software project, the code can be audited, and coreboot does
regularly audit code. The other main benefit is [*freedom* to study, adapt and
share the code](https://writefreesoftware.org/), a freedom denied by most boot
firmware, but not Libreboot! Booting Linux/BSD is also [well](../docs/linux/) 
[supported](../docs/bsd/).

*This* new release, Libreboot 20230625, released today 25 June 2023, is
a new *stable* release of Libreboot. The previous stable release was
Libreboot 20220710, released on 10 July 2022.

### A note about the changelog

The changes listed here are relative to Libreboot *20230423*, *not* 20220710.
Therefore, to get a full list of changes since 20220710 (the previous stable
release, where releases in-between then and now were *testing*), you should read
the [20221214](libreboot20221214.md), [20230319](libreboot20230319.md),
[20230413](libreboot20230413.md) and [20230423](libreboot20230423.md)
announcements before reading the *20230625* change log, in order to get
a complete picture.

### Build from source

*This* release was build-tested on Debian *Sid*, as of 25 June 2023. Your
mileage may vary, with other distros. Refer to Libreboot documentation.

Work done since last release
----------------------------

### New mainboards now supported:

These boards were added to Libreboot:

* Laptop: HP EliteBook 2570p (courtesy Riku Viitanen and Johan Ehnberg
  `johan@molnix.com`)
* Desktop: HP 8300 USDT (courtesy Riku Viitanen, who *also* ported this
  to coreboot)
* Desktop: Gigabyte GA-G41M-ES2L was *re-added*, having been previously
  deleted from Libreboot.

The focus has been on stability, auditing the build system and fixing bugs,
thus hardware support is not greatly improved since the last release. Moving
forward, the next Libreboot release will be a *testing* release and *will*
focus on hardware support in addition to payloads (linux boot, UEFI etc).

### No-microcode ROMs available

Since Libreboot 20221214 and up, CPU microcode is
included by default on all applicable x86 mainboards. However,
Libreboot provided (and still provides) documentation for how
to manually remove it.

Such removal is still possible, but *this* release now provides
two sets of ROM images for each x86 mainboard: one set of ROMs
will *contain* CPU microcode, and another set *excludes* them.
For more context, see [Binary Blob Reduction Policy](policy.md)
and [Freedom Status](../freedom-status.md).

An earlier article was written about this, [here](microcode.md)

### Massive build system audit

The full change log below will list all build system changes,
but may not provide the overall picture, so to be clear: the
Libreboot build system has undergone an intense audit since the
last release. Over half of it has been re-written, or heavily
re-factored. The logic is much cleaner now (better coding style,
based on the OpenBSD coding style) and many bugs are fixed.

An earlier article was written about this, [here](audit.md)

### Full list of build system changes

Certain build system changes will not be listed here in detail, if they
pertain to general code style cleanup/re-factoring, as already alluded to
in the section above. However, specific *bug fixes* will be listed here,
some of which are *also* referenced in the article linked above.

Newest changes first, oldest changes last:

* Coreboot utilities: build them and place them under `cbutils/` from the main
  lbmk directory. Use these binaries during build (ROM images, for example).
  this makes handling of coreboot more robust, where we can now
  run `make distclean` without worrying about utilities. This also fixes the
  bug where utilities were being needlessly re-compiled, slowing down the build
  of multiple ROM images.
* Boot logo size reduced to ~3KB, down from ~12KB. Courtesy Riku Viitanen. This
  was done, without loss of image quality.
* `cros`: Disable coreboot-related BL31 features. This fixes poweroff on gru
  chromebooks. Patch courtesy of Alper Nebi Yasak.
* `u-boot`: Increase EFI variable buffer size. This fixes an error where
  Debian's signed shim allocates too many EFI variables to fit in the space
  provided, breaking the boot process in Debian. Patch courtesy Alper Nebi Yasak
* Re-added Gigabyte GA-G41M-ES2L mainboard. It actually does boot, but we have
  always known that raminit is quite experimental on this board. You just have
  to cycle through RAM until you find modules that work.
* `nyan` chromebooks: Removed for now (not yet stable) - this release only
  contains `gru` chromebooks, where ARM-based chromebooks are concerned.
* `blobutil/download`: Exit with zero status (success) immediately, when
  no board configs exist for a specified target. This fixes
  the `./build boot roms all` command, when the relevant script encounters
  certain targets defined under `resources/coreboot`, where previously the
  command would fail due to implicit references to such files.
* Main `lbmk` script: Exit non-zero if a called script fails, explicitly. The
  script previously relied on `-e` which isn't always completely reliable.
  The new handling also explicitly prints `lbmk error` on stdout, to let the
  operator know that an error did in fact occur.
* Board target `t440p_12mb` renamed to `t440plibremrc_12mb`
* `build/boot/roms`: No-microcode ROM images provided, alongside the default
  ROMs that include microcode by default. This change is also alluded to in
  the sections above.
* `blobutil/download`: Cache downloads based on checksum, to avoid needlessly
  re-downloading files per mainboard. This is useful when compiling multiple
  ROM images, for multiple mainboards. Patch courtesy Riku Viitanen.
* Dell Latitude E6400 (board): Fix SD cards (SD cards now work fine). Patch
  courtesy Nicholas Chin.
* `util/nvmutil`: Vastly more efficient code (part of the audit, referenced in
  above).
* `util/nvmutil`: [unveil](https://man.openbsd.org/unveil.2) now used, in
  addition to pledge when compiled on OpenBSD.
* `util/nvmutil`: Hardened pledge calls, when compiled on OpenBSD.
* utils: Properly detect OpenBSD, to test whether pledge should be used.
* board `hp8200sff`: Add 4MB ROM configs, for internal flashing, patch courtesy
  of Riku Viitanen.
* Unified *all* scripts in the main directory of lbmk (build system). All core
  logic is now a *single* script, removing needless code repetition.
* `download/coreboot` fix: Do not needlessly re-download a given coreboot
  tree, if it was already downloaded.
* Removed help text in certain scripts (unnecessary, can just read docs)
* `build/release/roms`: Fix error handling inside subshells.
* `build/release/src`: Fix error handling, previously not handled inside
  subshells.
* `build/ich9utils`: Fix error handling (subshells)
* `build/descriptors`: Fixed error handling (certain errors weren't handled)
* `build/grub`: Actually handle errors. Logic was handled in subshells, where
  greater care is required for error handling. Exit with non-zero status upon
  any error.
* `gitclone`: Only delete the old project directory *if* the new download has
  succeeded, to avoid, for example, complete purging of files when your internet
  is down. In such cases, it's better that the script simply fails and no action
  is taken. This fix implements such caution.
* `blobutil` fix: Actually exit with non-zero status (fail) when a called script
  fails. This previously wasn't done at all.
* `.gitcheck` (script): Another bug fix, actually *do* clean coreboot git
  credentials, when temporary ones were set, per coreboot tree. This previously
  was not handled at all. (this script removes the need to manually set git
  name/email, by setting a temporary one which makes the coreboot build system
  happy when building ROM images)
* `.gitcheck` (script): Actually *run* gitcheck-clean. It wasn't being executed
  at all, which screwed up the user's git credentials in some cases (after
  running lbmk, you would sometimes commit a patch and find that your name is
  now *lbmkplaceholder*).
* `util/spkmodem_recv`: Also: DEBUG define no longer used. It is now an argument
  namely `-d`, which can be passed to the program. The code for debug is now
  present in any build. Usage: `spkmodem-recv -d`
* New util: `util/spkmodem_recv` - imported from coreboot, which in turn forked
  it originally from GRUB. This is a receiving client for *spkmodem*, a type
  of serial console provided via pulses over PC speaker. Libreboot's version
  greatly improves the error handling, and it has been re-factored for OpenBSD
  coding style, to replace the (very horrible, very messy) GNU coding style.
  It is also [pledged](https://man.openbsd.org/pledge.2) in
  Libreboot's version. For reference,
  [here](https://git.savannah.gnu.org/cgit/grub.git/plain/util/spkmodem-recv.c?id=822b726b33b8dc07dd01b257a2dfcc7b07d12e2f)
  is the GRUB version, and
  [here](https://raw.githubusercontent.com/coreboot/coreboot/8febc91b3041a1d027bf0d36d30ccb119496524f/util/spkmodem_recv/spkmodem-recv.c) is coreboot's version of it. And now to blow your mind,
  [here](https://browse.libreboot.org/lbmk.git/tree/util/spkmodem_recv/spkmodem-recv.c?id=b508245451b71b3443fa3202f3863a6de731e9c8)
  is the Libreboot version present in release 20230625. A very much GNU program,
  but you wouldn't know it by looking! This fork was mostly done for fun, very
  much a *because we can* affair. BONUS: BSD-style error handling (`err.h`)
* download/coreboot: Run `extra.sh` directly from the directory for the given
  coreboot tree, if the script exists. No board in Libreboot has ever included
  an `extra.sh` script, but it can be used to extend the default functionality
  of lbmk where lots of custom logic is needed for a given board. For example,
  if you needed to apply lots of patches from gerrit in a specific order, in a
  way that could not be generalised for other mainboards where it's really
  only applicable to that mainboard, you might use `extra.sh`. This was briefly
  used in the erstwhile osboot project, on a few boards.
* download/coreboot: Use the `gitclone` script for actually cloning coreboot,
  while still using the main script for handling trees/revisions and such.
  (this is one of the scripts that got re-written during the audit, mentioned
  in the previous section above)
* build/payload/seabios: *Much* stricter error handling, exit with non-zero
  status (fail) on more error conditions, should they occur.
* download/mrc (script): *Much* stricter error handling, exit with non-zero
  status (fail) on more error conditions, should they occur.
* gitclone (script): Check whether a given patch exists before applying. Works
  around a quirk in most shells where `*` will error out if no files exist.
* download/grub (script): If downloading `gnulib` fails, scrap the *grub*
  download aswell, and exit with non-zero status (fail). This is done because
  gnulib is a dependency of GRUB.
* blobutil/inject (script): When inserting `gbe.bin`, check that the file
  actually exists, and exit with non-zero status (fail) otherwise.
* blobutil/inject (script): When inserting `me.bin`, check that the given
  path is defined and exit with non-zero status (fail) otherwise.
* blobutil/inject (script): Use x86 top-aligned MRC offset, when
  inserting `mrc.bin` on Haswell ROM images (ThinkPad T440p/W541). Previously,
  lbmk would insert them based on the offset as per ROM image file size, but
  it is possible (and more reliable) to have cbfstool calculate that position
  within the file based on where the ROM image would be loaded into memory.
  The result is identical in practise (same checksum for the file), but this is
  more robust for a future scenario where, for example, Libreboot might provide
  different sized images. For example, if the flash were upgraded to 16MB rather
  than 12MB on a T440p (Libreboot still only provides 12MB images for T440p).
* Coreboot build system: patch it to no longer warn about lack of payload
  when building (patch courtesy of Nicholas Chin). Libreboot's build system is
  designed to build ROM images without a payload, and then compile/add payloads
  itself, as opposed to using the *coreboot* build system for payloads.
* Don't patch the build system for python2 anymore. Prefer python3, and use
  it exclusively. No more python2 code was needed anymore, in lbmk's
  dependencies.
* build/boot/roms (script): Fix faulty variable expansion for list of keymaps.
  This fixes keymap handling when generating GRUB payloads for each keymap.
* The entire build system: massive code cleanup as alluded to above.
* Coreboot: Patched the coreboot device tree on Dell Latitude E6400, so that
  the Nvidia GPU can be initialised properly (patch courtesy Nicholas Chin) -
  only Intel GPU variants of E6400 are officially supported in this release.
* SeaBIOS payload: Provide `normal` config for setups where no VGA ROM init
  from CBFS is provided, either in SeaBIOS or GRUB(via coreboot). This would
  be used, for example, on desktops that lack onboard graphics, where it is
  expected that a graphics card would be used.
* Fixed the Makefile to match current commands available in lbmk (use of
  the Makefile is still optional, and direct use of lbmk is still recommended)
* Patched util `bios_extract` to work nicely with Dell E6400 update files,
  patch courtesy of Nicholas Chin.
* New util: `bios_extract` (used to VGA ROM from Dell updates, for Nvidia GPU
  model of Dell Latitude E6400, though this release only officially supports
  the Intel GPU variant)
* Fixed unifont package info in Fedora 38 dependencies script
* util/e6400-flash-unlock: Minor code cleanup
* Don't force console mode in GRUB (reverses experimental change made in
  the 20230423 release)

Hardware supported in this release
----------------------------------

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

### Desktops (AMD, Intel, x86)

-   [Gigabyte GA-G41M-ES2L motherboard](../docs/install/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/install/acer_g43t-am3.md)
-   Intel D510MO and D410PT motherboards
-   Apple iMac 5,2
-   [HP Elite 8200 SFF/MT](../docs/install/hp8200sff.md) (HP 6200 Pro Business probably works too)
-   [HP Elite 8300 USDT](../docs/install/hp8300usdt.md)

### Laptops (Intel, x86)

-   **[Dell Latitude E6400](../docs/install/latitude.md) (easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/install/x200.md)
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/install/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/install/t400.md)
-   [Lenovo ThinkPad T500](../docs/install/t500.md)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad W500](../docs/install/t500.md)
-   Lenovo ThinkPad R500
-   [Apple MacBook1,1 and MacBook2,1](../docs/install/macbook21.md)
-   [Lenovo ThinkPad T440p](../docs/install/t440p_external.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   [Lenovo ThinkPad W541](../docs/install/ivy_has_common.md)
-   [HP EliteBook 2560p](../docs/install/hp2560p.md)
-   [HP EliteBook 2570p](../docs/install/hp2570p.md)
-   [HP EliteBook Folio 9470m](../docs/install/hp9470m.md)

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Downloads
---------

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.

Post-release errata
-------------------

When building ROM images from the release archives, the following error
is observed in some cases, depending on distro:

```
In file included from src/lib/version.c:4:
build/build.h:10:32: error: 'libreboot' undeclared here (not in a function)
   10 | #define COREBOOT_MAJOR_VERSION libreboot-20230625
      |                                ^~~~~~~~~
src/lib/version.c:35:46: note: in expansion of macro 'COREBOOT_MAJOR_VERSION'
   35 | const unsigned int coreboot_major_revision = COREBOOT_MAJOR_VERSION;
      |                                              ^~~~~~~~~~~~~~~~~~~~~~
```

This happened when a user tried to build for ThinkPad W541 on an Arch Linux
system. The fix is available here:

<https://browse.libreboot.org/lbmk.git/patch/?id=f34e07ae27e3e6e8508cdebcbd09fdf73fca302d>

Apply this patch to your local release archive, and it should fix the issue.
