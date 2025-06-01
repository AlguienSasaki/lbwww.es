% Libreboot 20240612 released!
% Leah Rowe
% 12 June 2024

A corresponding [Libreboot 20240612](https://canoeboot.org/news/canoeboot20240612.html)
release is also available.

**[PLEASE READ THESE INSTRUCTIONS BEFORE INSTALLING](../docs/install/ivy_has_common.md), OR
YOU MAY BRICK YOUR MACHINE!! - Please click the link and follow the instructions
there, before flashing. For posterity,
[here is the link again](../docs/install/ivy_has_common.md).**

Open source BIOS/UEFI firmware
-----------------------------

Libreboot is a free/open source BIOS/UEFI replacement on x86 and ARM, providing
boot firmware that initialises the hardware in your computer, to then load an
operating system (e.g. Linux/BSD). It is specifically
a *[coreboot distribution](../docs/maintain/)*,
in the same way that Debian is a Linux distribution. It provides an automated
build system to produce coreboot ROM images with a variety of payloads such as
GRUB or SeaBIOS, with regular well-tested releases to make coreboot as easy
to use as possible for non-technical users. From a project management perspective,
this works in *exactly* the same way as a Linux distro, providing the same type
of infrastructure, but for your boot firmware instead of your operating system.
It makes use of [coreboot](https://www.coreboot.org/) for hardware initialisation,
and then a payload such as [SeaBIOS](https://www.seabios.org/SeaBIOS)
or GRUB to boot your operating
system; on ARM(chromebooks), we provide *U-Boot* (as a coreboot payload).

This is a *bugfix* release, and is considered stable. It fixes a series of bugs
that were discovered in the previous Libreboot 20240504 release from 4 May 2024.
The [errata](libreboot20240504.md#errata) on Libreboot 20240504 meant that all
ROM images had to be removed, so a new stable release had to be made ASAP to
compensate.

A new *testing* release is planned for July, adding many more new motherboards;
today's stable release only fixes bugs and adds some new features to the build
system, which have been rigorously tested during the course of the recent audit.

The changes of the recent [5th build system audit](audit5.md) are included, in
this release, in addition to a few minor fixes made since that date. The audit
was completed on 9 June 2024 and today is 12 June 2024. The release came unstuck.

Changes since Audit 5
--------------------

Audit 5 was only recent, and forms most of the changes in this release, so look
further down for a list of those changes or read [the audit 5 page](audit5.md).

Some minor changes have been made in the few days since completion of that
audit, namely:

* Add a patch from Nico Huber fixing the Intel graphics device on certain
  Xeon processors that can be used on Haswell macchines. Unlikely to be used
  by Libreboot users, but for example it has been discovered that the Dell
  Precision T1700 is mostly compatible with the 9020 MT images; some GPIOs and
  such may not be configured correctly yet, and a proper T1700 port is not yet
  available.
* Add a patch from Mate Kukri fixing DP++ on Dell OptiPlex 9020 ports; enables
  use of a passive adapter for HDMI/DVI on the displayport.
* Coreboot trees: use coreboot's own nasm mirror as backup, instead of the
  macports mirror that was used in audit5. In audit5, a feature was added where
  crossgcc tarballs are downloaded by lbmk, with redundant links, rather than
  relying on the coreboot build system to do this. (and this means that cross
  gcc tarballs are now included in the release tarballs, enabling fully offline
  builds on boards that don't need to download vendor files at build time)
* NVMe patch for GRUB payload: it is still present, but it is now *only* enabled
  on boards that can physically *have* NVMe SSDs; on boards that don't need the
  patch, it is not present. This means there are three GRUB trees: `default`
  which most boards use and lacks nvme/xhci support, `nvme` which contains the
  NVMe support and `xhci` which contains both xHCI and NVMe support. Each board
  is configured to use the appropriate GRUB tree, as required.

The reason for separating the NVMe patch to only those boards that need it, is
precisely to avoid any potential issues if a board doesn't need it. The NVMe
patch has been extensively tested, on all of the boards that actually have it.

Audit 5 changes
---------------

Since the recent audit 5 changes are included in this release, the changelog
of that audit has simply been copied for sake of efficiency. Firstly:

### Modest code size reduction

There are 1482 lines of shell script in the build system, versus 1680 in the
Libreboot 20240504 release. Libreboot's build system is written purely in
POSIX sh; not BASH, not KSH, not ZSH, jush sh!

This is a difference of 198 lines, or a 12% reduction. Despite the reduction,
numerous features have been added and a large number of bugs were fixed.

Summarised list of changes
--------------------------

Changes are in order per category, from newest to oldest:

### Feature changes

* **Download crossgcc tarballs as dependencies, when cloning coreboot.** We
  previously relied on the coreboot build system, which automatically fetches
  these when running `make crossgcc`, which we run automatically. However,
  the coreboot logic has to be patched for reliability because the GNU HTTP 302
  redirect often fails so we use a static mirror, and the logic has no
  redundancy. With this new change, we use the same tarballs but we specify
  two URLs, a main and a backup. This also means that the tarballs will once
  again be included in Libreboot release archives, enabling offline builds.
* **Support downloading files as submodules, in Git repositories.** This
  complements the pre-existing feature where sub-repositories (Git) can be
  cloned into a subdirectory of a given main repo. We use this for crossgcc,
  as referenced above.
* New files under `config/dependencies/` for Fedora 40 and Ubuntu 24.04. Now
  you can run `./build dependencies fedora40`
  and `./build dependencies ubuntu2404` on each respective distro, to get
  the right build dependencies for building Libreboot from lbmk.
* **NEVER** run `git submodule update`, *ever*. Instead, rely *solely* on
  config/submodule/ to define which dependencies should be downloaded, to each
  given subdirectory within a main project. This is using a feature described
  later on (in this audit report), whereby projects can have redundant
  submodule repositories defined; initially, this feature was an *override*
  where otherwise the submodule update command would be executed if
  the `.gitmodules` file existed for a given project; this override is now
  the *only* way to do it, and is thus the default behaviour. This may be
  considered a preventative bug fix, in case certain projects auto-download
  submodules that might cause us trouble in the future. It's better that we
  maintain tight control of submodules.
* **Summarising the next few changes mentioned below: out-of-source builds
  are now fully supported, for both single- and multi-tree projects.** (it was
  previously only supported on multi-tree projects)
* Moved builds of coreboot utilities (e.g. cbfstool) to `elf/utilname`,
  e.g. `elf/cbfstool/default/cbfstool` would be the new cbfstool binary location
  for the one build from coreboot in the `default` tree.
* script/trees: Now single-tree builds are skipped if a build exists
  under `elf/projectname/`, based on the presence of a `build.list` file; this
  is consistent with the same behaviour pre-existing for multi-tree projects.
* When building memtest86plus, the binary is now placed out-of-source,
  into `elf/memtest86plus`.
* When building flashprog, the binary is now placed out-of-source,
  into `elf/flashprog/`.
* Use new function `singletree` to decide whether to use submodules, rather
  than hardcoding a check for *coreboot* - NOTE: use of submodules was later
  disabled during this audit, replaced with custom handling in lbmk.
* For error exits caused by *improper commands* (as opposed to fault conditions
  while processing valid commands), don't directly call `err`; instead, call a
  newly written function `badcmd` which says that much, and links to the
  website (if `docs/` is present as in releases, it also points there).
* Added a `projectsite` file pointing to libreboot.org, complementing the
  existing `projectname` file which contains the word `libreboot`. This is
  used in the `version` command.
* **GRUB is now a multi-tree project.** Each given coreboot target can
  specify which GRUB tree it wants to use, each containing its own revision
  and patches, with its own GRUB configuration file. This can be used later on
  to provide specific optimisations on each given motherboard, but it is used
  at present to exclude xHCI patches on boards that don't need it; please also
  read the bugfix section (of this audit report) pertaining to this same topic,
  for more context. Before this change was implemented, all motherboards used
  the exact same GRUB revision, with the same patches and the same config.
* grub.cfg: scan `grub2/` last, on each given device/partition; this speeds
  up the boot time in most tests, because most setups use `grub/`,
  but `grub2/` is still used on legacy setups so we have to support it and, for
  reasons mentioned in the bullet point below, GRUB is very inefficient at
  generating the list of devices/partitions when using the `*` wildcard, so
  we can't scan `grub*/`.
* grub.cfg: it now scans a reduced set of devices/partitions by default, while
  still ensuring (in practise, on real systems) that all such devices and
  partitions will be scanned. We hardcode this, because the `*` wildcard in
  GRUB is *very slow* on some machines, due to the way the GRUB kernel
  constantly re-initialises the list of devices and partitions during operation.
  Scanning an *excessive* number of hardcoded device/partition numbers slows
  down the boot too, so this has been optimised. It has been tested and it
  shouldn't cause any issues on machines/setups that people actually use.
* **grub.cfg: scan distro-provided grub.cfg from ESP;** we previously only
  scanned the ESP for isolinux/syslinux configurations (which GRUB can parse).
* grub.cfg: Don't search for `*_grub.cfg` as this slows down the bootup
  sequence, and nobody really uses this anymore; Libreboot's GRUB is much more
  robust these days, pretty much booting anything automatically, but you used
  to have to (regularly) use a `libreboot_grub.cfg` file to override the default
  one provided by your distro. This legacy cruft has been removed, entirely!
* Added the `nuke()` function to delete files systematically, based on the
  presence of a `nuke.list` file which contains files/directory paths relative
  to the root directory of a given Git repository. This is used to delete a
  poorly licensed source file in U-Boot (strlcat.c).
* **T440p/W541 laptops: Enable NVMe SSDs in `grub_scan_disk`**; an e-key adapter
  can be used in the WLAN slot, to add an NVMe SSD. Even though throughput
  is limited by the x1 PCI-E width, it's still a viable upgrade as it offers
  slightly improved read/write performance compared to any SATA SSD.
* script/roms: Allow to override `grub_scan_disk` via `-s`, for
  example: `./build roms -s nvme t1650_12mb`
* **grub.cfg: Use `grub_scan_disk` to set boot order (rather, boot order by
  device type).** It is possible now to configure each motherboard with this
  variable, so that certain types of devices are scanned in a precise order;
  for example, scan NVMe SSDs first.
* **include/git.sh: Allow manual override of `git submodule` handling**, instead
  directly downloading Git repositories using `git clone`, into the subdirectory
  of a given main Git repository (as per `src/projectname` scheme). With this
  feature, it is possible now to specify a *backup* submodule repository, for
  redundancy, all while still allowing to reset the revision (and *patch* the
  given submodule). This has been used to provide greater redundancy when
  downloading coreboot submodules. It also allows to *limit* the number of
  submodules, so now we only download the ones we need, thus saving bandwidth
  especially during very large and long build sessions. - *NOTE: this was
  later changed so as to be the ONLY method for downloading submodules, skipping
  the actual git-submodule-update command entirely, on all projects.*
* **Native NVMe driver added to the GRUB payload**, allowing users to boot from
  NVMe SSDs where present on a given motherboard. The patch is courtesy of
  Mate Kukri, who ported SeaBIOS's own NVMe driver, converting all of the
  code to run properly within GRUB's own kernel. NVMe SSDs are now fully
  bootable on all machines that can have them, offering vastly superior
  read and write performance when compared to SATA SSDs.
* include/git.sh: Allow patching git submodules (NOTE: support for submodules
  was removed entirely, later in the audit, in favour of custom logic in lbmk
  for the downloading of such dependencies).
* Added Portuguese keyboard support in the GRUB payload (patch courtesy of
  the contributor by alias `samuraikid`).
* Removed all help commands, because it's just a duplication of documentation
  that is already included in releases anyway, and people using the Git
  repository require internet access anyway, so they can just use the website.
* Main build script: removed the functionality for generating source tarballs
  where the only source code included is U-Boot; we do not need this, because
  the larger source tarball containing all of Libreboot also contains U-Boot.
* include/option.sh: Don't bother checking for GNU Tar, because we were only
  using it for reproducible tarball generation which didn't work yet anyway;
  there are still ways of doing it with BSD tar and so on.
* Print a two-line break before confirming the location of the generated
  release archive, when running release builds. This makes it more obvious
  to the operator.
* **Removed the MRC (vendor raminit code) on Intel Haswell** (4th generation)
  hardware, namely the ThinkPad T440p, W541, Dell OptiPlex 9020 MT
  and Dell OptiPlex 9020 SFF; the libre raminit now works well, and S3 works.
* Removed all status checks from script/roms (formerly script/build/roms),
  because it's better to document this instead, and rely on testing regardless.

### Bug fixes

Some of these changes fix actual issues that were found in testing, while
others were fixed *before* being triggered/reported and are thus *preventative
bug fixes*. The logic in lbmk has been very intensively audited as is customary!

The changes are, from newest to earliest:

* script/trees: Exit with error status if a given project is not defined. It
  was previously decided that this script could be used to directly run Makefiles
  from any given directory, but this is no longer done as it was error-prone;
  this change prevents such usage, thereby preventing unstable conditions within
  the build system.
* **Create a lock file when running lbmk.** Only do it from the main parent
  instance, but not child instances of it; delete it at the end, after exiting
  from the parent process. If starting a separate parent process, that one
  will now immediately exit (with error status) if the lock file exists. This
  prevents the fault condition where the user accidentally runs the same lbmk
  instance twice, from the same work directory; it is only designed to be
  executed once, per work directory. This is similar to the locking feature
  you find in package managers such as apt-get. Also do this in release/
  directories, while building (but don't include a lock file inside the tarball).
* include/git.sh: When doing a global check for files in every project all at
  once, as defined by each respective (if existent) `nuke.list` file, hide
  the output. Only show the output when running it on a specific project, not
  the one in the for loop. This prevents user confusion / false bug reports.
* include/git.sh: Download coreboot as defined by `xtree` *before* downloading
  the main project that defined it, to prevent a situation where the main project
  is downloaded successfully but not the dependency (defined by `xtree`); this
  is to maintain the integrity of the build system under fault conditions.
* include/lib.sh: When a download fails (running the `download` function),
  don't then say that the file is "missing". Instead, actually say that the
  download failed, so that the operator has a better understanding.
* include/lib.sh: Hide stderr on the `download` function, for the initial
  check when verifying an existing file; although no problem existed on
  technical terms, the output was confusing because it made the user think
  there was a problem. The logic then downloads and re-verifies, and the
  output indicating *that* verification has not been hidden; if the file
  already exists, this is simply indicated by `e()`. This is considered a bug
  fix, because it fixes the bug where users made erroneous bug reports, by
  re-engineering the situation so that they do not make such erroneous reports.
  TL;DR hide a totally benign (non-)error message.
* include/git.sh: Provide better user feedback about what is being downloaded
  and where - although nothing was broken before, this lack of feedback was a
  bug because it made debugging harder. Provide more clarity for the user.
* include/git.sh: Download dependencies *before*, not *after*, downloading the
  project sources that depend on it. For example, pico-serprog depends on
  pico-sdk. If you were to download pico-sdk *after* pico-serprog, the latter
  may be downloaded and placed in src/, but then the former (sdk) could fail
  due to bad internet, and now the overall downloaded code is corrupt, and there
  was nothing checking for this after the fact; checking for it would be bloat.
  By downloading the dependency *before*, then if *that* download fails, so
  does the main one, and integrity is maintained within the build system.
* Preventative bugfix: don't check empty paths in `copy_elf` (of script/trees),
  even though this potential bug was not yet triggered. Play it safe.
* script/trees: Don't check pre-existing builds in elf/ if `build.list` is
  missing, otherwise it's too soon and builds are prevented in the first place;
  this was caused initially when supporting out-of-source builds for single-tree
  projects, as was already done on multi-tree. Now this is fixed.
* Documentation: only define the Untitled Static Site Generator
  in `config/git` - the dependencies (markdown files and images) are now
  defined in config/submodules/ instead. This prevents the bug where you could
  download one of the dependencies first which would make the main project,
  Untitled, un-downloadable, since the dependency projects go in subdirectories
  of the main project that depends on them.
* Handle serprog dependencies in config/submodule instead of relying on
  the git submodule update command, and only provide necessary modules. This
  prevents the bug where downloading a dependency first later prevented the
  main project from being downloaded, if the dependency was in a subdirectory
  of what depends on it.
* Build coreboot utilities on a number of threads as defined by `XBMK_THREADS`;
  although they already compiled, they would always do so on a single thread,
  which is considered a bug. Now they can be compiled on multiple threads.
* include/lib.sh: Don't use `./update trees -f` to build coreboot *utilities*,
  because it's quite error prone and not what that script is designed to do;
  it is only designed to operate based on strictly defined single- and
  multi-tree projects. Instead, call `make` directly.
* Don't use the presence of a `build.list` file to detect a multi-tree project
  when running `./update trees`; instead, check the presence of `target.cfg`
  down one level from `config/project/`, so: `config/project/*/target.cfg`
  instead of `config/project/target.cfg`. This way, if someone working on lbmk
  accidentally adds that `build.list` file in the wrong place, lbmk won't
  become unusable. This also means that single-tree projects can now provide
  a `build.list` file! (and some of them now do - look at the features section
  on this page)
* Move check for *root user* to include/lib.sh, *before* the version/versiondate
  files are written; these files need to be writeable by the standard user,
  otherwise lbmk will exit. If you run lbmk as root, except when running the
  dependencies command, it exits with error status; ironically, that very same
  check then prevented running as root-root, causing lbmk to become unusable
  until those files were either deleted or had ownership changed. This fix
  prevents the bug from occuring ever again, but people who were previously
  affected still have to fix these files (if they were written as root).
* Move dependency handling to include/lib.sh, *before* the version/versiondate
  files are written, and *exit* before they are written; this prevents writing
  the version/versiondate files as root, which previously occured when running
  a command such as `./build dependencies debian` (installs build dependencies
  from apt-get on a Debian machine). This bug ironically prevented lbmk from
  running at all, under such conditions, because the dependencies script
  required root, but lbmk exits with error status if running anything else as
  root, and if version/versiondate are owned by root, that prevents lbmk from
  running because writing to these files is the first thing it does, so an exit
  with error status would otherwise occur.
* config/git/: Bump to a newer revision of Untitled (static site generator),
  which thereby also imports the same fix as described in the next bullet
  point below, because Untitled had (and now no longer has) the exact same bug.
* include/lib.sh: check environmental variables properly, for example
  check that `${XBMK_RELEASE+x}` isn't unset; it was previously grepping
  the output of `set`, which led to a bug report by a user who had the
  variable `TMUX_TMPDIR` set, whereas `TMPDIR` was unset and lbmk was checking
  the latter; in this example, the bug caused lbmk to act as though `TMPDIR`
  was set, when it in fact wasn't, and code that used it then crashed because
  lbmk does `set -u -e` (and it does this precisely to catch such bugs like the
  one you're reading about now so that they can be fixed, like this one was!)
* **Re-configured GRUB so that only the Haswell and Broadwell machines contain
  xHCI support**, where it doesn't cause any issues (and is required), while
  other motherboards use a version of GRUB that lacks support for xHCI. This is a
  mitigations against the bug reported in [lbmk
  issue 216](https://codeberg.org/libreboot/lbmk/issues/216). This is done, by
  using the new *multi-tree* GRUB handling, which is mentioned above in
  in the section (of this audit report) pertaining to *feature changes*, whereby
  each motherboard can have its own GRUB revisions and patches, with its own
  GRUB configuration file (that could be uniquely optimised for it).
* **Fix vboot build issue when running lbmk in i686 (32-bit) host machines**.
  The patch, courtesy of *Luke T. Schumaker*, adapts vboot's vmlinuz extract
  function so that it uses pointer logic directly, instead of defining
  integers (of type `ssize_t`) which, the way it was written, caused GCC to
  believe that there would be a buffer overflow in code; the new code is more
  robust and should prevent such an issue. This is both an *acute* bug fix,
  fixing a bug that was actually triggered, and a preventative bug fix as the
  original code wasn't correct either, even on AMD64 hosts (where it happened
  to compile anyway).
* include/vendor.sh: Skip a given file if the path to it is `/dev/null` - this
  fixes a bug exposed by the previous change two bullet points down (fine
  grained error control), because VGA ROMs are handled but the KGPE-D16 target
  mitigates a crash bug when PIKE2008's option ROM is executed by SeaBIOS, by
  inserting fake (empty) option ROMs for it into CBFS, and it does so by
  telling coreboot to insert a *VGA option ROM* with the correct PCI device
  and vendor ID, pointing (you guess it) to *`/dev/null`*. This makes the
  KGPE-D16 target now return (when run through `./vendor download`) with zero
  status, instead of error status.
* Do not allow dashes in coreboot target names, to mitigate a bug exposed by
  the previous change listed below (regarding fine-grained error control). This
  fixes build errors such
  as: `include/lib.sh: line 115: kcma-d8-rdimm=config/vendor: No such file or directory`;
  dashes were replaced with underscores, on those target names.
* include/vendor.sh: Much more fine-grained error control, when running
  the `download` command. Certain parts need sh error handling to be less
  strict, so `set +u +e` is used; when it's safe to turn on strict error
  handling again, `set -u -e` is used. It used to be that `set +u +e` was the
  only behaviour, when running this command. This actually exposed a number
  of bugs that were previously hidden.
* include/vendor.sh: Don't exit with error status when running those commands
  on a target that has no configs; instead, skip those targets.
* **GRUB: Never run it as a primary payload on any target but QEMU**. This is
  a preventative bug fix, after lbmk bug report issue 216:
  <https://codeberg.org/libreboot/lbmk/issues/216> - although it was caused by
  the xHCI patches, and only happened on Sandybridge hardware, and although
  this was later removed on those boards, GRUB is very complex and likely has
  a lot of memory corruption issues. SeaBIOS is more reliable, so: Libreboot
  only provides *SeaBIOS* as primary payload, but allows you to execute GRUB
  from the SeaBIOS menu (the very same GRUB). Additionally: lbmk already
  supported a configuration whereby SeaBIOS reads a `bootorder` file in CBFS,
  making it try to run the GRUB payload first, while still allowing you to
  interrupt by pressing ESC to bring up an alternative boot select menu. This
  is now the *default*, on all x86 motherboards. This is a mitigation against
  future instability in GRUB because, if such issues happen again, it will not
  cause a brick since you can just use SeaBIOS instead, and skip booting to
  the GRUB payload (on the affected machines, BIOS GRUB still worked, which
  your distro provides and SeaBIOS executes it). *NOTE: GRUB was later made
  into a multi-tree project, with certain motherboards using a version that
  has the xHCI patches, if required, because the machines that actually need
  xHCI support were not affected by the bug referenced in issue 216.*
* Main build script: Check SUID before checking Git name/email, otherwise the
  version/versiondate files could be written as root and thus prevent building
  of lbmk, which (for most commands) is intentionally engineered to exit (with
  error status) if you run it as root.
* script/trees: Reset variable `makeargs` per target, so as to prevent
  pollution of this variable when switching from one build target to the next.
* script/trees: Added `UPDATED_SUBMODULES=1` to the make command when running
  any coreboot `make` command, to prevent coreboot from automatically fetching
  certain Git submodules; this is a preventative fix, fixed before it became
  a bug, which it likely would have become at some point as this is exactly
  what the coreboot build system does!
* Main build script: hide the output of `git init` when lbmk re-initialises the
  Git history, to prevent its output from being wrongly inserted into the
  output of commands such as `./build roms list` - such pollution would cause
  build errors, so it's important that the Git initialisation function either
  doesn't output anything, or that it should cause an *exit* if output is to be
  required.
* Added the `CHANGELOG` file to `.gitignore`. This means `./update release`
  will now work, on release archives, because lbmk re-initialises Git history
  when doing so, but the CHANGELOG file (when present) causes lbmk to skip
  all source downloads (which the release builder relies on).
* **Fix garbled output on 1440x900 monitors when using the Dell Latitude E6400.**
  The E6400 uses a reference clock (`DPLL_REF_SSCLK`) set to 100MHz, whereas
  libgfxinit assumed 96MHz. This timing descrepancy did not cause an issue on
  lower resolution displays, so we never caught it in earlier testing. Patch
  courtesy of Nicholas Chin, who debugged this issue alongside the user who
  reported it. It was fixed by making such timing configurable, within the
  coreboot build system, setting it to 100MHz on Dell Latitude E6400.
* script/roms: Skip a target when its config directory is missing, so that
  running a coreboot target with no configs in it will not yield an error;
  instead, it will now cause a non-error return.
* include/option.sh: If `.git` is missing, in a bare copy of lbmk (not a
  release archive), recreate the version/versiondate files manually so as to
  prevent a build error. Use of `lbmk.git` or the release archives is
  recommended, but some users directly download snapshots of `lbmk.git` from
  sites such as Codeberg, and there's no way for us to turn off this feature;
  even if we did, it may be present on other Git hosting sites, where users
  might host their own copy of lbmk.
* include/option.sh: Don't return non-zero status from the function
  named `mkrom_tarball`, because certain other functions rely on its return
  value to always be *zero*; instead, call `err` which will then yield
  an *exit* (with non-zero status). This means that the function will now
  always *return* zero, when it returns.
* include/vendor.sh: Print an error message when the target is ill-defined,
  for downloading and/or insertion of vendor files.
* include/git.sh: Remove `.git` directories *per-project*, as and when each
  project is being downloaded, instead of having it done all in bulk by the
  main build script. This kicks in when `XBMK_RELEASE` is set (release builds),
  to correct the over-use of disk space during such very large builds processes.
  This makes the build system less likely to OOM when running it inside tmpfs.
* Main build script: initialise Git history *before* running any command,
  because this is required for reliable use of the coreboot build system, which
  the *inject* command makes heavy use of. This reduces the number of errors,
  when running these commands from a release archive, where lbmk re-initialises
  a new Git history when you run it for the first time.
* Main build script: define `xp` as a global variable, to prevent it from
  being lost between functions.
* Fixed missing deletion of strlcat.c in U-Boot sources
* script/roms: Create full release tarball name, when generating releases.
* Main build script: exit (with error status) if not running directly from
  the root of the lbmk work directory.

### General code cleanup

In addition to *general* very sweeping code cleanup, condensing code lines
where possible and so on:

* include/lib.sh: Simplified the `download` function (used for crossgcc tarballs).
* include/lib.sh: Simplified the `singletree` function.
* include/git.sh: Simplified the `link_crossgcc` function.
* include/git.sh: Simplified the `nuke` function, because it was over-engineered
  to the extreme. Now it's more reasonable.
* include/lib.sh: Move download logic here from include/vendor.sh, for the
  feature where *files* can be downloaded as submodules, within Git repositories.
  Please read the notes about this in the *features* section.
* include/lib.sh: Shortened a string in the `e` function, so that the line
  does not exceed a length of 80 characters.
* include/git.sh: Unified the handling of git clone/reset/am commands into a
  single function, rather than duplicating the same logic across multiple
  functions.
* script/trees: simplify the `copy_elf` function; don't create the elf directory,
  create one defined by `dest_dir` instead (which is the elf directory with
  the subdirectory for that project concatenated). Only create it within
  the `copy_elf` function, which is only called if actually compiling the
  code. This avoids creating empty directories under elf/, for example under
  fault conditions.
* include/git.sh: Additional code cleanup, removing certain code that was in
  place because the code used to handle both `git submodule update` and the
  custom *override* logic for submodules; now only the override is used, at all
  times, so the code was cleaned up and optimised only for this.
* include/git.sh: Reduced code indentation in function `fetch_submodule`.
* include/git.sh: Renamed a few variables for increased code clarity.
* script/trees: Unified handling of coreboot `makeargs`.
* Moved function `handle_coreboot_utils` to script/trees (and renamed it
  to `check_coreboot_utils`), as it's only ever used from there.
* Moved variable `cbcfgsdir` to include/vendor.sh, because it's only used there.
* Moved cfgsdir/datadir variables to include/lib.sh, because it's also used
  from script/roms and script/trees; unify them under a common location.
* Handle `build.list` from config/data/, not config/ - this avoids needing to
  check for `build.list` in the `items` function on include/lib.sh, and it is
  now avoided.
* include/lib.sh: More user-friendly output from the `e` function, telling the
  user whether or not a file/directory exists. This is regularly used, for
  example when trying to download a project and the source code was already
  prepared.
* U-Boot on QEMU: removed the (currently) unused x86 target.
* grub.cfg: Split function `try_user_config` into multiple smaller functions.
* grub.cfg: Don't scan ESP on btrfs subvols as the ESP is always on a FAT32
  partition. This saves time during the bootup sequence.
* include/vendor.sh: Remove unnecessary assignment; `dl_fail` was being set
  to `n` and then immediately to `y`. Now it is simply set to `y`.
* Renamed include/option.sh to include/lib.sh
* Main build script: simplified the logic for Git repository initialisation
  by *returning non-zero status*, instead of calling err, and handling this
  return status in the calling function.
* Main build script: condensed the logic for Git name/email checking into a
  simply for loop running `eval`, rather than having lots of separate but very
  similar Git commands.
* script/trees: Removed a few unused variables.
* include/git.sh: Moved logic for copying a Git repository to a new function.
* include/git.sh: Moved function `link_crossgcc` to a different location
  within the file, for proper top-down order of logic (required as per the
  lbmk coding style).
* include/git.sh: Split logic for crossgcc symlinking into its own function.
* include/git.sh: Skip submodule checks if `.gitmodules` missing (NOTE: later
  replaced with custom submodule handling in lbmk).
* include/git.sh: Merged `patch_submodules` in `prep_submodules` (NOTE: ditto
  to the same note below).
* include/git.sh: Split up submodule handling into a new function (NOTE: support
  for submodules was later replaced with custom logic in lbmk).
* include/git.sh: Shortened a few variable names.
* include/git.sh: Removed redundant check for the existence of the patches
  directory, when patching a given project. This is unnecessary, where it was
  removed, because the patching function itself also checks this. Reduction
  in code size by *one line*.
* include/git.sh: Removed function `fetch_from_upstream` and merged its logic
  into calling function `fetch_project_trees`, the only calling function, since
  the logic in `fetch_from_upstream` was very small and splitting made no sense.
* include/option.sh: Renamed `mktar_release` to `mkrom_tarball`.
* script/roms: Renamed function `moverom` to `copyrom`, because it runs `cp`,
  not `mv`, therefore is is *copying* a file, not moving it.
* script/roms: Simplified the logic for listing available serprog build targets.
* script/roms: General simplification of configuration handling for payloads.
* include/vendor.sh: Removed duplicated (and unnecessary) config check,
  because it was already done immediately afterward (I accidentally did the
  same check twice, in immediate succession).
* include/vendor.sh: General simplification of defconfig handling logic.
* Main build script: removed the `excmd` function and merged its logic into
  the `main` function, and then `main` was cleaned up significantly.
* Main build script: don't make `script_path` a global variable; this allowed
  a reduction in code size by precisely *one line of code*.
* Main build script: merged the functionality of function `check_git` into
  the `main` function, then deleted function `check_git` (which was in
  the file include/option.sh).
* Main build script: general simplification of the logic handling source code
  downloads in function `fetch_trees`.
* Main build script: Use `UTC+0000` when initialising git repository commit
  dates (for initial commits).
* Removed the `check_project` function, and placed its logic directly
  inside `include/option.sh` so that it automatically runs in every script
  that sources it.
* Main build script: General cleanup on the code handling file deletions
  under function `fetch_trees`.
* Main build script: delete function `mkversion` and, in its calling function,
  simply print the string contained in variable `relname`.
* Main build script: general cleanup on the logic that handles tarballs.
* Main build script: Remove `mkrom_images`, and move its logic into the only
  calling function within that same file.
* include/option.sh: Removed the function `insert_version_files` and merged
  its logic into its only calling function.
* Unified all logic for handling SHA512 checksums, placing it inside
  include/option.sh for use elsewhere.
* Move image tarball generation to script/roms (formerly script/build/roms).
* Removed redundant function `extract_ref` from include/mrc.sh
* Removed an errant comment from include/git.sh
* Switched to a one-level directory structure for main scripts, rather than
  two-level; for example, script/build/roms is now script/roms
* Merged scripts under script/vendor/ into include/vendor.sh and stub it
  from the main build script
* Merged script/update/release into the main build script
* Merged script/build/serprog into script/build/roms
* script/build/roms: remove unnecessary command (errant return)
* Merged include/err.sh with include/option.sh, into include/option.sh
* script/build/roms: fixed improper use of variable outside a function
* build/build/roms: more reliable exit status in `skip_board()`
* script/build/roms: split up `main()` into multiple smaller functions

Revision updates
----------------

Some revisions were updated as part of standard routine, but happened to be
done during this audit. Those updates are as follows:

### SeaBIOS

Bump SeaBIOS to revision `e5f2e4c69643bc3cd385306a9e5d29e11578148c`, which has
these changes relative to the old one:

```
* e5f2e4c6 pciinit: don't misalign large BARs
* 731c88d5 stdvgaio: Only read/write one color palette entry at a time
* c5a361c0 stdvga: Add stdvga_set_vertical_size() helper function
* 22c91412 stdvga: Rename stdvga_get_vde() to stdvga_get_vertical_size()
* 549463db stdvga: Rename stdvga_set_scan_lines() to stdvga_set_character_height()
* c67914ac stdvga: Rename stdvga_set_text_block_specifier() to stdvga_set_font_location()
* aa94925d stdvga: Rework stdvga palette index paging interface functions
* 8de51a5a stdvga: Rename stdvga_toggle_intensity() to stdvga_set_palette_blinking()
* 96c7781f stdvga: Add comments to interface functions in stdvga.c
* 2996819f stdvga: Rename CGA palette functions
* 91368088 stdvgamodes: Improve naming of dac palette tables
* 70f43981 stdvgamodes: No need to store pelmask in vga_modes[]
* 1588fd14 vgasrc: Rename vgahw_get_linesize() to vgahw_minimum_linelength()
* d73e18bb vgasrc: Use curmode_g instead of vmode_g when mode is the current video mode
* 192e23b7 vbe: implement function 09h (get/set palette data)
* 3722c21d vgasrc: round up save/restore size
* 5d87ff25 vbe: Add VBE 2.0+ OemData field to struct vbe_info
* 163fd9f0 fix smbios data length overflow
* 82faf1d5 Add LBA 64bit support for reads beyond 2TB.
* 3f082f38 Add AHCI Power ON + ICC_ACTIVE into port setup code
* 3ae88886 esp-scsi: terminate DMA transfer when ESP data transfer completes
* a6ed6b70 limit address space used for pci devices.
```

### Flashprog

Updated to revision 5b4fdd1 from 2 May 2024, rebasing the MX workaround patch.

This imports upstream changes, relative to the previous revision:

```
* 5b4fdd1 z60_flashprog.rules: Add udev rule for CH347
* 72c9e40 meson: Check for CPU families with known raw mem access
* 3458220 platform/meson: Port pciutils/pci.h workaround to Meson
* f279762 platform/meson: Check for libi386 on NetBSD
* 14da5f7 README: Convert to Markdown
* 8ddea57 README: Document branching and release policy
* 2522456 util/list_yet_unsupported_chips.sh: Fix path
* cbf9c11 spi: Don't cross 16MiB boundaries with long writes
* 823a704 dediprog: Skip warning on first attempt to read device string
* e8463c8 dediprog: Revise prefix check for given programmer id
* 38af1a1 dediprog: Revise id matching
* 4661e7c amd_spi100: Use flashprog_read_chunked() for progress reporting
* cdcfda2 read_memmapped: Use flashprog_read_chunked() for progress reporting
* 7679b5c spi25: Replace spi_read_chunked() with more abstract version
* ca1c7fd spi25: Normalize parameters of spi_nbyte_read()
* e36e3dc dediprog: Use default_spi_write_256
* 522a86d linux_spi: Use default_spi_read()/_write_256()
* 806509b cli_classic: Turn progress reporting into a progress bar
* 842d678 libflashrom: Return progress state to the library user
* aa714dd flashprog.c: Let select_erase_functions() return byte count
* 2eed4cf serprog: Add SPI Mode and CS Mode commands
* 821a085 dediprog: Implement id reading for SF600 and later
* 274e655 dediprog: Read device string early
* 0057822 dediprog: Add protocol detection for SF700 & SF600Plus-G2
* fb176d2 dediprog: Use more general 4BA write mode for newer protocols
* 0ab5c3d dediprog: Split device type and version parsing
* bdef5c2 dediprog: Use unsigned conversions to parse device string
* 5262e29 dediprog: Try to request 32B device string (instead of 16B)
* e76e21f dediprog: Get rid of some unnecessary hex constants
* 5a09d1e udelay: Lower the sleep vs delay threshold
* 03ad4a4 linux_mtd: Provide no-op delay implementation
* 211c6ec serprog: Refine flushing before synchronization
* 383b7fe serprog: Test synchronicity before trying to synchronize
* d7318ea serprog: Move synchronicity test into separate function
* 9a11cbf Let the flash context directly point to the used master
* aabb3e0 writeprotect: Hook wp functions into the chip driver
* 89569d6 memory_mapped: Reduce `decode_sizes` to a single `max_rom_decode`
* 929d2e1 internal: Pass programmer context down into chipset enables
* 7c717c3 internal: Pass programmer context down into board enables
* e3a2688 Pass programmer context to programmer->init()
* 2b66ad9 Start implementing struct flashprog_programmer
* 4517e92 memory_bus: Drop stale `size == 0` workaround and FIXME
* b197402 memory_bus: Split register mapping into own function
* 0e76d99 memory_bus: Move (un)map_flash_region into par master
* 9eec407 Perform default mapping only for respective chips
* 56b53dd wbsio_spi: Request memory mapping locally
* 5596190 it87spi: Request memory mapping locally
* 46449b4 spi25: Drop stale `bus == SPI` guards
* ab6b18f spi25: Move 4BA preparations into spi_prepare_4ba() hook
* 901fb95 Add prepare/finish_access() hooks for chip drivers
* a96aaa3 dediprog: Support long writes of 16MiB and more
* 1338936 Consider 4BA support when filtering erase functions
* 8d36db6 flashprog.8: Fix up serprog example
* d2ac303 flashprog.8: document new serprog cs parameter
* d1b9153 chipset_enable.c: Add Genoa to mendocino entry
```

As a reminder:

Libreboot now uses Flashprog instead of Flashrom; Flashprog is a fork of
Flashrom, lead by Nico Huber after a dispute with the new leadership of
Flashrom, and it was felt that Flashprog is a better choice for Libreboot.

Git log
------

This entire set of changelogs is based on the precise Git history in lbmk,
relative to Libreboot 20240504 which is from where the audit began.

The latest changes are listed first, going all the way down to earlier changes:

```
* 2ee186ae minor code cleanup in the build system 
* c5441bb9 re-add ability to use cbfs grub.cfg as default 
* d33556c6 trees: exit with error if project undefined 
* 1799a336 build: also make a lock file during release build 
* 78426a97 lib.sh: more useful lock message 
* e80c4b73 create a lock file during builds 
* a0710ef9 git.sh: hide e() output on for loop 
* 86eb566b lib.sh: fix regression 
* fbcdf33f git.sh: download xtree *before*, not after 
* 6a3d8a96 git.sh: fix deletion path in nuke() 
* 3478b288 lib.sh: less confusing error in download() 
* f3f5b99c lib.sh: hide stderr on download() 
* 3440e1f6 lib.sh: simplify download() 
* 75b39dbe lib.sh: fix redundancy in download() 
* 26df6e7a lib.sh: simplify singletree() 
* 9cdf4192 git.sh: further simplify nuke() 
* 1cede024 git.sh: simplify link_crossgcc() 
* 77e482aa git.sh: simplify nuke() 
|\  
| * 046007b4 Add dependency scripts for Fedora 40 and Ubuntu 24.04 
* | a0eb79df add crossgcc tarballs to config/submodules/ 
* | b0d1ad32 git.sh: support downloading *files* as submodules 
* | 1a44fcfa git.sh: remove unnecessary line break 
* | 74ae84af vendor.sh: add a return at the end of mkdirs 
* | c202dc61 vendor.sh: move download logic to lib.sh 
* | 08d0a1d5 lib.sh: shorten a string in e() 
* | 9b00b30a move uefiextract to elf/uefitool/ 
|/  
* 05d301bd git.sh: fix submodule path 
* 7e15859b git.sh: simplify prep_submodules() 
* acd3608b git.sh: unified handling of git clone/reset/am 
* 668bcbf6 trees: simplified copy_elf() handling 
* 3eef7f37 git.sh: simplify submodule handling 
* 4b1b1f50 git.sh: provide feedback for repository downloads 
* d4324768 git.sh: download "depend" projects *before* 
* a4549e93 git.sh: reduced indentation in fetch_submodule 
* 11c47ba7 git.sh: reduced indentation in prep_submodules 
* 9c1ea8f9 git.sh: *never* run git submodule update 
* 137321eb lib.sh: rename variable for clarity 
* 7bfb1d62 trees: don't check empty path in copy_elf() 
* 0b7566cb trees: fix build issue caused by bad elf check 
* 7aa9f224 trees: fix listfile check in copy_elf() 
* 06c78e13 trees: don't say check elf/ if build.list missing 
* dea41f13 trees: don't do elfcheck if build.list missing 
* 3bd562a2 define mdfiles/images in config/submodules/docs/ 
* bff75628 libopencm3 to config/submodules/ on stm32-vserprog 
* d9b9f6db add tinyusb to config/submodule/ for pico-sdk 
* 099ee3f4 config/git: use "depend" for serprog dependencies 
* d0f99c2f trees: unified coreboot makeargs 
* a7889c5a trees: use multiple threads to build cbutils 
* d41658f1 move handle_coreboot_utils to script/trees 
* c0822ac4 put coreboot utils in elf/, not cbutils/ 
* d1ba0851 fix build issue building coreboot utils 
* 7e49fe4b trees: skip single-tree build if a build exists 
* 12774274 use correct memtest86plus path in script/roms 
* 8511615e put memtest86plus builds in elf/memtest86plus/ 
* 176b936d put flashprog builds in elf/flashprog/ 
* 48cbb30d trees: also print "DONE! check elf/dir" on single 
* 315fed5f trees: handle build-test on multi-tree projects 
* b8112af9 git.sh: use singletree() to decide submodules 
* 78f7e429 move cbcfgsdir variable to vendor.sh 
* 810ad480 move cfgsdir/datadir variables to lib.sh 
* ba36f26d handle build.list from config/data/, not config/ 
* bea089bb don't use build.list to detect multi-tree projects 
* 6e1b8087 move id check to lib.sh too 
* 62c25ac7 move root check to lib.sh (bugfix) 
* 75382a41 bugfix: move dependencies handling to lib.sh 
* c6aff769 bump untitled revision again 
* 414a605a bump untitled revision in git config 
* 7d562679 lib.sh bugfix: check environmental variables right 
* 53dd4bc4 lib.sh: more friendly output from e() 
* c2793e7a badcmd: don't print "no context given" 
* 49ae4f91 badcmd: link directly to the maintenance manual 
* 00653aab better help text on invalid commands 
* afac9a06 build: print the project website address on help 
* 1e534e7d add projectsite file: point to libreboot.org 
* 429e91f9 make GRUB multi-tree and re-add xhci patches 
* 9daf7f05 u-boot on qemu: remove currently unused x86 target 
* 6d59f1d0 grub.cfg: scan /boot/grub.cfg last 
* 2becc736 grub.cfg: scan grub2/ last 
* cfc5265f grub.cfg: search a reduced list of devs/partitions 
* 42b5b58d grub.cfg: scan grub.cfg from ESP 
* b3d58f1e grub.cfg: split up try_user_config 
* 2ea5e61c grub.cfg: don't search for *_grub.cfg 
* c742a89d grub.cfg: remove unnecessary path for isolinux 
* e0b2216f grub.cfg: don't scan EFI on btrfs subvols 
*   38135f9e Merge pull request 'Fix building vboot on i686' (#218) from lukeshu/lbmk:lukeshu/i686 into master 
|\  
| * 221206b4 Fix building vboot on i686 
* | a76dda93 vendor.sh: remove unnecessary assignment 
* | 17a9d11d git.sh: do not remove .submodules 
* | 13d4b6d3 delete u-boot test/lib/strlcat.c using nuke() 
* | f6cbc501 import nuke() from cbmk cdce8ba70b 
|/  
* 7fbcb7be coreboot t440p/w541: enable nvme in grub_scan_disk 
* 47f582d4 ./vendor download: skip if file path is /dev/null 
* e7cb10d6 do not allow dashes in coreboot target names 
* e9b9e825 ./vendor download: more fine-tuned error control 
* 0dd0dfaf vendor.sh: don't error on main targets 
* a4bd49de roms: allow user override of grub_scan_disk 
* b00800a7 grub.cfg: actually support setting boot order 
* 4488745c trees: use CPUS=x on regular coreboot make 
* 7d50e09f update gitignore 
* b78f62c7 roms: fix bad eval when comparing options 
* b11e4c9f grub.cfg: add spdx header 
* 3998a3ba re-configure grub_scan_disk on various targets 
* 1c4d6498 remove grub_scan_disk in all target.cfg files 
* e1883f1d grub.cfg: use grub_scan_disk to set boot order 
* c94cecd8 GRUB: remove XHCI patches for now (will re-add) 
* ff2997d6 minor correction 
* d855408a roms: make grubfirst if seabios_withgrub=y 
* ec761c88 coreboot: only run GRUB as a secondary payload 
* 64c64bcf flashprog: bump to 5b4fdd1 from 2 May 2024 
* 914852dd rename include/option.sh to include/lib.sh 
* dc7b72f3 roms: rename bstr variable 
* 5c14e8e1 general code cleanup in the build system 
* 48c2cef8 build: simplify git_init() 
* db06bbdb build: do root check before git check 
* 8d199a31 build: simplify git checks 
* 8da2559b option.sh: fix bad check for version/versiondate 
* d32968c7 trees: reset makeargs per target/project 
* 7bab0cf9 trees: also use UPDATED_SUBMODULES=1 on crossgcc 
* 0a50eaf2 trees: add UPDATED_SUBMODULES to coreboot make 
* ff0840bd trees: write -C on the make command first not last 
* b91ee727 config: add backup coreboot submodule repositories 
* 4a3ebe84 coreboot/default: remove chromeec from module.list 
* 9c5890e9 git.sh: break if a submodule clone succeeds 
* fdb08143 coreboot: only download the necessary submodules 
* 1cb255e8 git.sh: allow finer control of git submodules 
* 5d87eea7 build: hide git-init output 
* b8ec7d56 option.sh: generate version file if .git not found 
* 87c361f3 update/trees: remove unused variable 
* da427272 git.sh: move repo copying to a new function 
* 093c4a36 git.sh: move link_crossgcc to end of file 
* 73a2d991 git.sh: move xgcc linking to a new function 
* d7749876 git.sh: skip submodules if .gitmodules missing 
* c3e1aa34 git.sh: merge patch_submodules in prep_submodules 
* a4163330 git.sh: split submodule handling to new function 
* aa4faf08 git.sh: remove errant line break 
* 00142696 git.sh: remove another meaningless check 
* fc3b0ba8 git.sh: shorter variable names 
* dae10dd4 git.sh: remove meaningless check 
* c148fa53 git.sh: remove variable not meaningfully used 
* 079afb5b add CHANGELOG to .gitignore 
*   0d8781ef Merge pull request 'Fix E6400 display reference clock patches' (#214) from nic3-14159/lbmk:fix-e6400-igpu-ref-clock into master 
|\  
| * 9f50e362 Fix E6400 display reference clock patches 
|/  
* e5a5935d fix building coreboot images on i686 hosts 
*   a2ac4d13 Merge pull request 'Also try unlocking encrypted volume on NVMe' (#213) from mkukri/lbmk:master into master 
|\  
| * 77ebd050 Also try unlocking encrypted volume on NVMe 
* | 287d0555 Merge pull request 'Add NVMe support to GRUB2 payload' (#212) from mkukri/lbmk:master into master 
|\| 
| * abe6717c Add NVMe support to GRUB2 payload 
* |   47d77c94 Merge pull request 'Fix E6400 display issue with 1440 x 900 panel' (#211) from nic3-14159/lbmk:fix-e6400-igpu-ref-clock into master 
|\ \  
| * | 8629873a Fix E6400 display issue with 1440 x 900 panel 
| |/  
* |   0beecd1b Merge pull request 'Add pt qwerty keymap to lbmk' (#210) from samuraikid/lbmk:master into master 
|\ \  
| * | 8d723d14 Add pt qwerty keymap to lbmk 
* | | 835e5ad0 git.sh: fix invalid command in git_prep() 
| |/  
|/|   
* | 1e54db29 git.sh: allow patching submodules 
* | 00e00a18 git.sh: don't delete .git if src/project/project 
* | 245b4eb2 build/roms: skip target if config/ dir missing 
* | aadccc59 more minor cleanup in the build system 
* | 5b8928c7 git.sh: remove fetch_from_upstream() 
* | 71baf653 option.sh: don't return 1 in mkrom_tarball 
* | 1fe9c4b8 option.sh: mktar_release to mkrom_tarball 
* | cc7ed692 build/roms: rename moverom to copyrom 
* | b40118ae minor code cleanup in the build system 
|/  
* 998f30ad build/roms: simplify serprog list command 
* 21a7efaa build/roms: simplified config payload checks 
* 5b5dccd6 vendor.sh: further simplify config handling 
* 8418ea9a vendor.sh: greatly simplified config handling 
* 53b394f5 vendor.sh: move config checks to detect_firmware 
* bb7255c3 vendor.sh: print an error upon ill-defined target 
* 3f73f3d0 vendor.sh: remove redundant check 
* 32923f56 vendor.sh: simplify defconfig check 
* f8e3ca3b git.sh: Remove .git if XBMK_RELEASE=y 
* dd851caa build: remove initcmd() and simplify main() 
* 4ea843a4 build: initialise git first (before commands) 
* 5702f5a4 build: remove excmd() and simplify main() 
* b76a70c3 build: don't make script_path a global variable 
* 839ef680 lbmk: allow easier sync with cbmk 
* 885fcebd remove help commands (user should read docs) 
* c6ba0a0e option.sh: delete check_git() 
* 313c4c01 build: define "xp" in the global variables 
* 350857ff build: simplify for loop in fetch_trees() 
* 8e05399d build: simplified downloads in fetch_trees() 
* 914ff1ad ./build release: don't do u-boot-only archives 
* 5c3fb9a4 build: use utc+0 when initialising git repo dates 
* e281966f remove check_project() (always set variables) 
* ee2bf0d2 build: simplify deletions in fetch_trees() 
* 39df6230 build: delete mkversion() (just print relname) 
* a40a6129 build/roms: clean up tarball handling 
* e5ffb2af rm src/u-boot/*/test/lib/strlcat.c in u-boot 
* c149cbb8 build: remove mkrom_images 
* 4135ce5e build: use same tarball name on uboot-only release 
* 189b70dd build/roms: create full release tarball name 
* 36d45474 option.sh: don't bother checking for GNU tar 
* f0b604fc option.sh: remove insert_version_files() 
* 267c13cc cleanup: remove mkvdir 
* 08c9f94a unified sha512sum creation for tarballs 
* 1ce7e339 move rom tarball creation to script/roms 
* 190495d2 disable x301 for next release (for now) 
* 03fae0cf mrc.sh: remove redundant function extract_ref() 
* f66ceef6 print two line breaks before confirming release 
* cc339741 remove haswell mrc file (libre raminit stable now) 
* 05fbd392 remove all status checks. only handle release. 
* 8ba0fd83 git.sh: remove errant comment 
* d7ce26dc move script/*/* to script/ 
* 029291e5 merge script/vendor/* into include/vendor.sh 
* c8fb24bb build: print usage for special commands 
* 5f63b594 merge script/update/release into build 
* e1ea5dd0 bump seabios to e5f2e4c69643bc3cd385306a9e5d29e11578148c 
* 052414c0 build: further prevent non-lbmk-work-directory 
* fb8d0c86 build: exit if not running from lbmk directory 
* 38aaaecf build/roms: print serprog help 
* e3cb3a40 merge script/build/serprog with script/build/roms 
* 297af7e6 build/roms: remove unnecessary command 
* 5e4009b5 merge include/err.sh with include/option.sh 
* 58400fc4 err.sh: correct copyright info 
* aa5937ed build/roms: don't rely on x in handle_target 
* 580a5559 build/roms: don't use exit status from skip_board 
* 2fcbff68 build/roms: split up main() 
* d13d9308 build/roms: allow searching status by mismatch 
```

This is 211 changes, since Libreboot 20240504.

Errata
------

### Revision 1

A bug was found, where booting GRUB (payload) on Haswell ThinkPads with a USB
device plugged in would sometimes cause GRUB to hang. This was fixed with the
following patch:
<https://browse.libreboot.org/lbmk.git/commit/?h=20240612_branch&id=70398439baa0127e62d0a7786e21b7de68f1495d>

This patch is included now, on the release archives.

The following archives have been *replaced*, with ones that include this fix:

* Source archive (now marked `20240612rev1`, not `20240612`)
* ROM image archives for: T440p, W541, 9020 SFF and 9020 MT. The new archives
  are marked `20240612rev1`, not `20240612`.

This particular bug was nasty, because some of these machines might have
internal USB devices plugged in, such as smartcard readers. It would not have
caused the system to be unusable, because SeaBIOS is the primary payload and
GRUB (payload) is loaded from that.

If you were affected by this bug, please consider switching to the new images,
where this has been fixed. The fix was very simple:

Don't load the xHCI module in GRUB, where it is used, immediately upon booting
GRUB; instead, load it much later on, from `grub.cfg`. This is done, by putting
it on *install* modules, instead of modules. The *install* modules do not
auto-load when GRUB starts.

A full investigation is still required, but this workaround fixes the issue as
far as the user is concerned.

### Revision 2

NOTE: Source code archive not available for rev2, because another revision
was made that also includes the rev2 change; a 'rev3` source archive is
available.

Revision 2 fixed a minor mistake in the Intel Flash Descriptor on HP 8200
machines, specifically the 4MB target; the Platform Data region was wrong,
which caused issues when flashing. The
file `libreboot-20240612rev2_hp8200sff_4mb.tar.xz` is available,
replacing `libreboot-20240612_hp8200sff_4mb.tar.xz`, on Libreboot rsync.

### Revision 3

Removed `util/autoport/`, becaues it's not required in release archives.

The source code archive is now `libreboot-20240612rev3_src.tar.xz` for this
release, on Libreboot rsync. It contains the original 20240612 changes, in
addition to rev1 and rev2 changes, alongside rev3 (removal of autoport).

The autoport source files lack license headers, which is OK in the context
of `coreboot.git` which contains a fallback COPYING file, but our own
equivalent of that file contains a different license, so the way we were
packaging autoport wasn`t ideal; it's possible that we may have unintentionally
violated the license. This is the first reason for the removal.

The second reason for the removal is that we only intend for this to be used
for development anyway, where it is recommended that you use `lbmk.git`, not
the release archives, and upstream (coreboot) has merged the Haswell support
patches that we were importing, so our copy of autoport is no longer useful.
