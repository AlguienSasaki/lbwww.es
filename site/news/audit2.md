% Libreboot Build System Audit 2
% Leah Rowe
% 11 September 2023

Free as in freedom!
--------------------

This article pertains to the current Libreboot (lbmk) revision at this time
of writing, which is `c400916e33b8870384c30b83a9ecb9cdf7121917` from 11
September 2023.

A lot of work has gone into Libreboot since the [20230625
release](libreboot20230625.md), the main focus of which has been yet another
audit; the [previous audit](audit.md) was mild by comparison.

Great care is always taken when writing for [lbmk](../docs/maintain/), which
is Libreboot's build system, but in the past, auditing was always done ad-hoc.
Since the start of 2023, auditing is now a main aspect of Libreboot development,
and the result has been that the *quality* of Libreboot's build system has
improved greatly. The same care has been given to documentation as well.

Many bugs have been fixed, and the Libreboot build system (lbmk) is generally
much more efficient now, compared to the Libreboot 20230625 release. The main
focus has been this audit, now concluded, and the next focus shall once again
be adding more motherboards to Libreboot, with a view to making a full new release
some time during September 2023.

Before diving into details, here is a brief summary of the recent audit, and
the changes made under it, which accounts for almost *200* individual changes.
This is the *summarised* list, and may not list all changes, but it's pretty
exhaustive:

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
* More reliable configs now used on desktop motherboards: SeaBIOS-only for start,
  but GRUB still available where feasible (in the SeaBIOS menu). This makes it
  more fool proof for a user who might use integrated graphics and then switch
  to a graphics card; the very same images will work.
* TMPDIR environmental variable now set, and exported from main parent process
  when running lbmk; child processes inherit it, and a single tmp dir is used.
  This is then automatically cleaned, upon exit from lbmk; previously, lbmk did
  not cleanly handle `/tmp` at all, but now it's pretty reliable.

Several other features have been added too, despite the overall code size
reduction; for example, Riku Viitanen implemented support for building RP2040
images e.g. Pico Pi, and stm32-vserprog images, for use in small MCU-based SPI
flashers (via serprog protocol).

FULL list of changes (from lbmk git log)
---------------------------------------

Almost all of the changes are post-20230625 release, and almost all of them
are audit-related, so I've simply pasted every commit between the last release
and now. A few of them, e.g. the new motherboard ports, are not audited-related.

Entries that give time frames such as *"14 hours ago"* are relative to
today, 11 September 2023, at some time around 3PM UK time. Changes on top are
newer, and changes further down are older:

