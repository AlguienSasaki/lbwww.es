% Libreboot 20240126 released!
% Leah Rowe
% 26 January 2024

Introduction
============

*This* new release, Libreboot 20240126, released today 26 January 2024, is
a new *testing* release of Libreboot. The previous release was
Libreboot 20231106, released on 6 November 2023.

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

Testing needed!
===============

The coreboot revision has been updated very recently. Make sure to
have [external flashing gear](../docs/install/spi.md) ready just in case!

Work done since last release
============================

New mainboards supported
------------------------

These new mainboards have been added:

* [HP EliteBook 820 G2](hp820g2.md), added by Leah Rowe to lbmk, using the
  coreboot port done by Iru Cai.
* HP Compaq Elite 8300 CMT, added by Riku Viitanen
* HP EliteBook 8460p, added by Riku Viitanen
* Dell Latitude E6530, added by Nicholas Chin
* ThinkPad X220 eDP variant (distinct from regular X220, this model is created
  by modification, soldering a modboard that permits use of eDP screens
  instead of LVDS, for higher resolution displays). Added by IRC user `risapav`
  who did not give their real name.

Conversely, the [ThinkPad X201 was removed](x201.md).

S3 fixed on GM45 thinkpads
--------------------------

The new coreboot revision includes a fix within it that fixes S3 suspend/resume
on GM45 thinkpads (e.g. X200, T400). This was broken in the previous Libreboot
release, but now it works again.

Modest code size reduction
--------------------------

See: [Libreboot build system audit 4](audit4.md)

These and subsequent changes are included in today's release. The build system
has been further optimised, both in terms of code size and performance.

GRUB 2.12 revision now used
---------------------------

The previous Libreboot release used a revision from GRUB 2.12-rc1, but now
it uses the GRUB 2.12 released during December 2023.

GRUB support for EFI System Partition
-------------------------------------

We don't use UEFI on x86, but the GRUB config in Libreboot's GRUB payload
has now been modified, to also scan `grub.cfg` from `EFI/` directories.
It also now scans directly for extlinux and syslinux configs, in addition
to GRUB configs (GRUB can parse syslinux/extlinux configs). This should
make Libreboot's GRUB payload more compatible with a wider variety
of distro setups.

The above change pertaining to ESP has also been applied to booting of
installers (e.g. USB media). Syslinux/Extlinux/GRUB config scanning has been
merged together there, so now a lot more distro installers should boot
automatically, without manual tweaking/intervention from the user.

Summarised changes since 20231106
-------------------------------

The changes mentioned above are only some of the changes made in this release.

And now, the full list of changes, in order from recent to oldest (newer
changes first):

* Updated `coreboot/default` and `coreboot/dell` to newer coreboot revision
  from January 2024, whereas the previous release used coreboot
  from October 2023. The patches, including board port patches, have been
  updated - notably, the E6430/E6530 and 8300CMT ports have been modified to
  define SPD location in devicetree, rather than `early_init.c` (thanks to
  Nicholas Chin for the warning).
* Re-use crossgcc builds across coreboot trees, when possible, to speed up the
  overall build time when building across multiple trees. This is done
  using the `xtree` and `tree_depend` variables in `target.cfg` files, for
  each coreboot tree and, in the case of xtree, it can be defined in a given
  coreboot board target.
* Removed mainboard: Lenovo X201 (due to thermal safety issues on this machine,
  when using a neutered ME - this issue only affects systems that use ME6,
  whereas older/newer platfroms are unaffected).
* coreboot/fam15h: More fixes, e.g. disable `-Werror` on binutils 2.32, and
  patch GNAT to make it work a bit nicer when built with newer host toolchains.
  These boards were build-tested in mid-January 2024, confirmed to once again
  build on Debian Sid, Gentoo and Arch Linux, up to date as of that time.
* GRUB: Disable `-Werror` when building, to prevent treating warnings as errors.
  This fixes the build on several distros, where CFLAGS is very strict.
* Updated the dependencies config for archlinux packages (added pandoc)
* **NEW MAINBOARD:** HP EliteBook 820 G2 (broadwell platform)
* `build/roms`: general code cleanup and optimisations, for example `grub.elf`
  is no longer re-built if it already exists. Dependency checking is simplified.
