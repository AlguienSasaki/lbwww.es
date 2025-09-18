% Libreboot Build System Audit 6
% Leah Rowe
% 19 July 2024

Heavy amount of code reduction in this audit, and general cleanup. A new
Libreboot release is planned, for the early days of August 2024.

Efficient software freedom
--------------------------

Libreboot is a free/opensource boot firmware project. It replaces your
proprietary BIOS/UEFI firmware, on supported x86 and ARM computers. It does
this by providing an automated build system to download, patch and compile
the various upstream sources (e.g. coreboot, GRUB, SeaBIOS). Coreboot is used
for hardware initialisation, configuring everything from your CPU, memory
controller all way to peripherals, readying the hardware so that it can run
software, e.g. Linux/BSD operating systems. You can essentially think of *lbmk*,
which is Libreboot's build system, as a *source-based package manager*. It is
what the Libreboot releases are built with. The *lbmk* build system essentially
implements a *[coreboot distro](../docs/maintain/)*,
the same way you might think of a Linux
distribution; it systematically downloads, resets (to specific revisions) and
patches various upstream project sources such as coreboot, U-Boot and GRUB,
automatically building entire coreboot images. This build system is what creates
Libreboot release images and source tarballs.

Extensive auditing has been performed on lbmk, since the Libreboot 20240612
release. These audits fix bugs, reduce code bloat and generally improve the
efficiency of lbmk, adding and removing features in a careful, *conservative*
way, with a focus on *clean code*.

Remember the magic words: code equals bugs. Fewer lines of code will yield
fewer bugs.

This article covers changes from Libreboot 20240612, up to
revision `31f1e4dadfcc1ceecacec50dd2a14e63a44364bd` from 19 July 2024.

You can read about the *previous* audit in the article
for [Libreboot Build System Audit 5](audit5.md).

### Notable code size reduction

There are 1109 lines of shell script in the build system, versus 1482 in the
Libreboot 20240612 release. Libreboot's build system is written *entirely* in
POSIX sh; not BASH, not KSH, not ZSH, jush sh! It is the *build system of
build systems*; the design focuses on re-using (read: abusing) the build systems
of the projects it uses, avoiding as much bloat as possible. More information is
available in the [lbmk build system documentation](../docs/maintain/).

This is a difference of 373 lines, or a *25% reduction*. The main focus has been
on removing legacy bloat from the build system, vastly increasing the overall
code cleanliness and efficiency. Several new safety and redundancy features were
added, such as auto-rebuild when configs change(and auto-redownload) for sources,
and additional redundant handling of both repositories and file downloads, cached
during operation to avoid over-use of internet bandwidth.

Plan for next release
---------------------

This code reduction has been the main focus; the focus now, at the next release,
will be new payloads and boards, in addition to testing newer upstream revisions
of projects such as coreboot, on every machine supported by Libreboot. A release
is planned for early August 2024.

A *lot* of work on new ports is planned. There are a number of new motherboards
that will be available, in the next Libreboot release.

Summarised list of changes
--------------------------

The most interesting changes are marked in **bold**. "Interesting" means that
the change greatly improves the usefulness/reliability of Libreboot, or that it
affects the user in a profound and noticeable way.

Changes are in order per category, from newest to oldest:

### Feature changes

Some unused features have been removed, and yet more added. The overall focus of
Audit 6 has been to remove legacy cruft from lbmk, and in general to simplify
the operation of lbmk, Libreboot's build system. Several new safety features
were implementated for this audit, in addition to features that generally
improve the redundancy of certain operations, and overall reliability in lbmk.

The changes are as follows:

* **Put the cache directory location in an environmental variable,
  named `XBMK_CACHE`.** If unset, it defaults to `$PWD/cache`, where PWD is the
  current location that the instance of lbmk is running from, otherwise if
  defined, the location is used. This means that the same cached files are
  re-used on the lbmk copy created when running `./update release`, thus
  saving on a lot of internet bandwidth if the main instance already has
  a lot of files cached - it also means that you can re-use another cache
  from elsewhere, like if you have multiple clones of lbmk on your system.
