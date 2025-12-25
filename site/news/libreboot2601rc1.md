% Libreboot 26.01rc1 "Luminous Lemon" released!
% Leah Rowe
% 30 June 2025

The codename for this release series is *Tenacious Tomato*. Today's release is
a beta/RC.

This is the first RC release in the Libreboot 26.01 release series. It is
expected that a new RC will be issued weekly between now and the end of January
2026; the final in January 2026 will become the full Libreboot 26.01 release.

Binaries are available to download from Libreboot RSYNC and HTTP mirrors.

Testing very much welcome. Please test this release on your board and
[contact Libreboot](../contact.md) if you encounter problems. It is expected
that all boards will be stable, but as this is an RC release, you should
expect bugs. Nearly all of the code revisions have been updated, for instance
GRUB and coreboot.

Libreboot 26.01 RC1 release change log is simply a Git log, relative
to the Libreboot 25.06 release; a proper release page will be written for
the full 26.01 release, which will be ready in the final days of January 2026.

But here are some brief mentions of changes, before going into the full git logs:

Several new mainboards have been added, such as:

* Topton X2E N150
* Lenovo ThinkPad T580
* HP Pro 3500 Series

Numerous build system improvements were made, making the build process more
deterministic. Several safety improvements, e.g. reduced use of eval, better
hashing of project files (especially checking vendor file extraction).

Several bug fixes, e.g. ThunderBolt supported added to the ThinkPad T480,
headphone jack auto-detection on T480 also. (ditto T480s)

A lot more new work will be done between now and the end of January 2026. It is
very much the case that Libreboot 26.01 is an ongoing project. Therefore, this
first RC release can be considered a snapshot of that progress. It was always
the case that a December 25th release was planned, but this was originally
going to be a full *Libreboot 25.12* release - now it will be Libreboot 26.01.

Work is ongoing to integrate Google Chromebooks, by integrating the
MrChromebox distribution of coreboot into Libreboot's build system.

Improved safety when handling vendor file downloads. For example, the sbase
implementation of sha512sum is now provided by lbmk and compiled by it, rather
than relying on the host's sha512sum implementation. This ensures more
consistent functionality, for such a critical tool (lbmk uses sha512sum to
hash everything). This is also used when checking project files, to see
when a project changed and has to be recompiled.

Minor features added, such as CBFS-based backend courtesy Nicholas Chin, for
boards where NVRAM is not used; ThinkPad T480 for example.

First, the shortlog:

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

Next, the full log:

