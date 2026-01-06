% Libreboot 26.01 RC1 "Tenacious Tomato" released!
% Leah Rowe
% 25 December 2025

Today's Libreboot 26.01 RC1 revision is a *testing release*, whereas the previous
stable release was Libreboot 20241206. This revised release log lists all
changes as of today, 25 December 2025, since the Libreboot 20241206 release of
December 2024.

**You can find today's release in 
<https://rsync.libreboot.org/testing/26.01rc1/> or you can use one of
the [mirrors](../download.md).**

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>
<img tabindex=1 class="r" src="https://av.minifree.org/t580.jpg" /><span class="f"><img src="https://av.minifree.org/t580.jpg" /></span>

This is the *first* Release Candidate (RC1) in the Libreboot 26.01 release
series, codenamed "Tenacious Tomato". It is expected that new RCs will be
issued weekly, between now and the time of the full release.

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

Detailed list of changes
-------------------------

### Board support

As stated above, the following boards have been added:

* HP Pro 3500 Series (board port by *Vesek*)
* Topton XE2 N150 (board port by Riku Viitanen)
* ThinkPad T580 (board port by Johann C. Rode).

More boards are planned, for the final release in late January 2026. Boards
will be added when feasible, in each new RC.

Work is ongoing to integrate Google Chromebooks, by integrating the
MrChromebox distribution of coreboot into Libreboot's build system.

### Revision updates

**TODO: Only include the latest rev updates. Don't mention earlier duplicates,
  unless there's something special about them.**

In descending order from latest changes to earliest changes:

* Updated PCSX-Redux to rev `b745534eb231d3699ec57949f16a9a7bd5b79385`, which
  contains various bug fixes and feature improvements.
* Updated flashprog to revision `ffcf92fb`.
* Updated GRUB to revision `25b7f6b93` from December 2025. This is a little bit
  after 2.14-rc1 and adds a lot of bug fixes and new features. For example,
  the PHC argon2 implementation could be dropped because GRUB now imports a
  newer version of libgcrypt, which supports argon2. Other improvements as well,
  such as a general support for more ciphers, or support for things like BLS
  and UKIs; BLS and UKIs are untested in the the 26.01 release, but there is no
  reason in principle why they shouldn't work..
* Updated deguard to the latest revision, which adds files for the other recently
  added Kabylake ThinkPads e.g. ThinkPad X280, T580, T470s.
* coreboot/default: Update to revision `def7aa7094` from December 2025.
* coreboot/default: rev 9e41c7cec7, 18 July 2025. **NOTE: later updated
  again, before RC**
* pcsx-redux: bump rev 8f8cc3d5, 20 June 2025. **NOTE: later updated again,
  before RC.**
* coreboot/default: bump rev 812d0e2f62, 5 Apr 2025. **NOTE: later updated
  again before the RC.**
* deguard: bump to rev 0ed3e4f. **NOTE: Later updated again before RC.**
* GRUB: Bump to a68a7dece, 23 June 2025. **NOTE: Later updated again to
  a December 2025 release before this RC was built.**
* SeaBIOS: Update to rev b686f460, June 2025. **NOTE: This was updated again
  to a December 2025 revision, just before this RC release.**

### Licensing

<img tabindex=1 class="r" src="https://av.libreboot.org/hp8460p/hp8460p.webp" /><span class="f"><img src="https://av.libreboot.org/hp8460p/hp8460p.webp" /></span>

TODO write this section.

### Security

In descending order from latest changes to earliest changes:

* All of lbmk: Reduced use of `eval` statements; eval much be used with care, in
  sh, and avoided if possible, because it can very easily allow code injection
  vulnerabilities if handled carelessly. Even if the code is currently correct,
  mistakes could be made in the future, and it's unknown what person might
  touch your code in the future. It's always better to just assume the worst.
  There were *no* actual known bugs caused by eval, because Libreboot has an
  extremely sensible and conservative build system design.

### Feature changes

<img tabindex=1 class="r" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

In descending order from latest changes to earliest changes:

* ThinkPad T480/s: Add ThunderBolt PCI-E support.
* ThinkPad T480/s: fix headphone jack detection. You previously had to switch
  the port manually, using a program such as `pavucontrol`. This is now fixed,
  in 
* Initial work on a script (`chromebook.md`) to support Google Chromebooks,
  by importing (and adapting) configs from the MrChromebox coreboot distribution
  into Libreboot's build system. This feature is intended for completion by
  the time of the full Libreboot 26.01 release, but it was still work in
  progress on the initial 26.01 RC1. 