* Allow using coreboot's own build system, to build payloads. Libreboot's
  build system builds and adds payloads, with coreboot configurations
  enabling `CONFIG_PAYLOAD_NONE` exclusively. However, advanced users may
  wish to use something else such as Tianocore, which Libreboot may/will not
  provide (with Tianocore it's **will not**). Simply set `build_depend=""`
  in the `target.cfg` file for a given motherboard, and then enable a payload
  under coreboot's menuconfig interface, or by direct modification of
  the defconfig file. When `CONFIG_PAYLOAD_NONE` is not set, lbmk will skip
  adding a payload, because it's a given that then coreboot's own build system
  would have done it. Libreboot will always enable `CONFIG_PAYLOAD_NONE` on
  all official targets, so this feature is only intended for power users.
* **Unified cache file handling** for vendor files, and files defined as modules
  for specific projects. They are now cached within the same base directory.
* **`script/trees`: Automatically re-download and re-build a given project, when
  configuration changes**, on any file under specific directories in `config/`
  pertaining to it; this is done by concatenating the sha512sum hashes for
  each of those files, and then making a sha512sum hash from that. The hash
  is stored, and compared to the current one; if it differs, then you know
  something changed. This feature does not track modifications to the actual
  downloaded source code for a given project, because this change also tracks
  the `patches/` directories; direct modification (by the user) of the source
  is not assumed, under the lbmk model, but checking for this may be possible.
  For example, run the same concatenation logic on a file containing the output
  of commands such as `git describe` and `git status`, perhaps `git diff`. It
  was decided that the latter would be undesirable, because *developers* (of
  Libreboot, working with lbmk) may want to experiment with quick changes,
  before preparing patch files to go under `config/`. - the user previously
  had to know which files and directories to manually remove, when re-running
  build commands after modifying a given project within lbmk. Now it's automatic.
* `include/git.sh`: Don't download multi-tree projects to `src/project/project`,
  e.g. `src/coreboot/coreboot`, when downloading the upstream repository, for
  later copying to the actual destination e.g. `src/coreboot/default` - instead,
  use the same cached location as alluded to in the two changes below. This
  avoids needing to delet those directories, when preparing release archives.
* `include/git.sh`: Complementing the change seen below, also cache *files*
  universally, for vendor files and for submodules files (e.g. crossgcc files).
  This avoids wasting bandwidth when building for multiple coreboot trees, in
  cases where multiple trees share the same URLs for crossgcc tarballs.
* **`include/git.sh`: Cache all git downloads**, reset to `HEAD`, before copying
  to the final location; this is also done on submodules. If a cached repository
  exists, run `git pull` on it but without an exit under fault conditions; if
  a fault occurs, including when trying to reset on a copy of it, fall back to
  the old logic where the repository is downloaded (online) directly to the
  destination. This change results in substantially reduced internet bandwidth
  usage, especially when downloading submodules for multiple coreboot trees,
  in cases where those trees share common revisions.
* **`script/trees`: Support `-d` (flag) for custom build logic**; it does the same
  thing as `-b`, but with a variable named `dry` being set to `:`; this is
  prefixed on most build-related commands, and can be further prefixed as
  desired, in mkhelper functions. The result is that you can do *dry runs* on
  a given build. This is used, to provide building of coreboot utilities and
  downloading of vendor files, without needing specific hacks to be hardcoded
  in `script/trees`. The `./update trees -b coreboot TREE utils` command is
  no longer available; instead, do `./update trees -d coreboot TREE`; if
  the TREE argument is instead an actual motherboard target, it also does the
  vendor file download, if required. The `./vendor download` command is still
  available, and multiple board names can now be provided as argument, because
  for example, `./build roms x220_8mb x230_12mb` would
  run `./update trees -d coreboot x220_8mb x230_12mb`, and the trees script
  already supports running for multiple target names, within multi-tree
  projects. The result of `-d`, combined with use of mkhelper functions,
  permits very flexible use of the `trees` script, essentially allowing it
  to be extended on a plugin-type infrastructure, in any number of ways. The
  reason for implementing this is precisely so that project-specific hacks can
  be removed; the `trees` script now contains much more generalised logic,
  and project-specific logic was moved to mkhelper functions.
* **`script/trees`: A `premake` variable is supported in `mkhelper.cfg`** files,
  defining a function to be executed *before* running `make`, on a given project.
* **`script/trees`: A `postmake` variable is supported in `mkhelper.cfg`**
  and `target.cfg` files, defining a function to be executed immediately after
  running `make`, on a given project. This is used during coreboot builds, to
  add payloads on a given coreboot image. It can be used for any other purpose.
  More specifically: `postmake` is executed after handling the defconfig file
  entirely, rather than just after running `make`; the latter is handled
  by `mkhelper`.
* `script/trees`: Support *build* dependencies, complementing the
  existing *fetch* dependencies. The `build_depend` variable can define
  which other sources need to be built, before building the current target.
  This is being used on coreboot targets, to manually and surgically specify
  which GRUB and SeaBIOS trees should be compiled, *per* coreboot target.
  A fallback declaration is specified in `config/data/coreboot/mkhelper.cfg`,
  for cases where a `target.cfg` file does not specify it. This change means
  that certain hardcoded *build dependency* logic was then removed, instead
  relying on this much more generalised `build_depend` logic.
* `script/trees`: Allow global configuration on multi-tree projects, via
  file `config/data/PROJECT/mkhelper.cfg`,
  e.g. `config/data/coreboot/mkhelper.cfg`. This is loaded before `target.cfg`.
  That way, global configuration can be provided and then overridden.
* `script/trees`: If a valid flag is passed, but without argument, a loop is
  now executed with that flag on every defined project, both single- and
  multi-tree. For example, if `-f` is passed, it will download every project.
  If `-b` is passed, it will build every project. When it passes over multi-tree
  projects, it operates on every tree within that project. This fact enabled
  other parts of the build system to be simplified; for example, the logic
  in `mkrelease()` no longer needs to implement the same kind of loop, instead
  simply running `./update trees -f` to download every project and tree. Similar
  changes have been made, on that same basis.
* `script/trees`: Support use of a variable named `mkhelper`, first defined in
  project `target.cfg` files. This variable defines the name of a function, to
  be called immediately after running `make` for that project, under
  the `run_make_command()` function. This was initially implemented, so that
  GRUB's `grub-mkstandalone` utility could be called from the `trees` script,
  but in a way that doesn't hardcode such execution. This is because part of
  the audit's main focus is to remove project-specific hacks, generalising a
  lot more logic. Prior to Audit 6, the `trees` script contained a lot of hacks
  for specific projects, notably coreboot and GRUB, to compile them properly.
* `script/trees`: Only permit one single-tree project to be built, where an
  argument is provided. This script previously supported building multiple
  single-tree projects, defined *by argument*, but it was quite error prone
  and there's no clean way to otherwise do it. We don't use the script this
  way, anywhere in lbmk, and users are advised the same.
* **`script/roms`: *Only* Support SeaBIOS and Sea*GRUB*, on x86 motherboards**.
  SeaGRUB is a configuration whereby SeaBIOS starts first, but immediately tries
  to load GRUB from the flash. This complements the other change, listed below.
  We will no longer provide configurations where GRUB is the primary payload,
  precisely to mitigate the same issue as described below (lbmk issue 216).
  If *GRUB* is enabled, on a given motherboard, SeaBIOS-only setups are not
  provided; only SeaGRUB is provided. You can press ESC in the SeaGRUB menu,
  to access other boot methods besides *GRUB from flash*, so you can use it
  in the same way; additionally, you can remove the `bootorder` file from CBFS
  if you wish, or change the bootorder file, in such a way that GRUB does not
  load first. For example, you might want a BSD installation, and BSD
  bootloaders should be loaded from SeaBIOS directly. This can be done as
  follows: `cbfstool libreboot.rom remove -n bootorder` - then flash it. The
  SeaBIOS code is much smaller and more robust than GRUB, so it's a safe choice
  of primary payload, and it's extremely unlikely that SeaBIOS would ever break.
* `script/roms`: Removed support for `grubonly` SeaBIOS. This was a feature
  where SeaBIOS would try to load GRUB first (from CBFS), but also disable the
  SeaBIOS menu. We still provide SeaGRUB, but the SeaBIOS menu is always
  accessible now. This is to mitigate future issues similar to [lbmk
  issue 216](https://codeberg.org/libreboot/lbmk/issues/216). The
  Libreboot 20240612 already removed actual configs using `grubonly`, and we
  will no longer provide them in Libreboot, so this feature in lbmk became
  legacy cruft that can simply be removed. The idea is simple: *safety first*.
  This has implications for [GRUB hardening](../docs/linux/grub_hardening.md);
  you can insert the correct bootorder file, to once again disable SeaBIOS, but
  you must first verify that GRUB is stable. The GRUB hardening guide already
  documents how to do this.
* `script/roms`: Don't insert `timeout.cfg`. Instead, hardcode the GRUB timeout
  to a reasonable default (8 seconds). You can still insert your own timeout
  config as before, and GRUB will honour it.
* `script/roms`: Insert the background image in GRUB's memdisk, instead of
  coreboot's CBFS. This reduces the amount of code, because we use it as an
  argument on the `grub-mkstandalone` command, and it means that the GRUB
  images can be used standalone in other build systems. The image is very
  small, so it's OK to do it this way.
* `script/roms`: Removed extra checks on `grub_scan_disk`, because we no longer
  allow it to be overridden, instead relying only on `target.cfg` files, which
  are carefully crafted per board. This setting configures the order of which
  types of devices Libreboot's `grub.cfg` scans first, between device types:
  PCI-E/NVMe, SATA/AHCI and IDE/ATA. Mate Kukri added NVMe SSD support no GRUB,
  in the Libreboot 20240612 release.
* `script/roms`: Removed dangerous `-p`, `-s` and `-d` options. Due to lbmk
  issue 216 (GRUB memory issue) in Libreboot 20240504, several safety
  mitigations have been made, for anti-brick purposes. These options enabled
  use of a custom payload and keymap, which meant that users were able to more
  easily create a setup affected by 216. See:
  <https://codeberg.org/libreboot/lbmk/issues/216> - this was fully mitigated,
  by virtue of configuration, but the issue itself is not yet fixed as of the
  day of today's announcement, the one you're reading now, so it's still
  possible to create such a broken setup. Libreboot mitigates this fact, by
  avoiding such configurations.

### Configuration changes

This pertains to anything under `config/`, for any changes that are of note,
but it does not pertain to *revisions* for specific projects, nor does it
cover specific patches applied to projects; the latter will be covered in other
sections of this audit announcement.

The changes are as follows:

* coreboot: Set `postmake` in `config/data/coreboot/mkhelper.cfg`, defining
  a function that adds payloads to the coreboot image that was just built.
  This mkhelper config also integrates `include/rom.sh`, containing these
  functions. This replicates the functionality originally provided
  by `script/roms`.
* coreboot: Set `build_depend` on `target.cfg` files for specific motherboards.
  This is used to manually specify which GRUB and SeaBIOS trees should be
  compiled, required when compiling for a specific target, for the next
  stage where a payload is added to the coreboot image, because lbmk does
  not use coreboot's own payload logic, instead building and adding everything
  itself (the coreboot configs all enable `CONFIG_PAYLOAD_NONE`). For cases
  where a `target.cfg` file does *not* specify `mkhelper`, a fallback default
  entry is dictated within `config/data/coreboot/mkhelper.cfg`.
* **GRUB: Only load the `xhci` module from `grub.cfg`**; the `install_modules`
  variable now defines `xhci`, instead of `modules` defining it. This means
  that the xHCI module is no longer automatically loaded during GRUB machine
  initialisation, but it's then loaded immediately when the GRUB shell starts,
  and the `grub.cfg` file is first parsed, then executed. This mitigates a
  previous issue where booting into GRUB with a USB device connected sometimes
  made GRUB hang; this issue affected Libreboot 20240612, on Haswell thinkpads,
  and revised images (and a revised source tarball) was then issued for that
  release, containing the very fix described here.
* coreboot: Define `makeargs` in `config/data/coreboot/mkhelper.cfg`. It was
  not practical to define this, and other settings, in each `target.cfg` file
  due to how many targets there are, but the mkhelper feature makes this much
  easier. To mitigate the previous issue, the `trees` script hardcoded
  coreboot makeargs in a special variable, `cbmakeargs`; this variable has
  since been removed, because it's no longer used.
* GRUB: Define common build options in `config/data/grub/mkhelper.cfg`,
  instead of repeating them in `target.cfg` files for each GRUB tree.
  The `mkhelper.cfg` feature was added, defined in the Features section above,
  which is loaded per-project on multi-tree projects, before each target
  file. It allows easier configuration tree-wide on multi-tree projects.

### Bug fixes

There are fewer *acute* bug fixes in Audit 6, because bugfixes was the primary
focus of *Audit 5*. The word *acute* is used, referring to triggered bugs, because
many of these changes are *pre-emptive* bug fixes, *preventing* future bugs that
have been foreseen, based on future modification to existing logic. The focus of
Audit 6 has been general code reduction, feature simplification and improvements
to safety features / error handling (many pre-emptive bugfixes implement newer
safety features, but are listed as pre-emptive fixes, not features).

The changes are as follows:

* `script/trees`: Hardcode `makeargs` for crossgcc specifically, by using
  another variable (within that script) called `xgccargs`. This prevents
  the build threads being default (one thread), ensuring that we set it to
  the value as defined by `XBMK_THREADS`. The bug was triggered when building
  the U-Boot images, because U-Boot does not specify `CPUS=` on makeargs.
* **`include/git.sh`: Re-try `git pull` three times**, when updating the cache
  Git repository for a given project or submodule. This mitigates possible
  instability in the user's internet connection, where it's online but may
  otherwise experience jitter (the same issue could also occur on the internet
  connection hooking up whichever server lbmk is interacting with, such as
  the coreboot web server).
* U-Boot (patch): Courtesy of Alper Nebi Yasak, a mitigation is provided in
  U-Boot working around the lack of support for clock timings on eDP displays;
  the clock setting was removed in the devicetree, because display works without
  it, but with it, display breaks because drivers do not yet support it.
* U-Boot (patch): Courtesy of Alper Nebi Yasak, a fix was made that prevents
  breaking the build process for U-Boot, when building binman images. In lbmk,
  we do not use the BL31 firmware from U-Boot itself, because coreboot does
  this, so it's patched out in Libreboot and we defer to coreboot's logic.
* `script/trees`: Don't continue, when no argument is given. `main()` used to
  be the only funsction that runs, in this script, but now we source a config
  file afterward, and run the build after that. Whan a flag is provided without
  OPTARG, this means that we are continuing such action in error; in practice,
  it meant that nothing was done because the command was set by `mkhelpercfg`
  and `cmd`, which would be empty in this situation, but it might cause issues
  depending on the `sh` implementation, so this fix is pre-emptive.
* `include/`: Remove `+x` permissions on these files, because they must never
  be executed directly; they are stubbed from `build` and `script/trees`. This
  is listed as a bug fix, because having executable permission on these files
  is a potential security issue, depending on how lbmk might be modified in
  the future, so it's a pre-emptive fix.
* `include/rom.sh`: Only build coreboot utilities *before* building coreboot
  itself. This prevents `coreboot.rom` from being wiped out, when building it,
  due to idiosyncrasis in coreboot's own build system. This is a pre-emptive
  fix, because the issue didn't really cause actual trouble in practice,
  except under certain test conditions that are unlikely in real use.
* `include/vendor.sh`: Properly quote the filename, in a check for `ec.bin`,
  on the `extract_kbc1126ec()` function - in practice, this caused no issue, so
  this is a pre-emptive bug fix, but quotes are now used to prevent globbing.
  Overlooked in a previous audit, where anti-globbing was actually a priority
  in that audit.
* `include/vendor.sh`: Skip serprog targets, if defined, because these are
  not coreboot and they are built using separate logic, but they are placed in
  the same directory within release archives. This prevents an exit under
  fault conditions, instead causing a regular exit (zero status). This was never
  triggered during regular builds, because of other conditions checked
  throughout, but some users may run the `inject` command, which this fix is for.
  The issue was previously fixed for `download`, but overlooked for `inject`.
* `include/vendor.sh`: Don't inject `/dev/null` into an image. In some cases,
  a given board may define empty option ROMs which refer to `/dev/null`. This
  is used during the coreboot build process, in cases where we want such images
  inserted, but not handled by vendor scripts. Specifically: PIKE2008 images
  were inserted empty, for KGPE-D16, by re-using the *VGA ROM* options in the
  coreboot build system. The `vendor.sh` logic also uses that same config item,
  which was tripping up on these boards. Simply skip insertion (in `vendor.sh`)
  file location is `/dev/null`. This prevents errors during the build process,
  in some limited cases.
* `include/lib.sh`: Much stricter checks in `chkvars()`; the variable string
  may have been set, but otherwise empty. This function was only checking `+x`
  via variable expansion, which only yields negative if the variable was never
  declared. The intention is to check both unitialised and empty strings, per
  variable. This is a *pre-emptive* bug fix, because this bug did not seemingly
  cause any issues in practice, but a bug is still a bug.
* **`include/vendor.sh`: Re-added the `modify_gbe()` function**, seldom used but
  nonetheless required; it's called when the user specifies a MAC address,
  during vendorfile insertion on release images. Direct use of `nvmutil` is
  still recommended, simply to gain an understanding of how it works; the MAC
  address feature in `vendor.sh` simply runs `nvmutil`.
* `script/roms`: Made U-Boot be built *before* checking `ubootelf`, instead
  of after. The check was flawed, because if U-Boot didn't exist, but a given
  build would be vzerified by the first check, the check would still fail after
  build, because the check works by first checking the existence of a given
  U-Boot build file, deferring to another and then exiting if that doesn't
  exist, with error status. This is a *pre-emptive* bug fix, because this
  bug wasn't triggered in practice since actual targets already used the
  fallback build, not the first one. Therefore, this fix is pre-emptive for
  future U-Boot targets in lbmk.
* `include/lib.sh`: Do not exceed 80 characters on the line that checks
  the `versiondate` variable. Such excess is considered a bug.
* `include/vendor.sh`: Rename variable `release` to `vrelease`. Also remove
  the `inject_vendorfiles()` function and merge into `vendor_download()`.
  The `release` variable is included now in some `target.cfg` files, so we want
  to avoid conflicting with it. This is a pre-emptive bug fix, because the bug
  described had not been triggered in practice, but was theoretically possible.
* `script/trees`: Exit with error status if `target.cfg` not provided, on
  multi-tree projejcts. It was already by design that this should occur,
  but we previously did not support `target.cfg` files on single-tree projects.
  Single-tree `target.cfg` support was later added, done by making `target.cfg`
  optional there, but the change accidentally made it optional on multi-tree
  projects. In practice, all multi-tree projects included this file per target,
  so this is a pre-emptive bug fix on the theory that future maintainers might
  accidentally exclude `target.cfg` on a given tree, within a multi-tree
  project.
* `build`: Don't delete `TMPDIR` if it's `/tmp`. We set it to a subdirectory
  under `/tmp`, and subsequent `mktemp` calls create resources under it, but
  it's possible in the future that this functionality might be broken; therefore,
  this is a pre-emptive bug fix, on the (correct) theory that code equals bugs,
  and that regressions can and will occur in the future. One could say that this
  is a *safety feature*; it could just as easily be listed on *Feature changes*.
  This is more strongly a pre-emptive bug fix, as opposed to an actual feature.
  Upon exit from the main parent instance of lbmk, `TMPDIR` is deleted. It is
  set to a subdirectory of `/tmp`, precisely so that we have a unified directory
  location for all temporary files, which then get deleted in bulk (upon exit).
* `script/trees`: Explicitly err if `OPTARG` is not set, for a given flag. We
  otherwise rely on `-e`, but different `sh` implementations may behave
  differently. Therefore, this is a emp-emptive bug fix, on the theory that
  execution may otherwise continue erroneously (depending on the implementation).
  NOTE: this is only listed for documentary reasons, but the script was later
  modified to *allow* a lack of argument, whereby the script would be re-called
  recursively on all projects with the exact same flag, while defining an
  argument throughout.
* `script/trees`: Call err if multiple flags are provided. The script is only
  designed for one flag. Exit with error status if multiple flags are provided.
* `script/trees`: Explicitly set `mode` for flag `-b`, rather than leaving it
  at the default state (empty). This is to mitigate the possibility (from later
  code changes) where other flags might set the string. We need the string to be
  empty, when the `-b` flag is used; this is a pre-emptive bug fix, fixing a
  bug that doesn't actually exist, but rather, preventing such a bug later on.
* `include/lib.sh`: Make the presence of a `projectname` file mandatory, with
  the project name inside. This prevents the theoretical issue where a project
  is nameless, because this file is used during the build process when naming
  release tarballs; theoreticaly, because in practice, lbmk would never remove
  this file, but the build system is designed to be as adaptable as possible.
* **`script/trees`: If the crossgcc build fails, run it again first**, before
  returning with error status. If the second build passes, don't yield an error.
  Sometimes the crossgcc build can fail for whatever reason, but a second run
  will succeed. This avoids having to re-run the *rest* of lbmk, just to re-run
  the crossgcc build. In most cases, the first attempt passes. Coreboot's
  own `buildgcc` script is quite complex, and error-prone.
* `script/trees`: Don't check if crossgcc is already built. The check was done
  based on whether the `xgcc` directory exists, but coreboot's build system
  may have exited with error status before, having not completed all builds.
  The result was that subsequent runs would result in a permanently broken
  crossgcc build, unless the user manually intervened. Instead, lbmk always
  runs the crossgcc build script from coreboot; coreboot itself has a much
  more thorough set of checks, skipping a given build that was already performed,
  but the check is a bit slower, adding a few additional seconds to the build
  time per target. The bug that this fixes was in fact triggered, during random
  testing, leading to the fix.
* `include/lib.sh`: Fixed error when running `./build dependencies`. The `e()`
  and `setvars()` functions were declared *after* the dependencies function,
  but are now declared before. Also: after calling `install_packages`, it
  was doing a return when it should have done an exit. All of this lead to
  certain temporary files being created; the context here was that the user
  would be running `./build dependencies` as root, which lead to some of those
  files being created as root, but lbmk exits with not-zero status when you
  run it as root, except when running the dependencies command. Therefore, this
  could create situations where the user can longer run lbmk without intervention
  such as changing permission on certain files. Avoid the issue entirely.

### General code cleanup

Extensive code cleanup has been performed, as was the main purpose of Audit 6.

The changes are as follows:

* **Removed `util/autoport`**, because the Haswell support that we merged for it
  is now included in upstream(coreboot), so it will be included in future
  coreboot revisions for lbmk.
* `script/trees`: Move dependency building (based on `build_depend`) to a new
  function, called from `configure_project()`, instead of including the logic
  directly within the latter.
* `script/trees`: Merge `build_targets()` into `handle_targets()`.
* `script/trees`: Use `:` as a prefix on certain build commands, when doing
  try runs (dry builds); the `-d` flag was initially implemented with a lot
  of if/else chains, but `:` does nothing in sh (that's what it's for, to do
  nothing), including if arguments are provided to it. This change enables
  much more granually disablement of certain commands, which would otherwise
  require many more if/else chains. This change reduced the amount of code.
* `script/trees`: Remove all project-specific hacks. The coreboot-specific
  includes were moved to coreboot's `mkhelper.cfg` file. Serprog and coreboot
  images are handled by mkhelper functions now.
* `include/rom.sh`: It also provides building of serprog images. The mkhelper
  configuration is provided for `pico-serprog` and `stm32-vserprog`. The
  old `./build roms serprog` commands still work, but you are now advised to
  run `./update trees -b stm32-vserprog` or `./update trees -b pico-serprog`,
  which will now result in the same builds being provided under `bin/`.
* `include/rom.sh`: New file, replacing `script/roms`. It contains the same
  functions, more or less, for building coreboot images. The `trees` script
  already produces coreboot images, and payloads were added by `script/roms`;
  now, those same functions (for adding payloads) are stubbed
  via mkhelper configuration in `config/data/coreboot/mkhelper.cfg`.
  The command `./update trees -b coreboot` (whether specifying additional
  targets, or specifying none and thus building all of them), now builds
  coreboot images with payloads inserted, and never leaves them on-disk without
  payloads; this removes an existing issue where the user might accidentally
  use the no-payload images, mitigated by provided a warning but still an issue
  regardless (for example, what if the user doesn't understand English very
  well?). The old `./build roms` command is now deprecated, but still provided
  for backward compatibility, but you are now advised to run the trees command.
  The `bin/` directory is still the place where coreboot images go, but they
  no longer appear(without payloads) under `elf/` - only the `bin/` images are
  provided.
* `script/roms`: Removed unnecessary calls to `./update trees -b` for payloads,
  because coreboot targets now specify `build_depend` which is used generically
  per tree, per multi-tree project, to provide such build dependencies.
* `script/trees`: Rename function `load_target_config()`
  to `configure_project()`, because the function also now handles building to
  some extent (based on `build_depend`), not just mere loading of config files.
* `include/vendor.sh`: Removed `mkdirs()` and merged its logic into the only
  calling function, `fetch()`.
* `include/git.sh`: Simplified `git_am_patches()` by condensing several `for`
  loops together, into a single `for` loop performing each task.
* `include/git.sh` and `script/trees`: Tidy up the use of global variables.
  Some of them were only initialised in `git.sh` but also used in the `trees`
  script, which means they should be initialised in the `trees` script.
* `include/git.sh`: Simplified initialisation of `loc` (single code line)
* `script/trees`: Simplified `distclean` directory check, by reducing the
  nesting of if/else statements.
* `include/git.sh`: Condensed a few code lines in `fetch_targets()`.
* `include/git.sh`: Shorter `git_prep` command in `fetch_targets()`.
* `script/trees`: Simplified multi-tree bare repository cloning. The `git_prep`
  function already creates a given directory where source code goes, so we
  don't need to handle it from the `trees` script.
* `script/trees`: Merged `prepare_new_tree()` with `fetch_targets()`.
* `script/trees`: Simplified `distclean` handling; condensed a few code lines.
* `script/trees`: Further simplified `copy_elf()`; condensed a few code lines.
* `script/trees`: Added explicit return to the end of `handle_defconfig()`.
* `script/trees`: Renamed `check_config()` to `check_defconfig()`, for clarity.
* `script/trees`: Removed variable `config_name`, because it's only used
  once, so its value was instead hardcoded, without declaring a variable.
* `script/trees`: Renamed variable `config` to `defconfig`, for clarity.
* `include/git.sh`: Removed redundant `xtree` variable, already defined
  in `trees`.
* `script/trees`: Removed various comments that were redundant, because the
  code that they covered was already simple enough that what they did was
  obvious.
* `script/trees`: Cleaned up `handle_makefile()` a bit; condensed a few lines.
* `script/trees`: Renamed `load_project_config()` to `load_target_config()`,
  for clarity.
* `script/trees`: Download multi-tree *bare* repositories *before* given trees.
  When downloading multi-tree projects, revisions can be reset to `HEAD`
  instead of the actual revision for a given target. This occurs when the bare
  repository (e.g. `src/coreboot/coreboot`) does not exist and has to be fetched
  first. Bare repository downloading does not rely on `target.cfg`,
  only `pkg.cfg`, but uses the same variables, e.g. `rev`. So: instead of
  using a separate variable name, do the bare repository download first.
  This means that the `git.sh` logic can be much cleaner, in that it *only*
  copies the bare repository and then runs `git_prep`. The bare repository is
  closed directly by calling the relevant function from `trees`, which is
  therefore the same behaviour as when cloning single-tree projects.
* `script/trees`: The `_setcfgarg` variable was removed, in
  function `load_project_config()` because it became disused and thus redundant.
* `script/trees`: Unified multi-tree and single-tree configuration handling.
  The same functions that load configurations for single- and multi-tree have
  been merged with the one in `git.sh`, and then that was moved back into
  the `trees` script. Now `git.sh` only handles the actual downloading and
  patching of sources, but not configuration of them; the latter is handled
  directly within the `trees` script. The benefit of this change is that it
  generally allows far more flexibility; many functions that use such
  configuration logic can now be unified under this script, for instance
  using the `mkhelper` feature mentioned elsewhere on this page - and that is
  precisely what was later done, in further changes that are detailed above.
* `script/trees`: Unified handling of source downloads. Run it from
  function `load_project_config()`. The logic used to be split, between
  single- and multi-tree projects, but now it is consistent throughout.
* `include/git.sh`: Rename `fetch_project_trees()` to `fetch_targets()`.
* `include/git.sh`: Rename `fetch_project_repo()` to `fetch_project()`.
* `script/trees`: Add explicit return at the end of `check_coreboot_utils()`.
* `include/vendor.sh`: Remove unnecessary check; we don't need to check whether
  the `grub_install_modules` and `grub_modules` variables are set, because they
  always are, and we may want to have a setup in the future where no such
  modules are defined anyway, for testing purposes. Remove this check entirely.
  This pertains to `mkpayload_grub()`, which produces the `grub.elf` payload
  builds.
* `include/vendor.sh`: Remove unnecessary check; we don't need to check
  the existence of the option ROM in `extract_e6400vga()`, because a command
  comes right after it that copies it. If the file doesn't exist, `cp` will
  yield error status, causing the same kind of error returned from lbmk.
* `include/vendor.sh`: Condensed `inject()` a bit, by simplifying some of
  the if/else chains and especially the way in which they return.
* `include/vendor.sh`: Simplified checks for E6400 VGA config; it was being done
  in a complicated for loop running `eval`, but similar functionality is
  already implemented in `chkvars()`, which is now used instead.
* `include/vendor.sh`: Simplified the otherwise over-engineered path checks,
  when reading vendor paths from inside coreboot configuration files.
* `include/vendor.sh`: Simplified handling of the kbc1126 utility in coreboot.
  We were checking whether the binary exists, but `make` already does this.
  We still check whether the directory exists, because not all coreboot trees
  contain this utility; where they don't, but it is needed, we can always
  add it with a patch, but this is currently not required.
* `include/vendor.sh`: Simplified utils handling in `bootstrap()`; a single
  for loop is now used, for all utilities, when running the `trees` script.
* `include/vendor.sh`: Simplified initialisatino of the `_7ztest` variable;
  it is included now in the main call to `setvars`, under global variables.
* `include/vendor.sh`: Condensed a few code lines in `detect_board()`.
* `include/vendor.sh`: Condensed a few code lines in `patch_rom()`.
* `include/vendor.sh`: Add explicit return at the end of `bootstrap()`.
* `include/vendor.sh`: Renamed `getcfg()` to `readkconfig()`.
* `include/vendor.sh`: Merged `cfgutils()` into `readcfg()`.
* `include/vendor.sh`: Merged `build_dependencies_inject()`
  into `vendor_inject()`.
* `include/vendor.sh`: Tidied up a few variable initialisations, and the
  handling of `cbutils` (coreboot utilities) in this context.
* `include/vendor.sh`: Clean up GbE region handling; there were actually two
  separate sets of logic for handling the MAC address. It was unified, so that
  only `modify_gbe()` is used throughout.
* `include/vendor.sh`: General code simplification. Many variables were declared
  separately, instead of being declared with `setvars`, and a lot of for loops
  were condensed.
* `include/vendor.sh`: Simplified the `modify_gbe()` function. It was re-added,
  and the function was still using a much older coding style from before all
  the various lbmk audits. The function is now about half the size, while
  performing the same task.
* Generally tidied up a few `setvars` calls, condensing them them so that they
  used fewer code lines, while still observing the 80-character rule.
* `script/roms`: Explicitly shift by 1 for all targets. The way this script
  worked, it would go through all arguments populating each one inside a
  single string containing them, shifting until no arguments were left. We want
  to make sure it is shiftedh by *one*, and make this intention clear.
* `script/roms`: Add return value for the `list` command, because we want a
  specific return value but `return` passes the value of the previous return,
  if a given `return` call does not specify an argument, and there's no way to
  predict what the previous value would have been. However, this is not a bug
  fix, pre-emptive or otherwsise, because nothing actually using this command
  relied in any way on its return value.
* `include/vendor.sh`: Remove unnecessary checks. The trees script already checks
  the existing of binaries and sources, so it didn't need to be done here.
* `include/vendor.sh`: Remove unnecessary coreboot tree check. We didn't need to
  download a coreboot tree, because this script also handled coreboot utilities,
  which in turn would download the necessary coreboot tree in the same way.
* `include/vendor.sh`: Remove unnecessary `cbutils` variable check; the same
  check (of coreboot utilities) was already performed in `script/trees`.
* `include/vendor.sh`: Remove unnecessary check; `mktemp` will never return
  empty output, and it was used just prior to an `mkdir` command on that output,
  which would subsequently return an error if the argument is empty.
* `include/vendor.sh`: Condensed a few code lines in `check_board()`.
* `include/vendor.sh`: Condensed some code lines handling variable initialisation.
* `include/vendor.sh`: Remove unnecessary `cbfstool` variable check; the same
  check is also performed in `script/trees`.
* `include/vendor.sh`: Simplified the `cbfstoolref` variable check; condense
  a few code lines. This variable refers to cbfstool from coreboot 4.13, used
  for extraction of the *refcode* file on HP EliteBook 820 G2, at build time.
* `include/lib.sh`: Tidy up `err_()`; condense a few code lines.
* `include/lib.sh`: Add explicit return to the end of `chkvars()`.
* `include/vendor.sh`: Unified reading of board configs. The `inject`
  and `download` functions had separate logic for this, which is now implemented
  in a single function throughout.
* `include/vendor.sh`: Simplify variable check in `getcfg()`; use `chkvars()`
  to check the `vcfg` and `tree` variables (exit with error status if they
  were not initialised).
* `include/vendor.sh`: Removed unnecessary `cbdir` check. We don't need to
  check it, because the `trees` script already checks it.
* Remove use of `_xm` in the build system, which was a variable containing a
  given prefix as set throughout, for each function. It was included in error
  messages, when returning from a function under fault conditions, but it was
  no longer used; a relic of lbmk from several audits ago.
* `script/trees`: Simplify single- and multi-tree handling in `main()`. Use
  shorthand notation on a single line, initialising it based on single-tree
  and then checking that the given project is multi-tree.
* `script/trees`: Rename `build_projects()` to `build_project()`. The script used
  to support building multiple single-tree projects, but this behaviour was buggy
  and unused, so it was removed. Rename the function accordingly.
* `script/trees`: Set `btype` if `target.cfg` doesn't exist; set to `auto`.
  The build system assumes multi-tree, if Kconfig files are used. Single-tree
  projects will not typically define `btype`, so just default it to `auto`.
* `include/lib.sh`: Removed unused `cbdir` variable.
* `script/roms`: Don't assign `cbdir`, because it wasn't even used here.
* `script/trees`: Don't hardcode skipping kconfig files based on whether the
  project is GRUB. Instead, define `btype` in project `target.cfg` files, and
  define this in GRUB. Some projects are multi-tree but do not use Kconfig files,
  and GRUB is one of them; we must avoid commands such as `make menuconfig`,
  doing nothing and returning with zero status explicitly, otherwise it would
  yield an error, which is a problem if you ran the trees script with a flag
  but no argument (which, as documented elsewhere in this report, was later
  made to operate that flag on every project in succession).
* `include/git.sh`: Don't check that a given source directory exists, because
  the main `trees` script already does this before running fetch commands, which
  are implemented in `git.sh`.
* `build`: Move `git_err()` here, from `include/lib.sh`, because it's only used
  in the main `build` script.
* `include/lib.sh`: Simplify `singletree()`; condense a few code lines.
* `include/lib.sh`: Add an explicit return to the end of `check_defconfig()`.
* `include/lib.sh`: condense the `e()` function a bit (was already done before,
  and done again).
* `include/lib.sh`: Simplified `TMPDIR` handling; remove the `tmpdir` variable
  and use `TMPDIR` directly. Use a new variable `xbmk_parent`, which is set
  to `y` only on the main instance; child instances of lbmk do not set it,
  signalling that `TMPDIR` should not be removed upon exit (it should only
  be removed upon exit from the main parent instance of lbmk, not child
  instances executed by it).
* `include/lib.sh`: Condense if/else logic in `setcfg()`. Use shorthand
  notation instead, on conditional statements.
* `include/lib.sh`: Condensed `setvars()` a bit.
* `include/lib.sh`: Simplified lock message, when a lock file exists while
  trying to run lbmk.
* `include/lib.sh`: Simplified reading of version files. A single for loop
  now initialises all of these variables with a single `eval` command inside.
* `include/git.sh`, `include/lib.sh`, `script/roms` and `script/trees`: Simplify
  use of environmental variables. The variables themselves were being copied
  to other variables, of lowercase naming, but this was unnecessary. The
  environmental variables are now used, directly, while being properly set per
  each running instance of lbmk.
* `script/roms`: Remove the `t` variable, which was used for temporary ROM
  images. The script now operates on images more directly, and this variable
  is no longer used (previously used for storing paths to temporary images).
* `script/roms`: Simplified logic for checking U-Boot builds.
* `script/roms`: Simplify initmode/displaymode loop, when building multiple
  coreboot images per target.
* `script/roms`: Renamed a few functions and variables for code clarity.
* `script/roms`: Build coreboot images earlier on in the process, to avoid
  duplicate work. It was built per coreboot configuration, but the logic
  builds all images per target, so it doesn't make sense to run the build twice.
  The subsequent runs would just skip, because the images already existed.
* `script/roms`: Generalised more logic, merging a lot of functions and
  reducing a lot of code repetition.
* `include/lib.sh`: Remove `badcmd()`, because it's bloat. This function
  simply write "bad command", followed by a call to the err function. Instead,
  simply call err with the same string, when required.
* `include/lib.sh`, `include/git.sh` and `include/vendor.sh`, `script/trees`
  and `script/roms`: More unified handling of project configurations. Remove
  the function `scan_config()`, which worked with a specially crafted format
  that we had for vendor files and project (source repo) configs. Instead,
  use flat config files. Individual `target.cfg` files for coreboot now contain
  a `vcfg` variable pointing to a directory, containing vendorfile config.
  This change resulted in a sloccount reduction of about 20, in the build
  system, and the new config handling is much more robust.
* `script/trees`: More robust check for `make fetch`, when handling source
  downloads. Don't use shorthand form; instead, use if/else chains to
  determine whether to skip a target after download.
* `include/vendor.sh`: Minor code cleanup (condense a few code lines).
* `include/git.sh`: General code cleanup in `fetch_submodules()`. Several
  code lines have been condensed.
* `script/roms`: Merge `mkserprog()` into `main()`, because it was essentially
  performing the same functions, but for checking serprog targets. Unify the
  logic, for handling coreboot, stm32 serprog and rp2040 serprog targets.
* `script/roms`: Reduced code indentation in `build_grub_roms()`.
* `script/roms`: Remove `build_payloads()` and split it into smaller functions.
* `script/roms`: Condense lines together that check for various payloads.
* `script/roms`: Removed the `mt86bin` variable, because it's only used once.
  Instead, hardcode the path to memtest86plus when building it.
* `script/roms`: Merge `build_uboot_payload()` into `build_payloads()`, because
  both functions are very simple and do essentially the same thing.
* `script/roms`: Simplified check for u-boot payload; we only need to know
  whether it's enabled, not whether it's also disabled. If set to enable
  other than `y`, set it to `n` blindly.
* `script/roms`: Simplify the check for serprog source directories. Instead
  of an if/else chain, re-use variables and use a single `eval` command
  between the two types; serprog rp2040 and serprog stm32.
* `script/roms`: Simplify the loop for building serprog images, by re-using
  variables more aggressively and removing the if/else chain for serprog type.
* `script/roms`: Shorter variable names for serprog build logic.
* `script/roms`: Simplified serprog image copy, by using a single `eval`
  call referencing `stm32src` or `rp2040src`, thus unifying this logic between
  both types of serprog build.
* `script/roms`: Rename `picosrc` to `rp2040src`; this is in preparation for
  the next change, mentioned above.
* `script/roms`: Remove confirmation dialog at the end of a serprog build,
  because the message was overly pedantic and something the user already knows.
* `script/roms`: Merge the serprog handling into a single helper function.
* `script/roms`: Renamed the `x` variable to `it` in a for loop, to avoid
  possible conflict with another same-named variable elsewhere in this script.
* `script/roms`: Don't call `x_` when using `cproms()`, since it never returns
  with non-zero status anyway.
* `script/roms`: Move the U-Boot images, instead of copying. This way, we don't
  have to clean up the temporary file that was used for it.
* `script/roms`: Allow flexible use of `mv` or `cp` in `cproms()`.
* `script/roms`, `include/lib.sh` and `script/trees`: Use a common string
  for setting `elfdir`, which is where builds are copied to upon completion.
* `script/roms`: Merge `mkUbootRom()` into `build_uboot_roms()`.
* `script/roms`: Remove unused variables in `mkSeabiosRom()`.
* `script/roms`: Remove unused variables in `mkUbootRom()`.
* `script/roms`: Remove unnecessary variable assignment; `cbcfg` was already
  properly initialised as a global variable, so this was re-used instead.
* `script/roms`: Removed unnecessary check for `payload_uboot`;
  the `uboot_config` variable, if set, clearly indicates use of U-Boot.
* `build`: Remove unused `linkpath` and `linkname` variables.
* `script/roms`: Make `tmpcfg` a variable, re-used elsewhere when a temporary
  file needs to be written. Simply overwrite the file as required, instead of
  always calling `mktemp`.
* `script/roms`: Simplified timeout/scandisk insertion. We don't need to
  call `mktemp` every time. Just use a static temporary file and keep
  overwriting it, when we need to make a new configuration file inside CBFS.
* `script/roms`: Simplified SeaGRUB check in `build_grub_roms()`; condensed
  a few code lines.
* `script/roms`: Simplify `mkserprog()`; condensed several code lines and
  removed unused variables!
* `script/roms`: Simplify the Serprog build functions; condensed several
  code lines and removed unused variables.
* `script/roms`: Simplify U-Boot payload check (same change as below).
* `script/roms`: Simplify GRUB-only check on SeaGRUB (same change as below).
* `script/roms`: Simplify SeaGRUB check in `configure_target()`; condensed
  the if/else checks based on which payloads are configured.
* `script/roms`: don't use `x_()` to call `build_grub_roms()`, since that
  function never returns non-zero status anyway.
* `script/trees`: Simplify `copy_elf()`; condense the `while` loop handling
  file copies, for copying builds to their destination paths once complete.
* `script/trees`: Remove unnecessary check in `copy_elf()`; we don't need to
  check whether the `listfile` string is empty, because a check comes after
  it as to whether the file it defines exists, and checking an empty string
  there will always yield a negative result, as intended.
* `include/lib.sh`: Add generic `cbfs()` function, which is now used throughout
  lbmk when adding files to CBFS on coreboot images. This simplifies handling
  of CBFS in lbmk.
* `script/roms`: merged handling of build targets into the main script,
  simplifying the loop going through them all; each helper function returns
  with non-zero status if the build is to cease, and the target is skipped.
* `script/roms`: Simplified handling of multiple coreboot build targets, when
  looping through them all during the build process.
* `include/lib.sh`: added a unified function `chkvars()`, now used for checking
  variable initialisation; if a variable is not initialised, it causes an exit
  with non-zero status.
* `script/roms`: shorter variable names, condensed several functions.

Revision updates
----------------

### U-Boot

Alper Nebi Yasak is the maintainer of U-Boot, within Libreboot, and submitted
a patch updating U-Boot to v2024.07, on the `gru_bob` and `gru_kevin`
Chromebooks. Several improvements have been merged upstream, like Alper's
patches for Qemu, so these patches have been dropped in lbmk because they're
now included by default, in the new U-Boot revision.

Alper also tested the latest coreboot revision and it worked fine, on these
boards, but revision updates will be done *immediately after* Audit 6
is announced, as a priority for further work that is to be completed for
the next Libreboot release, ETA August 2024.

Git log
--------

This git log covers all changes in this audit, relative to Libreboot 20240612.

```
* 31f1e4dad vendor.sh: don't use XBMK_CACHE for appdir 
* 2b50b3ea9 put cachedir in environmental variable 
* 0e0b12a63 git.sh: warn when a cached clone fails 
* 9b1b95576 git.sh: fix typo in git command 
* 82bdf2707 git.sh: fix lack of error exits on fault
* 64283a1fb build: remove tmp/ on release archives
* bf85246c4 trees: hardcode makeargs for xgcc 
* c6e6d96ce allow using coreboot's build system to add payload 
* 78cdc56ae trees: remove unnecessary command 
* e69903789 build: remove cache/ on making releases 
* 79f50b379 unify caching of files into cache/ 
* a9f9de885 unified cache file handling for vendorfile/subfile 
* ef867eccc git.sh: remove previous tmprepo if args>5 
* 4d5385a14 git.sh: try direct clone if cached git fails 
* 0cd52fc7f git.sh: re-try git pull three times 
* 0b9cd77fe trees: auto-delete+auto-rebuild if project changes 
* 4438368c0 trees: also remove single-tree repo/p/ 
* 570e3abb2 trees: remove repo/p, not src/p/p 
* 583502027 git.sh: don't download to src/project/project/ 
* 9f09728ac git.sh: cache git downloads to repo/ 
* f367afabc remove util/autoport 
* 373c2eb23 trees: move dependency building to new function 
*   12c3956f5 Merge pull request 'Update U-Boot to v2024.07' (#225) from alpernebbi/lbmk:uboot-v2024.07 into master 
|\  
| * 708fc14c6 u-boot: Fix display initialization on gru boards 
| * 0cc7736b4 u-boot: Avoid breaking build for U-Boot-only binman images 
| * 2ecec55af u-boot: Update to v2024.07 
|/  
* 090cf7ad5 trees: fix bad comparison in configure_project() 
* e37779c11 trees: don't continue if no argument given 
* 3263eeb6b trees: general code cleanup 
* 17f3e61a0 trees: merge build_targets() with handle_targets() 
* c0a4df688 trees: use wrapper for dry-running make commands 
* 459db1cc2 trees: remove project-specific hacks 
* e9f66ec4a remove executable permission on include/ 
* 91994b785 rom.sh: actually say if a cbutil exists 
* 97b777715 rom.sh: avoid re-building cbutils if built 
* 494b94799 rom.sh: only before cbutils before coreboot 
* 469cc64f9 trees: fix bad comparison 
* 5a1d2401c minor cleanup 
* 964617214 trees: support -d (dry run) for custom build logic 
* e850c06bd rom.sh: only make rom tarball if release=y 
* 2f3cc5d37 rom.sh: new file, to replace script/roms 
* 889afe168 roms: remove unnecessary trees checks 
* c241a3ef4 coreboot: set build_depend on target.cfg files 
* 6d4fb200a trees: rename load_target_config() 
* 87df6ae36 trees: support build dependencies, not just fetch 
* 23ca49bee GRUB: only load xhci from grub.cfg 
* 065453b72 trees: just do makeargs on coreboot, not cbmakearg 
* 2ab1d9949 trees: fix bad rm -Rf command (-Rf, not Rf) 
* d6ff009e2 roms: fix bad comparison in configure_target() 
* 3ee045f9a GRUB: use mkhelper.cfg for common variables 
* 3ef84af9f trees: allow global config on multi-tree projects 
* 2b49714a6 trees: handle all projects if project undefined 
* b7f12ade0 vendor.sh: remove mkdirs() 
* 0018600d6 git.sh: simpler for loop in git_am_patches() 
* 5882056a2 git.sh: merge for loops in git_am_patches() 
* 67421a21f trees and git.sh: tidy up global variables 
* 55dbd72aa git.sh: simplified initialisation of "loc" 
* 0cb84a8dd trees: simplified distclean directory check 
* 5ba0433b5 git.sh: condense fetch_targets() a bit 
* 4772186b1 git.sh: short git_prep command in fetch_targets() 
* c62dbdbe8 trees: only do bare multi-tree clone on git 
* 89a81a291 trees: simplified multi-tree bare repo clone 
* 017fd8259 git.sh: merge prepare_new_tree with fetch_targets 
* f937a1142 trees run_make_command: simpler distclean handling 
* 88f741db9 trees: condense copy_elf() a bit 
* d5928c9ba trees: add return to handle_defconfig() 
* 1a7c8acb0 trees: rename check_config to check_defconfig 
* ba7b3a3b2 trees: remove variable "config_name" 
* 030f1afd8 trees: rename variable "config" to defconfig 
* 739496061 git.sh: remove duplicate "xtree" variable 
* 0c4ad24b0 trees: remove unnecessary commonts 
* eacc41e82 trees: condense run_make_command() a bit 
* 563948141 trees: condense handle_makefile() a bit 
* 1ec9fc336 trees: mv load_project_config load_target_config 
* 789631ccb trees, multi: download bare project *before* trees 
* a5e724e51 trees: unified multi-tree configuration handling 
* cfc9f62ff trees: unified handling of source downloads 
* 7bf283520 git.sh: rename Fetch_project_trees fetch_targets 
* e0c244f37 git.sh: rename fetch_project_repo to fetch_project 
* 02f741795 trees: better skip-clean string in handle_src_tree 
* f7ec42d37 trees: add return to check_coreboot_utils() 
* 8d02adfbb trees: simplify "utilmode" variable initialisation 
* d1b7882c5 vendor.sh readcfg: split the -b coreboot command 
* fbe36b01a trees mkpayload_grub: remove unnecessary check 
* 088a79918 vendor.sh: remove unnecessary check 
* 8c5f78d3d vendor.sh: condense inject() a bit 
* 42925ce79 vendor.sh extract_kbc1126: use quote on file check 
* ddcc74777 vendor.sh extract_kbc1126ec: simplify build check 
* 7a12cd770 vendor.sh: simplify e6400 vga rom file check 
* 163bcfcdf vendor.sh: simplify variable checks for e6400vga 
* cbac2087c vendor.sh: condense fetch() a bit more 
* 675d2c037 vendor.sh: remove unnecessary check 
* 5e46b9f43 vendor.sh: simplify kbc1126 util handling 
* 7086b6868 vendor.sh: simplify bootstrap() utils handling 
* 6a05487ef vendor.sh: simplified initialisation of _7ztest 
* 972e61070 vendor.sh: condense detect_board() a bit 
* bf5acedb1 vendor.sh: condense patch_rom() a bit 
* 8b8cf070d vendor.sh: add return to end of bootstrap() 
* d85b9de2d vendor.sh: rename getcfg() to readkconfig() 
* 53b69f673 vendor.sh: merge cfgutils() into readcfg() 
* 55677d821 vendor.sh: remove build_dependencies_inject() 
* c76419861 vendor.sh: simplify initialisation of variables 
* d63da943c vendor.sh inject: skip serprog targets 
* c6df9666a vendor.sh: don't inject /dev/null 
* f7ab70f60 vendor.sh: simplify inject() 
* ee5bab2c9 vendor.sh: clean up GbE handling 
* f2d53eb84 vendor.sh: condense fetch() a bit 
* 85c2eb0f7 vendor.sh: general code cleanup 
* a8f272536 lib.sh: stricter check in chkvars() 
* fa5af956c vendor.sh: simplified modify_gbe handling 
* 155ce49b1 vendor.sh: simplify modify_gbe() 
* 08bce7ad3 vendor.sh: minor code cleanup 
* 636b86237 vendor.sh: re-add modify_gbe() 
* 2eb0163cc Revert "vendor.sh: remove unnecessary check" 
* 3f37c3788 tidy up some setvars lists 
* ed3cce84b roms: explicitly shift by 1 on the "all" target 
* 359d00bd5 roms: add return value for the list command 
* 3f540e756 roms: build u-boot *before* checking ubootelf 
* afc003068 remove more unnecessary checks on trees commands 
* 35c487106 vendor.sh: remove unnecessary checks 
* ce03786f6 vendor.sh: remove unnecessary command 
* e01b5c974 vendor.sh: remove unnecesessary cbutils check 
* 8196ce6dd vendor.sh: remove unnecessary check 
* 8f50131e1 vendor.sh: condense check_board() a bit 
* d12a68601 vendor fetch(): condense variable initialisation 
* 708e306e5 vendor.sh: remove unnecessary cbfstool check 
* 75951c469 vendor.sh: simplified cbfstoolref check 
* d36c07847 lib.sh: keep versiondate check to 80 characters 
* 40d3bb19b lib.sh: condense for loop 
* 5725e3c4e lib.sh: condense err_() a bit 
* e3546f77b lib.sh: add a return to the end of chkvars() 
* d0f68a0fb vendor.sh: rename release to vrelease 
* 473f27784 vendor.sh: unified reading of board configs 
* a2f4eba58 vendor.sh: simplify variable check in getcfg() 
* 4afcbcb64 vendor.sh: remove unnecessary cbdir check 
* fd037722a remove use of _xm variable in the build system 
* 136787185 trees: don't hardcode use of mkpayload_grub 
* c59fbb365 trees: simplify single/multi handling in main() 
* 902b98d1a trees: rename build_projects to build_project 
* e1e04aa80 trees: err if target.cfg not given if multi-tree 
* 93ff80d96 trees: set btype if target.cfg doesn't exist 
* 732c5908d lib.sh: remove unused cbdir variable 
* 119cebc4d roms: remove unnecessary assignment to cbdir 
* 7a15ba18c trees: avoid kconfig make commands generically 
* eb9c1872b git.sh: remove unnecessary check 
* 9aec992ff lib.sh: move git_err() to build 
* 6fc7cd3c1 lib.sh: condense singletree() a bit 
* 35d09e426 lib.sh: add a return to the end of check_defconfig 
* 144b3c93a trees: condense elfcheck() a bit 
* 7b8c2bd41 lib.sh: condense e() a bit 
* ffdecb0ce trees: shorten the final confirmation message 
* ed6acfee8 lib.sh: make elf/coreboot* a dot directory 
* 44fb98736 build: don't rm TMPDIR if it's /tmp 
* fac74cd60 lib.sh: simplified TMPDIR handling 
* 671893a80 lib.sh: condense setcfg() if/else logic 
* 19bc5845a trees: remove redundant space in printf 
* 9154ba5b8 trees: explicitly err if OPTARG is not set 
* f383b1ad7 trees: only permit one single-tree project 
* a64e2db58 trees: call err if multiple flags are provided 
* 0e97e98fe trees: explicitly set mode for -b 
* 7f7b640d3 roms: re-add compression of coreboot images 
* 1ab9189cb roms: build coreboot *after* checking variables 
* 2b4c6ee5c lib.sh: introduce mandatory check of projectname 
* c79fb125e lib.sh: condense setvars() a bit 
* 1387dc0ae simplified lock message 
* 20ac7ec67 lib.sh: simplify reading of version files 
* 4e48fa808 lib.sh: simplify use of environment variables 
* b4fb25100 roms main(): confirm what serprog images are built 
* 5e2308ce0 roms: remove unused variable name 
* 2960abd00 roms: remove redundant printf 
* c661eca6b roms: optimise u-boot elf check 
* 8af9f904c roms: simplify build_roms() 
* c9f26dbc9 roms: make the bin/elf message more polite 
* 9484eda5a roms: re-add final confirmation of targets 
* 2b7df7ab2 roms: rename functions for extra clarity 
* 6753222d0 roms: build coreboot early to avoid duplicate work 
* 19e7c1eab trees: try xgcc build twice if first attempt fails 
* 6468bdb3a trees: don't check if xgcc is already built 
* d5baaff0e lib.sh: fix error running ./build dependencies 
* 0158a0811 roms: general code cleanup 
* e67cd1716 roms: only support SeaBIOS/SeaGRUB on x86 
* 71137b12b roms: remove support for "grubonly" seabios 
* d4d5d2902 use backticks on eval commands, not subshells 
* c1527b611 lib.sh: remove badcmd() 
* fc7ae3e59 lib.sh: more unified config handling 
* ad1602569 trees: more robust check to avoid "make fetch" 
* a6b1a6bdd roms: fix lack of backslash on multi-line command 
* 8c4a91850 vendor.sh: more cleanup 
* 471129684 Revert "roms: remove build_payloads() and split it up" 
* 5a4fc97c6 vendor.sh: correction (s/scancfg/scan_config) 
* d65e4fac1 git.sh: revert modification to for loop 
* bc61c39ec vendor.sh: minor code cleanup 
* 808458ced minor code cleanup in the build system 
* 4ab99d546 git.sh: general code cleanup in fetch_submodule() 
* cd1d84789 git.sh: reduced indentation on repo/file check 
* 4f6dda136 git.sh: simplified repo/backup check 
* 185d76f57 roms: merge mkserprog() into main() 
* 893e88bc8 roms: don't insert timeout.cfg 
* abfc799fd correction 
* a0da8fdef roms: reduce indentation in build_grub_roms() 
* 383433d4f roms: re-introduce accidentally disabled check 
* 3610667e3 roms: remove build_payloads() and split it up 
* 29a7123c0 roms: group some commands that are similar 
* 2d6946775 roms: remove mt86bin variable 
* 920e5ba2d roms: merge build_uboot_payload to build_payloads 
* a96c4b59f roms: simplify payload_uboot y/n check 
* 28682b1a4 roms: simplify the check for serprog srcdir 
* b61dd4c25 roms: simplify the loop for building serprog roms 
* 6df17860e roms: shorten variable serprog_boards_dir 
* ea5b5b072 roms: simplified serprog image copy 
* ea9bdfce4 roms: rename picosrc variable to rp2040src 
* 1a4f97ffd roms: remove useless confirmation in mkserprogfw 
* 1881d34db roms: merge serprog build into one function 
* f7e28964a roms: remind the user about gkb files 
* f928ac5c7 roms: rename x variable to it in for loop 
* 58a451865 roms: don't use x_ to call cproms() 
* bc853fbb8 roms build_uboot_roms(): move rom, don't copy 
* 12b26f207 roms cproms(): allow other commands besides cp 
* e67628c6a unify coreboot elfdir (DO_NOT_FLASH) 
* 8b58c1eac roms: merge mkUbootRom() into build_uboot_roms() 
* c3f0a109c roms: simplify mkSeabiosRom() 
* c8944f1ca roms: simplify mkUbootRom() 
* 92aa83a23 roms: simplify build_roms() 
* d3e788645 roms: remove unnecessary check 
* ef411c596 build: remove unused variables 
* 6dc051558 roms: further clean up build_grub_roms() 
* 0e9b36c58 roms: simplify timeout/scandisk insertion 
* 3a7b3660f roms: simplify seagrub check in build_grub_roms 
* 4b764d26f roms: simplify mkserprog() 
* 167e7447a roms: simplify the serprog build functions 
* 7bc9fcc34 script/roms: fix serprog build commands 
* 0dfe3aed9 roms: simplified ubootelf check 
* a9166898d roms: simplify grubonly check in configure_target 
* 90017cdc5 roms: simplify seagrub check in configure_target 
* 817004e15 roms: don't use x_ to call build_grub_roms 
* 0812d5321 trees: simplify copy_elf() 
* 331c4097f trees: remove unnecessary check in copy_elf 
* 340eea0b1 grub: insert background in memdisk instead 
* ed9c90e59 roms: unify all add-payload commands 
* 26451775d roms: don't add grub keymaps at all 
* af8296ce6 roms: merge handle_coreboot_target into main() 
* d1c0c3464 roms: simplify target check (whether roms built) 
* f626b25db roms: simplify main() again 
* eb9a688ee roms: remove redundant check on grub_scan_disk 
* a4328cb11 roms: remove dangerous runtime p/s/d options 
* 167e1a0fb unified checks for variable initialisation 
* 98724d701 lib.sh: remove the items() function 
* baea03c67 roms: simplify main() 
```

This is 247 changes in total, between 12 June 2024 and 19 July 2024.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