```
commit a59fc6a3535b437a1dba0841d36a02a930e1cb0b
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Dec 25 00:44:51 2025 +0100

    Libreboot 26.01 RC1 Tenacious Tomato
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b259f3493d31fd0dfa01611ddd29161e6b8c469e
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 19:14:43 2025 +0100

    update pcsx-redux openbios
    
    use revision:
    b745534eb231d3699ec57949f16a9a7bd5b79385
    
    This brings the following upstream changes:
    
    *   b745534e Merge pull request #1957 from wheremyfoodat/splash
    |\
    | *   33adecd6 Merge branch 'main' into splash
    | |\
    | |/
    |/|
    * |   d01e438e Merge pull request #1979 from nicolasnoble/upgrade-fmt
    |\ \
    | * | ae8c5cb8 Updating fmt to 12.1.0
    |/ /
    * |   fd9959a8 Merge pull request #1978 from malucard/all
    |\ \
    | * | 3ecd98e6 change deprecated fmt::localtime to std::localtime
    |/ /
    | * eec812e3 Nix: Stop using the outdated version of stb from nixpkgs (#7)
    | *   836a0e10 Merge branch 'main' into splash
    | |\
    | |/
    |/|
    * |   e90d74d6 Merge pull request #1968 from wheremyfoodat/fix-coroutine
    |\ \
    | * | 62a23f8f Fix coroutine build on AppleClang 17
    * | |   a56a96be Merge pull request #1974 from njfox/aur-ci-dependencies
    |\ \ \
    | * | | 98d10525 add --syncdeps to install missing PKGBUILD dependencies
    | |/ /
    * | |   3c155400 Merge pull request #1977 from aybe/fix-icon-overlay
    |\ \ \
    | * | | 79de997d Fix spurious folder icon overlay
    * | | |   1d573efa Merge pull request #1976 from aybe/fix-missing-directory-extension
    |\ \ \ \
    | * | | | b13b80c7 Render directory node fully (including extension)
    | |/ / /
    * | | |   1a678f42 Merge pull request #1975 from aybe/fix-wrong-c-drive
    |\ \ \ \
    | |/ / /
    |/| | |
    | * | | c37d0597 Fix C:\ showing current directory
    |/ / /
    * / / 47d83a2b Padding isos to 2 minutes instead of 2 seconds.
    |/ /
    * |   718f0912 Merge pull request #1962 from njfox/aur-ci
    |\ \
    | * \   c70d85e4 Merge branch 'main' into aur-ci
    | |\ \
    | |/ /
    |/| |
    * | |   87f8e861 Merge pull request #1964 from nicolasnoble/tpageloc-fix
    |\ \ \
    | * \ \   acdc7e50 Merge branch 'main' into tpageloc-fix
    | |\ \ \
    | |/ / /
    |/| | |
    * | | |   a11e5ed6 Merge pull request #1965 from nicolasnoble/brew-hotfix
    |\ \ \ \
    | * | | | d6e15260 Now fixing psyqo...
    | * | | | a7035f32 Fixing OpenBIOS build under 15.2...
    | * | | | 909eb220 Also patching gcc now.
    | * | | | c319254e Adding zlib patch for binutils on macos...
    | * | | | d56b30e3 Derp.
    | * | | | 00bd1db0 Upgrading gcc and gdb as well...
    | * | | | 27e6fab9 Adding verbose mode.
    | * | | | 5cb9084d Restoring readme properly.
    | * | | | d911c2b1 Updating to binutils 2.45.
    | * | | | a759cb84 Fixing https://github.com/orgs/Homebrew/discussions/6351
    |/ / / /
    | * / / 6de0f096 Properly returning a TPageLoc on TPageAttr::getPageLoc
    |/ / /
    | * |   98c53855 Merge branch 'main' into aur-ci
    | |\ \
    | |/ /
    |/| |
    * | |   a285e14e Merge pull request #1955 from wheremyfoodat/mac-bundle
    |\ \ \
    | * | | 97b9651b MacOS bundle script: Enable game mode support, list app as game
    | * | | 0e0fabf2 MacOS bundle script: Clean up temporary image files
    * | | |   1d54cbe6 Merge pull request #1958 from wheremyfoodat/aa64-flush-cache
    |\ \ \ \
    | * | | | 053eb573 arm64 JIT: Fix broken cache invalidation
    | |/ / /
    * | | |   52d9ddc5 Merge pull request #1956 from wheremyfoodat/jit-fixes
    |\ \ \ \
    | * | | | 4dd04425 a64 JIT: Fix setRWX function returning nothing on Apple
    | |/ / /
    * | | |   4e846d89 Merge pull request #1959 from wheremyfoodat/no-portable
    |\ \ \ \
    | * | | | 169788fb Add --no-portable flag
    | |/ / /
    * | | |   7bce4ecd Merge pull request #1960 from wheremyfoodat/remove-dynarec-dump
    |\ \ \ \
    | * | | | 8c995593 arm64 JIT: Don't unconditionally dump a JIT dump in the user's files
    | |/ / /
    * | | |   3a8ddc79 Merge pull request #1963 from njfox/openbios-fix-function-signatures
    |\ \ \ \
    | |/ / /
    |/| | |
    | * | | 98d9684b move pointer asterisk to be consistent
    | * | | e7ad2b1e fix function definitions to include correct argument types
    |/ / /
    | * | bdaa1fe9 remove duplicate git dependency
    | * | 53326672 combine dependency updates/installation and fix indentation
    | * | f4d1f073 build against PR and push instead of master
    |/ /
    | * cdec82a4 Fix VS project files
    | * 587ce8e9 GUI: Better splash image, bug fixes
    | * 0b73a07a Add splash screen generation script
    | * 3c4afbb6 Initial splash image draft
    |/
    *   4f4a00fe Merge pull request #1953 from cleverca22/fix-submodules
    |\
    | * 2bc000e8 fix submodules
    * |   a4d6bcc4 Merge pull request #1954 from yaz0r/gdb_sharedmem
    |\ \
    | |/
    |/|
    | * 390ccf63 Address comments
    | * 07e9b472 Add monitor command to retrieve the shared memory name. Only for wram so far.
    |/
    *   1b0cbe5e Merge pull request #1952 from yaz0r/gdbfix
    |\
    | * b4e77deb Fix GDB packet p (readRegister)
    |/
    *   a2a6d77c Merge pull request #1951 from nicolasnoble/linuxdeploy
    |\
    | * 2388bcb1 Switching to linuxdeploy.
    |/
    *   ec1154ad Merge pull request #1950 from grumpycoders/revert-1947-appimage-bullshit
    |\
    | * 5a66d37f Revert "Sorting out yet another AppImage breakage."
    |/
    *   a1f02931 Merge pull request #1940 from Forceh91/patch-1
    |\
    | * e23f9d01 Re-adding all examples.
    | * f1b8694b Fixing test suite.
    | *   78b4e9f1 Merge branch 'main' into patch-1
    | |\
    | |/
    |/|
    * |   b62b506f Merge pull request #1944 from eliasdaler/authoring_quiet
    |\ \
    | * \   572e18ba Merge branch 'main' into authoring_quiet
    | |\ \
    | |/ /
    |/| |
    * | |   cb651973 Merge pull request #1946 from nicolasnoble/pcsx-io-fixes
    |\ \ \
    | * \ \   fca66d17 Merge branch 'main' into pcsx-io-fixes
    | |\ \ \
    | |/ / /
    |/| | |
    * | | | b2066c72 Merge pull request #1947 from nicolasnoble/appimage-bullshit
    * | | | e5244aea Sorting out yet another AppImage breakage.
     / / /
    * / / e1be0936 Fixing accesses to the pcsx IO system.
     / /
    * / 0fbc4206 autoring: add -q option
     /
    * eaff5ed2 Merge branch 'main' into patch-1
    * c0fa3b2f chore: nitpick changes from coderabbit
    * 3b3e7986 chore: create readme with community examples
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a4ad3afdbbaa34e89c45451b5526054d2ee6fa49
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 19:02:08 2025 +0100

    bump flashprog rev: ffcf92fb, December 2025
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 68e0b5dddceaf559952e4ad6ed4106aaf9c0398b
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 15:51:34 2025 +0100

    init.sh: Explicitly export UTF-8 locale
    
    C.UTF-8, instead of just C.
    
    This fixes a build issue in GRUB on my Arch Linux test bench.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b990d54cffdf0a6c43bfda72a7ef49ef1c3d31b5
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 02:35:30 2025 +0100

    grub modules: add more gcry_ ciphers
    
    Since the libgcrypt update in GRUB, which imported
    GNU's own Argon2 implementation, other ciphers have
    also been introduced.
    
    This patch adds them to lbmk's GRUB build.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5df6b924d729d7a0042fec19cf3be1c1817314ca
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 01:58:24 2025 +0100

    bump seabios revision
    
    by the time i'd done this, i'd realised that seabios only
    modified some documentation upstream. the code has not
    changed since last update, upstream.
    
    no point scrapping the update now. now we have slightly
    better documentation for seabios!
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9228a0c02ce40519db8ec22b09808f57aa1552b8
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 24 01:47:28 2025 +0100

    update grub again, to 25b7f6b93
    
    i had a build error before, when trying this absolute
    most up to dave revision.
    
    i found that it was this patch:
    
    commit 1a5417f39a0ccefcdd5440f2a67f84d2d2e26960
    Author: Nicholas Vinson <nvinson234@gmail.com>
    Date:   Tue Nov 18 19:38:07 2025 -0500
    
    For now, I just revert it.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 512cce07708630e546e071d7050b98da64438a77
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Dec 23 19:19:07 2025 +0100

    GRUB: update to rev 2.14-rc1 December 2025
    
    The argon2 patches were dropped (PHC implementation),
    because GRUB has now its own Argon2 KDF implementation
    as part of its recent libgcrypt update.
    
    Argon2 support is therefore retained, but GNU's implementation
    has replaced the one that Libreboot previously used.
    
    This brings in the following upstream changes:
    
    * 280715ec6 Release 2.14~rc1
    * e549317e1 windows: Fix symbol table generation during module conversion from PE to ELF
    * a340750f9 windows: Fix relocation sections generation during module conversion from PE to ELF
    * c602035a9 loader/efi/linux: Fix compile error with Clang
    * f62269767 build: Add tpm2key.asn file for reference to dist archive
    * 46be4488f build: Include new zstd test support files in dist archive
    * fa79d5ea9 build: Include MAINTAINERS and SECURITY files in dist archive
    * 8271bcc13 build: Add appended signatures header file to EXTRA_DIST
    * 11845da2b lib/xzembed/xz_dec_stream: Replace grub_memcpy() call with memcpy()
    * 7ded35fea bootstrap: Fix patching warnings
    * 4129e9ad6 tss2: Always init out buffer before calling tpm2_submit_command_real()
    * 000e48b42 fs/ntfs: Correct next_attribute validation
    * 5ff9c43cf kern/ieee1275/init: Use net config for boot location instead of firmware bootpath
    * c2cae77ab net/tftp: Fix NULL pointer dereference in grub_net_udp_close()
    * fadc94b91 net/dns: Prevent UAF and double free
    * cd24e2591 net/bootp: Prevent a UAF in network interface unregister
    * fca6c0afd docs: Document lsmemregions and memtools commands
    * 42c099786 commands/memtools: Add lsmemregions command
    * 20211246a tests/file_filter: Add zstd tests
    * 092449f63 tests/file_filter: Add zstd test file
    * 356dcac9a tests/file_filter: Regenerate gpg keys
    * 8ea83c3ee io/zstdio: Implement zstdio decompression
    * 7c22c1000 fs/btrfs: Update doc link for bootloader support
    * 6435551a6 docs: Add Btrfs env block and special env vars
    * b63447748 util/grub.d/00_header.in: Wire grub.cfg to use env_block when present
    * e4e177661 fs/btrfs: Add environment block to reserved header area
    * d6525f0e6 util/grub-editenv: Add probe call for external envblk
    * 4b5ea8dca util/grub-editenv: Wire list_variables() to optional fs_envblk
    * 84e2bc2f1 util/grub-editenv: Wire unset_variables() to optional fs_envblk
    * c7c901916 util/grub-editenv: Wire set_variables() to optional fs_envblk
    * 2abdd8cd2 util/grub-editenv: Add fs_envblk write helper
    * e4d684cc4 util/grub-editenv: Add fs_envblk open helper
    * 512e33ec7 tests: Add "z" length modifier printf tests
    * 3c9762b12 kern/misc: Add the "z" length modifier support
    * 91ddada64 disk/cryptodisk: Add --hw-accel to enable hardware acceleration
    * f8f68f14a libgcrypt: Add hardware acceleration for gcry_sha512
    * 70b2f5f08 libgcrypt: Add hardware acceleration for gcry_sha256
    * 2158d8e8a libgcrypt: Declare the sha256 shaext function
    * 0ff5faf8c libgcrypt: Implement _gcry_get_hw_features()
    * 812356191 libgcrypt: Copy sha512 x86_64 assembly files
    * 7f9c590af libgcrypt: Copy sha256 x86_64 assembly files
    * 8423176f1 lib/hwfeatures-gcry: Enable SSE and AVX for x86_64 EFI
    * 06a5b88ba lib/hwfeatures-gcry: Introduce functions to manage hardware features
    * d01abd713 configure: Tweak autoconf/automake files to detect x86_64 features
    * a122e0262 lib/pbkdf2: Optimize PBKDF2 by reusing HMAC handle
    * 961e38b2b lib/crypto: Introduce new HMAC functions to reuse buffers
    * 59304a7b5 docs: Document argon2 and argon2_test modules
    * 28dbe8a3b kern/misc: Implement faster grub_memcpy() for aligned buffers
    * da01eb0c5 tests/util/grub-fs-tester: Use Argon2id for LUKS2 test
    * c1bd9fc82 tests: Integrate Argon2 tests into functional_test
    * 6a525ee64 tests: Import Argon2 tests from libgcrypt
    * 6052fc2cf disk/luks2: Add Argon2 support
    * 66b8718f9 argon2: Introduce grub_crypto_argon2()
    * de201105d libgcrypt/kdf: Fix 64-bit modulus on 32-bit platforms
    * 93544861b libgcrypt/kdf: Remove unsupported KDFs
    * 1ff720641 libgcrypt/kdf: Get rid of gpg_err_code_from_errno()
    * 0c06a454f libgcrypt/kdf: Implement hash_buffers() for BLAKE2b-512
    * bc94dfd54 crypto: Update crypto.h for libgcrypt KDF functions
    * 5b81f490c util/import_gcry: Import kdf.c for Argon2
    * 6b5c671d3 commands/menuentry: Fix for out of bound access
    * 21cdcb125 tests/tpm2_key_protector_test: Add a test for PCR Capping
    * afddba012 tpm2_key_protector: Support PCR capping
    * ae7a39900 tss2: Implement grub_tcg2_cap_pcr() for emu
    * 7b39970e9 tss2: Implement grub_tcg2_cap_pcr() for ieee1275
    * 39f98e471 tss2: Implement grub_tcg2_cap_pcr() for EFI
    * d47d261ec tss2: Introduce grub_tcg2_cap_pcr()
    * b2549b4d3 tss2: Add TPM2_PCR_Event command
    * e1b9d92a8 loader/i386/linux: Transfer EDID information to kernel
    * a8379e693 fs/hfsplus: Allow reading files created by Mac OS 9
    * c5ff0d616 docs: Fix build warnings in libgcrypt and blsuki doc
    * fa93f2412 kern/command,commands/extcmd: Perform explicit NULL check in both the unregister helpers
    * 9a725391f commands/efi/tpm: Call get_active_pcr_banks() only with TCG2 1.1 or newer
    * 894241c85 kern: Include function name on debug and error print functions
    * 75a20cc14 kern: Make grub_error() more verbose
    * 8abbafa49 net/tcp: Fix TCP port number reused on reboot
    * 3dff10a97 docs/grub: Document appended signature
    * 0f2dda8cf docs/grub: Document signing GRUB with an appended signature
    * 0b59d379f docs/grub: Document signing GRUB under UEFI
    * dbfa3d7d7 appended signatures: Verification tests
    * 7f68c7195 appended signatures: GRUB commands to manage the hashes
    * 6cb58b1c9 appended signatures: GRUB commands to manage the certificates
    * ab7b17717 appended signatures: Using db and dbx lists for signature verification
    * 97f7001e1 appended signatures: Create db and dbx lists
    * b5e872417 appended signatures: Introducing key management environment variable
    * 76158ed1a powerpc/ieee1275: Read the db and dbx secure boot variables
    * 069f3614e appended signatures: Support verifying appended signatures
    * f8e8779d8 powerpc/ieee1275: Enter lockdown based on /ibm, secure-boot
    * e95c52f1f appended signatures: Parse X.509 certificates
    * a33754979 appended signatures: Parse PKCS#7 signed data
    * 3e4ff6ffb appended signatures: Parse ASN1 node
    * 7d28bdb0b appended signatures: Import GNUTLS's ASN.1 description files
    * 1fca5f397 grub-install: Support embedding x509 certificates
    * aefe0de22 pgp: Rename OBJ_TYPE_PUBKEY to OBJ_TYPE_GPG_PUBKEY
    * f826cc8b0 crypto: Move storage for grub_crypto_pk_* to crypto.c
    * 31cc7dfe5 powerpc/ieee1275: Add support for signing GRUB with an appended signature
    * ee789e1a6 lib/b64dec: Use grub_size_t instead of size_t for _gpgrt_b64dec_proc() function definition
    * abb8fb6d1 util/grub-mkimagexx: Fix riscv32 relocation offset
    * 1f9092bfd libgcrypt: Allow GRUB to build with Clang
    * 1d2ee8f8b tests: Add test ISO files to dist package
    * dfa3dbf61 tests: Test dates outside of 32-bit Unix range
    * 6837293b8 lib/datetime: Support dates outside of 1901..2038 range
    * 02788bfdf bootstrap: Ensure shallow gnulib clone works on newer git
    * 4e42199f3 docs: Correct some URLs
    * 733cc28eb docs: Update Future section to reflect current release
    * 54c8573ef docs: Document new libgrypt modules
    * 1562dee69 docs: Clarify section heading and fix wording
    * cf1b75a14 BUGS: Update to point to bug tracking system
    * 236663dfb INSTALL: Document libtasn1 needed for grub-protect
    * 7bfb38627 po: Update translations to build with gettext 0.26
    * 49e76ad16 term/efi/console: Treat key.scan_code 0x0102 (suspend) as Enter
    * de72f3998 util/bash-completion.d/Makefile.am: s/mkrescure/mkrescue/g
    * 14c2966c7 blsuki: Add uki command to load Unified Kernel Image entries
    * 5190df851 blsuki: Check for mounted /boot in emu
    * 51b960132 util/misc.c: Change offset type for grub_util_write_image_at()
    * 8cee1c284 blsuki: Add blscfg command to parse Boot Loader Specification snippets
    * e016d6d60 kern/misc: Implement grub_strtok()
    * 587db89af kern/xen: Add Xen command line parsing
    * b2a975bc5 include/xen/xen.h: Add warning comment for cmd_line
    * 19c698d12 zfs: Fix LINUX_ROOT_DEVICE when grub-probe fails
    * 6898fcf74 relocator: Switch to own page table while moving chunks
    * 67a95527b configure: Generate tar-ustar tarball instead of tar-v7
    * 29d515b4c build: Add new libgcrypt and libtasn1 related files to EXTRA_DISTS
    * eb76b064d build: Add util/import_gcrypt_inth.sed to EXTRA_DISTS
    * eb56a6af9 include/xen/xen.h: Rename MAX_GUEST_CMDLINE to GRUB_XEN_MAX_GUEST_CMDLINE
    * 80df5e132 loader/arm64/xen_boot: Set correctly bootargs property for modules
    * 6831d242a loader/efi/linux: Return correct size from LoadFile2
    * f326c5c47 commands/bli: Set LoaderTpm2ActivePcrBanks runtime variable
    * 0e367796c docs: Write how to import new libgcrypt
    * b930bfa37 libgcrypt: Fix a memory leak
    * d48c277c4 libgcrypt: Don't use 64-bit division on platforms where it's slow
    * de49514c9 util/import_gcry: Fix pylint warnings
    * 334353a97 util/import_gcry: Make compatible with Python 3.4
    * 2a6de4209 libgcrypt: Import blake family of hashes
    * e54187912 libgcrypt: Ignore sign-compare warnings
    * e3b78e49c libgcrypt: Remove now unneeded compilation flag
    * e23704ad4 libgcrypt: Fix Coverity warnings
    * d65810b01 keccak: Disable acceleration with SSE asm
    * f808ef0d2 tests: Add DSA and RSA SEXP tests
    * 0739d24cd libgcrypt: Adjust import script, definitions and API users for libgcrypt 1.11
    * 3e1c2890b b64dec: Add harness for compilation in GRUB environment
    * 5ca0d5e41 b64dec: Import b64dec from gpg-error
    * 3312af6e0 libgcrypt: Import libgcrypt 1.11
    * a0d4c94ef loader/efi/linux: Use shim loader image handle where available
    * 1b9a84e63 loader/efi/chainloader: Use shim loader image handle where available
    * e31d0cd7f efi/sb: Add API for retrieving shim loader image handles
    * ed7e053a3 efi/sb: Add support for the shim loader protocol
    * 70897d3d3 efi: Provide wrappers for load_image, start_image and unload_image
    * 030a70fca loader/arm64/xen_boot: Consider alignment calling grub_arch_efi_linux_boot_image()
    * e82609a47 loader/arm64/xen_boot: Use bool instead of int
    * d1a470b69 loader/arm64/xen_boot: Remove correctly all modules loaded by xen_module command
    * cf5e52fa8 dl: Fix grub_dl_is_persistent() for emu

commit 8b338404e81ae9c29f2d0822b401c92972368aa9
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Dec 23 18:49:16 2025 +0100

    hp8300cmt and dell 780: use legacy verbs
    
    see patches. coreboot making changes upstream. these
    are used in the meantime.
    
    this prevents build errors. (again, see patches, specifically
    the 780 one explains rationale)
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0ff8110a5566211573efa461ee1077e5637c5198
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Dec 23 12:15:06 2025 +0000

    fix 3050micro vbt path again
    
    i got the variable wrong, putting the dollar sign inside
    the brackets, rather than outside.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ce302356fc5cd0336d6f32e362655af003cff76b
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Dec 23 11:15:43 2025 +0000

    coreboot/dell3050: fix VBT-related build error
    
    During routine build testing, I noticed that the VBT path was
    wrong, because this port had been converted into a variant.
    
    This is because of the OptiPlex 5040 port that is under
    review on the coreboot Gerrit website.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2fe3b6ee6609a3115836eb881cbd4104af2e868f
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Dec 22 11:58:38 2025 +0000

    actually enable TBT on T580
    
    lbmk uses make-oldconfig before running a build anyway,
    so it would have been fine. however, it's best to just
    enable it outright.
    
    this change was generated by doing:
    
    ./mk -u coreboot t580_vfsp_16mb
    
    which runs make-oldconfig on the configs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 6e253b3e73e41227b5950f96b29bb6f066239656
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Dec 22 11:10:05 2025 +0000

    thunderbolt support for thinkpad t580
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9142c8b41921e659d69878702089eb37c87f02b5
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Dec 21 13:15:58 2025 +0100

    ThinkPad T580 support.
    
    Yes.
    
    Thank you Johann C. Rode for this excellent coreboot port.
    
    You're a star.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 992b03a38308d77db52f6a2a0f2b40c6fa08e305
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Dec 20 22:37:33 2025 +0100

    coreboot/default: use 3rdparty/cmocka by default
    
    this is used by the coreboot build system, for tests.
    
    upstream tries system cmocka by default, but we want the
    one in 3rdparty to be used.
    
    i've changed the default to 0 (try 3rdparty cmocka). you
    can still override this at runtime.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e7628421d7ef07d02f8d074f53e3a00cea465012
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Dec 20 22:29:56 2025 +0100

    coreboot/default: add cmocka submodule
    
    otherwise, we get the following from coreboot's build
    system, when performin operations in it:
    
    tests/Makefile.mk:31: No system cmocka, build from 3rdparty instead...
    
    However, *we* (Libreboot project) patch coreboot's build system,
    so as to not download submodules itself, because lbmk handles
    them manually. This is because lbmk's submodule handling has some
    extra redundancy features.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 946ede9e7d6af4823b40ad8b6f8b812d75df4e4d
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Dec 20 20:14:07 2025 +0100

    fix ifdtool build on coreboot/default
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 24f25bf7bc193e9afa6bca558522c68ecd054217
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Dec 19 08:17:44 2025 +0000

    update deguard
    
    this adds the recent kaby thinkpads
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 63b527e8ff7eb173fc479d572a286e5938883afd
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Dec 19 07:30:59 2025 +0000

    cb/default: bump to rev def7aa7094, December 2025
    
    latest coreboot revision of of today / right now at the
    time of this commit
    
    this brings the following upstream changes:
    
    * def7aa7094 arch/riscv/smp: Fix race condition
    * fc37085ddb Documentation/vboot: Update vboot supported boards list
    * 5bb7a83a7a acpi/acpi_apic.c: Generate MADT LAPIC entries based on current mode
    * 077191641b vendorcode/amd/opensil/Makefile.mk: Add 0x prefix for BIOS address
    * 4b353affd4 soc/amd/common/acpi/lpc.asl: Add HPET device
    * 0a867b3971 acpi/ivrs: Fill second EFR image value
    * 92f03c0c28 mb/google/ocelot/var/kodkod: Config touchpad I2C frequency
    * a923470688 drv/intel/mipi_camera: Remove duplicate comments for DSM methods
    * 896984e800 mb/google/bluey/quartz: Enable parallel charging support
    * de87ea0efa mb/google/bluey: Add parallel charging infrastructure
    * cfb0d8a144 mb/google/bluey: Enable PD negotiation when battery is missing
    * ddc1b51b43 mb/google/bluey: Enable PD negotiation in charging modes
    * c4ee22e267 Reapply "mb/google/bluey: Implement EC-based off-mode detection"
    * a225eefd4c drivers/spi: Add Macronix MX77U51250F chip id
    * 793c15a866 mb/google/ocelot: Fix Gen4 SSD power sequencing
    * b689671e79 include/acpi/acpi_apei.h: Add MCE APEI structs
    * 7a41dc416b include/acpi/acpi_apei.h: Add NMI APEI struct
    * 5251284e39 include/acpi/acpi_apei.h: Add PCIe APEI structs
    * 53350d5c8d include/acpi/acpi_apei.h: Add internal acpi_head_t struct
    * 5001b07f9c drivers/intel/mipi_camera: Add validation and remove unused defaults
    * 0f1ae4ae5f drivers/gfx/generic: Add support for non-VGA devices
    * ae0d232402 drivers/intel/mipi_camera: Add ACPI device type selection
    * 1532eb60ee drv/intel/mipi_camera: Add CVF Support DSM function
    * a64b93562d drv/intel/mipi_camera: Add I2C V2 DSM function
    * c8f89e00e4 drv/intel/mipi_camera: Refactor DSM generator functions
    * ea099e8b8c drivers/intel/mipi_camera: Split DSM generation into per-UUID functions
    * 6459a2007a mb/{google,intel}: Add ROM type and address for MIPI camera sensors
    * 11ea868b02 mb/google/volteer/var/drobit: Update pl2 minimum value
    * 8ed87f71ec mb/google/volteer: Make touchpad wake user-selectable via CFR
    * 7e1962a3cc mb/google/hatch: Make touchpad wake user-selectable via CFR
    * 152914272c device/azalia: Drop spurious read-back of STATESTS
    * 6e074550a5 device/azalia: Repurpose azalia_set_bits() for link-reset only
    * ecf202b8e4 device/azalia: Add missing 521us delay after RESET# de-assertion
    * d5d27badd4 Documentation: Add coreboot release 26.03 template
    * 8e23c46beb Docs/releases: Update release notes for 25.12 release
    * 01bc527afa soc/qualcomm/common: Add CMD-DB driver support
    * 2277edff88 soc/qualcomm/x1p42100: Split dram_aop region to map dram_aop_cmd_db as non-cacheable
    * a4cc178486 soc/qualcomm/common: Map AOP CMD-DB region as uncached region in MMU
    * 1b5f105595 mb/google/ocelot/var/ocelot: disable ISH UART0 RX pin
    * b67725d3f5 Revert "mb/google/bluey: Implement EC-based off-mode detection"
    * 1dc3e45f7c mipi: Support passing user data to mipi_cmd_func_t
    * 42c1947d99 mb/google/bluey: Implement EC-based off-mode detection
    * e54b82b85b mb/google/ocelot/var/kodkod: Enable pcie_rp5 to allow proper enumeration of pcie_rp6
    * 4cc830349c mb/google/rex/var/karis: Add fw_config probe to enable all wifi
    * 173a32aa55 MAINTAINERS: Add Jayvik Desai to Google Bluey & Qualcomm SoCs
    * f28997dcdd soc/qualcomm/common: Add PD negotiation attribute macro
    * b70309350f arch/x86/acpi_bert_storage.c: Fix Error Section GUID compare
    * 847d91b82e include/acpi/acpi_apei.h: Update APEI structs for better readability
    * 679ea61d4d include/acpi/acpi_apei.h: Add APEI definitions
    * eb79807bec soc/qualcomm/x1p42100: Add mainboard hook for QcLib override
    * 8ece648c30 soc/qualcomm/common: Use bitwise OR for global_attributes
    * 22e54a701d soc/qualcomm/x1p42100: Add AOP, QDSS, and QSEE regions to SSRAM layout
    * 4d53aa7704 soc/qualcomm/x1p42100: Relocate PRERAM stack to BSRAM memory
    * a26b718d5a soc/qc/x1p42100: Define pre- & post-RAM stack regions in linker script
    * 1b599a8844 arch/arm64: Add an alternative entry point for ramstage code
    * 641f7ac677 arch/arm64: Introduce distinct PRERAM and POSTRAM stack regions
    * 2183326306 soc/qualcomm/x1p42100: Rename qcsdi region to aop_sdi in memlayout
    * fad9878a3e vc/intel/fsp/fsp2_0/wildcatlake: Update WCL FSP headers to FSP WCL.3393.02
    * 71b79018da util/release/genrelnotes: Restore to saved HEAD instead of origin/main
    * 9f6e297399 vc/intel/fsp/mtl: Fix license header in MemInfoHob.h
    * 2171af0f5f mb/lenovo/sklkbl_x280: Fix build failure
    * a5d3c4c119 mb/lenovo/sklkbl: Fix headphone jack
    * 0b4d41004d mb/lenovo/sklkbl: Add Lenovo Thinkpad X280 as a variant
    * 9a3818f9b6 soc/mediatek/common: Print DRAM calibration status as string
    * 1e8cea55a0 soc/mediatek/common/emi: Cache SDRAM size
    * 9203cc827f soc/mediatek/mt8196: Add MTE tag memory to bootmem
    * 3d5135fdd0 lib/bootmem: Add memory type for Armv9 MTE tag storage
    * 2d45723d87 lib/bootmem: Add bootmem_add_range_from function
    * 4f78a40f53 mb/google/ocelot/var/ocicat: Update Touchscreen settings
    * 510f1950b4 mb/google/ocelot/var/ocicat: Update devicetree
    * 35cb6aea50 mb/google/hatch: Fix CFR pointer
    * 8f34fdfab3 Remove <swab.h> and swabXX() functions
    * d556bc65c2 mb/google/fatcat/var/ruby: Change GPIO pins to fix audio function
    * 6b5a872ce8 soc/mediatek: Pass dsi_regs/mipi_tx_regs to DSI API
    * 74c13eead4 soc/mediatek/mt8196: Define dsi_regs/mipi_tx_regs structs
    * a3317182ff soc/mediatek/common: Move dsi0 definition to dsi_register_v*.h
    * 3607024944 soc/mediatek: Move mtk_dsi_init declaration to display_dsi.h
    * 7ef424c75e soc/mediatek/common: Rename mipi_tx to mipi_tx0
    * cf0b91d774 soc/mediatek: Move dsi_regs/mipi_tx_regs definitions to soc/dsi_reg.h
    * 403a42f1f0 soc/mediatek/mt8173: Fix mipi_tx1 address
    * 72010408b5 mb/google/eve: Add CFR option menu support
    * c36b149392 mb/google/eve/Makefile: Organize and group entries by stage
    * f61ecfa154 mb/google/link: Add CFR option menu support
    * 75460f531c mb/google/poppy: Add CFR option to enable/disable IPU cameras
    * ae8f2d8cee mb/google/poppy: Add CFR option menu support
    * 99d67bae63 mb/google/glados: Add CFR option menu support
    * 0d81d38a31 mb/google/skyrim: Add CFR option menu support
    * c397821cb6 mb/google/guybrush: Add CFR option menu support
    * c1f0be39da mb/google/zork: Add CFR option menu support
    * d105934073 mb/google/kahlee: Add CFR option menu support
    * 46a32a2b56 mb/google/sarien: Add CFR option menu support
    * e3bee6397d mb/google/rex: Add CFR option menu support
    * d0345005ad mb/google/brox: Add CFR option menu support
    * ee599486ac mb/google/dedede/galtic: Add CFR option for touchpad type
    * 7a78543eca mb/google/dedede/drawcia: Add CFR option for touchscreen selection
    * 1890c6d165 mb/google/dedede: Add CFR option menu support
    * bd89858f09 mb/google/brya: Add CFR option menu support
    * a58e0704c7 mb/google/volteer: Add CFR option menu support
    * 8b34490137 mb/google/drallion: Add CFR option menu support
    * 0762c7d6ee mb/google/drallion/Makefile: Organize and group entries by stage
    * 89902e8c80 mb/google/hatch: Add CFR option menu support
    * 0f93d154b2 mb/google/octopus: Add CFR option menu support
    * f0346845cd mb/google/reef: Add CFR option menu support
    * 2b959f4560 mb/google/puff: Add CFR option for automatic fan control
    * a8a77f9da2 mb/google/fizz: Add CFR option for automatic fan control
    * cde4280796 soc/intel/apollolake: Add CFR objects for existing options
    * 2e10f75751 mb/google/sarien: Increase size of SMMSTORE to 512KB
    * 91c6a0b5e6 mb/google/reef: Increase size of SMMSTORE to 512KB
    * 36a345f99e mb/google/octopus: Increase size of SMMSTORE to 512KB
    * d7cb2d2bc5 mb/google/drallion: Increase size of SMMSTORE to 512KB
    * d32a372846 drivers/smmstore: Increase default size of store to 512KB
    * c109fc92ff libpayload: Add API to get physical memory size
    * 519332de10 mb/google/fatcat/var/ruby: Modify VCCCORE VR Fast Vmode ICC limit
    * 7f93e2fe29 soc/intel/*: Add CFR option to enable/disable the Intel iGPU
    * d5ea359347 soc/intel/**/cfr.h: Fix typo of "ACPI" in UI help text
    * 02a2fe7907 Merge coreboot and libpayload <endian.h> into commonlib
    * 5eb7b8bd34 payloads/external/edk2/Makefile: Set SMBIOS to 3.0.0
    * c3afc13a0a soc/qualcomm/x1p42100: Update memlayout for BL31 region and realign TA region
    * 04f83ff7dc cpu/x86/mtrr: Simplify MTRR solution calculation on AMD systems
    * 6957f84aa7 soc/qualcomm/x1p42100: Define MDSS domain registers for display clock enablement
    * 9a95aef482 soc/qualcomm/common: Add API to enable Lucidole PLL for X1P42100
    * 5eaf85d19b soc/intel/skylake: Use CSE reset status for reset
    * 84a4cdc6a5 soc/intel/*: Only skip PMC fallback on successful CSE reset
    * 4f52ca6ba6 soc/intel/common/cse: Return usable error codes
    * 8795680828 cpu/x86/lapic/lapic.c: Set spurious interrupt vector to 0xF
    * 41348477e3 sb/intel/common/firmware/Makefile.mk: fix INTEL_IFD_SET_TOP_SWAP_BOOTBLOCK_SIZE
    * c11faad2bf mb/google/skywalker: Correct MIPI panel power sequence
    * 459cdd09f4 mb/google/rauru: Add variant-specific firmware config
    * fee2befc82 3rdparty/blobs: Update to upstream main
    * 626789b40a mb/siemens/mc_ehl{1..5}: Unify devicetrees SerialIoI2cPadsTermination
    * 0513c45a38 mb/google/nissa/var/pujjoga: Generate RAM ID for MT62F1G32D2DS-031RF WT:C
    * 4a5d0dee4a soc/mediatek/mt8189: Correct AUX LDO mask bit definition
    * db01aa6cb2 commonlib/device_tree.c: Fix skipping NOP tokens
    * 29bec62a22 cpu/x86/Kconfig: Remove SOC_SETS_MSRS option
    * f4aeac4276 soc/amd/glinda: Set FSP UPDs from devicetree for USB4
    * f68450e39b vendorcode/amd/fsp/glinda: Update FSP UPDs
    * 244e8edf18 soc/amd/glinda/Kconfig: Add Faegan SoC as Glinda variant
    * 9e5c7eb3f8 soc/amd/glinda: Add XGBE devices
    * 87475d693a soc/amd/glinda: Remove set_resets_to_cold
    * dcd4f07188 soc/amd/common/fsp: Fill in DIMM voltages
    * 8929659d93 soc/amd/common/acpi/lpc.asl: Report ESPI1 fixed resource
    * 3053cd2dad soc/amd/common/acpi/lpc.asl: Report fixed base addresses
    * 7e1aa974bf soc/mediatek/common: Refactor mtk_ddp_mode_set to support dual DSI and DSC for MIPI
    * 3aaeca8378 soc/mediatek/common: Refactor mtk_dsi_dphy_timing
    * aeee9450a2 mb/google/ocelot/var/matsu: Add fw_config definitions with UFSC
    * 67a7e06c38 drivers/tpm: Remove duplicated op
    * ac5c57d24a drivers/tpm/ppi: Fix generated ACPI
    * d922ad79c6 mb/google/fatcat/var/ruby: Add firmware configuration fields with UFSC
    * 980c269643 mb/google/ocelot/var/ojal: Enable Audio Codec and update FW config
    * 8e7975edfd mb/google/ocelot/var/ojal: enable CS42L43 driver options
    * 9a6d1d4d69 mb/ocelot/var/ocicat: Modify ocicat Kconfig for bring up
    * 94fe4c6926 mb/google/ocelot/var/kodkod: Add wake configuration to cnvi_bluetooth
    * e3588d82bc mb/google/ocelot/var/kodkod: Enable CNVi Wi-Fi and BT cores
    * 36edc2e371 soc/qualcomm/x1p42100: Add Dload mode detection and ramdump packing
    * 26a18c674d acpi: Clear whole FACS table before filling it
    * 5a6addca4b mb/google/fatcat/var/ruby: Change GPIO pins to fix audio function
    * e6a8143d8b drivers/intel/touch: Add support for new Intel touch I2C _DSD entries
    * 25c4501223 device/dram/ddr3: Fill in voltage fields for SMBIOS type 17
    * 273a41c4d9 commonlib/memory_info: Introduce new fields to memory_info structure
    * abe1ac0744 mb/google/brya/var/uldrenite: Add memory MT62F1G32D2DS-031RF WT:C
    * 0599f3e1bd mb/google/bluey: Condition slow charging enablement on charger presence
    * 8b3ceacd93 ec/google: Check AC charger presence by reading host event register
    * 5b544c67eb mb/google/rauru: Add MIPI panel support with BOE NS130069-M00
    * ed9239cd85 mb/google/nissa/var/gothrax: Add Samsung parts to RAM ID table
    * 445961c604 soc/qualcomm/common: Add support for loading ramdump image
    * 3c563669b5 soc/qualcomm/x1p42100: Add support for APDP image packing in CBFS
    * 1d70286d4e soc/qualcomm/common: Add support for loading APDP image
    * fc9f828ac0 mainboard/google/bluey: Select VBOOT_ALWAYS_ENABLE_DISPLAY
    * c77d256886 {mb, security}: Use EC_REBOOT_FLAG_IMMEDIATE for cold reboots
    * 1a0d123ec1 ec/google/chromeec: Update EC headers
    * 3bd554feb2 soc/mediatek/mt8196: Align the struct for storing DRAM calibration data
    * 33fc33c132 soc/amd/common/block/cpu/noncar: Add support for bootblock CRTM init
    * 8b97968e53 soc/amd/common/block/pci/amd_pci_mmconf.c: Support 64bit ECAM MMCONF
    * 7a98a62f7b drivers/intel/gma: Unify coding style
    * 23b00a06da drivers/intel/gma: Fix brightness handling with valid-cache logic
    * 4068ba39f8 soc/intel/common/block/rtc/rtc.c: Top Swap: add Slot B selection mechanism
    * a65b874472 mb/dell: Convert OptiPlex 3050 into variant
    * 2ce1068542 mb/google/ocelot/var/ojal: Decrease display count from 5 to 4
    * b31e62ae5c mb/intel/ptlrvp: Add LPCAMM T3 RVP board support
    * 58cdf9e668 soc/intel/pantherlake: Add LPCAMM memory support
    * 67777b7671 soc/intel/common: Add LPCAMM memory topology support
    * bb18e0b91d mb/google/skyrim: Increase size of SMMSTORE to 512KB
    * dd1e54efc0 mb/google/zork: Increase size of SMMSTORE to 512KB
    * 60e375fae4 mb/google/guybrush: Increase size of SMMSTORE to 512KB
    * eb52862132 mb/google/brox/var/caboc: Update SSD port and FPMCU setting
    * 85101704ae mb/ocelot/var/ocicat: Create ocicat variant
    * 2975d7220a mb/google/ocelot/var/matsu: Update devicetree
    * 0e9d85425e mb/google/ocelot/var/matsu: Fix GPP_V3 internal pull-up configuration
    * 4f257a28f8 mb/google/ocelot: Add EC_GOOGLE_CHROMEEC_SKUID config
    * db2ac42405 soc/mediatek/common: Refactor DDP mode setting
    * d51f780515 mb/siemens/mc_rpl: Correct SMBIOS socket type to BGA1744
    * 2f95552802 mb/siemens/mc_ehl: Move Kconfig switch to variants
    * 3c49c13995 util/ifdtool: fix typo PSL->MSL
    * a87e699f04 mb/lenovo/m900_tiny: Enable Vboot
    * afc191357f 3rdparty/intel-microcode: Update to upstream main
    * cbfa28b06e mb/google/fatcat/var/ruby: Modify power limit configuration
    * f13e800a71 mb/amd/crater/Kconfig: Use A/B recovery scheme for renoir
    * 416f67f670 vendorcode/amd/fsp/.../fsp_h_c99.h: Use fsp2_0 structs
    * b94a84a792 drivers/efi: Exclude verstage from EFI variable store files
    * bae5262c69 include/option: Add verstage stub for UEFI variable store backend
    * 2d78478345 drivers/intel/gma: Reapply cached brightness once BCLM is valid
    * 2ad08f9d72 drivers/intel/gma: Expose full brightness ladder
    * 2e96a71e6f drivers/intel/gma: Cache brightness level
    * 36632a08a8 soc/qualcomm/x1p42100: Reserve 33 MB DRAM memory for Display requirement
    * 5807b59fc5 mb/google/rauru: Report panel ID for sapphire
    * 49da58dccf drivers/mipi: Add support for BOE NS130069-M00 panel
    * e9ebcb2918 mb/{google,intel}: Fix MIPI camera VCM type and address configuration
    * 30b4383944 mb/{google,intel}: Set SSDB platform field for MIPI camera sensors
    * 4c025191c7 drivers/intel/mipi_camera: Remove disable_ssdb_defaults option
    * c75236d436 drivers/intel/mipi_camera: Set additional SSDB defaults
    * 866b79c9fe drivers/intel/mipi_camera: Always generate PLD for camera sensors
    * c6ed8c91fb drivers/intel/mipi_camera: Document more SSDB fields
    * 423fbcd06b drivers/intel/mipi_camera: Adopt SSDB sensor SKU bitfield
    * aa18a6fe8d drivers/intel/mipi_camera: Codify SSDB field enums
    * f8d12a0bdb drivers/intel/mipi_camera: Add SSDB platform subtype enum
    * 99cb6415ba drivers/intel/mipi_camera: Rename flash enum to match SSDB field
    * c91ea7c582 drivers/intel/mipi_camera: Flesh out SSDB platform enum
    * 0361e1a865 drivers/intel/mipi_camera: Verify SSDB struct size at build time
    * b5d68e41a2 drivers/intel/mipi_camera: Tidy SSDB comment wrapping
    * ab4c2fd0e8 drivers/intel/mipi_camera: Extract SSDB definitions into separate header
    * d09ea1c351 cpu/intel: Add SMBIOS Socket BGA1744 type
    * d97cb61b50 ec/google/chromeec: Add CFR option for RGB keyboard boot color
    * e695731399 ec/google/chromeec: Add RGB keyboard helper functions and enum
    * 4eb524ee9d spd/ddr4: Add three more parts
    * e4a809d441 spd/ddr4: Double packageBusWidth of dual die package parts to 16
    * 8753155f71 mb/google/slippy/var/peppy: Add CFR menu option for touchpad type
    * 6f6a10df88 mb/google/slippy: Add CFR option menu support
    * e366e0ba7d mb/google/slippy/Makefile: Organize and group entries by stage
    * 6be83443e5 mb/google/auron/var/lulu: Add CFR option to enable/disable touchscreen
    * 88d3f563b3 mb/google/auron: Add CFR option menu support
    * 7ed515d1c3 mb/google/auron/Makefile: Organize and group entries by stage
    * e15895b5c4 mb/google/poppy/var/nautilus/acpi: Fix CI02 comment
    * 4dc03c54fc mb/google/poppy/var/nocturne: Hide FPR device in ACPI
    * e85a0b7ff1 mb/google/puff: Remove unsupported EC features
    * 3459502e0c mb/starlabs/starfighter: Enable pmc_shared_sram device
    * 9b0af48604 mb/starlabs/starbook/mtl: Update GPIO config
    * d3d4571411 soc/intel/common/block/graphics: Use Xeon W-11865MRE IGD PCI ID
    * 1cfe413f95 soc/intel/common/block/lpc: Support RM590E eSPI
    * c195859748 soc/amd: add ACPI code for I3C controller
    * 02342b31df soc/amd/*/memmap.c: Report FCH MMIO regions as reserved
    * 5078d32ccc mb/google/brya: Enable ACPI S3 sleep state support
    * eb504eb49a mb/samsung/lumpy: Fix HDA pin configuration issues
    * afd5e5d444 mb/samsung/lumpy: Convert HDA verbs to use AZALIA_PIN_DESC macros
    * 109672a9a9 drivers/intel/gma: Guard legacy brightness fallback
    * 908c2b54c6 mb/starlabs/starbook/mtl: Fix Card Reader USB Port
    * 796d3b37aa mb/google/fatcat/var/moonstone: Update fw_config definitions with UFSC
    * 2ce4e09469 drivers/intel/fsp2_0: Add typedef FSP_UPD_HEADER
    * 7afe1e9f9d mb/google/fatcat/var/lapis: Adjust touchpanel power on timing
    * 36f4341533 mb/starlabs/starfighter: Add Arrow Lake (285H) variant
    * 80cf2008a9 spd/lp5: Add SPD for MT62F1G32D2DS-031RF WT:C
    * f1d1c825dc mb/siemens/mc_rpl1: Enable IBECC
    * 866a0591f7 mb/siemens/mc_rpl1: Set coreboot ready LED
    * 801795d4dd mb/siemens/mc_ehl6: Alphabetize Kconfig options
    * 1a11dca12d mb/siemens/mc_ehl6: Send POST codes to NC FPGA via PCI
    * fceb033372 mb/siemens/mc_ehl6: Limit PCIe RP7 speed to Gen2
    * 760c3f6abc mb/siemens/mc_ehl6: Activate SATA interface port 1
    * 54f2652bde mb/siemens/mc_ehl6: Enable auto impedance calibration on GbE 0
    * b6e7f3e005 mb/siemens/mc_ehl6: Change GbE LED settings
    * aad2b715ea mb/siemens/mc_ehl6: Remove PSE GbE 1
    * e19f2b313e mb/siemens/mc_ehl6: Enable PCHHOT_N via GPIO
    * 43d5f70576 mb/siemens/mc_ehl6: Enable PTM for all enabled PCIe RPs
    * 864e3ca661 mb/siemens/mc_ehl6: Adjust I2C setup
    * 31f44f5521 mipi: Add DSC configuration and rate control parameters to panel header
    * 743e31939c drivers/intel/fsp2_0/.../fsp/upd.h: Fix excess endif
    * 2aadfc2b5e soc/amd/common/block/acpi: Add ACPI HEST table
    * cc542c15f4 include/acpi: Move Error definitions/declarations into acpi_apei.h
    * 2462e3a027 soc/mediatek/mt8188: Adjust memlayout for bootblock
    * 804aab3abb mb/google/fatcat/var/ruby: Modify usb3 port setting
    * fecf05c4f2 mb/google/trulo/var/kaladin: Mute speaker amp to prevent pop noise on reboot
    * f35cb39de5 soc/amd/cezanne: Increase APOB DRAM size for Renoir
    * 384e6e1c37 soc/amd/cezanne: Remove set_resets_to_cold
    * 97291b5838 soc/amd/cezanne: Optionally propagate UART0 through ACPI
    * 149d11d1d8 soc/amd/cezanne/Kconfig: Select Kconfig to program the PSP_ADDR MSR
    * 520bc70b57 mb/amd/crater: Configure UART1 GPIOs
    * 0ed1529ce3 src/vc/amd/fsp: fix type 17 DMI info
    * e8599956dc mb/amd/crater: Make NVMe reset GPIO configurable
    * 67bf203e52 mb/google/nissa/var/guren: Add missing settings for WWAN
    * 3dabe4f857 mb/google/brox/var/caboc: Increase I2C0 touchpad tHD to 0.53 us
    * 12e763eece device/pci_ids: Add DIDs for TGL-H (GT1 and RM590E)
    * 01540f036e mb/google/fatcat/var/ruby: Use spd-11 for MT62F1G32D2DS-020 WT:D and K3KL8L80EM-MGCV
    * 14e96b4ef1 mb/google/brox/caboc: Mute speaker amp to prevent pop noise on reboot
    * 567186a000 mb/google/bluey: Add support for off-mode charging
    * 27fcb8617d commonlib: Add CBMEM ID to store boot mode
    * d6132c4c03 mb/google/bluey: Use SOC PMIC API to detect off-mode charging event
    * 201ebd48ee soc/qc/x1p42100: Add APIs to read PON reason from PMIC
    * 293f3a7f5c soc/qc/spmi: Add API to read byte array
    * 9f675eb96b soc/qc/common: Update SPMI_ADDR macro for better type safety
    * cb1045a8b8 soc/intel/pantherlake: Update GT domain TDC value for PTL_TDC_1 SKU
    * b0ee0c4620 soc/intel/pantherlake: Fix IA domain TDC value for PTL_TDC_2 SKU
    * 6dbcf903a5 soc/intel/pantherlake: Add ICC Max and TDC settings for SKU_7
    * 2148143ae9 soc/intel/pantherlake: Separate TDC configuration for different TDPs
    * c7273c8ddc mb/google/fatcat/var/ruby: Add proto touch panel address
    * 8575232317 mb/google/ruby: Migrate to UFSC
    * 47101fc224 soc/amd/cezanne/Kconfig: Make AMDFW_CONFIG_FILE configurable
    * c9f124a8fb mb/amd/crater/ec: Make macro ENABLE_M2_SSD1 a Kconfig option
    * 430e34cd0f mb/amd/crater: Move gpio configuration to early_gpio
    * 14b5c004f5 mb/amd/crater/ec: Create function to get board revision
    * bd858faee8 mb/amd/crater: Add XGBE support
    * f61553c9fa vc/amd/fsp/cezanne: Add Renoir FSP
    * 87f8d15c87 ec/google/chromeec/cfr: Fix CFR callback signatures
    * 7fb0f14ebe libpayload: arm64: Fix asynchronous exception routing in payload
    * b584967d04 mb/google/ocelot: Add wake configuration to cnvi_bluetooth
    * fc88b62174 mb/siemens/mc_ehl6: Enable PCIe root ports and clocks
    * 5a4c749520 mb/siemens/mc_ehl6: Add new board variant based on mc_ehl2
    * f5f304a5f3 mb/google/skywalker: Disable CHROMEOS_USE_EC_WATCHDOG_FLAG
    * e4b0410946 soc/mediatek/mt8189: Enable MEDIATEK_WDT_RESET_BY_SW
    * 1ae0cebff3 soc/mediatek: Add Kconfig option MEDIATEK_WDT_RESET_BY_SW
    * 7d3bf767cc soc/mediatek/mt8189: Move WATCHDOG_TOMBSTONE from SRAM to SRAM_L2C
    * a64dd410d8 mb/google/fatcat/var/ruby: Update EN_SPK_PA GPIO pin configuration
    * 2f9b4ad6a5 soc/qualcomm/x1p42100: Add DFSR table configuration support
    * 51e99de558 soc/intel/common/block/rtc/rtc.c: control Top Swap via CMOS option
    * 56be23114e mb/google/rauru: Use chromeos-legacy.fmd for Hylia and Navi
    * 10802bac16 spd/lp5: Modify SPD for MT62F1G32D2DS-020 WT:D and K3KL8L80EM-MGCV
    * e5d10e5d23 mb/lenovo/t480: Fix headphone jack
    * 40b2a2b03c soc/mediatek/mt8196/booker: Refactor CMO property clearing with loop
    * 1b0f9c5458 mb/google/nissa/var/telith: Add parade touchscreen support
    * ddb3f0b17f drivers/hwid_dmi: Populate SMBIOS product name from CBFS hwid file
    * 7a1e63308a acpigen_ps2_keybd: map insert
    * 607740999d acpigen_ps2_keybd: map capslock
    * a7efa40e39 acpigen_ps2_keybd: map KEY_HOMEPAGE to 0xaa scancode and TK_HOME
    * fd603e5102 libpayload: Add CBMEM_ID_MEMINFO to sysinfo
    * bae3e02662 include: commonlib: Move memory_info and dimm_info to commonlib
    * d03799ec3c soc/mediatek/mt8196: Configure registers and parameters required for MTE
    * 7521f3ea83 soc/qualcomm/x1p42100: Define pre and post-RAM DMA coherent regions
    * d277b35307 soc/qualcomm/x1p42100: Relocate ddr_information and watchdog tombstone
    * 958099b114 soc/qualcomm: Map the post-RAM DMA coherent buffer
    * 931fa9c01d memlayout: Introduce PRERAM and POSTRAM DMA coherent regions
    * af9d809823 soc/qualcomm/x1p42100: Move coreboot stack to SSRAM
    * fec1032ee8 arch/arm64: Add timestamps for Secure OS (BL32) loading
    * d0177bd102 soc/qualcomm: Add QCLib execution timestamps
    * 0145ebe847 commonlib: Add timestamps for Qualcomm QCLib and ARM TFA
    * cf2978f4b6 drivers/vpd: Search VPD info at 0x0 first
    * a56a97d167 mb/starlabs/common/cfr: Adjust help text for S0IX
    * 541fd14fd9 mb/google/nissa/var/uldren: Increase Touch IC enable delay time
    * 0c18e7680a mb/google/fatcat/var/ruby: Remove GPP_D16 and GPP_D17 in fw_config.c
    * 237944186e mb/google/nissa/pujjolo: update verb table
    * cae53bea52 ec/google/chromeec: Add CFR options for keyboard backlight and fan control
    * 1ca1dc6c31 ec/google/chromeec: Add ability to enable auto fan control via setup option
    * 450389be05 ec/google/chromeec: Add fan presence helper function
    * 9d355a39aa mb/amd/crater/ec.c: Enable power/reset for PCIe lanes
    * 141fd11d79 mb/amd/crater: Rename ETH_AIC_SLOT_ONLY -> PCIE_DT_SLOT
    * 1e28ff6955 src/mb/amd/crater/port_descriptors_renoir.c: Prettify code
    * a48fd9ed7f soc/amd/cezanne: Add SOC_AMD_RENOIR as a Cezanne variant
    * 760e19e18f mb/lenovo/sklkbl: Use spd_tools infrastructure for SPD binaries
    * 8a83b86254 spd/ddr4: add parts
    * 0ef4bd807c mb/google/ocelot/var/ocelot: Update DDR5 memory configs
    * 7b11254d58 mb/google/ocelot/var/kodkod: Add overridetree
    * aa1d44b644 mb/google/ocelot/var/kodkod: Update gpio settings
    * a363007c3b ec/dell/mec5035: Route power button event to host
    * 18dbeca5f4 util/autoport/azalia.go: Select CONFIG_AZALIA_USE_LEGACY_VERB_TABLE
    * e9c47bf99e drivers/intel/fsp2_0: Add 1-bpp monochrome option for VGA mode 12
    * f643141728 mb/google/fatcat: Option to enable monochrome VGA mode 12
    * e05492cfb4 soc/intel/pantherlake/romstage: Configure VGA mode 12 monochrome buffer
    * d3760cdfdf mb/google/bluey: Configure QUP0 SE5 as I2C
    * abc87d533d mb/google/bluey: Introduce config to specify absence of USB-A port
    * 872e06d60c mb/samsung/stumpy: inline fan thresholds and drop GNVS programming
    * 8401bbd2ff mb/google/fatcat/var/ruby: Change touch panel address
    * a4242e5c38 ec/starlabs/merlin: Fix get_ec_value_from_option() value validation
    * 567470cbb3 payloads/edk2: Add iPXE EFI support for EDK2 payload
    * f40de4e162 payloads/ipxe: Default enable serial output only if CONSOLE_SERIAL
    * 962edb7e6d payloads/ipxe: Guard PXE_ROM_ID for non-EFI builds
    * a907c6fb8d payloads/ipxe: Default to building from master branch
    * 4081793ff2 payloads/external/edk2: Replace dependencies on EDK2_REPO_MRCHROMEBOX
    * 10d606bfca soc/intel/common/acpi: Add P2SB write functions
    * 7436c59875 util/amdtool: Add support for Phoenix AM5 CPUs
    * 8f3626c4b5 util/amdtool: Add utility to dump useful information on AMD CPUs
    * 3cf976e51a soc/mediatek/mt8196: Add dual display pipe path
    * 14595d64de lib/edid_fill_fb: Add dual pipe flag to lb_framebuffer_flags
    * 486b1b51af mb/google/bluey: Cache low battery mode check
    * 33418b7e68 soc/qc/x1p42100: Disable compression for peripheral firmware binaries
    * 5bfc2d23bb soc/qc/x1p42100:: Select Secure OS options in SoC Kconfig
    * f5f943c1c3 bluey/kconfig: Consolidate SPI flash driver selection
    * ee59936e83 commonlib/device_tree: Add an API to check if a DT is an overlay
    * 0416ac9829 mb/google/var/fatcat/lapis: Modify type-A USB3 port0/1 tx_de_emp
    * dfe553aebb util/intelvbttool/Makefile: Add install target
    * 493e3d182e payloads/external/iPXE: Allow building EFI target
    * 62fc93de90 soc/qualcomm/x1p42100: Add NVMe Power Loss Notification GPIO configuration
    * 2b7b89ae31 soc/qualcomm/x1p42100: Update PCIE PHY init sequence
    * 91594f4894 drivers/option/cfr.c: Replace memcpy() to avoid uninitialized object
    * 04ea4724e2 Makefile.mk: separate bootblocks into BOOTBLOCK and TOPSWAP
    * f164feba3e ifittool: allow adding files from a separate region
    * 91073f37d7 mb/google/brya: Increase RW_SECTION_* by 256KB for 16MiB boards
    * a43498e193 util/inteltool: Enable dumping GPIOs from Tiger Lake IoT PCH
    * 04778ddd38 drivers/option/cfr: Remove old sm_object from constructor
    * b535db8f1e soc/intel/cmn/usb: Add helper macro for USB 3.0 port TX configuration
    * c197643d44 drivers/intel/gma/acpi: Add power management methods for GFX0
    * 773997c92d drivers/intel/touch: Avoid returning undefined pointer
    * 9dc35142ac soc/amd/stoneyridge: Generate SATA ACPI registers at runtime
    * d62653749c payloads/libpayload: Support legacy LZ4 compression format
    * 29faf77d4a mb/siemens/mc_rpl1: Limit CPU RP1 to PCIe Gen2 speed
    * fad0908c5b soc/intel/alderlake: Make CPU RP PCIe speed configurable
    * 04d5201426 treewide: Fix include guards
    * 971f10d1d7 mb/google/var/fatcat/lapis: set custom SVID/SSID to load fw for CS35L56
    * ace2e540d0 soc/intel/pantherlake: Update CONSOLE_UART_BASE_ADDRESS Kconfig value
    * a65d9fe589 mb/google/fatcat: Remove FSP_UGOP_EARLY_SIGN_OF_LIFE from Lapis
    * 729918628d 3rdparty/blobs: Update to upstream main
    * d4bee96484 mb/lenovo/sklkbl: refactor memory_init_params to use gpio_base2_value()
    * 2a9deabc35 commonlib/coreboot_tables.h: Fix lb_smmstorev2 alignment
    * 0ba4505024 payloads/external/edk2/Makefile: Configure AP wakeup in UEFI payload
    * 87c3373925 mb/google/fatcat: Add FW_CONFIG Support for TAS2563
    * e08a35f806 drivers/sof: Add support for tas2563 speaker topology
    * a3ea128ecf drivers/i2c/tas2563: Add driver for generating device in SSDT
    * 240e17025c src/soc/intel/ptl: Add LPSS UART DMA control
    * afa6c31ef5 soc/intel/alderlake/romstage/fsp_params.c: Refactor `pcie_rp_init()`
    * ec5b5386d4 soc/intel/mtl/romstage/fsp_params.c: Refactor `pcie_rp_init()`
    * 4c5c62bc8d mb/google/fatcat/var/ruby: Modify gpio pin for enabling audio function
    * 2804a0d771 mb/google/fatcat/var/lapis: Update fw_config definitions with UFSC
    * 0e1742a7e2 mb/google/bluey: Control slow battery charging via boot mode
    * 113cef70fd soc/intel: Move USB port macros (2.0/3.0/TCSS) to IA common header
    * 3c69295ce4 mb/google/fatcat/var/ruby: Add new supported memory part
    * d18cc50e6a soc/intel/xeon_sp: Use common smm_relocate
    * d0c936eea1 mb/google/nissa/var/guren: Tune SX9324 register for 5G LTE module
    * 8851b5b0e7 soc/intel/pantherlake: Program HDA SVID/SSID
    * c917ecf21e soc/intel/{adl,mtl}: Fix CLKSRC handling for compliance mode
    * 312d455a93 soc/intel/{adl,mtl}/romstage/fsp_params.c: Fix printf specifier
    * 786ac14d48 drivers/option/cfr: Add optional override table for default values
    * ea84a29a27 mb/google/trulo/var/pujjoquince: Enable Bayhub LV2 driver
    * 84e000b88e libpayload: arm64: Fix alignment for exception_state
    * 9210f2fd1c mb/google/fatcat/var/lapis: add ILITEK touchscreen support
    * b933a554ba mb/google/var/fatcat/lapis: Modify fw_config for audio and touch
    * b3b7b7a027 mb/emulation/qemu-q35/Makefile.mk: Use all-y for memmap.c
    * bb3e59051a mb/google/brya: Check power state before process _ON method for BT
    * 0f7c54d7d1 mb/google/fatcat/var/felino: Disable card reader in coreboot
    * 7946d2d65d mb/google/fatcat/var/felino: Add reset_gpio for SSD RTD3 configuration
    * 8e2567c7a9 mb/google/fatcat/var/lapis: Adjust touchpad I2C frequency
    * f8da2bf9b2 mb/google/fatcat/var/ruby: Modify camera enable gpio pin.
    * 45163509cf util/cbfstool/cbfs-payload-linux.c: Remove TODO
    * bdcd65bd7f ec/starlabs/merlin: Add battery capacity offsets
    * 224ddb85e3 ec/starlabs/merlin: Choose a better default for GPE SCI
    * 696344ac01 ec/starlabs/merlin: Optimise Kconfig defaults
    * 649a6a591b ec/starlabs/merlin: Correct Kconfig dependancies
    * 0d35c3fcc3 mb/starlabs/starbook: Fix inclusion of CPU RP ASPM option
    * 87475ef37f mb/starlabs/common: Move power profile enum to common code
    * 98e0ff1e4b mb/starlabs/*: Move DMIC disabling code to common dir
    * 082ad480d9 mb/starlabs/*: Separate WiFi and Bluetooth controls
    * abf630c96b mb/starlabs/byte_adl: Add wireless CFR object
    * a58d99575e mb/starlabs/*: Move CFR object defs to common directory
    * 74dcb4c679 mb/starlabs/common: Adjust the includes inline with coreboot
    * 6b30e1f46b mb/google/dedede/var/pirika: Add support memory for CXMT CXDB5CBAM-MA-B
    * d4e8af407c spd/lp4x: Add CXMT CXDB5CBAM-MA-B memory
    * 8d7183a904 ec/google/chromeec: Add option to set keyboard backlight level at boot
    * 973d0faf65 util/amdfwtool: Move needs_ish and combo_new_rab to data_parse.c
    * 8449a15aed soc/qualcomm/x1p42100: Reduce USB OTG state enable timeout to 20ms
    * f4af55a008 spd/lp5: Add SPD for MT62F1G32D2DS-020 WT:D and K3KL8L80EM-MGCV
    * 9bea3130b5 mb/google/skywalker: Add AW88081 support for beep sound
    * d8bcf242c6 Revert "commonlib/endian: Silence GCC -Warray-bounds false positives"
    * 66039a61f1 Revert "commonlib/endian: Restore -Warray-bounds at the end of file"
    * c27c9db51b mb/samsung/stumpy: Fix thermal configuration issues
    * 288889cd98 mb/google/jecht: Fix thermal configuration issues
    * 9fa2d55987 mb/google/beltino: Fix thermal configuration issues
    * 76a48b5158 mb/google/beltino: Remove unused GNVS fan configuration
    * 8cc8f219bf Documentation/drivers: Add ACPI five-level fan control documentation
    * 47c4da36c4 util/amdfwtool/data_parse.c: Remove duplicate MP2_CFG_FILE
    * fa23504c59 mb/siemens/mc_rpl: Alphabetize Kconfig options
    * 8411fef90f mb/siemens/mc_rpl1: Switch from LPSS UART to legacy 8250 I/O UART
    * 534fceb36a mb/siemens/mc_rpl1: Set PCI bridge function for NC_FPGA
    * 8ce9255f82 mb/samsung/stumpy: Fix ACPI fan control FNP4 power resource
    * f12f292d91 mb/google/intel/*: Fix ACPI fan control FNP4 power resource
    * b9b95c917a mb/google/guado: Fix ACPI fan control FNP4 power resource
    * 8a518b2134 mb/google/beltino: Fix ACPI fan control FNP4 power resource
    * 6d751ef987 payloads/edk2: Drop EDK2_PCO_MMIO_EMMC Kconfig option
    * 4423a0b390 payloads/edk2: Drop EDK2_UFS_ENABLE Kconfig option
    * a86a5ebf7c payloads/edk2: Update default MrChromebox branch from 2502 to 2508
    * 40a8466655 mb/google/fatcat: Add option to enable VGA mode 12
    * 824992ddef soc/intel/pantherlake/romstage: Configure VGA mode 12 planar buffer
    * 74113f5d5e drivers/intel/fsp2_0: Add options to config VGA mode 12
    * 75318743c3 commonlib: Add pKVM DRNG related timestamps
    * 2646eeffb2 mb/google/skywalker: Create variant Dooku
    * 1b2675e4f2 soc/mediatek/mt8189: Require libbl31.a to exist
    * 9c0a914192 libpayload: Fix printf edge cases for precision
    * ebc2030ff7 soc/intel/pantherlake: Enable HW managed microphone privacy
    * 4d9dacae16 mb/google/fatcat/var/ruby: Modify touchpad device setting to enable function
    * 2821e8e2ae soc/intel/ptl: Remove redundant HAVE_BMP_LOGO_COMPRESS_LZMA Kconfig
    * eadf2ee4a3 ec/starlabs/merlin/cfr: Replace integer literals with named constants
    * 2bbc0f1ad3 mb/starlabs/starbook/{kbl,cml,tgl}: Control USB Bluetooth with wireless
    * 2c9596555b mb/google/skyrim: Use edge (vs level) triggering for PS2K
    * 0c3f304d5a mb/starlabs/byte_adl: Enable Wake On Lan
    * ca5b51ef9c mb/google/bluey: Select QTEE firmware dependent Kconfig
    * fe7b75d792 mb/google/fatcat/var/moonstone: Add fw_config touchscreen setting
    * cfdaff3f70 commonlib/endian: Restore -Warray-bounds at the end of file
    * 32ec29a51e mb/google/fatcat/var/kinmen: Disable RT721 clock stop support
    * 4b50bc9e5f payloads/external/U-Boot/Makefile: Add custom repo and tag
    * 3d41ac370d payloads/external/U-Boot/Makefile: Remove conditional
    * c3b5c8723e ec/google/chromeec: Add function to determine keyboard backlight presence
    * 7afd731849 ec/google/chromeec/acpi: Fix long battery string reporting for Windows
    * bc475414c7 MAINTAINERS: Add Chen-Tsung Hsieh for MediaTek platforms
    * 65dc0bdd7e mainboard/fatcat/lapis: Override PMC GPE configuration
    * 4ea33e5ffa mb/google/fatcat/var/lapis: Enable THC HID over I2C mode
    * 111da2557a mb/google/fatcat: Preserve VGPIO GPE for THC wake on touch
    * e167e56883 mb/google/nissa/var/yavilla: Add stop pin for G2 touchscreen
    * 1fa24898e2 soc/intel/common/block/pcie: Move speed helper to pcie_helpers.c
    * 66f40a86de ec/starlabs/merlin: Move version offsets to ECDEFs
    * 82367b8205 mb/google/ocelot/var/matsu: Add overridetree
    * 05c0e16593 mb/google/ocelot/var/matsu: Update GPIO table
    * 98c7be30ad mb/google/fatcat/var/moonstone: Support new schematic changes
    * 5eafe672e3 vc/intel/fsp: Update WCL FSP headers to 3344_03
    * 641eeca835 lib/vga_gfx: Fix left-up and right-up orientations
    * a4f067c058 Makefile.mk: Align FMAP COREBOOT region to 4k boundary
    * 410506b47c Makefile.mk: Print all FMAP region sizes in hexadecimal format
    * c189604f43 mb/goog/ocelot/var/ocelot: Enable rp5 if PCIE WiFi detected
    * 4a806a6865 ec/starlabs/merlin: Only show EC FW options for ITE EC
    * 04d9a0d0f0 ec/starlabs/merlin/Kconfig: Fix typo in description/help text
    * 1eda98a16e mb/siemens/mc_rpl1: Document CLKSRC 2 usage for PCIe RP5
    * 1815a204b4 src/mainboard/lenovo: Add smbios_slot_desc, fix register types
    * da204a92c1 vc/intel/fsp/wildcatlake: Expose PchHdaMicPrivacyMode
    * 105598545e soc/intel/pantherlake: Update thermal design current parameters
    * a5b51ab285 mb/google/rex/var/kanix: Add H58G66CK8BX147 to RAM ID table
    * d7f427a7d2 util/xcompile: Fix compiler detection on newer Linux distros
    * 82d90b1f21 Revert "mb/starlabs/*/rpl: Re-enable GpioOverride"
    * 784d8f25f9 mb/google/fatcat/var/ruby: Enable panel touch function
    * 56a8c40efa mb/starlabs/starlite_adl: Add missing ACPI entry for USB card reader
    * 4047a44b68 sio/nuvoton/common/nuvoton.h: Add common Nuvoton SIO LDNs
    * 3a1d6d5ded mb/google/brox/var/caboc: Adjust WWAN power off sequence
    * b42f74122c Documentation/mb/lenovo: Adjust docs for Thinkpad T470s/T580
    * 70e79f43b1 Haswell NRI: Print and fill in memory-related info
    * 1730d05ec3 nb/intel/haswell: Factor out `report_memory_config()`
    * 3bbfbd37e1 nb/intel/haswell: Factor out `setup_sdram_meminfo()`
    * 3ffb01e9cb mb/siemens/mc_rpl1: Disable I2C1 and enable I2C6
    * 4563db2807 mb/google/nissa/var/riven: Add H58G66CK8BX147 to RAM ID table
    * a748e8b82b mb/google/fatcat/var/ruby: Enable touchpad function using I2C interface
    * a92a2ee5d6 mb/starlabs/byte_adl: Expose fan control option in CFR
    * bbb895436f mb/nissa/var/pujjoga: Add single ram configuration
    * e2cf7f7dc7 soc/mediatek/common: Fix MMU assertion for framebuffer region
    * a5ddfa963f mb/starlabs/starlite_adl: Increase ME region size to match IFD
    * 7484a887b8 mb/google/ocelot: Fix EC sync IRQ configuration for board variants
    * b1ed60b910 mb/google/fatcat/var/ruby: Disable FSP_UGOP_EARLY_SIGN_OF_LIFE temporarily
    * fea1b2abbe mb/google/nissa/var/pujjocento: Adjust touch panel timing for stability
    * 8f1c54685a drivers/mipi: Fix pixel clock and enable C-PHY for TM_TL121BVMS07_00C
    * 979fdee1d9 soc/mediatek/mt8189: Support MIPI C-PHY interface
    * 44635f328c soc/mediatek/common: Add C-PHY support for MIPI DSI
    * c63e901b99 mipi: Add panel flags to support C-PHY interface
    * 385ae6669b mb/gigabyte/ga-h77m-d3h/devicetree.cb: Re-enable IGD and PCIe VGA
    * 2c6cf2c2a8 include/cper.h: Update CPER structures with __packed attribute
    * d97644dd3f mb/asrock: Add Z77 Extreme4
    * a73db6d451 mb/intel/ptlrvp: Add fw_config support for SPD selection
    * 3ef1cf9f84 soc/amd/turin_poc: Add Turin SoC structure as a copy of genoa_poc
    * 668d643e5c mb/lenovo/sklkbl_thinkpad: Add Lenovo Thinkpad T470s as a variant
    * 8a6f9bf731 ec/google/chromeec: Update EC headers
    * 745f1312aa include/cper.h: Update comments to UEFI spec version 2.10
    * aab8ad98b6 mb/google/ocelot: Create kodkod variant
    * 2279ba80e1 .gitignore: Add .clangd as a "Development friendly file"
    * cd4af952e7 mb/google/ocelot/var/ocelot: Update DDR5 memory configs
    * 1af54d9784 drivers/intel/touch: Change I2C speed type to i2c_speed enum
    * 84a348f4bf ec/starlabs/merlin: Remove the fast charge option
    * 1699d455e7 vc/intel/fsp: Update PTL FSP headers to FSP 3373_03
    * b87a9795de tree: Use boolean for s3resume
    * ec1068883f mb/google/nissa/var/guren: Add initial WWAN related settings
    * 752d49a4ff mb/google/fatcat/var/moonstone: Disable RT721 clock stop support
    * 155041ad4c soc/qualcomm/x1p42100: Add EUSB2 HS repeater support for USB Type-C
    * 6e45016610 intel soc,southbridge: Add Kconfig to set TSBS in IFD during build
    * f4271cad0a ifdtool: Add set top swap size PCH strap subcommand
    * ab4b82fb3c util/lint: Add a license check exception for .gitkeep files
    * 03524780ff soc/qualcomm/x1p42100: Support loading QTEE FW config files
    * 50adb3f23c mb/google/bluey: Increase FW_MAIN_A/B slot size to 4.5MB
    * bbdf2eab6a soc/mediatek: Rename DSI common files for improved readability
    * 8a2c04e04d mb/starlabs/*/rpl: Re-enable GpioOverride
    * 9ff9f2904b mb/google/bluey/var/quartz: Enable all spi flash drivers
    * f6743fba29 mb/google/fatcat/var/moonstone: Enable Intel DPTF support
    * fc736de10e ec/starlabs/merlin: Remove the EC_STARLABS_NEED_ITE_BIN option
    * 3dee4cd0c0 soc/intel/pantherlake: Correct Touch Controller Speed Configuration
    * 7376761bdf mb/nissa/var/pujjoquince: Modify fingerprint configuration
    * 6ffbc9a929 soc/mediatek: Move mtk_dsi_reset() to mtk_dsi_common.c for reuse
    * 668ea97075 commonlib/endian: Silence GCC -Warray-bounds false positives
    * 4a3cc37cbd crossgcc: Upgrade binutils from version 2.44 to 2.45
    * 35d4b3f2f4 arch/arm64: Support to load QTEE firmware in x1P42100
    * e38056bef8 amdfwtool: Move ISH before PSP L2
    * c1c83df3b5 mb/emulation/qemu-riscv: Enable ACPI by default
    * 5daf497df4 arch/riscv: Add ACPI support for riscv
    * 5c85793d26 mb/google/fatcat/var/lapis: Add cs42l43 and cs35l56 Soundwire links
    * 14fc6c3469 crossgcc: Drop nds32le-elf toolchain from default builds
    * fce489e9e5 drivers/intel/touch: Check SoC I2C speed function exists before calling
    * 249883d5bf mb/starlabs/starlite_adl: Squash SB and non-SB board variants
    * 80861a9f69 mb/starlabs/starlite_adl: Add CFR option for USB card reader
    * 5c1a9fa809 mb/google/fatcat: Create ruby variant
    * 43df7b14ae mb/var/uldrenite: Fix ISH UART port and VR configuration mismatch
    * 1a0b7195f9 mb/google/nissa/var/glassway: Removed the flag of DB_1A for pmc_mux
    * 9f0f373ff9 mainboard/amd/crater: Select the option to keep the AMD ACP active in S3
    * f04c45acee mb/google/fatcat/var/lapis: enable CS35L56_FAMILY and CS42L43 driver options
    * 78e7dcb152 drivers/soundwire/cs42l43: Add optional properties for controlling jack and accessory detect
    * f1c973bbff drivers/soundwire/cs42l43: Support Cirrus Logic CS42L43 codec
    * 35970abcdf mb/google/nissa/var/dirks: Add H58G56CK8BX146 to RAM ID table
    * 402ac7cd81 crossgcc: Upgrade acpica from 20250404 to 20250807
    * 3aa312e4c9 soc/mediatek/mt8189: Add DSI path support and update mutex
    * 6b93516e02 soc/intel/baytrail/acpi: Add missing MMIO window below 4GB
    * 321d8c5b21 soc/intel/braswell/acpi: Add missing MMIO window below 4GB
    * 74d7a21382 nb/intel/haswell/acpi: Add missing MMIO window below 4GB
    * 07f25bef86 mb/google/ocelot/var/ocelot: fix gpio settings
    * 10b0697dc3 soc/intel/pantherlake: Update power limits and voltage regulator parameters
    * 163e6a502c mb/starlabs/common: Deduplicate Pin Mix
    * 7622a57771 mb/starlabs/common: Move the SMBIOS code to common directory
    * e7dd184e5f Makefile.mk: Add support for mainboard vendor common code
    * dd3d7dcdfa mb/purism/librem_skl: Add CFR option menu support
    * 0d50fdf035 mb/purism/librem_l1um_v2: Add CFR option menu support
    * e9a01dea32 mb/purism/librem_jsl: Add CFR option menu support
    * 08efea5141 mb/purism/librem_cnl: Add CFR option menu support
    * 1d02f139d3 mb/google/fatcat: Add wake configuration to cnvi_bluetooth
    * cf97e1bc25 mb/intel/ptlrvp: Add power meter acpi changes
    * 7e3883633a mb/google/brox/var/jubilant: Apply fw_config to enable/disable I2C1
    * 114d24cd7a lib: Generalize BMP_LOGO help text
    * e468e32dfb mb/google/*: Update Kconfig.name with actual device names
    * 85d7a1c85f drivers/ipmi: add Block Transfer (BT) interface
    * fe26234cf2 mb/google/trulo/var/uldrenite: Update DPTF parameters
    * 660f71e704 mb/google/trulo/var/uldrenite: Set GPP_E16 to NC for non-WWAN SKU
    * 3747b47df1 mb/lenovo/sklkbl_thinkpad: Add Lenovo Thinkpad T580 as a variant
    * d23eaa356f util/lint: maintainers-syntax: Add a check to ensure paths exist
    * 079c9c47aa soc/amd/cezanne: Add config option to keep ACP running in ACPI S3 state
    * f665e189da mb/starlabs/{starbook/mtl,byte_adl}: Select USB4_PCIE_RESOURCES
    * a7a49e5f74 mb/starlabs/starfighter: Correct reference for second TBT port
    * f22bcc1d42 mb/starlabs/starbook/rpl: Disconnect unused GPIOs
    * a8c70f7578 mb/starlabs/starbook/rpl: Reconfigure TBT GPIOs
    * 83aa4417cb mb/starlabs/starbook/rpl: Tidy up GPIO config straps
    * 7ad632cbc7 mb/starlabs/starbook/adl: Disconnect unused GPIOs
    * d7627a39e8 mb/starlabs/starbook/adl: Tidy up GPIO config straps
    * 711d49d4ec mb/starlabs/starbook/adl: Configure additional SSD GPIOs
    * 90d87c5941 mb/starlabs/starbook/*: Remove comments for unused GPIOs
    * f6b5e26fe7 soc/mediatek/mt8196: Add 24MB framebuffer region
    * 815f3f7df2 mb/google/rauru: Increase RW firmware sections size to 1756KB
    * 193420fe0b soc/mediatek/common: Add bootsplash support
    * bdd4561536 soc/mediatek/mt8196: Add mtk_ddp_ovlsys_start for rendering framebuffer
    * 0ff213d711 soc/mediatek/common: Conditionally set up framebuffer
    * 5271ac7ac5 soc/qualcomm/x1p42100: Reserve DDR carveout region
    * 19feafc018 drivers/intel/fsp2_0/ppi/mp_service_ppi: Support CPU_V2_EXTENDED_TOPOLOGY
    * 16feb1bb28 mb/google/brya/var/nissa: Add missing device type to gfx device
    * f4ecb69314 util/inteltool: Add Twin Lake UHD Graphics PCI IDs
    * ed736a47d8 mb/starlabs/byte_adl: Configure additional SSD GPIOs
    * 38525716d8 mb/starlabs/starbook/adl: Re-order the config strap GPIOs
    * 2c465c0e21 mb/starlabs/starbook/adl: Re-order GPIOs to match other boards
    * 115a6ce36a mb/starlabs/starbook/adl: Correct clock request number in comment
    * 1b5aaaefd9 soc/intel/meteorlake: Fix IGD IRQ
    * 06de11693f mb/starlabs/starfighter: Fix Thunderbolt disabling code
    * 5e36d9ba04 mb/starlabs/starbook/mtl: Update the VBT from 256 to 261
    * fba8c14c27 mb/google/brya: add cnvi BT recovery mechanism
    * 1fb4a7409b soc/intel/pantherlake: Add VR power state current thresholds
    * 59ede353c5 soc/intel/pantherlake: Add Thermal Design Current (TDC) configuration
    * c54658d200 soc/intel/pantherlake: Add ICC Max configuration support
    * 0d1545ffac soc/intel/pantherlake: Add hysteresis window UPDs support
    * 8f24546fc4 vc/intel/fsp/fsp2_0/wildcatlake: Expose Thermal current thresholds and mode
    * 04affc3354 mb/google/ocelot: Update gpio's for ALC721 sndw
    * 3ac1a2b124 MAINTAINERS: Drop non-existant TPM files from VBOOT
    * af8b15ae04 Revert "libpayload: Define UCHAR_MAX/CHAR_MIN/CHAR_MAX"
    * 5e64ae2554 mb/starlabs/starbook/mtl: Enable PCH Energy
    * 375847acfe soc/intel/meteorlake: Configure PmcPchLpmS0ixSubStateEnableMask
    * db0faffdb8 mb/starlabs/*: Add comment about not configuring eSPI GPIOs
    * 990ad929a0 mb/starlabs/starbook/tgl: Don't configure eSPI GPIOs
    * 7ebcd6763f soc/qualcomm/x1p42100: Handle Type-C polarity for USB4/DP PHY init
    * f1708cf21a drivers/intel/touch: Enhance Intel touch driver for new devices
    * 55bf4ea07e cpu/x86/topology: Add tile and die ID CPU topology fields
    * 0c97aed8ac mb/google/fatcat/var/lapis: Modify touchpad and touchpanel configuration
    * 9e4a0a6026 mb/starlabs/starbook/mtl: Don't configure eSPI GPIOs
    * 3e0457e087 security/vboot/Makefile.mk: Fix building vboot lib with OpenSIL
    * 60ef877d93 mb/google/skywalker: Modify the RST pin naming
    * d5a8cec748 soc/intel/meteorlake: Rely on FSP_DIMM_INFO
    * e168a516e4 soc/intel/pantherlake: Rely on FSP_DIMM_INFO
    * 23419df34c drivers/intel/fsp2_0: Implement API to retrieve DIMM info
    * 1f328351e6 mb/starlabs/*: Select SPD_READ_BY_WORD
    * 88439b4cd3 mb/starlabs/starbook/mtl: Set the VPU default to disabled
    * 8ffa58723a soc/qualcomm/x1p42100: Add USB Type-C support
    * 45cedbb992 soc/qualcomm/x1p42100: Add HS/SS PHY support for USB Type-C ports
    * b18dfde22a soc/qualcomm/x1p42100: Add Clock support for USB Type-C ports
    * c7e4ef822d mb/starlabs/{starbook,starfighter}: Remove DRIVER_TPM_SPI_CHIP
    * 0c73e45493 ec/starlabs/merlin: Add disabled option for lid switch
    * ac7bb7694d mb/starlabs/starbook/mtl: Configure eSPI GPIO Mux
    * b37821ac25 mb/starlabs/*: Unify settings across device VBTs
    * ac8765c88a mb/starlabs/*: Correct USB Type-C Port Configuration
    * f7512c8647 mb/starlabs/starbook/{adl,rpl}: Remove USB OverCurrent Configuration
    * bf67771656 mb/google/fatcat/var/lapis: Update gpio GPP_E07 configuration
    * ff5daa0581 MAINTAINERS: Remove '/' from the beginning of paths
    * 6bfa257eef MAINTAINERS: Correct the path of cbmem_id.h
    * d7ae81132b MAINTAINERS: Correct asus/p8z77-series to asus/p8x7x-series
    * ef8eb79636 MAINTAINERS: Rename util/ipqheader to util/qualcomm
    * 0965bb9f68 MAINTAINERS: Remove non-existant mainboards
    * f5d1505c6b mb/google/fatcat/var/moonstone: Add Elan touchpad support
    * 24bfeb154e mb/google/fatcat/var/moonstone: Add focaltech touchscreen support
    * 1580346fa7 mb/google/fatcat/var/moonstone: correct the Kconfig settting
    * 150647a2fb ec/google/chromeec: Fix ACPI _CRS method generation for LPC memory range
    * ffa262db59 Documentation/FIT: reference archived copy of Intel TXT lab handout
    * f47e6c3905 MAINTAINERS: Fix typo "copperlake_sp" to "cooperlake_sp"
    * 1af0497c12 mb/google/dedede: Fix MAINBOARD_FAMILY conditional
    * b4b6c3aa55 mb/google/brya/var/{marasov,mithrax,omnigul}: Add SOF chip driver entries
    * 738fd2efc9 util/chromeos/extract_blobs: Add support for command line params
    * e59c5abd13 ec/google/chromeec: Add EC_GOOGLE_CHROMEEC_FW_CONFIG_FROM_UFSC
    * 341b108a71 mb/starlabs/starfighter: Add missing GPP_A5 definition
    * 414f1a61dd vc/intel/fsp/fsp2_0/pantherlake: Expose Thermal current thresholds and mode
    * 2e92833172 soc/qualcomm/common/usb/qmpv4_usb_phy: Fix delay value in comment to 10 ms
    * b48532c694 soc/mediatek: Refactor MMU configuration for DMA region
    * 28a8eaa57b soc/mediatek/mt8192: Clean up memlayout.ld
    * bca876849a soc/mediatek/common: Add enable parameter for configure_backlight
    * 46ce812c1b mb/google/skywalker: Create variant Grogu
    * 98a5445328 MAINTAINERS: Correct paths for Dell Latitude mainboards
    * 984ee53de8 mb/asus/p8x7x-series: Introduce CFR setup menu
    * 830ec89bca mb/google/bluey: Update mainboard part number for QuenbiH
    * c2fcf69e41 arch/x86: Use boolean for flag_is_changeable_p()
    * 2a791fcd66 mb/imb-1222/hda: Use AZALIA_PIN_CFG_NC() for disabled SPDIF_OUT2 pin
    * 217a7962d0 ec/google/chromeec: Update EC headers
    * 59cbb073c2 util/chromeos/crosfirmware.sh: Fix download of ninja (baytrail) recovery
    * fba92daed3 soc/qualcomm/x1p42100: Clean up DDR and IMEM memory layout
    * 5609174786 mb/google/rauru: Create variant Sapphire
    * 386feb720e soc/mediatek/mt8196: Add DVFS support for the second SoC SKU
    * 4b93b36170 mb/purism: add missing terminators to azalia codec tables
    * a927d124be mb/asus: Replace verb tables with reworked implementation
    * 9c0c925fe6 mb/siemens/mc_rpl1: Send POST codes to NC FPGA via PCI
    * 10361583b3 mb/siemens/mc_rpl: Add code to wait for legacy devices before PCI scan
    * d9979ba6a3 mb/siemens/mc_rpl: Sort includes alphabetically
    * d9b609b139 nb/intel/haswell: Use boolean for cbmem_was_initted
    * 1f2408f573 console: Fix flushing for slow consoles
    * 6a016a784b Documentation: Finalize 25.09 release notes
    * a0c5669c1b mb/asrock/imb-1222: Use macros for HDA verb table
    * c94ca87d40 mb/google/fatcat/var/kinmen: Enable Intel DPTF support
    * fe5f8494f6 docs/releases: Remove outdated "Upcoming release" in titles
    * aef86a7e89 mb/google/ocelot/var/ocelot: disable HDA GPIOs by default
    * 21f6ccf3a4 soc/intel/pantherlake: Use CPU ID mask for all stepping
    * 8bc41fc937 mb/google/trulo/var/pujjocento: Update DTT settings for thermal control
    * d5f1ecedf7 {device/azalia_codec,mainboard}: Use node ID enums for Realtek ALC887
    * 02059c2250 mb/google/trulo/var/pujjoquince: Disable ISH gpio setting by fw_config
    * 58459b8210 mb/var/uldrenite,orisa: Include the variant GPIO header
    * 88ad238eca mb/google/skywalker: Fix incorrect GPIO_USB3_HUB_RST_L pull-down
    * b8a8800152 mb/google/fatcat/var/lapis: Configure gpio of fingerprint sensor
    * 4431848ee6 acpi: Move most of HEST ACPI table to common code
    * ce1ced7f6a mb/intel/ptlrvp: Fix WIFI driver device settings under root port 4
    * 691a23a272 mb/google/fatcat: Fix WIFI driver device settings under root port 4
    * 16bf80b9b1 tests/imd: Fix invalid NULL comparison on uintptr_t
    * 6d816b3b43 mb/asus/h610i-plus-d4: Add missing AZALIA_USE_LEGACY_VERB_TABLE
    * 6f042c6ae4 lib: coreboot_tables: Fix grammar of *These information* in comment
    * bcc7fce590 mb/topton/adl: Add TWL variant (X2E_N150)
    * f634121fa4 mb/purism: Replace verb tables with reworked implementation
    * 20d4042458 mb/asrock: Replace verb tables with reworked implementation
    * 2b7dbf80c9 mb/apple: Replace verb tables with reworked implementation
    * 970249694f mb/amd: Replace verb tables with reworked implementation
    * 94beaa7ab3 mb/acer: Replace verb tables with reworked implementation
    * f3db3a19d5 mb/51nb: Replace verb tables with reworked implementation
    * bc92d9a666 nb/intel/haswell/minihd.c: Add reworked verb table implementation
    * 69781b9806 soc/intel/broadwell/minihd.c: Add reworked verb table implementation
    * 31fc5b06a6 device: Introduce reworked azalia verb table
    * 50a59d4464 device: Add Kconfig to prepare for reworked verb table implementation
    * a3e2073591 lib/vga_gfx: Add API to render text on a bitmap buffer
    * ec2875e38f mb/google/ocelot/var/ojal: Enable FPS and update FW config
    * beb0951c1c mb/google/ocelot/var/ojal: Update touchpad config
    * bd933b641e mb/google/ocelot/var/ojal: Add overridetree
    * 82a9e601bd mb/google/ocelot/var/ojal: Add initial GPIOs config
    * 622c504a71 mb/emulation/qemu-riscv: Select DRIVERS_EMULATION_QEMU_FW_CFG
    * ba3f529681 drivers/emulation/qemu: Adjust fw_cfg driver for Arm and RISCV
    * c1e0384367 arch/riscv/include: Cast 'id' to int in OTHER_HLS()
    * 67a3fb6abe mb/asus: Add PRIME H610i-PLUS D4 (Alderlake/LGA1700)
    * 4f13f72dbc libpayload: Define UCHAR_MAX/CHAR_MIN/CHAR_MAX
    * 7f8c442a09 soc/intel/meteorlake: Correct function naming
    * 6d265ca31d device/pci_device: Fix typo in comments
    * d4b6b55977 payloads/Kconfig: default to Skiboot payload on PPC64
    * cb899b0c4d mb/google/brox/var/caboc: Update HDA verb table
    * 15b903e1fd soc/intel/pantherlake: Add DDR5 memory type debug message
    * a5252bd5b9 drivers/soundwire/cs35l56: Support Cirrus Logic CS35L56 Smart Amplifier Family
    * 7d44128b2f mb/asus/p8z77-m_pro: Enable serial port A instead
    * 894c8069fc superio/nuvoton: Add NCT5535D
    * 69b0541375 sio/nuvoton/nct6779d: Add power loss resume support
    * f61ffb68c9 soc/intel/pantherlake: Remove unused TxDqDqs retraining parameter
    * 3a84c93b5b soc/intel/pantherlake: Correct function naming and code style
    * 3b34079b19 mb/google/trulo/var/kaladin: Enable External bypass config2 settings
    * 61488ffd57 mb/google/skywalker: Add CS35L51 support for beep sound for Padme
    * 4a7d779ed0 soc/mediatek/mt8196: Set RTC EOSC calibration to 8 seconds
    * 07df08836e Docs/releases: Update release notes for 25.09 release
    * 8f52c0774e docs/security/vboot: Update supported board list
    * 1f08b36f84 Documentation: Add coreboot release 25.12 template
    * 02980f0ea6 soc/amd/common/block/psp: Add comments
    * a17a41559a soc/amd/common/block/psp: Add BIOS SPI flash semaphore
    * 038262155e soc/amd/common/block/psp/psp_smi_flash: Fix flash busy check
    * 67e3579d61 sb/intel/lynxpoint: Enable PCIe Relaxed Order
    * 865649edc0 util/docker/jenkins-node: Use the correct branch for encapsulate
    * 6af7d299b2 mb/google/skywalker: Add MIPI panel support with TM_TL121BVMS07_00C
    * 4fc5f7a843 mb/google/fatcat/var/lapis: Modify the gpio order of mem_id
    * 2764a508ad mb/google/fatcat/var/lapis: Add 4 DDR modules to RAM id table
    * 886bd1d186 spd/lp5: Add Samsung K3KLALA0EM-MGCU memory part
    * 0a6f3e3868 mb/google/brox/var/caboc: Add PDC FW hash to hint romstage init
    * cbd1529126 mb/google/brox: Update Auxiliary Firmware Version check
    * 0d4c0ee7fc ec/google/chromeec: Add API for AP shutdown command
    * bbd72abae5 ec/google/chromeec: Update EC headers
    * 21ca3c5f3d mb/intel/ptlrvp: Update CKD/QCK mapping parameters
    * 3d7b898ff4 mb/google/ocelot/var/ocelot: Disable ALC721 clock stop support
    * c822148f2b mb/google/fatcat/var/lapis: Modify dq/dqs setting
    * 78fb910fe2 mb/google/fatcat/var/lapis: Update the configuration of fw_config
    * eb3497fae4 mb/google/fatcat/var/lapis: Update tpm i2c configuration
    * 3a33217349 mb/google/fatcat/var/lapis: Update thermal strategy
    * 36d2dc7cb9 mb/google/ocelot: Update wake event mapping for gspi0
    * 59bd0e3206 mb/google/ocelot/var/ocelot: Update USB and TCSS port configuration
    * c4627e0dda mb/google/ocelot: Remove FP_PRESENT probe from ISH device configuration
    * 8e9ec16f45 mb/google/trulo/var/pujjolo: Add tablet mode fw config for ish fw
    * e3a2d1cecf soc/qualcomm/qclib: Improve logging on invalid MRC cache data
    * 289c01e6fb mb/google/ocelot: implement variant_memory_sku()
    * fbb68982c9 mainboard/google/ocelot: Update PCIe root port for SD card interface
    * c98155cbcd soc/intel/pantherlake: Generate TME keys only if TME is enabled
    * d8ed977358 mb/google/skywalker: Remove space before tabs in gpio.h
    * 1e7908fa9f mb/google/skywalker: Set up all output GPIOs
    * 14e6c62c10 mb/google/skywalker: Define all GPIO pins
    * 2859a5cba5 mb/{google,intel}/{fatcat,ptlrvp}: Prevent access to disconnected camera
    * ffae0f7d73 security/vboot: Extend CROS_EC_HASH_TIMEOUT_MS
    * d2345e0c60 mb/google/fatcat: Set `SkipExtGfxScan` FSP-M UPD
    * 8953c772cf lib: Fix bad whitespace in add_bmp_logo_file_to_cbfs_call
    * ef0c650edf soc/intel/cmn/blk/fast_spi: Cancel DMA transfer before locking
    * b3ad2aa3e7 mb/google/ocelot: GPIO config for headphone jack detection
    * 508c399bc1 mb/goog/ocelot/var/ocelot: add H58G66CK8BX147 memory option
    * d8a3f2aedd mb/goog/ocelot/var/ocelot: add H58GE6AK8BX104 memory option
    * f4110cebf6 spd/lp5: Add SPD for H58GE6AK8BX104
    * 7acc99c3d2 acpi/acpi_pm: Fix compilation without SMBIOS
    * c77d3d67cf mb/google/skywalker: Report panel ID and SKU ID for padme
    * 6185983028 soc/intel/pantherlake: Standardize macros for core count and SKUs
    * 9a8402adf9 mb/google/trulo/var/kaladin: Update HDA verb table
    * 9af9e1d1f4 mb/google/trulo/var/kaladin: Add eMMC DLL settings
    * 3b4c446fbb mb/google/bluey: Configure QUP0 SE1 as I2C
    * ddf5987c1e drivers/mipi: Add support for TM_TL121BVMS07_00C panel
    * 0fec287327 mb/google/nissa/var/dirks: Drive GPIO GPP_D2 high to fix noise issue
    * 5a9ca2b040 mb/starlabs/starbook/mtl: Set SPD size to 512
    * 79119456a2 soc/amd/common/block/iommu: Add missing newline to debug print
    * 81bb2663b7 soc/qualcomm/x1p42100: Select HAVE_CBFS_FILE_OPTION_BACKEND
    * bf83dd9927 soc/qualcomm/common/qclib: Introduce runtime debug log level control
    * cf3af46e50 mb/google/skywalker: Create variant Padme
    * 2b1809e026 mb/google/fatcat: Increase Fast VMode I_TRIP threshold to 63A
    * 2a7a0e86cd mb/google/fatcat: Configure Acoustic noise mitigation
    * 6c06602c75 mb/google/brya/var/uldrenite: Add fw_config probe for touchpad
    * a3b73464b5 soc/qualcomm/x1p42100/usb: Fix code comments and debug messages
    * e924021e69 mb/google/trulo/var/kaladin: Add GTH1563 and GTH7503
    * d1967d927a spd/lp5: Add SPD for MT62F1G32D2DS-031 WT:C and MT62F2G32D4DS-031 WT:C
    * 2e10ddb1ee mb/starlabs/starbook/mtl: Make TCSS notify the IGD of changes
    * 47fb46e0e4 vc/intel/fsp/mtl: Update the headers to 5124_47 (13.0.228.64)
    * bb760bc9f3 Kconfig: Introduce HAVE_CBFS_FILE_OPTION_BACKEND
    * f1b83c8759 mb/google/rex/var/kanix: Add K3KL8L80EM-MGCU to RAM ID table
    * bcb3263078 mb/goog/ocelot/var/ocelot: add H58G66BK8BX067 memory option
    * 751afeb060 mb/google/brox/var/caboc: Update HDA verb table
    * 56700713de mb/google/trulo/var/kaladin: Disable eMMC GPIOs via firmware config
    * 93c147c5e6 commonlib/device_tree: Add dt_add_iommu_addr_prop function
    * d3d2f0f1c8 mb/google/fatcat/var/moonstone: Add to support ALC1320 Smart Amp
    * 1da045f6a5 mb/google/skywalker: Add API support for regulator VCN18
    * fe70426dd7 soc/mediatek/common: Add support for regulator VCN18
    * f4a123f055 tests: Allow specifying using system Cmocka or building from source
    * e7d598ba2c Reland "tests: Allow specifying vboot source directory"
    * a348ef46db mb/google/trulo/var/pujjolo: Change setting for lite ISH fw
    * 16db59ccef mb/google/rex/var/karis: Add K3KL8L80EM-MGCU to RAM ID table
    * 3639648f81 mb/google/fatcat/var/felino: Set GPP_A15 and GPP_B23 as not used
    * 8585591596 mb/google/fatcat/var/lapis: Set GPP_A15 as not used
    * b9af91dfe1 mb/starlabs/starlite_adl: Drop HDMI entries from verb table
    * 461c6a7d31 mb/starlabs/starfighter/rpl: Drop HDMI entries from verb table
    * fc3a647579 mb/starlabs/starbook/rpl: Drop HDMI entries from verb table
    * a88d9e1033 mb/starlabs/starbook/mtl: Drop HDMI entries from verb table
    * 90f94287fd mb/starlabs/starbook/adl_n: Drop HDMI entries from verb table
    * 684530ebdc mb/starlabs/starbook/adl: Drop HDMI entries from verb table
    * 258da6b1ef mb/goog/ocelot/var/ocelot: add H58G66BK7BX067 memory option
    * 883103c77f mb/google/ocelot: Disable memory training progress bar
    * f3a49c8b3d mb/google/ocelot/var/ocelot: Disable audio for invalid Audio FW_CONFIG
    * be3148575e mainboard/google/ocelot: Set OEM footer logo bottom margin
    * 092fca3210 mb/google/fatcat/var/kinmen: Add support ALC1320 Smart Amp
    * 4ba1b615db mb/starlabs/starlite_adl: Use macros for HDA verb table
    * ca8d6a7512 mb/starlabs/starfighter/rpl: Use macros for HDA verb table
    * c30163dace mb/starlabs/starbook/tgl: Use macros for HDA verb table
    * 15111ebb21 mb/starlabs/starbook/rpl: Use macros for HDA verb table
    * 6d6a280ab2 mb/starlabs/starbook/mtl: Use macros for HDA verb table
    * 543f6c2a52 mb/starlabs/starbook/kbl: Use macros for HDA verb table
    * 6d7c8f5477 mb/starlabs/starbook/cml: Use macros for HDA verb table
    * 515f566840 mb/starlabs/starbook/adl_n: Use macros for HDA verb table
    * 4b61d4de5f mb/starlabs/starbook/adl: Use macros for HDA verb table
    * 8bc0eddf15 soc/intel/pantherlake: Add support for a new Panther Lake B0 SKU
    * 2b84d26f55 payloads/edk2: configure capsule updates
    * f3211e9639 soc/intel/pantherlake: Add support for Acoustic Noise Mitigation UPDs
    * 2c03fd06a9 mb/google/trulo/var/kaladin: Disable ISH via firmware config
    * f8574f7145 soc/intel/ptl: Add Wildcat Lake SKU power map
    * b1fe32dd9e mb/{intel,google}/{fatcat,ptlrvp}: Update GPP_A15 GPIO configuration
    * 6074ca18d3 mb/google/ocelot: Create matsu variant
    * 76e0f64035 mb/google/brya: Update GPIO_PCH_WP for trulo variants
    * b69e66721d mb/google/brya: Update GPIO_PCH_WP configuration in trulo baseboard
    * 17c623277b mb/google/trulo/var/pujjolo: Change stylus settings
    * 7f74155aa4 mb/google/trulo/var/uldrenite: Select USE_UNIFIED_AP_FIRMWARE_FOR_UFS_AND_NON_UFS
    * f373faa9c8 mb/google/trulo/var/uldrenite: Add fw_config probe for storage
    * a262cdbc27 mb/intel/ptlrvp: Add wake configuration to cnvi_bluetooth
    * 1c0186f280 soc/intel/common/block/cnvi: Add CNVi chip configuration support
    * bdbe8b9b6f util/kconfig: Fix xconfig
    * 241b940ac7 mb/google/nissa/var/rull: add RAM ID H58G56CK8BX146
    * 599d660c4b mb/google/fatcat: Enable support for Realtek EC
    * 89a3ae3d80 mb/google/trulo/var/pujjolo: Update GPP_D15 setting
    * b849c9daa1 3rdparty/qc_blobs: Update submodule to upstream main
    * 8159b2e06c device/azalia_codec: Add header with enums for Realtek node IDs
    * 18ae0c48e1 mb/google/fatcat/var/moonstone: Support new schematic changes
    * c1f76dd87e mb/google/brya/var/dochi: Add H58G56CK8BX146 to RAM ID table
    * 164b4a1d90 mb/google/nissa/var/craask: Add parade touchscreen support
    * 492826771e mb/google/bluey: Enable USB support
    * 96eb6a3ac1 soc/qualcomm/x1p42100: Add USB Type-A Host support
    * 2908a955e5 mb/google/rex/var/kanix: Add H58G56CK8BX146 to RAM ID table
    * 7fc414c886 mb/google/trulo/var/kaladin: Enable EC keyboard backlight
    * 859cc31e3a mb/google/brox/jubilant: Generate RAM IDs
    * a6c15129a7 mb/google/fatcat/var/moonstone: Generate SPD ID for memory module
    * 1028f3e846 soc/intel/pantherlake: Add Bluetooth to PME wake source mapping
    * 3f926bc110 commonlib/bsd: Add Bluetooth wake source in ELOG event data
    * 84ec1493a3 drivers/wifi/generic: Fix typo in header guard comment
    * 2e2490256f soc/qualcomm/x1p42100: Add USB clock support for X1P42100
    * 159afbc5d5 lib/lzmadecode: Increase decoding speed by 30%
    * 0b8ad35ac1 mb/starlabs/byte_adl: Adjust the VBT
    * d3cea61907 mb/starlabs/starlite_adl: Adjust the VBT
    * 3507992d1d mb/starlabs/starbook/adl_n: Adjust the VBT
    * 05cd5a7ab9 mb/google/nissa/var/telith: Generate RAM IDs for telith
    * a0bdf3961c soc/qualcomm/common: Add clock reset function support
    * cf11722e68 soc/mediatek/mt8189: Enable tracker debug hardware
    * 382a7caff3 soc/mediatek/mt8196: Refactor tracker driver to support new platform
    * 97f9ebb5c2 mb/google/ocelot: Create ojal variant
    * ef1d48ee1d util/lint: Don't check for Kconfig.name in common directory
    * 5cb36eb16c util/lint: Don't check for board_info.txt in common directory
    * 46b03e682c util/amdfwtool: Handle address mode properly for Turin
    * 97cf4a1919 util/amdfwtool/amdfwread: fix offset decision for PSP/BIOS directory lookup
    * 73dd7bb046 util/amdfwtool/amdfwread: add initial parsing for EFW structure
    * d4da533473 smbios.h: Update smbios_memory_type
    * 183589dcbd smbios.h: Update smbios_memory_form_factor
    * 58726e58e4 mb/starlabs/starbook/mtl: Adjust the VBT to fix hot plug
    * 80df8c336f mb/intel/ptlrvp: Update Kconfig for ptlrvp_chromeec4s and ptlrvp4es support
    * ed59c1de34 soc/qualcomm/x1p42100: Update TF-A memory reservation
    * 56dbafcff4 soc/intel/pantherlake: Remove UFS support
    * 5b46caef93 mainboard/intel/ptlrvp: Remove UFS support
    * 621633af9b mainboard/google/fatcat: Remove UFS support
    * 5e2f5050ba mb/starlabs/starbook/kbl: Update HDA verb table
    * 4626c053dd mb/starlabs/starbook/adl_n: Update HDA verb table
    * 6f11c31354 mb/starlabs/starbook/mtl: Update HDA verb table
    * b748a5e10b mb/starlabs/{starbook,starfighter}/rpl: Disable GPIO override
    * 29ca9c8bfa mb/google/bluey: Disable charging during normal boot
    * e82338b0a2 mb/google/bluey: Add boot mode to coreboot tables
    * 893a2b008a libpayload: Add coreboot boot mode table
    * a45c8441af lib: Add boot mode information to coreboot tables
    * c73f30e74b mb/google/nissa/var/riven: Add H58G56CK8BX146 to RAM ID table
    * 8c717df03a soc/intel/ptl: Update Wildcat Lake PCIe root port numbering
    * afaef0b904 mainboard/google/ocelot: Update GPIO configuration for SLP_S0_GATE
    * 97dbfd3098 cpu/intel/car/non-evict: Improve CAR setup
    * cd48dc7d69 mb/google/rex/var/karis: Add H58G66CK8BX147 to RAM ID table
    * ffbf40f6c0 ec/google/chromeec: Update EC headers
    * 517185eca2 mb/google/bluey: Configure touchpad power GPIO
    * baf159a1c8 mb/google/bluey: Configure GSC and EC for Quartz
    * f8685bb2ee soc/mediatek/mt8189: Enable lastbus debug hardware
    * 6e61ea65a8 mb/google/bluey: Add disable slow charging support
    * 45d1f9cce4 mb/google/bluey: Move charging functions to dedicated file
    * 9fb306f53c soc/qualcomm/x1p42100: Add SPMI driver to ramstage
    * ac5bb861d8 mb/google/brya/var/uldrenite: Update HDA verb table
    * f2d3051631 ec/lenovo/h8: Turn on PWR LED
    * d8de1c4974 ec/lenovo/h8: Disable POST codes
    * d5a92542aa mb/google/fatcat/var/fatcat: Disable ALC721 & ALC722 clock stop support
    * 3b2962929b lib/timestamp: Init TSC frequency early on x86
    * b0a63052b7 sb/intel/bd82x6x: Fix CPU replaced check
    * 9ecf04c2bc mb/google/nissa/var/quandiso: Generate RAM ID for MT62F512M32D1DS-023 WT:E
    * 16318a32ce spd/lp5x: Generate initial SPD for MT62F512M32D1DS-023 WT:E
    * 283c25beec mb/google/trulo/var/kaladin: Select Strauss keyboard to show G icon
    * 2709ae443b cpu/x86/entry16.S: Move reset vector to this file
    * 53810448fc cpu/x86/reset16.S: Remove handcoded reset vector
    * a1b7f5e1b8 mb/siemens/mc_rpl: Disable EIST to improve deterministic behavior
    * e6f8900c2d mb/siemens/mc_rpl: Disable S0ix power states
    * c71071397f soc/intel/common/fast_spi: Add static bus scanning
    * e73b4579c6 mb/siemens/mc_rpl: Disable DPTF
    * 77061d8427 mb/google/bluey: Add Quartz board (Qualcomm Hamoa)
    * ee1446a791 mainboard/emulation/qemu-q35: Do not compile memmap into SMM
    * a7b6590aca  mb/google/dedede/var/dexi: Add and use VBT
    * 70ce81c86f mb/google/dedede/var/dita: Add and use VBT
    * 87f5d4c54a tree: use boolean for PcieRpLtrEnable[]
    * 725ab7b066 soc/mediatek/common: Increase WAIT_AUX_READY_TIME_MS
    * f02e755364 config/builder/mitac: Hook up public FSP repo and microcode
    * fc62ffab48 soc/amd/common/fsp/dmi: Skip parsing when memory type UNKNOWN
    * c3071b7150 soc/amd/cezanne/fsp_m_params: add UPD pointer parameter to mb callback
    * eb9a673a8e soc/amd/cezanne: Add a Kconfig option for SERIRQ_CONTINUOUS_MODE
    * c590e8e75c mb/brya/var/uldrenite: Increase Touch IC enable delay time
    * 9996fc58fd mb/siemens/mc_rpl: Disable C1E state via MSR_POWER_CTL
    * c58c988b8e mb/siemens/mc_rpl: Remove unused code and power limit functionality
    * 8e5e87a1cf mb/siemens/mc_rpl1: Configure CPU power limits to 28W TDP
    * 4853f16a59 mb/google/fatcat/var/kinmen: Support new schematic changes
    * 9d67120078 mb/google/moonstone: Create moonstone variant
    * 00d954977c util/smmstoretool: Support other block sizes
    * 4fd3cb35c2 util/cbmem: Change abort() to exit(1) in die()
    * 62b6d1e336 mb/siemens/mc_rpl: Enable master bit in PCI config space if allowed
    * a8bce33b82 mb/siemens/mc_rpl: Disable Intel Turbo Boost
    * 1a9008b261 device/azalia: Use clrsetbits32() and friends
    * cbf8527345 device/azalia: Amend the mistake of codec_is_operative()
    * 0a328282ec device/azalia: Add enums for HDA verb and parameter IDs
    * c15006eb0c soc/intel/alderlake: Add 28W TDP support for RPL-P ID 8 (0xa716)
    * d7a996cf44 mb/siemens/mc_rpl1: Enable 4 P-Cores, disable E-Cores
    * 2f9273f1f4 mb/siemens/mc_rpl: Select FSP_TYPE_IOT
    * 1b14664311 mb/siemens/mc_rpl: Remove unused DPTF settings
    * 66a3f2a1b1 mb/siemens/mc_rpl: Disable SaGv
    * 993a9c9e14 mb/siemens/mc_rpl1: Configure SATA Ports
    * e03f50bf5f mb/siemens/mc_rpl: Enable Siemens NC_FPGA driver
    * 699c28c01d sb/intel/bd82x6x: Fix replay issues
    * c2110e3161 tree: Use true, false for PcieRpClkReqSupport
    * ebab858d92 soc/intel/pantherlake: Enable memory bandwidth compression for IGD
    * ad10d4a977 soc/intel/cmn/blk/graphics: Reserve memory compression region
    * 8a52418e9a commonlib/device_tree: Fix memory leak in fdt_unflatten()
    * 7896f4950c mb/google/skywalker: Turn off UFS power for eMMC SKUs
    * 22fe08c04b soc/mediatek/mt8189: Implement UFS power-off API for non-UFS SKUs
    * 5f0225a7b5 drivers/intel/fsp2_0: Refactor for earlier graphics memory WC MTRR
    * 1c571446ec soc/intel/common/block/systemagent: Increase MTRR region size to 32 MiB
    * 67afbf5f96 soc/intel/pantherlake: Add TDP mappings for Panther Lake-U SKUs
    * ec69479bdb mb/google/ocelot: Drop redundant SNDW GPIO mapping
    * 5f168e9441 mb/google/ocelot/var/ocelot: Conditionally init ALC256 HDA using fw_config
    * 152b584167 mb/goog/ocelot/var/ocelot: Add AUDIO_MAX98360_ALC5682I_I2S
    * 8f2633cd60 soc/power9/rom_media.c: find CBFS in PNOR
    * 44ec090551 ppc64: Kconfig switch for bootblock in SEEPROM, zero HRMOR
    * 921027e09b src/lib/cbmem_common: Delete a space(' ') in the source code
    * acb86babdf mb/protectli/vault_kbl/mainboard.c: bring back the beep
    * 76d45a8219 soc/amd/genoa_poc/root_complex.c: Explain the order of IOHCs
    * 8dcfa915f2 soc/amd/common/block/psp: Probe SPI flash early
    * 00217275b2 soc/amd/common/block: Don't clobber SPI registers
    * c13eadeadb soc/amd/common/block/psp/psp_smi_flash: Fix busy check
    * fbcf031181 mb/qemu-riscv: set PCI_IOBASE
    * bf0ee592f5 soc/intel/alderlake: Make SATA speed limit configurable
    * 482a2d6548 nb/intel/sandybridge/northbridge.c: Disable non-active PEG devices
    * 7e73d4ef30 Documentation: mb/erying/tgl: Update documentation
    * 73cba1fdea mb/erying/tgl: Introduce CFR
    * 23cf7c64f9 mb/erying/tgl: Use booleans in devicetree
    * 261b6b4fd1 soc/intel/skylake: Allow generating USB ACPI code
    * 23e92a5ac0 mb/erying/tgl: Map remaining USB ports
    * 762a535551 mb/erying/tgl: Clean up the GPIO table
    * 179b8444c3 soc/intel/xeon-sp/gnr: Hook up public FSP bin and headers
    * 42ba7a9e48 soc/intel/xeon_sp/gnr: Add Kconfig symbols for SKUs GNR-AP and GNR-SP
    * c732f406c7 mb/google/ocelot: ec.h: Disable sync IRQ, sync IRQ wake capable for OCELOT4ES
    * 73961bf680 mb/google/ocelot: Use same mainboard part number for all ocelot variants
    * 691d5e84cd mainboard/google/oceot: Drop redundant logo_valignment selection
    * b67d88aecb mb/google/bluey: Enable PMIC based slow charging in romstage
    * dcb7c317c2 mb/siemens/mc_rpl1: Enable Intel I210 MACPHY driver
    * 2b26ea0eda mb/siemens/mc_rpl1: Configure SPI and implement TPM support
    * 2bcd7f1522 mb/siemens/mc_rpl1: Adjust UART settings and enable LPSS UART
    * 524fd18bd6 mb/siemens/mc_rpl1: Create variant specific Kconfig file
    * c7cd4e3305 mb/siemens/mc_rpl: Move SOC selection to baseboard
    * 6427e51c4f mb/siemens/mc_rpl1: Adjust USB port settings in devicetree
    * 71c4619045 mb/siemens/mc_rpl: Remove unused devices from devicetree and Kconfig
    * 296f5968d3 mb/siemens/mc_rpl1: Adjust I2C bus enablement in devicetree
    * a1dd6bfc22 mb/siemens/mc_rpl1: Adjust PCIe settings in devicetree
    * f94469c2a9 mb/google/nissa/var/pujjolo/pujjoquince: Add wifi sar table
    * 6781f458ee mb/google/trulo/var/pujjolo: Enable fivr settings
    * 17a88540fd soc/qualcomm/x1p42100: Use SPMI driver
    * c1128ae649 soc/qualcomm/cmn: Add SPMI driver
    * 0eebd5596b mb/google/fatcat: Create lapis variant
    * 4931b978d9 soc/mediatek: Increase CBFS cache to 8MB in memlayout.ld
    * 234eb53ed9 nb/intel/sandybridge/raminit: Speed up reading SPD EEPROMs
    * 7d57333529 ec/starlabs/merlin: Add a "off" mode for the power LED
    * 36624072a6 mb/google/trulo/var/pujjolo: Update wlan rtd3 settings
    * 42a5c189b2 mb/lenovo/X220: Add CFR support
    * 8509798006 sb/intel/common/smbus: Use proper delay instruction
    * 5f7b5fcb19 mb/starlabs/byte: Lower the PL4 value to 65W
    * 4a6a0de029 3rdparty/fsp: Update to upstream master
    * de98da43fa 3rdparty/intel-microcode: Update to upstream main
    * bdee19ba87 soc/qualcomm/x1p42100: Add ASCII memory map diagram to memlayout.ld
    * 51a8e238b0 lib: Correct logo bottom margin handling for all panel orientations
    * 9999a4aebb mb/google/nissa/var/pujjocento: Change touchscreen properties
    * 8d2df573a8 soc/qualcomm/x1p42100/qclib: Support to pack and load CPR binary in CBFS
    * a484a6529c soc/qualcomm/common/qclib: Support to declare cpr_settings region
    * dc04ee827b mb/google/fatcat/var/kinmen: Generate SPD ID for memory modules
    * e7cdf035fb mb/google/brox/var/caboc: Enable RTD3 for SSD to resolve S0ix issue
    * cec34128d0 soc/qualcomm/x1p42100: Support to load CPUCP firmware in x1p42100
    * a2b6e20509 soc/mediatek/common: Increase per-channel SPMI max byte count to 2
    * 6ba2df9be5 soc/mediatek/common: Use polling to reduce eDP HPD wait time
    * ee347d8812 soc/qualcomm/common/qclib: Support to load AOP config and meta in CBFS
    * 3f4c84513d soc/qualcomm/x1p42100/qclib: Support to pack AOP config and meta in CBFS
    * 5de5b519ca mb/prodrive/atlas/vpd.c: Replace union {0} initializers with {} for C23 compliance
    * 48207895af lint: Warn about using change IDs for merged changes
    * 6acf07022d Doc/contributing: Add clarification on how to reference other commits
    * 40d0ec0fa4 Revert "soc/mediatek/common: Remove 200 ms delay from eDP init path"
    * 244a34b3d0 cpu/x86/mp_init: Refactor ICR wait logic
    * eee5be070a cpu/intel: Use mtrr_use_temp_range()
    * e37a53a2fc arch/x86/memcpy: Fix undefined behaviour
    * 7c0f7e0b3f vc/intel/fsp: Update PTL FSP headers to FSP 3272_04
    * d315f26217 payload/seabios: Update from 1.16.3 to 1.17.0
    * c61a762a47 mb/google/bluey: Add QuenbiH board
    * 9edf49b008 mb/google/bluey: Add BlueyH board
    * 9868417d5e mb/google/bluey: Refactor Kconfig for Hamoa SoC
    * 74d91d0b76 mb/google/nissa/var/glassway: Support Memory MICRON MT62F512M32D2DR-031WT:B
    * 7eb832b1dc mb/google/skywalker: Configure GPIO GPIO_AP_EDP_BKLTEN as output
    * cdd42ccde8 soc/qualcomm/x1p42100: Use 4K for memory region alignment
    * 2146ecc8e1 mb/google/brox/caboc: Enable PEG60 with PEG62
    * 6925fd69f8 soc/qualcomm: Move common region macros to `soc/memlayout.h`
    * d220b65b8f soc/qualcomm/qcs405: Add common include path
    * b25939786d soc/qualcomm/x1p42100: Refactor CBMEM top address to use linker symbols
    * d6ec4f108d soc/qualcomm/x1p42100: Mark additional reserved memory ranges
    * 1b760645b9 soc/qc/x1p42100: Dynamically configure DRAM resources in ramstage
    * 276432faf7 soc/qualcomm/common: Add MMU configuration for fragmented DRAM regions
    * b4347f11d9 include: Make DRAM an explicit region
    * 11c8d423d1 soc/qc/common: Remove ddr_base from qc_mmu_dram_config_post_dram_init
    * 73de3f95ac mb/google/bluey: Support hardware watchdog logging
    * 25e0a4642c mb/google/brox/var/caboc: Update LAN LED behavior
    * e5ff7cb186 mb/google/ocelot/var/ocelot: Update DDI port Configuration
    * 8df079c609 mb/lattepanda/mu: Enable CRB TPM (Intel fTPM)
    * 6e9c0a26e3 device/device_util: Fix format specifier for DEVICE_PATH_GICC_V3
    * 4a82f37525 mb/google/nissa/var/quandiso: Generate new RAM ID
    * 17a7c351b8 mb/google/brya/var/kaladin/hda_verb.c: Correct number of entries to 21
    * b65b98ace6 mb/goog/ocelot/var/ocelot: switch to H58G56BK8BX068 memory part
    * 8097809c8a libpayload: Fix strsep() edge cases
    * e38a216368 soc/intel/pantherlake: Rearm and clear only for valid crashlog in PMC
    * 510686add4 soc/intel/pantherlake: Rearm crashlog using watcher
    * 609eb4c5f1 mb/google/ocelot/var/ocelot:  Remove unused I2C controllers
    * df7bf9404d soc/mediatek/common: Remove 200 ms delay from eDP init path
    * a70bf82036 soc/mediatek/common: Measure eDP initialization time
    * 6bb1ba95e1 soc/mediatek/common/dp: Move mtk_edp_init to dptx_common.c
    * e49e8c6355 soc/qc/x1p42100: Add memory layouts for CPUCP and TZ regions
    * c418a3b843 mb/google/brox/var/caboc: Update WWAN gpio
    * 77b52ed3cc mb/google/brox/var/caboc: HDA: Correct number of jacks to 35
    * e31fbc493d soc/qualcomm/cmn/qclib: Support reuse of existing DDR training data
    * bdcf19f404 mb/google/trulo/var/pujjolo: Add fw config for PDC
    * 13897bde9a mb/google/trulo/var/pujjolo: Add wlan rtd3 setting
    * 90589d44d2 soc/qualcomm/x1p42100: Reserve DDR memory regions for AOP and BL31
    * 2e61995b2f soc/qualcomm/x1p42100: Add support for Hamoa SoC
    * 281b01ce5e soc/qualcomm/x1p42100: Remove unused PMIC file from CBFS
    * ecbca16bf4 tree: Replace union {0} initializers with {} for C23 compliance
    * b74d2b77d2 mb/google/trulo/var/kaladin: Add WIFI SAR table
    * 4b46a0690e mb/hp: Add HP ProDesk 600 G1 SFF Business PC (Haswell / NPCD379 SIO)
    * 2339508b6c mb/google/trulo/var/pujjolo: Update P-sensor parameters
    * cd2a969c82 soc/intel/pantherlake: Remove storage-off related code
    * fe6fa36504 mb/asrock: Add SPR 1S server board ASRock Rack SPC741D8-2L2T/BCM
    * b486c84b23 mb/google/trulo/var/pujjolo: Update DTT settings for thermal control
    * ece0072d1c mb/google/trulo/var/pujjolo: Update verb table to fix pop noise
    * 795157a606 mb/google/bluey: Increase MRC cache size
    * 34d9305dcc soc/qc/x1p42100: Pack QcLib DTB into CBFS
    * 8f09629fb1 spi_flash: Fix initialization of `flags` field in lb_spi_flash
    * ab2ef8878c mb/google/trulo/var/pujjocento: Update touchscreen information
    * 0bedce05d8 mb/google/nissa/var/pujjocento: Change touchpanel sequence to meet spec
    * 543fb60ec4 mb/google/brox/var/lotso: Set slew rate to 1/8
    * c114906239 mb/google/trulo/var/pujjocento: Update DTT settings for thermal control
    * b603f23088 mb/google/bluey: Avoid using function call table
    * dc64b9659d soc/qc/cmn: Refactor qclib_load_and_run function
    * e290bb6750 mb/baseboard/ptlrvp: Disable memory training progress bar
    * 05a38e2af3 mb/google/fatcat: Disable memory training progress bar
    * f789899dac sb/intel/common/gpio: Move register defines
    * d6ceaf72da mb/samsung/lumpy: Use gpio_base2_value
    * 21639c3771 mb/getac/p470: Use common gpio functions
    * 8d4bb94663 sb/intel/common/gpio: Add and use gpio_invert()
    * 85306062d8 mb/google/skywalker: Create variant Tarkin
    * 1da2f46db8 soc/intel/alderlake: Restore mem_init_override_channel_mask()
    * f0d5b25e02 mb/google/trulo/var/kaladin: Add firmware name and gpio for ISH
    * 0b3fc8ce2d mb/google/nissa/var/pujjoniru: Decrease cpu power limits
    * f01cc9258b mb/google/rex/var/screebo: Use ACPI for touchscreen power sequencing
    * ef11f95125 soc/qualcomm/x1p42100: Set 1ms TX delay
    * 2c8d157ea4 {drivers, soc/qualcomm/common}: Add configurable delay for UART bitbang
    * b0d2d522ea soc/qualcomm/x1p42100: Enable bootblock compression
    * 1e11bda5d0 soc/intel/cmn/smbus: Drop use of update_spd_len()
    * 910f111891 soc/intel/mtl: Fill in SPD data on both channels of DDR5 memory
    * 0da943ed99 soc/intel/meteorlake: Fix DDR5 channel mapping
    * 87c9bb3994 soc/intel/adl: Fill in SPD data on both channels of DDR5 memory
    * a23be7a6fe mb/google/fatcat/var/francka: Disable ALC721 & ALC722 clock stop support
    * 227d434e2d drivers/soundwire/alc711: Support clock stop flag from devicetree
    * 49219f1ce1 Docs: Use markdown autolinks instead of Sphinx doc directive
    * 6afc1ff9ac soc/mediatek/mt8189: Disable 8189G APU power to reduce power consumption
    * 965131e40f soc/mediatek/common: Fix build error by including stdint.h in cpu_id.h
    * e49743755d mb/google/ocelot: Select EC_GOOGLE_CHROMEEC_MEC for MCHP variants
    * 32b944b77a mb/google/brox/var/caboc: Update hda_verb table
    * ba228d160f mb/google/fatcat: Create new kinmen4es variant
    * 6a42eb9134 soc/intel/pantherlake: Disable memory training progress bar
    * e9cb352706 soc/common/smbus: Support reading SPD5 hubs for DDR5
    * cba46a41b7 mainboard/{hardkernel,protectli}: Drop use of DRAM_SUPPORT_DDR5
    * a79f341d29 mb/google/trulo/var/pujjolo: Disable mipi camera dmic LED
    * 9411c6e7c7 util/amdfwtool: Fix NULL pointer dereference in fill_dir_header
    * 280d3a25e8 util/lint/kconfig_lint: Fix operator precedence issue
    * fbc2d76ab3 soc/intel/*: Select 'DRAM_SUPPORT_DDRx' as appropriate
    * 008f0ec078 util/smmstoretool: Alias EfiImageSecurityDatabaseGuid to "secureboot"
    * 88aeb8b7cd util/smmstoretool: Allow setting authenticated variable
    * e977560e72 payloads/edk2: Increase non-full-screen menu size
    * ac7487d766 mb/google/fatcat: Use same MAINBOARD_PART_NUMBER for felino variants
    * 0f84878c89 mb/google/brox: Handle NULL return value in variant_get_auxfw_version_file
    * 749fd1a8d8 soc/intel/pantherlake: Use macro for VGA Init Control
    * 3c4fb7b729 mb/google/trulo/var/pujjolo: Update verb table
    * 2ae0f6cdb9 mb/google/trulo/var/kaladin: Add fw config for ELAN touchscreen
    * 53dd93ff14 libpayload/drivers/pci_qcom: Fix address during ATU config
    * 54016e273e util/cbmem/sysfs_drv: Fix incompatible pointer type for 'size'
    * a9997f2d7f soc/intel/cmn/block: Request bus master in final op for DSP and HDA
    * fea789ed63 mb/google/fatcat/var/francka: Use ACPI for touchscreen power sequencing
    * 211526ff38 Revert "mb/google/brya: Fix mux_conn index used by ec/google/chromeec"
    * 7b91339e55 Revert "mb/google/brya: Fix pmc_mux port mapping for mithrax and felwinter"
    * 8a45e505b9 soc/mediatek/common/dp: Change dptx_hal_phy_wait_aux_ldo_ready to static
    * 350c977fef soc/intel/pantherlake: Clear crashlog record using watcher
    * ae942a70b8 mb/google/trulo/var/kaladin: Update GPIOs table
    * 0a4bc79685 mb/google/trulo/var/kaladin: Update USB2 driving settings
    * f34bc61ca7 mb/google/trulo/var/pujjolo: Correct the Goodix touchpad description
    * d4b735f9f1 mb/google/ocelot: Turn off unused I2C ports
    * 190c27d08b mb/google/brya/var/marasov: Add SPD ID for K3KL6L60GM-MGCT and K3KL8L80EM-MGCU
    * d79febf356 soc/qc/x1p42100: Enable QcLib, SHRM and AOP firmware load
    * db10b681b4 soc/qc/x1p42100: Load and populate QcLib interface table entries
    * eee3ea0346 mb/google/bluey: Enable PCIE Feature for bluey
    * 6f115f7bf0 soc/qualcomm/x1p42100: Configure Gen4 PHY link for x1p42100
    * 823fa6b8f6 soc/qualcomm/common: Integrate QMP PCIe 4.0 PHY 2x2/1x4
    * 6bb199d258 mb/google/fatcat/var/fatcat: Move `ISH_GP_x` pads to fw_config.c
    * a5212f15ce mb/google/fatcat/var/fatcat: Remove unused GPP_B06 GPIO configuration
    * d7415f5d9a mb/google/trulo/var/kaladin: Remove external bypass settings
    * 479b39c3e9 mb/google/ocelot: Update wake on touch GPIO
    * 7095c99a87 util/cbmem: Add support for CBMEM in sysfs
    * eeb15e83cb mb/gigabyte: Add ga-h81m-d2w (ITE8620E superio)
    * 5537ce7c2f mb/google/fatcat: Fix GPIO config for headphone jack detection
    * 953957e961 mb/google/trulo/var/pujjolo: Change ICCmax at VCCIN_AUX from 25A to 27A
    * 87d5c7224b mb/google/trulo/var/pujjolo: Select Strauss keyboard to show G icon
    * eb005f5f5c mb/google/brya/var: Clarify comment for 'tcss_aux_ori'
    * 85b26f75d2 soc/intel/xeon_sp: Remove fast_spi_cache_bios_region
    * fc4911ec35 soc/qualcomm/x1p42100: Add CPU Clock boost support for X1P42100
    * 1a9fb29a53 soc/qualcomm/common: Add API to enable Zondaole PLL for X1P42100
    * e272b20c85 sb/intel/common: Remove unused function prototype
    * c54fde5040 sb/inte/common/gpio: Implement gpio_input() and gpio_output()
    * 55bed620a4 mb/dell: Use gpio_base2_value
    * 84899e6fb7 sb/intel: Convert set_gpio to gpio_set
    * 0c79443ca9 sb/intel/*/gpio: Convert get_gpios to gpio_base2_value
    * 69364fc9e0 sb/intel: Convert get_gpio() to gpio_get()
    * 2d7891abe2 sb/intel: Add soc/gpio.h
    * 6a20caea01 drivers/lenovo/hybrid_graphics: Add missing header
    * 04cc15feb4 sb/intel/common/pmutil: Drop unused header
    * b44c0ab25b ec/lenovo/pmh7: Include stdbool.h
    * b20f6d27e2 device/dram: Rename 'USE_DDRx' config options
    * 1ade8247ce mb/google/trulo/var/pujjocento: update hda_verb table for ALC257
    * 28848dc4fb mb/google/trulo/var/kaladin: Add elan touchscreen support
    * 9a89e3b4c6 mb/google/trulo/var/kaladin: Select USE_UNIFIED_AP_FIRMWARE_FOR_UFS_AND_NON_UFS
    * f84934a203 mb/google/trulo/var/kaladin: Add DRAM part H58G56CK8BX146
    * dd76bcc4c3 soc/qc/sc7280: Relocate SHRM firmware load to common Qualcomm path
    * 88b10e09b6 mb/google/ocelot: Set TPM_TIS_ACPI_INTERRUPT for all ocelot variants
    * c2dfe61016 mb/google/ocelot: Disable CNVi bluetooth core in the baseboard code
    * 69e6d96aad mb/google/fatcat: Configure host command ranges for FATCAT4ES variant
    * 8bdc170901 mb/google/brox/var/lotso: Update VR domain config
    * a6842184ab mb/google/fatcat/var/kinmen: Use ACPI for touchscreen power sequencing
    * 712aac780f mb/google/rex/kanix: Use ACPI for touchscreen power sequencing
    * 26e6d0be00 mb/google/rex/var/karis: Use ACPI for touchscreen power sequencing
    * 00d99a6e9b mb/google/brox/var/lotso: Configure Acoustic noise mitigation
    * 4b6ebbdd94 mb/google/skywalker: Initialize clkbuf and srclken in romstage
    * f131f0e336 soc/mediatek/mt8189: Add clk_buf and srclken_rc drivers
    * 2de0158eec soc/intel/pantherlake: Add asynchronous CBFS file loading support
    * eb1b5ee116 soc/intel/cmn/block/fast_spi: Introduce a DMA transfer queue
    * 182ba52792 soc/intel/pantherlake: Remove mailbox interface offset
    * 2ee78458be soc/intel/pantherlake: Use CONSUMED_BIOS bit
    * 9a8ba5b39a {lib, drivers/intel}: Move BMP rendering logic out of SoC code
    * a617317775 mb/google/fatcat/var/kinmen: Support SAR table selection via FW_CONFIG
    * 92dd8cea59 mb/google/nissa/var/riven: Add parade touchscreen support
    * b98f786375 mb/google/fatcat/var/francka: Increase reset delay to 100ms for ILTK0001
    * 5f0177ac5d mb/google/trulo/var/pujjolo: Update Stylus IRQ wakeup group
    * 5b1a8b53b6 soc/amd/common/i23c_pad_ctrl: add I3C pad config options
    * 4b58ec5ac2 soc/amd/common/block/psp: Add fTPM specific bits
    * 15bf25de78 Documentation/soc/intel: Update the referenced linuxboot_defconfig
    * debfac6352 mb/google/ocelot/var/ocelot: Add wake support for touchpad
    * b6425a9a78 soc/amd/common: Add comments about bootblock
    * 69888bc7fc util/cbfstool/amdcompress: Bail out on invalid ELF
    * 3b008bde8c soc/mediatek/mt8196: Fix intermittent black screen issue
    * da33feeb51 soc/mediatek/mt8189: Correct thermal SRAM base address and length
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit aa0e4205d657d9468b933599e091523c21bbde12
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Dec 15 14:04:21 2025 +0000

    re-base the T480 thunderbolt patch
    
    i noticed that the enablement patch came first,
    before the actual driver. while this functioned
    overall, it was obviously flawed in terms of
    the resulting git history. the person who sent
    the patch previously had 0046- on both patch
    names, which meant that alphabetical sorting
    caused the enablement patch to be applied
    before the driver patch.
    
    furthermore:
    
    it seems that the submitted had manually re-applied
    the same Kconfig changes in the enablement patch,
    adding their own name - since Kconfig is not
    copyrightable anyway, in this specific example, or
    otherwise trivial, it's probably fine, but the
    original author on the gerrit patch is actually
    Matt DeVillier:
    
    https://review.coreboot.org/c/coreboot/+/88490
    
    I have therefore simply re-based by checking out
    Matt's patch, on patchset 1.
    
    However, patchset 1 of Matt's patch uses patch
    set 16 of:
    
    https://review.coreboot.org/c/coreboot/+/75286
    
    HustlerOne's lbmk merge uses patchset 18:
    
    https://review.coreboot.org/c/coreboot/+/75286/18
    
    The differences between the two can be observed, thus:
    
    https://review.coreboot.org/c/coreboot/+/75286/16..18
    
    It should be clarified that these patches are not
    upstreamed yet, but under heavy review on gerrit.
    However, testing has revealed that the patch is
    mostly stable.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f3dc54432e9df32080a062f942d8b81e2102170f
Merge: ec8617f27a 809e5d29a4
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Mon Dec 15 14:57:52 2025 +0100

    Merge pull request 'T480: Add Thunderbolt support' (#387) from hustlerone/lbmk-alpine_ridge:master into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/387

commit 809e5d29a4f34b4160133f06001271e8627485c4
Author: Hustler One <nine-ball@tutanota.com>
Date:   Wed Dec 10 17:05:26 2025 +0100

    T480(s): Add Thunderbolt support

commit ec8617f27a00b34b0d500b3d21f94872402816f8
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Dec 10 22:30:09 2025 +0000

    coreboot/t480s: fix headphone jack detection
    
    this is merged from a patch sent to coreboot by
    Matt DeVillier, based upon the work done by
    Arthur Heymans for the regular T480
    
    https://review.coreboot.org/c/coreboot/+/90450/2
    
    yes
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 724f905aafa5361bc960ae0d351bc80b9d658a7f
Merge: 9d740e9512 2dea4b79b2
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Tue Dec 2 22:17:21 2025 +0100

    Merge pull request 'T480: Fix headphone jack detection' (#385) from Riku_V/lbmk:t480verbfix into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/385

commit 2dea4b79b29781307d5f002c38b963291ed51970
Author: Riku Viitanen <riku.viitanen@protonmail.com>
Date:   Tue Dec 2 21:58:04 2025 +0200

    T480: Fix headphone jack detection
    
    Recently, a HDA verb bugfix was merged to coreboot:
    https://review.coreboot.org/c/coreboot/+/90023
    
    Tested on a T480. Before the fix, detection *never* worked.
    After fix, it seems to reliably work.
    
    TODO: update docs to reflect this.
    
    Signed-off-by: Riku Viitanen <riku.viitanen@protonmail.com>

commit 9d740e95129ffd3b77623f327d6057a2459586c8
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Nov 22 21:26:40 2025 +0000

    grub: add a keyboard layout for norway
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7a9977af729f3b57845aa0e6d07c32ea4003d213
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Nov 18 18:47:23 2025 +0000

    rebase recent dell thermal safety patch
    
    it didn't apply. i will soon update the coreboot revisions
    ready for Libreboot 25.12
    
    just rebase this patch for now
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit efd17fde1961b0327eaf578a6dcd4e7077698aa7
Merge: d1606ef11f fda3ac975a
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Tue Nov 18 15:13:22 2025 +0100

    Merge pull request 'removed duplicate "payload_uboot" for e6230_12mb' (#374) from honzo/lbmk:e6230_12mb_deduplicated into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/374

commit d1606ef11f0486bdf5d0c00d02df7da8309ad8f3
Merge: 48b00ce3c8 831fa657de
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Tue Nov 18 15:12:48 2025 +0100

    Merge pull request 'fixed typo in "grub_scan_disk" for macbook11_16mb' (#375) from honzo/lbmk:macbook11_16mb_fixed_typo into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/375

commit 48b00ce3c8cca6c022b4cbcaa27fe2348998ca97
Merge: 1b10c072d3 c0dab33972
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Tue Nov 18 15:12:28 2025 +0100

    Merge pull request 'config/coreboot/default: Disable Latitude early thermal shutdown' (#376) from nic3-14159/lbmk:mec5035-updates into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/376

commit c0dab3397273f6266637a7ffc547476b14856f31
Author: Nicholas Chin <nic.c3.14@gmail.com>
Date:   Tue Nov 18 06:48:38 2025 -0700

    config/coreboot/default: Disable Latitude early thermal shutdown
    
    Disable the sudden EC initiated shutdown on the Sandy Bridge/Ivy Bridge
    Dell Latitude laptops as soon as the reaches 87 degrees, allowing the
    standard CPU thermal throttling mechanisms to work.
    
    Fixes: https://codeberg.org/libreboot/lbmk/issues/202
    
    Signed-off-by: Nicholas Chin <nic.c3.14@gmail.com>

commit fda3ac975abaf465023680e5e53fd02b797741f6
Author: libreboot Contributor <contributor@libreboot.org>
Date:   Sun Nov 16 00:19:36 2025 +0000

    removed duplicate "payload_uboot" for e6230_12mb

commit 831fa657de4b9fa77460a9de3967894c10f6d1ec
Author: libreboot Contributor <contributor@libreboot.org>
Date:   Sun Nov 16 00:18:31 2025 +0000

    fixed typo in "grub_scan_disk" for macbook11_16mb

commit 1b10c072d3c008de033a763bea846fc9abc5187e
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Nov 15 16:57:48 2025 +0000

    tree.sh: tidy up check_gnu_path
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7ef7e02f7325026c15c34fe8114d7d465e206a41
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Nov 15 16:55:27 2025 +0000

    lib.sh: reduce indentation in setvars
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 214ed3efd25cc456f6d6120783b01abc2f3dcd5b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Nov 15 16:51:59 2025 +0000

    get.sh: reduce indendation in fetch_targets
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9d6af0063b627a26e678b646179c2aaeb063e0d6
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Nov 15 16:51:10 2025 +0000

    get.sh: reduce indentation in clone_project
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d7869a56f52540f833c04c6ece6cbd42dc24b89d
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 11 16:27:47 2025 +0100

    WIP: chromebook integration script
    
    I intend to merge every Chromebook that Mrchromebox supports,
    into Libreboot, ready for the Libreboot 25.12 release. Work
    is still ongoing, and several changes need to happen in lbmk.
    
    I started working on it a few weeks ago (today is
    14 November 2025 as I push this).
    
    Still TODO:
    
    * Automatically create lbmk coreboot targets, based
      on the configs present in MrChromebox git
    * Re-work git repository management in lbmk, such that
      a list of upstreams is used, instead of a hardcoded
      list per configuration; this will allow us to use
      different remotes across the same project, even where
      they diverge. This would then allow us to use the
      MrChromebook repository directly, instead of cherry-picking
      patches into upstream coreboot
    * The note above about remotes would also mean that we can
      use MrChromebox's own edk2 repository directly. All of this
      would reduce the burden on lbmk.git
    * Support building edk2 payloads, exactly mirroring the
      setups used on MrChromebox builds
    
    There are some things that need to be checked first, for
    boards that use MMC-based or eMMC-based storage, for the
    GRUB and SeaBIOS payloads, also U-Boot, because I will
    also be using these.
    
    As such, this current script shall sit in lbmk master, but
    it is not yet finished.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 25f523bbacb4374337f443f0ffd9b458aa328c50
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 22:40:29 2025 +0100

    get.sh: return clone_project if multi-tree
    
    this is the true fix, replacing the fixes previously
    reverted.
    
    the problem with the old fix was that it was a hack,
    and could result in the archived backup of a code repo
    being the wrong one; the destination was the one for
    the main repo, but what if we were cloning the backup?
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit bec7e6d4cbbb47afb26980056d8f46fc8e4f992c
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 22:32:43 2025 +0100

    Revert "get.sh: don't frivolously copy tmp git clones"
    
    This reverts commit b840cf3a832de815a87d9d10b698eec97aceb342.

commit f632b8aed7cff03f513c750f4a8ec515ed3fbba6
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 22:32:35 2025 +0100

    Revert "get.sh: remove a redundant check"
    
    This reverts commit e2a97455cc25921dcb9f5e3a4bf06cdfb3e34b49.

commit e2a97455cc25921dcb9f5e3a4bf06cdfb3e34b49
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 21:01:31 2025 +0100

    get.sh: remove a redundant check
    
    loc is never empty.
    
    if it is, it's a bug. don't hide bugs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b840cf3a832de815a87d9d10b698eec97aceb342
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 20:57:18 2025 +0100

    get.sh: don't frivolously copy tmp git clones
    
    this fixes a regression in a previous patch, this time
    also taking account for the different cache locations.
    
    all of get.sh needs to be purged, and re-written clean.
    it looks clean. but it's years of hacks.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2aea7f622949ecdc9b1a434eff8c789a357e4c67
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 16:07:16 2025 +0100

    Revert "get.sh: make forcepull a macro"
    
    This reverts commit b3232a7c4a6466381d798d7beda56fd020d86d54.

commit b3232a7c4a6466381d798d7beda56fd020d86d54
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 14:02:55 2025 +0100

    get.sh: make forcepull a macro
    
    :
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 54aa5b7d329526152c915d5a8603107248cf0a09
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 13:57:14 2025 +0100

    tree.sh: unify -f/-F in case/switch handling
    
    they're the same commands, but -F does forcepull
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 96f786b962e2a933ddf69cd74f8b2635f7b142b1
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 17 13:29:56 2025 +0100

    tree.sh: convert do_make into a macro
    
    use it similarly to if_dry_build/if_not_dry_build
    
    there is nothing cooler than an sh macro
    
    :
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 47f08e2e6df999bd12526fb755f50f4aed8e560d
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 16 15:57:31 2025 +0100

    git: don't use review.coreboot.org as main
    
    where backup links are available, use those as main instead.
    
    this is because of the new XBMK_CACHE_MIRROR variable, which
    makes --mirror be used
    
    when performed on review.coreboot.org, this also pulls down
    all changes from gerrit code review; the github backups for
    example only contain the official branches, but gerrit creates
    a new ref per merge request.
    
    a user can still run ./mk -F to force pulling all repos,
    including the coreboot.org ones, but use of -f will skip the
    coreboot.org ones if the backup links worked and contain the
    local commit needed, by a given project used in xbmk.
    
    this patch won't change any real-world behaviour for xbmk
    users, but it is done as a courtesy to the coreboot project,
    in that it largely avoids a sudden surge in coreboot.org's
    traffic if lots of users start doing XBMK_CACHE_MIRROR=y
    
    if XBMK_CACHE_MIRROR is not set, or set to anything other
    than y, a regular clone is performed, saving cached sources
    to cache/clone/ - otherwise, cache/mirror/ is used.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e1b6ccf69ec42f24d751466cee00b265fe44e899
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 16 15:46:44 2025 +0100

    xbmk: sort global variables alphabetically
    
    also separate some of the special ones.
    
    this makes the variables easier to read/find.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d84a556bf084b4622cf389847ee3f888bb4dbbe5
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 16 14:22:06 2025 +0100

    get.sh: use the same directory map as --mirror
    
    Don't hardcode the cache directory, and don't store
    remotes anymore. This change retains compatibility
    in practice, with the older directory location, because
    it's extremely unlikely that newly generated locations
    would conflict with old ones.
    
    With this new change, non-mirror git clone caches are
    now done twice; one directory per remote, rather than
    one directory with two remotes.
    
    This is just inherently much more reliable.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b333ddfe73ce986f1b79f276bbac2c84a11d8461
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 16 13:30:55 2025 +0100

    get.sh: use --keep-cr on git-am
    
    Some repositories might use CR-LF line endings. This option
    keeps Git from mangling patches when merging.
    
    Repositories that don't do this, such as ALL repositories
    currently used by xbmk, will be unaffected by this change.
    
    This is being done in preparation for importing MrChromebox
    edk2, as Intel's own edk2 repository on GitHub uses these.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d83dd506c2b8a5846e2f6a07cbd93ef6ba37ded9
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 16 00:26:46 2025 +0100

    get.sh: More reliable git remote caching
    
    Don't do one repository for all remotes. Do one *clone* per
    remote.
    
    This also means that users no longer download information twice,
    in practice, because the backup repository will only be downloaded
    if the main one didn't work.
    
    Theoretically, this change is makes the process less efficient, but
    in practise it's more reliable now.
    
    We do now use --mirror on the git clone command for caches, but we
    already did git pull --all before.
    
    This just ensures that we absolutely have all local code.
    
    NOTE:
    
    The new code isn't used by default. To use it, you must do:
    
    export XBMK_CACHE_MIRROR="y"
    
    Otherwise, the old behaviour will continue to be used. This is
    because the new code, while correct, puts more strain on upstream
    servers (more code being downloaded), and can result in higher amounts
    of disk space being used. The old behaviour wasn't broken, so we'll
    also support that method.
    
    TODO: perhaps also have a check in place to re-use both caches,
    where available, regardless of XBMK_CACHE_MIRROR?
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 18c63682e7a1e7b90ade549d5c84a3c3e4dcdccd
Merge: 11a3e9d887 7b42779912
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Thu Oct 9 12:07:15 2025 +0200

    Merge pull request 'Add Fedora 43 to dependencies' (#364) from bauduser/lbmk:fedora43dependency into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/364

commit 7b42779912dcacc0045db723a9f120c5c7499ef2
Author: bauduser <bauduser@protonmail.com>
Date:   Thu Oct 9 02:56:09 2025 -0700

    Add Fedora 43 to dependencies

commit 11a3e9d88799cfffa77beb836cdef54516690591
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Oct 7 05:52:36 2025 +0100

    xbmk: minor code cleanup (79 character rule)
    
    recent re-factoring lead to certain code lines that
    exceeded 79 characters in length.
    
    we like to avoid this, whenever possible.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit fb95e4ad687e8263948ebcad4c403ce04b8b5a0d
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Oct 7 03:02:01 2025 +0100

    tree.sh: add missing -F flag
    
    i support -F, but didn't include it in the
    actual getopt string.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ca5f0a5eddc39946e03c0a352ff9885f4a1e7d0c
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Oct 7 01:25:19 2025 +0100

    get.sh: use git-show instead, for rev checks
    
    whatchanged is deprecated, and results in an error
    on modern git versions, prompting you to include
    the --i-still-use-this argument
    
    what absolute, utter fucking arrogance. i use the
    whatchanged feature every fucking day.
    
    i will be complaining to git-scm.com about this.
    
    but that's what we do in libreboot. we adapt.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8636d7497cb455ac15407e737b479180186996ee
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Oct 6 13:28:34 2025 +0100

    rom.sh/tree.sh: clean up if_not_dry_build
    
    the way it was used is messy, and a relic of the
    old chained command coding style, from before when
    i recently loosened that requirement.
    
    the new focus is simple, readable code, regardless
    of size.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1b54c7a744ac474bdcf0844ace97741b949a6fa1
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Oct 6 12:43:04 2025 +0100

    rom.sh: use if_dry_build macro
    
    instead of checking if_not_dry_build.
    
    use it here the same way.
    
    yes. shell script macros. it's how i roll.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit afccecbde01ce657bc7a65c2f69f9b4fbd958739
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Oct 6 07:44:38 2025 +0100

    rom.sh: don't run add_cbfs_option on dry builds
    
    i added this in an earlier version of the patch, but
    for some reason removed it.
    
    this is necessary, or the build system will fail.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7d597bc4a1867089eadbd8ab0f6784f57a186cad
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Oct 6 04:48:48 2025 +0100

    disable stack overflow debug on alderlake
    
    see patch
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c716341c130b3746c0994af780f407f4fbd75004
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Oct 6 02:28:24 2025 +0100

    cb/kabylake: don't hardcode power_on_after_fail
    
    I realised that the Dell OptiPlex 3050 Micro has NVRAM available.
    Use that backend, and hardcode power_on_after_fail to Disable,
    which is already done in cmos.default.
    
    The Lenovo ThinkPad T480 currently has no option table in coreboot,
    besides the CBFS one. For this, the CBFS option table has been
    enabled, and the build system has been modified to insert
    a relevant config for power_on_after_fail.
    
    Nicholas Chin informs me that Kabylake generally has legacy NVRAM,
    so enabling it for the T480/T480s should work, but we'll need
    to use it in the future anyway; better to just use CBFS now.
    
    I *could* use the CBFS backend on 3050micro as well.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b5ad829ffe9dfb9f5a32f4ac95159a35d2662877
Merge: 9b104fca44 e82e2a1332
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Mon Oct 6 03:22:07 2025 +0200

    Merge pull request 'config/coreboot/default: Add Haswell NRI SMBIOS type 16/17 patch' (#363) from noisytoot/haswell-nri-smbios-memory into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/363

commit e82e2a133210d5e8fe195a2b94e491b4f9a3605f
Author: Ron Nazarov <ron@noisytoot.org>
Date:   Mon Oct 6 01:12:25 2025 +0100

    config/coreboot/default: Add Haswell NRI SMBIOS type 16/17 patch
    
    This patch implements SMBIOS type 16 and 17 for Haswell NRI, making
    `dmidecode -t memory` work.
    
    From https://review.coreboot.org/c/coreboot/+/89385

commit 9b104fca44a1d4a73fbfb4eed308098c51cc43e3
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Oct 5 01:23:32 2025 +0100

    init.sh: only create cache/ here
    
    also, the check is -e, not -d, because we
    might be operating on a symlink.
    
    it's a bit hacky but this should work.
    
    the previous change (now reverted) broke
    re-use of the main cache/ in release work
    directories.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ee2bca65f6e8eb8b1732803aece31c0eee03aaed
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Oct 5 01:21:26 2025 +0100

    Revert "init.sh: explicitly create cache/"
    
    This reverts commit 23f98c29581e7d2cf5639e93fd89a6e64bec5635.

commit 23f98c29581e7d2cf5639e93fd89a6e64bec5635
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Oct 5 01:04:25 2025 +0100

    init.sh: explicitly create cache/
    
    otherwise, an error occurs when doing ./mk release
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1e488aae78d96eedcd80b9a10fa2e7fa6e374ec2
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 23:56:06 2025 +0100

    Revert "remove unar from dependencies"
    
    This reverts commit e8a3cd8cd0e99dbb99dbb1811c0396d2a3ee00c4.
    
    We still need this for extracting the CAB files containing
    KBC1126 EC firmware.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c1d6cd22c2390c7317b8996f9b993697491c1eab
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 22:36:11 2025 +0100

    xbmk: don't call mkdir. use xbmkdir (new function)
    
    xbmkdir checks if a directory exists, before running
    mkdir, and then still uses -p
    
    i was testing xbmk on arch linux today, and noticed
    that it errored out when a directory already exists.
    
    i'm mitigating against buggy or differently behaving
    mkdir implementations this way, by wrapping around
    it.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f358cfaa55219c537de0eb534997be36cba93f14
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 22:22:55 2025 +0100

    coreboot/x2e_n150: fix the alderlake n fsp link
    
    this fixes ./mk inject
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 6a00b7a584aebd7ecc04f6b8b2e813f357127089
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 22:21:23 2025 +0100

    coreboot/default: don't require alderlake fsp repo
    
    we need the full fd path to be automatically set. this
    patch prevents it from being removed by ./mk -u coreboot
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 247cb854890fd3dfbead71143dc9be18577ce4ce
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 22:02:03 2025 +0100

    don't compress alderlake fsp
    
    for reproducibility, we must not compress it
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 183f3781509226efc13b8c1b2db10485193514d3
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 20:11:17 2025 +0100

    coreboot/x2e_n150: unset CONFIG_FSP_FULL_FD
    
    otherwise, ./mk -u screws up the FSP path
    
    we were still using the correct path for downloading
    in ./mk inject, and in practise, the file used by
    coreboot would have been the same, but without our
    hash verification after splitting up the FSP.
    
    that's the main reason we split FSP in lbmk, rather
    than relying on coreboot's logic for this.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 31fa7ea5915e23d405034537e860fec7d2058ab6
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 16:43:37 2025 +0100

    vendor.sh: re-do the previously reverted change
    
    but do it better. this time, the change won't cause any
    behavioural differences.
    
    the reason for the change is we don't want "$@" inside
    an eval statement, if such calamity can be avoided.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2956fcc0516d2db11294a05e9f21b1d8b73c8e63
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 16:14:36 2025 +0100

    vendor.sh: fix setvfile
    
    this reverts change made to this function in:
    
    commit 4f01dc704a1ed0e18fc0efc1500e61b4bc41b0e6
    Author: Leah Rowe <leah@libreboot.org>
    Date:   Sat Oct 4 06:13:15 2025 +0100
    
        xbmk: remove even more eval statements
    
    for some reason, the new code caused sch5545 ec firmware
    to never download.
    
    the old code wasn't horribly broken, so just use that.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8334c93dac9d6d601c9ae3e585195f0171513293
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 10:09:24 2025 +0100

    release.sh: preserve clean sbase before building
    
    this way, the clean version can be placed inside the
    release tarball.
    
    there is a make clean option in sbase, but we should
    not really on this.
    
    the design of xbmk is that a clean src tarball is
    created. there must not be build artifications in it.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8969cc734f8f9855fa4bbfa92c18224adb9e5ddc
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 09:56:42 2025 +0100

    xbmk: use sbase sha512sum, not host sha512sum
    
    the --status flag seems to be a GNUism
    
    as stated in the previous commit, i import sbase
    suckless now, so as to have a consistent implementation
    of sha512sum.
    
    this ensures that its output is reliable, when i'm using
    the output of this command within backticks.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e9a910b33c7837b4b868e3abda18eb4810df7f02
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 09:14:33 2025 +0100

    config/git: import suckless sbase
    
    i currently use the output of sha512sum in several
    places of xbmk, which is a bit unreliable in case
    output changes.
    
    other cases where i use util outputs in variables
    are probably reliable, because i'm using mostly
    posix utilities in those.
    
    to mitigate this, i now import suckless sbase, which
    has a reasonable sha512sum implementation.
    
    *every* binary it builds is being placed in build.list,
    because i'll probably start using more of them.
    
    for example, i may start modifying the "date"
    implementation, adding the GNU-specific options that
    i need as mentioned on init.sh
    
    i'm importing it in util/ because the sha512sum
    util is needed for verifying project sources, so
    if sbase itself is a "project source", that means
    we can into a chicken and egg bootstrapping problem.
    
    this is sbase at revision:
    055cc1ae1b3a13c3d8f25af0a4a3316590efcd48
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2cfaba181b3c68761871fa47b32725c934423c14
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:41:25 2025 +0100

    xbmk: rename cv variables, for clarity
    
    the new names are still a bit crap, but a bit better.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1943dba60836abc9446df64dbc3aac881dfdd285
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:37:41 2025 +0100

    tree.sh: rename xtree to xgcctree, for clarity
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 51e424c7d189815ad3931ea07fc1f2b2b48e10c6
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:36:18 2025 +0100

    tree.sh: rename btype to buildtype, for clarity
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d95af9ba4429963a2301a93c0759e107753a4e08
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:33:26 2025 +0100

    vendor.sh: rename _t to blobtype, for code clarity
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 63002732f50671f76b8a92eba417819dfd39aee3
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:29:33 2025 +0100

    tree.sh: rename _f to flag, for code clarity
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b7a9aad9fc700ddc38f0ce89f52b8fe7aa2fe86a
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:20:27 2025 +0100

    get.sh: delete tmp patch list when done
    
    yet another oversight
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a115679c57dd4cef3f98d1b753af082742569054
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:17:53 2025 +0100

    get.sh: sort patches when applying
    
    this was an oversight in my recent patch unrolling
    the condensed code lines, to remove eval statements.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0776eb414c82cff51a25fdbcbaed8368cfa05a9f
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:12:24 2025 +0100

    vendor.sh: make the fsp hack a bit cleaner
    
    it's still a dirty hack. i really should make
    a better check here.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 70cdb03f7f4c229917b096bde14473e6609f4643
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:08:34 2025 +0100

    vendor.sh: correction to fsp hack
    
    i unrolled these lines earlier, but this line was
    incorrect; dl was already handled. it's dl_bkup
    that we have to handle here.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1eafcf9029eb245b73e0af33672848e368082029
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:07:07 2025 +0100

    vendor.sh: minor cleanup
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit efd1db1ca37019333df27ae3f37e019f3eb13376
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 08:03:12 2025 +0100

    release.sh: remove eval statement in nuke()
    
    the symlink check is what made me use eval, but the
    symlink check is not required, since i check every
    entry that goes in nuke.list anyway.
    
    not having that symlink check is safer than having
    an eval statement on that line.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b4c7cac8a2f1a50542df723458da59e6063b6b89
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 07:50:34 2025 +0100

    xbmk: rename the "dry" variable to if_not_dry_run
    
    and add a line break where it is used
    
    now it is essentially a macro of sorts, used in
    terms of syntax, to mean the same as:
    
    if [ "$dry" != ":" ]; do
            thing
    fi
    
    in this case, we say:
    
    $if_not_dry_build \
            thing
    
    yes. macros in sh are a thing.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9f84bd4f3469b5c59774db05ccc84c9d69bf1134
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 07:36:19 2025 +0100

    coreboot/mkhelper: don't use eval
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4f01dc704a1ed0e18fc0efc1500e61b4bc41b0e6
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 06:13:15 2025 +0100

    xbmk: remove even more eval statements
    
    in one or two cases, the use of eval is retained, but
    modified so as to be safer.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7f8d85140fd229e97e539ca463fbd94545997fd6
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 05:23:47 2025 +0100

    xbmk: remove the setcfg function
    
    this allows me to remove several eval calls, and the
    errors relating to configs can now show exactly which
    function they occured in, allowing for easier debugging.
    
    once again, eval should be used sparingly if at all.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0a74cc8ec6038492f64074e18cc10ba7ab31ac94
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 03:05:23 2025 +0100

    xbmk: clean up a few err calls
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a09ec1d02b275fa0d79a0a21be77d450fd86318b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 02:55:52 2025 +0100

    xbmk: remove more eval statements
    
    i will eventually find a way to remove them all,
    while still leaving the code completely clean.
    
    in practise, i never use the contents of a file
    for eval and the inputs are carefully checked.
    
    however, over-use of eval is always a bad idea
    in shell scripting.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0605fbe72032125c8b430f1454794be40ab61009
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Oct 4 02:43:24 2025 +0100

    xbmk: general cleanup: unroll condensed code lines
    
    i overlooked a number of lines, during previous cleanup
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e1c70f43198e7d709026ab7cd0422adc24083bd4
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 3 22:43:06 2025 +0100

    vendor.sh: remove superfluous AND
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e1c580f6bc9e7ab0fcdd4da40e9e01efeaf9cde1
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Oct 3 00:04:23 2025 +0100

    grub/xhci_nvme: fix target.cfg
    
    it still said tree="xhci"
    
    it should say  xhci_nvme
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 093606784af0442350caf2cdacdde9324c8d582d
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 23:59:57 2025 +0100

    add fedora42 dependencies from bauduser
    
    this was sent by bauduser, who messed up the pull
    request (number 362). i'm simply merging the
    change manually.
    
    once again, i instructed this contributor to
    properly learn git vcs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5e35b0684df4d0914b5e1845f422474c06464b02
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 23:57:49 2025 +0100

    dependency/debian: libstdc++-arm-none-eabi-newlib
    
    this is needed to make pico-serprog compile.
    
    this change is submitted by "bauduser" in lbmk pull
    request #362, but the PR was messed up. for such
    a trivial change, I simply  merged this change
    manually, instructing the contributor to properly
    learn git vcs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit af88a066d0e4fc5aaa63b2e7a4989d1f64b3f7b2
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 23:47:31 2025 +0100

    grub/xhci: rename to grub/xhci_nvme
    
    we have the "default" tree, then the "nvme" tree which adds
    nvme support.
    
    the "xhci" tree adds xhci patches, *and* nvme patches.
    
    riku suggested that i rename it accordingly, and his advice
    was quite correct, indeed wise.
    
    this will reduce confusion for contributors, including *myself*.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d90defeae3e4e7c91ba521e44071d962abb33d3f
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 07:07:54 2025 +0100

    mrc.sh: remove superfluous eval statement
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a74af6aa0579e6442646713c794c4ecc62902831
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 06:55:29 2025 +0100

    tree.sh: remove superfluous eval statements
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 15cefca84b4708b1e837a7092a7372c29f842e42
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 06:53:36 2025 +0100

    rom.sh: remove superfluous eval statement
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2b4b5bf82e7b77fc4912028901df421f5e67fb0b
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 06:52:10 2025 +0100

    inject: remove superfluous use of eval
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 85b10a674b8cbb141db340c2be1b4be33237c9d4
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 06:51:03 2025 +0100

    init.sh: reduce the use of eval statements
    
    also remove the unused _nogit variable
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 99f2c0fcf9791247b32248774fb465b950747a52
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Oct 2 06:27:39 2025 +0100

    get.sh: reduce the number of eval statements
    
    also split up try_fetch()
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit be1f4ebb9ccf1802d970c17a8b8a5259954d112a
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 16:51:00 2025 +0100

    get.sh: allow force-pull via -F instead of -f
    
    use of ./mk -F behaves the same as -f before the
    previous commit.
    
    this can be useful, during development when we want
    to update revisions.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 24f120d1b8d607a55d21f01032cb785813432a43
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 16:28:04 2025 +0100

    get.sh: only pull if the local revision is missing
    
    we pull from upstream in cached git repos, before performing
    an operation, and we run from the cache, but we do this every
    time, even if a local revision exists, defeating the purpose
    of the caching; on unreliable/intermittent internet connections,
    this can cause a problem.
    
    this also causes us problems with gnulib.git and grub.cfg, which
    for *some reason* are really slow, even when doing a pull.
    
    this change improves the efficiency of the build system, during
    release builds, on a development repository where we already
    have lots of caches.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 084b8b65c69f6c0411be42b843c3e0f99a387282
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 09:26:46 2025 +0100

    u-boot: make the libreboot logo rainbow again
    
    i like the rainbow
    
    removing it was a mistake
    
    this patch brings it back
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2ac51f442b2760d3c3509683e7eb3d179e4df52f
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 07:57:39 2025 +0100

    init.sh: break up xbmk_set_env
    
    what this function does will differ wildly,
    depending on whether it's a child instance
    or a parent instance of xbmk.
    
    break up this function accordingly.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8b351e51aa61c2091d7e4abbaf494c251e0c9c98
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 07:42:10 2025 +0100

    tree.sh: break up check_gnu_path to subfunctions
    
    this whole check could probably be removed, honestly.
    
    it was only put in place during the debian trixie testing
    release cycle, before they finally updated gnat just before
    the stable release of trixie came out.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 3b6d2b799ccff1f1803401d7e2ff64aa21a26b3b
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Oct 1 07:19:04 2025 +0100

    vendor.sh: break up the extract_kbc1126 subshell
    
    stick it in a new function, for easier reading.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 347330a61122cadbee538baae020415cb71b380a
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 23:20:25 2025 +0100

    coreboot/x2e_n150: rename config to fspgop
    
    because it's using fspgop init code, not libgfxinit
    
    this is enabled by the previous patch, which now properly
    handles seabios payloads when dealing with this.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f272c4d1c412787cc2412130c1b91949bceee3c8
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 23:18:50 2025 +0100

    rom.sh: support "fspgop" init mode
    
    for all intents and purposes, this functions like libgfxinit
    corebootfb, but uses intel fsp's video initialisation instead
    of coreboot's native initialisation code
    
    this is currently in use on the x2e n150 mainboard, whose
    config is dubiously named "libgfxinit_corebootfb"
    
    now the config can be renamed, and will be, in the next commit.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit aa3ccf04330962f029bd7c0a6104b2285b6719c7
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 22:09:58 2025 +0100

    fix up old comment in vendor/x2e_n150
    
    theu current comment is for an old version of the n150
    patch, before it was actually merged. the comment has
    been adjusted, to match the actual implementation that
    was merged.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b786918b2b34531e3c50f03968c1232a5dc7ba1b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 11:27:16 2025 +0100

    vendor.sh: use bsdunzip, not unzip
    
    most implementations of unzip are info-zip
    
    we already compile libarchive for bsdtar, to extract
    rar archives in vendor.sh
    
    now we also use bsdunzip
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c6939cf390f8733d1e2e7f98300e560d95ff271c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 11:23:52 2025 +0100

    libarchive: also copy bsdunzip and bsdcpio
    
    bsdunzip in particular, can be used instead of relying
    on the host to provide unzip.
    
    most linux hosts use info-zip as the implementation,
    which bsdzip is compatible with.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 06c1ed009d9a2ad35782b7df338db600ae5e9ad9
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 03:20:07 2025 +0100

    add -p flag to old me_cleaner too
    
    this lets you skip fptr checks
    
    not currently used on this version, but i want this
    patch here so that it can be in the future
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e8a3cd8cd0e99dbb99dbb1811c0396d2a3ee00c4
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 02:31:43 2025 +0100

    remove unar from dependencies
    
    we use bsdtar now
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8e8f29c2e571d09382d707924dfbd6742bf48896
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 02:29:04 2025 +0100

    vendor.sh: remove false error message
    
    mkdst cycles through a bunch of outputted files
    when running an extract function, to find the
    right file as per defined checksums; if one is
    found, it can still show errors for the others,
    leading the user to think something is wrong.
    
    remove their fear by removing this benign error.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4075c8be38c49f4b6de6366455bc1ced9826a54f
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 01:55:47 2025 +0100

    vendor.sh: use bsdtar, not unar
    
    unar is buggy and crap
    
    and bsdtar has superior licensing
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e527820ceb71166b0bf80336d48799ff3bbf2868
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 01:44:50 2025 +0100

    vendor.sh: don't use unrar
    
    the only practical way to use it is to to use
    the non-free version; currently used as a
    fallback if unar fails.
    
    however, i'm also going to scrap unar and
    use bsdtar instead.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7b297a44ce1b985485c41d8f9f4fd07e52d4f4b3
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 28 01:35:26 2025 +0100

    config/git: Support building libarchive
    
    This is for bsdtar, which we will use in place
    of unar, because unar is not available on all
    of the distros, and we had some recent problems
    with it.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d4f5fdec06c64d73b5df3b51aa681220896d38a2
Merge: 9da4fa64a6 b4c3bafb0e
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Sun Sep 28 01:25:27 2025 +0200

    Merge pull request 'New mainboard: X2E_N150' (#361) from Riku_V/lbmk:x2en150 into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/361

commit b4c3bafb0eb7de0cd836d66a1b675430645d8513
Author: Riku Viitanen <riku.viitanen@protonmail.com>
Date:   Sat Sep 27 10:53:05 2025 +0300

    New mainboard: X2E_N150
    
    Patch in Gerrit: https://review.coreboot.org/c/coreboot/+/89281
    Not working: USB3 ports only work at USB2 speeds.
    
    IFD:
    Modified the original by:
    - Removing Device Exp2 region (empty anyway)
    - Enlarging the BIOS region to use this freed space
    - Setting the HAP bit in PCHSTRP55 using a fork of
      me_cleaner: https://github.com/XutaxKamay/me_cleaner
    
    Signed-off-by: Riku Viitanen <riku.viitanen@protonmail.com>
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9da4fa64a6f5e332f0df8d308d29ec1c3f63dac1
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Sep 27 15:56:39 2025 +0100

    coreboot/default: allow alderlake fsp in releases
    
    i delete unneeded fsp modules in releases, to cut down
    on the tarball size. so, currently, only kabylake fsp
    is distributed.
    
    i'm now also allowing alderlake fsp, because riku has
    sent a patch adding an alderlake machine to libreboot.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c9d6143e20fba08ff8a55712da2709d1f7f27681
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 13:33:36 2025 +0100

    gru bob/kevin: make u-boot bootflow timeout 8secs
    
    not 30secs
    
    it's 8 seconds on x86
    
    8 is more reasonable. 30 feels too long.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit fa6c3512d683554c84d0b5f335607be118cdf9c9
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:25:13 2025 +0100

    rom.sh: remove TODO note
    
    the return is necessary when release=n while doing
    release builds, because it prevents a build error
    since the given images don't exist in that scenario.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 574fd30cdea0d9b53200ed3dc9260ebd65ad0fab
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:21:40 2025 +0100

    inject.sh: don't exit from patch_release
    
    return instead. xbmk's coding style specifically
    prohibits anything other than x_ or err from
    running "exit".
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4fbafaaa46aba852b44dd80f3bae5635ad17e476
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:14:45 2025 +0100

    init.sh: remove comment in pybin
    
    the following checks on the path mitigates the
    lack of error handling in the findpath command
    that sets pypath.
    
    this was all thought of when i initially wrote
    this code. it's perfectly fine.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 204e310f5a0ff51e53bd481d6eaa135fc629618e
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:13:17 2025 +0100

    get.sh: remove unnecessary check in try_copy
    
    the check for whether a file is present is unnecessary,
    because the following cp command would also print the
    file name if it doesn't exist, and exit with the same
    non-zero status.
    
    let cp do the work.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b4fbdb448d72845586623a8551ceec98e4fecd3c
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:12:21 2025 +0100

    get.sh: rename try_file to try_fetch
    
    the previous function name was misleading, because
    this tries multiple methods including git and curl.
    
    therefore, this was renamed to match what it dose.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e42cb4f4cd513f512397ff7a46b6174b39a9708e
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 26 00:09:46 2025 +0100

    xbmk: tidy up some if statements
    
    this is an extension of the previous work to unroll
    most of the condensed code lines.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f5060232e16ab1c041f0108a5f65d8f96f79756d
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 25 23:54:16 2025 +0100

    init.sh: remove TODO note
    
    on further inspection, the following check ensures that the
    python version number is 3.
    
    if anything went wrong, the possibility alluded to in the
    comment wouldn't actually matter in practise.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0303167e25c2bb5856eb2fe15694d8081543623a
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 25 02:24:05 2025 +0100

    init.sh: create TMPDIR *after* suid check
    
    otherwise, it may get created as the root user, disabling
    further use of lbmk until manual user intervention.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1a74172a17656414357399f588192105c146a67b
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 25 02:18:40 2025 +0100

    release.sh: use cache src on release builds
    
    use what's in cache/clone/ from the main directory
    
    this speeds up the build process
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 6bb4e2c72a98a7864bbd629c7adf804b09189d73
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 23:34:12 2025 +0100

    init.sh: remove symlink check on XBMK_CACHE
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit dc722b5bb87d0c24269d57d09f917e9b1cb26fce
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 23:06:16 2025 +0100

    init.sh: switch back to old TMPDIR checks
    
    the new check is still retained, to the extent that
    the lock file still contains the TMPDIR string, and
    it's checked whether this changed during execution.
    
    however, the current TMPDIR handling is over-engineered
    and prevents the re-use of project source caches when
    doing release builds; this means that the release builds
    happen much more slowly, especially for slow internet
    connections.
    
    this change *fixes* that bug. now release builds once
    again re-use the main cache/ directory.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4686ba8a4af7d3bc53a0447a2aeeb299780d03be
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 20:14:51 2025 +0100

    make notices a bit more readable
    
    add line breaks, so that the license and author are
    visually separated. this makes it easier to read.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5a8f350bc7a1f48c352d89c01c3948b58ba7c894
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 18:19:48 2025 +0100

    release.sh: fix typo in script: ./mk, not ./mx
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8347e2c85da0841dd35d6f94a5b3cf14bd1adff0
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 13:19:23 2025 +0100

    xbmk: cleanup of recent code refactoring
    
    be a bit less pedantic about if else clauses. leave the
    big ones still with then on separate lines, where else
    is specified.
    
    also unroll a few condensed code lines where i missed
    a few.
    
    sloccount 2303 in lbmk. that's still only slightly bigger
    than libreboot 20260907 which was 2180, and still much
    smaller than libreboot 20230625 which was 3322.
    
    this is *without* the condensed codelines, so now the only
    thing that's reduced is the overall amount of logic present
    in the build system.
    
    and i should clarify that lbmk is presently much more powerful
    than both of those two versions (20160907/20230625).
    
    the 2016 one is useful for comparison historically, since that
    was the last major version of libreboot prior to the great
    second coming of leah in 2021; and the 2023 june release was
    basically the last one before the great audits of 2023 to
    2025 began.
    
    not to brag (not much anyway), but all of this means that lbmk
    is an insanely efficient build system, considering all the
    features it has and what it does.
    
    i unrolled the condensed code style in lbmk, making the scripts
    a lot easier to read, because i received complainst about the
    condensed style previously used; nicholas chin and alper nebi
    yasak both told me that it sucked, and riku viitanen had hinted
    at that same fact several months prior.
    
    so hopefully now, lbmk is a bit nicer. those and other people
    often find it challenging to challenge me because for reason
    they assume i'll get upset and fly off the handle, but it's the
    opposite. i want constant criticism, so that i know to improve!
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 03bf6c185b96e631d0f52a67b685628514b3776c
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 08:53:24 2025 +0100

    mk: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0275c60111be536bc1d775d572ddaa34899886ab
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 08:37:18 2025 +0100

    get.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 193001bc712372eb1fd50fd1411badde2c9e4635
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 06:54:36 2025 +0100

    init.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ace167445f323ff4c87e8a037b553aa0c3132c1c
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 03:13:42 2025 +0100

    inject.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 94ab695457b2827cf9eb1c08fd38d8844c06f83c
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 24 02:15:41 2025 +0100

    lib.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8d5d6a3e2aac1557838be59d8f1a32608f97e6af
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 23 12:19:47 2025 +0100

    mrc.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 45c4d4045cae5ff7d144a3c04afa61b94fdcaa44
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 23 12:18:15 2025 +0100

    release.sh: unroll condensed code lines
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f5c91ff0ee73cfbb3383ca5bdc20ee11898b3b69
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 23 11:47:25 2025 +0100

    rom.sh: unroll condensed code lines
    
    ditto to last commit
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e2825864278cdd6f9ca9210e9e9b55108e8b5093
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 23 05:55:18 2025 +0100

    tree.sh: unroll condensed commands
    
    i went further than in the previosu commit. in this
    commit, i also provide indentation inside subshells,
    to make it clearer that soomething is being done
    inside a subshell.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8b3f476b577da3820d603fb081d105bfed3eca1a
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 23 02:34:00 2025 +0100

    vendor.sh: unroll condensed code lines
    
    this is part of a general effort to make lbmk
    easier for novices to understand.
    
    more commits to follow (one for every script).
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 6b796e2b4c0598d8dabfa0ee503418c1ad01e06b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 21 19:52:14 2025 +0100

    init.sh: make TMPDIR *after* calling xbmkpkg
    
    otherwise, running ./mk dependencies as root will
    create xbmkwd/ (temporary directories) as root,
    which will then prevent non-root instances of lbmk
    from being able to make temporary files.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2c02b17810da58ef482608be844493ad25b96c0f
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 19 16:47:05 2025 +0100

    lenovo/t440p: add a target with 4mb cbfs
    
    this is exactly the same as the normal t440p config,
    except that cbfs is 4mb instead of 8mb.
    
    this is useful when externally updating libreboot,
    or unbricking; it could also be used for lazier
    installation, where you only flash the 2nd chip
    without doing a disassembly to get at the other one,
    if the user didn't care about neutering the ME.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c11c5a7a14a75abb2287b5ccab714d7b44c9c1c9
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 18 23:35:12 2025 +0100

    Revert "xbmk: don't use backticks for command substitution"
    
    This reverts commit 4999a49de39667b3239fc2010d0e99c958b29417.

commit 14bcb3a6fa445cde3dd11db88f100fa037bd558d
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 18 14:55:52 2025 +0100

    config/dependencies: cleanup
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit fe301a019be5f388cadb086961030802ef14bc80
Author: Leah Rowe <leah@libreboot.org>
Date:   Wed Sep 17 17:27:39 2025 +0100

    vendor.sh: add missing error handle on sha512sum
    
    it's still outputting to a file, with an error handle
    there, but use of x_ on the sha512sum command itself
    adds further assurance of reliability.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c734a6e75710921cc3734f2e284dfaac80a6f6af
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 14 19:23:40 2025 +0100

    tree.sh: fix bad variable reference
    
    we didn't want to say a variable name here.
    we only wanted to say "trees".
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 995963baf4267e2d6221a5efc210130cfb1c64b7
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Sep 13 13:27:47 2025 +0100

    xbmk: much more verbose error messages
    
    use the new functionality in err(), whereby a given
    function name and arguments can be provided, for
    debugging purposes.
    
    something similar was already done in a few places,
    and replaced with this unified functionality.
    
    this patch will make xbmk much easier to debug, under
    fault conditions.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7bed68f5b7096349bc8a6f48d2f4394db7a581af
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Sep 13 12:05:37 2025 +0100

    lib.sh: use xprintf in err()
    
    if more than one argument is provided, it is interpreted
    as a command, and the command is outputted.
    
    this means that now for example, where you have:
    
    ls -l foo | err "could not list directory"
    
    you could do:
    
    ls -l foo | err "could not list directory" "$@"
    
    this would show all the arguments given to the calling
    function that tried to run "ls"
    
    let's say that function was called bar, you might do:
    
    ls -l foo | err "could not list directory" bar "$@"
    
    right now, it's not easy to provide good debug info
    where err is used, unless it was called with x_, which
    provides the command/arguments that was bugging out.
    
    with this, we now have an easy and readable/maintainable
    way to do the same thing everywhere in xbmk.
    
    this will now be done, in a follow-up commit.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit edcf8cead8bc04bca1a29ad50acdb835c936cafd
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Sep 13 11:50:44 2025 +0100

    lib.sh: use xprintf in x_
    
    don't echo the arguments
    
    this new logic shows quotes, in error outputs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 333739961aba2920405fb5399679bc7a9db81796
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Sep 13 11:31:43 2025 +0100

    lib.sh: check args for errors in fx_ and dx_
    
    check that there are at least two arguments, and ensure that
    they are not empty.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ce5127e46b89c2e5716668911bc82834d06660ff
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 12:16:33 2025 +0100

    inject.sh: add missing semicolon in case
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0deac58e418f97b9701b585dc5bdfbaab526e6e4
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 11:26:19 2025 +0100

    vendor.sh: tidy up apply_deguard_hack()
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c738698ccab4a3bc38f37397019cae8e876318ea
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 11:20:35 2025 +0100

    vendor.sh: tidy up extract_intel_me()
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e9c7338cdad33766aa08ad53401eb8cb9b8d95fe
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 10:55:18 2025 +0100

    vendor.sh: clean out 7ztest on startup
    
    otherwise, some files from a previous me.bin scan
    might still be there, which could lead to the wrong
    me.bin being found.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 07cda7fa12b0e99f2e8b3044865b56d16de6ab00
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 10:53:38 2025 +0100

    vendor.sh: remove unnecessary rm -Rf command
    
    this file being deleted was never created.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4999a49de39667b3239fc2010d0e99c958b29417
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 11 10:17:50 2025 +0100

    xbmk: don't use backticks for command substitution
    
    the newer way handles escaped characters better, and it
    can be nested more easily. it's also more readable.
    
    personally, i prefer the old way, because it's more
    minimalist, but it occurs to me that a lot of people
    nowadays don't know about backticks, but they do know
    of the modern way.
    
    to make the code more readable, i have modernised it.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5cfe54b06d74e6392fc7312c41b5ca299704f7ca
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 9 19:59:46 2025 +0100

    Revert "inject.sh: put tmpromdel in xbtmp, not cache/"
    
    This reverts commit a444910bf2f2cd39039116f38439012031b11963.

commit 9d32af58ebd65f21a0a72bc4d976c3c5433e74a8
Author: Polarian <polarian@polarian.dev>
Date:   Tue Sep 9 13:52:21 2025 +0100

    Update arch dependencies
    
    * unifont was split into pcf-unifont and psf-unifont
    * mipsel packages have dropped the cross prefix
    
    Signed-off-by: Polarian <polarian@polarian.dev>

commit 269fa65b93adc74d61c7e610aa7d75f74897cd68
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 9 11:51:27 2025 +0100

    init.sh: write-protect the lock file on startup
    
    you can still remove it with the -f flag on rm, but
    xbmk only does that on exit from the main parent
    instance, or after each build stage in release.sh
    
    because of this, the user could still manually override
    the lock file; this would cause running instances of lbmk
    to restart wrongly as parent instances.
    
    there's no way to fix any of this, but users don't normally
    put -f in their rm commands.
    
    however, this is also a preventative bug fix. if a bug
    is ever caused in the future, where the lock file is
    created erroneously, the write protection will prevent that,
    so long as *it* is still done.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit dea587a16f9629ceb8e73b3114ba578da7b2be46
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 19:43:08 2025 +0100

    Revert "git/grub: use codeberg as the main mirror"
    
    This reverts commit d06d6a190569574423ff5e60d27ee7f03208dd16.

commit c3e52bc2ee8be6b957c39f3e284f65b018ae8c34
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 19:37:23 2025 +0100

    Revert "change grub git again"
    
    This reverts commit 1e07c4eb02da5c51d5278583b2a3e2de551f2a62.

commit a00f4153c68e92fdaafb2d3e17d88f9cf7aeb168
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 19:12:58 2025 +0100

    Revert "vendor.sh: put _pre_dest in xbtmp, not cache/"
    
    This reverts commit 69934d18cc5fa93cdf5dd24ac36e9cb4b3cd364a.

commit 2873ed5e7eb859efee070366e296057bfd0bb10c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 19:12:27 2025 +0100

    Revert "vendor.sh: put appdir in xbtmp"
    
    This reverts commit 4c74311eae6c25d082735532471da248596182c9.

commit 1e07c4eb02da5c51d5278583b2a3e2de551f2a62
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 18:29:48 2025 +0100

    change grub git again
    
    this time to source hut.
    
    for some reason, *grub* is slow no matter what repo
    provider i host it on??
    
    i tested srht just now, and it seems ok. let's use that.
    
    i'm *paying* for this sourcehut account, so it better be
    good!
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 53491bdca2428378b08e5b8aaf00d339114764f2
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 17:49:12 2025 +0100

    tree.sh: don't combine remkdir/cd gnupath
    
    it's stupid. separate them, to make the code readable.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c4cd9c08df60ee5d62df6db75aab593fc14d6bad
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 17:45:36 2025 +0100

    release.sh: delete xbmkwd on src tarballs
    
    the previously deleted tmp/ directory was a relic
    from prior to recent tmpdir changes.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4c74311eae6c25d082735532471da248596182c9
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 17:33:54 2025 +0100

    vendor.sh: put appdir in xbtmp
    
    do away with redundant variable "vendir"
    
    the "appdir" directory is for files extracted from
    vendor updates, which are then further processed to
    create the real files that we need, such as me.bin
    images processed via me_cleaner.
    
    thus, appdir should go in xbtmp.
    
    the appdir currently clutters vendorfiles/, which is
    not ideal.
    
    we want it to be that the vendorfiles/ directory only
    contains the final firmwares.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a444910bf2f2cd39039116f38439012031b11963
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 17:28:19 2025 +0100

    inject.sh: put tmpromdel in xbtmp, not cache/
    
    cache/ is meant for permanent cached files, not for
    temporary files.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 23fb10c3f5ea4ac434aaa6e9dbab290a7bb6b3f1
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 17:04:58 2025 +0100

    mk: include mrc.sh before inject.sh
    
    and vendor.sh before mrc.sh
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 69934d18cc5fa93cdf5dd24ac36e9cb4b3cd364a
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 15:26:35 2025 +0100

    vendor.sh: put _pre_dest in xbtmp, not cache/
    
    XBMK_CACHE (cache/) is meant for permanently cached
    files, not temporary files.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 4475b4db4b3552866889ea025656bdbf3683032c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 15:24:02 2025 +0100

    init.sh/tree.sh: put PATH dirs in xbtmp
    
    e.g. gnupath, xbmkpath
    
    these currently go in XBMK_CACHE/, which is bad
    because they're meant to be temporary.
    
    XBMK_CACHE is for permanent files.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f5e8483f417db6b36a2ad660faf40475edd5ed1b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 14:14:46 2025 +0100

    init.sh: bail if date is non-GNU (for now)
    
    We currently use GNU-only options in the date command,
    when initialising a Git repository.
    
    This isn't a problem in practise, on non-GNU implementations
    if not initialising a Git repository, because it's only
    used in that situation.
    
    In practise, only those systems with GNU coreutils and libc
    are used to compile releases, so this is OK for me at least.
    
    Future portability improvements will correct the issue, and
    then this error check can be removed.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 882a6917bc4438c7d49c2511b03b634fd25d4f39
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 14:06:57 2025 +0100

    lib/init.sh: sanitize the version string
    
    the release functions in release.sh rely on the
    version string *not* being a path containing slashes.
    just a single string e.g. "foo", not e.g. "foo/bar"
    
    this is because several checks there make that
    assumption. in practise, we always ensure that tags
    and such do not contain these characters.
    
    however, someone else working on their own version
    of xbmk might not know of this design flaw, so let's
    try to correct it in code.
    
    we can add more filtering as designed, in the relevant
    function (xbmk_sanitize_version).
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1c02f2a7704199f5a19c244ccae5b9e5c5b38dd5
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Sep 7 13:35:17 2025 +0100

    release.sh: put vdir in xmtmp, not XBMK_CACHE
    
    XBMK_CACHE is meant for permanent cached files, not
    temporarily files.
    
    the temporary release files are copied upon successful
    return, to their rightful place under release/
    
    this new change also reduces the chance of race
    conditions, if multiple xbmk instances are used; while
    not yet supported as a use-case, this is a goal for a
    future design change.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5096e0040f3c231393fc8a2b2fa2d344de354158
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 5 11:46:16 2025 +0100

    mk: hardened PWD check (deny symlinks)
    
    we check if the first argument is "./mk" and bail if not,
    which forces you to be in the xbmk work directory.
    
    however, this check is flawed because symlinks were still
    possible.
    
    this patch prevents a same-named symlink "mk" pointing to
    the real mk from being used.
    
    this hardening is necessary, due to several built-in
    assumptions inherent within the design of xbmk.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d06d6a190569574423ff5e60d27ee7f03208dd16
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 5 11:16:51 2025 +0100

    git/grub: use codeberg as the main mirror
    
    the gnu one is often really slow, for some reason.
    
    use the official gnu mirror only as a backup.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 92ecf145fa1587f414cc4d0ceeed1e4bab286600
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Sep 5 03:57:07 2025 +0100

    get.sh: put tmpgit in xbtmp
    
    similar to the previous failed patch, which tried to
    also generate it each time, but that led to issues.
    
    this version of the same change merely maintains the
    current hardcoding logic, while putting it in xbtmp.
    
    that way, it's more robustly cleared upon exit from
    the parent instance of xbmk.
    
    this also reduces the chance of race conditions,
    since it's in a unique place each time, rather than
    going in XBMK_CACHE.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ddbefea475719d43996c502275bee1e1341460a2
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 4 15:27:20 2025 +0100

    Revert "get.sh: put tmpclone dirs in xbtmp"
    
    This reverts commit 01a779d4ebcdfc7df406263aeb1dffb800eb0220.
    
    This commit broke ./mk -d coreboot for vendor files in lbmk.

commit aa38608bffe6bcdf8e6f17f4f985044ee91e2da0
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 4 15:27:07 2025 +0100

    Revert "tree.sh: add missing colon at the end of trees()"
    
    This reverts commit 568887cd5efd5df519c7f4f593300eb3ae2beaae.
    
    This commit broke ./mk -d coreboot for vendor files in lbmk.

commit f6d5b447578c6f3cc61e1259631947c0695767a3
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Sep 4 15:26:56 2025 +0100

    Revert "vendor.sh: looser error handling on find_me"
    
    This reverts commit c9a81292e5cfa99de2fdbe20e6a6c9da154c6d7d.
    
    This never caused actual issues. Keep it strict.

commit c9a81292e5cfa99de2fdbe20e6a6c9da154c6d7d
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 07:37:32 2025 +0100

    vendor.sh: looser error handling on find_me
    
    i added a stricter check recently, but this broke
    extraction on fresh lbmk clones, tested when doing
    a release-build test.
    
    loosen it up again, but only for find_me
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 769d645c2aff263603e75da60108698f5fc814e9
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 06:52:35 2025 +0100

    init.sh: create separate lock in release dirs
    
    this removes the current hackiness, preventing
    build errors since xbtmp is now based on xbmkpwd,
    which changes when we're in the release dir.
    
    XBMK_RELEASE is still set accordingly, so this
    will still work the same way.
    
    this is also cleaner in general.
    
    XBMK_CACHE is still the same, so the release work
    directory still re-uses files from the main work
    directory, rather than re-creating them.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 568887cd5efd5df519c7f4f593300eb3ae2beaae
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 06:49:38 2025 +0100

    tree.sh: add missing colon at the end of trees()
    
    this is because when using chained commands at the end
    of functions, sometimes you have to explicitly terminate
    the line.
    
    the way i do it in this patch is common across the
    build system, to mitigate this sh quirk.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 01a779d4ebcdfc7df406263aeb1dffb800eb0220
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 06:21:10 2025 +0100

    get.sh: put tmpclone dirs in xbtmp
    
    and generate them, don't hardcode them - this reduces
    the chance of race conditions, which we have seen in
    the past and which current execution flow in xbmk even
    mitigates in a few places, by doing things in a certain
    order.
    
    this change makes the code more robust and easier to
    maintain.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 0d107ad872b1eb87dcd46e8dc66963696ad08d82
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 06:08:57 2025 +0100

    xbmk: remove xbloc, re-use xbtmp instead
    
    we no longer separate them. xbloc was the on-disk
    tmp directory, whereas xbtmp used to be in /tmp
    which we assumed to be tmpfs (it may not be, but
    often is on many workstation setups - and our
    documentation recommended doing this).
    
    as mentioned in the previous commit, benchmarking
    shows little speed difference using tmpfs /tmp
    versus on-disk /tmp, for our purposes at least.
    therefore, the handling of tmp files is being
    greatly simplified.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 64b69907ababbafc13a0fdaabc3aaf439e02ec86
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 06:02:57 2025 +0100

    init.sh: put TMPDIR inside xbmkpwd, not /tmp
    
    This way, all operations will be done inside the xbmk
    work directory. This is being done, so that I can then
    reliably sandybox certain commands in future commits,
    for example the "rm" command.
    
    This will also allow me to unify the location of all
    temporary files, in future commits. I previously used
    the /tmp directory because it's tmpfs-based on many
    setups, and this is great for performance. However, in
    practise, I never noticed any difference in performance
    when benchmarking it (testing /tmp on-disk versus tmpfs).
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 8a8be1dec99f37932a2bedea7cc0427ea90903e6
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 04:02:49 2025 +0100

    get.sh: remove unnecessary variable, repofail
    
    it is entirely unused
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 3af4c7bcb1f154a9a2611e547f6c79275e19bd85
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 03:35:48 2025 +0100

    rom.sh: simplify rmodtool copy handling
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit bec4c63b25c5e9a2330c4ff283ff4df06afcd07b
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 03:31:34 2025 +0100

    release.sh: remove support for the -d flag
    
    this lets you change the directory for outputted
    release files, versus the default "release" directory.
    
    this code is buggy, because it could let you overwrite
    a part of xbmk or worse - and checking for such bad
    usage would require a lot more code.
    
    knobs are for nobs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e05961458953fdf8d55ddd871663a452ef0e8d43
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 03:22:12 2025 +0100

    get.sh xbget: don't use eval for file/dir checks
    
    the actual code works fine, but it's quite hacky.
    
    there are times when use of eval is acceptable; this
    is not one of those times, but i'd used it in this
    instance when i was being a bit crazy about code size
    reductions during my audits.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 934e89510e1e30ab6f8eb40e6cbebd2cfd9b0bf0
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Sep 2 00:19:15 2025 +0100

    vendor.sh: remove unnecessary error check
    
    setting a variable in this way will never result in
    an error. this is a relic from a prior re-factoring
    versus older versions of the code.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b1f485d0f2897b18272bdf504f8476ce5efcd7dc
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Sep 1 08:46:20 2025 +0100

    release.sh: fix broken release lock file handling
    
    we need to copy the main lock file, rather than creating
    a new, empty one. this is because the new lock file
    handling requires it, and the release lock file will
    be used during release builds.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ed84d33e59b8b2cf1789eb591c592d921907dbdd
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Sep 1 08:14:49 2025 +0100

    lib.sh and rom.sh: stricter mktemp usage
    
    error out under fault condition
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 40f064ae336f125762584be8a6ddd52d7ff1cd01
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Sep 1 07:08:47 2025 +0100

    inject/vendor.sh: stricter set + usage
    
    set - as soon as possible, for example in the extract_me
    function.
    
    we only turn off error handling when certain error-prone
    tasks are performed, and mitigations are in place after
    these commands run to make sure that the result was valid.
    
    this is because in some cases, we want certain buggy behaviours
    to be permitted, with errors handled in a more fine-tuned way,
    because sh can sometimes be much stricter depending on the
    implementation; otherwise, we almost always rely on -e -u in
    most of the build system.
    
    this mainly affects the vendorfile insertion logic.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 24a8226fee0eb320c7b16da94971402b42a7ce87
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 21:37:30 2025 +0100

    init.sh: tidy up xbmk_set_env
    
    this is a general function that sets variables,
    but there are many types of variables to be set.
    
    rather than have all the logic inside this function,
    handle it in subfunctions called by xbmk_set_env.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7c04cd37b52afe971cafcb607f07428c46ea9b24
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 21:33:00 2025 +0100

    init.sh: tidy up xbmk_child_set_tmp
    
    the checks of xbmk cache/threads is unrelated.
    
    this has been moved back to the calling function.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2d20be3d99b260e55798866292c46340e8f183f9
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 21:30:29 2025 +0100

    init.sh: remove unnecessary lockfile checks
    
    we don't need these anymore, because we now know
    whether or not the lock file exists in these cases.
    
    this is because child/parent instance determination
    is now done based on the presence of that file, rather
    than how TMPDIR is set; and TMPDIR is now set accordingly,
    via more robust logic as in previous patching.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c148b220d852db1d9a0037f06dc655bd93488925
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 21:27:51 2025 +0100

    init.sh: move TMPDIR handling to own functions
    
    this makes xbmk_set_env easier to read
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f96bf2b2cd32f05a0e7b7c4f7bbeaefe5dea246b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 20:39:07 2025 +0100

    init.sh: prevent race condition with TMPDIR
    
    it is extremely unlikely to occur, but this patch reduces
    the likelihood even further. that unlikely occurance is:
    
    when creating a TMPDIR, it's possible that it was already
    created before. this is OK on child instances, where that
    is the intended behaviour (unified TMPDIR), but not for
    parent instances.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit bbce74d78af9ef871bff52f44638eda57a26973e
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 20:03:34 2025 +0100

    init.sh: MUCH safer TMPDIR handling
    
    we previously checked whether xbmk was running a child
    instance, based on the initialisation of TMPDIR, but
    this relied on unreliable string substitutions, which
    could not be made inherently reliable. there were also
    no checks on whether the given TMPDIR, even if correct,
    was a directory or whether it was a symlink; there were
    also no checks on whether it changed.
    
    now with this change, child instances are detected by
    the presence of the lock file. the parent instance
    writes the generated TMPDIR location in that file, and
    this is checked again in the child instance, to ensure
    that the TMPDIR didn't change; it also errors out if
    the TMPDIR doesn't exist or if it is a symlink.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ad58364e189d90284e8bc888ec1bf1b555768c47
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 31 18:27:09 2025 +0100

    mk: simplify the main script check
    
    it's still not perfect, but now it's unambiguous.
    
    the previous generic check was written based on the
    fact that xbmk's main script used to also be called
    via several symlinks, which is no longer the case.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 09646783a5b352d5d7ad166d01db9e0536a56ca5
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 30 07:22:38 2025 +0100

    config/dependencies: add --no-install-recommends
    
    use this on the debian dependencies, otherwise it installs
    a bunch of extra crap e.g. xorg crap, in some circumstances,
    which someone probably won't want when they're in a minimal
    chroot or something.
    
    reported by ron nazarov. thank you ron!
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d9011da0eb26744b64b8f0161d6f39b145b0af60
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Aug 29 04:44:52 2025 +0100

    inject.sh: redirect stderer to /dev/null FIRST
    
    for the grep command, we must ensure that errors are
    suppressed *BEFORE* outputting to a file. depending
    on the sh implementation, the previous code might
    have begun outputting to a file before suppressing
    errors.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 50a0bac01ce23a569fa7bfea3a8553173aa61448
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Aug 26 07:01:35 2025 +0100

    vendor.sh: tidy up extract_intel_me
    
    too many chained commands. break it out a bit.
    
    this makes it more readable, without changing behaviour.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9fb707b49bbf9e56a850562b09f37fcf017b078c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 18:31:33 2025 +0100

    rom.sh: safer use of cat in copyps1bios
    
    the output to a file also has its own error handling,
    but x_ can be used safely to provide additional assurance
    that the script will break if an error occurs.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 89238c057928376c9ebee92c1a5a2976ef8de33c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 17:38:29 2025 +0100

    tree.sh: tidy up check_cross_compiler
    
    group related operations together, without whitespace.
    
    declare all variables at the start of the function.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 07562e3f281492cb040072e78468114d1a960bee
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 16:02:40 2025 +0100

    tree.sh: don't re-check xgcc needlessly
    
    instead, create a file indicating that a given xgcc
    target had already been built successfully, within a
    given coreboot tree.
    
    this will considerably speed up the building of release
    archives, especially when there are a lot of boards.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 66f1be1ba77b44bfae29beb7678fb0598ede8030
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 15:49:10 2025 +0100

    tree.sh: check xgcc AFTER checking elfdir
    
    if e.g. elf/coreboot/default/w500_16mb contains readied
    images from before, crossgcc is still being checked.
    
    if you already built all the coreboot images, and wanted
    to just modify all the payloads for example, this would
    result in a much slower re-build process, because it is
    needlessly re-checking crossgcc every time.
    
    by doing it this way, we need up the testing of payloads
    quite considerably, during xbmk development.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit c12965f8e4205cc28d9f468891cd20330b28c53c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 15:35:47 2025 +0100

    lib.sh: safer pad_one_byte function
    
    instead of copying to a temp file and then
    concatenating with padding back to the main
    file, we concatenate and create the temp file,
    then move the temp file back to the main file.
    
    this is because cat can be quite error prone,
    more so than mv, so this will reduce the chance
    of corrupt files being left behind depending
    on the context (of course, the latter is often
    avoided due to xbmk's design, which emphasises
    use of temporary files first).
    
    this matches the same design used in the function
    unpad_one_byte, which creates the deconcatenated
    output in a temporary file first, moving it back.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7e6f52ec0a7a4106378191d352a862c0a03e71c4
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 03:59:35 2025 +0100

    vendor.sh: use pad/unpad functions
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 616ef52a6f892a9a9021f90bbd0f48fad5253f71
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 23 03:48:27 2025 +0100

    lib.sh: additional error handling on cat
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 41945a222005d94e0e51adb1ba8ceb869b110bae
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 10 14:50:32 2025 +0100

    tree.sh: Delete files *before* updating hashes
    
    The current logic deletes old project files e.g. sources,
    but *after* updating the project hash.
    
    This means that if a deletion fails, and the directory
    is still there (e.g. src/coreboot/default/) afterward, it's
    now a tainted archive, yet the hash has been updated, so
    subsequent runs of the build system will cause unknown
    errors.
    
    This patch fixes that, by first copying the new hash to
    a temporary file. *Then*, deletions are handled, and the
    final hash file is updated afterward.
    
    The code is now a bit more bloated as a result, but this will
    reduce the risk of tainted sources being handled under fault
    conditions.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit d44c143846c15ba2032340db085f2ec31365182b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 10 14:17:57 2025 +0100

    tree.sh: rename hashtype to hashname
    
    since it's the name, e.g. "default", referring to a
    project tree (in this example, coreboot/default).
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 289c4e1c2fd4dfc7e396da37eb30b15dbcb33b8e
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 10 14:15:55 2025 +0100

    tree.sh: rename hashname to hashdir
    
    since it's only ever used as a directory name
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit b31f2387ee13c243ee7bdc3e193d91fc1088d8bb
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 10 14:13:47 2025 +0100

    tree.sh: rename hashvar to badhashvar
    
    now the code that uses it makes a bit more sense
    to the casual reader.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 2c24b94d80350b11ab34ef783c3822af6b24e1c8
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Aug 10 14:12:48 2025 +0100

    tree.sh: rename function and remove comments
    
    the new function names make the comments redundant. the
    code is now self-explanatory.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ef79b1108294cad62d90d02a598c18b64f1632db
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Aug 4 09:18:04 2025 +0100

    tree.sh: don't delete builds if tree==target
    
    in that case, the previous tree-wide check will cover
    it, so the current logic wastes computational time.
    
    this patch therefore somewhat optimises the code.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1c5c28f2cb2d2e553a248631fb27f45ecab6e958
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Aug 4 09:16:58 2025 +0100

    tree.sh: re-add comments to check_hashes
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 3d5a6bccae629918de943b3d13fbc615a37821f2
Author: Leah Rowe <leah@libreboot.org>
Date:   Sat Aug 2 11:12:49 2025 +0100

    tree.sh: unified project hash handling
    
    the target/project hash checks are basically identical,
    so let's unify them under a single function.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a499d5bba2f9f35dd10c0bab4b9c2313f744da33
Author: Leah Rowe <leah@libreboot.org>
Date:   Fri Aug 1 05:27:33 2025 +0100

    rom.sh: Don't run mkhelpers if release=n
    
    This fixes the following error on ./mk release:
    
    cp: cannot stat 'elf/coreboot/default/d510mo/libgfxinit_txtmode/coreboot.rom': No such file or directory
    
    I recently re-wrote the handling of coreboot images, and
    I overlooked this entirely. When a given target specifies
    release=n, it has to be skipped, so builds are not done.
    
    The "release" variable is already checked, in mkcoreboottar.
    
    Let's also put it in the other mkhelper functions, to make sure
    there are no errors.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 84a1ff85b0706d7eb47118dd37f0f16443d48108
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 20 04:44:14 2025 +0100

    coreboot/default: rev 9e41c7cec7, 18 July 2025
    
    T480/T480s patches were dropped since they're included as
    part of the upstream code now.
    
    This update brings the following upstream changes:
    
    * 9e41c7cec7 soc/intel/cmn/block/fast_spi: Lock DMA before exiting coreboot
    * c1d45ef93b mb/google/trulo/var/kaladin: Update touchpad settings
    * f13f980e03 mb/google/trulo/var/kaladin: Add fw_config probe for storage
    * 50c39b3a22 mb/google/trulo/var/kaladin: Fix Type C function
    * f0d50aa404 commonlib/include/commonlib: Add volatile qualifier
    * 3828153ea5 soc/intel/xeon_sp/gnr: Use official microcodes
    * a87cbcd3c9 soc/intel/xeon_sp/ibl: Config ACPI base using PMC device
    * 480ac15044 util/cbfstool: Prevent overflow when sorting fit table entries
    * bf4f08f3b6 mb/hp/snb_ivb_desktops/variants/compaq_8300_elite_sff: early VGA output
    * dd19f6bc5a util/cbmem: Extract devmem and common code to separate files
    * def945f3ba soc/intel/apollolake: Measure the IBBL, IBB and OBB from the bootblock
    * fbb0738272 mb/google/brox/var/lotso: Decrease cpu power limits
    * ce88b12420 mb/google/ocelot: Set correct TPM I2C bus for all ocelot model variants
    * e050e2fbfc mb/google/ocelot/var/ocelot: Remove irrelevant comment
    * b66c8ea3d3 mb/google/ocelot/var/ocelot: Remove Bluetooth Audio offload
    * d5d633f607 mb/google/ocelot/var/ocelot: Update variant.c
    * 3b069d320c cbfs: Add a function to wait for all CBFS preload operations to complete
    * a7710ed8fd Documentation: coding_style: Add *long* to long multi-line comment example
    * 19d7104d85 drivers/intel/touch: Use recommended short multi-line comment style
    * 451988d015 mb/google/trulo/var/pujjolo: Fix Goodix touchscreen function
    * 542e52c126 soc/qualcomm/x1p42100: Optimize memory layout for X1P42100
    * 2e47bd50f2 mb/google/trulo/var/pujjocento: Add 6W and 15W DPTF parameters
    * 6e4f4538bb soc/intel/{tgl,adl,mtl,ptl}: Default to Software Connection Manager
    * 1b8dd662a9 soc/qualcomm/x1p42100: Add PCIE Clock support for x1p42100
    * 4d3def7514 soc/mediatek/mt8189: Fix timer reset in BL31 by using time_prepare_v2
    * d898653b0e soc/meidatek/mt8196: Extract common timer code for reuse
    * d1c096a5b9 src/soc/mt8196: Correct systimer register offset
    * edaa67d0c9 mb/google/skywalker: Add thermal init flow in romstage
    * 6aec09875b soc/mediatek/mt8189: Add thermal driver
    * 5cc4b9e6ce soc/amd/common/cpu/noncar: Add bootblock overlap detection
    * 67cd138df9 soc/intel/apollolake: Add missing header in measured_boot.h
    * a428481574 mb/google/nissa/var/dirks: Update power limits
    * 55ae0d8a37 mb/google/nissa/var/baseboard/nissa: Add power limits functions
    * 82163aedc6 soc/amd/common/block/cpu/noncar: Move BSS and DATA out of PT_LOAD
    * 6405641647 mb/google/fatcat: Use same mainboard part number for all fatcat variants
    * c5613469ae device: Make a note that SeaBIOS doesn't support above 4G MMIO
    * ced4c09359 soc/intel/xeon_sp/gnr: Implement get_mmio_high_base_size
    * 7100f226ca vc/intel/fsp/fsp2_0/wcl: Add FSP headers for WCL FSP
    * 5171098814 drivers/qemu/bochs: Allow building for non-x86 architectures
    * d233b6c903 payloads/external/LinuxBoot/Makefile: Fix build prerequisite
    * 502d19be89 payloads/external/LinuxBoot/targets/u-root.mk: Add missing prerequisite
    * cba0f0b8b9 payloads/external/LinuxBoot: Rename build target
    * 43a54e3b1b util/amdfwtool: Add binary parsing
    * 85da3954d0 .gitmodules: Ignore changes make by what-jenkins-does
    * 397c5fe420 Documentation: Add a mainboard entry for the Lenovo T480/T480s
    * 6768586353 Documentation: Add information about the deguard utility
    * ad8b738af0 mb/lenovo: Add ThinkPad T480 and ThinkPad T480s
    * 96e381766e ec/lenovo: Add support for MEC1653 EC
    * 2181b02765 util/smmstoretool: Properly initialise the authenticated variable header
    * 3058464263 util/smmstoretool: Add support for creating variable from file contents
    * b49f567e45 util/smmstoretool: Ensure that the FVB header isn't too large
    * a6fbaa47ea util/smmstoretool: Clarify the `auth_vars` field
    * 3698517d82 mb/amd: Use mec152x tool
    * 5a0953614b util/amdtools: Add ec_usb_pd_fw
    * e63620012c util: Add Microchip EC FW tool
    * 0b5ce9d9f0 soc/intel/apollolake: Add support for IFWI Measured Boot
    * 289cff3423 soc/intel/apollolake: Load the IBB into CAR
    * 2408695dd3 soc/intel/apollolake: Add a loader for the IBB
    * 61b66e9a81 soc/intel/apollolake: Add function to clear MCA in Bank 4
    * 138402e7ff soc/intel/apollolake: Create IBB, IBBL and OBB
    * 61b4e1983c mb/google/fatcat: Update PCH reset power cycle duration to 1 second
    * e9af95d5ab soc/intel/pantherlake: Configure FSP UPDs for minimum assertion widths
    * 79bd154b49 drivers/genesyslogic/gl9763e: Mask replay timer timeout of AER
    * a775bfc2b2 soc/mediatek/mt8189: Specify MTKLIB_PATH for building BL31
    * e583b2ffb7 soc/meidatek/mt8196: Extract common thermal code for reuse
    * f62734976c mb/dell: Convert E6400 into a variant
    * 8d60bf9975 mb/google/fatcat: select MIPI pre-prod if PTL pre-prod SoC is set
    * 2f978ecab3 mb/google/fatcat: Choose platforms with pre-prod Panther Lake SoC
    * eb1483ba17 soc/mediatek/mt8189: Increase SCP clock frequency from 26MHz to 416MHz
    * 9c5557f982 util/abuild: Add --sequential-boards option
    * 9e5234feee payloads/external/edk2: Drop our toolchain override
    * 8d9e18a122 payloads/edk2: Indicate whether edk2-platforms is available
    * 626fd50a94 mb/google/fatcat/var/kinmen: Enable ISH
    * e7cefe4f41 soc/mediatek/mt8196: Move srclken_rc related code to common
    * e9731f8925 soc/intel/pantherlake: Add configs for pre-production silicon
    * 8687b3d108 mb/google/trulo/var/pujjolo: Add ISH firmware config
    * 722c9314c7 mb/google/dedede/var/awasuki: Add 2 HYNIX modules to RAM id table
    * 6082bd7711 ec/lenovo/h8: Rework invalid temperature reporting
    * 621b1061d0 ec/lenovo/h8: Add Kconfig to select use of Thermal Zone 1
    * bc116b8797 ec/lenovo/h8: Replace chip regs for BT/WWAN detect with Kconfig options
    * d9169ef617 ec/lenovo/pmh7: Add CFR objects for existing options
    * 45d9973a6d ec/lenovo/h8: Add CFR objects for existing options
    * ce5a1e8a51 mb/google/brox: Create caboc variant
    * d745d38393 soc/intel/cmn/block/fast_spi: Add DMA support
    * 8e666c367d soc/qualcomm/x1p42100: Update boot critical firmware memory layout
    * e35c784847 Doc/gfx/libgfxinit.md: Fix file names in source code references
    * 0e682859e7 payloads/external/U-Boot: Upgrade from 2024.07 to v2025.07
    * 8b52167a9f arch/x86: Add support for cooperative multitasking on x86_64
    * 569b7a8861 Docs/releases: Finalize 25.06 release notes
    * 5db8bf0cfa mb/trulo/var/pujjolo: Enable USB3 WWAN device
    * e013c9586c mb/trulo/var/pujjolo: Modify mipi camera parameters
    * 7b8520ab69 mb/trulo/var/pujjolo: Update fingerprint enable pin status
    * f74027d5ae mb/google/nissa/var/craask: Add elan touchscreen support
    * 396a883a0c mb/hp/snb_ivb_desktops: Include PS/2 controller ASL code for MS Windows
    * 18c067d392 mb/google/fatcat/var/kinmen: Add Synaptics touchpad
    * 2f5b384ba5 soc/mediatek/mt8189: Enable EARLY_MMU_INIT to improve boot time
    * d5bce8c420 mb/hp: Add HP 260 G1 DM Business PC (Haswell)
    * 48c6f66fa4 mb/google/ocelot: Update TPM_TIS_ACPI_INTERRUPT value in Kconfig
    * 0660fe50de mb/google/ocelot: Update GPE configuration
    * 5b3063802e mb/google/fatcat/var/kinmen: Fix touchscreen IRQ setting
    * 6c4e502fdd mb/google/nissa/var/pujjocento: Reduce PL4 to 38W with no battery
    * 6e92554ab6 mb/trulo/var/pujjolo: Modify FW_CONFIG for mipi camera
    * 4f5f75da34 mb/trulo/var/pujjolo: Correct USB3 Type-A OC pins
    * a1dfd39e04 mb/google/fatcat/var/kinmen: Add AUDIO_UNKNOWN and probe for ALC721
    * 306544b427 mb/google/fatcat/var/francka: Add AUDIO_UNKNOWN and audio probes
    * edf47d44cd mb/google/fatcat/var/fatcat: Disable Audio for invalid Audio FW_CONFIG
    * 454079c3bc lib/cbfs: Ensure cache buffer alignment in ramstage
    * 0ef670a66a mb/google/ocelot/var/ocelot: Configure FPS related changes
    * 6ab37f0e0e mb/google/ocelot/var/ocelot: Add FW_CONFIG for Finger Print
    * 3f61df24d5 mb/google/ocelot/var/ocelot: Add FW_CONFIG for Storage
    * bb95a26cda mb/google/ocelot/var/ocelot: Add FW_CONFIG for WiFi
    * 410b3c697f mb/google/ocelot/var/ocelot: Add FW_CONIG for ISH
    * afaf4c3d7b mb/google/brya/variants/pujjolo: Update ISH GPIOs and add ISH firmware name
    * f6de6f8933 mb/google/fatcat: Drop redundant SNDW GPIO mapping
    * 584fdd6572 soc/mediatek/mt8196: Remove redundant bootblock.c from Makefile.mk
    * 24ea6937f2 soc/intel/apollolake: Add the Kconfig options for IFWI Boot Profile
    * c68645cd88 util/supermicro: Fix mem leak in get_line_as_int error conditions
    * 05396238da libpayload/drivers: Fix mem-leak in cbmem_console error condition
    * 1219981177 drivers/emu/qemu: Add a comment about fw_cfg assumptions
    * d866e72b3a mb/google/fatcat/var/kinmen: Set CRFP to use GPIO for status
    * 4367daae20 drivers/spi: Add option to generate proper PowerResource _STA
    * 03c331399c mb/google/nissa/var/craask: Add focaltech touchscreen support
    * b3d7c40fb5 mb/siemens/mc_rpl: Remove code for board_id
    * 5de16ed1b8 mb/siemens/mc_rpl: Remove unused embedded controller code
    * a1067ec6de mb/siemens/mc_rpl: Remove unneeded code to select a VBT name in CBFS
    * 463cda84d2 mb/siemens/mc_rpl: Remove unused Type-C data definition
    * dcbe591201 mb/siemens/mc_rpl: Use SPD data from HWInfo instead of from CBFS
    * 6c059f8af3 IVB mainboards: Drop 1024M option for gfx_uma_size
    * 3b61dbaa06 mb/asus/p8z77-m_pro: Remove incorrect gfx_uma_size options
    * 2b7115b139 mb/hp/snb_ivb_desktops: Add gfx_uma_size options up to 512MB
    * d99769bbde mb/hp/snb_ivb_desktops/variants: enable 4th sata port on tested models
    * 95784dbafb mb/google/ocelot/var/ocelot: Add FW_CONFIG for Audio
    * f323adb19f soc/mediatek/mt8189: Increase SPI NOR clock rate from 26MHz to 52MHz
    * 689af47b52 commonlib: Add pvmfw related timestamps
    * f1d06a5ad4 soc/intel/common/block/memory: Provide a way to use SPD data from memory
    * 11b1dc0a97 Reapply "util/cbmem: Consolidate CBMEM and coreboot table access"
    * 13f1c6118e Documentation: Update cbmem.md with more information
    * 07267d19ce arch/x86/postcar_loader: Add comment line for reloc_params assignment
    * e94ac6e655 mb/google/nissa/var/pujjocento: Reduce PL4 to 38 W with no battery
    * 2eaec1b53a sbom: Fix build with merged bootblock and romstage
    * 267f08dafd MAINTAINERS: Add KunYi Chen as maintainer for LattePanda Mu
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit cc2f08e7bbf9fc32bdd37079ee93b477ef4e8d09
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Jul 14 11:18:42 2025 +0100

    Revert "coreboot: rev 8b52167a9f 13 July 2025, rebase t480"
    
    This reverts commit 32dfdfbb01b84bb9c058e9b84305f15a14d4e231.
    
    The update caused an issue on T480:
    
    Backlight comes on, then off, then on, then off, repeatedly, and
    never gets to the payload. Will have to investigate further.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 32dfdfbb01b84bb9c058e9b84305f15a14d4e231
Author: Leah Rowe <leah@libreboot.org>
Date:   Mon Jul 14 01:48:35 2025 +0100

    coreboot: rev 8b52167a9f 13 July 2025, rebase t480
    
    coreboot/default: update t480 patches to set 38
    
    see: https://review.coreboot.org/c/coreboot/+/83274/38
    
    I was previously using:
    https://review.coreboot.org/c/coreboot/+/83274/25
    
    Matt DeViller aka MrChromeBox, recently took over the
    patch set, tidying up and re-factoring the code so that
    it's more suitable for upstream. Several hacky behaviours
    were removed, for example the MEC1663 code is now its own
    code in coreboot, rather than being bolted onto the H8s code.
    
    Certain T480-specific changes made to global parts of the
    coreboot code are now done only on the tree itself.
    
    Mate Kukri has also tested Matt's recent updates. More
    testing still needed on Nvidia dGPU models, which never
    worked before anyway; Intel GPU models should still work.
    
    Thermas zone handling is also improved. See patch:
    https://review.coreboot.org/c/coreboot/+/88415/1
    https://review.coreboot.org/c/coreboot/+/88416/2
    
    Functionally, this is mostly the same as before. As I said,
    Matt has focused on code cleanup, so that the board can be
    properly upstreamed. Hopefully this will be merged soon,
    in coreboot-main.
    
    Besides this, the following upstream changes were imported:
    
    * 8b52167a9f arch/x86: Add support for cooperative multitasking on x86_64
    * 569b7a8861 Docs/releases: Finalize 25.06 release notes
    * 5db8bf0cfa mb/trulo/var/pujjolo: Enable USB3 WWAN device
    * e013c9586c mb/trulo/var/pujjolo: Modify mipi camera parameters
    * 7b8520ab69 mb/trulo/var/pujjolo: Update fingerprint enable pin status
    * f74027d5ae mb/google/nissa/var/craask: Add elan touchscreen support
    * 396a883a0c mb/hp/snb_ivb_desktops: Include PS/2 controller ASL code for MS Windows
    * 18c067d392 mb/google/fatcat/var/kinmen: Add Synaptics touchpad
    * 2f5b384ba5 soc/mediatek/mt8189: Enable EARLY_MMU_INIT to improve boot time
    * d5bce8c420 mb/hp: Add HP 260 G1 DM Business PC (Haswell)
    * 48c6f66fa4 mb/google/ocelot: Update TPM_TIS_ACPI_INTERRUPT value in Kconfig
    * 0660fe50de mb/google/ocelot: Update GPE configuration
    * 5b3063802e mb/google/fatcat/var/kinmen: Fix touchscreen IRQ setting
    * 6c4e502fdd mb/google/nissa/var/pujjocento: Reduce PL4 to 38W with no battery
    * 6e92554ab6 mb/trulo/var/pujjolo: Modify FW_CONFIG for mipi camera
    * 4f5f75da34 mb/trulo/var/pujjolo: Correct USB3 Type-A OC pins
    * a1dfd39e04 mb/google/fatcat/var/kinmen: Add AUDIO_UNKNOWN and probe for ALC721
    * 306544b427 mb/google/fatcat/var/francka: Add AUDIO_UNKNOWN and audio probes
    * edf47d44cd mb/google/fatcat/var/fatcat: Disable Audio for invalid Audio FW_CONFIG
    * 454079c3bc lib/cbfs: Ensure cache buffer alignment in ramstage
    * 0ef670a66a mb/google/ocelot/var/ocelot: Configure FPS related changes
    * 6ab37f0e0e mb/google/ocelot/var/ocelot: Add FW_CONFIG for Finger Print
    * 3f61df24d5 mb/google/ocelot/var/ocelot: Add FW_CONFIG for Storage
    * bb95a26cda mb/google/ocelot/var/ocelot: Add FW_CONFIG for WiFi
    * 410b3c697f mb/google/ocelot/var/ocelot: Add FW_CONIG for ISH
    * afaf4c3d7b mb/google/brya/variants/pujjolo: Update ISH GPIOs and add ISH firmware name
    * f6de6f8933 mb/google/fatcat: Drop redundant SNDW GPIO mapping
    * 584fdd6572 soc/mediatek/mt8196: Remove redundant bootblock.c from Makefile.mk
    * 24ea6937f2 soc/intel/apollolake: Add the Kconfig options for IFWI Boot Profile
    * c68645cd88 util/supermicro: Fix mem leak in get_line_as_int error conditions
    * 05396238da libpayload/drivers: Fix mem-leak in cbmem_console error condition
    * 1219981177 drivers/emu/qemu: Add a comment about fw_cfg assumptions
    * d866e72b3a mb/google/fatcat/var/kinmen: Set CRFP to use GPIO for status
    * 4367daae20 drivers/spi: Add option to generate proper PowerResource _STA
    * 03c331399c mb/google/nissa/var/craask: Add focaltech touchscreen support
    * b3d7c40fb5 mb/siemens/mc_rpl: Remove code for board_id
    * 5de16ed1b8 mb/siemens/mc_rpl: Remove unused embedded controller code
    * a1067ec6de mb/siemens/mc_rpl: Remove unneeded code to select a VBT name in CBFS
    * 463cda84d2 mb/siemens/mc_rpl: Remove unused Type-C data definition
    * dcbe591201 mb/siemens/mc_rpl: Use SPD data from HWInfo instead of from CBFS
    * 6c059f8af3 IVB mainboards: Drop 1024M option for gfx_uma_size
    * 3b61dbaa06 mb/asus/p8z77-m_pro: Remove incorrect gfx_uma_size options
    * 2b7115b139 mb/hp/snb_ivb_desktops: Add gfx_uma_size options up to 512MB
    * d99769bbde mb/hp/snb_ivb_desktops/variants: enable 4th sata port on tested models
    * 95784dbafb mb/google/ocelot/var/ocelot: Add FW_CONFIG for Audio
    * f323adb19f soc/mediatek/mt8189: Increase SPI NOR clock rate from 26MHz to 52MHz
    * 689af47b52 commonlib: Add pvmfw related timestamps
    * f1d06a5ad4 soc/intel/common/block/memory: Provide a way to use SPD data from memory
    * 11b1dc0a97 Reapply "util/cbmem: Consolidate CBMEM and coreboot table access"
    * 13f1c6118e Documentation: Update cbmem.md with more information
    * 07267d19ce arch/x86/postcar_loader: Add comment line for reloc_params assignment
    * e94ac6e655 mb/google/nissa/var/pujjocento: Reduce PL4 to 38 W with no battery
    * 2eaec1b53a sbom: Fix build with merged bootblock and romstage
    * 267f08dafd MAINTAINERS: Add KunYi Chen as maintainer for LattePanda Mu
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 122d009af149b3af83176fcf77a962bdc8b52d64
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 13 15:21:46 2025 +0100

    coreboot/default: probe EDID twice in libgfxinit
    
    this mitigates buggy video converters e.g. displayport
    to hdmi, where sometimes the display doesn't come up.
    
    sometimes you have to probe them twice. this is apparently
    what linux does, according to nicholas chin's interpretation.
    
    this is a really quick and dirty patch that worked for
    Noisytoot on IRC, tested on their Dell OptiPlex 5050 SFF
    which they are porting; the port otherwise works, and this
    patch enables them to use their displayport adapter.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit dac3d6d06aa00cbf764a60494761f74dd043a310
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Jul 10 10:41:31 2025 +0100

    rom.sh: Don't build coreboot utils if dry=":"
    
    This fixes a build error when doing ./mk release, after
    a regression caused by the last few commits.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 6f7525a5b35e373dc4496cd4c6af4e61cb3b534c
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Jul 10 03:15:18 2025 +0100

    cache coreboot builds in elf/ again
    
    This was a problem when I did it before, because individual
    target builds weren't automatically re-compiled when needed.
    
    The recent design improvements in lbmk enable this to be
    done again.
    
    Cached images in elf/ have no payloads, so they are a liability,
    therefore they are padded by one byte to prevent flashing. This
    solves the problem that the previous caching had.
    
    With this change, modifying payloads can be tested without
    needing to re-build coreboot each time.
    
    The cached coreboot builds are also automatically re-built when
    needed, which is another improvement this time, compared to
    the last time coreboot builds were cached in this manner.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 7c6c9ff5470c049d055133776a4e7baee8c842fd
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Jul 10 01:46:43 2025 +0100

    tree.sh: delete individual target builds if needed
    
    Detect when a config changes. This is done even if the
    entire tree doesn't change.
    
    This is already done per-tree if files change, but
    individual project files don't change.
    
    For example, if a grub.cfg changes, the given cached
    build for that GRUB tree isn't deleted. Same thing if
    a given U-Boot config doesn't change.
    
    This patch fixes a longstanding design flaw of lbmk,
    making auto-re-builds more reliable. This complements
    another recent change, that deletes all target builds
    of a given tree when the tree changes.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit fb95230a4c343647e96c58f79b142ccc46e620af
Author: Leah Rowe <leah@libreboot.org>
Date:   Thu Jul 10 01:07:55 2025 +0100

    tree.sh: Remove redundant deletion
    
    Target builds go inside a common directory for
    the given tree now, which gets deleted, thus
    deleting all target builds of that given tree.
    
    Therefore, the deletion being removed is redundant.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit e25bca2ef7798c0454130ae3ea85f37d8a2d87bd
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Jul 8 17:57:53 2025 +0100

    T480/T480: Drop redundant PcieRpEnable from dt
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit cc82b733d3dbf5f91badede6daf88ba724a0d9d8
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Jul 8 13:20:28 2025 +0100

    tree.sh: Place target builds under tree/target/
    
    as opposed to target/
    
    for example:
    
    image the command:
    
    ./mk -b u-boot amd64coreboot
    
    This would put the U-Boot binaries here:
    elf/u-boot/amd64coreboot/default/
    
    With this change, they now go here:
    
    elf/u-boot/x86_64/amd64coreboot/default/
    
    This solves a problem that existed previously, where
    you could modify a given tree in a multi-tree project,
    but cached builds for targets branching separately off
    of each tree would not be deleted, and thus not re-built.
    
    This accomplishes such a result, without needing to
    further check hashes of individual targets.
    
    The latter will still be done, in a future change, because
    this change doesn't fix another problem:
    
    If you change a given config, e.g. targetname "foo" which
    uses tree "bar", elf/foo/ would not be removed automatically
    for re-build.
    
    So this change only deletes individual target builds when
    their master tree changes.
    
    Where the target and tree are the same, this also means
    elf/tree/target/
    
    for example: seabios/default would create binaries in:
    
    elf/seabios/default/default/
    
    not:
    
    elf/seabios/default/
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 9541dfcefae0e5f4262dc965e4f109d73c505a78
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Jul 8 10:39:54 2025 +0100

    rom.sh: bump pcsx-redux copyright date to 2025
    
    it's 2019-2025 now, not 2019-2024, because i recently imported
    new pcsx-redux upstream changes that go up to June 2025.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 91a63ccd1faac0431d8717942d6da8a57846f370
Author: Leah Rowe <leah@libreboot.org>
Date:   Tue Jul 8 01:59:13 2025 +0100

    hppro3500: enlarge CBFS to match the BIOS region
    
    i enlarged the BIOS region in a previos commit, but I forgot
    to enlarge CBFS. it's the policy of lbmk to enlarge CBFS when
    possible, after applying a truncated ME configuration.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit ed839db0a06b6ac05054994c25f8343565a1c184
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 21:07:19 2025 +0100

    pcsx-redux: bump rev 8f8cc3d5, 20 June 2025
    
    Many other changes were imported into the wider pcsx-redux
    tree, but we're mainly concerned with the OpenBIOS diffs.
    
    This update brings in the following upstream changes, for
    PCSX-Redux OpenBIOS:
    
    * 35de25bb Fixing realloc's edge case.
    * b8a9080d OpenBIOS: Annotate sio0/driver.c with enums
    * c7cec91e OpenBIOS: Refactor card driver
    * 4e42a6b6 Move OpenBIOS SIO to a seperate header and add enums
    * a50434c5 Remove OpenBIOS dead sio1 code
    * 9c3d3a1e Renaming readAligned to load32Aligned.
    * 1b8312e5 [Chores] Format code
    * 8b9df484 Simplifying openbios allocation scenario.
    * a658a18d Brand new memory allocator.
    * ba48f01b Bumped copyright date to 2025
    * 64b63a13 Bumped copyright date to 2025
    * 3ada28e3 [Chores] Format code
    * d25af104 Fixing setjmp/longjmp attributes.
    * e51ffafa Assign _bu_init alias to 0x55.
    * ae1dd51e Split out the common thread structures to its own file.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit dd65f55b29546b25889f45ba5dbb5b316ddd9c5c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 18:59:47 2025 +0100

    coreboot/default: bump rev 812d0e2f62, 5 Apr 2025
    
    There are *many* excellent changes. These changes are of note,
    for Haswell mainboards (raminit improvements, courtesy Angel Pons):
    
    * d5854e4139 Haswell NRI: Implement COMP offset optimisation
    * f14880934b Haswell NRI: Use final timings after basic training
    * ab29f52ee2 Haswell NRI: Measure per-task execution time
    * 4ae9a79d8d Haswell NRI: Remove unused `SPD_LEN` define
    * 0c5286ba34 Haswell NRI: Tidy up REUT subsequence programming
    * 7766228798 Haswell NRI: Deduplicate PCODE mailbox functions
    
    My GMP fixes have been removed, because upstream did
    similar fixes which accomplish the same result.
    
    This brings in the following upstream changes:
    
    * 812d0e2f62 Documentation/lib: Update Timestamp documentation
    * d461627668 payloads/Kconfig: Allow compression for Linux payloads
    * f3ca3aa16b util/cbfstool/cbfs-payload-linux: Copy segments when compression fails
    * 29440057b0 mb/starlabs/{adl_n,twl}: Don't use the IOT FSP
    * eaf76d2dd1 vc/intel/fsp2_0/twinlake: Update FSP headers
    * d5854e4139 Haswell NRI: Implement COMP offset optimisation
    * 2739c4b773 SBOM: Change makefile to get versions from build.h
    * a4156f99ff soc/intel/ptl: Add Wildcat Lake CPU ID to platform reporting
    * 9f73b04074 soc/intel/pantherlake: Add new MCH ID for Wildcat Lake
    * 167c771bc5 mb/google/fatcat/var/francka: Enable audio codec ALC722/ALC1320
    * d8455dfbf6 mb/trulo/var/pujjolo: Change wifi SAR id fw config bits
    * a9e97268fe crossgcc: Fix acpica base url version
    * dabc200abb mb/lenovo/m900_tiny: enable power LED blink in S3 and S4
    * cb86b9a089 mb/lenovo/m900_tiny: Put options in CFR cbtable
    * 26d6da4533 mb/lattepanda/mu: Correct UART1 pinmux for native mode
    * 2ec9a9f17d mb/lattepanda/mu: Update eDP/HDMI in devicetree
    * 3cfa24c1bf mb/lattepanda/mu: Enable PMC drivers
    * c3dba4da2b mb/lattepanda/mu: Add flashmap definition
    * b5db9bcc9d soc/intel/alderlake: Enable USB3 HSIO related parameters for USB3 GEN2 support
    * da49da6c82 soc/intel: Add Arrow Lake-S/HX IDs
    * 8cec500968 mb/google/skywalker: Configure firmware display for eDP panel
    * 78a89d4d70 soc/mediatek/mt8189: Extract code to disable secure mode from DDP driver
    * 8d8d0f9746 soc/qualcomm/x1p42100: Add Clock support for x1p42100
    * 20c2813891 soc/qualcomm/common: Update QUP register structure for QUP v3.2
    * d24c4086e1 Documentation: Add Ramstage Bootstates
    * 10d01fc2de Documentation: Add Threads
    * faf0f29f8d mb/google/ocelot: Update EC host command range for variants
    * ac4dfa5762 mb/hp/snb_ivb_desktops: Add Compaq Pro 6300 MT/SFF variant
    * 984c64295b drivers/crb/tpm: Add new method to retrieve base address
    * 1e8e5d902a mb/starlabs/starlite_adl: Add support for MXC6655 accelerometer
    * 5993dd6ef5 Documentation: Add Timers, Stopwatch, and Delays
    * 4f1f502fd5 soc/mediatek/mt8189: Add PI image loader in ramstage
    * e3ffa3c14f soc/meidatek/mt8196: Move PI image related code to common
    * e96bf7e094 soc/qc/x1p42100: Support to generate Bootblock as multi ELF
    * ae5810e358 util/qualcomm: Add MBN v7 format support
    * 626c5364b8 tree: Use boolean for PcieRpSlotImplemented[]
    * a90a7e0aed mb/google/bluey: Specify ROM size per board variant
    * 0c9204046a mb/google/bluey: Update SPI flash vendor selection
    * d636b38577 soc/qualcomm/x1p42100: Select ARM64_USE_ARM_TRUSTED_FIRMWARE
    * 17abedaef6 include/smp/node: Drop unused is_smp_boot
    * c0413336bc acpi/acpi.h: Use boolean
    * 9be383b855 drivers/lenovo/hybrid_graphics/chip: Use boolean when appropriate
    * f33507c1d8 mb/{google/zork,novacustom/mtl-h}: Use true/false for boolean
    * ae282fe502 drivers/generic/bayhub: Use boolean for power_saving
    * 0a94fcd2db crossgcc: upgrade binutils from version 2.43 to 2.44
    * 316f76635f soc/mediatek/mt8189: Use pmif_spmi_v2 for MT8189
    * f3bd8b7a07 soc/mediatek/pmif_spmi: Move pmif_spmi_force_normal_mode() to common
    * ef10e93e0a tree: Replace scan-build by clang-tidy
    * 6707e9281c mb/google/brox: Update cpu power limits
    * f1aa0a175b util/crossgcc: Build compiler-rt using runtimes
    * b0e0c688c8 buildgcc: Use -d to check libstdc++ include directory
    * f2fed71533 crossgcc: Upgrade acpica from 20241212 to 20250404
    * 07a8737cbd crossgcc:Initialize OPT_LDFLAGS to avoid unset variable in IASL build
    * ad9bfd4243 crossgcc: Always update HOSTCFLAGS from GMP headers if already built
    * c3f5d7c1ee crossgcc: Upgrade MPFR from 4.2.1 to 4.2.2
    * a3ea1cb542 util/crossgcc: Upgrade CMake from 3.31.3 to 4.0.3
    * f9cde87f5a crossgcc/buildgcc: Fix GMP-6.3.0 build with GCC 15 using proper prototypes
    * 35d6ee9223 crossgcc/buildgcc: Remove invalid option for CMake
    * bd36a4a465 util/lint: Remove missing dirs from checkpatch linter
    * a0f2e42879 util/lint: Improve final newline check
    * 6cb9efa19a util/lint: Ignore opensil for Kconfig linter
    * 5228b3ef7b util/lint: Ignore binary files for cb lowercase linter
    * 58d450d2dc util/crossgcc/buildgcc: Reorganize toolchain version variables
    * baf28f8668 mb/trulo/var/pujjolo: Add GPE configuration
    * eb749f2416 spd/lp5: Add SPD for MT62F2G32D4DS-023 WT:C
    * 731bea2fc1 mb/lattepanda/mu: Make VBT compatible with ADL-N FSP IPU25.3
    * 6b7f697309 util/amdfwtool/amdfwread.c: Properly error out in relative_offset()
    * 4a99023e0f util/amdfwtool/amdfwread.c: Remove APOB_NV special case
    * 000ac2cc38 util/amdfwtool/amdfwtool.c: Use physical address for APOB_NV
    * d0355cb647 util/amdfwtool: Move APOB_NV quirk to amdfwtool.c
    * 6fa44461e7 mb/google/fatcat/var/kinmen: Add Fn key scancode
    * dd7956bfc5 mb/google/ocelot: Update GPIO table
    * 1222c704b5 mb/google/fatcat/var/felino: Add pull high setting on GPP_V12 and NC_LOCK GPP_F09
    * 61d74dc8f7 payloads: Propagate SPI flash address mode flag to libpayload
    * 8dec5fcaf8 drivers/spi: Add 4-byte address mode flag to lb_spi_flash
    * a01c368a8a drivers/spi: Refactor 4-byte addressing mode handling in SPI flash
    * 30e7e604fb mb/google/fatcat/var/fatcat Align I2S and DMIC pad configuration
    * 9fe1546ffe Docs/releases: Update 25.06 release notes
    * 5c281529ea mb/trulo/var/pujjolo: Add FW_CONFIG for mipi camera
    * e5e79de8cc mb/trulo/var/pujjoquince: Add Fingerprint function
    * 0dcea61e7c cbfstool: Add multi ELF support
    * 6a02f2d4a7 util/qualcomm: Add script to concatenate ELF images
    * 3a0b6f625a mb/google/trulo/var/pujjocento: Enable WiFi SAR table
    * 64a79d23e8 mb/trulo/var/pujjoquince: Add SD card function
    * b726a9c7e9 mb/google/fatcat: Create new fatcatite4es variant
    * e7984f39eb mb/google/fatcat: Create new fatcatnuvo4es variant
    * cdf0c76dc8 mb/google/fatcat: Create new fatcat4es variant
    * 8e5bdde028 mb/intel/ptlrvp: Add support for H58G66BK7BX067 memory
    * 110aebb4d2 mb/google/nissa/var/riven: Add focaltech touchscreen support
    * bc8876d56d Revert "soc/intel/xeon_sp/uncore.c: mark TSEG/SMM region as reserved"
    * 64d4888349 mb/google/dedede/var/magolor: Generate SPD ID for CXMT CXDB4CBAM-ML-A
    * cc116e08aa mb/google/nissa/var/quandiso: Add touchscreen FTSC1000 support
    * 35648dc37b acpi: Add _func suffix for callback functions
    * 03be570994 mb/google/rauru: Remove unused get_oled_description
    * cc0a410ff5 soc/mediatek/dp: Correct eDP register settings for dptx_v2
    * 056405a10d mb/google/ocelot: convert variants for use with ES silicon
    * 4ef51ffbd7 mb/google/skywalker: Add panel driver in mainboard
    * cdb49c4d2e soc/mediatek/mt8189: Add ddp driver to support eDP output
    * d8fc5eba2d soc/mediatek/mt8189: Add eDP driver
    * cfd0b4dd20 soc/mediatek/mt8189: Change msdcpll default freq to 384MHz
    * a60c5d205b mb/google/nissa/var/meliks: Initialize display signals on user mode
    * f846ec1e37 mainboard/google/fatcat: Set OEM footer logo bottom margin
    * 97f92d5c69 drivers/intel: Add support for configurable footer logo bottom margin
    * 3e0d8a2f2c mb/google/bluey: Enable 4-byte addressing mode
    * 5568bee055 drivers/spi: Support forced 4-byte address mode via 0xB7 command
    * a66d2d41f5 mb/lenovo/m900/devicetree.cb: Use OC6 enum
    * f14880934b Haswell NRI: Use final timings after basic training
    * 0e5d1d29bd soc/intel/skylake: Expand USB OC pins enum to OC7
    * 1f28803dcd mb/trulo/var/kaladin: Create kaladin variant
    * 1c2978dba6 mb/google/dedede/var/awasuki: Add ChangXin modules to RAM id table
    * 912161e52d spd/lp4x: Modify parameters of SPD for NT6AP1024F32BL-J1
    * 47f1b798e4 util/amdfwtool/amdfwtool.c: Remove APOB_NV src address check
    * c7fe471482 mb/novacustom/mtl-h/var/dgpu: Add NVIDIA dGPU ASL code
    * 24d8e6f35e Revert "mb/google/brox: Handle GPI_INT pin lower to GPI_WAKE"
    * cf7159af64 mb/siemens/mc_ehl3: Limit eMMC speed mode to DDR50
    * 68ede7b860 mb/google/nissa/var/meliks: Configure Acoustic noise mitigation
    * ea3cc3231c mb/intel/ptlrvp: Remove power limit constraints
    * cae47dfd44 soc/intel/pantherlake: Correct DRHC and SATC in DMAR table
    * e58883aace soc/intel/pantherlake: Refactor VR Fast Vmode I_TRIP threshold settings
    * abbf549558 mb/google/fatcat: Add support for new Panther Lake IDs
    * 57bffed893 soc/intel/pantherlake: Add new PCI and CPU IDs
    * 59fce656b1 soc/intel/pantherlake: Enable Thermal Design Current for various SKUs
    * 5a2de49baa soc/intel/cmn/blk/power_limit: Add helper functions to romstage
    * 2207a4b59a vc/intel/fsp/fsp2_0/pantherlake: Add TDC current limit configuration
    * efa24540b0 drivers/wifi/generic: Implement Bluetooth PRR DSM functions
    * d92b6163e7 drivers/wifi/generic: Implement Wi-Fi PRR DSM functions
    * 81d7bc386e soc/intel/cmn/blk/cnvi: Set WFDL default value to 50 ms
    * 1be1ccb42e soc/intel/cmn/blk/cnvi: Use WFDL field for Wi-Fi PLDR reset delay
    * ff46501d6d soc/intel/cmn/blk/cnvi: Correct generated ACPI code in comments
    * 782ae11bc7 soc/intel/cmn/blk/cnvi: Add _PRR method for Bluetooth CNVi Reset
    * 23f5df6eae mb/google/nissa/var/yavilla: Add H58G66CK8BX147 to RAM ID table
    * 4a2c61a8a0 soc/qc/x1p42100: Allow building QC platform without upstream blobs
    * 2a09db3c29 drivers/intel: Refactor logo rendering with helper functions
    * 57d29ebd74 vc/google/chromeos: Don't pack `cb_plus_logo.bmp` if footer is present
    * ef051256dc mainboard/google/fatcat: Drop logo_valignment selection
    * dfeaead9f2 drivers/intel: Add horizontal logo alignment for splash screen
    * ced9f91ae9 soc/intel/cmn: Improve comments for fw_splash_vertical_alignment enum
    * d309a9dfa8 drivers/intel/fsp2_0: Suppress OEM footer in low-battery mode
    * 4373eea5d8 {lib, drivers/intel}: Add splash screen footer
    * be5609bdaf lib: Introduce a new function `bmp_load_logo_by_type()`
    * a1dbb4076c lib: Add support for different bootsplash types
    * f48865ab9a drivers/intel/fsp2_0: Refactor bitmap loading and GOP BLT conversion
    * f3f9c0bd8e soc/intel/ptl: Add PCIe ACPI support for Wildcat Lake SoC
    * ba715b3d25 mb/google/nissa/var/guren: Add SPD ID for MT62F512M32D2DR-031 WT:B
    * 43b6f44e22 soc/mediatek/mt8189: Remove ulposc1 hardware calibration
    * f63016c36f soc/mediatek: Unify DPTX swing/preemphasis API
    * df91698b11 soc/mediatek/mt8196: Refactor mt8196 eDP driver for better code sharing
    * 03fca0f0b4 mb/google/brox: Enable support for Realtek EC
    * c8eb52c10c ec/google/chromeec: Modify Realtek EC initialization timing
    * e2ac46bcc7 spd/lp5: Add SPD for hynix H58G66CK8BX147
    * 812379f500 soc/mediatek/common: Move map_to_lpddr_dram_type() to common for reuse
    * 7c19b1fa58 mb/google/skywalker: Run MTK FSP binary in ramstage
    * 89e4fff2d3 crossgcc/buildgcc: introduce RISCV_ISA_SPEC for RISC-V ISA specification
    * 620c8d9f71 mb/google/brask/var/constitution: Generate RAM ID for B3221XM3BDGVI
    * 57b12d2171 spd/lp4x: Generate initial SPD for B3221XM3BDGVI
    * 7c0da94aeb mb/google/brya/var/pujjoga: Add and select VBT
    * bcd569faf1 mb/google/skywalker: Create variant Baze
    * fb2c834f7c mb/trulo/var/pujjolo: Fix p-sensor function
    * a7cd5c8c6b mb/trulo/var/pujjolo: Enable USB3 functions
    * 2c53151c0c mb/trulo/var/pujjolo: Enable Ax211 wifi function
    * ad78fc535a mb/trulo/var/pujjolo: Add single ram configuration
    * f941b51e0e soc/mediatek/mt8189: Correct MFG MUX OPP init setting
    * a1d9b69f47 soc/qc/x1p42100: Add metadata files for shrm and cpucp
    * b369756680 util/qualcomm: Add script to extract a segment from ELF
    * 19d1604fd7 mb/google/bluey: Update flash layout
    * b9aae6180b mb/google/nissa/var/meliks: Link touchscreen device with display panel
    * 6e58c0148b Reland "libpayload: arm64: Reduce DMA allocator space to 1MB"
    * f18420b6a9 mb/google/fatcat: Create new felino4es variant
    * 992ba78142 mb/google/fatcat: Create `felino` model for easier variant integration
    * afbc9126f9 mb/trulo/var/pujjolo: Update GPIOs and probe SD card to fix S0ix suspend
    * b3b1809764 mb/google/octopus: Correct channel count for DMIC
    * 9accaa7238 mb/google/poppy: Correct channel count for DMIC
    * 41e09a5c59 mb/google/fizz/var/karma: Correct channel count for DMIC
    * fed7ad967a mb/google/reef: Correct channel count for DMIC
    * 686dea9883 mb/google/glados: Correct channel count for DMIC
    * ea6f150d9d soc/intel/cmd/blk/cnvi: Correct conditional logic for CNVI readiness
    * 29dd511628 soc/intel: Move CNVI sideband definitions to SoC-specific files
    * ea8a3e685f soc/intel/cmn/blk/cnvi: Add descriptive comments for PRRS and RSTT
    * d17ace2c1b soc/intel/cmn/blk/cnvi: Remove hardcoded offset in OperationRegion
    * bb3a484e36 soc/intel/*/acpi: Move the BASE ACPI method to northbridge
    * 3c88e629d9 mb/google/brox/var/lotso: Generate RAM IDs for lotso
    * 1bdf89d78c device/device_util.c: Complete function documentation
    * bc84e1ba42 soc/intel/cmn/acpi: Refactor `SPCO` ASL method
    * 4bf0f4fab3 mb/google/fatcat/var/felino: Add PIXART touchpad to devicetree
    * 8269a89d32 mb/google/fatcat/var/felino: Add Synaptics touchpad to devicetree
    * 4d9dfb63bd Documentation: fix broken flashrom.org link
    * 3696fea4e0 mb/google/ocelot: add BOARD_GOOGLE_OCELOTMCHP
    * 6ebd30bf7d mb/google/ocelot: add BOARD_GOOGLE_OCELOTITE
    * da122fe8f5 mb/starlabs/*: Use PLTRST for PCH Strap GPIOs
    * 7f03e3bd6c drivers/efi/efivars: Change printk level from ERROR to DEBUG
    * c740786f12 drivers/gfx/generic: Use 'noop_read_resources'
    * b1759c9bd6 mb/starlabs/starlite: Adjust the Flash Map to match the Twin Lake IFD
    * cc1f0e5c90 mb/starlabs/starbook: Disable TME_KEY_REGENERATION_ON_WARM_BOOT
    * 9381dd0cbf soc/intel/meteorlake: Make TME_KEY_REGENERATION_ON_WARM_BOOT selectable
    * 9b91d50fc1 mb/siemens/mc_rpl1: Add GPIO configuration
    * f44b19f2dc soc/intel/pantherlake: Fix ACPI can't tag data node error
    * fad0064377 soc/intel/ptl: Add UFS support for Wildcat Lake SOC
    * 3a065dbbfc mb/google/nissa/var/yavilla: Add H58G56CK8BX146 to RAM ID table
    * 98f1886c89 mb/google/nissa/var/yaviks: Add H58G56CK8BX146 to RAM ID table
    * 3711be4e18 soc/intel/xeon_sp: Use Kconfig to define SPI_BASE_ADDRESS
    * ad05c65d72 soc/intel/xeon_sp: Initialize SPI before using it
    * b4f2a51533 libpayload/arch/arm64/mmu: Fix missing CBMEM in used ranges
    * 6da913bd46 docs/security/vboot: Update supported board list
    * a0e6fd9a95 Documentation: Add coreboot release 25.09 template
    * d4a33638f5 mb/trulo/var/pujjolo: Change dram id table
    * 2ee72eaab1 soc/intel/pantherlake: Update CONSOLE_UART_BASE_ADDRESS Kconfig value
    * 25385df017 vc/intel/fsp/ptl: Update PTL header files to FSP 3182_01
    * 2286134002 mb/google/fatcat/var/felino: Configure CAM_VDD_EN_SOC GPIO to restore camera function
    * 486604360c mb/google/ocelot: add BOARD_GOOGLE_MODEL_OCELOT
    * f6926dc8a5 mb/google/nissa/var/pujjoniru: Update DTT settings for thermal control
    * bb022f18d5 mb/google/dedede/var/awasuki: Add 2 NANYA modules to RAM id table
    * 4ef1258436 spd/lp4x: Add Nanya memory part
    * 99c138ec50 soc/mediatek: Don't attempt de-assert PERST# without pci_root_bus
    * b9754131a6 mb/google/ocelot/var/ocelot: Update initial overridetree settings
    * 4199351c1b Revert "libpayload: arm64: Reduce DMA allocator space to 1MB"
    * a11eacc204 mb/msi/{ms7d25,ms7e06}/devicetree.cb: Add fan control config
    * a069c920f5 mb/msi/{ms7d25,ms7e06}: Mimic the vendor BIOS early SIO init
    * 3c23d7b3a9 src/superio/nuvoton: Add HWM initialization code
    * ace18dea15 mainboard: Add 2S Intel Birch Stream MiTAC Computing R520G6SB
    * 4569adeedc mainboard: Add 1S Intel Birch Stream MiTAC Computing SC513G6
    * ab29f52ee2 Haswell NRI: Measure per-task execution time
    * 925845c38c mb/google/ocelot: Update Kconfig
    * c796c68dec mb/google/ocelot: Update MAINBOARD_PART_NUMBER
    * b322d30944 mb/google/brya/var/moxie: Enable RTD3 for SSD to resolve S0ix issue
    * f85f7d7aed mb/intel/beechnutcity_crb: Use host address for BiosRegionBase
    * 4d3dc433f9 mb/intel/avenuecity_crb: Use host address for BiosRegionBase
    * 881fe9cef6 soc/intel/alderlake: Add cpuid_to_adl mapping for Core 3 N350 SoC
    * 08c8a74170 mb/trulo/var/pujjolo: Add MB usb-a port3 function.
    * 317affb0ad mb/trulo/var/pujjolo: Enable Elan touchscreen function.
    * de259ad970 mb/trulo/var/pujjolo: Enable s0ix function
    * 712dfb3761 Revert "util/cbmem: Consolidate CBMEM and coreboot table access"
    * 30865c2fb1 mb/amd/birman_plus: Skip i2c_early init
    * f2e488cfbf mb/google/fatcat: Add power limit overrides for H204 and H404 SKUs
    * 1537c89e8d soc/intel/cmn/block/power_limit: Enforce variant PL4 for Fast VMode
    * d9c5cef7f0 soc/intel/pantherlake: Add Fast VMode PL4 Power Limit configuration
    * b879342fe6 soc/intel/pantherlake: Add support for the H204 SKU
    * b42842bbe5 mb/google/brox: Add brox_rtk_ec variant
    * 73cc8a413a treewide: Work around GCC 15 Werror=unterminated-string-initialization
    * d00f5c2d8c mb/google/skywalker: Reset xsphy0 in mainboard_init
    * 40bf6c28f8 soc/mediatek/mt8189: Add support for USB port 0 reset
    * 26fd33a92a mb/starlabs/starlite_adl/acpi: Fix _GPE callback type
    * d14a3e23da mb/starlabs/starlite_adl: Clarify pmc_gpe0_dw0 mapping in devicetree
    * 2c0417ea06 mb/starlabs/starlite_adl: Remove duplicate GPP_E12 entry
    * 7e711a5bef Reland "soc/mediatek/mt8196: Specify MTKLIB_PATH for building BL31"
    * 47f2c17961 mb/starlabs/*: Add CFR option to enable/disable S0ix
    * dc3d524d19 mb/starlabs/starlite_adl: Use SoC common CFR forms
    * 808c982104 mb/starlabs/starfighter: Use SoC common CFR forms
    * 644fd7b7f5 mb/starlabs/starbook: Use SoC common CFR forms
    * c7a1539d87 mb/starlabs/lite: Use SoC common CFR forms
    * 3f16609ba2 mb/starlabs/byte: Use SoC common CFR forms
    * c3be703b71 soc/intel/common/cfr: Add bool option for auto power on
    * b3ac5ecdac soc/intel/cmn/block/cfr: Add CFR form for pciexp_aspm_cpu
    * 9f8e5ab661 soc/intel/cmn/block/aspm: Use separate option variable for CPU RP
    * 4247128e39 soc/intel/cmn/block/aspm: Fix ASPM control for CPU root ports
    * b66b7f7860 commonlib/device_tree.c: Add a function that reads FDT ints
    * c776d2dbd6 ec/google: Add support for Realtek EC in ChromeOS EC
    * 8b54428200 mb/google/nissa: Override GPIO_PCH_WP for pujjocento variant
    * 2060f24d60 mb/system76/mtl: Add Darter Pro 11 variants
    * c2496bc62e drivers/analogix/anx7625: Add a retry mechanism to decode EDID
    * 7b1eac4192 soc/mediatek/mt8189: Enable MUXes for improved peripheral stability
    * da54093bb9 Update arm-trusted-firmware submodule to upstream master
    * 40c84c2577 mb/starlabs/*: Tidy up the devicetree files
    * cb7d2ebe5c mb/starlabs/starbook/{kbl,cml,tgl}: Remove generic.detect from the touchpad
    * c4eb645a0b update_submodules: Fix submodule path handling
    * 581af94115 ec/dasharo/ec: Add DTT power and battery participants
    * f6dd8f534f MAINTAINERS: Add Dinesh as intel/pantherlake and google/fatcat maintainer
    * f2310ab35e update_submodules: Prefix commit title with relative path
    * 5fcbc709ec mb/google/fatcat/fmap: Add 1 MB from SI_BIOS to SI_All
    * 24778a25de mb/trulo/var/pujjolo: Fix gtx functions.
    * 619699648f soc/intel/pantherlake: Simplify P2SB and P2SB2 device operations
    * beafbfd29a soc/intel/pantherlake: Remove IOE support and references
    * 5277bc4efc soc/intel/pantherlake: Resolve memory corruption by using P2SB2 driver
    * 61ac238bb5 soc/intel/common/block/p2sb: Add driver for second P2SB device
    * 8961f6681f soc/intel/common/acpi/pcie_clk: Fix ACPI conditional compilation error
    * 471df8ca5e util/crossgcc/buildgcc: Fix GMP build on GCC 15
    * c24a12db86 util/cbmem: Consolidate CBMEM and coreboot table access
    * 99e5a386c2 mb/amd/birman_plus/glinda: Add onboard devices
    * 033810a7db payloads/libpayload/Makefile.mk: Replace nm with $(NM)
    * a1738e87b5 soc/intel/panterlake: avoid SPI access delay
    * 359ae67668 elog: Handle elog in later boot phase
    * bf330f2dd0 security/vboot: Back up CMOS data later boot phase
    * 45febdec26 mb/starlabs/starfighter: Add reset GPIO for the USB Bluetooth
    * a9a51f9916 mb/starlabs/starfighter: Add missing ASPM config for the SSD
    * 644ebf5ebc mb/starlabs/starbook/{adl,rpl}: Add generic Graphics driver config
    * 902df45eab mb/starlabs/starfighter: Remove the overcurrent config
    * b872c50f90 mb/starlabs/starfighter: Add generic Graphics driver config
    * cfdf5906fd mb/starlabs/starfighter: Tidy comments for board ID GPIOs
    * 9950825a2b mb/starlabs/starlite_adl: Remove extra lines
    * 6d079d45d1 mb/starlabs/byte_adl: Remove comments for disconnected GPIOs
    * f6a45f6856 mb/starlabs/byte_adl: Re-organise GPIOs
    * 63f781b508 mb/starlabs/byte_adl: Disconnect unused GPIOs
    * 6aeebc4b4b mb/starlabs/byte_adl: Reconfigure PCH Strap GPIOs
    * 5f9046cbb4 mb/starlabs/byte_adl: Remove vGPIO configuration
    * c589142c28 mb/starlabs/byte_adl: Add the Byte Mk III variant
    * 2cb9c3ee46 mb/starlabs/byte_adl: Update the VBT to the Twin Lake version
    * ad8ccf4822 Update arm-trusted-firmware submodule to upstream master
    * c615de7248 soc/amd/glinda: Don't let OS put debug UART into D3
    * 0251e98e9e util/amdfwtool: Do not attempt to continue processing `--help`
    * 0af68855c0 mb/google/nissa/var/pujjoniru: Config AUX gpio to correct TCSS port
    * bba9d27145 mb/google/ocelot: Remove power limit override functionality
    * b3776e23a7 ec/google/chromeec: Add SPI/I2C EC communication files to bootblock
    * be6787a55e mb/google/skywalker: Add storage types to fw_config
    * 0a41779e2e mb/google/skywalker: Add eMMC configuration
    * 3e6b47980a mb/google/skywalker: Add support for getting storage id
    * de251dd677 soc/mediatek/mt8189: Add support AUXADC
    * a283246ef7 soc/mediatek/common: Refactor auxadc driver to support new platform
    * 94686e581a mb/google/skywalker: Add DVFS support in romstage
    * 8ede4bc67b soc/mediatek/mt8189: Add DVFS driver
    * 096ce4b244 soc/mediatek/mt8196: Move dvfs_init() declaration to dvfs_common.h
    * 0b1bc3df2c mb/trulo/var/pujjocento: Support x32 memory configuration
    * 7690442d88 mb/starlabs/byte_adl: Tidy the Kconfig selections
    * ab8339770e 3rdparty/fsp: Update submodule to upstream master
    * 8e3adf778b soc/mediatek: Add data_version to ddr_base_info struct
    * 0cdd4125be mb/trulo/var/pujjolo: Fix touchscreen function and boot up issue
    * 99e0484000 mb/google/bluey: Increase bootblock size to 120KB
    * 1840fb49e0 mb/google/trulo/var/pujjocento: Update gpio setting for DDI-B
    * 69a067a9d6 mb/google/skywalker: Add RT1019 support for beep sound
    * 4caf5131b9 mb/google/skywalker: Add ALC5645 support for beep sound
    * 623caa537f mb/google/skywalker: Add RT9123 support for beep sound
    * 16ff3b33ce mb/google/skywalker: Add SD card configurations
    * 3b68408693 mb/google/trulo/var/pujjocento: Configure USB related settings
    * 6c87853a83 mb/google/bluey: Implement board and SKU ID retrieval
    * 830a887ecb mb/google/bluey: Add WLAN and SSD PCI devices to devicetree
    * 891c208835 soc/qualcomm/x1p42100: Enable basic PCIe support
    * a5d99a814a soc/qc/x1p42100: Perform `soc_mmu_init` inside early bootblock init
    * 481001e13b soc/qualcomm/x1p42100: Add placeholder for early clock initialization
    * 77c6104645 Revert "mb/google/rex: Enable use_gpio_for_status for touchscreen"
    * 715e7e51c5 mb/google/fatcat/var/francka: Add support for DMIC0
    * c16891ecbd soc/intel/meteorlake: Use CACHE_TMP_RAMTOP for TME exclusion range
    * 394dfcaa7b mb/intel/ptlrvp: Handle GPIO support for DDR5 configuration
    * 58165618da mb/google/byra/var/craask: Add VBT for HDMI variant
    * ab160ca301 mb/google/byra/var/teliks: Add default VBT
    * 4d5b32f7f7 mb/google/ocelot/var/ocelot: remove unused gpios
    * 0e5757bfa7 mb/google/trulo/var/pujjocento: Update DTT settings for thermal control
    * c34baacc72 soc/mediatek/common: Add UFS2.2 and eMMC definitions to storage.h
    * f325409784 soc/mediatek/mt8189: Add SD card support
    * ae435c014c soc/mediatek/mt8189: Configure and early initialize eMMC
    * 91ebbb8d35 mb/trulo/var/pujjolo: Modify pujjolo variant
    * aea05e51a7 mb/google/trulo/var/pujjocento: Enable WWAN function
    * 47133a716d mb/google/trulo/var/pujjocento: Add P-sensor support
    * 04c0527aba soc/mediatek/mt8189: Support different PMIC soluitons for MT8189(G/H)
    * 80149f55f7 soc/mediatek/common: Convert spmi_dev_cnt to a function
    * dcf403e43a mb/google/skywalker: Configure fingerprint pins
    * 508d910ed4 libpayload/arch/mock: Select ARCH_HAS_NDELAY for ARCH_MOCK
    * 96ac0224ab pci: Add support for assigning resources to SR-IOV VF BARs
    * ba8be19122 mb/intel/ptlrvp: Update Kconfig for PTLRVP_CHROMEEC
    * 7cbbf786cc update_submodules: Use relative paths to submodules
    * dcc8400e27 mb/google/fatcat/var/felino: Modify GPIOs config
    * 99af85ad36 mb/google/puff: Add VBTs for Moonbuggy and Scout variants
    * 4ae9a79d8d Haswell NRI: Remove unused `SPD_LEN` define
    * 0c5286ba34 Haswell NRI: Tidy up REUT subsequence programming
    * 7766228798 Haswell NRI: Deduplicate PCODE mailbox functions
    * ae68ef3684 cpu/intel/haswell: Export PCODE mailbox functions
    * ddce240d34 cpu/intel/haswell: Clean up Makefile
    * 2117ed850f mb/google/ocelot/var/ocelot: fix storage configs for ocelot
    * c5488c0d6d mb/google/ocelot/var/ocelot: update gpios
    * 6602a4462b mb/google/ocelot/var/ocelot: Enable hda device for AUDIO_ALC721_SNDW.
    * 99b6ff25d4 soc/mediatek/mt8189: Add MTK FSP loader in ramstage
    * c4fe5e2483 mb/google/skywalker: Pass reset GPIO parameter to BL31
    * f59ced2c7c mb/google/fatcat/var/francka: boot up by pressing power button in S5
    * df0221e62a libpayload: Protect against trying to use weak symbols in the wrong way
    * d27e8ef460 update_submodules: Add an empty log line between each iteration
    * d9bd7ce89f mb/google/fatcat/var/francka: Enable audio codec ALC721
    * 48fbd99223 mb/google/fatcat/var/francka: Set the default HDA GPIO pin to an NC pin
    * 3b975f92c7 soc/intel/pantherlake: Select TME support for the SoC
    * 8408bd4863 soc/intel/pantherlake: Add TME configuration
    * dc36a725d6 3rdparty/fsp: Update submodule to upstream master
    * 1f47b0e018 3rdparty/intel-microcode: Update submodule to upstream main
    * 4c446751c6 {commonlib, drivers}: Track firmware splash screen rendering completion
    * ccb8b34194 Revert "mb/var/uldrenite: Use VBT with limited resolution for 4GBx32 memory"
    * 1a00629ae2 mb/google/skywalker: Set up open-drain ChromeOS pins
    * 0f2942b513 mb/google/skywalker: Raise little core CPU frequency to 2.0 GHz
    * 0ba0d03140 mb/google/skywalker: Implement regulator interface
    * 090bce1042 soc/mediatek/common: Add VMODEM and VSRAM_MD bucks support for MT6359
    * c74610afae soc/mediatek/mt8189: Shut down PMIC on power key long press
    * d1f7565403 mb/google/skywalker: Notify EC that AP is in S0
    * da45a88dd3 util/abuild: Fix checking of missing_arches
    * cdcbb71936 mb/google/link: Use chromeec_smi_sleep()
    * 885aacf004 mb/google/byra/var/teliks: Add VBT for 11" panel option
    * 2ce777f178 mb/google/byra/var/yaviks: Add VBT for yavilla
    * 0db4444446 mb/google/byra/var/pujjo: Add VBT for pujjo1e
    * 8c3e6ea319 mb/google/nissa/var/pujjocento: Enable touchpad
    * 643bba345c mb/google/trulo/var/pujjocento: Enable touchscreen
    * 3ecaf04dad mb/var/uldrenite: Use VBT with limited resolution for 4GBx32 memory
    * 71ae2c7366 mb/google/octopus: Add VBTs for DOOD and FOOB variants
    * 7a703fc1fb mb/google/rex: Select IOM_ACPI_DEVICE_VISIBLE
    * 0121d0e3e0 ec/google/chromeec/smi: Clear events before enabling wake mask
    * 5a947da94e mb/google/sarien: Update VBT from v221 to v228
    * 0fba735482 soc/intel/cmn/blk: Refactor CSE status flag and optimize forced sync
    * bb8d069dd3 vc/google/chromeos: Move pvmfw cbmem and enable
    * f562992da1 mb/google/trulo/var/pujjocento: Enable EC keyboard backlight
    * d281a3c559 mb/google/trulo/var/pujjocento: Configure tcss_aux_ori
    * 7150c5e2fe mb/google/skywalker: Create variant Anakin
    * aedc177f00 libpayload: arm64: Reduce DMA allocator space to 1MB
    * 4ccb4a78c4 libpayload: Use Kconfig instead of weak symbol for arch_ndelay()
    * 37513297d3 libpayload: Use Kconfig for architecture memcpy, not weak symbols
    * bcbe17dea3 mb/google/skywalker: Configure TPM
    * 3d40b7d018 soc/mediatek/mt8189: Increase bootblock size from 60KB to 70KB
    * 8d25cf3ae7 soc/mediatek/mt8189: Add SSPM loader
    * 8ab9f56470 mb/google/skywalker: Set up SPM in mainboard
    * 368eeb7da4 soc/mediatek/mt8189: Add MCUPM loader
    * bc3af56fdd soc/mediatek/mt8189: Add SPM loader
    * 98782a59e9 mb/google/fatcat/var/kinmen: Add overridetree
    * bbcb222f0b mb/google/fatcat/var/kinmen: Update GPIO table
    * 743e3a07f5 mb/google/brya/var/nissa: Remove duplicate ACPI device GFX0
    * 87110309d4 mt8196: Remove mcupm_plat.h header from mcupm_plat.h
    * d6fe379e9c mb/google/ocelot: Enable LP5 and DDR5 memory configuration
    * 2985af84c3 mb/google/trulo/var/pujjocento: Add Fn key scancode
    * dafd7d6eb9 mb/google/nissa/var/dirks: Deassert RTL8111H's ISOLATE_ODL earlier
    * c1df30db18 mb/siemens/mc_rpl: Delete fw_config since it is not used
    * 7fbea3175d mb/siemens/mc_rpl: Remove unused gpio and devicetree files
    * 8fdf8694e3 mb/siemens/mc_rpl: Remove Chrome OS and EC as they are not used
    * e020979993 mb/siemens/mc_rpl: Adjust the flash map file
    * 71fb8f63e0 mb/siemens/mc_rpl: Add new mainboard based on Intel's Alder Lake RVP
    * 918f21b72d drivers/spi/winbond.c: Add W25Q64JV signature
    * 278a6d2682 mb/google/trulo/var/pujjocento: update hda_verb table for ALC257
    * 43f7c537f8 mb/google/trulo/var/pujjocento: update GPP_R4/GPP_R5 setting
    * e4fc00adbe soc/amd/common/block: Enable MMCONF first
    * cbbf380fa4 soc/amd/common/block/lpc: Use ROM3 window if possible
    * 9d878fc6c0 soc/intel/xeon_sp: Add support for Emerald Rapids (5th Gen Xeon-SP) CPUs
    * bd66b8cdd2 mb/google/nissa/var/rull: Enable VBOOT_EC_SYNC_ESOL for rull device
    * 3155b2f64c mb/dell/haswell_latitude: Correct BOARD_ROMSIZE_KB_* for E7240
    * 4d30d06637 mainboard/google/ocelot: Configure middle logo vertical alignment
    * 583bf972c5 mb/google/ocelot: Remove NPK device
    * 2bec5a9d9a soc/mediatek/mt8189: Check eFuse ECC in WDT init
    * a89406790a mb/google/nissa/var/meliks: Set vccin_aux_imon_iccmax to 25A
    * ccd4d1d1db mb/var/uldrenite: Make two pins NC to reduce S0ix power consumption
    * e2baa9c7ed mb/google/bluey: Create Quenbi variant
    * a98511fd23 mb/google/bluey: Only select EC_GOOGLE_CHROMEEC_SWITCHES with VBOOT
    * 756d02f779 mb/google/fatcat: Remove extraneous space in Felino Kconfig name
    * 8de02842d5 soc/intel/common/block/cpu: Execute post_cpus_init at BS_DEV_ENABLE
    * 0baf47e03b vc/intel/fsp/ptl: Update header files from FSP 3071_00 to FSP 3144_01
    * 61f043de4a mb/google/skywalker: Initialize DPM in ramstage
    * 3f8702a0d6 soc/mediatek/mt8189: Add DPM v2 driver configuration
    * d5bfa1c697 soc/mediatek/common: Add DPM V2 non-broadcast mode support
    * 24ab31f477 mb/google/skywalker: Enable RTC boot init
    * b288aaee85 soc/mediatek/mt8189: Use common RTC driver MT6359
    * 12d6d0606c mb/google/skywalker: Initialize PMIC in romstage
    * 2a3fd0659d soc/mediatek/mt8189: Add PMIC MT6315 driver
    * 42ac3ccff4 soc/mediatek/mt8189: Add PMIC MT6359 driver
    * a2010cf5ee mb/novacustom/mtl-h/Makefile.mk: include tas5825m.c in the build
    * 2033075753 intel/alderlake/romstage: Implement eSOL during EC software sync
    * e6a7666bcd cpu/intel/car: Skip EC software sync in common code
    * ac4503d0dd security/vboot: Introduce VBOOT_EC_SYNC_ESOL Kconfig option
    * 8a4b3e1346 cpu/intel/microcode: Add error handling if microcode directory is empty
    * cb77cafbb4 soc/mediatek/mt8189: Add SPMI and PWRAP driver
    * b9a4d6ede1 soc/mediatek/common: Correct MT6359 RTC EOSC setting
    * ae2f3ab153 mb/system76: Add SMBIOS slot descriptions
    * c0113106fa nb/amd/pi/00730F01/northbridge: skip IVRS when IOMMU is disabled
    * 5e2aee4474 soc/mediatek/mt8196: Move sspm_enable_sram() to common code
    * c81b08c4ba util/abuild: Fix building ChromeOS boards
    * 62b823f69e mb/google/bluey: Increase flash size to 64MB for W25Q512NWEIM
    * 276eb20b04 mb/google/bluey: Limit SPI flash support to Winbond
    * 47c171a157 mb/google/bluey: Make Chrome EC optional
    * 139a5b6fe0 mb/google/bluey: Select MISSING_BOARD_RESET due to lack of Chrome EC
    * f9d933db36 mb/google/bluey: Introduce MAINBOARD_HAS_GOOGLE_TPM Kconfig
    * e8450f78a0 mb/google/bluey: Make GPIO setups conditional on Kconfig options
    * 4e8ea210bb mb/google/fatcat/var/felino: Add pull high setting on GPP_C03/GPP_C04 in gpio.c
    * 65523e98a6 soc/mediatek: Extract DPM common code
    * aaf373c253 mb/google/skywalker: Implement sku_id()
    * be675e5369 mb/google/skywalker: Configure GPIO XHCI_INIT_DONE as output
    * 9a60da5297 mb/google/skywalker: Enable ChromeOS EC
    * c443478509 mb/trulo/var/pujjolo: Create pujjolo variant
    * 24757047e5 util/abuild: Fix merge error
    * d93f7f01a6 mb/topton/adl: Use CFR setup menu to manage options
    * b59fef9678 soc/intel/cmn/cse: Add Kconfig to set ME default CFR option state
    * 50a5fe77de soc/intel/meteorlake: Add CFR objects for existing options
    * d53f00fbd9 soc/intel/meteorlake: Hook up the VT-d setting to option API
    * e356483eb6 soc/intel/jasperlake: Add CFR objects for existing options
    * 87663d1c0a soc/intel/jasperlake: Hook up the VT-d setting to option API
    * 2c0c2f46d7 soc/intel/tigerlake: Add CFR objects for existing options
    * d06c8dde58 soc/intel/tigerlake: Hook up the VT-d setting to option API
    * 3cfb24a326 soc/intel/alderlake: Hook up the VT-d setting to option API
    * 6f9df7ace4 soc/intel/cannonlake: Add/use enums for IGD config
    * c8199f26e0 soc/intel/skylake: Add/use enums for IGD config
    * 947dd07823 soc/intel/jasperlake: Hook up IGD config to option API
    * 09adda95b9 soc/intel/meteorlake: Hook up IGD config to option API
    * dcbb5771c9 soc/intel/tigerlake: Hook up IGD config to option API
    * d930a3542c soc/intel/alderlake: Hook up IGD config to option API
    * 9faf7ce4f4 soc/intel/alderlake: Add CFR objects for existing options
    * 011baca89d cpu/x86/smm/smm_module_loader: Install bigger page tables
    * aa121a9bbe payloads/external/edk2/Makefile: Set OemId Pcd
    * ca9616b984 ec/system76/ec: Add config for 2nd fan without GPU
    * f1f58b20b9 soc/mediatek/mt8189: Add SPI driver support
    * d4a759a068 mb/system76/mtl: darp10: Add TCSS configs
    * 85972101e6 commonlib/device_tree: Make *path const in dt_find_node()
    * de9d76c761 mb/starlabs/starbook/tgl: Configure the eSPI GPIOs
    * af7fb83ed0 soc/intel/apollolake: Hook up S0ix setting to option API
    * 9979be7482 drivers/intel/fsp2_0: Remove redundant NULL checks and simplify code
    * 6f9de346ae Revert "soc/amd/glinda/Makefile.mk: Use relative address for APOB_NV"
    * d263e0bd92 soc/amd/glinda/Makefile.mk: Use relative address for APOB_NV
    * 4f7ea3667c mb/google/rex/var/kanix: Tune camera I2C timing
    * f0ad05b57e mb/google/brya/var/uldrenite: Fix USB_OC1 for USB3 A0 port
    * 1140891211 mb/google/bluey: Initialize I2C, SPI, and GPIOs in bootblock
    * ba8407f0c1 soc/intel: Add Arrow Lake-H/U IDs
    * 3e1f96a0f4 mb/system76/mtl: Add Lemur Pro 13
    * 3008b8de53 soc/intel/skylake: Show that SMRAM is unconditionally locked
    * e6dc71fe9f util/superiotool: Dump one more NCT6779D register
    * b50ceba64a mb/amd: Increase ROM size on boards, incorrectly limited to 16 MB
    * 850703b32b mb/google/bluey: Configure FPMCU power, reset, and QUPv3 peripherals
    * b4c6984a40 soc/qualcomm/x1p42100: Initialize QSPI and QUPv3 in bootblock
    * fe34206442 soc/mediatek/mt8189: Add audio/display bus protection release functions
    * c2b17a083d soc/mediatek/mt8189: Add PLL and clock init support
    * e4cbd9ea9f soc/mediatek/mt8189: Add MTCMOS init support
    * 5cf460dce9 soc/mediatek/mt8196: Fix RTC protection register unlock failure
    * 2c986d016e MAINTAINERS: Add Google Bluey and Qualcomm SOC maintainers
    * 64fe6fd94a util/abuild: fix TODO and update targets variable to an array
    * 902288db22 util/abuild: Update version and date string
    * 8504c796fc util/abuild: Remove obsolete FIXME
    * a8e1113e3b util/abuild: Check functions directly instead of with $?
    * b128abcdad util/abuild: Add quotes around variables
    * 52b932df3b util/abuild: Group printfs to timestamps file together
    * ad19c94d87 util/abuild: Fix shellcheck warnings about local vars
    * d88ea14e8d util/abuild: Remove unused debug() function
    * 82dea9d6d1 util/abuild: Disable shellcheck warning on interrupt()
    * a2baaec067 util/abuild: Use ${} around variable names
    * 9ddb54e6ad util/abuild: Update syntax from 'function func' to 'func()'
    * f66c7c1037 util/abuild: Update echo to printf for consistency.
    * 49ae935b37 util/abuild: Change [...] to [[...]] for consistency
    * ea32e30a18 mb/starlabs/*/cfr: Remove `reboot_counter` CFR option
    * d4cb553986 mb/starlabs/*/cfr: Remove `boot_option` CFR option
    * 452e179727 mb/starlabs/*/cfr: Use global console CFR object
    * aebef78622 xcompile: Use Walloc-size GCC option
    * 074dd4f6f5 mb/google/fatcat: Set logo vertical alignment to middle for variants
    * 02ca72b2d4 soc/intel/meteorlake: Hook up Pch Sleep Assertion widths
    * 166f0ea146 util/abuild: Identify abuild builds with an env variable
    * 511872dae3 mb/dell: Convert Latitude E7240 into a variant
    * b5581d556b drivers/mrc_cache: Measure MRC cache as runtime data
    * 05eb3e3716 mb/google/skywalker: Create variant Yoda
    * c8ddae9ebe mb/google/puff: Use CFR setup menu to manage options
    * dc19824e56 mb/google/fizz: Use CFR setup menu to manage options
    * 1d62a1e857 mb/google/jecht: Clean up makefile
    * 4112c77919 mb/google/jecht: Use CFR setup menu to manage options
    * 6eddde31bb mb/google/beltino: Clean up makefile
    * 445575525c mb/google/beltino: Use CFR setup menu to manage options
    * 376a5acc24 util/lint: Add lint file for gofmt
    * 4456c125f6 soc/mediatek/common: Move PMIF SPI macros to per-SoC's header
    * 8efdbf0c34 mb/google/nissa/var/meliks: Use default domain_vr_config[] settings
    * f07a1a76f3 mb/google/brya: Enable GNA scoring accelerator
    * 6c830088da mb/google/rex/var/screebo: Generate RAM IDs
    * ac2bd75817 spd/lp5: Add SPD for K3KL9L90EM-MGCU
    * 66873a3812 vc/amd/fsp/glinda: Update SMBIOS Type 17 information
    * b23db384a9 vc/amd/fsp: Update SMBIOS Type 17 PartNumber size
    * 0b120de7c0 Documentation: Update documentation for Topton X2F-N100
    * d50019d432 mb/starlabs/starbook_mtl: Select SKIP_SEND_CONNECT_TOPOLOGY_CMD
    * 4aa1861fbb mb/starlabs/starbook/mtl: Configure sleep assertion times
    * 183c414577 soc/intel/meteorlake: Add Kconfig to skip FSP TBT connect topology
    * aa1eba2f25 drivers/intel/fsp2_0: Enable firmware splash using 24-bit BMP logo
    * da29107572 mb/google/fatcat/var/francka: Reduce generic reset delay to 10ms
    * 60916d0f10 mb/trulo/var/uldrenite: Support different ISH UART mappings
    * 3fe4b00966 mb/trulo/var/uldrenite: Swap ISH UART from UART1 to UART0
    * 407c7d0da3 Documentation: Add Device Operations
    * 20d7eaeb0f Documentation: Add chip operations
    * bf38f8eddc vc/intel/fsp2: Drop superfluous header for Raptor Lake S
    * eec228987e mb/intel/coffelake_rvp: Make use of chipset devicetree
    * c9f4cfa463 AUTHORS: Update list to 25.03
    * da5101fde4 cpu/x86/smm: Drop unused label
    * 9154070320 mb/asus/h61-series: Add H61M-A/USB3
    * e8c724fe1a mb/lenovo/m900_tiny: Update VBT to build 1037 with Kaby Lake gfx support
    * 21ca6701ff mb/google/{drallion,hatch,sarien}: Skip adding DTT/TCPU to SSDT
    * 5bf88a44e9 drivers/smmstore: Support 64-bit MMIO addresses
    * 2706ce0266 mb/intel/ptlrvp: Add GPIO support for T4 LP5 board
    * 40b62ff6c4 mb/intel/ptlrvp: Add memory configuration support for T4 LP5 board
    * 7f826fddc5 mb/intel/ptlrvp: Compile variant.c in ramstage for ptlrvp
    * 0ca46ac0d2 soc/intel/pantherlake: Enable coreboot native logo rendering
    * 210371e25b mainboard/google/fatcat: Configure middle logo vertical alignment
    * e446c1f917 drivers/intel/fsp2_0: Introduce coreboot native logo rendering
    * 2f23896299 soc/intel/intelblocks/cfg: Add splash screen vertical alignment options
    * 78d15d9a12 drivers/intel/fsp2_0: Add Kconfig to select FSP for BMP rendering
    * 5f941893ef cpu/x86/mtrr: Introduce mtrrlib with common MTRR helper functions
    * e180971560 drivers/intel/fsp2_0: Move graphics info struct/GUID to FSP header
    * 18b4349422 mb/var/uldrenite: Fix fw_config_gpio_padbased_override not being called
    * a6be271e63 arch/x86: Unify GDT entries
    * 1e7e4e943f soc/intel/tigerlake: Hook up S0ix setting to option API
    * ba4b26c4fc soc/intel/meteorlake: Hook up S0ix setting to option API
    * 514ad949e3 soc/intel/jasperlake: Hook up S0ix setting to option API
    * 55afbe250d soc/intel/elkhartlake: Hook up S0ix setting to option API
    * 3cc728110d soc/intel/alderlake: Hook up S0ix setting to option API
    * 245cba6795 cpu/x86/smm: Add support for exception handling
    * 2e27ceed67 mb/google/volteer/var/elemi: Check FP presence against SKU ID
    * 663dbd462a soc/amd/phoenix: Remove outdated TODO comments
    * b1b8b0e8e1 mb/starlabs/starbook/tgl: Reconfigure PCH Strap GPIOs
    * 36ac6226ff util/autoport: Add function to create empty files
    * f8071719e7 soc/intel/ptl: Add Wildcat Lake platform reporting
    * db4162adce soc/intel/ptl: Add Wildcat Lake PCIe Device details
    * 1baf0baf58 soc/intel/ptl: Add Wildcat Lake SoC device tree
    * 2fc246cd2d mb/google/ocelot: Remove unused devices from devicetree
    * 3278551f8c drivers/intel/fsp2_0: Include coreboot_tables.h in fsp_gop_blt.h
    * 18172b6009 mb/google/bluey: Add SoundWire amp and SD card GPIOs to lb_gpios
    * 80901a4494 mb/google/bluey: Add GPIOs for Soundwire, Display, and SD Card
    * 1015d4332f mainboard/google/bluey: Add fingerprint sensor GPIO entries
    * a85b9a21b2 mb/intel/ptlrvp: Add support for DDR5 configuration
    * c1bcb43f7c cbgfx: Prevent divide-by-zero edge case in Lanczos kernel
    * 565c768c20 soc/intel/alderlake: only add wifi Mitigation if DRIVERS_WIFI_GENERIC
    * ac948173ad mb/starlabs/starfighter/rpl: Add ramstage.c to makefile
    * f1509a467c mb/starlabs/starfighter: Add CFR option to use native panel resolution
    * 3593314cf5 mb/starlabs/starlite_adl: Add CFR option to use native panel resolution
    * d13afbbbca mb/starlabs/starbook: Add CFR option to use native panel resolution
    * 8fa84d9111 mb/starlabs/*: Add CFR entry for Bluetooth RTD3
    * d2b0220a38 allocator_v4: Re-enable top-down allocation for edk2
    * 4d7b56cdaa soc/intel/cmn/cse_lite: Fix handling of soft disable state
    * 33b3269d91 soc/intel/cmn/cse: Add function to check if ME state is M3_NO_UMA
    * 30a4fec86e mb/google/fatcat/var/kinmen: Generate SPD ID for Micron modules
    * cf5696834b soc/intel/ptl: Refactor Panther Lake SoC configuration
    * e99532d99b soc/mediatek/common: Update SPMI calibration process
    * f83fb11e5f soc/mediatek/mt8189: Add CPU segment ID support
    * 7b27b1ca99 soc/mediatek/mt8189: Fix incorrect GPIO register address
    * f2cf732997 libpayload: usbmsc: Correctly deal with disks larger than 2TB
    * 173c5d0aad src/arch/x86/c_start: Delete duplicated code masking stack pointer
    * 1166f9be0d include/console: Add CFR object for setting the logging level
    * 0f0d5fc725 soc/intel/apollolake/acpi: Add function to get PCIe BAR
    * 5d3664ce3b mb/starlabs/starbook/adl_n: Update VBT to fix HDMI output
    * 4b765fdd98 mb/google/fatcat: Disable EnableFastVmode on Panther Lake H SoC
    * f63c3bb297 soc/intel/cannonlake: Hook up DPTF device to devicetree
    * b7d59185ab soc/intel/common/dtt: Add Kconfig to skip SSDT generation
    * 094f75162f cpu/x86/64bit/pt: Fix integer arithmethic in assembly
    * bbd8f0aef8 soc/intel/ptl: Refactoring NUM_COMx_GRP_PADS calculation
    * cf47edb173 ec/google/wilco/acpi: Add UCSI port data
    * 89e915e981 ec/google/wilco/acpi: Fix S3/S4 support
    * 4a89d1b77d soc/intel/ptl: Add GPIO ACPI support for Wildcat Lake SoC
    * a4a2cdeb17 soc/intel/ptl: Add GPIOs for Wildcat Lake SoC
    * 2ce567f1d0 soc/intel/common/block/cse: Prevent HECI commands when flash descriptor override is set
    * 9660279966 drivers/usb/intel_bluetooth: Hook RTD3 up to the option API
    * e2705d93d8 soc/intel/pantherlake: Reduce IGD stolen memory size from 128MB to 64MB
    * 9f98a2a78a mb/asus/p8z77-v_le_plus: Use additional rt8168 MAC programming
    * 2b598a9472 drivers/net/r8168.c: Add option to program MAC address to ERI registers
    * 4b871c6314 ec/intel: read board ID one time from EC per stage
    * 08722cd9f9 mb/google/dedede/var/beadrix: Add Ziliatech part to RAM ID table
    * c0920396d0 mb/google/bluey: Make GSC_AP_INT GPIO configurable via Kconfig
    * 2e387e13f5 mb/google/fatcat/var/francka: Conditionally init HDA
    * e545494f6d mb/google/fatcat/var/fatcat: Conditionally init ALC256 HDA
    * 03d2ef67d7 soc/intel/cmn/hda: Introduce mainboard hook for HDA initialization
    * 85c65b0c20 mb/google/fatcat: Remove NPK device from fatcat and francka variants
    * 92955fbfa6 mb/google/trulo/var/uldrenite: Configure GPP_E9 as NF2
    * aafcb01ec4 mb/intel/ptlrvp: Synchronize codebase with fatcat
    * effd1ffdad mb/google/ocelot: Update Ocelot board
    * 1044f03878 payloads/external/edk2: Set StatusD register to work around failing AMD boot
    * 2170ad0c60 Documentation/lib/timestamp.md: Reformat to 72 characters per line
    * 22118a137b mb/google/fatcat/var/kinmen: Add memory settings
    * 54c87dbed0 mb/google/trulo/var/uldrenite: Update DPTF parameters
    * 9ef62ad64c mb/intel/ptlrvp: Introduce PTL RVP External and Internal EC Configurations
    * 7c965f9df0 MAINTAINERS: Add Nick, Avi, and Pranava for new google/ocelot entry
    * d2e698056e mb/google/bluey: Set correct Kconfig defaults for peripherals
    * 34d6bc8784 soc/qualcomm/x1p42100: Set correct Kconfig defaults for peripherals
    * 2201f57493 soc/qualcomm/x1p42100: Add QUP Serial Engine (SE) entries
    * 6a503fe5a4 mb/google/var/uldrenite: Configure GPP_A16 as NF4
    * c2c95fbd24 sb/intel/lynxpoint: Add CFR objects for existing options
    * 96fd20c5e0 soc/intel/broadwell: Add CFR objects for existing options
    * ce6f7820f4 ec/google/chromeec: Increase EC status timeout to 30 seconds
    * 17347eedc3 soc/intel/cannonlake: Add CFR objects for existing options
    * ad704e0500 soc/intel/cannonlake: Hook up the VT-d setting to option API
    * 7f8d1f2086 mb/google/nissa/var/pujjoniru: Support x32 memory configuration
    * fe881c990c mb/google/brya: Create pujjocento variant
    * 7da36ad79a mb/google/bluey: Add initial support for Bluey
    * 57d7957e3c soc/qualcomm/x1p42100: Add initial SoC skeleton for X1P-42-100
    * c82f5fe133 soc/amd/glinda: Select SOC_FILL_CPU_CACHE_INFO
    * ee76692571 payload/external/edk2: Add Kconfig to support use of PCIe OpROMs
    * fb3f025ea6 soc/amd/common/cpu/noncar: Add SMBIOS helper
    * 36f01c3481 mb/google/fatcat/var/felino: Add Write Protect GPIO to cros_gpios
    * 934fcfb6a0 soc/mediatek/mt8189: Add I2C driver support
    * b3bdffa475 soc/mediatek/common: Move I2C functions to common code
    * 1e0941c295 mb/google/ocelot: Select Wildcat Lake(WCL) SoC config
    * b249275e3d mb/amd/crater: Fix some ec defines
    * 443f514365 mb/amd/crater: Add touchscreen support
    * 4e55225f2c mb/amd/crater: Add missing dxio descriptors
    * 608db150f1 smmrelocate: Drop unused parameter
    * 157b7ae778 payloads/edk2: Update default branch for MrChromebox repo to 2025-02
    * 76a1e81b10 mb/starlabs/*: Unify Sleep S3 and S4 GPIO configurations
    * fed584e100 soc/intel: Add Wildcat Lake CPU and PCIe device IDs
    * 5d7e2b4c0c mb/google/fatcat: Disable VR settings on Panther Lake H SoC
    * 8be95806a6 mb/google/ocelot/var/ocelot: update gpios
    * 92f9c8a985 mb/google/ocelot: update FW_CONFIG
    * 49bf8f94a0 soc/intel/common: Add CFR objects for existing options
    * 509b01c3b6 soc/intel/cannonlake: Hook up S0ix setting to option API
    * b830fdc2d7 soc/intel/cannonlake: Hook up IGD config to option API
    * 5efb54d371 soc/intel/broadwell: Allow ME enable/disable to be set via option
    * 42379e7f76 sb/intel/lynxpoint: Allow ME enable/disable via option
    * 204aae207d mb/samsung/stumpy: Clean up makefile
    * e3d3fc5b4a mb/samsung/stumpy: Use CFR setup menu to manage options
    * 936ca8404a drivers/option/cfr: Select EFI variable store when edk2 payload used
    * 20ceed1929 drivers/efi/fw_info: Select necessary UDK binding as needed
    * c0e3f6d1d2 drivers/efi/variable_store: Select necessary UDK binding as needed
    * a899359720 sb/intel/bd82x6x: Add CFR objects for existing options
    * ada6b98766 nb/intel/sandybridge: Add CFR objects for existing options
    * f14aa06606 soc/intel/skylake: Add CFR objects for existing options
    * f51c0bb090 soc/intel/skylake: Hook up IGD config to option API
    * 32c78b7e22 soc/intel/skylake: Hook up S0ix setting to option API
    * 73b095d5ea mb/starlabs/*: Select DRIVERS_OPTION_CFR_ENABLED
    * 4eba4e3f26 superio/ite/it8772f: Program power state after failure
    * fbca3e6806 superio/ite/*: Move setting of power state to common code
    * 60b414fc13 soc/intel/cannonlake: Drop redundant PcieRpEnable
    * ee30558c49 soc/intel/skylake: Drop redundant PcieRpEnable
    * 439d7fb7d0 mb/google/brya: Create epic variant
    * c4e6050146 mb/google/skywalker: Create variant Obiwan
    * 0cc0e6996c drivers/smmstore: allow full flash access for capsule updates
    * 7814b8a6be Revert "soc/mediatek/mt8196: Specify MTKLIB_PATH for building BL31"
    * 14b66cb01b soc/intel/pantherlake: Add new SoC config for Intel Wildcat Lake(WCL)
    * d14ebe3957 mb/google/fatcat/var/felino: Use GPP_C08 for GPIO_PCH_WP
    * 6322be7992 sb/intel/bd82x6x/me.h: Add missing definitions
    * 2f62dd8a6b mb/google/brya/var/uldrenite: Configure ISH_GP5 GPIO
    * 3ce612194c mb/google/rex: Generate RAM IDs
    * 430ab9257b spd/lp5: Add SPD for K3KL8L80EM-MGCU
    * c7a450ba7d Documentation/mainboard/asrock/imb-1222.md: Update information
    * ac7717a7b0 mainboard/asrock/imb-1222: Enable USB3 port in WWAN slot
    * eb68ff66eb mb/asrock/imb-1222: Update GPIO config using new intelp2m
    * a1210875e9 mb/imb-1222: Update some GPIOs according to new vendor config
    * fc8e88da9b drivers/intel/mipi_camera: Rework info print output
    * d04d7d80b0 drivers/intel/mipi_camera: Only generate ADR if no HID supplied
    * 36c89598a7 mb/erying/tgl: fsp_params: Replace half_populated with statement
    * 0307f52cd9 soc/mediatek/mt8196: Move SPM loader functions to common part
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit f6da49b3a7ba59b04668dbc9e9b031df14678114
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 17:54:51 2025 +0100

    deguard: bump to rev 0ed3e4f
    
    There really isn't anything functionally different. However,
    this means one less patch is needed in lbmk.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 81dc1a7f897d95e272e25969a59ccad48bb49879
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 15:48:02 2025 +0100

    GRUB: Bump to rev a68a7dece, 23 June 2025
    
    NOTE: gfxterm_menu module removed, because of this
    change by upstream:
    
    commit ca2a91f43bf6e1df23a07c295534f871ddf2d401
    Author: Glenn Washburn <development@efficientek.com>
    Date:   Mon May 5 16:11:36 2025 -0500
    
        tests: Disable gfxterm_menu and cmdline_cat tests
    
    This brings in the following changes from upstream:
    
    * a68a7dece loader/i386/pc/linux: Fix resource leak
    * de80acf36 loader/efi/linux: Unload previous Linux kernel/initrd before updating kernel size
    * 249db11d8 loader/efi/linux: Correctly terminate load_options member
    * f3b339af1 loader/efi/linux: Use sizeof() instead of constant
    * c2b2e0dcf loader/efi/linux: Use proper type for len variable
    * de4e8e2aa loader/efi/linux: Do not pass excessive size for source string
    * 8c8f96664 loader/efi/linux: Remove useless assignment
    * 8ebf155af include/grub/charset.h: Update documentation
    * 2f2ed28d5 Revert "lzma: Make sure we don't dereference past array"
    * 2539ede82 tests/util/grub-shell: Correct netboot and file_filter test failure
    * 8c2d4e64f normal/charset: Fix underflow and overflow in loop init
    * ba8eadde6 dl: Provide a fake grub_dl_set_persistent() and grub_dl_is_persistent() for the emu target
    * 409e72ced util/grub-protect: Correct uninit "err" variable
    * 5eca564b1 gnulib: Bring back the fix for resolving unused variable issue
    * ac1512b87 gnulib: Add patch to allow GRUB w/GCC-15 compile
    * db506b3b8 gnulib/regexec: Fix resource leak
    * bba7dd736 gnulib/regcomp: Fix resource leak
    * 91cb7ff6b tests/tpm2_key_protector_test: Add tests for SHA-384 PCR bank
    * 451e227e5 tpm2_key_protector: Dump the PCR bank for key unsealing
    * 11caacdb2 util/grub-protect: Fix the hash algorithm of PCR digest
    * ce23919ca build: Add new header files to dist to allow building from tar
    * e3b15bafd build: Remove extra_deps.lst from EXTRA_DIST
    * 40e261b89 lib/LzmaEnc: Validate "len" before subtracting
    * 86e8f2c4b osdep/unix/hostdisk: Fix signed integer overflow
    * 438f05581 disk/luks2: Add attempting to decrypt message to align with luks and geli modules
    * 20e6d0c4a osdep/linux/getroot: Detect DDF container similar to IMSM
    * b71bc0f8b fs/fshelp: Avoid possible NULL pointer deference
    * 272ff81cb fs/ntfs: Correct possible infinite loops/hangs
    * 8c95307a0 fs/ntfs: Correct possible access violations
    * 06914b614 fs/ntfs: Correct attribute vs attribute list validation
    * 0e1762c8a fs/ntfs: Correct regression with run list calculation
    * be303f8c1 lib/envblk: Ignore empty new lines while parsing env files
    * 34bd00ee2 fs/zfs: Fix another memory leak in ZFS code
    * ca2a91f43 tests: Disable gfxterm_menu and cmdline_cat tests
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 5b2661a485f172e3b6298981df1d171951db9a1b
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 15:31:48 2025 +0100

    SeaBIOS/default: Bump to rev b686f460, 28 Jun 2025
    
    This brings in the following changes:
    
    * b686f460 sercon: Fix keycodes for F11 and F12
    * b52ca86e docs: Note v1.17.0 release
    * a6c8e8bb ahci: Fix hangs due to controller reset
    
    The serial console fix is useful to us, as is the AHCI
    fix; the latter was previously mitigated by removing
    SeaBIOS's AHCI reset patch.
    
    Upstream realised that the AHCI controllers need to have
    a timeout on them when resetting them, because they don't
    always react immediately to commands.
    
    This makes the AHCI behaviour more correct, in SeaBIOS.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 248192ad9a13b998080c5cda62c56a814a6651d8
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 04:34:14 2025 +0100

    ifd/hppro3500: use truncated ME, enlarge BIOS size
    
    i did:
    
    ifdtool -f layout.txt ifd.bin
    
    changed layout.txt to say this:
    
    00000000:00000fff fd
    00019000:007fffff bios
    00001000:00018fff me
    00fff000:00000fff gbe
    00fff000:00000fff pd
    
    then i did:
    
    ifdtool -n layout.txt ifd.bin -O ifd.bin
    
    this was done to the ifd for hp 3500 pro, based on
    the 96KB size of the truncated me.bin via me_cleaner,
    when downloading vendor files in lbmk.
    
    it's the policy of libreboot that me.bin should always
    be shrunk, and the BIOS region enlarged.
    
    in the original HP 3500 PRO patch submitted, the ME region
    was larger, with region boundaries like this:
    
    00000000:00000fff fd
    00400000:007fffff bios
    00001000:003fffff me
    00fff000:00000fff gbe
    00fff000:00000fff pd
    
    In the above example, you see that the BIOS region is 4MB.
    
    In the new setup, BIOS is about 7.9MB.
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 1cd8353082377d4e64bc9c11400b88e17b5293ce
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 04:25:22 2025 +0100

    ifd/hppro3500: unlock regions by default
    
    coreboot already unlocks the regions during build, by default,
    anyway, and this was present in the submitter's patch.
    
    however, it's also good to unlock the IFD regions. like so:
    
    ifdtool --unlock ifd.bin -O ifd.bin
    
    this has been done, on the ifd for hp pro 3500
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 67858207eb229f5eae9e23e211dbb25289392f34
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 04:22:48 2025 +0100

    ifd/hppro3500: set HAP bit by default
    
    ifdtool --altmedisable 1 ifd.bin -O ifd.bin
    
    always remember to do this, when adding a new
    ifd to lbmk. i merged the 3500 port anyway, since
    the submitted already used me_cleaner anyway, but
    setting the HAP bit is also useful. for example, if
    someone was to only flash the BIOS region, which is
    possible in this case since the submitter also
    didn't truncate the ME region or enlarge the BIOS
    region.
    
    in that case, flashing IFD and BIOS is another valid
    way to do it, where IFD's HAP bit is set
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit a13772bf31fc2e63c38067a3baffe3cabd011b0d
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 04:15:22 2025 +0100

    cb/hppro3500: use seagrub, not grubsea
    
    We want graphics cards to work out of the box. This is
    why SeaGRUB is default, on desktops; SeaBIOS also has
    better code quality and is less likely to break, so it
    provides a nice fallback in case the GRUB payload is ever
    buggy during development (this decision was made ever
    since the botched May 2024 release)
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 047ddf40d8f9418df49d465d9364600621c3cc09
Merge: d25aaac9ad 587af4a7b6
Author: Leah Rowe <vimuser@noreply.codeberg.org>
Date:   Sun Jul 6 05:10:17 2025 +0200

    Merge pull request 'Add HP Pro 3500 Series' (#350) from JoelLinn/lbmk:feature-port-hppro3500 into master
    
    Reviewed-on: https://codeberg.org/libreboot/lbmk/pulls/350

commit d25aaac9ad9a366c05f0b67af9cf9c8e3b35479c
Author: Leah Rowe <leah@libreboot.org>
Date:   Sun Jul 6 03:25:35 2025 +0100

    lib.sh: remove erroneous break from fx_
    
    it means nothing here. in context, if a non-zero return
    is observed, we should not do anything here, which is
    already the behaviour anyway, except that "break" means
    nothing since we're not in a loop here.
    
    where an error exit should be observed, x_ is used inside
    the command given for fx_
    
    Signed-off-by: Leah Rowe <leah@libreboot.org>

commit 587af4a7b601198f4d701d5e81c2299b5df8413d
Author: Joel Linn <jl@conductive.de>
Date:   Sun Jun 15 15:20:32 2025 +0200

    Add HP Pro 3500 Series
    
    Everything should work except cpu fan control because ME cleaning breaks PECI.
```

I had other stuff going on in my personal life so didn't do much Libreboot
development, between October 2025 to November 2025. This is why the release
has been pushed back to January 2026 instead of December 2025.

When life throws you lemons, make tomatoes.