* Greatly simplified `include/git.sh` which handles downloading of sources.
* GRUB: bumped the revision again, to the 2.12 release which came out
  on 20 December 2023. We previously bumped this to a November revision,
  mentioned earlier in this changelog, but now we can use the officia/
  GRUB 2.12 release.
* `target.cfg` files are now possible for single-tree projects, not just
  multi-tree projects. For single-tree projects, it goes
  at `config/projectname/target.cfg`, and it goes on the
  existing `config/projectname/tree/target.cfg` location for multi-tree projects.
* `script/update/trees`: Support adding custom arguments to the make command,
  when running a makefile within a project. This is defined by `makeargs` in
  the `target.cfg` file for a given project.
* Generic cmake handling now supported, in `script/update/trees` - it is no
  longer hardcoded, for the `uefitool` package (from which we
  run `uefiextract`). Simply: define `cmakedir` in a project `target.cfg` file.
  It could even be `cmakedir="."` if you wish, so that all of it is built,
  though we specify `cmakedir="UEFIExtract"` for uefitool.
* Unify `script/update/trees` and `script/build/grub` - now, generic autoconf
  handling is present in `script/update/trees`, so it can be used for any
  project, including GRUB, and arguments for configure/autogen/bootstrap scripts
  are configurable in project `target.cfg` files. The actual running
  of `grub-mkstandalone` is now handled from `script/build/roms`. This reduces
  the number of shell scripts from 12 to 11, in lbmk.
* disable u-boot on x86 qemu for now (prevents a build error in lbmk)
* `script/build/serprog`: Return error status (exit) if basename fails, when
  processing various board targets available on stm32/rp2040 projects. Patch
  courtesy of Leah Rowe.
* **NEW BOARD:** HP 8300 CMT mainboard, added by Riku Viitanen, who worked
  on it with a tester in the IRC channel.
* Fixed implicit typecasting bug on flashprog 1.2 source code, thus preventing
  a build issue (tested on Debian 12.2). Patch courtesy of Leah Rowe.
* `script/build/roms`: Rename `check_target` to `configure_target`
  and `prepare_target` to `configure_dependencies`, for increased code clarity.
* `git/pico-serprog`: Use Riku's newer revision, which sets the drive level
  to 12mA by default, up to the previous default of 4mA. The new level is
  within safety specifications on all flash ICs, but will result in greater
  reliability, especially on ISP-based flashing setups. Also merged a fix by
  Riku Viitanen, fixing a build error on boards where `PICO_DEFAULT_LED_PIN` is
  not defined by the pico sdk; in these cases, the status LED is simply unused.
* `script/build/roms`: Improved error handling for ROM image build functions,
  which run inside subshells. They now more reliably cause an exit from lbmk,
  under error conditions, and those exits are guaranteed now to be non-zero. 
  Patch courtesy of Leah Rowe.
* `script/build/roms`: Remove redundant check on cros roms, where it
  checked whether initmode was normal; on these setups, it's always libgfxinit.
* **BOARD:** the `e6400_4mb` target is unaltered, but now a
  new `e6400nivida_4mb` target is defined. This target disabled libgfxinit,
  and only executes the nvidia VGA ROM, so it's only applicable to the nvidia
  variant of Dell Latitude E6400. The `e6400_4mb` variant enables both, but
  causes some issues on nvidia models when `nomodeset` is used - only KMS works,
  but nouveau is unstable in certain conditions and on some linux/mesa versions,
  with this board when Libreboot is used. So it's recommended on nvidia variants
  to use the `e6400nvidia_4mb` option, and use `e6400_4mb` only on intel variants.
  Patch courtesy Leah Rowe, responding to a bug report on the IRC channel.
* set projectname/version strings properly, in scripts that call it. It was
  previously done unconditionally in err.sh, without being called, but this
  was later changed, and scripts that use it weren't adapted, so it turned
  out that the strings were empty when used. Now the strings are properly set,
  in all scripts that need them. These strings are provided by the build system,
  which checks itself at startup. Patch courtesy of Leah Rowe.
