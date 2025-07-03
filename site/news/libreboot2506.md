% Libreboot 25.06 "Luminous Lemon" released!
% Leah Rowe
% 30 June 2025

A corresponding [Canoeboot 25.06](https://canoeboot.org/news/canoeboot2506.html)
release is also available.

There *was* a Libreboot 25.04 release in April 2025, but that is retroactively
regarded as an RC of 25.06. The original 25.06 release announcement showed
changes since 25.04, but the changelog is now relative to December 2024.
This reflects the [revised release schedule](revisions.md). It means that the
changelog is much bigger, and also includes the changes that went in
Libreboot 25.04.

Today's Libreboot 25.06 revision is a *stable release*, whereas the previous
stable release was Libreboot 20241206. This revised release log lists all
changes as of today, 30 June 2025, since the Libreboot 20241206 release of
December 2024.

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

Open source BIOS/UEFI firmware
----------------------------

Libreboot is a free/open source BIOS/UEFI replacement on x86 and ARM, providing
boot firmware that initialises the hardware in your computer, to then load an
operating system (e.g. Linux/BSD). It is specifically
a *[coreboot distribution](../docs/maintain/)*,
in the same way that Debian is a Linux distribution. It provides an automated
build system to produce coreboot ROM images with a variety of payloads such as
GRUB or SeaBIOS, with regular well-tested releases to make coreboot as easy
to use as possible for non-technical users. From a project management perspective,
this works in *exactly* the same way as a Linux distro, providing a source-based
package manager (called lbmk) which patches sources and compiles coreboot images.
It makes use of [coreboot](https://www.coreboot.org/) for hardware initialisation,
and then a payload such as [SeaBIOS](https://www.seabios.org/SeaBIOS)
or GRUB to boot your operating
system; on ARM(chromebooks) and certain x86 mainboards, we provide *U-Boot* (as
a coreboot payload), which provides a lightweight UEFI implementation..

Sumarised list of changes
-------------------------

This section provides a brief overview, summarising all of the changes. The
next sections (after this) show *all* changes in detail.

The most important changes are, thus:

New mainboards:

* Acer Q45T-AM support added (similar to G43T-AM3 mainboard)
* Dell Precision T1700 SFF and MT

Revision updates:

* GRUB, SeaBIOS, Untitled, flashprog, U-Boot, uefitool have all been updated
  to newer revisions, from ~April 2025.

Security:

* GRUB has has *a lot* of security fixes applied to it from upstream, including
  a very large series of *73* major security fixes, and a few minor tweaks
  after the fact.
* Globbing issues fixed in the Libreboot build system, lbmk. Better error
  handling in general.
* ThinkPad T480/3050micro: Disable hyperthreading by default

Feature changes:

* Better, more reliable caching of Git repositories and files during download.
  Re-builds of sources make better use of local caching, instead of downloading
  from scratch every time (e.g. coreboot and GRUB sources).
* Handling of vendor files is more reliable, caching everything more aggressively
  and even verifying checksums of *extracted* files, from inside update archives.
* Non-root USB hub support added to GRUB, for xHCI devices
* GRUB: better LVM scanning, for auto-boot especially with encrypted `/boot/`;
  Linux distros are easier to handle, in general.
* Safer handling of vendor files; release images padded to prevent flashing,
  where such files are needed, until they have been inserted.
* Better MAC address handling, on IFD-based systems. Insertion of MAC addresses
  is done by default, randomised by default, unless overridden by the user.

Config changes:

* Removed unnecessary sources by default, to make source archives smaller. Only
  the sources needed to build the binaries are included, in many cases.
* Updated various dependencies configs, for installing build dependencies in
  various Linux distros (for the `./build dependencies` command which installs
  them in your distro).

Bug fixes:

* Better checksum verification for project files, when deciding whether to
  re-build a given upstream source.
* General build system fixes, making the build process more reliable, with
  much stricter error handling (and some false error conditions have also been
  removed).
* GRUB payload: Mark E820 reserved for cbmem, which means that you no longer
  need `iomem=relaxed` (kernel option) at boot time, to access the cbmem console.
* Use `x` instead of `?` in nvmutil, as a character for randomness in MAC
  addresses, to work around a design quirk in ZSH.
* Where files are operated on post-build, e.g. coreboot images, more strictly
  operate on them first, erroring out more reliably when a fault occurs; prevent
  bad files from being copied to final build destinations. This reduces the
  chance of bad/corrupt build artifacts being present in release builds.
* HP EliteBook 820 G2 images now included in releases, because handling of
  the refcode files was corrected so that checksum verification passes during
  insertion.

This, and more, has all been done. There was also a general focus on heavily
auditing the build system, lbmk, so as to clean up the code. The amount of
overall code in lbmk was *reduced*, without removing functionality.

These next sections will repeat many of the above items, but in more detail.

Detailed list of changes
-------------------------

### Board support

The priority for the first half of 2025 has been on further auditing the
Libreboot build system, so fewer board ports were added. More board ports
will be added instead in the December 2025 release (a lot more).

The following boards have been added since the Libreboot 20241206 release:

* Acer Q45T-AM support added (similar to G43T-AM3 mainboard)
* Dell Precision T1700 SFF and MT

Board ports were low priority for this release; now it shall be the focus,
between June 2025 and October 2025, ready for the 25.12 release cycle leading
into December 2025.

Dell Precision T1700 is essentially the OptiPlex 9020 but with a slightly
different, code-compatible PCH that also supports ECC memory features when an
Intel Xeon processor is installed.

### Revision updates

In descending order from latest changes to earliest changes:

* GRUB: Update to revision 73d1c959e (14 March 2025)
* Bump SeaBIOS to to rev 9029a010, 4 March 2025
* Updated Untitled to newer LBSSG repository.
* Bump flashprog to rev e060018 (1 March 2025)
* Bump U-Boot on ARM64 boards to U-Boot v2025.04. Patching courtesy of
  Alper Nebi Yasak.
* Bump uefitool to rev a072527, 26 Apr 2025 to fix CMake compatibility issue
  since CMake 4 no longer supports version 3.5, whereas the old uefitool had
  an earlier version as the minimum supported. This fixed a minor build error.
* Merged coreboot/next with coreboot/default
* Bump coreboot/next to rev c247f62749b as of 20 April 2025
* Bump coreboot/default to rev c247f62749b as of 20 April 2025
* Bump flashprog to revision eb2c041 (14 Nov 2024).

The GRUB revision includes a number of critical CVE fixes, and regression
fixes, that were also included in Libreboot 20241206 rev11. Some later fixes
are also present, such as wiping LUKS keys from memory after successfully
booting Linux (Linux handles LUKS itself, and starts the process again).

The NASM version was updated to version 2.16.03 on coreboot/fam15h, to prevent
build errors, instead of fixing the old NASM 2.14.02. Tested on Debian Sid
Experimental, with GCC15-based toolchain, and on Fedora 42.

PICO support: Reverted to the old pico serprog/sdk repositories used
in Libreboot 20240612. This is temporary, because pico2 support is currently
broken, so this release only has pico1 support, when dealing with Rpi Pico
devices. Upstream pico-serprog works fine on pico2, so this will be fixed
in and re-updated again in a future revision release. The pico2 update images
were retroactively removed from the 20241206 release on rsync.

A patch from upstream was backported to the old pico-sdk version, so that it
builds correctly on newer GCC15 (tested on Debian Sid with "Experimental"
packages enabled).

### Licensing

<img tabindex=1 class="r" src="https://av.libreboot.org/hp8460p/hp8460p.webp" /><span class="f"><img src="https://av.libreboot.org/hp8460p/hp8460p.webp" /></span>

* Added SPDX license headers to almost every configuration file in lbmk.

### Security

These can be considered bug fixes, but these are special fixes that are of
massive concern to users.

* This GRUB change was merged, in the aforementioned revision
  update: `dbc0eb5bd disk/cryptodisk: Wipe the passphrase from memory` - this
  wipes the LUKS key from memory, after GRUB exits, where one was created
  by GRUB while unlocking a given volume.
* Merged *73* critical CVE fixes into the GNU GRUB source code, from upstream.
* Stricter use of pledge and unveil in the nvmutil source code.
* *Much* safer `./mk inject`. It used to be that the tarballs were extracted
  and files inserted into the extracted images, but the tarballs were left
  unmodified; many users thought then that they should extract the tarball
  and flash that, which lead to bricks. And it was easy to flash uninjected
  images, where files (e.g. Intel ME) are needed, so now ROM images are padded
  by one byte, to prevent flashing, and the user is strongly reminded to
  inject files first; upon running the `./mk inject` commands, these images
  are then safe to flash.
* Fix globbing issues in lbmk by double-quoting variables everywhere, and
  generally making sure that certain characters are escaped properly when
  necessary. To reduce the chance of bad commands being run by mistake or
  intentionally.
* Removed auto-confirm on `./mk dependencies` commands, to mitigate the risk
  of a buggy package manager on the user's distro possibly removing many
  packages. Now the user must confirm their choice, e.g. when a conflict
  occurs, instead of the package manager already deciding for the user.
* ThinkPad T480 / OptiPlex 3050: Disable HyperThreading/SMT by default, for
  security, to reduce the attack vector of certain speculative execution-based
  exploits.

### Feature changes

<img tabindex=1 class="r" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

In descending order from latest changes to earliest changes:

* `init.sh`: looser `XBMK_THREADS` validation; correct it on child instances,
  if it's not set, or set incorrectly.
* `get.sh`: use subshells on `try_` functions, wrapped in an error handler
  so as to provide more verbose output under fault conditions. This makes it
  easier to debug when a download fails.
* `git.sh`: Re-implement redundant git downloads, more reliably than before; all
  repositories are now cached, reliably, including submodules, even when upstream
  repo links differ wildly. This reduces the amount of internet bandwidth used,
  when handling multiple builds.
* `release.sh`: build in tmp directory first, leaving old files behind under
  fault conditions, for further analysis
* `inject.sh`: re-add mac address confirmation, for user-friendliness, when
  running the inject commands.
* `init.sh`: Resolve `XBMK_CACHE` via readlink
* `init.sh`: Use `readlink` in `pybin()`, with realpath only as fallback. This
  makes the function more redundant, working on more systems by default.
* `lib.sh`: support any command on `find_exec()` (later renamed); this is a
  generic function, that implements a while loop for a given set of files,
  based on the output a command that generates those paths. This is operated
  on by a function, defined when calling find\_exec. This unifies all use of
  while loops on lists of files and directories, throughout xbmk, rather
  than re-implementing the for/while loops each time.
* `inject.sh`: simplify kconfig scanning by using the `fe_` with a new
  function, `scankconfig()`. This new function checks *all* coreboot configs
  for a given target, whereas the old behaviour only resulted in the *first*
  config being checked. In practise, this causes no real behaviour changes.
* `rom.sh`: Print the rom image path being generated
* `lib.sh`: Add warning if x_ is called without args
* `init.sh`: More verbose error info, on non-zero exits.
* `util/nvmutil`: Within each 4KB part, only handle 4KB, even if the block
  size is bigger. This means using less memory, and modification of anything
  past 4KB is not required.
* `util/nvmutil`: Support 16KB and 128KB GbE files, in addition to the
  usual 8KB files. The size is based on the block size of the flash you use.
* Added non-root USB3 hub support to GRUB on the xHCI implementation, courtesy
  of a patch from Nitrokey.
* GRUB: Scan LUKS inside *every* LVM, to support the uncommon use case where
  LUKS is inside LVM, instead of LVM inside LUKS. It is theoretically possible,
  even if ill advised.
* GRUB: Scan *every* LVM device, where available, as a fallback at boot time
  when all else fails.
* Release ROMs prefixed with a "DO NOT FLASH" warning and padded by one byte,
  where vendor files are required. The `./mk inject` commands remove this
  prefix/padding, after vendor files are inserted and checksums verified.
* Better detecting of whether vendor files are needed, and confirmation to
  the user while running `./mk inject` commands.
* Allow restoring the default MAC address on `./mk inject` commands, by using
  the `setmac restore` arguments.
* Randomise the MAC address by default, where applicable, when running
  the `./mk inject` commands, because lots of users previously flashed without
  changing it, so lots of users had generic MAC addresses.
  The `setmac keep` argument prevents this from happening, where desired.
* `include/vendor.sh`: More user-friendly debug messages, for the user to
  know what's going on.
* `util/nvmutil`: Add uninstall command to the Makefile
* `util/nvmutil`: Add distclean command to the Makefile
* `util/nvmutil`: Nicer hexdump display, similar to `hexdump -C`.
* Support a `re` argument in `./mk dependencies` Fedora commands, for
  re-installation of packages as desired.
* Support `--reinstall` in the `./mk dependencies debian` command, when the
  user wants to re-install dependencies.
* Put temporary `PATH` directory in the normal `XBMK_CACHE` directory, and
  clear it whenever a new parent instance of the build system is executed.
  This is used for the GCC/GNAT matched symlinks, for example, or the python
  symlink created at startup.
* Pico 2 support briefly added, but was a bit buggy for now, so it's removed
  in this release, and was retroactively removed in rsync for the
  Libreboot 20241206 release; this will be re-added in a future release.
* Added GRUB-first payload setups as an option, but not enabled by default.
  The user can add `payload_grubsea="y"` in the `target.cfg` file for a given
  mainboard.
* Support automatically downloading Lenovo ThunderBolt firmware for the
  ThinkPad T480, automatically padding it for installation. This update fixes
  a charging bug that affected some earlier launch models.
* Insert GRUB backgrounds in CBFS instead of GRUB memdisk, which makes
  GRUB background images easier to replace.

### Configuration changes

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi_pico/soic16_x200.webp" /><span class="f"><img src="https://av.libreboot.org/rpi_pico/soic16_x200.webp" /></span>

In descending order from the latest changes to the earliest changes:

* ifd/hp8300usdt: set the HAP bit by default; it was previously not set, but
  the `me_cleaner` config was nonetheless used, and ME Soft Temporary Disable
  was also used. As a result, this change is basically redundant, but otherwise
  technically correct (more so than the previous behaviour).
* coreboot: Remove unused vboot tests (futility tests), to shrink the size of
  release tarballs.
* coreboot/default: Remove unneeded FSP modules when downloading, because
  only the Kabylake version is needed at this time. This is done, using
  the `nuke()` function via `nuke.list` files. This shrinks the size of
  release tarballs.
* `dependencies/debian`: add `libx86`
* HP 820 G2: Use fam15h cbfstool tree for refcode; this avoids the need to
  clutter the source code with an entire additional coreboot tree, thus reducing
  the size of releases.
* A GRUB configuration change was made, fixing auto-scanning of LVMs when
  doing cryptomount.
* T480/3050micro: Removed the `fsp` targets, because we only need
  the `vfsp` targets.
* Added `gnutls-devel` to Fedora 41 dependencies.
* Added `libx86` to Arch dependencies, needed for the `int` utility.
* Added `less` to Arch dependencies, because it's needed for certain commands
  e.g. git commands.
* GRUB: Use the codeberg mirror first, to mitigate GNU mirrors often being
  slow or rate limited, e.g. for gnulib downloads.
* fedora41/dependencies: add libuuid-devel
* Added `uuid-devel` to fedora41 dependencies
* flashprog: Disable `-Werror` to prevent minor warnings being treated as errors.

### Bug fixes

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi/wson8/0003.jpg" /><span class="f"><img src="https://av.libreboot.org/rpi/wson8/0003.jpg" /></span>

This combines both build system fixes, and changes to upstream sources (e.g.
coreboot and various payloads like SeaBIOS/GRUB, utilities like flashprog, and
so on).

The following bug fixes have been merged (in descending order from the latest
changes to the earliest changes):

* `tree.sh`: add sha512 error for `check_project_hashes()`. Handle errors
  in `sha512sum` and `awk`; also check that `project.hash` exists and error
  out if it doesn't, when checking a given project hash. We know that the
  project hash file should always exist, and always be read; technically,
  find might not yield results, but then an empty file would be produced.
  the empty file edge-case scenario would already have resulted in an error
  exit inside `configure_project()`, so that's already covered.
* `tree.sh`: add error checking in `check_project_hashes()`, when reading
  the `old_pkhash` variable; we need to error out where a read error occurs.
  such an error is extremely unlikely, so this fix is largely theoretical and
  preventative.
* `tree.sh`: more reliable clean in `run_make_command()`; don't do a no-op if
  it fails, instead fall back to the `clean` method, and throw an error
  if *that* fails. The no-op existed because not all projects have distclean,
  but we always intend for them to be cleaned. This therefore prevents
  further unhandled error conditions, in such edge cases.
* put coreboot utils in `elf/coreboot/TREE`, to prevent old binaries from
  still being used when a code change is made.
* `release.sh`: use printf to create version files, instead of copying the
  version files, because they don't exist in some cases, so this prevents
  an error condition.
* `init.sh`: error out if .git/ is a symlink; this is a preventative bug fix,
  to prevent future unknown bugs in such a scenario.
* `get.sh`: Properly error out if `tmpclone()` fails, where it previously
  failed to throw an error under certain fault conditions.
* `tree.sh`: Don't auto-run make-oldconfig; it now must be applied permanently,
  via e.g. `./mk -u` commands. Otherwise, undesirable changes can sometimes
  be made during build time, especially on projects that don't use scons
  quite as reliably, as in the U-Boot build system.
* `get.sh`: re-generate remotes every time, on cached Git repositories, so
  that configuration changes in `config/git/` are automatically applied when
  dealing with multiple versions of a given upstream project.
* `release.sh`: copy version files to `rsrc` (release source directory),
  otherwise an `unknown` version number is erroneously created. This fixes
  a regression caused by previous optimisation to `init.sh`
* xbmk: add fake config makefile args to `flashprog`, and `pcsx-redux`, to
  prevent `./mk -u` (without additional arguments) from erroneously exiting
  with error status. otherwise, an error can occur in such conditions if
  a Makefile has not yet been created.
* `rom.sh`: skip running `copyps1bios()` on dry builds, otherwise
  running `./mk -d` without argument will cause an error.
* `tree.sh`: Don't run make-clean on dry runs (`./mk -d`), to prevent error
  conditions while building GRUB, if `./mk -d` is passed without additional
  argument, since the latter requiires running autoconf to get a Makefile in
  the GRUB build system.
* `get.sh`: add missing check in `fetch_project()`; we were checking the
  main URL on a download, but not the backup URL.
* `get.sh`: stricter URL check in `xbmkget()`; throw an error if a URL is
  empty, rather than skipping to the next. If a URL is set but fails, then
  falling back to the next is OK (or throw an error if the backup is set,
  and also failed).
* `get.sh`: Make `xbmkget` always throw an error upon exiting the loop check;
  it was previously throwing an error if the for loop returned with zero status.
  Depending on the sh implementation, or changes made in the future, this could
  cause unpredictable buggy behaviour. Therefore, the error exit is much
  stricter now, and less ambiguous, to prevent future bugs, because it is
  imperative that execution must never continue under fault conditions. If a
  file or repository is successfully handled, a return (zero) occurs, otherwise
  the loop exits and a non-zero exit occurs.
* `tree.sh`: fix up `copy_elf()`, or specifically fix a bad `for` loop,
  because shorthand conditionals are used and the way they were used can be
  buggy on some sh implementations, so they are terminated more explicitly.
* xbmk: stricter handling of files on while loops, to prevent instances where
  execution continues under fault conditions. This prevents other, less
  predictable bugs in the future.
* `init.sh`: Hardcode `XBMK_CACHE` for integrity; this is a bug fix, because
  there's too much that can be wrong with this being configurable, so now it
  is hardcoded at runtime. It was never intended to be configurable anyway.
* `init.sh`: check/validate version/versiondate once read, in child instances
  of xbmk, to further verify that they were previously set, and set correctly.
  This is theoretically a preventative bug fix.
* `init.sh`: force an error condition if the xbmk version was not read. This
  prevents further erroneous state within xbmk.
* `init.sh`: check the `lock` file BEFORE `git init`, to prevent erroneous
  initialisation while another xbmk parent instance is running.
* `init.sh`: return from xbmk child instances in `set_env()` instead. This is
  easier than the previous check, preventing the initialisation of a git repo
  and/or recreation of xbmktmp and xbmklocal by erroneoues parent executions
  of xbmk while another parent is running - the latter of which could have
  caused a massively unpredictable build failure, so this is also a
  preemptive bug fix, fixing and preventing all kinds of weird unknown bugs.
* `vendor.sh`: Remove `_dest` if it's bad; this complements a bug fix, in the
  bug fix section above, that caches the extracted files and hashes them. On a
  subsequent run where the given file is needed, it is *re-checked* if the
  final file exists. This mitigates the possibility that corruption may have
  occured, under unhandled fault conditions. Therefore, this is a preventative
  bug fix.
* `release.sh`: don't move `src/docs/` to `docs/` inside release archives,
  because otherwise `./mk -b` will fail, inside release archives.
* `vendor.sh`: Properly verify SHA512SUM on extraction. This is performed on
  the actual extracted files, alongside the existing check on downloaded files.
  This mitigates against future fault conditions in the extraction process,
  thus fixing a major design flaw. *This change also caches those files, thus
  speeding up extractions when they're done multiple times.*
* `get.sh` submodules: Don't delete files recursively. Use `rm -R` instead
  of `rm -Rf`, on files.
* `git.sh`: Only create destination repo on success; don't leave a broken cache
  laying around, which would otherwise break the build system under certain
  conditions.
* `release.sh`: removed an unnecessary `src_dirname` variable
* `release.sh`: delete tmp/cache from release tarballs
* `inject.sh`: Remove confusing path on tar creation; that is, don't print said
  path, because temporary paths are printed during this, when creating tarballs.
  In this file, the correct path is printed at the end of the process, when
  handling an images tarball.
* `tree.sh`: only create elfdir in `copy_elf()`, to prevent empty directories
  being created where a project provides `build.list`, but where no actual
  configs are being built on a given target name.
* `mrc.sh`: operate on refcode in tmp area first, to prevent bad files from
  being saved to the final destination under fault conditions. This pertains
  to the change made at build time that enables GbE devices from the refcode.
* `inject.sh`: use subshell to speed up `find_me()` (this is a bug fix, because
  slowness is a bug)
* `mk`: add missing error handli for `mk -f` (when doing releases)
* `git.sh`: hard fail if git am fails (regression fix)
* `git.sh`: Hard fail if reset fails; allowing re-try when cloning fails, but
  the reset-fail scenario didn't cause any exit at all. This is fixed now.
* `init.sh`: Only check `XBMK_CACHE` if it exists
* `grub.cfg`: fix trying to boot all logical volumes after unlocking an encrypted
  volume; this makes booting LVMs more reliable, on encrypted boot setups.
* `init.sh`: also allow `XBMK_RELEASE=Y` or `=N`, not just `n` and `y`,
  because some people use uppercase here. This is considered a bug fix, but
  could just as easily have been in the features section.
* `init.sh`: check `XBMK_CACHE` is a directory instead of a file.
* `init.sh`: run `set_version` *before* `set_env`, to prevent a future situation
  where the version is not set correctly. In general, the version should always
  be set as early as poessible when running xbmk.
* `inject.sh`: clean up tmp me file before extract; this is a preventative fix,
  to ensure that cross-flashing does not occur.
* `lib.sh`: re-add missing break in fe/fx\_, that caused improper exits (or non
  exits) in some cases.
* `singletree/elfcheck`: use `fx_`, not `fe_`; this is a less strict test, to
  prevent certain errors under specific edge-case conditions.
* `rom.sh`: Safer `cprom()`; don't insert special files like GRUB keymaps AFTER
  copying the system ROM to the final destination; do it BEFORE, instead, to
  ensure that bad images aren't left in place under fault conditions.
* `rom.sh`: specifically check keymaps in `cprom()`; it previously checked
  whether a setup is *not* seauboot, which was valid, but future conditionals
  would break this check. the code has been changed in advance, to prevent bugs
  in a future revision of xbmk.
* `mk`: Fix bad error handling for `gnu_setver`; I accidentally mixed and/or in
  a shorthand conditional statement, which leads to buggy behaviour in various
  implementations of sh.
* GRUB: Mark E820 reserved on coreboot memory, to fix cbmem when running
  with strict `/dev/mem` access; otherwise, restrictions on access to memory
  below 1MB will cause an error when trying to access the cbmem console.
* `lib.sh`: set `-u -e` in `err()` in case they were set `+u +e` in other parts
  of xbmk.
* `init.sh`: Silence the output of git config --global
* `init.sh`: Run git name/email check before init; otherwise, it returns if
  init is already done, which could lead to an error later when building
  coreboot.
* `lib.sh`: stricter `xbmk_err` check in `err()`
* `lib.sh`: simplify err-not-set handling
* `lib.sh` err: add missing redirect to stderr
* xbmk: MUCH safer `err` function; make it an actual function, instead of
  a variable. Initially, this function was made to then check a variable,
  that refers to a function, and a fallback was provided for non-zero exit
  in case the pointed function didn't, but it was later made to be just a
  simple function that exits with a message. Code equals bugs, so fewer lines
  of code will yield fewer bugs.
* `lib.sh`: Make x_ err if first arg is empty; this is a preventative bug fix,
  to make the build system still exit under such conditions, but it would
  result in an empty error message.
* `lib.sh`: Make err_ always exit no matter what; this is a preventative bug
  fix, just making the exit stricter in all cases.
* `mk`: re-make gnupath/ after handling crossgcc, rather than deleting files
  within. This makes the creation of it more reliable.
* `mk`: re-make gnupath/ for each cross compiler, to ensure that no stagnant
  build artifacts are re-used
* `inject.sh`: Stricter TBFW handling; don't copy it until it has been
  properly padded to the correct size.
* `init.sh`: *Re-create* tmpdirs on parent instance, to ensure that they are
  not cluttered with old files that might cause weird bugs in the future; this
  is a preventative bug fix.
* `init.sh`: Always create xbmklocal, to prevent errors in the case when it
  isn't created automatically in certain child instances, like when running
  a *release* copy of the build system, during release builds.
* `lib.sh`: Fix bad touch command
* `inject.sh`: always re-build nvmutil, so that changes to it are automatically
  re-applied when running the build system again. (and only build it once,
  for a given instance of xbmk)
* `util/nvmutil`:  use `x`, not `?`, for random characters, while still
  supporting `?` for backwards compatibility. This is because ZSH errors out
  when providing the old characters, in some setups. Use of `x` is more
  reliable, across several implementations of sh, e.g. `xx:xx:xx:xx:xx:xx`
  would be a full random MAC address.
* `lib.sh` find\_ex: explicitly create the tmp file, to prevent errors, which
  were nonetheless unlikely to begin with.
* `init.sh`: Explicitly create the xbmktmp directory (make sure to do this
  when creating this which is a temporary directory).
* `lib.sh`: add fe_ which is fx_ but err on find
* xbmk: unified execution on `find` commands. Handle it with a new special
  function that is common across the build system.
* `mk`: Download vendorfiles before building release, to mitigate intermittent
  internet connectivity during release builds, otherwise a release build could
  fail. This way, all downloads are done simultaneously, since downloads are
  the fastest part, even on a crap internet connection.
* Revert AHCI reset patch for SeaBIOS, which caused AHCI not to work in SeaBIOS
  on the 25.04 release; the latter was also revised, to fix this. SeaBIOS has
  since added a new release, which includes a fix that delays AHCI reset, to
  mitigate in cases where the controller isn't ready sooner. However, this
  release simply reverts the AHCI reset patch for now. The AHCI reset plus delay
  will be present in Libreboot's next release, after 25.06.
* lenovo/t420: Add missing text-mode configuration
* coreboot (all trees): Added patch fixing GMP build errors on modern
  GCC15 hostcc.
* coreboot (all trees): Fixed building of crossgcc with newer GCC15. Patches
  courtesy of Alper Nebi Yasak.
* coreboot (all trees): Added a patch to fix building coreboot utils with
  newer GCC15.
* dependencies/debian: Fixed the libusb package name for newer Debian releases,
  courtesy of Alper Nebi Yasak.
* SeaBIOS: Fixed `malloc_fn` function pointers in the `romfile` patch, courtesy
  of Alper Nebi Yasak. Fix build errors on GCC 15.
* `include/lib.sh`: Force use of System Python e.g. `/usr/bin/python`, when
  a python venv is detected. This prevents the build system from hanging.
* coreboot `g43t_am3`: Fixed the `data.vbt` path.
* Alper Nebi Yasak fixed the Python 2/3 detection in some edge cases when
  the `python` command is python2.
* `include/lib.sh` (later `init.sh`): Do root check *first*, right after
  the dependencies check, whereas it previously did the python check before
  checking for root user.
* lbmk: Don't use TMPDIR directly, use another variable containing its value,
  and make sure it doesn't get changed wrongly. This reduces the possibility
  of accidentally leaving old tmp files laying around.
* `include/lib.sh`: `tar` commands now return an exit with error, if a fault
  occurs, whereas it didn't before, due to piped output. This is done
  using the `x_` wrapper on tar commands, to provide error exits.
* `mk`: function `check_project_hashes` now returns an error, if the sha512sum
  command fails. It previously didn't, due to piped outputs. It's now mitigated
  by using `x_` on piped commands, for error exits.
* Forking of lbmk parent instance to child instance isno longer handled by
  variables. It's been simplified, to only be based on whether TMPDIR is set,
  and it's generally more robust now in this release. The old code sometimes
  broke under certain edge cases.
* `include/vendor.sh` (later renaming to `inject.sh`): General code cleanup,
  about 100 sloc removed without reducing features.
* lbmk: Initialise `PATH` to a standard string if not set, on the parent
  instance of lbmk.
* lbmk: Use `pwd` instead of the `PWD` variable, resetting the latter safely
  as lbmk runs. This prevents lbmk from changing directory to an erroneous
  system path, if `PWD` wasn't properly set for some reason. This is a
  preventative bug fix, because no actual issue ever occured in practise.
* Much safer Python version check at lbmk startup, using data structures that
  are provided universally by all Python implementations, instead of
  relying on the output of `--version`.
* Fixed T480 backlight controls, courtesy of a patch from Mate Kukri.
* Set up Python in `PATH` when lbmk starts, to ensure that it is always
  version 3. This is checked at startup.
* `include/vendor.sh`: Prevent double-nuke, where a given tarball already had
  vendor files removed prior to release.
* `include/vendor.sh`: Allow setting a MAC address even if vendor files aren't
  needed.
* `include/vendor.sh`: Download utils even if `vcfg` is not set, in case the
  user is also setting a MAC address.
* `util/nvmutil`: Honour the `INSTALL` variable, if set by the user, otherwise
  it is set to `install` by default.
* `util/nvmutil`: Don't do `clean` when running `uninstall`.
* `util/nvmutil`: Proper DESTDIR/PREFIX handling, whereas it was not handled
  properly at all before.
* `util/nvmutil`: Only set CC/CFLAGS if unset, and use sensible defaults.
* Fixed various shellcheck errors in lbmk.
* HP EliteBook 820 G2: Fixed vendor file insertion and set `release=y`. The
  insertion of Intel MRC and refcode previously didn't pass checksum validation.
* ThinkPad T480 / OptiPlex 3050: Force power-off state upon recovery from
  power loss, otherwise the system always turns on as soon as a charger is
  plugged in. This is configured by hardcoding, due to a current lack of any
  option table on the T480.
* Debian dependencies: replace liblz4-tool with lz4 and liblz4-dev. The latter
  is also available in Debian Trixie and Sid, at this time, in addition to
  Debian Bookworm, so it works on all of them.
* U-Boot (x86): Fixed a bug since Swig 4.3.0 changed the syntax for its
  language-specific AppendOut functions. A patch from upstream was backported,
  and the patch is also compatible with older versions of Swig.
* In lbmk scripts, use `command -v` instead of `which`, to find the locations
  of certain binaries. This is a bug fix, since `which` is non-standard and
  so could break on some setups.
* Crossgcc: when building it for coreboot, fix mismatching GCC/GNAT versions
  so that they match, if multiple versions are present. This was done because
  Debain Trixie initially had GCC 14 and GNAT 13, whereas we need GNAT to build
  the Intel video init code on  many mainboards.
* T480/T480: Disable TPM2 to mitigate a hang in SeaBIOS due to buggy drivers.
* `dependencies/debian`: Fix the `python3-distutils` package, renamed it
  to `python3-distutils-extra`, which works on bookworm *and* newer, but the
  former did not.
* `git.sh`: don't initialise the `livepull` variable globally, reset it per
  target instead, to prevent some repositories from being wrongly re-cloned.
* Thinkpad T480 / Dell OptiPlex 3050: Handle FSP insertion post-release, rather
  than providing FSP images directly in release images. It is now handled by
  the `./mk inject` command, copying the reference image from coreboot and
  splitting it upp and rebasing it, to mitigate certain technicalities of
  Intel's FSP license, which otherwise permits free redistribution.
* Safer, more reliable exit when handling vendor files, because in some cases
  lbmk was leaving the `lock` file in place (erroneously).
* Safer exit when running the `./mk dependencies` commands, so that lbmk is
  more likely to exit, because it was theoretically possible that it might
  not under certain edge cases.
* Disable nvme hotplug on Dell OptiPlex 3050 Micro, to prevent replugging in
  Linux, which would otherwise lead to possible data corruption.
* T480: Fix coreboot SPD size to 512 instead of 256 (it was already
  auto-corrected to 512 at build time, but the original configs were 256 which
  is wrong).
* Add tarballs and gpg signatures to `.gitignore`

### General code cleanup

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi/wson8/0007.jpg" /><span class="f"><img src="https://av.libreboot.org/rpi/wson8/0007.jpg" /></span>

Another bug focus in this release was to clean up the logic of Libreboot's
build system, and fix several bugs, especially those relating to error handling.

A lot of cleanup was done on the init functions used by the build system, to
initialise common variables, such as environmental variables, and temporary
files and/or directories; such logic was moved to a new script called `init.sh`.

In descending order from the latest changes to the earliest changes:

* `lib.sh`: simplify `setvars()`
* `lib.sh`: simplify `chkvars()`
* `mk`: simplify `main()`
* `get.sh`: simplify `fetch_project()`
* `get.sh`: simplify `try_copy()`
* `get.sh`: tidy up `bad_checksum()`
* `get.sh`: simplify `fetch_targets()`
* general cleanup in `get.sh` and `vendor.sh`
* xbmk: rename `xbmklocal`/`xbmktmp` variables (shorten them)
* `get.sh`: consolidate printf statements
* `get.sh`: remove redundant printf in `fetch_project()`
* `get.sh`: remove superfluous command in `try_git()`
* `vendor.sh`: simplify `fetch()`
* `rom.sh`: simplify `mkcoreboottar()`
* `rom.sh`: rename `mkvendorfiles()`, which actually handles general init
  tasks, including the processing of vendor files where appropriate.
* `rom.sh`: simplify ccache handling for coreboot; make-oldconfig wasn't
  needed at all, when cooking configs to enable ccache, so the *cook* function
  became much smaller and was then merged with `mkvendorfiles()`
* `rom.sh`: simplify u-boot payload handling, by using a single variable name
  that defines the type of U-Boot tree. This allows several other U-Boot-related
  checks to be greatly simplified, as they were.
* `vendor.sh`: add a colon at the end of a `for` loop
* `get.sh`: make `xbmkget()` easier to understand, by not using shorthand
  conditional statements in the for loop handling a repository or file
  download.
* `init.sh`: merge `xbmk_lock()` with `xbmk_set_env()`
* `init.sh`: set pyver from `set_env()` instead of the main function.
* `init.sh`: merge `xbmk_mkdirs()` with `set_env()`
* `init.sh`: only update version files on parent, to speed up xbmk
* `init.sh`: simplify unknown version creation, where none was created and
  no Git metadata exists.
* `init.sh`: only set xbmk version on parent instance; we only need to read
  what was set, on child instances. In other words, apply the principle of
  least privelege.
* `init.sh`: initialise variables AFTER path, to avoid unnecessary work inside
  child instances of xbmk.
* `init.sh`: merge `create_pathdirs()` with `set_pyver()`
* `init.sh`: Set python version only on parent instances of xbmk, to speed
  up operation of the xbmk build system.
* `init.sh`: `xbmk_create_tmpdir()` to `xbmk_mkdirs()`
* `init.sh`: move `gnupath` creation to `create_tmpdir()`
* `init.sh`: move PATH init to `set_env()`
* `inject.sh`: shorten the `nukemode` variable name
* `vendor.sh`: simplify `mksha512sum()`
* `get.sh`: simplify `bad_checksum()`
* `release.sh`: use `x_()` on find command for `nuke()`, so as to remove the
  need for a more complicated while loop inside said function.
* `get.sh`: move `nuke()` to `release.sh` and only run it on releases; don't
  do it on normal xbmk Git. It's only needed in the former context, because
  that has to do with distribution by the project, and this makes
  development easier. Therefore, files are only purged within the release
  archives, but not during development.
* `release.sh`: simplify `prep_release_bin()`
* `mrc.sh`: merge `extract_mrc()` with `extract_shellball()`
* `get.sh`: simplify `tmpclone()`
* `get.sh`: simplify `nuke()` by using `fx_` for the file loop
* `get.sh`: simplify `try_copy()`
* `get.sh`: simplify `fetch_submodules()` config check
* `get.sh`: simplify `fetch_submodules()` by using `xbmkget()` for everything,
  instead of implementing redundant logic in the build system.
* `git.sh`: rename to `get.sh`
* `rom.sh`: reduce indendation in `check_coreboot_utils`; simplify the for
  loop by replacing it with a call to `fx_` instead.
* `release.sh`: simplify `release()`
* `release.sh`: clean up the `vdir` after release
* `release.sh`: removed an unnecessary `mkdir` command
* `release.sh`: split up `build_release()` into smaller functions
* `lib.sh`: remove `rmgit()`
* `lib.sh`: remove the unnecessary `mk()` function
* `lib.sh`: move `xbmkget()` to `git.sh`
* `lib.sh`: move `mksha512sum()` to `vendor.sh`
* `lib.sh`: split up `try_file()` into smaller functions
* `lib.sh`: move `_ua` to `try_file()`
* `mrc.sh`: minor cleanup
* `inject.sh`: remove the `hashfiles` variable
* `inject.sh`: define `xchanged` here instead
* `vendor.sh`: remove `check_vcfg()`
* `vendor.sh`: simplify `getvfile()`
* `vendor.sh`: simplify `setvfile()`
* `lib.sh`: use `fx_` in `rmgit()`
* `lib.sh`: split up `xbmkget()` into smaller functions
* `inject.sh`: only compile nvmutil if needed
* `inject.sh`: simplified serprog check
* `vendor.sh`: tidy up variables
* `vendor.sh`: split up `setvfile()` into smaller functions
* `inject.sh`: further cleanup for `vendor.sh`, such that all vendor-download
  functions are only defined in `vendor.sh`; this means that the Canoeboot
  version of the file can remain in much closer sync, with fewer differences.
* `tree.sh`: simplified srcdir check on make-clean
* `inject.sh`: split download functions to a new file, `vendor.sh`
* `inject.sh`: split up the inject functions into smaller functions for
  each specific task.
* xbmk: use `x_` instead of `err`, where appropriate, because it handles
  globbing perfectly these days, and `x_` is cleaner in most cases.
* `mrc.sh`: fix outdated info in a comment
* `inject.sh`: use direct comparison for metmp, to speed up checking so many
  files.
* `mk`: remove unnecessary line break
* `mk`: re-split tree logic to new file, `include/tree.sh`
* `mk`: move release functions to `include/release.sh`
* `git.sh`: use `setvars()` for fail variables
* `init.sh`: remove useless export; variables that are y/n can just be reset
  to `n` if not set to `y`, for simplicity.
* `init.sh`: export `LOCALVERSION` in `set_env` instead of `set_version`.
* `inject.sh`: simplify `extract_kbc1126ec()`
* `inject.sh`: simplified MAC address handling
* `inject.sh`: Simplify `patch_release_roms()`
* `lib.sh`: Remove useless command in `err()`
* `inject.sh`: rename `copytb` and `preprom` functions (make the names shorter).
* `lib.sh`: Simplified `fx_()` and removed `fe_()`; fe didn't prefix `x_` to
  a given command, but fx did. Now, it is prefix manually, for greater control,
  on commands that need stricter error handling, while it can be avoided on
  commands where strict error handling is unfeasible.
* `mk`: Create serprog tarballs here instead; `rom.sh` was simplified to use
  mkhelp when building actual images.
* build serprog images using `fx_` *defined inside mkhelper*, to tidy up xbmk
* `rom.sh`: build serprog images with `fx_`, rather than implementing a
  specific for loop.
* `inject.sh`: insanely optimise the me bruteforce, by operating on files
  recursively via the `fx_` function, instead of manually implementing a
  recursive file search, when bruteforce-extracting `me.bin` images.
* `git.sh`: Simplify git am handling by using the new `fe_` or `fx_` function,
  instead of making a specific while loop.
* `inject.sh`: remove an unused function
* `init.sh`: New function `dx_` to execute path files; this is used instead
  of for loops all around xbmk, to simplify operations where the output of
  a file search is used as argument to a function.
* `inject.sh`: Further simplified FSP extraction
* `lib.sh` `find_ex`: Write sort errors to `/dev/null`
* `lib.sh` `x_()`: Remove warning of empty args; it's really not required,
  since it's obvious anyway in the resulting final error message.
* xbmk: Replace `err()` with much simpler implementation, for reliability
  and bug prevention.
* `rom.sh`: simplify `mkseagrub()`
* `mk`: simplify `elfcheck()`
* `lib.sh`: simplify `singletree()`
* `git.sh`: move `singletree()` to `lib.sh`
* `inject.sh`: simplify `extract_intel_me_bruteforce()`
* `inject.sh`: Remove unnecessary check
* `inject.sh` `extract_intel_me()`: reduce indentation
* `inject.sh`: Move FSP extraction only to `extract_fsp`, since that's the
  only place where it's needed.
* `inject.sh`: tidy up intel me handling
* `inject.sh`: tidy up the deguard command
* `lib.sh`: simplify `err()`
* `init.sh`: single-quote xbmklock in `xbmk_lock()`
* `init.sh`: define lock file in a variable instead; this makes it more
  flexible, because the path can be checked and then re-used nicely.
* `init.sh`: tidy up `xbmk_child_exec()`; make the command style more consistent
* `lib.sh`: rename errx to xmsg
* `inject.sh`: tidy up TBFW handling
* `inject.sh`: remove useless comment block
* `init.sh`: tidy up the python version check
* `init.sh`: move non-init functions to `lib.sh`
* `init.sh`: simplify dependencies handling
* `rom.sh`: tidy up `copyps1bios()`
* `mk`: tidy up xgccargs handling
* `mk`: generally removed dead code
* `init.sh`: tidy up pathdir creation
* `mk`: tidy up `check_cross_compiler()`
* `mk`: reduce indentation in `check_cross_compiler()`
* `mk`: Allow use of x_ on prefix functions
* `mk`: tidy up `check_project_hashes()` sha512sum check
* `mk`: simplify `check_gnu_path()`
* `inject.sh`: general code cleanup
* `inject.sh`: simplify `extract_archive()`
* `inject.sh`: simplified fsp extraction
* `inject.sh`: Remove redundant code in copy_tbfw
* xbmk: Unified local ./tmp handling
* `lib.sh`: redirect find errors to `/dev/null` to prevent clutter on
  the user's terminal
* `init.sh`: unified handling of ./tmp
* `mk`: include rom.sh directly
* `lib.sh`: Simplify rmgit()
* `lib.sh`: support multiple arguments in remkdir()
* `lib.sh`: simplify remkdir()
* xbmk: move x_() to `lib.sh`
* `init.sh`: move setvars/err_ to lib.sh
* `init.sh`: Generally modularised it, moving separate tasks into separate
  functions, rathher than having it be one big monolith.
* `vendor.sh` was renamed to `inject.sh`, so that future changes can be
  in better sync between lbmk and cbmk on this file, because the cbmk version
  has the MAC address changer (but no vendorfile handling). In the future,
  this will be split so that `vendor.sh` exists again, containing only the
  vendorfile handling, and `inject.sh` will only handle MAC addresses.
* `init.sh`: Several variables were moved out of this file and elsewwhere in
  lbmk.
* Moved the `singletree` function to `git.sh` instead of `lib.sh`
* Moved the `cbfs` function from `lib.sh` to `rom.sh`.
* `include/lib.sh`: Use a more top-down function order, more clear, and it was
  split into an extra file `init.sh` that does the most basic lbmk initialisation
  at startup, whereas what remains in `lib.sh` really are generic library
  functions used throughout lbmk.
* `include/git.sh`: Removed unused crossgcc linking feature, because we don't
  use it anymore (coreboot trees have their own crossgcc and never link to
  another these days). Libreboot used to have many more coreboot trees, some
  of which re-used crossgcc from another tree. Similarly, the accompanying
  variable `tree_depend` is no longer handled. The `xtree` variable is still
  handled, because projects like U-Boot use that to configure crossgcc.
* include/vendor.sh: Removed unnecessary check against the ROM image size.
  Generally simplified the processing of release images.
* include/git.sh`: Removed many redundant functions, merging several of them.
* `include/mrc.sh`: Fixed a bad print, making proper use of a string inside
  a printf statement.
* Simplified many file checks in lbmk, by using the `e` function.
* Removed a bunch of useless `eval` commands in general, throughout lbmk,
  making the code much cleaner.
* lbmk: the `x_` function is now used much more aggressively, for error
  handling, simplifying error handling in lbmk overall.
* `mk` main script: Merged the `trees` script with it, so now it's all one
  script. The `mk` script is now the only executable script in lbmk.
* `mk` (main script): The `roms` command is removed (legacy / obsolete).
* The version/versiondate files are now dotfiles, to hide during operation.
* `include/lib.sh`: Hardcoded projectname/projectsite variables, instead
  of storing them in a file.
* `trees` script: Unified handling of flags (same string used in error output),
  to ensure that error(usage) messages always match.
* `trees` script (later merged into `mk`): Removed a lot of old bloat.
* `util/nvmutil`: Make the checksum word position a define. Generally cleaned
  up a lot of code to make it clearer for the reader. Added more verbose
  messages to the user, confirming things such as how much was read or written
  on the user's file system. Various miscallaneous bug fixes (edge cases that
  were unlikely to ever be triggered).
* `util/nvmutil`: More efficient use of memory when handling files.
* `util/nvmutil`: Much cleaner handling of user input.
* `util/nvmutil: More granular MAC address parsing errors, easy for debugging.
* `util/nvmutil`: Make the Gbe Checksum a define, for readibility.
* `util/nvmutil`: Obey the 79-character-per-line limit, as per lbmk coding
  style.
* `util/nvmutil`: Tidied up several pledge calls
* Removed use of several unnecessary subshells and `eval` statements in lbmk.
* `trees`: Later, the GCC/GNAT matching feature was rewritten to work both ways,
  where an older GCC was matched to GNAT and vice versa, whereas it previously
  only went one way. `gcc` and `gnat` are manipulated in `PATH` to ensure that
  the user has a consistent version of both.
* `path.sh` later merged into the `trees` script (which later merged
  into the main `mk` script). This `path.sh` is what contained the first
  implementation of the GNAT/GCC version matching feature.
* `path.sh`: Remove unnecessary shebang, and the same on other `include/`
  scripts. NOTE: `path.sh` was later merged into `lib.sh`, which then became
  split into `init.sh` in later changes (see above).
* Removed legacy build system commands e.g. `./build` and `./update`; now
  only the newer `./mk` commands are supported. This and the change below was
  briefly reverted, for the 20241206 revisions, but then re-introduced.
* Removed the deprecated `./vendor` command; now only `./mk` commands are
  used. The `./mk -d coreboot target` commands are used, for downloading
  vendor files.
* Removed unused patch that was for the original deguard implementation,
  prior to Mate Kukri's re-write of it.

Git log
-------

This log shows all changes in today's release, from 30 June 2025, ever
since the Libreboot 20241206 release of 6 December 2025:

```
* c46a71138c7 Libreboot 25.06 release 
* b1ef562b767 tree.sh: add sha512 error for check_project_hashes 
* 04bee3834d0 tree.sh: add error check in check_project_hashes() 
* 677dfc4d103 tree.sh: more reliable clean in run_make_command 
* 267d4c90341 inject.sh: add missing semicolons 
* 974bdbb3815 vendor.sh: fix bad cbfstool path 
* dc6996252a0 put coreboot utils in elf/coreboot/TREE 
* b77154640de release.sh: use printf to create version files 
* dee6997d0cc lib.sh: simplify setvars() 
* 79ded40f3d0 lib.sh: simplify chkvars() 
* 5036a0bc501 mk: simplify main() 
* 41308ee9244 get.sh: simplify fetch_project() 
* b5867be214d get.sh: simplify try_copy() 
* 495098d6a71 get.sh: tidy up bad_checksum() 
* 671e3aa27b4 get.sh: simplify fetch_targets() 
* 09b6e91803d general cleanup in get.sh and vendor.sh 
* 18dacd4c22b xbmk: rename xbmklocal/xbmktmp variables 
* e981132c829 get.sh: consolidate printf statements 
* afc36754b13 get.sh: remove redundant printf in fetch_project 
* ffe387ac6b9 get.sh: remove superfluous command in try_git() 
* ba7c49c090b vendor.sh: simplify fetch() 
* 30bc3732c39 init.sh: error out if .git/ is a symlink 
* 2493203ee53 get.sh: Properly error out if tmpclone fails 
* ad333ae2481 tree.sh: Don't auto-run make-oldconfig 
* 97ce531c341 rom.sh: simplify mkcoreboottar() 
* a47e9811723 rom.sh: rename mkvendorfiles 
* d2e148fdd9d rom.sh: simplify ccache handling for coreboot 
* 8c3f10ba402 rom.sh: simplify u-boot payload handling 
* 3e28873532b ifd/hp8300usdt: set the HAP bit by default 
* 452aeb6001a coreboot: Remove unused vboot tests 
* 64cc91bca33 coreboot/default: Remove unneeded FSP modules 
* 0216a3104a5 get.sh: Always update git remotes 
* 419733d3073 get.sh: re-generate remotes every time 
* 231b320e63b release.sh: copy version files to rsrc 
* fc0720184d9 xbmk: add fake config makefile args to flashprog 
* f9266601b8c vendor.sh: add colon at the end of a for loop 
* 8e0c6059d15 rom.sh: skip copyps1bios on dry builds 
* a3250d14474 tree.sh: Don't run make-clean on dry runs 
* 24b8e633e03 GRUB: Update to revision 73d1c959e (14 March 2025) 
* f6b77822835 Revert "vendor.sh: optimise find_me()" 
* fb7aaa78bb0 vendor.sh: optimise find_me() 
* 903f78bf080 get.sh: add missing check in fetch_project() 
* f15bb8153a3 get.sh: stricter URL check in xbmkget() 
* cdc0fb49e1c get.sh: make xbmkget() easier to understand 
* 620c1dd6fae get.sh: Make xbmkget err on exiting the loop check 
* 900da04efa9 tree.sh: fix up copy_elf(), bad for loop 
* 8aaf404ddea lib.sh: Use while, not for, to process arguments 
* d9c64b26754 xbmk: stricter handling of files on while loops 
* b25a4876434 init.sh: looser XBMK_THREADS validation 
* 769a97aed5a init.sh: Hardcode XBMK_CACHE for integrity 
* 265ec0b7673 dependencies/debian: add libx86 
* 2702a43a86d init.sh: merge xbmk_lock() with xbmk_set_env() 
* fc4006ce877 init.sh: move xbmk_set_version 
* 962902a1c4a init.sh: set pyver from set_env 
* 158c56072c0 init.sh: merge xbmk_mkdirs with set_env 
* 5f022acbf47 init.sh: check version/versiondate once read 
* 485a60e2f6a init.sh: error if version not read 
* 99f09f25ef3 init.sh: only update version files on parent 
* 94437278dc7 init.sh: simplify unknown version creation 
* 6b603b9fbf4 init.sh: only set xbmk version on parent instance 
* ac36ea7f950 init.sh: initialise variables AFTER path 
* 484afcb9196 init.sh: merge create_pathdirs with set_pyver 
* d0bee6b4ebb init.sh: Set python version only on parent 
* 4aa69a7d1f0 init.sh: remove useless command 
* 36ffe6ef501 init.sh: remove useless comment 
* 0343081d905 init.sh: xbmk_create_tmpdir to xbmk_mkdirs 
* c75bc0449d0 init.sh: move gnupath creation to create_tmpdir 
* 253aa81a3f9 init.sh: move PATH init to set_env 
* e05a18d3513 init.sh: check the lock file BEFORE git init 
* cde3b7051e4 init.sh: return from child in set_env instead 
* 7ec9ee42283 inject.sh: shorten the nukemode variable name 
* b48eb161e49 vendor.sh: simplify mksha512sum() 
* ac609d5aae4 vendor.sh: Remove _dest if it's bad 
* a3e1ed9823d release.sh: rename relsrc to rsrc 
* 44df3b2bff8 release.sh: tidy up nuke() 
* 3c58181f69e get.sh: remove useless message 
* 01a0217c1e3 get.sh: simplify bad_checksum() 
* 4ca57943d70 release.sh: simplify nuke() EVEN MORE, yet again 
* 47a3982bbea release.sh: use x_ on find command for nuke() 
* 6dc71cc0246 release.sh: simplify nuke() EVEN MORE 
* 05c07f7401b get.sh: move nuke() to release.sh 
* 587d245cafa release.sh: simplify prep_release_bin() 
* 136bd66c280 mrc.sh: merge extract_mrc with extract_shellball 
* dbe109d7b54 release.sh: don't move src/docs/ 
* 840d6a1d277 get.sh: FURTHER simplify nuke() 
* d2564fd9457 get.sh: simplify tmpclone() 
* 6dea381614d get.sh: fix bad mkdir command 
* 6a2ed9428b7 vendor.sh: Fix broken KBC1126 insertion 
* 4313b474a59 vendor.sh: additional safety check 
* d668f3a3529 vendor.sh: Properly verify SHA512SUM on extraction 
* a191d22bd6d get.sh: add missing eval to dx_ in nuke() 
* c8813c9a144 properly exit 1 when calling fx_ 
* 208dfc89bd5 get.sh: simplify nuke() 
* 46f42291d3c get.sh: fix broken printf statement 
* f29aa9c8d59 get.sh: use subshells on try_ functions 
* e62886dedae get.sh: simplify try_copy() 
* d9ed03f9ea5 get.sh submodules: Don't delete files recursively 
* 8d5475ed5b5 get.sh: simplify fetch_submodules() config check 
* 21867b7d805 get.sh: simplify fetch_submodules() 
* e9fe5a74a2e get.sh: fix caching of crossgcc tarballs 
* 6089716f07c release.sh: Don't run prep_release with fx_ 
* b04c86e5740 git.sh: rename to get.sh 
* 3c23ff4fa18 git.sh: Only create destination repo on success 
* ed8a33d6fb1 git.sh: cleanup 
* 1ca26c5d238 git.sh: Re-implement redundant git downloads 
* e38805a9448 rom.sh: reduce indendation in check_coreboot_utils 
* 6bf24221e60 release.sh: simplify release() 
* 66f7ecdb2d7 release.sh: clean up the vdir after release 
* d4c0479093a release.sh: remove src_dirname variable 
* 6d3a6347c3e release.sh: build in tmp directory first 
* a0105e1ab44 release.sh: remove unnecessary mkdir command 
* f4871da9bca release.sh: split up build_release() 
* c85aff5c54e release.sh: delete tmp/cache from the tarball 
* 92954eeb38f lib.sh: remove rmgit() 
* 05b5914b354 lib.sh: remove mk() 
* c9696e23338 lib.sh: move xbmkget() to git.sh 
* 23913bb8d2a lib.sh: move mksha512sum() to vendor.sh 
* 80f0562e8d1 lib.sh: split up try_file() 
* 89cd828e87c lib.sh: move _ua to try_file() 
* 308a9ab1e17 mrc.sh: minor cleanup 
* 40163dcfa4e mrc.sh: update copyright year to include 2025 
* ef800b652c8 inject.sh: remove the hashfiles variable 
* 311ae2f8df2 inject.sh: define xchanged here instead 
* 76f81697e6e vendor.sh: remove check_vcfg() 
* 97d4d020d97 vendor.sh: simplify getvfile() 
* 57f896ac016 vendor.sh: simplify setvfile() 
* 3879f6c4d8f lib.sh: use fx_ in rmgit() 
* 0911a5a5aed lib.sh: split up xbmkget() 
* a449afb287f inject.sh: only compile nvmutil if needed 
* 2bbf2ae80b7 inject.sh: simplified serprog check 
* 9c27b7437cf vendor.sh: tidy up variables 
* 0cc816167bb vendor.sh: split up setvfile() 
* 7d90d434252 remove another confusing message 
* a0c436ad4ba inject.sh: Remove confusing path on tar creation 
* dcfd3e632e2 inject.sh: re-add mac address confirmation 
* e5af201060e inject.sh: further cleanup for vendor.sh 
* 0aa99f4bf8b tree.sh: only create elfdir in copy_elf() 
* a8e374020c0 tree.sh: simplified srcdir check on make-clean 
* 0f931b508a8 inject.sh: split to vendor.sh the download parts 
* 3554b5aad9c inject.sh: split up the inject functions 
* 81dbde7e09f lbmk: use x_ instead of err, where appropriate 
* 14d46abceda mrc.sh: operate on refcode in tmp area first 
* 6e521c2e1ea mrc.sh: fix outdated info in the comment 
* 23486abef3a inject.sh: use direct comparison for metmp 
* 91220ce1833 inject.sh: use subshell to speed up find_me() 
* ff33ec3352b mk: use zero exit instead, to run trees 
* c2b627dc6d0 remove useless comment 
* 066402b7e7a mk: remove unnecessary line break 
* 7012c00ed11 mk: re-split tree logic to include/tree.sh 
* 50ce1ac9b22 mk: move release functions to idnclude/release.sh 
* 1ce3e7a3d39 mk: add missing error handli for mk -f 
* 0d876622fcb git.sh: re-write tmpclone without caching 
* 454f11bdd7b git.sh: use setvars for fail variables 
* 6bdb15fd329 git.sh: hard fail if git am fails 
* 93d4eca04ae git.sh: Hard fail if reset fails 
* a3ba8acface init.sh: Only check XBMK_CACHE if it exists 
* 021e7615c84 HP 820 G2: Use fam15h cbfstool tree for refcode 
* fe926052441 also fix the other grub trees 
*   a8594762d27 Merge pull request 'fix trying to boot all logical volumes after unlocking an encrypted volume' (#330) from cqst/lbmk:master into master 
|\  
| * e084b06dc76 fix trying to boot all logical volumes after unlocking an encrypted volume 
|/  
* 2cea8517f3b init.sh: remove useless export 
* 1b0afdcea22 init.sh: also allow XBMK_RELEASE=Y or N 
* 570f1417a80 init.sh: Resolve XBMK_CACHE via readlink 
* e1af1055ed1 init.sh: check XBMK_CACHE is a directory instead 
* e1628ad8f3e init.sh: export LOCALVERSION in set_env 
* 40a944118f2 init.sh: run set_version before set_env 
* cba04aa74b8 init.sh: Use readlink in pybin() 
* a94bd3c0939 inject.sh: simplify extract_kbc1126ec() 
* e3098c61f43 inject.sh: simplified MAC address handling 
* d530e68594d inject.sh: Simplify patch_release_roms() 
* 7f71328f0e2 lib.sh: Remove useless command in err() 
* 394b4ea7a59 inject.sh: rename copytb and preprom functions 
* ec5c954337b lib.sh: Simplified fx_() and removed fe_() 
* 1390f7f8007 mk: Create serprog tarballs here instead 
* 0ef77e65832 build serprog using fe_ *defined inside mkhelper* 
* d2e6f989d7e rom.sh: build serprog images with fe_ 
* 0faef899469 lib.sh: support any command on find_exec() 
* 2b7f6b7d7ce inject.sh: Simplify extract_intel_me_bruteforce() 
* 485d785d331 inject.sh: clean up tmp me file before extract 
* fac99aa2d44 lib.sh: re-add missing break in fe/fx_ 
* 03300766d14 inject.sh: tidy up extract_intel_me_bruteforce 
* 4781dbd2a05 inject.sh: fix oversight in me bruteforce 
* cf78583a6d8 inject.sh: remove unnecessary check 
* 5657cc1afb3 inject.sh: don't use subshell for me bruteforce 
* 5686f35e0f1 inject.sh: insanely optimise the me bruteforce 
* e8be3fd1d41 git.sh: Simplify git am handling 
* 4c1de1ad126 inject.sh: remove unused function 
* 282b939d9da init.sh: New function dx_ to execute path files 
* 73074dedee3 inject.sh: Further simplified FSP extraction 
* 7585336b914 inject.sh: simplify kconfig scanning 
* ef38333f8b0 lib.sh find_ex: Write sort errors to /dev/null 
* c275f35e7e2 lib.sh x_(): Remove warning of empty args 
* 17d826d3a96 lbmk: Replace err with much simpler implementation 
* f98e34a24dd singletree/elfcheck: use fx_, not fe_ 
* 8ca06463ebc rom.sh: Print the rom image path being generated 
* dc9fe517cb0 rom.sh: Safer cprom() 
* 2be8d1c7982 rom.sh: specifically check keymaps in cprom() 
* 89a8cd4936a rom.sh: simplify mkseagrub() 
* c2182d82193 mk: simplify elfcheck() 
* 437ac2454c1 lib.sh: simplify singletree() 
* 62ec3dac075 git.sh: move singletree() to lib.sh 
* 6b247c93e25 mk: Fix bad error handling for gnu_setver 
* ee8bb28ba21 GRUB: Mark E820 reserved on coreboot memory 
* 61ec396ef6d inject.sh: simplify extract_intel_me_bruteforce() 
* e4edc2194d3 inject.sh: Remove unnecessary check 
* f4057d7daab inject.sh extract_intel_me(): reduce indentation 
* b7ca59debe6 inject.sh: Move FSP extraction only to extract_fsp 
* eb882de94cb inject.sh: tidy up intel me handling 
* 153dd76a82e inject.sh: tidy up the deguard command 
* 428c46ca2b1 lib.sh: set -u -e in err() 
* 20c87308587 lib.sh: Provide error message where none is given 
* 35265731c5b init.sh: Silence the output of git config --global 
* 5e3aaa1eb8b init.sh: Run git name/email check before init 
* a3b5626f53d lib.sh: stricter xbmk_err check in err() 
* 51b2a1159d0 lib.sh: simplify err-not-set handling 
* 61e5fd1a0b2 lib.sh: Add warning if x_ is called without args 
* 4020fb43280 lib.sh: simplify err() 
* b51846da6de init.sh: single-quote xbmklock in xbmk_lock() 
* 8b7bd992f66 init.sh: define lock file in a variable instead 
* 9611c19e7ed init.sh: tidy up xbmk_child_exec() 
* 37ca0c90e1c lib.sh err: add missing redirect to stderr 
* 54291ebb720 lbmk: MUCH safer err function 
* 3f7dc2a55f5 lib.sh: rename errx to xmsg 
* 59c94664e3e lib.sh: Make x_ err if first arg is empty 
* 91bb6cbede0 lib.sh: Make err_ always exit no matter what 
* b19c4f8f674 inject.sh: tidy up TBFW handling 
* 439020fbda5 inject.sh: remove useless comment block 
* 6e447876cca init.sh: tidy up the python version check 
* 7392f6fc8ec init.sh: move non-init functions to lib.sh 
* 7acec7a3a1d init.sh: simplify dependencies handling 
* 93ba36ae456 rom.sh: tidy up copyps1bios() 
* fc71e52fdfc mk: tidy up xgccargs handling 
* 184871bc17c mk: remove useless code 
* b6a2dc4ea3c init.sh: tidy up pathdir creation 
* f5b2bdb8868 mk: re-make gnupath/ after handling crossgcc 
* 1b7a9fd637d mk: tidy up check_cross_compiler 
* 488d52e784f mk: re-make gnupath/ for each cross compiler 
* c33467df1e6 mk: reduce indentation in check_cross_compiler() 
* aa4083443b1 mk: Allow use of x_ on prefix functions 
* 8f828e6cd35 mk: tidy up check_project_hashes() sha512sum check 
* 7a2f33264d7 mk: simplify check_gnu_path() 
* 46b968a6e85 inject.sh: minor code cleanup 
* 5499ae66bd8 inject.sh: simplify extract_archive() 
* 72f4412a52d inject.sh: simplified fsp extraction 
* bf569d2b4dc inject.sh: Remove redundant code in copy_tbfw 
* 8de0ed811fb inject.sh: Stricter TBFW handling 
* 530e4109a2b init.sh: *Re-create* tmpdirs on parent instance 
* 498f5a26cc8 init.sh: Always create xbmklocal 
* 00d22f20829 lbmk: Unified local ./tmp handling 
* 0f7b3691aba lib.sh: redirect find errors to /dev/null 
* 7fadb17fd9e lib.sh: Fix bad touch command 
* 0b09d970732 inject.sh: Only build nvmutil once 
* 308df9ca406 inject.sh: always re-build nvmutil 
* 44a1cc9ef85 util/nvmutil:  use x, not ?, for random characters 
* a17875c3459 lib.sh find_ex: explicitly create the tmp file 
* 0ffaf5c7331 init.sh: Explicitly create the xbmktmp directory 
* fcc52b986e7 init.sh: unified handling of ./tmp 
* 47762c84ad0 lib.sh: add fe_ which is fx_ but err on find 
* d18d1c2cae2 lbmk: unified execution on find commands 
* 773d2deaca0 NEW MAINBOARD: Dell Precision T1700 SFF and MT 
* 9b11e93686c mk: include rom.sh directly 
* 1f7e4b35cb2 mk: Download vendorfiles before building release 
* acb0ea202f2 lib.sh: Simplify rmgit() 
* 15b76bc202f lib.sh: support multiple arguments in remkdir() 
* f3ae3dbbbe4 lib.sh: simplify remkdir() 
* 6c4d88f2686 move x_() to lib.sh 
* 2ae565ba93a init.sh: move setvars/err_ to lib.sh 
* c073ee9d4fc Restore SeaBIOS 9029a010 update, but with AHCI fix 
* 8245f0b3211 Revert "seabios: bump to rev 9029a010, 4 March 2025" 
* 4c50157234d coreboot/t420_8mb: add missing txtmode config 
* f21749da8b1 Libreboot 25.04 Corny Calamity 
* bb5f5cd5763 add pico-sdk backport patch fixing gcc 14.x 
* 4f77125066d coreboot/fam15h: update submodule for nasm 
* 0f2202554ab coreboot/fam15h: update nasm to 2.16.03 
* 2009c26f0aa serprog: Remove pico2 support for the time being 
* a08b8d94fc5 seabios: bump to rev 9029a010, 4 March 2025 
* 342eca6f3d1 update untitled 
* b0a6d4711a3 coreboot413: add alper's fix to cbfstool for gcc15 
* 628ae867c9a flashprog: bump to rev e060018 (1 March 2025) 
* 5e96db5a2b4 further gcc-15 fix for gmp on -std=23 
* 9a9cd26b2d5 coreboot/default and fam15h: gmp fix, gcc15 hostcc 
* 80007223c85 lib.sh: Provide printf for mktarball 
*   a16c483e5fd Merge pull request 'coreboot: fam15h: Add patches to fix build with GCC 15 as host compiler' (#318) from alpernebbi/lbmk:coreboot-fam15h-gcc15 into master 
|\  
| * 685685ab0e4 coreboot: fam15h: Add patches to fix build with GCC 15 as host compiler 
|/  
*   02110f2bc1d Merge pull request 'coreboot: Add patch to fix build with GCC 15 as host compiler' (#317) from alpernebbi/lbmk:coreboot-gcc15-nonstring into master 
|\  
| * 5ad1de3931a coreboot: Add patch to fix build with GCC 15 as host compiler 
|/  
*   9e7bceb7fa9 Merge pull request 'seabios: Fix malloc_fn function pointer in romfile patch' (#313) from alpernebbi/lbmk:seabios-romfile-malloc-fptr into master 
|\  
| * 35c853f8b33 seabios: Fix malloc_fn function pointer in romfile patch 
* |   686e136f150 Merge pull request 'dependencies/debian: Fix libusb package name' (#315) from alpernebbi/lbmk:debian-libusb-dependency into master 
|\ \  
| * | 6f120f01588 dependencies/debian: Fix libusb package name 
| |/  
* / d8b0e749983 init.sh: fix yet another double quote for dotfiles 
|/  
*   780844112ae Merge pull request 'Update U-Boot to v2025.10' (#305) from alpernebbi/lbmk:uboot-v2025.04 into master 
|\  
| * 1265927ca38 u-boot: gru: Disable INIT_SP_RELATIVE 
| * 5bea1fade9a u-boot: arm64: Expand our modified defconfigs to full configs 
| * fd56d8ada13 u-boot: arm64: Merge our modifications into new defconfigs 
| * ed9ddd7415f u-boot: arm64: Add new upstream defconfigs 
| * b1fa44858cb u-boot: arm64: Rebase to v2025.04 
| * 976fc6890ae u-boot: arm64: Save our modifications to the upstream defconfigs 
| * 418570a6172 u-boot: arm64: Turn configs into defconfigs 
|/  
* 093a86d9c09 init.sh: don't use eval to read version files 
* 3045079947b init.sh: use backslash for dotfiles in eval 
* da108d1c045 mk: Don't run mkhelpers if mode is set 
* 71a58a38ab4 mk: condense main() again 
* f3882b9bf21 init.sh: make git name/email error more useful 
* 9cebda333d5 init.sh: move git name/mail check to xbmk_git_init 
* ea081adc4ca init.sh: tidy up the git name/email check 
* 3292bded692 mk: make main() more readable 
* 97a5e3d15ed mk: move git check to init.sh xbmk_set_version 
* 11cd952060d init.sh: tidy up xbmk_init() 
* f6c5c8d396d mk: move git_init to init.sh 
* ec1c92238cc init.sh: minor cleanup 
* e009f09e7fa init.sh: clean up setvars 
* 9ec72153408 init.sh setvars: make err a printf for eval 
* 18ad654a1f7 init.sh: merge xbmk_child_init with xbmk_init 
* 15268202478 init.sh: split xbmk_child_init into functions 
* 0280cd4c0e7 init.sh: move parent fork to new function 
* a0e1d42ff74 init.sh: Provide more complete error info 
* a8f0623efbb update uefitool to rev a072527, 26 Apr 2025 
* c698972130f rename include/vendor.sh to inject.sh 
* 24e488aae56 lib.sh: move _ua to the xbmkget function 
* 6779d3f9915 move variables out of init.sh to others 
* 848159fa0eb lib.sh: rename vendor_checksum 
* 1de77c6558c lib.sh: move singletree() to git.sh 
* 703fe444312 lib.sh: move cbfs() to rom.sh 
* b57952e90d2 re-split include/init.sh to lib.sh 
* 8ecb62c6628 rename include/lib.sh to init.sh 
* ce4381169fa lib.sh: introduce more top-down function order 
* 15b64cfebe8 mk/git.sh: remove tree_depend variable 
* 9b8179c0e5d git.sh: remove unused xgcc linking feature 
* 4624c6e536c mk: remove unused variables (ser/xp) 
* aba5b3a3532 mk: simplify main() 
* 0ab7c6ff9cf lib.sh: use realpath to get sys python on venv 
* 8edea026c58 lib.sh: Force use of System Python to prevent hang 
* b1b964fa5c3 lib.sh: further condense the python check 
* 9543a325acb lib.sh: further simplify the python check 
* 9baabed7186 lib.sh: condense the python check 
* 0c5c5ffc873 lib.sh: simplify mk() 
* 83022b6ba83 lib.sh: simplify cbfs() 
* 13ad839691d lib.sh: simplify the python check 
* b1ea4165754 mk: remove mkhelp() and use x_() instead 
* 4cf64e59ed0 mk: simplify handling of trees() 
* d0581914c74 coreboot/hp8300cmt: purge xhci_overcurrent_mapping 
* cb52fc4ba82 Fix VBT path on HP Elite desktops 
* 2bee87cfc26 lib.sh: add missing copyright year 
* 4b7ab403c65 ifd/q45t_am: unlock regions by default 
* 564155277ea coreboot/g43t_am3: use ifd-based setup 
* 0ddd1963751 coreboot/q45t_am3: use ifd-based setup 
* 3b2d933842a coreboot/default: add missing submodules 
* a10d81399c7 NEW MAINBOARD: Acer Q45T-AM (G43T-AM3 variant) 
* d114e0a765c mk: don't print confirmation of git pkg.cfg 
* f59c24f12aa coreboot/g43t_am3: fix data.vbt path 
* 21020fa319a add missing config/data/coreboot/0 
*   2b4629d790b Merge pull request 'lib.sh: Fix python3 detection when 'python' is python2' (#290) from alpernebbi/lbmk:python3-detection-fix into master 
|\  
| * a18d287a81e lib.sh: Fix python3 detection when 'python' is python2 
|/  
* c7569a67145 coreboot/next: merge with coreboot/default 
* 762c7ff43eb coreboot/default: Update, c247f62749b (8 Feb 2025) 
* 86e7aa80c51 Update the GRUB revisions 
* 8d57bf6009e Revert "git.sh: minor cleanup" 
* a2898771f6e lib.sh: perform root check even earlier 
* 779f6003421 lib.sh: tidy up opening logic (put it together) 
* bac4be99c20 lib.sh: do root check before python check 
* e63d8dd20d9 git.sh: minor cleanup 
* 11078508a25 lib.sh: simplify mktarball() 
* 087bbedc5f8 vendor.sh: tidy up vendor_download() 
* e11fd52d958 mk: tidy up check_gnu_path() 
* 3442f4278ed mk: simplify check_project_hashes() 
* 6b6a0fa607c lib.sh: fix missing s/TMPDIR/xbmktmp 
* e07a2adb130 lbmk: don't handle TMPDIR directly 
* 9d3b52cd1d2 rom.sh: minor cleanup 
* b4402c54258 vendor.sh: yet even more code cleanup 
* fe5bdc7633d vendor.sh: even more cleanup 
* fcedb17a9a1 vendor.sh: more cleanup 
* 4e2b59ed3ff vendor.sh: minor cleanup 
* a3acf4c3f95 vendor.sh: simplify process_release_roms 
* 30213a96883 vendor.sh: remove unnecessary check 
* 38df7275f12 git.sh: remove unnecessary comment 
* f5891fb6991 git.sh: remove link_crossgcc() 
* a685654b90f git.sh: remove move_repo() 
* e4aa62f79a8 git.sh: remove prep_submodule() 
* 2839feb9e43 git.sh: make git_prep command clearer 
* 410fa702c9c mrc.sh: Make proper use of variable inside printf 
* 075902c3ea7 simplify a few file checks 
* b2255425eba rom.sh: remove unnecessary check 
* 39640d76a75 lbmk: minor cleanup 
* c8dc701f3eb lib.sh mktarball: stricter tar error handling 
* 58a53d7046f vendor.sh: don't err on bruteforce me extract 
* 958fa34832a mk check_project_hashes: handle error on sha512sum 
* 8b4b069e3f6 vendor.sh: remove unnecessary xchanged="y" 
* 166dbb04c92 vendor.sh: set need_files="n" if skipping patch 
* e90657cc734 vendor.sh: Don't handle vendor files if not needed 
* 2e10a45fa36 Revert "lib.sh: use eval for the command in x_" 
* 738d4bb6b6d lib.sh: fix bad eval writing resized file 
* eb9e5d2d5d4 lib.sh: fix bad eval writing version/versiondate 
* 3bfdecdc75b lib.sh: use eval for the command in x_ 
* 4fa3bb9e5b1 mk: use eval to run mkhelp commands 
* 9b3635718a8 mk: tidy up the switch/case block in main() 
* 0c381028abc mk: tidier error handling 
* 023f9cf0498 lib.sh: tidy up the error handling 
* cb3253befb9 rom.sh: tidy up error handling 
* 7af46721bcb vendor.sh: tidy up error handling 
* 04ebb3b91a0 vendor.sh: tidy up decat_fspfd() 
* 0c87fdf96ad git.sh: clean up fetch_project() 
* 9eb8856b3c5 mk: Remove unnecessary argument checks on trees() 
* 52f3d54116f vendor.sh: properly call err_ in fail_inject 
* c4c6692b761 remove xbmk_parent, handle forking in lib.sh 
* fd5431db05d lib.sh: define x_ right after err_ 
* 972681a127b mk: minor cleanup 
* b41cd39b686 lib.sh: minor cleanup 
* 49939502648 mrc.sh: minor cleanup 
* c158d82298b rom.sh: minor cleanup 
* cb36248c8c0 vendor.sh: tidy up check_release() 
* 409cab39c56 vendor.sh: tidy up vendor_inject() 
* 12b1623e473 vendor.sh: tidy up readcfg() 
* 0d85f061e2e vendor.sh: tidy up patch_release_roms() 
* 61f20141028 vendor.sh: tidy up process_release_roms() 
* 5901f36e49d vendor.sh: tidy up patch_rom() 
* 082930ce0e7 vendor.sh: tidy up inject() 
* e1f91f30372 vendor.sh: tidy up modify_mac_addresses() 
* 3181ac50126 script/trees: merge with mk and delete script/ 
* 3d03dd1a507 mk: remove the legacy "roms" command 
* f0c629dcc6c lib.sh: write version/versiondate to dotfiles 
* 23b942c83e9 lib.sh: hardcode projectname/projectsite 
* a03bb793aea remove update/vendor symlinks 
* d7f80ebe71e move build to mk 
* 57d58527fd0 trees: unify the execution of mkhelper commands 
* e5262da4be7 trees: tidy up configure_project() 
* 51798278397 build: make coreboot building an else in "roms" 
* c189257888a trees: don't build dependencies if dry=":" 
* 115a66fddd3 trees: unified handling of flags 
* 3ea633cc791 trees: simplified handling of badhash/do_make 
* 9be40e94a2b trees: don't set mode on ./mk -b 
* 67ad7c2635c trees: don't set mod on ./mk -d 
* 24448948419 trees: don't initialise mode to "all" 
* 97c50a39a60 trees: clean up some comments 
* cfb14fd8dd8 vendor.sh: simplified readkconfig() 
* 5b697b93a2d lib.sh: double-quote pwd to prevent globbing 
* 5a0a24f5559 lbmk: unified PWD handling (work directory) 
* a25a29cfbb7 lib.sh: initialise PATH if it's unset 
* 1022abf6991 move XBMKPATH to include/lib.sh 
* 0764c969a29 lbmk: use pwd util, not PWD environmental variable 
* f98b9b01107 clean up a few semicolons in the build system 
* 8ccb61cc718 trees: err if first argument is not a flag 
* 947c3e1a176 trees: err if no argument given 
* edbbde0b12d trees: set dry=":" on ./mk -f 
* 33bb0ecf764 trees: clean up initialisation of the dry variable 
* c7636ff1dfc trees: initialise mode to "all", not "" 
* d0bd12631a6 trees: don't abuse the mode variable on -f 
* c4cd876c609 trees: Add missing flag to error output 
* 5ebcae5235f lbmk: minor code formatting cleanup 
* 70cef71dbab grub/xhci: Remove unused patch 
* 3f14a470a2e remove _fsp targets (keep _vfsp) 
* d7312260e7e util/nvmutil: remove excessive comments 
* e348ea0381a Bump GRUB revision to add 73 security patches 
*   4b228c11f9f Merge pull request 'Update pico-serprog revision' (#271) from Riku_V/lbmk:master into master 
|\  
| * a8359e30b27 Update pico-serprog revision 
|/  
* d2cb954933b util/nvmutil: Fix bad error messages on R/W 
* e1e515bd22a util/nvmutil: hardened pledge on help output 
*   ada057a865c Merge pull request 'Simplify the README' (#269) from runxiyu/lbmk:readme-simplification into master 
|\  
| * 9ced146b47c README.md: Use newlines instead of bulleted list for docs/support links 
| * 266122592cd README.md: Use the EFF's page on Right to Repair 
| * e36aa8c5a5c README.md: Vastly simplify it 
| * c17f4381ce5 README.md: Mention SeaBIOS and U-Boot instead of Tianocore as payloads 
|/  
*   47eb049cb47 Merge pull request 'deps/arch: genisoimage belongs to cdrtools' (#267) from runxiyu/lbmk:master into master 
|\  
| * fa9a0df2458 deps/arch: genisoimage belongs to cdrtools 
|/  
* a98490573be util/nvmutil: only set mac_updated at the end 
* 6b9cf09ca21 restore old x230 gbe file 
* 8a435355135 util/nvmutil: Fix bad comparison 
* a65a0c2f963 util/nvmutil: allow ./nvm gbe MAC 
* 96356ce94f6 util/nvmutil: move "e" to swap() 
* b1d8975959d util/nvmutil: Only read up to 4KB on larger gbe 
* 6821659bcb2 util/nvmutil: fix minor mistake (line break) 
* 3bb7520f6d9 util/nvmutil: do setmac if only filename given 
* d94b274fd9f vendor.sh: don't error if grep -v fails 
* 6ebdd3c72ba vendor.sh: Don't show gbe filename on inject 
* a08748a9eda util/nvmutil: don't say write not needed if errno 
* 6841a351ebc util/nvmutil: print dump *after* modification 
* da0a6c216cf util/nvmutil: verbosely print the written MAC 
* db5879c6b5a util/nvmutil: minor cleanup in cmd_dump 
* bd7215d1eb7 util/nvmutil: show nvm words written on writeGbe 
* c70117c79c4 util/nvmutil: clean up readonly check on writeGbe 
* cf5a63e65ca util/nvmutil: Remove useless gbeFileChanged var 
* 83601aa524b util/nvmutil: reset errno if any MAC updated 
* 3e86bf5ce25 util/nvmutil: reset errno when writing a MAC 
* bcf53cc2cc0 util/nvmutil: show total number of bytes read 
* c91cc329cf8 util/nvmutil: rename tbw/bw to tnw/nw 
* 90607108330 util/nvmutil: err if bytes read lower than nf 
* c72f699d368 util/nvmutil: err if fewer bytes written 
* d666f67ebe5 util/nvmutil: Show bytes written in writeGbe 
* b2d6393ed5f util/nvmutil swap(): ensure that no overflow occurs 
* 063fef14d34 util/nvmutil: make swap() a bit clearer 
* fd1bbdc96cb util/nvmutil: make 0x3f checksum position a define 
* 5ddf7f251d6 util/nvmutil: make 128 (nvm area) a define 
* 8850acc7da6 util/nvmutil swap(): Only handle the nvm area 
* 49506a88328 util/nvmutil: move write checks to writeGbe 
* 948377b0e7e util/nvmutil: make cmd_swap its own function again 
* 6e134c9f4bf util/nvmutil: minor cleanup 
* 98e105ac4f1 util/nvmutil: allocate less memory for setchecksum 
* 52e8ea57f7b util/nvmutil: Further reduce memory usage 
* 7a7d356824e util/nvmutil: Remove unnecessary buf16 variable 
* cdf23975bc1 util/nvmutil: Only allocate needed memory for file 
* ed45da9cae5 util/nvmutil: Remove unnecessary buffer 
* ec3148dc3b5 util/nvmutil: Show specific error for bad cmd argc 
* 073420d3056 util/nvmutil: cleaner argument handling 
* a6c18734e70 util/nvmutil: extreme pledge/unveil hardening 
* deb307eaf63 util/nvmutil: more minor cleanup 
* c14eccaf153 util/nvmutil: more granular MAC parsing errors 
* 88fb9cc90ea util/nvmutil: more cleanup 
* 5aaf27f80c3 remove errant comment in nvmutil 
* c829b45c17c util/nvmutil: support 16kb and 128kb gbe files 
* a98ca5bf65c util/nvmutil: Prevent unveil allowing dir access 
* 68c32034a00 typo: nvme should say nvm in nvmutil.c 
* c944c2bbac7 util/nvmutil: General code cleanup 
* 8c65e64e398 snip 
* f666652fe15 snip 
* 64d3c7b5150 grub/xhci: Add xHCI non-root-hub fixes from Nitrokey 
* 7bf0d4c2ed5 add gnults-devel to fedora 41 dependencies 
* 66d084e7f7c grub.cfg: scan luks *inside lvm* 
* 5a3b0dab966 grub.cfg: Scan *every* LVM device 
* 3c9f4be76f6 Libreboot 20241206, 8th revision 
* d4cc94d6b44 rom.sh: don't run mkpicotool on dry builds 
* de6d2f556f1 pico-sdk: Import picotool as a dependency 
* 4210ee68ea2 lib.sh: Much safer python version check 
* 8c7ba6131cc coreboot/next uprev: Fix T480 backlight keys 
* 411fb697dfc set up python in PATH, ensuring that it is python3 
* e8336bcc3ca vendor.sh: Proper semantics on prefix file names 
* 63f45782638 vendor.sh: Confirm if need_files=n 
* 13b06ae130f vendor.sh: Allow restoring the default GbE file 
* ab8feff92e0 vendor.sh: set random MAC address *by default* 
* 0ceaa01d45d vendor.sh: add clarification to nogbe warning 
* 4d5caf1dcfc vendor.sh: check that the vcfg file exists 
* fc4ee88e167 vendor.sh: error out if nuking failed 
* 8819a93d89b add line break, part 3 
* 8ce1a00f517 add line break, part 2 
* bc2c14e76a8 add line break 
* c762850311a vendor.sh: prevent double-nuke 
* 68299ad05ca vendor.sh: much more verbose errors/confirmation 
* b8e6d12f3d9 add libx86 to arch dependencies 
* cf8ad497b4e vendor.sh: Remove unnecessary return 
* c858099b359 vendor.sh: Download utils even if vcfg unset 
* ce16856a242 vendor.sh: Allow setmac if vendorfiles not needed 
* 4b51787d078 add less to arch dependencies 
* 8bd028ec153 lib.sh: Set python after dependencies 
* 44b6df7c24c update my copyright years on modified scripts 
* 818f3d630c2 vendor.sh: Don't error if vcfg is unset 
* 432a1a5bca7 lib.sh: Fix unescaped quotes in chkvars() 
* a73b0fd910a Revert "fix more unescaped quotes in eval" 
* ec6bcc1fba5 fix more unescaped quotes in eval 
* 5284f20b981 fix ./mk dependencies build issue 
* d825f9a9683 rom.sh: Remove errant GRUB modules check 
* 4149f3dc81a submodule/grub: use codeberg for 1st gnulib mirror 
* 0305975e705 util/nvmutil: Update AUTHORS and COPYING files 
* 20b192e13bd util/nvmutil: Describe nvmutil in help output 
* d1ca21628cb util/nvmutil: Remove the correct binary on uninstall 
* e63fe256dfc util/spkmodem-recv: More correct Makefile 
* efd50ee548b util/nvmutil: Honour the INSTALL variable 
* 8008838abbc util/nvmutil: Don't clean when doing uninstall 
* 982f257f58a util/nvmutil: Proper DESTDIR/PREFIX handling 
* 3f85ae5f853 util/nvmutil: Set CC and CFLAGS only if unset 
* 2c7b9fb9412 util/nvmutil: Capitalise BABA 
* 57f9906f6d1 util/nvmutil: Add uninstall to Makefile 
* 4defe2c6085 util/nvmutil: Add distclean to Makefile 
* 033e4cd9d50 util/nvmutil: Make the GbE checksum a define 
* 874317c4e59 util/nvmutil: nicer hexdump display 
* a338e585eed util/nvmutil: show the correct hexdump order 
* b032e483ef1 lib.sh mktarball: cleaner if statement 
* 0cf58c22734 fix lbmk shellcheck errors 
* 8276560cc99 lib.sh and rom.sh: update my header 
* 08e86d2218c vendor.sh inject: reset err upon return 
* 41275d699ca vendor.sh: MUCH, MUCH, MUCH safer ./mk inject 
* ed7293494e3 util/nvmutil: Obey the 79-character per line limit 
* 637b5e36fd2 util/nvmutil: Tidy up copyright header 
* cd28db883e2 vendor.sh: fix comment 
* 57971ceb227 util/nvmutil: Fix another straggler 
* 15b37b2a1ab util/nvmutil: Tidy up pledge calls 
* e8799310db2 hp820g2: fix vendorfile inject and set release=y 
* f9ab082ec19 fedora41/dependencies: add libuuid-devel 
* 661591f9f0b add uuid-devel to fedora41 dependencies 
* 1a46c047386 support ./mk dependencies fedora reinstall 
* d58d63569f1 fix missing semicolon in grub nvme patch 
* 95ea3293df5 bump seabios to rev 1602647f1 (7 November 2024) 
* 6d7e6c361b3 Bump GRUB revision to 6811f6f09 (26 November 2024) 
* 09a01477df6 t480/3050micro: force power off post power failure 
* d344cd95eac flashprog: Disable -Werror 
* dc95e912bfe bump flashprog to revision eb2c041 (14 Nov 2024) 
* 27c8c1c16ba replace liblz4-tool with lz4 and liblz4-dev 
* d3a732a64db lib.sh dependencies: support --reinstall argument 
* 466ada423dd move xbmkpath to XBMK_CACHE/ 
* b0a23840327 Revert "Remove legacy update/vendor commands" 
* 3d7dd4aa9fe Fix U-Boot build issue with Swig 4.3.0 
* 0c810747469 use command -v instead of which 
* 6c7e3ce2d6e trees: remove unnecessary subshell 
* ad137eae89d trees: only symlink host gcc/gnat to build xgcc 
* cfb6de94c33 trees: correction on check_gnu_path 
* ec2f0716662 trees: match gcc/gnat versions both ways 
* f64b5996279 Merge path.sh into script/trees 
* 295463d281e path.sh: Further cleanup 
* 5b24e0a5a96 path.sh: More thorough gcc/gnat version check 
* 7849a075886 path.sh: minor cleanup 
* 17168a87dbf path.sh: remove unnecessary shebang 
* e565df94fd7 Fix globbing issue in lbmk 
* c80cc0a00b6 remove auto-confirm on distro dependencies 
* 01fc65a0a9d Mitigate Debian Trixie/Sid GCC/GNAT version mismatch 
* 424b0c7103b t480/3050micro: disable hyperthreading 
* 603105f3b4e t480/t480s: Disable TPM2 to mitigate SeaBIOS lag 
* 754bd1e6ca3 rom.sh: Name pico directory serprog_pico 
* db22308eba5 add 2024 to Riku's copyright header on rom.sh 
*   4fa5f696db8 Merge pull request 'rp2530' (#258) from Riku_V/lbmk:rp2530 into master 
|\  
| * a5e0360992d pico-sdk: update to 2.1.0 
| * e2f8cc7f3ee pico-serprog: enable building for multiple pico chips 
|/  
* ccc2b4d589f add spdx headers to dependencies configs 
* a3969701e6b dependencies/debian: fix debian sid 
* 8f370cb60d9 add spdx headers to various config files 
* d591ea4c5dc git.sh: don't initialise livepull globally 
* b5da9feba3b vendor.sh: Print useful message on ./mk inject 
* 12c6259cb2f vendor.sh: Handle FSP insertion post-release 
* 78132051462 Remove legacy update/vendor commands 
* 07037561bd6 lbmk: remove use of deprecated ./vendor command 
* 5d1f1823067 vendor.sh: Safer exit when vendorfiles not needed 
* a18175a5df9 data/deguard: Remove unused patch 
* ee8f53b96ff lib.sh: Safer exit from ./mk dependencies 
* a8b35c88cf1 remove geteltorito and mtools from lbmk 
* 1dd32ea5487 rom.sh: support grub-first setups 
* f7801ef4770 vendor.sh: delete old tb.bin first, just in case 
* 02cbf8a729d vendor.sh: make TBFW pad size configurable 
* 9884e5ed1b0 T480/T480S: Support fetching ThunderBolt firmware 
* 36b42dd1c11 also de-rainbow the u-boot menu 
* eafc82028a4 Revert "use rainbow deer on the grub background" 
* 44969c73bd2 rom.sh: insert grub background in cbfs not memdisk 
* 401efb24b22 use rainbow deer on the grub background 
* dc27cb91784 add some scripts to .gitignore 
* 3b6b283eabe disable 3050micro nvme hotplug 
* c2023921893 fix t480 spd size (512, not 256) 
* da527459b68 add tarballs and signatures to gitignore 
* b910424b5df fix another very stupid mistake 
* e3b77b132e6 fix the stupidest bug ever 
```

This is about 650 changes.

Revision releases
-----------------

When certain bugs are found, releases may be re-built and re-uploaded. When
this happens, the original release is replaced with a *revision release*.

Revisions are numbered; for example, the first post-release revision
is *rev1*.

### 25.06 (30 June 2025)

No revisions, thus far. The original 25.06 release is the current revision, so
it could be considered *rev0* (revision zero).

Build testing
-------------

This release was built on the latest Debian 12.10 Bookworm release, as of
this day. It was also build-tested successfully on the latest Arch Linux updates
as of 26 June 2025.

