% Libreboot 25.06 "Luminous Lemon" released!
% Leah Rowe
% 30 June 2025

A corresponding [Canoeboot 25.06](https://canoeboot.org/news/canoeboot2506.html)
release is also available.

Today's Libreboot 25.06 revision is a *stable release*, whereas the previous
stable release was Libreboot 20241206; the previous *testing* release was
Libreboot 25.04, and this releases fixes a few bugs. The codename for this release
is *Luminous Lemon*.

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

This release was built on the latest Debian 12.10 Bookworm release, as of
this day. It was also build-tested successfully on the latest Arch Linux updates
as of 26 June 2025.

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

New schedule and version numbers
-------------------------------

This change in version release scheme was previously reported in the news
post announcing Libreboot's new [formal release schedule](schedule.md). As per
that article, the next stable release will be Libreboot 25.06, in June 2025.

Regarding the changelog
--------------------

The Libreboot 25.04 releases were essentially RCs (release candidates) of
the Libreboot 25.06 release, given the [revised release schedule](revisions.md).

The focus since Libreboot 25.04 has been on fixing bugs, while making as few
breaking changes as possible, ideally none; this also means that fewer boards
were added, and fewer features were added. This is because major works are
done right up until RCs, about two months before official release, and then
bugs are fixed between then and the release; this changelog reflects that.

The [original schedule](schedule.md) created *four* annual releases: YY.04, YY.06,
YY.10 and YY.12. 04 and 10 being testing releases, leading up to 06 and 12. These
numbers refer to the month of the year, and YY represents the year.

The revised schedule changed this to *two* releases, with RCs that get deleted
after the stable release comes out; 25.04 shall be retained on rsync, but it
is retroactively (and informally) referred to as "Libreboot 25.06 RC0". This
explanation has been provided, for context.

Open source BIOS/UEFI firmware
----------------------------

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/grub.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/grub.jpg" /></span>

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

Summarised list of changes
-------------------------

### Board support

The following boards have been added since the Libreboot 20241206 release:

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

The GRUB update brings in several changes from upstream:

```
* 73d1c959e cryptocheck: Add --quiet option
* dbc0eb5bd disk/cryptodisk: Wipe the passphrase from memory
* 301b4ef25 disk/cryptodisk: Add the "erase secrets" function
* 23ec4535f docs: Document available crypto disks checks
* 10d778c4b commands/search: Add the diskfilter support
* 7a584fbde disk/diskfilter: Introduce the "cryptocheck" command
* ed691c0e0 commands/search: Introduce the --cryptodisk-only argument
* c448f511e kern/rescue_reader: Block the rescue mode until the CLI authentication
* 4abac0ad5 fs/xfs: Fix large extent counters incompat feature support
```

### Licensing

* no changes (relative to Libreboot 25.04)

### Security

These can be considered bug fixes, but these are special fixes that are of
massive concern to users.

* This GRUB change was merged, in the aforementioned revision
  update: `dbc0eb5bd disk/cryptodisk: Wipe the passphrase from memory` - this
  wipes the LUKS key from memory, after GRUB exits, where one was created
  by GRUB while unlocking a given volume.

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

### Bug fixes

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi/wson8/0003.jpg" /><span class="f"><img src="https://av.libreboot.org/rpi/wson8/0003.jpg" /></span>

Most of these are build system fixes, but for example there is also a fix
applied to the GRUB source code.

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

Git log
-------

This log is relative to Libreboot 25.04:

```
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
```

Revision releases
-----------------

When certain bugs are found, releases may be re-built and re-uploaded. When
this happens, the original release is replaced with a *revision release*.

Revisions are numbered; for example, the first post-release revision
is *rev1*.

### 25.06 (30 June 2025)

No revisions, thus far. The original 25.06 release is the current revision, so
it could be considered *rev0* (revision zero).