* `script/update/trees`: Fix infinite loop caused in some conditions, when
  vendor files are being used. The trees script calls the vendor download
  script, which in turn calls the trees script; an additional check has been
  introduced, to prevent a runaway condition where the two scripts endlessly
  call each other, thus preventing an infinite loop. Patch courtesy Leah Rowe.
* `script/vendor/download`: check whether a config file exists, before trying
  to read it. This reduces the chance of crashing the build system, when running
  the script on certain targets. Patch courtesy of Leah Rowe.
* `script/vendor/inject`: Fixed a bad error check, when running `cd` to switch
  to the ROM images directory, when creating archives. Patch courtesy Leah Rowe.
* `script/update/release`: Don't test `script/vendor/inject` at the end. This
  is regularly tested anyway, during development, so it's a waste of time to
  have it done by the release build script. This reduces the amount of time
  taken to build a release archive. Patch courtesy of Leah Rowe.
* `script/update/release`: Don't insert crossgcc tarballs into release
  archives. These are re-inserted needlessly, when they are already hosted by
  the GNU project and have decent mirrors in general. I always keep backups of
  these anyway, and do several mirrors, and not including them reduces the
  size of the Libreboot release archives. This means that the release archives
  now require an internet connection to use, because crossgcc tarballs will be
  downloaded at build time, but they are the same tarballs that you would
  download as part of a release anyway, but now if you're only building for 
  one coreboot tree within lbmk, you only need to download one set of archives
  instead of getting them all. Patch courtesy of Leah Rowe.
* `script/build/serprog`: general code cleanup, generalising a lot more code,
  especially the if/else chains for checking what type of firmware is build.
  Patch courtesy of Leah Rowe.
* main build script: simplified TMPDIR handling. There were cases where the
  TMPDIR variable was already set, and being re-set needlessly. The new code
  is simpler, and less error-prone. Patch courtesy of Leah Rowe.
* `include/mrc.sh`: general code cleanup, removing dead code and optimising the
  general style of it, to reduce sloccount. Patch courtesy of Leah Rowe.
* Corresponding to the change below by Riku, the vendor filenames were also
  changed to match the 68\* naming scheme.
* `config/vendor/sources`: document HP laptop ROM families, for certain models,
  according to name scheme 68SFC, 68SCE, 68ICE and 68ICF. Some of these boards
  iare part of the same families, and use the same blobs. Patch courtesy of
  Riku Viitanen.
* `script/build/roms`: remove the `modify_coreboot_rom` function. Fake PIKE2008
  ROMs are now inserted by defining option roms in the coreboot config, where
  the given path is `/dev/null`. The *top swap* setting on i945 is now enabled
  via `CONFIG_INTEL_ADD_TOP_SWAP_BOOTBLOCK` in the coreboot build system -
  basically, what this does is cat two identical copies of the bootblock together
  and insert the larger bootblock file (twice the size) at the end of the flash,
  whereas previously we did this in lbmk using dd. The benefit of the new setup
  is that it results in higher build speeds, and now the duplicated bootblock
  now appears in CBFS, so cbfstool will prevent overwriting it.
* `include/mrc.sh`: Also don't use the dedicated unzip logic in the script.
  Re-use the generic logic provided by `include/git.sh` instead. Patch courtesy
  of Leah Rowe.
* `include/mrc.sh`: Don't run the shellball provided by Google. Just directly
  run `unzip` instead. The shellball is just a zip file attached to some shell
  logic, that self-decompresses itself. Running unzip directly, gets you the
  same hash, and it's much simpler in lbmk. Patch courtesy of Leah Rowe, after
  the shellball's behaviour was pointed out by Nicholas Chin.
* `script/update/trees`: Greatly simplified crossgcc handling. Now the list of
  crossgcc targets are directly defined in `target.cfg` files, and the for loop
  in update/trees just goes through it, checking the relevant toolchain. Patch
  courtesy of Leah Rowe.
* `config/ifd/xx30`: Fixed bad component density count for 16MB setups, and
  reduced chip count from 2 to 1. This should make internal flashing work, on
  X230 setups where the user replaced the two flashes with a single 16MB flash.
  Patch courtesy of Nicholas Chin.
