% Libreboot 20231021 released!
% Leah Rowe
% 21 October 2023

**PLEASE READ THIS BEFORE INSTALLING:
[Safety advice when updating Libreboot on
Sandybridge/Ivybridge/Haswell](safety.md)**

Introduction
============

*This* new release, Libreboot 20231021, released today 21 October 2023, is
a new *testing* release of Libreboot. The previous release was
Libreboot 20230625, released on 25 June 2023.

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
the [list of supported hardware](../docs/hardware/) in Libreboot documentation.

Libreboot's main benefit is *higher boot speed*,
[better](../docs/linux/encryption.md) 
[security](../docs/linux/grub_hardening.md) and more
customisation options compared to most proprietary firmware. As a
[libre](policy.md) software project, the code can be audited, and coreboot does
regularly audit code. The other main benefit is [*freedom* to study, adapt and
share the code](https://writefreesoftware.org/), a freedom denied by most boot
firmware, but not Libreboot! Booting Linux/BSD is also [well](../docs/linux/) 
[supported](../docs/bsd/).

Work done since last release
============================

New mainboards now supported:
-----------------------------

The *primary* focus of this release has been build system improvements, and new
build system features. However, these boards were added to Libreboot:

* [HP EliteBook 2170p](hp2170p.md) (laptop) (see previous news page linked)
* [Dell Precision T1650](hp8470p_and_dell_t1650.md) (desktop) (see linked prior news page)
* [Dell Latitude E6430](../docs/install/e6430.md) (laptop), courtesy Nicholas Chin
* [HP EliteBook 8470p](hp8470p_and_dell_t1650.md) (see linked prior news page)

This release also *re-adds* the following boards, which were present also in
an experimental release on 10 July 2023, just after the 20230625 release; so,
today's release is the first main Libreboot release version to re-add them:

* ASUS KFSN4-DRE (desktop/workstation)
* ASUS KCMA-D8 (desktop/workstation)
* ASUS KGPE-D16 (workstation/server)

The three re-added ASUS boards are using coreboot `4.11_branch`, with several
fixes that I made on top of it back in July 2023, fixing build errors so that
they compile properly on modern distros/toolchains. More info can be found in a
previous news article: [ASUS KGPE-D16, KCMA-D8 and KFSN4-DRE re-added to
Libreboot](fam15h.md)

GRUB LUKS2 now supported (with argon2 key derivation)
---------------------------------------------------

This was covered in a [previous article](argon2.md), which you should read.
GRUB *2.12* is now the version that Librebooot uses, although it's currently in
RC status (but works fine in my testing).

The previous Libreboot release also supported LUKS2 in the GRUB payload, but
only with PBKDF2 key derivation; most modern LUKS2 setups use argon2 instead,
which GRUB did not support. This meant downgrading LUKS2 to use PBKDF2, or
downgrading to LUKS1, both of which are insecure by comparison.

*This* new Libreboot release imports the [PHC argon2
implementation](https://github.com/P-H-C/phc-winner-argon2) into GRUB,
courtesy of [Axel](https://axelen.xyz/) who initially ported the code to run
under GRUB *2.06*, but this Libreboot release uses GRUB *2.12* (an RC revision
from git, at present).

Axel's code was published to [this AUR repository](https://aur.archlinux.org/cgit/aur.git/tree/?h=grub-improved-luks2-git&id=1c7932d90f1f62d0fd5485c5eb8ad79fa4c2f50d)
which [Nicholas Johnson](https://nicholasjohnson.ch/) then rebased on top of
GRUB *2.12*, and I then imported the work done by Nicholas, with his blessing.

These libreboot patches added argon2 support:

* <https://browse.libreboot.org/lbmk.git/commit/?id=2c0c521e2f15776fd604f8da3bc924dec95e1fd1>
* <https://browse.libreboot.org/lbmk.git/commit/?id=fd6025321c4ae35e69a75b45d21bfbfb4eb2b3a0>
* <https://browse.libreboot.org/lbmk.git/commit/?id=438bf2c9b113eab11439c9c65449e269e5b4b095>

This means that you can now boot from encrypted `/boot` partitions. I'm very
grateful to everyone who made this possible!

Simplified commands (build system)
-------------------------

Simply put, lbmk (the Libreboot build system) is now *easier to use*; there
are only *12* shell scripts in this release, versus 50 or so in the previous
release, and the command structure has been simplified.

For example, `./build boot roms` is now `./build roms`, and the
various `./build module projectname` commands are e.g. `/update trees -b seabios`

As always, you can find information about *using* the build system in
the [Libreboot build instructions](../docs/build/) and in the [lbmk
maintenance manual](../docs/maintain/).

TWO massive audits. 50% code size reduction in lbmk.
--------------------------------------------

Libreboot's build system, lbmk, is written entirely in shell scripts. It is
an automatic build system that downloads, patches, configures and compiles
source trees such as coreboot and various payloads, to build complete ROM
images that are easier to install. More info about that is available in
the [lbmk maintenance manual](../docs/maintain/).

The primary focus of *this* release has cultiminated in two *audits*, namely
[Libreboot Build System Audit 2](audit2.md) and then [Libreboot Build System
Audit 3](audit3.md); the changes in these audits were made *after* the last
release, and so they are part of *today's* release.

Changes include things like vastly reduced code complexity (while not
sacrificing functionality), greater speed (at compiling, and boot speeds are
higher when you use the GRUB payload), many bug fixes and more.

You can learn more about the build system changes by reading those two pages.
Their changes will also be listed here. The Libreboot build system (lbmk) has
been completely re-designed, since the last release. It's the same fundamental
design, but all of the commands have changed, and everything is much simpler.

Serprog firmware building (RP2040 and STM32)
-----------------------------------

In addition to coreboot firmware, the Libreboot build system (lbmk) can now
build *serprog* firmware, specifically `pico-serprog` and `stm32-vserprog`, on
all devices that these projects support.

The *serprog* protocol is supported by flashrom, to provide SPI flashing. It
can be used to set up an external SPI flasher, for [flashing Libreboot
externally](../docs/install/spi.md).

Pre-compiled firmware images are available, for many of these devices, under
the `roms/` directory in this Libreboot 20231021 release! Riku Viitanen is the
one who added this capability to Libreboot.

Updated U-Boot revision (2023.10)
----------------------------

Alper Nebi Yasak submitted patches that update the U-Boot revision in
Libreboot, on `gru_bob` and `gru_kevin` chromebooks. Additionally, the `cros`
coreboot tree has been merged with the `default` tree instead (and the `default`
tree has been updated to coreboot from 12 October 2023).

Many improvements were made to these boards, which you can learn about by
reading these diffs:

* <https://browse.libreboot.org/lbmk.git/commit/?id=eb267733fabe6c773720706539ef37f1ce591f81>
* <https://browse.libreboot.org/lbmk.git/commit/?id=8b411963b7e4941cbd96ac874d0582eaa20ea998>
* <https://browse.libreboot.org/lbmk.git/commit/?id=b2d84213dae4e199b4e4fa4f70dd6e3fbf5d90c4>
* <https://browse.libreboot.org/lbmk.git/commit/?id=f459e05ecd40592d80d119d16449d40f0dfbfa78>
* <https://browse.libreboot.org/lbmk.git/commit/?id=5b4ced3329f5fd8cb1fa166c8ac424e0bb618d67>
* <https://browse.libreboot.org/lbmk.git/commit/?id=46e01c0e1dade74f5ce777bf8593fe2722318af2>
* <https://browse.libreboot.org/lbmk.git/commit/?id=7afe2f39189fa196547c3dd9f9f617cfab91d835>
* <https://browse.libreboot.org/lbmk.git/commit/?id=f7db91c848f1fbf6bea93b62dfa4313ff550eeec>
* <https://browse.libreboot.org/lbmk.git/commit/?id=f9bad4449aa97aa2eb21f2254c0ad1515119888a>
* <https://browse.libreboot.org/lbmk.git/commit/?id=fea0cec24a1f2b03cf3c8b928259222f0bcf2357>
* <https://browse.libreboot.org/lbmk.git/commit/?id=f08102a22731182e8ad2f678ab39b19508fd455a>
* <https://browse.libreboot.org/lbmk.git/commit/?id=4e7e4761918d2cb04f3bf664c8c0ea8426a0e3bc>
* <https://browse.libreboot.org/lbmk.git/commit/?id=6e65595da5301b9b8c435a9ab55e6f0d9b01a86d>
* <https://browse.libreboot.org/lbmk.git/commit/?id=4d9567a7561df6eeb0dd81f2faf522c8526163b0>

Thank you, Alper!

Coreboot, GRUB, U-Boot and SeaBIOS revisions
------------------------------------

In Libreboot 20231021 (*this release*):

* Coreboot (default): commit ID `d862695f5f432b5c78dada5f16c293a4c3f9fce6`, 12 October 2023
* Coreboot (cros): MERGED WITH `coreboot/default` (see above)
* Coreboot (fam15h\_udimm): commit ID `1c13f8d85c7306213cd525308ee8973e5663a3f8`, 16 June 2021
* GRUB: commit ID `e58b870ff926415e23fc386af41ff81b2f588763`, 3 October 2023
* SeaBIOS: commit ID `ea1b7a0733906b8425d948ae94fba63c32b1d425`, 24 August 2023
* U-Boot: commit ID `4459ed60cb1e0562bc5b40405e2b4b9bbf766d57`, 2 October 2023

In Libreboot 20230625 (*previous release*):

* Coreboot (default): commit ID `e70bc423f9a2e1d13827f2703efe1f9c72549f20`, 17 February 2023
* Coreboot (cros): commit ID `8da4bfe5b573f395057fbfb5a9d99b376e25c2a4` 2 June 2022
* Coreboot (fam15h\_udimm): DID NOT EXIST
* GRUB: commit ID `f7564844f82b57078d601befadc438b5bc1fa01b`, 14 February 2023
* SeaBIOS: commit ID `ea1b7a0733906b8425d948ae94fba63c32b1d425`, 20 January 2023
* U-Boot (for coreboot/cros): commit ID `890233ca5569e5787d8407596a12b9fca80952bf`, 9 January 2023

As you can see, all revisions are quite new in this release.

Build system tweaks
===================

resources/ now config/
----------------------

The `resources/scripts/` directory is now `script/`, and what was `resources/`
now only contains configuration data plus code patches for various projects,
so it has been renamed to `config/` - I considered splitting patches
into `patch/`, but the current directory structure for patches is not a problem
so I left it alone.

Also, the IFD/GbE files have been moved here, under `config/ifd/`. Vendor
downloads now go under `vendorfiles/`, separate from ifd/gbe files, because
the ifd/gbe files *are* only configuration files but they are stored in a (well
understood, parseable) binary format.

Full list of changes (detail)
--------------------

The changes are (not necessarily in order), but they do not necessarily cover
things like mainboards or extra features added; these are covered in sections
above. This list is essentially a combination of the audit2 and audit3 change
logs, combined:

* Much stricter, more robust error handling; too many changes to list here, so
  check the git log. Also, errors that *are not errors* are no longer treated as
  such; Libreboot 20230625's build system was actually too strict, sometimes.
* Most logic has been unified in single scripts that perform once type of task
  each, instead of multiple scripts performing the same type of talk; for
  example, defconfig-based projects now handled with the same scripts, and
  preparing trees for them is done the same. These unifications have been done
  carefully and incrementally, with great thought so as to prevent *spaghetti*.
  The code is clean, and small.
* GitHub is no longer used on main Git repository links, instead only as backup
* Backup repositories now defined, for all main repos under `config/git/`
* Single-tree projects are no longer needlessly re-downloaded when they already
  have been downloaded.
* Better integrity checking when downloading vendor files
* GRUB LUKS2 support now available, with argon2 key derivation; previously, only
  PBKDF2 worked so most LUKS2 setups were unbootable in Libreboot. This is fixed.
* Vastly reduced number of modules in GRUB, keeping only what is required.
* Scrubbing of vendor files *now* handled by the inject script, rather than
  the release script. This enables more robust handling of configs pertaining
  to vendor files, that tell lbmk where the files are and how to insert them; it
  therefore follows that this same script should be used to delete them.
* Use `--mtime` and option options in GNU Tar (if it is actually GNU Tar), when
  creating Tar archives. This results in partially reproducible source archives,
  and consistent hashes were seen in testing, but not between distros.
* Always re-inialitise `.git` within lbmk, for the build system itself, if
  Git history was removed as in releases. This work around some build systems
  like coreboot that use Git extensively, and are error-prone without it.
* More robust makefile handling in source trees; if one doesn't exist, error
  out but also check other makefile name combinations, and only error out if
  the command was to actually build.
* ROMs build script: support the "all" argument, even when getopt options are
  used e.g. `-k`
* Disabled the pager in `grub.cfg`, because it causes trouble in some
  non-interactive setups where the user sees an errant message on the screen
  and has to press enter. This fixes boot interruptions in some cases, allowing
  normal use of the machine. The pager was initially enabled many years ago,
  to make use of cat a bit easier in the GRUB shell, but the user can just
  enable the pager themselves if they really want to.
* U-Boot can now be compiled standalone, without using the ROMs build script,
  because crossgcc handling is provided for U-Boot now in addition to coreboot.
* Unified handling of git/vendor config files, containing URLs, revisions,
  checksums and so on. This is handled by a single function
  under `include/option.sh`
* All helper scripts are now under `include/`, and main scripts in `script/`,
  called by the main `build` script
* Intel ME extraction is now provided in one function, instead of two, when
  downloading vendor files per mainboard, before running it
  through `me_cleaner`
* Unified checking of the destination file, when downloading vendor updates.
  This results in more reliable checking of whether a vendor file has already
  been downloaded or not, where it is only handled if missing.
* Generally purge unused variables in shell scripts
* Simplified initialisation of variables in shell scripts, using the `setvars`
  function defined under `include/err.sh`
* Vendor scripts: archive extraction is now unified, the same method used for
  each archive. This enables more robust checking of hashes and so on.
* Support patch subdirectories, when applying patches. This is done recursively,
  making it possible to split up patch files into smaller sets inside sub
  directories, per each source tree (or target of each source tree, where a
  project is multi-tree within lbmk)
* SPDX license headers now used, almost universally, in all parts of lbmk.
* Files such as those under `config/git` or `config/vendor` are now
  concatenated, traversing recursively through the target directory; files first,
  then directories in order, and for each directory, follow the same pattern
  until all files are concatenated. This same logic is also used for patches.
  This now enables use of subdirectories, in some config/patch directories.
* General code cleanup on `util/nvmutil`
* Git histories are more thoroughly deleted, in third party source trees during
  release time.
* Symlinks in release archives are no longer hard copies; the symlinks are
  re-created by the release script, because it clones the current lbmk work
  directory via Git (local git clone), rather than just using `cp` to copy links.
* More deeply integrated the Intel MRC download script (from coreboot) into
  Libreboot's vendor scripts, removing its download logic and re-using that
  from Libreboot's scripts instead; now, the MRC script only contains extraction
  logic, and it is an *include* file, rather than a standalone script.
* Properly output to stderr, on printf commands in scripts where it is either
  a warning prior to calling `err`, or just something that belongs on the error
  output (instead of standard output).
* Don't use the `-B` option in make commands.
* Where no-microcode ROM images are provided, ensure that the ROM hashes still
  match when running the vendor inject script. This is only useful on the
  Dell Latitude E6400, which is otherwise FSDG-compatible but (in Libreboot)
  comes with or without microcode updates, and with or without the Nvidia VGA
  ROM (handled by vendor inject/download scripts) for dGPU variants. Verification
  previously failed, under certain conditions, when inserting that VGA ROM.
* SECURITY: Use sha512sum (not sha1sum) when verifying certain downloads. This
  reduces the chance for collisions, during checksum verification.
* Set GRUB timout to 5s by default, but allow override and set to 10s or 15s
  on some mainboards.
* Vendor scripts: don't use `/tmp` for ROM images when inserting vendor files.
  In case `/tmp` is a tmpfs and not much RAM is available, it is paramount that
  the user's file system is used instead, where there is likely greater capacity;
  it is done under `tmp/` in lbmk (not to be confused with `/tmp`).
* Support both curl and wget, where files are downloaded outside of Git; defer
  to Wget when Curl fails, and try each program three times before failing. This
  results in more resilient downloading, on wobbly internet connections.
* Don't clone Git repositories into `/tmp`, because it might be a tmpfs with
  little memory available; clone into `tmp/gitclone` instead, within lbmk,
  and `mv` it to avoid unnecessary additional writes (`mv` is much more efficient
  than `cp`, for this purpose).
* Removed unused `target.cfg` handling in vendor scripts, because they use
  the concatenated config format instead (they always have).
* move `me7_updater_parser.py` to `util/` (not under `script/`)
* The directory containing vendor files no longer exists in lbmk, because it
  is instead created when needed; the ifd/gbe files were moved to `config/ifd`
  so the vendorfile directory became redundant.
* Coreboot builds: automatically run make-oldconfig, to mitigate use of raw
  coreboot config where a revision was updated but the config was untouched.
  This may still result in a confirmation dialog, and it's still recommended
  that the configs be updated per revision (or switch them to defconfigs).
* Vastly simplified directory structure; `resources/scripts/` is now `script/`,
  and `resources/` was renamed to `config/`; ifd and gbe files were also moved
  to `config/ifd/`. Commands are now 1-argument instead of 2, for example
  the `./build boot roms` command is now `./build roms`.
* memtest86plus: only build it on 64-bit hosts, for now (32-bit building is
  broken on a lot of distros nowadays, and lbmk doesn't properly handle cross
  compilation except on coreboot or U-Boot)
* (courtesy of Riku Viitanen) don't use cat on loops that handle lines of text.
  Instead, use the `read` command that is built into `sh`, reading each line.
  This is more efficient, and provides more robust handling on lines with
  spaces in them.
* Don't support removal of microcode (during release time) on untested targets.
  Set `microcode_required="y"` on most boards, but leave it set to `"n"` on
  platfroms such as GM45 (ThinkPad X200/T400, Dell E6400, etc); anything FSDG
  compatible, in other words.
* Improved Dell Latitude E6400 support; the same image now provides iGPU and
  dGPU support, since it's SeaBIOS-only anyway, so a VGA ROM is inserted into
  the same ROM that also enables libgfxinit, enabling the Intel or Nvidia GPU
  to be used (if the VGA ROM is missing, only the Intel GPU will work).
* *ALL* projects now have submodules downloaded at build time, not just multi
  tree projects such as coreboot - and a few projects under `config/git` have
  had certain `depend` items removed, if a given project already defines it
  under `.gitmodules` (within its repository).
* Improved cbutils handling; it's now even less likely to needlessly re-build
  if it was already built.
* The release build script no longer archives what was already built, but
  instead builds from scratch, creating an archive from source downloads
  first before building the ROM archives. This saves time because it enables
  a single build test per release, whereas at was previously necessary to test
  the Git repository and then the release archive. Testing both is still desired,
  but this behaviour also means that whatever is built at release time is
  guaranteed to be the same as what the user would build (from archives).
* Improved handling of `target.cfg` files in multi-tree projects coreboot,
  SeaBIOS and U-Boot. Unified to all such projects, under one script, and
  with improved error handling.
* Only remove microcode (where that behaviour is enabled per board) in release
  ROMs, but not during build time. This results in reduced disk usage during
  development, but release archives still contain the no-microcode option if
  you want to use that; manual removal is also still possible, during development.
* GRUB payload: all ROM images now contain the same ELF, with all keymaps
  inserted. This speeds up the build process, and enables easier configuration
  when changing the keyboard layout because less re-flashing is needed.
* Simplified IFD handling on ICH9M platforms (e.g. X200/T400 thinkpads); the
  ich9gen utility wasn't needed anymore so ich9utils has been removed, and now
  the IFD/GbE files are included pre-assembled (generated by ich9gen). Ich9gen
  can still be used, or you can re-generate with coreboot's bincfg; the ifdtool
  util can be used to edit IFD and nvmutil (part of Libreboot) can change MAC
  addresses. The ich9utils code was always redundant for the last few years,
  especially since 2022 when nvmutil was first written.
* Running as root is now forbidden, for most commands; lbmk will exit with
  non-zero status if you try. The `./build dependencies x` commands still work
  as root (they're the only commands available as root).
* Enabled memtest86plus on more boards, where it wasn't previously enabled.
* Only enable SeaBIOS as first payload on desktops, but still enable GRUB as
  second payload where GRUB is known to work (on each given host). The text
  mode and coreboot framebuffer modes are provided in each case, where feasible.
* The `list` command has been mostly unified, making it easier to tell (from
  lbmk) what commands are available, without having to manually poke around
  under `script/`.
* The `-T0` flag is now used, universally, on xz commands. This makes `xz` run
  on multiple threads, greatly speeding up the creation of large tar archives.
* Universally use `-j` in make commands, for multi-threading, but it relies
  on `nproc` to get thread count, so this only works if you have `nproc` (you
  probably don't, if you run BSD; BSD porting is still on TODO for Libreboot)
* File names as arguments now universally have quotes wrapped around them, and
  similar auditing has been done to all variables used as arguments everywhere
  in lbmk. There were cases where multiple arguments were wrongly quoted then
  treated as a single argument, and vice versa. This is now fixed.
* Re-wrote `.gitcheck`; now, a global git name/email config is always required.
  The only behaviour (setting local config, and unsetting) was quite error-prone
  under fault conditions, where cleanup may not have been provided, or when
  execution was interrupted, resulting sometimes in accidentally committing
  to `lbmk.git` as author named `lbmkplaceholder`.
* The new BSD-like coding style is now used on *all* shell scripts in lbmk. A
  few scripts still used the old lbmk coding style, as of audit 2.
* Scripts no longer directly exit with non-zero status, under fault conditions;
  instead, `x_` or `err` is used to provide such behaviour. This results in all
  exits from lbmk being consolidated to `err`, under fault conditions. - zero
  exits are also consolidated, going only through the main script, which has its
  own exit function called `lbmk_exit` that provides `TMPDIR` cleanup.
* *Copy* `dl_path`, don't move it, when downloading and extracting a vendor
  file. This reduces the change of it being missing later when lbmk is run again.
* BSD-style error handling implemented, with an `err` function (and functions
  that use it) inside `include/err.sh`; there is also `x_` which can be used
  to run a command and exit automatically with non-zero status, useful because
  it provides more verbose output than if you just relied on `set -e`, and it
  still works when a script *does not* use `set -e` - however, it is not used
  on all functions, because it works by executing `$@` directly, which can break
  depending on arguments. Therefore, some scripts just default to `|| err` for
  providing breakage in scripts.
* Memtest *6.2* now used (instead of *5.x* releases). This is essentially a
  re-write, and it works on the coreboot framebuffer, whereas previous revisions
  only worked on text mode setups.
* NO MAKEFILE. The Makefile in lbmk has been removed. It was never meaningfully
  used because all it did was run lbmk commands, without implementing any logic
  itself. A Makefile may be added again in the future, but with a view to
  installing *just the build system* onto the host system, to then build ROM
  images under any number of directories. Lbmk's design is strictly no-Makefile,
  but it uses Makefiles provided by third party source trees when building them.
* Safer GRUB configuration file handling between GRUB memdisk and coreboot CBFS;
  it is no longer possible to boot without a GRUB config, because the one in
  GRUB memdisk is provided as a failsafe, overridden by *inserting* one in CBFS,
  but there is no config in CBFS by default anymore.
* The build system *warns* users about `elf/` vs `bin/`, when it comes to
  flashing coreboot ROM images; it tells them to use `bin/` because those
  images do contain payloads, whereas the ones under `elf/` do not.
* VASTLY more efficient build process; all coreboot ROMs without payload are
  now cached under `elf/`, as are payloads, then they are joined separately by
  the usual ROMs build script, and these cached ROMs contain many changes in
  them that were previously handled by `moverom` in the main ROM build script.
  Under the new design, repetitive steps are avoided; payloads are inserted into
  a copy of the cached ROMs under `TMPDIR`, *before* being copied for keymaps
  and small files; this eliminates delays caused by slow compression (LZMA is
  always used, when inserting payloads). After crossgcc and the payloads are
  compiled, the ROM with coreboot builds in under a minute, whereas it would
  have previously taken several minutes on most Libreboot-supported hardware.
* VASTLY reduced GRUB payload size; modules that aren't needed have been removed
  resulting in much smaller GRUB payloads, that also boot faster.
* ALL defconfig creation, updating and modification are handled by the same
  script that *also* handles compiling, as mentioned in the bullet-point below.
* ALL main source trees are now compiled, downloaded, configured and cleaned
  using the same script. The *download* (Git) logic is a separate file
  under `include/` and its functions are called by the main build script, which
  provides a stub for this.
* Scripts are no longer executed directly, ever, except the main script. All
  scripts are otherwise executed from `script/`, inheriting the `TMPDIR`
  variable set (and exported) by lbmk.
* Generally improved user feedback in scripts, especially the vendor scripts.
* Coreboot, U-Boot and SeaBIOS are now downloaded, configured and compiled using
  the exact same script. Although these codebases differ wildly, their build
  systems use the same design, and they are compatible from a user-interface
  perspective.
* Vastly improved `/tmp` handling; a universal `TMPDIR` is set (environmental
  variable) and exported to all child processes running lbmk scripts. On exit,
  the main tmp directory is purged, cleaning all tmp directories under it.
* Improved handling of vendor file hashes; previously, the backup would only
  be tried if the first one failed to download, but if the first file succeeded
  and yet had a bad hash, the backup would not be tried. Now the backup is tried
  when either the first download fails OR it has a bad hash, making downloads
  of vendor files more resilient to network failure.
* When extracting ME files from vendors, more types of archives are supported
  for decompression at build time.
* Fixed bug where vendor files were always being downloaded from backup URLs
  at build time.
* Spoof the user agent string mimicking that of Tor Browser, when downloading
  vendor files at build time. This circumvents restrictions based on user agent
  string, when lbmk interacts with certain HTTP servers.
* General simplification of coding style on all shell scripts.
* Abort (with non-zero exit) if KBC1126 EC firmware fails to download at build
  time.
* Fixed some variable initialisations in the coreboot ROM image build script
* Don't enable u-boot on QEMU x86 images (due to buggy builds, untested) 
* Haswell (libre MRC) coreboot tree: fixed acpica downloads, which no longer
  work on the upstream URL. Old acpica binaries now hosted on Libreboot rsync.
* Fixed coreboot-version file inserted into coreboot trees, when compiled
  on Libreboot release archives.
* Very general auditing has been done, finding and fixing bugs.
* Reduced the number of scripts significantly. There were about 50 scripts in
  the Libreboot 20230625 build system. There are closer to *20* in today's
  Libreboot revision.
* *Massively reduced the size of the build system*: where only shell scripts are
  concerned, the total sloccount in Libreboot 20230625 was 3388 source lines.
  As of *today*, that figure stands at 2644 source lines, a *22% reduction in
  code size*.
* Many scripts that were separate are now unified. For example: the scripts
  handling defconfigs files on SeaBIOS, u-Boot and coreboot have now been
  merged into a single script, performing the same work *better* in less code.
* Ditto many other scripts; repeated logic unified, logic generalised. The
  logic for *downloading* coreboot and u-boot was unified into one script,
  basing off of the coreboot one, and then expanding to also cover SeaBIOS.
  Most building (e.g. handling of Makefiles) is now done in a single script.
* Far superior error handling; in many scripts, the `-e` option in `sh` was
  heavily relied upon to catch errors, but now errors are handled much more
  verbosely. *Many* fault conditions previously did not make lbmk *exit* at all,
  let alone with non-zero status, and zero status was sometimes being returned
  under some edge cases that were tested. Error handling is more robust now.
* `util/ich9utils` (containing `ich9gen`) was *removed*, thus eliminating about
  3000 source lines (of C code) from lbmk. The `nvmutil` program, also provided
  by and originating from the Libreboot project, can already change GbE MAC
  addresses. Coreboot's bincfg can generate ich9m descriptors, and ifdtool can
  manipulate them; so the features provided by ich9utils were superfluous, since
  they are available in other projects that we ship. We now ship pre-built
  ifd/gbe configs on these machines, which can be modified or re-assembled
  manually if you want to. This eliminates a moving part from Libreboot, and
  speeds up the build a little bit.
* ROM images (of coreboot) build *much faster*: no-payload coreboot ROMs are
  cached on disk, as are payloads, where previously only the latter was cached.
  These cached images have as much inserted into them as possible, to eliminate
  redundant steps in the build process. The `elf` directory contains these, and
  the existing `bin` directory still holds the full ROM images (containing
  payloads) when compiled.
* GRUB payload: vastly reduced the size of the payload, by eliminating GRUB
  modules that were not needed. About 100KB of compressed space saved in flash!
* GRUB payload: [argon2 key derivation supported](argon2.md) - this means LUKS2
  decryption is now possible in GRUB. This work was performed by Nicholas
  Johnson, rebasing from Axel's AUR patch for GRUB 2.06 (Libreboot currently
  uses GRUB 2.12).
* Blobutil: generally more reliable now at downloading vendor files, especially
  under fault conditions; for example, if a download failed before, it'd try
  a backup link, but now it also tries the backup link if main download succeeds
  but checksum verification didn't; and SHA512 checksums are now used, for
  greater security, whereas Libreboot 20230625 used sha1sum (now we use
  sha512sum). A user agent is specified in wegt, matching that used by Tor
  Browser (which in turn mimics Firefox running on Windows). This is needed
  for some vendors, which seem to dislike wget's default user agent.
* The *new* coding style is now used on many more scripts, including
  the `build/boot/roms_helper` script - the new style is much cleaner,
  mandating that logic be top-down, with a `main()` function defined; it's
  basically inspired by the OpenBSD coding style for C programs, adapted to
  shell scripts.
* All GRUB keymaps now included; a single `grub.elf` is now used on all ROM
  images. The `grub.cfg` goes in GRUB memdisk now, but can be overridden by
  inserting a `grub.cfg` in CBFS; many behaviours are also controlled this way,
  for example to change keymaps and other behaviours. This results in *much*
  faster builds, because a different GRUB payload doesn't have to be added to
  each new ROM image; such takes time, due to time-expensive LZMA compression.
  This, plus the optimised set of GRUB modules, also makes GRUB itself load
  much faster. All of the fat has been trimmed, though still quite a lot more
  than a Crumb.
* A lot of scripts have been removed entirely, and their logic not replaced;
  in many cases, Libreboot's build system contained logic that had gone unused
  for many years.
* More reliable configs now used on desktop mainboards: SeaBIOS-only for start,
  but GRUB still available where feasible (in the SeaBIOS menu). This makes it
  more fool proof for a user who might use integrated graphics and then switch
  to a graphics card; the very same images will work.
* TMPDIR environmental variable now set, and exported from main parent process
  when running lbmk; child processes inherit it, and a single tmp dir is used.
  This is then automatically cleaned, upon exit from lbmk; previously, lbmk did
  not cleanly handle `/tmp` at all, but now it's pretty reliable.

FULL list of changes (git log)
------------------------------

The log is as follows, relative to Libreboot 20230625:

```
* c7e764a3 update/release: confirm vdir path on exit 
* 1c8b2114 update/release: copy crossgcc to archive 
* 54a05fc1 always re-generate .git in lbmk 
* 52c9416b update flashrom revision 
* af1c1e10 add backup git repo for flashrom 
* 18364822 Revert "config/git: don't download flashrom" 
* ac442808 config/git: add more backup repos 
* 75980052 git/config: don't use github on main repos 
* bf4ea810 config/git: don't download flashrom 
* da3044e7 git/config stm32-vserprog: don't fetch libopencm3 
* 782371a5 update/release: delete *all* .git and .gitmodules 
* 743a425c include/git: fix already-exists download message 
* 73145b79 Revert "Revert "include/git: don't re-download single-trees"" 
* 31b35bb4 include/git: fix error caused by sh idiosyncrasy 
* baa3d4f2 Revert "include/git: don't re-download single-trees" 
* 8de7bc93 include/git: don't re-download single-trees 
* d1f23eca config/git: remove rpi-pico-tinyusb dependency 
* 97e5207e config/git: give pico-sdk its own file 
* 182ee8e4 update/trees: don't run make if mode=fetch 
* 54eb347a include/git: fetch submodules on one-tree projects 
* f855611c include/git: only download submodules if possible 
* 0c32c1d6 update/release .git/*: delete one more level up 
* 0375cfaf update/release: don't hardcode project names 
* d245e0b1 consistent naming for src/pico-serprog 
* fac62a8c config/git: name files per download name 
* 0e1602f5 do a nice thing 
*   7b206008 Merge pull request 'fix_distro_dependencies - part 2' (#139) from andreamtp/lbmk:fix_distro_dependencies into master 
|\  
| * a16cd1a3 Added python-unversioned-command for Fedora38 
| * 8a063f6b Fix Debian/Ubuntu dependencies 
* | 6af65ad4 error handling code cleanup and fixes 
* | 4e54a051 another code cleanup 
* | 8d9aeef3 lbmk: use 2-level directory structure in script/ 
* | 0b98c9b0 minor code cleanup in shell scripts 
* | 8b6e44a1 Merge pull request 'Fix F38/Ubuntu 20.04 dependencies' (#137) from andreamtp/lbmk:fix_distro_dependencies into master 
|\| 
| * 6758b5c8 Fix F38/Ubuntu 20.04 dependencies 
* |   9fac3c12 Merge pull request 'Fix Void Dependencies for building Serprog' (#138) from neutrocyte/lbmk:fix_void_dependencies into master 
|\ \  
| |/  
|/|   
| * e63399cf Fixed Void Dependencies for building Serprog 
|/  
* 4cdf60e6 util/spkmodem-recv: detailed copyright history 
* fc2cab31 update/release: fix missing variable definition 
* c14461a5 delete include/vendor.sh and merge elsewhere 
* d8c2c245 vendor.sh: move some functions to vendor/download 
* 0f807762 update .gitignore for the dell-flash-unlock binary 
* 34b8687e coreboot/fam15h: remove redundant patch 
*   1a299f1b Merge pull request 'util/e6400-flash-unlock: Rename to dell-flash-unlock' (#135) from nic3-14159/lbmk:rename-e6400-flash-unlock into master 
|\  
| * 5d6946c4 util/e6400-flash-unlock: Rename to dell-flash-unlock 
* |   8583a05d Merge pull request 'Update U-Boot to v2023.10 and use default coreboot tree for gru chromebooks' (#136) from alpernebbi/lbmk:uboot-v2023.10 into master 
|\ \  
| * | 4d9567a7 coreboot: gru: Use default coreboot tree 
| * | 6e65595d u-boot: gru: Do not persist EFI variables 
| * | 4e7e4761 u-boot: gru: Enable more EFI commands 
| * | f08102a2 u-boot: gru: Enable more bootstd features 
| * | fea0cec2 u-boot: gru: Do not reset on panic 
| * | f9bad444 u-boot: gru: Enable poweroff command 
| * | f7db91c8 u-boot: gru: Disable VIDEO_COPY 
| * | 7afe2f39 u-boot: Set EFI variable buffer size to upstream value 
| * | 46e01c0e u-boot: Avoid building U-Boot-only binman images 
| * | 5b4ced33 u-boot: Add patch to avoid regulator errors 
| * | f459e05e u-boot: Update to v2023.10 
| * | b2d84213 update/project/trees: Add flags for more kconfig actions 
| * | 8b411963 u-boot: qemu_arm64_12mb: Remove misleading rev field 
| * | eb267733 build/fw/coreboot: Fix misuse of raw u-boot.bin as payload 
|/ /  
* | 65af756f x/xx: slightly more verbose error messages 
* | 19f1e008 vendor/inject: only build nvmutil if required 
* | 3f8636ff vendor/inject: simplified file handling 
* | 7b741dd0 update/release: remove unused variables 
* | e0feda63 update/release: fix/simplify mtime handling 
* | ec0b38af update/release: nuke roms using the inject script 
* | 2ebadb7f build/release: don't include tmp/ in src tarball 
* | 27aaae59 update/release: also set timestamp on srcdir 
* | ca78fc67 update/release: be more thorough updating times 
* | 7cd84aec update/release: use getops OPTARG correctly 
* | b5db0480 update/release: delete multi-tree upstream repos 
* | 6846c9f7 update/release: if *GNU* tar, use --mtime 
* | c401efdd build/release: support skipping rom builds 
* | 268fd6ce update/release: make src tarball first, then roms 
* | 653a8571 put space in the warning message about elf/ 
* | c44a38ae only build cbutils if required 
|/  
* 42068f7c coreboot/default bump: rev d862695f5f, 12 Oct 2023 
* 09881212 use me_cleaner from coreboot instead of upstream 
* 1f331642 nvmutil: simplify endianness handling 
* 3162d60d nvmutil: don't reset errno before write 
* f989360e nvmutil: reset errno on successful write 
* 3ad171fd nvmutil: simplify prototype declarations 
* 96fd88c5 build: fix bad command in help text 
* 5b8b55f2 build/fw/coreboot: fix bad commands in help text 
* 067a358d fix warning about coreboot elf/ vs bin/ 
*   13c58200 Merge pull request 'util/e6400-flash-unlock: Update to upstream version' (#134) from nic3-14159/lbmk:e6400-flash-unlock-updates into master 
|\  
| * 724cb39f util/e6400-flash-unlock: Update to upstream version 
* | 67ffb513 build/fw/coreboot: warning about bin/ versus elf/ 
|/  
* 634aac0b config/dependencies: fix unifont on arch/parabola 
* 7e3a031a include/err.sh: don't run check_git 
* b61e3feb config/dependencies/ubuntu: symlink to debian 
* 4ea9b9fb config/dependencies: add popos config 
* f8528d12 config/dependencies/debian: add autopoint 
* 21db72b6 disable 32-bit memtest86plus, only build 64-bit 
* d1ba94ea update/release/*: merge to update/project/release 
* e7a77b50 build/fw/coreboot: reset grub background each time 
* 92abbb25 update/release/roms: copy license files to archive 
* 85bee1f8 bump grub revision 
* d58bc5ff bump seabios revision 
* 1e89264c update/project/*: merge to update/project/trees 
* a413c01a update/project/trees: handle seen in fetch_config 
* c8bace0d build/fw/grub: re-add end confirmation message 
* ba324d8c build/coreboot/grub: move to build/fw/grub 
* 4708da2c use quotes when checking empty strings in scripts 
* 0fad3497 build/fw/coreboot: fix error "unexpected operator" 
* ea27c928 update/project/build: move helpers to option.sh 
* 0ed2ec29 build/coreboot/util: merge to update/project/build 
* b6d9e6c1 build/fw/coreboot: don't support no-all all arg 
* 0962600c build/fw/coreboot: correctly check built targets 
* fa8e204f unified projectname/version/versiondate handling 
* 24584296 put include/export.sh in build script 
* 62cc895c rename blob/ to vendor/ 
* 3c7e37b1 update/blobs: correct utils paths check 
* 5e81024e update/blobs: don't hardcode kbc1126 util check 
* 9f8f230b update/blobs: don't needlessly re-build uefitool 
* fe502da9 Rename blobs/ to blob/ 
* 4e39d5a5 put all src downloads under src/ 
* 965b6a7e rename build/firmware/ to build/fw/ 
* 5494ffb3 build/firmware/coreboot: confirm compiled roms 
* ce10c1b3 build/firmware/coreboot: support "all" without all 
* 2d483d2f move build/release/* to update/release 
* 315d0c45 mv build/fw/serprog,build/boot/roms build/firmware 
* 863081c3 remove build symlink, rename lbmk to build 
* 2d16e1ee rename build/project/trees to update/project/build 
* 1c2de7f9 unify build/grub/* to build/coreboot/grub 
* 176722a8 unify handle/make/* into build/project/trees 
* 9d419e77 handle/make/*: unified main() function 
* 10684102 general code cleanup in shell scripts 
* cad7648a build/boot/*: merge all logic into one script 
* 923a96c1 check git/version: properly call err() 
* 1223bfae check_git: call fail() first (fallback to err) 
* 727dc7ff more verbosely print git config error 
* fbd464b4 include/err.sh: checkgit,checkversion 
* e638c3e4 update/project/trees: remove errant assignments 
* 68e1787c update/project/trees: split up main() 
* 5de8eda2 general code cleanup in shell scripts 
* 334aa1f7 handle/make/config: fix formatting on variables 
* 8097baa0 handle/make/file: check for all default makefiles 
* 0db6c0a4 update/blobs/download: remove errant comment 
* 3af63fb8 handle/make/file: exit 0 if no makefile 
* ad74b4c2 handle/make/file: run extra arg before, not after 
* 2e60e117 grub.cfg: disable the pager 
* d9719cae handle/make/file: do multiple project arguments 
* cb29c96c lbmk: simplify/correct exit commands / cleanup 
* 9dce8236 update/project/trees: fix error handling on mkdir 
* 0f86a393 update/project/trees: optimise error handling 
* 67ac799d update/project/trees: simplified error handling 
* d38b958d include/err x_(): more verbose error message 
* 8886f995 include/err: remove unused variable 
* cd2caecb update/project/trees: general code cleanup 
* bcbd3734 update/project/trees: rm yet another rm line 
* 0a63dce3 update/project/trees: remove one more rm line 
* 91c0f942 update/project/trees: remove redundant rm command 
* 7bead4f5 update/project/trees: remove unnecessary linebreak 
* 1dd97470 update/project/trees: rm "seen" in the right place 
* a3b3196d build/grub/payload: remove unnecessary linebreaks 
* 3fcad603 build/coreboot/utils: remove unnecessary check 
* 0a711ebc build/coreboot/utils: simplify argument handling 
* 7ce3f93e build/boot/*: unify more logic in main() 
* 7b02bb9a do not handle errors on mktemp in shell scripts 
* 8c03b886 Greatly simplify error handling in shell scripts 
* 5f914a4d build/boot/roms: optimise main() for code size 
* 92c6da7b build/boot/roms_helper: shorten variable names 
* 2a6fcf70 build/boot/roms: dont do init/displaymode argument 
* 42d4fa9b include/boot.sh: simplify variable initialisation 
* 9bc9dddf build/boot/roms_helper: simplify rom file handling 
* c477599c build/boot/roms_helper: general code cleanup 
* 26fc3f13 general code formatting cleanup in shell scripts 
* 0a0defd3 simplify initialising variables in shell scripts 
* 49b266eb build/boot/roms: only do 1 custom kbd/payload/mode 
* d268f5eb build/boot/roms: move usage() to include/boot.sh 
* 7922b6e0 build/boot/*: unified main() function 
* f3c4f208 build/boot/roms: split up handle_targets() 
* 4afa0aaa build/boot/roms: check all targets before building 
* 6125d341 build/boot/roms: merge handle_targets/build_target 
* 13f5a432 build/boot/roms: only run confirm_targets once 
* 5462bf1c build/boot/roms: rename buildrom to build_target 
* fc097b3e build/boot/roms: split up main() 
* 895073d7 build/boot/roms: simplify buildrom() handling 
* df7305a5 build/boot/roms: support "all" if argument passed 
* b3e69cd9 build/boot/roms: move help() to bottom of file 
* 385eb90c update/*/*: unified scanning of revisions/sources 
* 9f5a5450 blobs/download: move helpers to include/blobutil 
* 416704fb include/blobutil: try curl first, then wget 
* 6519cea9 include/blobutil: simplify check_defconfig() 
* ac05e5ff blobs/download: do IntelME extract in one function 
* 9b94df5d blobs/download: do final check of _dest in fetch() 
* 9a7bf4af blobs/download: don't pass dl_path as argument 
* 2b7ae8e2 blob scripts: unified handling of blob destination 
* 8ea62a16 remove unused variables in blob scripts 
* 32da4e31 merge include/fetch.sh, blobutil.sh, defconfig.sh 
* 710171f9 update/blobs/*: simplify mrc.bin handling 
* 0bb3c596 update/blobs/*: unified download/checksum logic 
* 5d934be7 blobs/download: remove unnecessary linebreaks 
* 3256ef3e blobs/download: remove unnecessary messages 
* 178b888a include/blobutil: properly set global variables 
* e9e1a3b4 blobs/download: simplify downloading of files 
* 781d0a80 blobs/download: remove unnecessary error pipes 
* 9aef57df blobs/download: unified archive extraction 
* 74c48a88 move build/command/options to include/option.sh 
* a00b4337 build/release/roms: simplify strip_rom_image() 
* 3b9442f7 blobs/download: unified blobdir handling 
* 373c84e4 blobs/download: unified archive extraction 
* 1e92abb1 blobs/download: remove errant debug line 
* e73306ba remove script/update/blobs/extract 
* 16235cb6 blobs/download: simplify fetch_update() 
* d023327f blobs/download: greatly simplify sources handling 
* 65a32698 include/blobutil: simplify setting empty strings 
* 6b17cda1 blobs/download: simplify defconfig handling 
* b5628131 handle/make/config: check project in main() 
* f052f61f handle/make/config: split up main() 
* 67f4919f simplify getopts loops in shell scripts 
* 36b7f01a only update git submodules in project/trees 
* 81d073d5 update/project/*: unified git reset handling 
* eae173ec split up grub patches into subdirectories 
* a823bab3 include/git: support applying patch subdirectories 
* 3738ec90 update/project/*: unified patch handling 
* cd3225d8 update/project/trees: remove extra.sh handling 
* 42c9d7d2 build/grub/*: move common strings to variables 
* e94ba1f7 build/grub/payload: split up main() 
* b727f966 util/: use SPDX license and copyright headers 
* 20862019 Update email address for Leah Rowe copyrights 
* cc164209 Use SPDX license headers on all scripts 
* a7b767a4 update/repos: concatenate multiple revision files 
* 7966f911 handle/make/config: run fail() on error, not err() 
* 2d0e978c update grub revision 
* 905f3d8e util/nvmutil: remove xorswap() macro 
* 231015ef util/nvmutil: make setWord a macro 
* d9bed115 util/nvmutil: further optimise swap command 
* 5e801360 util/nvmutil: use correct comparisons on pointers 
* 137a548b util/nvmutil: optimise swap command 
* 4d448201 util/nvmutil: don't use err_if on argc check 
* 0897a0be util/nvmutil: always print filename in err_if 
* 9a92524a util/nvmutil: remove SIZE_8KB define 
* 5a129cea util/nvmutil: remove xpread/xpwrite macros 
* ac0e4999 util/nvmutil: remove unnecessary xclose macro 
* 83e6cfb2 util/nvmutil: simplify pledge and unveil handling 
*   7bb92acd Merge pull request 'merge serprog scripts' (#131) from Riku_V/lbmk:master into master 
|\  
| * 3c30e1e3 merge serprog scripts 
|/  
* f8704c0a lbmk: more verbose error messages 
* a1db59a5 lbmk: reduce indentation in execute_command() 
* a9ea277e lbmk: fail if ./build command options fails 
* f1f5b91a lbmk: simplify execute_command() 
* 662b9266 lbmk: remove "./buildpath mode all" 
* 4c734308 lbmk: break up main() 
* 5f197023 lbmk: always use lbmk_exit for exits 
* 3400e5a1 rel/src: fix multi-line command 
* 4df3d09b remove ich9utils entries from .gitignore 
*   20bf3a19 Merge pull request 'make clean stm32-vserprog for release' (#130) from Riku_V/lbmk:makeclean into master 
|\  
| * c3ac62b1 serprog: list available boards 
| * 24185bca fix typo serprog -> vserprog 
| * ccb36aa6 make libopencm3 correctly 
| * 5737abf0 make clean libopencm3 
| * 0bed0c35 Download libopencm3 before building 
| * 3d77b8a0 download and copy serprog related src 
| * 7dc86325 clean up pico-serprog for release 
| * 34d3629e make clean stm32-vserprog for release 
* | c400916e coreboot/hp8200sff_4mb: fix bad ifd path in config 
|/  
* 087f0e06 make lbmk help text actually vaguely helpful 
* 093d40ee build/release/src: be more thorough deleting .git 
* 630a6546 build/release/src: delete elf/ in srcdir 
* 0543350d handle/make/file: run make-clean first 
* 12f9afe6 build/release/src: remove cbutils/ in srcdir 
* fe00ab4e build/release/src: remove errant code 
* be4ed540 handle/make/config: distclean once per tree 
* f227cc08 handle/make/config: fix distclean/crossgcc-clean 
* 669c9770 handle/make/config: fix whitespace and 80-line bug 
* d28ad6aa build/release/roms: use -T0 on serprog tarballs 
* 308c21dd build/boot/roms stragglers: properly handle errors 
* c16b28ef build/release/src: re-create symlinks, don't copy 
* 32dcf9e5 coreboot/qemu_x86_12mb: re-add this mainboard 
* 5aef8156 scripts: use printf, not echo, where appropriate 
* 76e12cd4 update/blobs printf statements: use double quotes 
* 84bf47b5 scripts: better handling of printf: stdout/stderr 
* b78009e2 checkgit: properly print output to stderr 
* f45f5e62 update/project/*: remove redundant checks 
* 3e76e70d blobs/download: don't use the -B option in make 
* 877c691e build/release/roms: remove errant line break 
* f03efbc2 blobs/inject: add error condition on rm command 
* 20be007f blobs/inject: fix checksum validation if no-ucode 
* f989d5b4 blobs/sources: fix backup links on some files 
* 878550d5 use sha512sum to check downloads, not sha1sum 
*   022e0200 Merge pull request 'Add stm32-vserprog' (#129) from Riku_V/lbmk:stm32 into master 
|\  
| * bed444ff Add stm32-vserprog 
* | e9e4ada5 build/boot/rom: only insert scan.cfg if needed 
* | 0e3f3efc build/boot/roms: delete tmpcfg when done 
* | a69e8548 set grub.cfg timeout to 5s (10 on some boards) 
* | 4a459b02 Merge pull request 'pico-serprog improvements' (#128) from Riku_V/lbmk:master into master 
|\| 
| * 7b6fb958 Build pico-serprog binary release archive 
| * c292e01b Build for all pico board, not just the "original" 
| * 1bde6bb3 Support multiple dependencies per project 
| * 4d3b16da Cleaner parent directory creation 
|/  
* 7e8465be grub: re-add module: play 
* e3b9dfc9 util/nvmutil: put code all in nvmutil.c 
* 8fc5f6ed update/blobs/inject: use tmp/romdir, not TMPDIR 
* da991262 build/release/roms: use tmp/romdir, not TMPDIR 
* 15081ed9 grub: make backgrounds configurable in target.cfg 
* 0d315c3a curl/wget downloads: set re-try count to 3 
* bdf171e3 don't use /tmp/ for git clones. use tmp/ instead. 
* 196f293a build/release/roms: fix ucode handling 
* c0c7f3ae build/release/roms: simplify defcongic handling 
* a56cad71 update/blobs: unify global variables 
* 2cbc7eea update/blobs/*: unify checking of defconfig files 
* 52677309 update/blobs/extract: replace errant target code 
* ea7fae97 build/boot/roms: don't create empty bin/ directory 
* c62a4239 update/blobs/inject: remove errant target handling 
* 950166da update/blobs/download: remove errant code 
* 0668d234 add checkversion to build/release/src 
* c92a596c grub: remove xnu module 
* e659ddd8 grub: remove legacy file system modules 
* cf535785 re-add grub modules cat, eval and pbkdf2 
* 33e6088a move script/misc/versioncheck to main directory 
* 2c769dc1 move me7_update_parser.py to util/ 
* da3c9bb3 merge config/ and resources/ 
* a0501050 blobs/download: don't handle ifd/gbe files 
* 03788d14 move ifd/gbe configs into config/ifd/ 
* 6ddb0e09 run make oldconfig on coreboot/default mainboards 
* 19efdf9e ich9m mainboards: use pre-assembled ifd/gbe files 
* af8d8cda add ich9m ifd/gbe files 
* d554efae build/release/src: copy e6430 ifd/gbe 
* 09aae7be build/rpi-pico-serprog: better error handling 
* 1dc54608 fix rpi-pico builds when running it twice 
* c63052cf fix memtest86plus download/build 
*   fb4e6834 Merge pull request 'Add Dell Latitude E6430' (#124) from nic3-14159/lbmk:e6430 into master 
|\  
| * ebc04e52 Add Dell Latitude E6430 
* |   71d361aa Merge pull request 'Less cat abuse' (#123) from Riku_V/lbmk:cat into master 
|\ \  
| * | ef3fb05d Less cat abuse 
* | | eebf7133 switch repo links for pico-serprog 
| |/  
|/|   
* | 9ef8a7ea Merge pull request 'Automate pico-serprog builds' (#122) from Riku_V/lbmk:mkserprog into master 
|\| 
| * e369e8fb automate rpi-pico serprog builds 
|/  
* 92b4db69 build/release/src: only clean kbc1126 if it exists 
* 7c6b35cf unify build/clean scripts: use handle/make instead 
* cec37747 build/release/*: use -T0 in xz, for multithreading 
* b4b63adb don't support ucode removal on untested targets 
* b30c7e33 coreboot/e6400: support nvidia models 
* 436b2ccb handle/make/config -m/-u: actually copy configs 
* 3c7b09ac handle/make/config: properly handle cbutils 
* a3bc7ccd handle/make/file: fix uefitool builds 
* 4885c796 handle TMPDIR from include/export.sh 
* 56f16bc8 don't do cmake on uefitool if the Makefile exists 
* 98d1ea5a build/release/src: bugfix: actually copy cb/ub/sb 
* 755f925a build/release/src: copy handle symlink 
* 3ad29d2d build/release/src: remove Makefile reference 
* d69c231e build/release/src: fix bad variable reference 
* 38440153 update build/release/src based on lbmk changes 
* 0e782e7e update the fetch scripts themselves 
* 98f30b6d build/coreboot/utils: exit 1 if target.cfg missing 
* b9662fbe handle project downloads in main lbmk script 
* 12b33eb8 lbmk script: always clean up /tmp files 
* 225e2609 only remove microcode in build/release/roms 
* bf774acf move build/boot/rom moverom to handle/make/config 
* e5546128 build/release/roms: fix syntax error 
* fbda0f04 re-add /dev/null redirect on . ${1} 
* b2bad5a0 build/release/src: copy the include/ directory 
* eb54e427 grub: all one grub.elf containing keymaps and cfg 
* c6fd4d2a lbmk: run ./build dependencies *before* root check 
* 6722624d build/boot/roms: fix bad variable assignment 
* 55be6dda dependencies/ubuntu2004: update based on debian 
* 0052f9d0 fix: don't require git config for dependencies 
* 6dbddf85 build/boot/roms: simplify ich9m ifd handling 
* f5787c9e build/boot/roms ich9m ifd: use fast dd command 
* d9292cec build/boot/roms: use the new coding style 
* 4623f3f2 Remove superfluous GRUB modules (save CBFS space) 
* 623c3389 fix typo in error message ("as not permitted") 
* 4a280c62 .gitcheck: re-write entirely. force global config. 
* 355eb765 move resources/scripts/ to script/ 
* eed34d3e enable memtest86plus on various boards 
* bc0fb51d x86 desktops: only enable seabios_withgrub 
* 9457d6be unified list command for all scripts 
* 93d2dcad handle/make/config: add missing pipes for err 
* 0e6851c8 delete the Makefile 
* ebbefa60 handle/config/file: rename to handle/make/config 
* df6db1c6 handle/config: fix errant "handle src for" call 
* 6874bc39 "handle src for" - change to handle make file 
* 798ce03a handle/config: add missing error handle 
* 29a8193e build/src/for: rename to handle/make/file 
* 27c67295 handle/config/file: unified distclean handling 
* 197464bc build/src/for: use -j for multithreaded builds 
* 95f290d9 build/release/src: update based on recent changes 
* 5a47c01b scripts: put quotes around file/directory names 
* 1c8401be much, much stricter, more verbose error handling 
* 50c395df .gitcheck: continue if no .git (don't break) 
* be7a5b0c .gitcheck: must stricter error handling 
* 3a5ba57f .gitcheck: only redirect stdout to /dev/null 
* 8f4f0e00 use the new coding style in scripts 
* 4c6c7d10 scripts: never exit 1, always call err instead 
* 52f3fd35 blobs/download: copy dl_path, don't move it 
* 57adbc6e unify err functions across scripts 
* b3fbcdf6 .gitignore: ignore *all* seen files 
*   24f09335 Merge pull request 'hp8300usdt: enable mSATA' (#118) from Riku_V/lbmk:master into master 
|\  
| * df1e8913 hp8300usdt: enable mSATA 
* | dfb93166 Merge pull request 'memtest86+ v6.20' (#116) from Riku_V/lbmk:memtest into master 
|\| 
| * fa926632 memtest86+ v6.20 
|/  
*   1bd84209 Merge pull request 'osbmk->lbmk' (#117) from Riku_V/lbmk:osbmk-lbmk into master 
|\  
| * 7be203dd osbmk->lbmk 
* | 04ee2672 also clean up the main scripts 
* | 62f23123 general code cleanup on lbmk shell scripts 
|/  
* 7be47065 unify build/defconfig and modify/defconfig 
* 0faf2a0c main lbmk script: exit non-zero if argc is wrong 
* 6e92d9a3 fix "./build help" 
* 9031bb7b unify dependencies scripts 
* 023d6b69 unify build/clean into ./build release src 
* f893a29b unify most module build scripts 
* c83d1a8d unify grub scripts under one directory 
* 438bf2c9 grub/modules.list: add argon2 
* fd602532 grub: import phc argon2 implementation (for luks2) 
* 2c0c521e bump grub revision a bit 
* e076d893 unify update/modify coreboot/u-boot/seabios script 
* e25984d7 remove board: qemu_x86_12mb (not usable for now) 
* e5b898f6 consolidate u-boot/seabios/coreboot build scripts 
* 673b144a coreboot/fam15h: fix for gcc/gnat building 
* 63b0e99f don't call blobutil directly from lbmk 
* 08486227 remove download scripts, consolidate into script 
* 8459e33b improve user feedback in blobutil 
* 59dba6cf merge coreboot/u-boot download logic to one script 
* 2453c303 gitclone: always clean up /tmp 
* adeb065c fix permissions on arch dependencies script 
* 6075fed8 NEW BOARD: HP EliteBook 8470p (Intel GPU) 
* f9afeb6f NEW BOARD: Dell Precision T1650 
* f8f77cb2 NEW BOARD: HP EliteBook 2170p 
*   c5c89467 Merge pull request 'Update 'README.md'' (#89) from ewpr5kwu/lbmk:master into master 
|\  
| * 5204f0a9 Update 'README.md' 
* cb8bf380 bump seabios revision to 30 May 2023 
* 27ee975e bump grub revision to 2.12-rc1 
* 705149a3 coreboot/default: bump revision to 2 August 2023 
* 22ee7f74 blobs/download: save ME file to correct location 
* cdd83ab1 blobs/download: try backup if bad hash on main 
* f18b1859 blobs/download: support more formats on ME extract 
* f0efaf79 add unar to dependencies scripts 
* e8ba0f87 blobs/download: declare full user agent 
* 4875eef1 blobs/download: properly handle backup/main url 
* cca93ca3 blobs/download: don't download backup on main 
* 3aeefaa7 blobs/download: set common user agent string 
* 5e83d2bc blobs/download: simplify for loop 
* 8f1d3ad1 scripts: fix indentation in switch/case blocks 
* 748e0972 blobutil/ec: abort if kbc1126 ec extraction fails 
* e594ac16 coreboot/fam15h: remove unused files 
* 44bd077a Revert "build/boot/roms mkUBootRoms: initialise variables" 
* 7c90a407 build/boot/roms mkUBootRoms: initialise variables 
* d918139f coreboot/fam15h: re-enable microcode updates 
* 8c777428 board/qemu_x86: don't enable u-boot 
* fb44c349 coreboot/haswell: fix acpica downloads 
* af084014 coreboot: re-add asus kgpe-d16/kcma-d8/kfsn4-dre 
* e6002b91 coreboot/cros: fix acpica downloads 
* f34e07ae build/boot/roms: fix coreboot-version in releases 
```

This is 445 changes in total, since Libreboot 20230625.

Hardware supported in this release
==================================

All of the following are believed to *boot*, but if you have any issues,
please contact the Libreboot project. They are:

### Servers (AMD, x86)

-   [ASUS KFSN4-DRE motherboard](../docs/hardware/kfsn4-dre.md)
-   [ASUS KGPE-D16 motherboard](../docs/hardware/kgpe-d16.md)

Desktops (AMD, Intel, x86)
-----------------------

-   [Gigabyte GA-G41M-ES2L motherboard](../docs/hardware/ga-g41m-es2l.md)
-   [Acer G43T-AM3](../docs/hardware/acer_g43t-am3.md)
-   [Intel D510MO and D410PT motherboards](../docs/hardware/d510mo.md)
-   [Apple iMac 5,2](../docs/hardware/imac52.md)
-   [ASUS KCMA-D8 motherboard](../docs/hardware/kcma-d8.md)
-   [Dell Precision T1650](../docs/hardware/t1650.md) (**easy to flash without disassembly**)
-   [HP Elite 8200 SFF/MT](../docs/hardware/hp8200sff.md) (HP 6200 Pro Business probably works too)
-   [HP Elite 8300 USDT](../docs/hardware/hp8300usdt.md)

### Laptops (Intel, x86)

-   **[Dell Latitude E6400](../docs/hardware/e6400.md) (easy to flash, no disassembly, similar
    hardware to X200/T400)**
-   [Dell Latitude E6430 (Intel GPU](../docs/hardware/e6430.md) **(easy to flash, no disassembly)**
-   ThinkPad X60 / X60S / X60 Tablet
-   ThinkPad T60 (with Intel GPU)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](../docs/hardware/x200.md)
-   Lenovo ThinkPad X301
-   [Lenovo ThinkPad R400](../docs/hardware/r400.md)
-   [Lenovo ThinkPad T400 / T400S](../docs/hardware/t400.md)
-   [Lenovo ThinkPad T500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad T530 / W530](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad W500](../docs/hardware/t500.md)
-   [Lenovo ThinkPad R500](../docs/hardware/r500.md)
-   [Apple MacBook1,1 and MacBook2,1](../docs/hardware/macbook21.md)
-   [Lenovo ThinkPad T440p](../docs/install/t440p_external.md)
-   [Lenovo Thinkpad X220](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X220t](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad T420](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T420S](../docs/install/ivy_has_common.md)
-   [Lenovo ThinkPad T430](../docs/install/ivy_has_common.md)
-   [Lenovo Thinkpad X230](../docs/install/x230_external.md)
-   [Lenovo Thinkpad X230t](../docs/install/x230_external.md)
-   [Lenovo ThinkPad W541](../docs/install/ivy_has_common.md)
-   [HP EliteBook 2170p](../docs/hardware/hp2170p.md) (**socketed flash IC**)
-   [HP EliteBook 2560p](../docs/hardware/hp2560p.md)
-   [HP EliteBook 2570p](../docs/hardware/hp2570p.md)
-   [HP EliteBook 8470p](../docs/hardware/hp8470p.md)
-   [HP EliteBook Folio 9470m](../docs/hardware/hp9470m.md)

### Laptops (ARM, with U-Boot payload)

-   [ASUS Chromebook Flip C101 (gru-bob)](../docs/install/chromebooks.md)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](../docs/install/chromebooks.md)

Downloads
=========

You can find this release on the downloads page. At the time of this
announcement, some of the rsync mirrors may not have it yet, so please check
another one if your favourite one doesn't have it.

Post-release errata
===================

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