* Depending on the context, this may also be regarded as a bug fix:
  in `get.sh`, the git remote caching was re-implemented in such a way that the
  cached path corresponds directly to the URL where the code was hosted. This
  is done not only per-project, but per remote in each project; we have backup
  remotes for each project. In this way, the caching is much more reliable,
  because we no longer store multiple remotes in a single Git repository. This
  also allows us to use a diverging fork of a project, so long as the target
  revision exists in both trees. This is a prerequisite step before implementing
  support for more than two remotes per project, which will be used in the
  future to support multiple different forks of coreboot, *without* needing
  to write a lot of bloated custom code for what are essentially the same
  projects (from Libreboot's perspective).
* `init.sh`: Only ever create the `cache/` directory here, because we
  previously created it elsewhere. We must ensure that we always have reliable
  and centralised handling of certain critical directories.  
* Don't compress the Alderlake-N FSP on the Topton XE2 N150 mainboard.
  This is necessary for reliable insertion on release images.
* Use `sha512sum` from sbase, instead of relying on the host implementation.
  This increases reliability by virtue of increased consistency.
  The `--status` flag is no longer used, because this is specific only to
  the GNU implementation of sha512sum. Sbase source code is preserved, by
  building a temporary copy of it and then placing the binaries out of tree.
  (out of source builds)
* Imported the suckless `sbase` project into Libreboot's build system, for
  reliably consistent functionality in the `sha512sum` command. This could be
  used for other commands in the future (sbase implements an entire Unix
  userland comparable to e.g. coreutils, but the code is much cleaner and
  extremely reliable).
* `get.sh`: Support `-F` flag which always pulls down new code from a given
  upstream, even if the target revision is already cached locally. This is
  useful if you want to use the cache for something else, externally to lbmk.
  This is used during development, when updating code revisions, otherwise you
  would have to use the cache manually. By using it via lbmk, we can ensure
  more reliable operation in a way that reduces the risk of build system
  corruption via stale artifacts (though in practise, there will usually be
  a backup repository anyway, if something is wrong with the main one as lbmk
  is designed with a lot of redundancy).
* `rom.sh`: support `fspgop` init mode - really doesn't matter, for the user's
  perspective this is similar to using libgfxinit and works similarly, in
  payloads like SeaBIOS and GRUB. By "support", I mean that lbmk will *not*
  put `libgfxinit` in the file name for these images.
* Added a `-p` flag to `me_cleaner`, on all versions used. You can set the
  variable `MEclean="y"` in the config for a given board
  under `config/vendor/`, and `me_cleaner` will then avoid modifying the ME.
  The ME cleaner is still used for bruteforce ME extraction, but the -p option
  will specifically avoid erroring out even if there is an error with FPTR
  checks. This was necessary for the Topton XE1 N150, where we simply set the
  HAP (ME Disable) bit in the IFD, but we don't touch `me.bin` itself. This is
  a simple and powerful way to handle newer Intel ME images without increasing
  the complexity of Libreboot's build system design.
* Build system: We now build libarchive, for use of `bsdtar` and others, in
  a few places. This is part of a general desire to phase out e.g. 7z, unar
  etc, over time, since bsdtar supports nearly everything. Some work still needs
  doing to fully get rid of unar, because the KBC1126 EC firmware files still
  only work with unar at the moment.
* `init.sh`: TMPDIR is now a directory inside the build system work directory,
  instead of `/tmp`. This eliminates the risk that the user may have a tmpfs
  for /tmp; because we had to assume this was the case, this limited the amount
  of data we could safely write inside TMPDIR. Of course, it's still good to
  be frugal with use of TMPDIR regardless, but this is just another example
  of build system reliability improvements.
* `release.sh`: Removed support for the `-d` flag, which allowed one to create
  a custom path for release files to be made available. We don't really need it,
  and the error handling for such a feature needs to be quite complex. This was
  part of a general effort to clean up and simplify the build system.
* cache coreboot builds in `elf/` again - because of the change shown below,
  we can once again do this safely, since they are deleted as needed. Images
  are padded to avoid use, wherein the padding is only removed in versions
  that have the payloads added (this is for installation safety).
* `include/tree.sh`: delete individual target builds if needed. This is
  possible now, because target builds go in subdirectories under a directory
  for the entire tree. Now when you change a target config, it gets rebuilt
  automatically, but without needing to delete the entire tree (this could
  be considered a bug fix, as this was a previous lbmk design flaw, now
  corrected).

### Configuration changes

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi_pico/soic16_x200.webp" /><span class="f"><img src="https://av.libreboot.org/rpi_pico/soic16_x200.webp" /></span>

In descending order from the latest changes to the earliest changes:

* GRUB modules: Enable useof the ciphers in GRUB, e.g. blake. This is part of
  GRUB's recent update to a more modern version of libgcrypt.
* coreboot/default: Use the `cmocka` coreboot submodule, by default. This is
  used for various tests, but coreboot's default behaviour is to try using the
  host's version of cmocka. This is not ideal, because it means that such
  functionality is inconsistent; the argument against doing so is the same one
  that generally lead to the introduction of crossgcc. Therefore, coreboot's
  build system is modified to make it use its own specific version of cmocka
  by default. The operator can still override this by configuration, so no
  functioality was last.
* GRUB: Add keyboard layout for Norway.
* Coreboot: For main targets, use the Github backup mirror by default, instead
  of Coreboot's main clone URL. This is done as a courtesy, to reduce the load
  on coreboot's servers.
* Add configuration for installing build dependencies in Fedora 43.
* Kabylake coreboot mainboards: don't hardcode `power_on_after_fail`.
  Use the CBFS-based backend implemented by Nicholas Chin.
* Added support for SMBIOS type 16/17 in Haswell Native RAM initialisation,
  courtesy of Ron Nazarov who submitted a patch to coreboot; the patch was
  later improved upon by Angel Pons and included in Libreboot as a part of
  a general revision update.
* `grub/xhci`: Rename to `grub/xhci_nvme`, to make it clear that this tree
  has both patchsets. Some users were confused thinking the `xhci` branch does
  not contain NVMe drivers, even though it does!
* Make U-Boot rainbow again.
* `vendor.sh: Use `bsdunzip, rather than host `unzip`. This provides more
  consistent functionality, and compliments a previous decision to also
  use `bsdtar`; Libreboot compiles its own copy of libarchive at runtime,
  thereby providing each user with consistent functionality.`
* GRU bob/kevin chromebooks: Make the U-Boot bootflow menu timeout by eight
  seconds instead of thirty; thirty feels like too much. Eight seconds is about
  right, giving ample time for the user to interact, while also speeding up
  unattended (re-)booting.
* Used cached Git repositories when cloning inside release work directories,
  to speed up the build time.
* coreboot: Added a ThinkPad T440p target that has a 4MB CBFS area. This is for
  recovery purposes, because it's easier to flash just the 2nd chip with this
  4MB image. It is still recommended that the user makes use of the full image.
  Flashing the first chip is also required for neutering Intel ME; the 4MB
  image still has Intel ME inserted in the same way.
* config: Updated the Arch Linux dependencies, because the unifont package
  has since been split into multiple packages. We install what we need.
* `include/tree.sh`: Place target builds under `tree/target/` - this makes
  it easier to handle deletions, when project files change, because they're
  all subdirectories under the elf directory for the entire tree, of a given
  upstream project.
* `include/rom.sh`: bump pcsx-redux copyright date to 2025. This is because new changes
  from 2025 were imported when the PCSX-Redux Open BIOS was updated.
* HP Pro 3500: enlarge CBFS to match the BIOS region
* HP Pro 3500: Use a truncated ME by default. The original port (to Libreboot)
  used `me_cleaner`, but did not truncate the ME region and enlarge the BIOS
  region. This has now been done, as it leaves more space in CBFS for additional
  user-supplied coreboot payloads.
* HP Pro 3500: Unlock all flash regions by default. In practise, coreboot
  always does this anyway (unless you tell it not to). It is nonetheless
  a requirement for the default Libreboot setup.
* HP Pro 3500: set the HAP bit by default. In practise, coreboot disables
  Intel ME via Soft ME Disable, but the original port (to Libreboot) did not
  set the HAP bit; doing so is required, when feasible, on a given board.
* HP Pro 3500: Use SeaGRUB as the payload, not GRUBSea (start SeaBIOS first),
  as this is standard behaviour in Libreboot. The original addition of this
  port (to Libreboot) used GRUBSea.

### Bug fixes

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi/wson8/0003.jpg" /><span class="f"><img src="https://av.libreboot.org/rpi/wson8/0003.jpg" /></span>

This combines both build system fixes, and changes to upstream sources (e.g.
coreboot and various payloads like SeaBIOS/GRUB, utilities like flashprog, and
so on).

The following bug fixes have been merged (in descending order from the latest
changes to the earliest changes):

* `init.sh`: Explicitly export `C.UTF-8` as the system locale, rather than
  just `C`. This is more standard, and fixes a few minor build issues, for
  example in the GRUB project.
* coreboot: Disable early thermal shutdown on Dell Latitude laptops, courtesy
  of Nicholas Chin. These computers would previously shut down after the CPU
  reached 87c; with this fix from Nicholas, normal CPU throttling methods are
  used (allowing higher temperatures in a way that is still safe.
* `get.sh`: Return from `clone_project` if multi-tree, to fix a previous bug;
  previously, both single- and multi-tre ewould be handled by this function,
  with hacks that were quite buggy. Now the difference between the two is
  handled much more robustly.
* `get.sh`: Always assume that `loc` is *not* empty. We previously had a bug
  where sometimes for example, the backup remote on a given project was stored
  in the same location as for main. This was a relic/regression arising after
  the design change whereby separate clones are created for each remote,
  rather than handling multiple remotes in the same project clone.
* `get.sh`: Use `--keep-cr` in `git am` commands, for compatibility with code
  repositories that use CR-LF line endings. This is true of edk2, which I am
  working on integrating into Libreboot for certain boards.
* `get.sh`: Use `git show` instead of `git whatchanged`, for checking if
  a revision exists in `cache/`; the `whatchanged` command was deprecated, and
  now results in an error state (by default, unless changed by the user) if
  used on a modern version of Git.
* `get.sh`: only pull code down if a given revision is not cached locally; this
  fixes the issue where bandwidth would otherwise be wasted needlessly. This
  is part of a general effort to make the build system more efficient, especially
  the parts that deal with the internet.
* `vendor.sh`: Remove false error message about bad hashes when cycling through
  extracted files in `mkdst`; the way it works often is lots of files are
  extracted, but the final file is of a certain hash, that will be checked.
  If a correct file is found, the build system may still have told the user
  about invalid checksums on other files, leading them to think that the
  extraction failed. This situation is therefore avoided, with what to output
  being decided only once it is certain whether the extraction has failed.
* `inject.sh`: Don't directly exit from `patch_release`, because lbmk's policy
  requires that only the main function in `mk` shall exit; everything else
  falls back to mk. In this way, we can (and we do) have centralised handling
  of temporary files, and their subsequent cleanup on exit from the build system.
* `init.sh`: Create `TMPDIR` *after* suid check, otherwise it might be created
  as root, which would disable lbmk in a way not obvious to the user, for the
  very reason that lbmk prevents most tasks from being run as root. This is for
  security reasons.
* `vendor.sh`: Add missing error handly for sha512sum commands; it was
  already handled, but the output is directed to a file. The `x_` function
  is now used when calling sha512sum, for stricter error exits.
* `vendor.sh`: Clean out the *7ztest* directory in startup. Otherwise, some old
  files might still be there, which could lead to incorrect results. Output
  files are hashed now, so it's probably not a problem, but this is nevertheless
  a preventative bug fix, as an early warning against possible bugs that may
  be introduced later. (this is why xbmk hashes everything)
* `init.sh`: Write-protect the `lock` for on startup of the build system. It
  is deleted when the main parent instance exits. This may also be considered
  a general bug fix, but not really, because it doesn't fix the problem that
  the lock file could be removed by someone/something else. However, by setting
  chmod -w on it at runtime, it reduces the chance of *accidental* deletions,
  including by (for example) programmers errors in lbmk! So this is more of
  a preventative bug fix.
* `release.sh`: Delete the `xbmkwd` directory inside source tarballs. We
  previously caused an empty one to be present there; not really a problem,
  but it is unintentional and undesired behaviour.
* init.sh: Bail if `date` is a non-GNU implementation, because we currently
  rely on use of GNU-specific features in `date`, for initialisation of
  Git when working from release archives.
* `init.sh`: Added proper input sanitization to the version strings, because
  these are essentially user-inputted (it's based on use of `git tag`). This
  is to prevent certain as-yet undefined vulnerabilities e.g. code injection.
* mk (main script): hardened `PWD` checks - deny use of symlinks to the main
  script. This is extremely important, due to the design of xbmk; it assumes
  that the work directory is always the same place that `mk` is. We could have
  complicated and error-prone logic to `cd` inside of a subshell, but that
  would just be bloat.
* `get.sh`: Temporary directories for clones are now placed properly inside
  of TMPDIR, which (with use of mktemp) greatly reduces the chance of race
  conditions, if the same directory was to be used by multiple instances all
  at once.
* `lib.sh` and `rom.sh`: Much stricter error handling for `mktemp`.
* `inject/vendor.sh`: Much stricter use of `set +` - we use `set +u +e` in
  certain functions, when we need to have looser error handling in the built-in
  shell (we still carefully handle error conditions even then). The previous code
  was far too lax about how this is done. In general, we want error handling
  to be stricter when feasible.
* init.sh: Fixed a few race conditions with TMPDIR, by effectively eliminating
  the change that more than one instance of a given copy of the build system
  is actually running at any given time.
* init.sh: Fix safety issues in `TMPDIR` handling. We used to check whether
  the build system was running a child instance, based on initialisation of
  TMPDIR, but this is inherently unreliable. Instead, child instances are now
  detected by the presence of a lock file, and this lock file contains the
  path written to `TMPDIR`; if they don't match, or there is none, then we
  know the build system is running in its main parent process; and of course,
  the presence of a lock file will make a new parent process exit, to prevent
  corruption.
* Dependencies: for Debian-based distros, `--no-install-recommends` is now used.
  This prevents a lot of unnecessary dependencies being installed, so that we
  don't waste as much disk space on the user's computer.
* inject.sh: redirect `stderr` to `/dev/null` first, before grep. The way sh
  works, output to a file with `>` or `>>` is done left-to-right, though in
  practise this is implementation-defined. There was no observable difference
  either way, but this is more standard behaviour nonetheless.
* rom.sh: Safer use of cat in copyps1bios - it was piped, so there was no
  error handling. I added use of the `x_` command wrapper, for error handling.
  This is lbmk's way of implementing something similar to `set -o pipefail`,
  while only using POSIX `sh` features.
* tree.sh: don't keep re-checking a given crossgcc tree, for each target. a
  file is created in the project's elf directory, for a given tree (if multi
  tree). this avoids computationally expensive checks and shaves quite a bit
  of time, especially on full release bulids.
* tree.sh: check crossgcc builds *after* checking ELF directories. this is
  a hack, to fix a bug that previously existed where crossgcc was being built,
  if required for a build of a given project, when a non-build operation was
  being performed.
* lib.sh: Additional error handling on a cat command. The output was piped,
  which meant that there was effectively no error handling before this change,
  on that given command. It is the cat command that creates padded coreboot
  builds for releases.
* tree.sh: Delete files *before* updating hashes, when checking for
  project auto-deletion. This change fixes the previous bug where if a
  deletion files, the new hash would be recorded but with stale artifacts
  from previous project sources. This makes the build system more deterministic
  under fault conditions.
* tree.sh: don't delete builds if tree==target - this avoids wasting
  computational time, since prior tree-wide checks (for auto-deletion) would
  have already occured.
* rom.sh: Don't run mkhelpers if release=n - this fixes a build error
  when running release builds, for boards that set release=n, because
  otherwise certain dependencies were still being handled erroneously where
  other dependencies did not exist (in that build state).
* coreboot/default: probe EDID twice in libgfxinit - some buggy adapters
  don't have good timing, so software like Linux has to probe them twice.
  This behaviour has been implemented in Libreboot's version of coreboot.
* T480/T480: Drop redundant PcieRpEnable from - otherwise, it wouldn't build.
  Note that this was only relevant when Libreboot maintained T480 support
  patches out-of-tree. Before this RC release, the T480 was merged upstream
  in coreboot, and Libreboot updated its coreboot version to use the port
  provided there, so now upstream maintains this code.
* `include/lib.sh`: Don't break erroneously in a context where it's not used.

### General code cleanup

<img tabindex=1 class="r" src="https://av.libreboot.org/rpi/wson8/0007.jpg" /><span class="f"><img src="https://av.libreboot.org/rpi/wson8/0007.jpg" /></span>

Another bug focus in this release was to clean up the logic of Libreboot's
build system, and fix several bugs, especially those relating to error handling.

One of the bigger cleanups here is that previously, a lot of temporary files
were placed under `cache/`, but the latter is meant for permanent files,
not temporary files.

In descending order from the latest changes to the earliest changes:

* `tree.sh` (and `get.sh`): Implement a `forcepull` macro, for easier reading
  of the logic whether to skip pulling from upstream if a given revision
  exists (see: `-f` vs `-F` flags in mk).
* `tree.sh`; convert `do_make` into a macro. This is used to tell whether to
  do a full build, useful when doing dry builds for example.
* Entire build system (lbmk): Store variables in alphabetical order, and
  without using the `setvar` shorthand. This improves readibility of the
  scripts, for most people.
* Implement a `if_dry_build` macro in `tree.sh` and `rom.sh`, similar to
  the `if_not_dry_build` micro. It makes certain shortcode notations easier to
  read, when dealing with the idiosyncrasies of `-d` (dry) vs `-f` (full) builds.
* `tree.sh`: Renamed the `xtree` variable to `xgcctree`, for code clarity.
* `tree.sh`: Renamed the `btype` variable to `buildtype`, for code clarity.
* `vendor.sh`: Renamed the `t` (with underscore) variable to `blobtype`, for
  code clarity.
* `tree.sh`: Renamed the variable `f` (with underscore) to `flag`, for code
  clarity.
* xbmk: Renamed the `dry` variable to `if_not_dry_run`, which serves the same
  purpose, as essentially a macro in sh.
* xbmk: Removed the `setcfg` function, which then allowed me to remove a bunch
  of `eval` commands; now I just use the dot command to include files, and still
  with proper error handling.
* `init.sh`: Generally split up a lot of larger functions into smaller
  functions with stubs.
* `vendor.sh`: Generally break up a bunch of functions into subfunctions.
  E.g. the kbc1126 ec extraction is much cleaner now. Ditto `check_gnu_path`.
* `get.sh`: Rename `try_file` to `try_fetch`, because it's actually a generic
  function that also handles local file copying, not just downloads from
  the internet.
* A lot of functions in the build system were previously written in a quite
  highly condensed style. This has been unrolled, to create a more readable
  code style across the entire build system.
* `vendor.sh`: General cleanup of the functions. Ditto `inject.sh`. Split them
  out, make everything clearer for the reader.
* `init.sh` and `tree.sh`: PATH directories are now inside the proper TMPDIR,
  not cache/ - this reduces the chance of stale files being left behind and
  used by subsequent and/or other instances of xbmk.
* release.sh: vdir now inside the proper TMPDIR, not cache/ - this reduces
  the chance of large stale files being left behind improperly, especially
  under fault conditions.
* Entire build system: Remove the `xbloc` variable. We previously did TMPDIR
  inside /tmp, so we had to assume the user might have a tmpfs /tmp with limited
  memory available; we therefore maintained a less-than-reliable disk-based
  system of tmpfile handling; Unix doesn't have much flexibility here, so
  the `TMPDIR` variable is the only official one that defines such a path.
  Because we now force TMPDIR to be inside the xbmk work directory, we no longer
  need a separate system of tmpfile management. We can just do everything
  inside of the proper TMPDIR.
* `get.sh`: Remove unused variable `repofail`.
* `get.sh`: in the function `xbget`, use of `eval` has been greatly reduced.
  The `eval` function should be avoided when possible, because if misused, it
  could easily lead to code injection vulnerabilities.
* init.sh: Tidied up `xbmk_set_env`; it is actually a generic function that
  handles many different types of tasks, so it is now a smaller shim and logic
  has been split out.
* init.sh: TMPDIR handling is now its own helper function. Simplifications
  have also been made to the lock file handling. Functions specific to child
  instances and specific to parent instances are now separately; previously,
  the logic was all inside one function. The main function now, for forking
  the build system depending on the instance type, is now a very small shim
  function, making the logic much more reasonable.
* mk: Remove the check in the main script, that reacted according to whether
  it was being run by a symlink. Due to simplifications in lbmk, we now only
  ever use `./mk`.
* vendor.sh: Tidy up `extract_intel_me` - remove use of chained commands,
  make it easier for a normal person to read.
* tree.sh: rename hashvar to badhashvar - make it more obvious what it's doing.
  Ditto rename hashname to hashdir. Reame hashtype to hashname.
* tree.sh: unified project hash handling - for target builds and tree builds,
  the logic is mostly identical anyway. A helper function was implemented.

Git log
-------

This log shows all changes in today's release, from 30 June 2025, ever
since the Libreboot 20241206 release of 6 December 2025:

```
* a59fc6a353 Libreboot 26.01 RC1 Tenacious Tomato 
* b259f3493d update pcsx-redux openbios 
* a4ad3afdbb bump flashprog rev: ffcf92fb, December 2025 
* 68e0b5dddc init.sh: Explicitly export UTF-8 locale 
* b990d54cff grub modules: add more gcry_ ciphers 
* 5df6b924d7 bump seabios revision 
* 9228a0c02c update grub again, to 25b7f6b93 
* 512cce0770 GRUB: update to rev 2.14-rc1 December 2025 
* 8b338404e8 hp8300cmt and dell 780: use legacy verbs 
* 0ff8110a55 fix 3050micro vbt path again 
* ce302356fc coreboot/dell3050: fix VBT-related build error 
* 2fe3b6ee66 actually enable TBT on T580 
* 6e253b3e73 thunderbolt support for thinkpad t580 
* 9142c8b419 ThinkPad T580 support. 
* 992b03a383 coreboot/default: use 3rdparty/cmocka by default 
* e7628421d7 coreboot/default: add cmocka submodule 
* 946ede9e7d fix ifdtool build on coreboot/default 
* 24f25bf7bc update deguard 
* 63b527e8ff cb/default: bump to rev def7aa7094, December 2025 
* aa0e4205d6 re-base the T480 thunderbolt patch 
*   f3dc54432e Merge pull request 'T480: Add Thunderbolt support' (#387) from hustlerone/lbmk-alpine_ridge:master into master 
|\  
| * 809e5d29a4 T480(s): Add Thunderbolt support 
|/  
* ec8617f27a coreboot/t480s: fix headphone jack detection 
*   724f905aaf Merge pull request 'T480: Fix headphone jack detection' (#385) from Riku_V/lbmk:t480verbfix into master 
|\  
| * 2dea4b79b2 T480: Fix headphone jack detection 
|/  
* 9d740e9512 grub: add a keyboard layout for norway 
* 7a9977af72 rebase recent dell thermal safety patch 
*   efd17fde19 Merge pull request 'removed duplicate "payload_uboot" for e6230_12mb' (#374) from honzo/lbmk:e6230_12mb_deduplicated into master 
|\  
| * fda3ac975a removed duplicate "payload_uboot" for e6230_12mb 
* |   d1606ef11f Merge pull request 'fixed typo in "grub_scan_disk" for macbook11_16mb' (#375) from honzo/lbmk:macbook11_16mb_fixed_typo into master 
|\ \  
| * | 831fa657de fixed typo in "grub_scan_disk" for macbook11_16mb 
| |/  
* |   48b00ce3c8 Merge pull request 'config/coreboot/default: Disable Latitude early thermal shutdown' (#376) from nic3-14159/lbmk:mec5035-updates into master 
|\ \  
| |/  
|/|   
| * c0dab33972 config/coreboot/default: Disable Latitude early thermal shutdown 
|/  
* 1b10c072d3 tree.sh: tidy up check_gnu_path 
* 7ef7e02f73 lib.sh: reduce indentation in setvars 
* 214ed3efd2 get.sh: reduce indendation in fetch_targets 
* 9d6af0063b get.sh: reduce indentation in clone_project 
* d7869a56f5 WIP: chromebook integration script 
* 25f523bbac get.sh: return clone_project if multi-tree 
* bec7e6d4cb Revert "get.sh: don't frivolously copy tmp git clones" 
* f632b8aed7 Revert "get.sh: remove a redundant check" 
* e2a97455cc get.sh: remove a redundant check 
* b840cf3a83 get.sh: don't frivolously copy tmp git clones 
* 2aea7f6229 Revert "get.sh: make forcepull a macro" 
* b3232a7c4a get.sh: make forcepull a macro 
* 54aa5b7d32 tree.sh: unify -f/-F in case/switch handling 
* 96f786b962 tree.sh: convert do_make into a macro 
* 47f08e2e6d git: don't use review.coreboot.org as main 
* e1b6ccf69e xbmk: sort global variables alphabetically 
* d84a556bf0 get.sh: use the same directory map as --mirror 
* b333ddfe73 get.sh: use --keep-cr on git-am 
* d83dd506c2 get.sh: More reliable git remote caching 
*   18c63682e7 Merge pull request 'Add Fedora 43 to dependencies' (#364) from bauduser/lbmk:fedora43dependency into master 
|\  
| * 7b42779912 Add Fedora 43 to dependencies 
|/  
* 11a3e9d887 xbmk: minor code cleanup (79 character rule) 
* fb95e4ad68 tree.sh: add missing -F flag 
* ca5f0a5edd get.sh: use git-show instead, for rev checks 
* 8636d7497c rom.sh/tree.sh: clean up if_not_dry_build 
* 1b54c7a744 rom.sh: use if_dry_build macro 
* afccecbde0 rom.sh: don't run add_cbfs_option on dry builds 
* 7d597bc4a1 disable stack overflow debug on alderlake 
* c716341c13 cb/kabylake: don't hardcode power_on_after_fail 
*   b5ad829ffe Merge pull request 'config/coreboot/default: Add Haswell NRI SMBIOS type 16/17 patch' (#363) from noisytoot/haswell-nri-smbios-memory into master 
|\  
| * e82e2a1332 config/coreboot/default: Add Haswell NRI SMBIOS type 16/17 patch 
|/  
* 9b104fca44 init.sh: only create cache/ here 
* ee2bca65f6 Revert "init.sh: explicitly create cache/" 
* 23f98c2958 init.sh: explicitly create cache/ 
* 1e488aae78 Revert "remove unar from dependencies" 
* c1d6cd22c2 xbmk: don't call mkdir. use xbmkdir (new function) 
* f358cfaa55 coreboot/x2e_n150: fix the alderlake n fsp link 
* 6a00b7a584 coreboot/default: don't require alderlake fsp repo 
* 247cb85489 don't compress alderlake fsp 
* 183f378150 coreboot/x2e_n150: unset CONFIG_FSP_FULL_FD 
* 31fa7ea591 vendor.sh: re-do the previously reverted change 
* 2956fcc051 vendor.sh: fix setvfile 
* 8334c93dac release.sh: preserve clean sbase before building 
* 8969cc734f xbmk: use sbase sha512sum, not host sha512sum 
* e9a910b33c config/git: import suckless sbase 
* 2cfaba181b xbmk: rename cv variables, for clarity 
* 1943dba608 tree.sh: rename xtree to xgcctree, for clarity 
* 51e424c7d1 tree.sh: rename btype to buildtype, for clarity 
* d95af9ba44 vendor.sh: rename _t to blobtype, for code clarity 
* 63002732f5 tree.sh: rename _f to flag, for code clarity 
* b7a9aad9fc get.sh: delete tmp patch list when done 
* a115679c57 get.sh: sort patches when applying 
* 0776eb414c vendor.sh: make the fsp hack a bit cleaner 
* 70cdb03f7f vendor.sh: correction to fsp hack 
* 1eafcf9029 vendor.sh: minor cleanup 
* efd1db1ca3 release.sh: remove eval statement in nuke() 
* b4c7cac8a2 xbmk: rename the "dry" variable to if_not_dry_run 
* 9f84bd4f34 coreboot/mkhelper: don't use eval 
* 4f01dc704a xbmk: remove even more eval statements 
* 7f8d85140f xbmk: remove the setcfg function 
* 0a74cc8ec6 xbmk: clean up a few err calls 
* a09ec1d02b xbmk: remove more eval statements 
* 0605fbe720 xbmk: general cleanup: unroll condensed code lines 
* e1c70f4319 vendor.sh: remove superfluous AND 
* e1c580f6bc grub/xhci_nvme: fix target.cfg 
* 093606784a add fedora42 dependencies from bauduser 
* 5e35b0684d dependency/debian: libstdc++-arm-none-eabi-newlib 
* af88a066d0 grub/xhci: rename to grub/xhci_nvme 
* d90defeae3 mrc.sh: remove superfluous eval statement 
* a74af6aa05 tree.sh: remove superfluous eval statements 
* 15cefca84b rom.sh: remove superfluous eval statement 
* 2b4b5bf82e inject: remove superfluous use of eval 
* 85b10a674b init.sh: reduce the use of eval statements 
* 99f2c0fcf9 get.sh: reduce the number of eval statements 
* be1f4ebb9c get.sh: allow force-pull via -F instead of -f 
* 24f120d1b8 get.sh: only pull if the local revision is missing 
* 084b8b65c6 u-boot: make the libreboot logo rainbow again 
* 2ac51f442b init.sh: break up xbmk_set_env 
* 8b351e51aa tree.sh: break up check_gnu_path to subfunctions 
* 3b6d2b799c vendor.sh: break up the extract_kbc1126 subshell 
* 347330a611 coreboot/x2e_n150: rename config to fspgop 
* f272c4d1c4 rom.sh: support "fspgop" init mode 
* aa3ccf0433 fix up old comment in vendor/x2e_n150 
* b786918b2b vendor.sh: use bsdunzip, not unzip 
* c6939cf390 libarchive: also copy bsdunzip and bsdcpio 
* 06c1ed009d add -p flag to old me_cleaner too 
* e8a3cd8cd0 remove unar from dependencies 
* 8e8f29c2e5 vendor.sh: remove false error message 
* 4075c8be38 vendor.sh: use bsdtar, not unar 
* e527820ceb vendor.sh: don't use unrar 
* 7b297a44ce config/git: Support building libarchive 
*   d4f5fdec06 Merge pull request 'New mainboard: X2E_N150' (#361) from Riku_V/lbmk:x2en150 into master 
|\  
| * b4c3bafb0e New mainboard: X2E_N150 
* | 9da4fa64a6 coreboot/default: allow alderlake fsp in releases 
|/  
* c9d6143e20 gru bob/kevin: make u-boot bootflow timeout 8secs 
* fa6c3512d6 rom.sh: remove TODO note 
* 574fd30cde inject.sh: don't exit from patch_release 
* 4fbafaaa46 init.sh: remove comment in pybin 
* 204e310f5a get.sh: remove unnecessary check in try_copy 
* b4fbdb448d get.sh: rename try_file to try_fetch 
* e42cb4f4cd xbmk: tidy up some if statements 
* f5060232e1 init.sh: remove TODO note 
* 0303167e25 init.sh: create TMPDIR *after* suid check 
* 1a74172a17 release.sh: use cache src on release builds 
* 6bb4e2c72a init.sh: remove symlink check on XBMK_CACHE 
* dc722b5bb8 init.sh: switch back to old TMPDIR checks 
* 4686ba8a4a make notices a bit more readable 
* 5a8f350bc7 release.sh: fix typo in script: ./mk, not ./mx 
* 8347e2c85d xbmk: cleanup of recent code refactoring 
* 03bf6c185b mk: unroll condensed code lines 
* 0275c60111 get.sh: unroll condensed code lines 
* 193001bc71 init.sh: unroll condensed code lines 
* ace167445f inject.sh: unroll condensed code lines 
* 94ab695457 lib.sh: unroll condensed code lines 
* 8d5d6a3e2a mrc.sh: unroll condensed code lines 
* 45c4d4045c release.sh: unroll condensed code lines 
* f5c91ff0ee rom.sh: unroll condensed code lines 
* e282586427 tree.sh: unroll condensed commands 
* 8b3f476b57 vendor.sh: unroll condensed code lines 
* 6b796e2b4c init.sh: make TMPDIR *after* calling xbmkpkg 
* 2c02b17810 lenovo/t440p: add a target with 4mb cbfs 
* c11c5a7a14 Revert "xbmk: don't use backticks for command substitution" 
* 14bcb3a6fa config/dependencies: cleanup 
* fe301a019b vendor.sh: add missing error handle on sha512sum 
* c734a6e757 tree.sh: fix bad variable reference 
* 995963baf4 xbmk: much more verbose error messages 
* 7bed68f5b7 lib.sh: use xprintf in err() 
* edcf8cead8 lib.sh: use xprintf in x_ 
* 333739961a lib.sh: check args for errors in fx_ and dx_ 
* ce5127e46b inject.sh: add missing semicolon in case 
* 0deac58e41 vendor.sh: tidy up apply_deguard_hack() 
* c738698cca vendor.sh: tidy up extract_intel_me() 
* e9c7338cda vendor.sh: clean out 7ztest on startup 
* 07cda7fa12 vendor.sh: remove unnecessary rm -Rf command 
* 4999a49de3 xbmk: don't use backticks for command substitution 
* 5cfe54b06d Revert "inject.sh: put tmpromdel in xbtmp, not cache/" 
* 9d32af58eb Update arch dependencies 
* 269fa65b93 init.sh: write-protect the lock file on startup 
* dea587a16f Revert "git/grub: use codeberg as the main mirror" 
* c3e52bc2ee Revert "change grub git again" 
* a00f4153c6 Revert "vendor.sh: put _pre_dest in xbtmp, not cache/" 
* 2873ed5e7e Revert "vendor.sh: put appdir in xbtmp" 
* 1e07c4eb02 change grub git again 
* 53491bdca2 tree.sh: don't combine remkdir/cd gnupath 
* c4cd9c08df release.sh: delete xbmkwd on src tarballs 
* 4c74311eae vendor.sh: put appdir in xbtmp 
* a444910bf2 inject.sh: put tmpromdel in xbtmp, not cache/ 
* 23fb10c3f5 mk: include mrc.sh before inject.sh 
* 69934d18cc vendor.sh: put _pre_dest in xbtmp, not cache/ 
* 4475b4db4b init.sh/tree.sh: put PATH dirs in xbtmp 
* f5e8483f41 init.sh: bail if date is non-GNU (for now) 
* 882a6917bc lib/init.sh: sanitize the version string 
* 1c02f2a770 release.sh: put vdir in xmtmp, not XBMK_CACHE 
* 5096e0040f mk: hardened PWD check (deny symlinks) 
* d06d6a1905 git/grub: use codeberg as the main mirror 
* 92ecf145fa get.sh: put tmpgit in xbtmp 
* ddbefea475 Revert "get.sh: put tmpclone dirs in xbtmp" 
* aa38608bff Revert "tree.sh: add missing colon at the end of trees()" 
* f6d5b44757 Revert "vendor.sh: looser error handling on find_me" 
* c9a81292e5 vendor.sh: looser error handling on find_me 
* 769d645c2a init.sh: create separate lock in release dirs 
* 568887cd5e tree.sh: add missing colon at the end of trees() 
* 01a779d4eb get.sh: put tmpclone dirs in xbtmp 
* 0d107ad872 xbmk: remove xbloc, re-use xbtmp instead 
* 64b69907ab init.sh: put TMPDIR inside xbmkpwd, not /tmp 
* 8a8be1dec9 get.sh: remove unnecessary variable, repofail 
* 3af4c7bcb1 rom.sh: simplify rmodtool copy handling 
* bec4c63b25 release.sh: remove support for the -d flag 
* e059614589 get.sh xbget: don't use eval for file/dir checks 
* 934e89510e vendor.sh: remove unnecessary error check 
* b1f485d0f2 release.sh: fix broken release lock file handling 
* ed84d33e59 lib.sh and rom.sh: stricter mktemp usage 
* 40f064ae33 inject/vendor.sh: stricter set + usage 
* 24a8226fee init.sh: tidy up xbmk_set_env 
* 7c04cd37b5 init.sh: tidy up xbmk_child_set_tmp 
* 2d20be3d99 init.sh: remove unnecessary lockfile checks 
* c148b220d8 init.sh: move TMPDIR handling to own functions 
* f96bf2b2cd init.sh: prevent race condition with TMPDIR 
* bbce74d78a init.sh: MUCH safer TMPDIR handling 
* ad58364e18 mk: simplify the main script check 
* 09646783a5 config/dependencies: add --no-install-recommends 
* d9011da0eb inject.sh: redirect stderer to /dev/null FIRST 
* 50a0bac01c vendor.sh: tidy up extract_intel_me 
* 9fb707b49b rom.sh: safer use of cat in copyps1bios 
* 89238c0579 tree.sh: tidy up check_cross_compiler 
* 07562e3f28 tree.sh: don't re-check xgcc needlessly 
* 66f1be1ba7 tree.sh: check xgcc AFTER checking elfdir 
* c12965f8e4 lib.sh: safer pad_one_byte function 
* 7e6f52ec0a vendor.sh: use pad/unpad functions 
* 616ef52a6f lib.sh: additional error handling on cat 
* 41945a2220 tree.sh: Delete files *before* updating hashes 
* d44c143846 tree.sh: rename hashtype to hashname 
* 289c4e1c2f tree.sh: rename hashname to hashdir 
* b31f2387ee tree.sh: rename hashvar to badhashvar 
* 2c24b94d80 tree.sh: rename function and remove comments 
* ef79b11082 tree.sh: don't delete builds if tree==target 
* 1c5c28f2cb tree.sh: re-add comments to check_hashes 
* 3d5a6bccae tree.sh: unified project hash handling 
* a499d5bba2 rom.sh: Don't run mkhelpers if release=n 
* 84a1ff85b0 coreboot/default: rev 9e41c7cec7, 18 July 2025 
* cc2f08e7bb Revert "coreboot: rev 8b52167a9f 13 July 2025, rebase t480" 
* 32dfdfbb01 coreboot: rev 8b52167a9f 13 July 2025, rebase t480 
* 122d009af1 coreboot/default: probe EDID twice in libgfxinit 
* dac3d6d06a rom.sh: Don't build coreboot utils if dry=":" 
* 6f7525a5b3 cache coreboot builds in elf/ again 
* 7c6c9ff547 tree.sh: delete individual target builds if needed 
* fb95230a4c tree.sh: Remove redundant deletion 
* e25bca2ef7 T480/T480: Drop redundant PcieRpEnable from dt 
* cc82b733d3 tree.sh: Place target builds under tree/target/ 
* 9541dfcefa rom.sh: bump pcsx-redux copyright date to 2025 
* 91a63ccd1f hppro3500: enlarge CBFS to match the BIOS region 
* ed839db0a0 pcsx-redux: bump rev 8f8cc3d5, 20 June 2025 
* dd65f55b29 coreboot/default: bump rev 812d0e2f62, 5 Apr 2025 
* f6da49b3a7 deguard: bump to rev 0ed3e4f 
* 81dc1a7f89 GRUB: Bump to rev a68a7dece, 23 June 2025 
* 5b2661a485 SeaBIOS/default: Bump to rev b686f460, 28 Jun 2025 
* 248192ad9a ifd/hppro3500: use truncated ME, enlarge BIOS size 
* 1cd8353082 ifd/hppro3500: unlock regions by default 
* 67858207eb ifd/hppro3500: set HAP bit by default 
* a13772bf31 cb/hppro3500: use seagrub, not grubsea 
*   047ddf40d8 Merge pull request 'Add HP Pro 3500 Series' (#350) from JoelLinn/lbmk:feature-port-hppro3500 into master 
|\  
| * 587af4a7b6 Add HP Pro 3500 Series 
* d25aaac9ad lib.sh: remove erroneous break from fx_ 
```

Build testing
-------------

This release was built on the latest Debian 13 stable release, as of
this day. It was also build-tested successfully on the latest Arch Linux updates
as of 25 December 2025. Other distros (e.g. Fedora) are likely fine as well.



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