* **NEW BOARD:** HP EliteBook 8460p support added. Patch courtesy of Riku
  Viitanen.
* `include/git.sh`: Simplified submodule handling by not using subshells for
  running `cd` to a git repository. Instead, the `-C` option is used in Git.
* main build script, in the `initialise_command` (now `initcmd`) function:
  simplify handling of arguments, when determining what command was passed,
  and how to run it. Patch courtesy Leah Rowe.
* `script/update/release`: remove unnecessary "continue" command at the end of
  a for loop.
* `include/option.sh`: general code cleanup in the `scan_config()` function,
  on the if/else block assigning variables after scanning configs.
* `include/option.sh`: print errors to stdout instead, on the `item()` function
* `include/git.sh`: reduced code indentation, on the part that applies patches
  in a git repository.
* `include/git.sh`: simplify tree name check, by breaking earlier when the
  correct tree name is found on a multi-tree project. Patch courtesy of
  Leah Rowe.
* `grub.cfg`: Support scanning for *extlinux* configs, which are essentially the
  same as syslinux ones. If found, they are passed through GRUB's syslinux
  parser, which then presents a menu as if it were a GRUB configuration. This
  should increase compatibility with distros that use extlinux, such as
  the Alpine Linux distribution.
* `grub.cfg`: Handle GRUB *and* syslinux/extlinux configs, on the USB boot menu
  option. Now it scans for both, thus increasing compatibility with many modern
  Linux distro installers. Before this change, Libreboot's design was made with
  BIOS systems in mind, because we historically only supported systems that were
  BIOS-based, whereas GRUB is more common as a bootloader on UEFI-based install
  media, but in the past we mostly assumed isolinux/syslinux for that.
* `grub.cfg`: support ESP and extlinux setups. The so-called EFI System Partition
  is basically a FAT32 partition that can contain bootloaders and configurations,
  and it is commonly used on some machines that Libreboot supports, prior to
  Libreboot installation. GRUB also supports parsing syslinux configs, and extlinux
  configs are essentially the same. So now Libreboot GRUB automatically scans
  for GRUB *and* syslinux/extlinux configs by default, including on the ESP.
  This will increase compatibility with a wide variety of distros, *without*
  introducing UEFI support yet on x86, because those same Linux kernels can
  also run on bare metal (and this is exactly how it works, when you use GRUB
  as a payload).
* `grub.cfg`: Don't boot linux unless there is a grub.cfg file provided on
  the HDD/SSD. Previously, a fallback entry existed as a last resort, if all
  else failed, but it made several assumptions that are mostly no longer valid
  in 2023. Patch courtesy of Leah Rowe.
* `grub.cfg`: scan LVMs first. This makes it more likely that an encrypted
  system (encrypted `/boot`) will boot first, once found and unlocked. Patch
  courtesy of Leah Rowe.
* `dell-flash-unlock`: in the README, link to several useful guides including
  Open Security Training section about BIOS and SMM internals on x86. Patch
  courtesy of Nicholas Chin. In general, update the README to include instructions
  related to disabling /dev/mem protection, and info about the newer boards
  now added to lbmk, that the utility can be used on.
* `grub.cfg`: Added BTRFS subvolume support, by default. Patch courtesy of
  the contributor, alias `semigel`.
* **NEW BOARD:** EDP variant of the ThinkPad X220, for use with eDP mod kits.
  These kits enable use of much nicer, higher resolution screens. We already
  provided a variant of the X230 for this, in lbmk. Patch courtesy of the
  contributor, alias `risapav` (real name not given).
* lbmk: remove support for the DEBUG environmental variable. It was never very
  useful anyway, and introduced about 10 years ago when the Libreboot build
  system was much less advanced than it is now. We already have much better
  debugging these days, when dealing with build system issues. Patch courtesy
  of Leah Rowe.