```
* c400916e - (HEAD -> master) coreboot/hp8200sff_4mb: fix bad ifd path in config (14 hours ago) <Leah Rowe>
* 087f0e06 - make lbmk help text actually vaguely helpful (19 hours ago) <Leah Rowe>
* 093d40ee - build/release/src: be more thorough deleting .git (21 hours ago) <Leah Rowe>
* 630a6546 - build/release/src: delete elf/ in srcdir (21 hours ago) <Leah Rowe>
* 0543350d - handle/make/file: run make-clean first (21 hours ago) <Leah Rowe>
* 12f9afe6 - build/release/src: remove cbutils/ in srcdir (21 hours ago) <Leah Rowe>
* fe00ab4e - build/release/src: remove errant code (21 hours ago) <Leah Rowe>
* be4ed540 - handle/make/config: distclean once per tree (21 hours ago) <Leah Rowe>
* f227cc08 - handle/make/config: fix distclean/crossgcc-clean (22 hours ago) <Leah Rowe>
* 669c9770 - handle/make/config: fix whitespace and 80-line bug (23 hours ago) <Leah Rowe>
* d28ad6aa - build/release/roms: use -T0 on serprog tarballs (23 hours ago) <Leah Rowe>
* 308c21dd - build/boot/roms stragglers: properly handle errors (23 hours ago) <Leah Rowe>
* c16b28ef - build/release/src: re-create symlinks, don't copy (2 days ago) <Leah Rowe>
* 32dcf9e5 - coreboot/qemu_x86_12mb: re-add this motherboard (2 days ago) <Leah Rowe>
* 5aef8156 - scripts: use printf, not echo, where appropriate (2 days ago) <Leah Rowe>
* 76e12cd4 - update/blobs printf statements: use double quotes (2 days ago) <Leah Rowe>
* 84bf47b5 - scripts: better handling of printf: stdout/stderr (2 days ago) <Leah Rowe>
* b78009e2 - checkgit: properly print output to stderr (2 days ago) <Leah Rowe>
* f45f5e62 - update/project/*: remove redundant checks (2 days ago) <Leah Rowe>
* 3e76e70d - blobs/download: don't use the -B option in make (2 days ago) <Leah Rowe>
* 877c691e - build/release/roms: remove errant line break (2 days ago) <Leah Rowe>
* f03efbc2 - blobs/inject: add error condition on rm command (2 days ago) <Leah Rowe>
* 20be007f - blobs/inject: fix checksum validation if no-ucode (2 days ago) <Leah Rowe>
* f989d5b4 - blobs/sources: fix backup links on some files (2 days ago) <Leah Rowe>
* 878550d5 - use sha512sum to check downloads, not sha1sum (2 days ago) <Leah Rowe>
*   022e0200 - Merge pull request 'Add stm32-vserprog' (#129) from Riku_V/lbmk:stm32 into master (2 days ago) <Leah Rowe>
|\  
| * bed444ff - Add stm32-vserprog (2 days ago) <Riku Viitanen>
* | e9e4ada5 - build/boot/rom: only insert scan.cfg if needed (3 days ago) <Leah Rowe>
* | 0e3f3efc - build/boot/roms: delete tmpcfg when done (3 days ago) <Leah Rowe>
* | a69e8548 - set grub.cfg timeout to 5s (10 on some boards) (3 days ago) <Leah Rowe>
* | 4a459b02 - Merge pull request 'pico-serprog improvements' (#128) from Riku_V/lbmk:master into master (3 days ago) <Leah Rowe>
|\| 
| * 7b6fb958 - Build pico-serprog binary release archive (3 days ago) <Riku Viitanen>
| * c292e01b - Build for all pico board, not just the "original" (3 days ago) <Riku Viitanen>
| * 1bde6bb3 - Support multiple dependencies per project (3 days ago) <Riku Viitanen>
| * 4d3b16da - Cleaner parent directory creation (4 days ago) <Riku Viitanen>
|/  
* 7e8465be - grub: re-add module: play (5 days ago) <Leah Rowe>
* e3b9dfc9 - util/nvmutil: put code all in nvmutil.c (6 days ago) <Leah Rowe>
* 8fc5f6ed - update/blobs/inject: use tmp/romdir, not TMPDIR (7 days ago) <Leah Rowe>
* da991262 - build/release/roms: use tmp/romdir, not TMPDIR (7 days ago) <Leah Rowe>
* 15081ed9 - grub: make backgrounds configurable in target.cfg (7 days ago) <Leah Rowe>
* 0d315c3a - curl/wget downloads: set re-try count to 3 (7 days ago) <Leah Rowe>
* bdf171e3 - don't use /tmp/ for git clones. use tmp/ instead. (7 days ago) <Leah Rowe>
* 196f293a - build/release/roms: fix ucode handling (7 days ago) <Leah Rowe>
* c0c7f3ae - build/release/roms: simplify defcongic handling (7 days ago) <Leah Rowe>
* a56cad71 - update/blobs: unify global variables (7 days ago) <Leah Rowe>
* 2cbc7eea - update/blobs/*: unify checking of defconfig files (7 days ago) <Leah Rowe>
* 52677309 - update/blobs/extract: replace errant target code (7 days ago) <Leah Rowe>
* ea7fae97 - build/boot/roms: don't create empty bin/ directory (7 days ago) <Leah Rowe>
* c62a4239 - update/blobs/inject: remove errant target handling (7 days ago) <Leah Rowe>
* 950166da - update/blobs/download: remove errant code (7 days ago) <Leah Rowe>
* 0668d234 - add checkversion to build/release/src (7 days ago) <Leah Rowe>
* c92a596c - grub: remove xnu module (7 days ago) <Leah Rowe>
* e659ddd8 - grub: remove legacy file system modules (7 days ago) <Leah Rowe>
* cf535785 - re-add grub modules cat, eval and pbkdf2 (7 days ago) <Leah Rowe>
* 33e6088a - move script/misc/versioncheck to main directory (7 days ago) <Leah Rowe>
* 2c769dc1 - move me7_update_parser.py to util/ (7 days ago) <Leah Rowe>
* da3c9bb3 - merge config/ and resources/ (8 days ago) <Leah Rowe>
* a0501050 - blobs/download: don't handle ifd/gbe files (8 days ago) <Leah Rowe>
* 03788d14 - move ifd/gbe configs into config/ifd/ (8 days ago) <Leah Rowe>
* 6ddb0e09 - run make oldconfig on coreboot/default motherboards (8 days ago) <Leah Rowe>
* 19efdf9e - ich9m motherboards: use pre-assembled ifd/gbe files (8 days ago) <Leah Rowe>
* af8d8cda - add ich9m ifd/gbe files (8 days ago) <Leah Rowe>
* d554efae - build/release/src: copy e6430 ifd/gbe (8 days ago) <Leah Rowe>
* 09aae7be - build/rpi-pico-serprog: better error handling (8 days ago) <Leah Rowe>
* 1dc54608 - fix rpi-pico builds when running it twice (8 days ago) <Leah Rowe>
* c63052cf - fix memtest86plus download/build (8 days ago) <Leah Rowe>
*   fb4e6834 - Merge pull request 'Add Dell Latitude E6430' (#124) from nic3-14159/lbmk:e6430 into master (8 days ago) <Leah Rowe>
|\  
| * ebc04e52 - Add Dell Latitude E6430 (8 days ago) <Nicholas Chin>
* |   71d361aa - Merge pull request 'Less cat abuse' (#123) from Riku_V/lbmk:cat into master (8 days ago) <Leah Rowe>
|\ \  
| * | ef3fb05d - Less cat abuse (8 days ago) <Riku Viitanen>
* | | eebf7133 - switch repo links for pico-serprog (8 days ago) <Leah Rowe>
| |/  
|/|   
* | 9ef8a7ea - Merge pull request 'Automate pico-serprog builds' (#122) from Riku_V/lbmk:mkserprog into master (9 days ago) <Leah Rowe>
|\| 
| * e369e8fb - automate rpi-pico serprog builds (9 days ago) <Riku Viitanen>
|/  
* 92b4db69 - build/release/src: only clean kbc1126 if it exists (9 days ago) <Leah Rowe>
* 7c6b35cf - unify build/clean scripts: use handle/make instead (9 days ago) <Leah Rowe>
* cec37747 - build/release/*: use -T0 in xz, for multithreading (9 days ago) <Leah Rowe>
* b4b63adb - don't support ucode removal on untested targets (9 days ago) <Leah Rowe>
* b30c7e33 - coreboot/e6400: support nvidia models (9 days ago) <Leah Rowe>
* 436b2ccb - handle/make/config -m/-u: actually copy configs (9 days ago) <Leah Rowe>
* 3c7b09ac - handle/make/config: properly handle cbutils (9 days ago) <Leah Rowe>
* a3bc7ccd - handle/make/file: fix uefitool builds (9 days ago) <Leah Rowe>
* 4885c796 - handle TMPDIR from include/export.sh (9 days ago) <Leah Rowe>
* 56f16bc8 - don't do cmake on uefitool if the Makefile exists (10 days ago) <Leah Rowe>
* 98d1ea5a - build/release/src: bugfix: actually copy cb/ub/sb (10 days ago) <Leah Rowe>
* 755f925a - build/release/src: copy handle symlink (10 days ago) <Leah Rowe>
* 3ad29d2d - build/release/src: remove Makefile reference (10 days ago) <Leah Rowe>
* d69c231e - build/release/src: fix bad variable reference (10 days ago) <Leah Rowe>
* 38440153 - update build/release/src based on lbmk changes (10 days ago) <Leah Rowe>
* 0e782e7e - update the fetch scripts themselves (10 days ago) <Leah Rowe>
* 98f30b6d - build/coreboot/utils: exit 1 if target.cfg missing (10 days ago) <Leah Rowe>
* b9662fbe - handle project downloads in main lbmk script (10 days ago) <Leah Rowe>
* 12b33eb8 - lbmk script: always clean up /tmp files (11 days ago) <Leah Rowe>
* 225e2609 - only remove microcode in build/release/roms (11 days ago) <Leah Rowe>
* bf774acf - move build/boot/rom moverom to handle/make/config (11 days ago) <Leah Rowe>
* e5546128 - build/release/roms: fix syntax error (11 days ago) <Leah Rowe>
* fbda0f04 - re-add /dev/null redirect on . ${1} (11 days ago) <Leah Rowe>
* b2bad5a0 - build/release/src: copy the include/ directory (13 days ago) <Leah Rowe>
* eb54e427 - grub: all one grub.elf containing keymaps and cfg (2 weeks ago) <Leah Rowe>
* c6fd4d2a - lbmk: run ./build dependencies *before* root check (2 weeks ago) <Leah Rowe>
* 6722624d - build/boot/roms: fix bad variable assignment (2 weeks ago) <Leah Rowe>
* 55be6dda - dependencies/ubuntu2004: update based on debian (2 weeks ago) <Leah Rowe>
* 0052f9d0 - fix: don't require git config for dependencies (2 weeks ago) <Leah Rowe>
* 6dbddf85 - build/boot/roms: simplify ich9m ifd handling (2 weeks ago) <Leah Rowe>
* f5787c9e - build/boot/roms ich9m ifd: use fast dd command (2 weeks ago) <Leah Rowe>
* d9292cec - build/boot/roms: use the new coding style (2 weeks ago) <Leah Rowe>
* 4623f3f2 - Remove superfluous GRUB modules (save CBFS space) (2 weeks ago) <Leah Rowe>
* 623c3389 - fix typo in error message ("as not permitted") (2 weeks ago) <Leah Rowe>
* 4a280c62 - .gitcheck: re-write entirely. force global config. (2 weeks ago) <Leah Rowe>
* 355eb765 - move resources/scripts/ to script/ (2 weeks ago) <Leah Rowe>
* eed34d3e - enable memtest86plus on various boards (2 weeks ago) <Leah Rowe>
* bc0fb51d - x86 desktops: only enable seabios_withgrub (2 weeks ago) <Leah Rowe>
* 9457d6be - unified list command for all scripts (2 weeks ago) <Leah Rowe>
* 93d2dcad - handle/make/config: add missing pipes for err (2 weeks ago) <Leah Rowe>
* 0e6851c8 - delete the Makefile (2 weeks ago) <Leah Rowe>
* ebbefa60 - handle/config/file: rename to handle/make/config (2 weeks ago) <Leah Rowe>
* df6db1c6 - handle/config: fix errant "handle src for" call (2 weeks ago) <Leah Rowe>
* 6874bc39 - "handle src for" - change to handle make file (2 weeks ago) <Leah Rowe>
* 798ce03a - handle/config: add missing error handle (2 weeks ago) <Leah Rowe>
* 29a8193e - build/src/for: rename to handle/make/file (2 weeks ago) <Leah Rowe>
* 27c67295 - handle/config/file: unified distclean handling (2 weeks ago) <Leah Rowe>
* 197464bc - build/src/for: use -j for multithreaded builds (2 weeks ago) <Leah Rowe>
* 95f290d9 - build/release/src: update based on recent changes (2 weeks ago) <Leah Rowe>
* 5a47c01b - scripts: put quotes around file/directory names (2 weeks ago) <Leah Rowe>
* 1c8401be - much, much stricter, more verbose error handling (2 weeks ago) <Leah Rowe>
* 50c395df - .gitcheck: continue if no .git (don't break) (3 weeks ago) <Leah Rowe>
* be7a5b0c - .gitcheck: must stricter error handling (3 weeks ago) <Leah Rowe>
* 3a5ba57f - .gitcheck: only redirect stdout to /dev/null (3 weeks ago) <Leah Rowe>
* 8f4f0e00 - use the new coding style in scripts (3 weeks ago) <Leah Rowe>
* 4c6c7d10 - scripts: never exit 1, always call err instead (3 weeks ago) <Leah Rowe>
* 52f3fd35 - blobs/download: copy dl_path, don't move it (3 weeks ago) <Leah Rowe>
* 57adbc6e - unify err functions across scripts (3 weeks ago) <Leah Rowe>
* b3fbcdf6 - .gitignore: ignore *all* seen files (3 weeks ago) <Leah Rowe>
*   24f09335 - Merge pull request 'hp8300usdt: enable mSATA' (#118) from Riku_V/lbmk:master into master (3 weeks ago) <Leah Rowe>
|\  
| * df1e8913 - hp8300usdt: enable mSATA (3 weeks ago) <Riku Viitanen>
* | dfb93166 - Merge pull request 'memtest86+ v6.20' (#116) from Riku_V/lbmk:memtest into master (3 weeks ago) <Leah Rowe>
|\| 
| * fa926632 - memtest86+ v6.20 (3 weeks ago) <Riku Viitanen>
|/  
*   1bd84209 - Merge pull request 'osbmk->lbmk' (#117) from Riku_V/lbmk:osbmk-lbmk into master (3 weeks ago) <Leah Rowe>
|\  
| * 7be203dd - osbmk->lbmk (3 weeks ago) <Riku Viitanen>
* | 04ee2672 - also clean up the main scripts (3 weeks ago) <Leah Rowe>
* | 62f23123 - general code cleanup on lbmk shell scripts (3 weeks ago) <Leah Rowe>
|/  
* 7be47065 - unify build/defconfig and modify/defconfig (3 weeks ago) <Leah Rowe>
* 0faf2a0c - main lbmk script: exit non-zero if argc is wrong (3 weeks ago) <Leah Rowe>
* 6e92d9a3 - fix "./build help" (3 weeks ago) <Leah Rowe>
* 9031bb7b - unify dependencies scripts (3 weeks ago) <Leah Rowe>
* 023d6b69 - unify build/clean into ./build release src (3 weeks ago) <Leah Rowe>
* f893a29b - unify most module build scripts (3 weeks ago) <Leah Rowe>
* c83d1a8d - unify grub scripts under one directory (3 weeks ago) <Leah Rowe>
* 438bf2c9 - grub/modules.list: add argon2 (3 weeks ago) <Leah Rowe>
* fd602532 - grub: import phc argon2 implementation (for luks2) (3 weeks ago) <Leah Rowe>
* 2c0c521e - bump grub revision a bit (3 weeks ago) <Leah Rowe>
* e076d893 - unify update/modify coreboot/u-boot/seabios script (3 weeks ago) <Leah Rowe>
* e25984d7 - remove board: qemu_x86_12mb (not usable for now) (3 weeks ago) <Leah Rowe>
* e5b898f6 - consolidate u-boot/seabios/coreboot build scripts (3 weeks ago) <Leah Rowe>
* 673b144a - coreboot/fam15h: fix for gcc/gnat building (4 weeks ago) <Adrien 'neox' Bourmault>
* 63b0e99f - don't call blobutil directly from lbmk (4 weeks ago) <Leah Rowe>
* 08486227 - remove download scripts, consolidate into script (4 weeks ago) <Leah Rowe>
* 8459e33b - improve user feedback in blobutil (4 weeks ago) <Leah Rowe>
* 59dba6cf - merge coreboot/u-boot download logic to one script (4 weeks ago) <Leah Rowe>
* 2453c303 - gitclone: always clean up /tmp (4 weeks ago) <Leah Rowe>
* adeb065c - fix permissions on arch dependencies script (4 weeks ago) <Leah Rowe>
* 6075fed8 - NEW BOARD: HP EliteBook 8470p (Intel GPU) (4 weeks ago) <Leah Rowe>
* f9afeb6f - NEW BOARD: Dell Precision T1650 (4 weeks ago) <Leah Rowe>
* f8f77cb2 - NEW BOARD: HP EliteBook 2170p (5 weeks ago) <Leah Rowe>
*   c5c89467 - Merge pull request 'Update 'README.md'' (#89) from ewpr5kwu/lbmk:master into master (5 weeks ago) <Leah Rowe>
|\  
| * 5204f0a9 - Update 'README.md' (2 months ago) <ewpr5kwu>
* cb8bf380 - bump seabios revision to 30 May 2023 (5 weeks ago) <Leah Rowe>
* 27ee975e - bump grub revision to 2.12-rc1 (5 weeks ago) <Leah Rowe>
* 705149a3 - coreboot/default: bump revision to 2 August 2023 (5 weeks ago) <Leah Rowe>
* 22ee7f74 - blobs/download: save ME file to correct location (5 weeks ago) <Leah Rowe>
* cdd83ab1 - blobs/download: try backup if bad hash on main (5 weeks ago) <Leah Rowe>
* f18b1859 - blobs/download: support more formats on ME extract (5 weeks ago) <Leah Rowe>
* f0efaf79 - add unar to dependencies scripts (5 weeks ago) <Leah Rowe>
* e8ba0f87 - blobs/download: declare full user agent (6 weeks ago) <Leah Rowe>
* 4875eef1 - blobs/download: properly handle backup/main url (6 weeks ago) <Leah Rowe>
* cca93ca3 - blobs/download: don't download backup on main (6 weeks ago) <Leah Rowe>
* 3aeefaa7 - blobs/download: set common user agent string (6 weeks ago) <Leah Rowe>
* 5e83d2bc - blobs/download: simplify for loop (6 weeks ago) <Leah Rowe>
* 8f1d3ad1 - scripts: fix indentation in switch/case blocks (6 weeks ago) <Leah Rowe>
* 748e0972 - blobutil/ec: abort if kbc1126 ec extraction fails (7 weeks ago) <Leah Rowe>
* e594ac16 - coreboot/fam15h: remove unused files (7 weeks ago) <Leah Rowe>
* 44bd077a - Revert "build/boot/roms mkUBootRoms: initialise variables" (8 weeks ago) <Leah Rowe>
* 7c90a407 - build/boot/roms mkUBootRoms: initialise variables (8 weeks ago) <Leah Rowe>
* d918139f - coreboot/fam15h: re-enable microcode updates (8 weeks ago) <Leah Rowe>
* 8c777428 - board/qemu_x86: don't enable u-boot (8 weeks ago) <Leah Rowe>
* fb44c349 - coreboot/haswell: fix acpica downloads (8 weeks ago) <Leah Rowe>
* af084014 - coreboot: re-add asus kgpe-d16/kcma-d8/kfsn4-dre (8 weeks ago) <Leah Rowe>
* e6002b91 - coreboot/cros: fix acpica downloads (8 weeks ago) <Leah Rowe>
* f34e07ae - build/boot/roms: fix coreboot-version in releases (9 weeks ago) <Leah Rowe>
```

A new Libreboot release is planned soon.