* lbmk scripts: Did a general sweep with shellcheck, fixing errors that it
  flagged, such as lack of double quotes in some places, and non-standard
  behaviour being used. The actual [patch](https://browse.libreboot.org/lbmk.git/commit/?id=1eb4df6748f94a08d44c623a56417199b99b371d)
  shows what is meant by this. Patch courtesy of Leah Rowe.
* lbmk scripts: Handle exit status correctly, when dealing with subshells. This
  continues on from the other fix below, after doing a sweep of the entire
  build system. Patch courtesy of Leah Rowe.
* `script/update/trees`: Correctly return error status when `git am` fails,
  while applying patches. Although it was printing an error message, the
  script was making improper use of subshells, leading to a zero exit, when
  it should have exited with non-zero status in such situations. Patch/fix
  courtesy of Leah Rowe.
* Bumped flashprog revision back to version 1.2, because the 1.4 RC we were
  importing has issues (also, Nico Huber's flashprog will probably be used
  in future releases of Libreboot). Patch courtesy of Leah Rowe.
* Debian dependencies config: Fixed the freetype dependency, as tested on
  modern Debian Sid
* GRUB modules: re-added fat/ntfs file system support. Some users on EFI-based
  setups transferring to Libreboot couldn't boot their linux systems, which had
  files installed onto EFI System Partitions. The next release after
  Libreboot 20231106 will once again work correctly on such setups. Patch
  courtesy of Leah Rowe.
* **GRUB revision:** bumped it to November 2023 revision, which contains
  several important fixes. Look at the [patch](https://browse.libreboot.org/lbmk.git/commit/?id=47ef411efb6b669b7befd2a1cf22f999d4521999)
  for actual fixes, listed in the code import. It's a lot.
* main build script: set `--author` in Git, when re-initialising the Git
  history on release archives. Patch courtesy of Leah Rowe.
* lbmk return status: don't rely on return status for unconditional returns.
  There were some parts of lbmk that, in practise, would always exit, but could
  theoretically not, even when they are supposed to, regardless of exit status.
  Patch courtesy of Leah Rowe.
* lbmk help text: support showing the Libreboot version, project name and
  the version date, based on git metadata. Patch courtesy of Leah Rowe.
* Re-added GRUB modules: f2fs, json, read, scsi, sleep, diskfilter, hashsum,
  loadenv, setjump - needed on some setups. Patch courtesy of Leah Rowe.
* `util/nvmutil`: Added useful help dialog, showing usage instructions when
  no/invalid arguments are given. Patch courtesy of Riku Viitanen.
* Fixed the `util/nvmutil` Makefile, courtesy of Riku Viitanen. It was not
  properly formatted to include `nvmutil.c`, when running make-all.
* Dell Latitude E6430: Added VBT to CBFS, which was missing in the
  Libreboot 20231106 release. Also enable DRAM clear on boot. Patch courtesy
  of Nicholas Chin.
* **BOARD ADDED:** Dell Latitude E6530. Courtesy of Nicholas Chin.
* `config/ifd`: Renamed `e6430` to `dell_ivybridge`, because the files can be
  re-used by other planned ports for Dell latitudes in lbmk. Patch courtesy of
  Nicholas Chin.

Exact git log, relative to 20231106:

```
* 7f0ca545 Libreboot 20240126 
* 9071160c git.sh: also reset xtree/tree_depend here 
* 39688ebe update/trees: reset xtree/tree_depend before build 
* e5ebaa85 update config/git/docs to latest revs 
* 0a24b2e6 dell/e6*30: use generic PS2K/PS2M EISAID strings 
* 614c5efa update coreboot/dell to same rev as default 
* 4a6dc555 coreboot/default: update coreboot to January 2024 
* ece54631 script/vendor/inject: remove erroneous check 
* 8b4a4f79 fam15h boards: define xtree 
* dcf7da9a coreboot/fam15h_udimm: define xtree 
* f72a72af don't download projects on release archives 
* 435441d0 update/release: generate changelogs 
* a225e4d5 fix amd mainboard configs 
* c9961182 git.sh: fix bad call to ./update 
* 3e7e0c7d git.sh: support downloading dependency trees 
* 8f3d3ead re-use crossgcc builds on the coreboot trees 
* 8a9c70f2 allow multitree projects to define xgcc tree 
* c6d243af u-boot: don't define xarch in default 
* 9877eb09 coreboot/*/target.cfg: don't define xarch 
* e329b365 grub/target.cfg: move --disable-werror 
* b71d4fd0 coreboot/fam15h: disable -Werror on binutils 2.32 
* 1d971fcd grub: use --disable-werror on ./configure 
* 33e25a33 dependencies/arch: add pandoc to dependencies 
* 37817e6b GRUB: insert only 1 keymap per board, in cbfs 
* df007d22 build/roms: err if -k layout doesn't exist 
* d44c9551 build/roms: regression fix: uninitialised variable 
*   2b6beaf2 Merge pull request 'config/dependencies/trisquel: replaced package from ttf-unifont to fonts-unifont' (#177) from goodspeed/lbmk:master into master 
|\  
| * 59096d8d config/dependencies/trisquel: replaced package from ttf-unifont to fonts-unifont 
|/  
* 09bed9a4 REMOVE MAINBOARD: lenovo x201 
* 401c0882 NEW MAINBOARD: HP EliteBook 820 G2 
*   a8a7a51b Merge pull request 'Dell-flash-unlock README updates' (#175) from nic3-14159/lbmk:dell-flash-unlock-updates into master 
|\  
| * bdd32a5c README.md: Add Latitude E7270 as supported 
| * 051b17f4 README.md: Add notes about iopl and AC adapter requirement 
| * d2de03ce README.md: List E6520, E5530, and M4800 as supported 
|/  
* f5b04fa5 build/roms: tidy up payload configuration handling 
* 0b081218 build/roms: remove unused variable 
* 4870e84e build/roms: don't needlessly re-build grub.elf 
* 0e955f1e build/roms: create elf/grub if non-existent 
* 3b66a5bb git.cfg: simplified revision checking 
* a7f58abb fix oversight in previous commit 
* 2d7e7306 build/roms: rename more functions for clarity 
* 62a5f543 build/roms: rename payload functions for clarity 
* 042c7877 build/roms: simplify seabios dependency check 
* fcf2b2bb build/roms: simplify grub dependency check 
* 535c9007 add copyright 2024 leah rowe to edited files 
* cea88fa8 git.sh: simplify submodule handling in git_prep 
* 1fcbadb8 git.sh: further simplify git_prep 
* 48551ced git.sh: unify am/submodule and tree copying 
* ce67c99f git.sh cleanup: git am handling (remove patchfail) 
* e6953dc4 git.sh: clean up handling of tmp_git_dir 
* d819403a git.sh: fix regression: patches before submodulse 
* f4a14bd9 git.sh: clean up git submodule/am handling 
* 11a82163 Bump GRUB to 2.12 release
* f3098f56 git.sh multi-tree: grab submodules *after* patches 
* 5fb6e36f update/trees: clean up the coreboot-version check 
* 4c9ee172 update/trees: support custom make/autogen argument 
* c6a0e495 update/trees: generic cmake handling 
* 30337b8f update/trees: avoid namespace clash in function 
* b0615581 update/trees: dont hardcode autoconf/bootstrap arg 
* eb3a8e2b unify script/update/trees and script/build/grub 
* 34ded35f lbmk scripts: general code cleanup 
* 4e067799 disable u-boot on x86 qemu 
* bc87b5f6 lbmk scripts: general code cleanup 
* 0c1d08d8 build/serprog: err if basename fails 
* eff9130b update/trees: further simplify crossgcc handling 
* 6752780f coreboot: update hp elitebook configs 
*   15298985 Merge pull request 'Add HP 8300 CMT port' (#173) from Riku_V/lbmk:hp8300cmt into master 
|\  
| * 74147ea4 Add HP 8300 CMT port 
|/  
* 0aca6332 lbmk scripts: shorter code lines 
* 575332f2 fix flashprog build error (implicit enum typecast) 
* b4ab3057 lbmk scripts: general code cleanup 
* 38a7aa31 build/roms: rename two functions for clarity 
* 746d9cad build: remove test command 
* 655d3cdc lbmk scripts: general code cleanup/optimisation 
* 25f9d948 git/pico-serprog: update revision again 
* e0fee7a4 git/pico-serprog: update revision 
* a48b3841 build/roms: improved error handling for roms 
* 33695a56 build/roms: remove redundant check 
* d5f8f657 dell/e6400nvidia_4mb: new configuration (nvidia) 
* 9d5d98eb set version/projectname properly 
* aa525142 update/trees: fix infinite loop 
* 465077bc vendor/download: check whether configs exist first 
* 39293279 vendor/inject: fix dodgy error check (cd command) 
* f44b99c8 don't delete microcode updates in rom images 
* 72cd169e update/release: don't test ./vendor inject 
* e8eb52f8 update/release: don't insert crossgcc tarballs 
* b0e5fc9d lbmk scripts: general code cleanup 
* b111f484 build/serprog: general code cleanup 
* 2f98ca6d build: simplified TMPDIR handling 
* ab65ea4c general code cleanup 
* fa25414b mrc.sh: run debugfs from extract_partition 
* 8a875722 mrc.sh: general code cleanup 
*   12e644b3 Merge pull request 'hp-ec-fw' (#172) from Riku_V/lbmk:hp-ec-fw into master 
|\  
| * 584c66e9 rename hp elitebook ec fw after rom families 
| * f7fda791 document hp laptop rom families 
|/  
* 92986f0c build/roms: remove modify_coreboot_rom() 
* 493ebdfb mrc.sh: remove redundant extraction logic 
* 1153bc3b mrc.sh: don't run the shellball. use unzip. 
*   28dfd91a Merge pull request 'hp8460pintel: actually enable vbt' (#171) from Riku_V/lbmk:vbt8460p into master 
|\  
| * aa4160e3 hp8460pintel: actually enable vbt 
|/  
* 3ccf1941 update coreboot configs 
* 95788059 update/trees crossgcc: call err if arch isn't set 
* 90ac30b1 update/trees: simplified crossgcc handling 
*   4711098e Merge pull request 'config/ifd/xx30: Fix 16_ifd component density and count' (#170) from nic3-14159/lbmk:xx30_16_ifd_fix into master 
|\  
| * dbec5bf3 config/ifd/xx30: Fix 16_ifd component density and count 
* |   4fb48595 Merge pull request 'Add HP EliteBook 8460p' (#169) from Riku_V/lbmk:hp8460p into master 
|\ \  
| |/  
|/|   
| * b0b4f86b Add HP EliteBook 8460p 
* | 7f98ab8e git.sh: simplify submodule handling 
* | 124b5beb build initialise_command: simplify handling 
* | 9c00746b update/release: minor cleanup 
* | f6ebab57 option.sh scan_config: clean up if/else block 
* | 3b7009aa option.sh: print error on stderr, not stdout 
* | c75ca20c option.sh: don't rely on zero status on printf 
* | 578f105d git.sh git_am_patches: reduce indentation 
* | cbd19d81 git.sh fetch_config: simplify tree name check 
|/  
* b9f69f26 grub.cfg syslinux: support scanning /boot/EFI/ 
* 766bb46c grub.cfg: fix path 
* 430918ee grub.cfg: handle btrfs subvols for extlinux.conf 
* d74c6c71 grub.cfg: scan extlinux/extlinux.conf 
* f1d6c143 grub.cfg: support grub and extlinux on ata/ahci 
* 6db94c1a grub.cfg: merge isolinux/grub usb menuentries 
* c4544e04 grub.cfg: handle extlinux in the default menuentry 
* eaa1341b grub.cfg syslinux: support ESP and extlinux.conf 
* b817001e grub.cfg: don't boot linux without a grub.cfg 
* 2d6e5ca4 grub.cfg: scan lvm volumes last 
* 49eed9ac Revert "grub.cfg: try luks2/crypto-lvm before non-crypto" 
*   aed4dff8 Merge pull request 'Dell-flash-unlock README updates from upstream' (#168) from nic3-14159/lbmk:dell-flash-unlock-updates into master 
|\  
| * d207e9bc README.md: Add possibly not working systems 
| * ab59f912 README.md: Add E6500, E6420, and E6530 as supported 
| * ca28255d README.md: Add instructions for relaxing memory permissions 
| * f4819081 README.md: Add references to Open Security Training 
* | 20389655 grub.cfg: try luks2/crypto-lvm before non-crypto 
* |   3a36c827 Merge pull request 'master' (#165) from risapav/lbmk:master into master 
|\ \  
| * | 15226f9e added x220edp_8mb 
| * | 1c337ac7 added x220edp_8mb 
| * | e6629606 added x220edp_8mb 
| |/  
* |   0a8ef113 Merge pull request 'Update config/grub/config/grub.cfg' (#167) from semigel/lbmk:semigel-btrfs-subvol-patch-1 into master 
|\ \  
| |/  
|/|   
| * ababbc09 Update config/grub/config/grub.cfg 
|/  
* 39a3de57 remove DEBUG handling in lbmk (not needed) 
* 1eb4df67 fix several shellcheck warnings 
*   54ca5f24 Merge pull request 'config/dependencies/debian: add unifont-bin & xfonts-unifont' (#163) from Riku_V/lbmk:debgrub into master 
|\  
| * 2e6073f2 config/dependencies/debian: add unifont-bin & xfonts-unifont 
* | 2e779a54 handle errors on exits from subshells 
* | 9558e2fc improved safety/error handling on multitree git-am 
* | 7af200a1 Merge pull request 'fix void dependencies: openssl-devel' (#161) from Riku_V/lbmk:fixvoid into master 
|\| 
| * 6d8d2e75 fix void dependencies: openssl-devel 
|/  
*   cb3fad07 Merge pull request 'fix void dependencies: freetype-devel' (#160) from Riku_V/lbmk:fixvoid into master 
|\  
| * 01a82431 fix void dependencies: freetype-devel 
|/  
*   5a6dec97 Merge pull request 'fix void dependencies typo' (#159) from Riku_V/lbmk:fivoid into master 
|\  
| * 50bfe1ca fix void dependencies typo 
|/  
* 4e00ac00 revert flashprog back to version 1.2 
* 83c8248f dependencies/debian: fix libfreetype-dev 
* 39aad578 grub: re-add fat/nt file system modules 
* 47ef411e Bump GRUB revision to 8 November 2023 revision 
* ce1176f5 fix typo in help text 
* 70882902 build: set --author when running git init 
* 5af3ae05 lbmk: don't use status for unconditional returns 
* 64f93374 lbmk: support showing the revision in help text 
* f4b2a588 build: don't generate version/versiondate as root 
* c4d90087 add grub mods: diskfilter,hashsum,loadenv,setjmp 
* d0d6decb re-add grub modules: f2fs, json, read, scsi, sleep 
*   f60286a3 Merge pull request 'Dell Latitude E6530 support' (#151) from nic3-14159/lbmk:e6530 into master 
|\  
| * 36d4c906 Add Dell Latitude E6530 support 
| * 8bb95639 config/ifd: Rename e6430 to dell_ivybridge 
*   4d7cb99c Merge pull request 'config/coreboot/e6430_12mb: Fix configs' (#150) from nic3-14159/lbmk:e6430-config-fixes into master 
|\  
| * a02c7e31 config/coreboot/e6430_12mb: Fix configs 
* d3ade208 Merge pull request 'nvmhelp' (#152) from Riku_V/lbmk:nvmhelp into master 
* 86608721 nvmutil: print usage 
* f12f5c3a nvmutil: fix makefile 
```

Errata
======

Vendorfile inject script will report an error when inserting for HP 820 G2,
because the refcode is compressed via `-c lzma` option in cbfstool (or it is
compressed by the coreboot build system, if building from lbmk directly). The
way this compression is handled produces a result that is non-deterministic.
Prior to scrubbing of vendorfiles (when a release is build), ROMs are hashed
so that they can be checked during insertion post-release.

If you're in any doubt whatsoever, simply build this ROM [from
source](../docs/build/), using either the (source code) release archive,
or from `lbmk.git`.

The 820 G2 ROMs have been excluded in rsync, so you must build this from source.You can build it from source via the source code tarball in the release, or
from `lbmk.git`.
