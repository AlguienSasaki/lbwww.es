% Libreboot build system audit plus next-release plans
% Leah Rowe
% 13 June 2023

Introduction
------------

Literally about 200+ changes have been made to the Libreboot build system,
since the last release of Libreboot. This has been the primary focus, thus far.

In recent weeks, Libreboot's build system has gone through an intense audit and
until recent *days* has undergone extensive testing. There are still a few more
things to be done. The purpose of this audit has been to:

* Look for and fix bugs
* Improve code quality (nicer coding style)
* Reduce code repetition

This concerns only *lbmk*, the [automated build system](../docs/maintain/) that
Libreboot uses. This is what Libreboot uses to build coreboot, payloads,
utilities, ROM images and, ultimately, Libreboot releases, so one could argue
that lbmk *is* Libreboot. It automatically downloads, patches, pre-configures
and compiles everything from scratch, but in a way that is simplified from the
perspective of the end user (almost everything is just a single command, where
users are typically not required to edit any files unless they want to).

### Brief summary of changes

In short, the following work has been performed:

* [OpenBSD coding style](https://man.openbsd.org/style.9) more rigorously
  adopted, on some C utilities included in lbmk; though this was already the
  case for some of them.
* That same BSD coding style has been *adapted* for use with shell scripts.
* Vastly improved error handling, on some scripts (in many cases, errors that
  were previously unhandled are now handled fully).

Example of build system cleanup recently performed: all scripts in the main
directory of lbmk are now consolidated into a *single* shell script. Similar
cleanup is underway for other parts of the build system.

Libreboot's build system is already very efficient. It's *only* a few thousand
source lines (about 2200, at the last count). Libreboot's build system provides
the minimal glue necessary to download, patch, configure and build ROM images
for end users. The mentality behind is is that every codebase on average
probably has the same percentage of bugs, so *less code* means less bugs, so
*less code* is *good*. Many people ask *how* but the most important question
is usually *why* (or *when*). Libreboot's build system tries to avoid bloat
and *feature creep* when possible, intentionally refusing to implement certain
features that are considered superfluous.

Auditing of utilities
---------------------

This process has *also* been applied to some of the utilities (written in C)
that Libreboot includes as part of lbmk. The following utilities have been
audited:

* `spkmodem-recv` (more on this in a moment)
* `e6400-flash-unlock` (minor cleanup, basically removing one unused function)
* `nvmutil` (massive code size reduction, minor fixes here and there)

### Pledge

The `nvmutil` program was already pledged, when compiled on OpenBSD, but it was
not handled well. It is now handled correctly (correct ifdef rule), and it
has been *hardened* greatly.

The code has also been unveiled. See manpages:

<https://man.openbsd.org/pledge.2>

<https://man.openbsd.org/unveil.2>

### spkmodem-recv

This utility was *added* after the last release. It was imported from coreboot,
which in turn previously forked it from GRUB. It is a receiving client for
spkmodem, to provide a serial console via pulses on the standard *PC speaker*.

Libreboot's version is *heavily* re-factored, doing away with the messy GNU coding
style and replacing it with a BSD coding style (the licensing is unchanged).

In addition to code style changes, certain variables are renamed for clarity,
and certain redundant variables removed. Logic split into functions, and the
code is conditionally *pledged* if you compile it on OpenBSD (for OpenBSD),
see: <https://man.openbsd.org/pledge.2>

Other plans for next release
----------------------------

I have a bunch of Dell/HP boards that I plan to add, which I would have added
already but I've focused on the audit (which is more or less complete, now).

Besides this, I also wish to:

* Re-add Tianocore UEFI payload, on boards where this is feasible, providing it
  as an option alongside existing GRUB/SeaBIOS payloads
* Provide a Linux kexec payload, based on the Linux payload provided by Heads.
  I was initially working on a port of the OpenBSD userland to Linux and musl
  first, because I want to replace busybox, but this is taking too long so I've
  shelved it and will release that at a later date. The Heads build system is
  already excellent for this purpose (though they use busybox, in their linux
  distro), and I intend to adapt (directly adapt) their build system to be
  used in lbmk (re-implement the same logic, but in shell scripts, where Heads
  currently uses a lot of Makefiles that I find messy in comparison to lbmk's
  way of doing things - though their way also has its benefits)
* If time, test more ChromeOS boards (gru-* but also the nyan, peach and veyron
  boards, most of which are untested. Only the gru platforms are known to work
  with Libreboots u-boot payload, at present).

FULL list of changes so far since last release
----------------------------------------------

Not all patches are listed below, if they are patches not relevant, or patches
not currently pushed to upstream Libreboot git mirrors. The following patches
are available, live:

(time references such as "36 minutes ago" are relative to ~1.30am UK time
on 13 June 2023)

```
* ac0bd172 - (HEAD -> master) remove errant file (36 minutes ago) <Leah Rowe>
*   d617135d - (origin/master) Merge pull request 'lbmk: Fix regressions' (#77) from nic3-14159/lbmk:fix-lbmk into master (49 minutes ago) <Leah Rowe>
|\  
| * 0fade1b6 - lbmk: Fix regressions (61 minutes ago) <Nicholas Chin>
|/  
* b52a7f4f - util/spkmodem-recv: re-add full license header (2 hours ago) <Leah Rowe>
* 7ca9b987 - util/ich9gen: change default mac address (2 hours ago) <Leah Rowe>
*   e75dafa4 - Merge pull request 'Add 4MB version of HP 8200 SFF' (#72) from Riku_V/lbmk:hp8200sff_4mb into master (3 days ago) <Leah Rowe>
|\  
| * 0f7a5386 - Add 4MB version of HP 8200 SFF (2 weeks ago) <Riku Viitanen>
* |   e6d4aeb2 - Merge pull request 'Update Git revision for bios_extract' (#74) from nic3-14159/lbmk:update_bios_extract into master (3 days ago) <Leah Rowe>
|\ \  
| * | d059fefe - Update Git revision for bios_extract (3 days ago) <Nicholas Chin>
|/ /  
* | dee8f44b - util/spkmodem-recv: fix regression (5 days ago) <Leah Rowe>
* | f2822db9 - util/spkmodem-recv: make ringpos a global variable (7 days ago) <Leah Rowe>
* | 334bfedf - util/spkmodem-recv: simplify sample_cnt/char reset (8 days ago) <Leah Rowe>
* | 4a6b5827 - util/spkmodem-recv: print stats in other function (8 days ago) <Leah Rowe>
* | 2652a1dd - util/spkmodem-recv: only print unhandled err on -d (8 days ago) <Leah Rowe>
* | 3fb99a01 - util/spkmodem-recv: make debug a runtime option (8 days ago) <Leah Rowe>
* | 264a31b9 - util/spkmodem-recv: always disable line buffering (8 days ago) <Leah Rowe>
* | 118bb19f - util/spkmodem-recv: simplify stdout flush logic (8 days ago) <Leah Rowe>
* | af36cc7f - util/spkmodem-recv: rename variables for clarity (8 days ago) <Leah Rowe>
* | f7fccb59 - util/spkmodem-recv: split print_char() up (8 days ago) <Leah Rowe>
* | b40a30b1 - util/spkmodem-recv: reduce indent in print_char() (8 days ago) <Leah Rowe>
* | b21c1dd5 - util/spkmodem-recv: squash a few code lines (8 days ago) <Leah Rowe>
* | 3401f287 - util/spkmodem-recv: bsd-style indent (8 days ago) <Leah Rowe>
* | 2a6ad971 - util/spkmodem-recv: order prototypes per function (8 days ago) <Leah Rowe>
* | 212ce3a8 - util/spkmodem-recv: warn on unhandled exit error (8 days ago) <Leah Rowe>
* | 9a6d2908 - util/spkmodem-recv: another minor code cleanup (8 days ago) <Leah Rowe>
* | a61ab37b - util/spkmodem-recv: always set errno on err() (8 days ago) <Leah Rowe>
* | e8889fd1 - util/spkmodem-recv: minor code cleanup (8 days ago) <Leah Rowe>
* | 3c2a287e - util/spkmodem-recv: handle sample errors correctly (8 days ago) <Leah Rowe>
* | 979db74c - util/spkmodem-recv: simplify pulse check (8 days ago) <Leah Rowe>
* | 94aa43d8 - util/nvmutil: call unveil earlier, and harden (9 days ago) <Leah Rowe>
* | db63fcff - util/nvmutil: hardening: reduce pledges earlier (9 days ago) <Leah Rowe>
* | dbd6defe - util/nvmutil: fix faulty arg check (9 days ago) <Leah Rowe>
* | 270693fc - util/nvmutil: cleanup: move logic out of main() (10 days ago) <Leah Rowe>
* | 46a9eea0 - util/nvmutil: major cleanup. simpler arg handling. (10 days ago) <Leah Rowe>
* | c9fdfce3 - util/nvmutil: simplify writeGbeFile() (11 days ago) <Leah Rowe>
* | bdccd7cb - util/nvmutil: don't call writeGbeFile if O_RDONLY (11 days ago) <Leah Rowe>
* | 99258a38 - util/nvmutil: code cleanup (pledge/unveil calls) (11 days ago) <Leah Rowe>
* | 69fa333e - util/nvmutil: harden pledge/unveil calls (OpenBSD) (12 days ago) <Leah Rowe>
* | adf3aece - util/nvmutil: fix faulty fd check (12 days ago) <Leah Rowe>
* | b49da12d - util/nvmutil: only swap/copy if checksum is valid (12 days ago) <Leah Rowe>
* | 9aa34f1e - util/nvmutil: use bsd-style indentation (12 days ago) <Leah Rowe>
* | 18f39ab6 - util/nvmutil: clean up rhex() (12 days ago) <Leah Rowe>
* | 4d91bcc2 - util/nvmutil: check correct return value on close() (12 days ago) <Leah Rowe>
* | c2c31677 - util/nvmutil: massive code cleanup (12 days ago) <Leah Rowe>
* | f0846134 - util/nvmutil: move includes to nvmutil.h (12 days ago) <Leah Rowe>
* | 2dabafe6 - util/nvmutil: move xpledge/xunveil to nvmutil.h (12 days ago) <Leah Rowe>
* | 9a3e6516 - util/nvmutil: use SPDX license headers (12 days ago) <Leah Rowe>
* | 5d6af06a - util/nvmutil: move non-functions to nvmutil.h (12 days ago) <Leah Rowe>
* | a2136933 - util/nvmutil: use even more macros (code cleanup) (12 days ago) <Leah Rowe>
* | 5a9fac2a - util/nvmutil: remove unnecessary parentheses (12 days ago) <Leah Rowe>
* | 6885200c - util/nvmutil: simplify setWord() with word() macro (12 days ago) <Leah Rowe>
* | 7ab209d5 - util/nvmutil: do xor swap in a macro (12 days ago) <Leah Rowe>
* | 293ca0fc - util/nvmutil pledge,unveil: use correct err string (12 days ago) <Leah Rowe>
* | a1df8fd1 - util/nvmutil: ensure that errno is set on err() (12 days ago) <Leah Rowe>
* | 1f548604 - util/nvmutil: minor code cleanup (12 days ago) <Leah Rowe>
* | 8f1e6d79 - util/nvmutil: simplified error handling in main (13 days ago) <Leah Rowe>
* | 78fc8935 - util/nvmutil: Use unveil, and harden pledges (13 days ago) <Leah Rowe>
* | c2cd1916 - util/nvmutil: Harden pledge promises (13 days ago) <Leah Rowe>
* | c759a7a0 - util/nvmutil: Simplify use of pledge (on OpenBSD) (13 days ago) <Leah Rowe>
* | f37bd759 - util/nvmutil: Use correct pledge promise (OpenBSD) (13 days ago) <Leah Rowe>
* | 83ecf268 - util/*: Properly detect OpenBSD for pledge() call (13 days ago) <Leah Rowe>
* | 8df2f809 - util/e6400-flash-unlock: clean up commented code (2 weeks ago) <Leah Rowe>
* 06c92d4a - blobutil: merge with main script (2 weeks ago) <Leah Rowe>
* ff954c5b - unify download/build scripts (2 weeks ago) <Leah Rowe>
* 092600d1 - unify these scripts: build, modify and update (2 weeks ago) <Leah Rowe>
* 6344b196 - build/payload/seabios: reduced indentation (2 weeks ago) <Leah Rowe>
* 2be1a8ea - download/coreboot: fix error handling in subshell (3 weeks ago) <Leah Rowe>
* d0171eef - download/coreboot: don't needlessly re-download (3 weeks ago) <Leah Rowe>
* c616930b - download/coreboot: remove unnecessary bloat (3 weeks ago) <Leah Rowe>
* d1935c05 - build/clean/u-boot: remove unnecesssary check (3 weeks ago) <Leah Rowe>
* 676efbb0 - build/clean/u-boot: improved coding style (3 weeks ago) <Leah Rowe>
* 06a92f61 - build/clean/ich9utils: don't use subshell (3 weeks ago) <Leah Rowe>
* 43e2dfe2 - build/u-boot: top-down, split-function code style (3 weeks ago) <Leah Rowe>
* a8f0721a - build/payload/u-boot: 79 chars or less per line (3 weeks ago) <Leah Rowe>
* 89ac1ea5 - build/payload/u-boot: fix wrong attributions (3 weeks ago) <Leah Rowe>
* c973b959 - build/payload/grub: rename functions for clarity (3 weeks ago) <Leah Rowe>
* 51e0e401 - build/payload/grub: remove unnecessary check (3 weeks ago) <Leah Rowe>
* 8e206be7 - build/payload/grub: split logic into functions (3 weeks ago) <Leah Rowe>
* db7e8161 - build/payload/grub: 79 chars or less per line (3 weeks ago) <Leah Rowe>
* 92bd18c4 - build/release/roms: minor cleanup (3 weeks ago) <Leah Rowe>
* ec3d1006 - build/release/roms: handle argument properly (3 weeks ago) <Leah Rowe>
* e0b97660 - build/release/roms: remove superfluous comments (3 weeks ago) <Leah Rowe>
* 681538a2 - build/release/roms: handle errors inside subshell (3 weeks ago) <Leah Rowe>
* a9bd5442 - build/release/roms: split logic into functions (3 weeks ago) <Leah Rowe>
* 29833090 - build/release/roms: use tabs for indentation (3 weeks ago) <Leah Rowe>
* fff5fa53 - build/release/src: 79 chars or less per code line (3 weeks ago) <Leah Rowe>
* 1cdf1c7c - build/release/src: handle errors in subshells (3 weeks ago) <Leah Rowe>
* 16f878e8 - build/release/src: split logic into functions (3 weeks ago) <Leah Rowe>
* 4e2ee58a - build/ich9utils: simplify, fix error handling (3 weeks ago) <Leah Rowe>
* 93ec91e8 - build/memtest86plus: use tabs for indentation (3 weeks ago) <Leah Rowe>
* 4b80f250 - build/clean/crossgcc: better code style (3 weeks ago) <Leah Rowe>
* 187d5fa4 - build/descriptors: simplify and fix error handling (3 weeks ago) <Leah Rowe>
* a05be169 - build/grub: fix inconsistent indentation (3 weeks ago) <Leah Rowe>
* 02919c47 - build/grub: implement error handling (3 weeks ago) <Leah Rowe>
* 5bab3bbc - build/grub: introduce main(), split it up (3 weeks ago) <Leah Rowe>
* 277e1df0 - build/cbutils: remove unnecessary directory check (3 weeks ago) <Leah Rowe>
* ed9eb462 - build/cbutils: rename function for clarity (3 weeks ago) <Leah Rowe>
* b12dced4 - build/cbutil: avoid frivilous use of subshells (3 weeks ago) <Leah Rowe>
* 355a45b4 - build/cbutils: top-down coding style, main on top (3 weeks ago) <Leah Rowe>
* 9f58d4e4 - build/cbutils: 79 chars or less per line (3 weeks ago) <Leah Rowe>
* 691f2664 - build/cbutils: use tabs for indendation (3 weeks ago) <Leah Rowe>
* 3cbcfce9 - gitclone: add my copyright for recent changes (4 weeks ago) <Leah Rowe>
* 01a2ab37 - use env in shell scripts (4 weeks ago) <Leah Rowe>
* 1e8f2cc1 - gitclone: only rm the old directory at the end (4 weeks ago) <Leah Rowe>
* 3da8d20c - gitclone: stricter error handling (4 weeks ago) <Leah Rowe>
* e8048494 - gitclone: minor cleanup (4 weeks ago) <Leah Rowe>
* fd2ca12e - gitclone: split logic out of main() (4 weeks ago) <Leah Rowe>
* 08ad9eb1 - download/coreboot: minor cleanup (4 weeks ago) <Leah Rowe>
* 8d9570b6 - gitclone: cleaner coding style (4 weeks ago) <Leah Rowe>
* 4ac0bc8d - blobutil/download: minor code cleanup (4 weeks ago) <Leah Rowe>
* 9fb489ac - modify: clean up duplicated code (4 weeks ago) <Leah Rowe>
* f7f3aef1 - modify: cleaner coding style (4 weeks ago) <Leah Rowe>
* 34df727c - build: cleaner coding style (4 weeks ago) <Leah Rowe>
* 1a062bb6 - build: reduce code to less than 80 chars per line (4 weeks ago) <Leah Rowe>
* a212a5be - blobutil: exit 1 if a called script fails (4 weeks ago) <Leah Rowe>
* e6221571 - blobutil: cleaner coding style (4 weeks ago) <Leah Rowe>
* c08e3258 - .gitcheck: exit 1 if unsupported argument given (4 weeks ago) <Leah Rowe>
* c5122557 - .gitcheck: use subshells where appropriate (4 weeks ago) <Leah Rowe>
* dd8fb524 - .gitcheck: re-add redirection to /dev/null (4 weeks ago) <Leah Rowe>
* 82c4d7b2 - .gitcheck clean: clean coreboot directories too (4 weeks ago) <Leah Rowe>
* 0f3c3ca6 - .gitcheck: reduce indentation level for loop (4 weeks ago) <Leah Rowe>
* ecd7f1d1 - .gitcheck: move logic out of main() (4 weeks ago) <Leah Rowe>
* 829bc02b - .gitcheck: *actually* check coreboot directories (4 weeks ago) <Leah Rowe>
* 52bc07bc - .gitcheck: improved coding style (4 weeks ago) <Leah Rowe>
* 83235fb9 - .gitcheck: check argv when running gitcheck-clean (4 weeks ago) <Leah Rowe>
* 6ce77652 - .gitcheck: actually *run* gitcheck-clean (4 weeks ago) <Leah Rowe>
* 8782bff8 - download: code cleanup (4 weeks ago) <Leah Rowe>
* a232f9c5 - download: check for non-existent script in loop (4 weeks ago) <Leah Rowe>
* b4f1804e - download script: bugfix: gitcheck clean didn't run (4 weeks ago) <Leah Rowe>
* 62c88dfb - download script: improved coding style (4 weeks ago) <Leah Rowe>
* 5b594909 - util/spkmodem_recv: Use pledge but only on OpenBSD (4 weeks ago) <Leah Rowe>
* 25241ae2 - util/spkmodem_recv: Add -u flag (no line buffer) (4 weeks ago) <Leah Rowe>
* 01fdfa3a - util/spkmodem_recv: Tidy up global variables (4 weeks ago) <Leah Rowe>
* 50b35939 - util/spkmodem_recv: Make pulse variable global (4 weeks ago) <Leah Rowe>
* 14190de9 - util/spkmodem_recv: Use parentheses on comparisons (4 weeks ago) <Leah Rowe>
* c0f2bf30 - util/spkmodem_recv: Move global variable: pulse (4 weeks ago) <Leah Rowe>
* 5d03598b - util/spkmodem_recv: Purge unused global: amplitude (4 weeks ago) <Leah Rowe>
* 63e43819 - util/spkmodem_recv: Remove unused variable: pos (4 weeks ago) <Leah Rowe>
* a0abcb9f - util/spkmodem_recv: Re-order functions for clarity (4 weeks ago) <Leah Rowe>
* 93cc6642 - util/spkmodem_recv: Handle output in new function (4 weeks ago) <Leah Rowe>
* 27866e65 - util/spkmodem_recv: Re-order prototypes (4 weeks ago) <Leah Rowe>
* 8b851258 - util/spkmodem_recv: Rename functions for clarity (4 weeks ago) <Leah Rowe>
* 6c1bf756 - util/spkmodem_recv: Return errno in main (4 weeks ago) <Leah Rowe>
* c23806e1 - util/spkmodem_recv: Use correct printf specifier (4 weeks ago) <Leah Rowe>
* 0cc23b23 - util/spkmodem_recv: Add error handling (4 weeks ago) <Leah Rowe>
* 17932381 - util/spkmodem_recv: Move logic out of main (4 weeks ago) <Leah Rowe>
* 3d554294 - util/spkmodem_recv: Rename variable for clarity (4 weeks ago) <Leah Rowe>
* 697ae5e2 - util/spkmodem_recv: Remove use of static keyword (4 weeks ago) <Leah Rowe>
* 2c12e70c - util/spkmodem_recv: Rename variable for clarity (4 weeks ago) <Leah Rowe>
* 5b6f5cb0 - util/spkmodem_recv: Remove space in function calls (4 weeks ago) <Leah Rowe>
* abc5cfd3 - util/spkmodem_recv: Say frame in English (4 weeks ago) <Leah Rowe>
* e2864704 - util/spkmodem_recv: Top-down logic (main on top) (4 weeks ago) <Leah Rowe>
* 3722c1e6 - util/spkmodem_recv: simplified pulse check (4 weeks ago) <Leah Rowe>
* 88683b76 - util/spkmodem_recv: Define argc/argv in main (4 weeks ago) <Leah Rowe>
* 83b34e2f - util/spkmodem_recv: Reduced indentation in loop (4 weeks ago) <Leah Rowe>
* 22633e0d - util/spkmodem_recv: Use tabs for indentation (4 weeks ago) <Leah Rowe>
* 9152d0f9 - util/spkmodem_recv: Add clean to the Makefile (4 weeks ago) <Leah Rowe>
* 754410f2 - util/spkmodem_recv: Define CC in the Makefile (4 weeks ago) <Leah Rowe>
* f2887e9b - util/spkmodem_recv: Add strict CFLAGS (4 weeks ago) <Leah Rowe>
* b496ead7 - util/spkmodem_recv: Import from coreboot (4 weeks ago) <Leah Rowe>
* 52d87f5f - download/coreboot: minor code cleanup (4 weeks ago) <Leah Rowe>
* 7bd206b9 - download/coreboot: remove errant code (4 weeks ago) <Leah Rowe>
* bd82d90f - download/coreboot: tidy up variable names (4 weeks ago) <Leah Rowe>
* 66d06afd - download/coreboot: run extra.sh from cbtree (4 weeks ago) <Leah Rowe>
* c4b0825c - download/coreboot: avoid variable conflict (4 weeks ago) <Leah Rowe>
* 0e1e9c17 - download/coreboot: fix downloads without argument (4 weeks ago) <Leah Rowe>
* bea67353 - download/coreboot: much cleaner coding style (4 weeks ago) <Leah Rowe>
* 2d69072a - download/coreboot: clone upstream via ./gitclone (4 weeks ago) <Leah Rowe>
* c17423e4 - download/coreboot: simplify check (4 weeks ago) <Leah Rowe>
* 00cafd70 - download/coreboot: fix misnamed function (4 weeks ago) <Leah Rowe>
* 86512e84 - download/coreboot: simplify small if statements (4 weeks ago) <Leah Rowe>
* d28584f3 - download/coreboot: fetch config in new function (4 weeks ago) <Leah Rowe>
* 162f4bf5 - download/coreboot: use global variables (4 weeks ago) <Leah Rowe>
* 56b80c0a - download/coreboot: rename function for clarity (4 weeks ago) <Leah Rowe>
* ee79d8ba - download/coreboot: reduce indentation in loop (4 weeks ago) <Leah Rowe>
* f858baea - download/coreboot allow downloading specific trees (4 weeks ago) <Leah Rowe>
* a33e5c67 - download/coreboot: split config check to function (4 weeks ago) <Leah Rowe>
* 62038f1d - download/coreboot: fix misnamed variable (4 weeks ago) <Leah Rowe>
* 342e846f - download/coreboot: consistent function declaration (4 weeks ago) <Leah Rowe>
* c32ae597 - download/coreboot: rename function for clarity (4 weeks ago) <Leah Rowe>
* e47aaa8f - download/coreboot: prune errant comments (4 weeks ago) <Leah Rowe>
* 31d8fcd3 - download/coreboot: split main() (4 weeks ago) <Leah Rowe>
* 4c2cff5e - download/coreboot functions: rename board variable (4 weeks ago) <Leah Rowe>
* 7a6f40fc - download/coreboot: top-down re-ordering (4 weeks ago) <Leah Rowe>
* fd8b8084 - download/coreboot: simplified for loops (4 weeks ago) <Leah Rowe>
* b24fbc74 - download/coreboot: move initial logic to main() (4 weeks ago) <Leah Rowe>
* 2871db15 - download/coreboot: RFC 2646 compliance (4 weeks ago) <Leah Rowe>
* 8b4c1c16 - download/coreboot: consistent tab indentation (4 weeks ago) <Leah Rowe>
* 1388cccb - build/seabios: cleaner coding style (4 weeks ago) <Leah Rowe>
* ddad8f00 - build/seabios: simplify. stricter error handling (4 weeks ago) <Leah Rowe>
* b74e4078 - blobutil/download: cleaner coding style (4 weeks ago) <Leah Rowe>
* 557272fa - download/mrc: stricter error handling (4 weeks ago) <Leah Rowe>
* 7b36ffc1 - download/mrc: handle exit status within subshell (4 weeks ago) <Leah Rowe>
* 963b5247 - download/mrc: use cleaner coding style (4 weeks ago) <Leah Rowe>
* d89585fb - gitclone: check for invalid patch filename (4 weeks ago) <Leah Rowe>
* db3c1d9c - download/grub: delete grub if gnulib cloning fails (4 weeks ago) <Leah Rowe>
* d90dfb0a - build/dependencies/*: RFC 2646 compliance (4 weeks ago) <Leah Rowe>
* 48bda9e0 - update/coreboot: top-down coding style (4 weeks ago) <Leah Rowe>
* a35f0b65 - blobutil/extract: minor code style cleanup (4 weeks ago) <Leah Rowe>
* 009bf3b6 - blobutil/extract: split up extract_blobs() (4 weeks ago) <Leah Rowe>
* fd3936cc - blobutil/extract: cleaner coding style (4 weeks ago) <Leah Rowe>
* 1f8ad1e4 - blobutil/extract: simplified main() (4 weeks ago) <Leah Rowe>
* 1ffb32b7 - blobutil/extract: top-down logic (4 weeks ago) <Leah Rowe>
* 423e2033 - blobutil/extract: RFC 2646 compliance (80 chars) (4 weeks ago) <Leah Rowe>
* 26dfda0c - blobutil/inject: print script path on error (4 weeks ago) <Leah Rowe>
* 6289eeb5 - blobutil/inject: fail if gbe.bin doesn't exist (4 weeks ago) <Leah Rowe>
* 54f8a453 - blobutil/inject: check that me.bin exists (4 weeks ago) <Leah Rowe>
* d34f3813 - blobutil/inject: check me path (4 weeks ago) <Leah Rowe>
* 5da7554a - blobutil/inject: remove errant debug message (4 weeks ago) <Leah Rowe>
* 70e337af - blobutil/inject: use x86 top-aligned mrc offset (4 weeks ago) <Leah Rowe>
* 17429788 - remove errant code lines from last commit (4 weeks ago) <Leah Rowe>
* ee0b200f - blobutil/inject: massively improved coding style (4 weeks ago) <Leah Rowe>
*   75ad8b0d - Merge pull request 'Remove warning for coreboot images build without a payload' (#65) from nic3-14159/lbmk:remove-no-payload-warning into master (4 weeks ago) <Leah Rowe>
|\  
| * fdc9e444 - Remove warning for coreboot images build without a payload (4 weeks ago) <Nicholas Chin>
* | f2e31767 - modify/u-boot: cleaner coding style (4 weeks ago) <Leah Rowe>
* | 71cac866 - modify/coreboot: cleaner coding style (4 weeks ago) <Leah Rowe>
* | 174d3af7 - modify/seabios: cleaner coding style (4 weeks ago) <Leah Rowe>
* | c8dfc3cc - build/build/roms: simplify mkCoreboot() arguments (4 weeks ago) <Leah Rowe>
|/  
* d8a8a1c6 - build/boot/roms: don't use subshells frivilously (4 weeks ago) <Leah Rowe>
* 834be77c - build/boot/roms: remove errant debug line (4 weeks ago) <Leah Rowe>
* 39c14398 - build/boot/roms: simplify build_rom_images() (4 weeks ago) <Leah Rowe>
* 65dfdd56 - build/boot/roms: use fast dd command for ich9m ifd (4 weeks ago) <Leah Rowe>
* 6a4ce66f - build/boot/roms: don't run ich9gen twice (4 weeks ago) <Leah Rowe>
* 1e9ed989 - build/boot/roms: simplify moverom() (4 weeks ago) <Leah Rowe>
* 5811e53e - build/boot/roms: remove unused legacy code (4 weeks ago) <Leah Rowe>
* 3bd82b76 - build/boot/roms: reduced code indentation (4 weeks ago) <Leah Rowe>
* 9eee0fb4 - build/boot/roms: split main() to topdown functions (4 weeks ago) <Leah Rowe>
* bceb5f2e - build/roms_helper: move logic into main() (4 weeks ago) <Leah Rowe>
* df611f9b - remove ga-g41m-es2l board for now (5 weeks ago) <Leah Rowe>
* 3da0ee4f - remove python3 patches (5 weeks ago) <Leah Rowe>
* 6290f999 - build/boot/roms_helper: further cleanup (5 weeks ago) <Leah Rowe>
* 722c844e - build/boot/roms: top-down function order (5 weeks ago) <Leah Rowe>
* 5f44556f - build/roms: general code style cleanup (5 weeks ago) <Leah Rowe>
* d521fca7 - build/roms: fix faulty keymap list expansion (5 weeks ago) <Leah Rowe>
* 67a607b8 - build/boot/roms*: RFC 2646 compliance (5 weeks ago) <Leah Rowe>
* 79939f2f - Add devicetree patch for E6400 with Nvidia GPU (5 weeks ago) <Nicholas Chin>
* 3f1ee015 - seabios: do normal config, disable oprom in vgarom (5 weeks ago) <Leah Rowe>
*   450f19bd - Merge pull request 'hp9470m: fix board name in smbios' (#57) from Riku_V/lbmk:master into master (5 weeks ago) <Leah Rowe>
|\  
| * 15ad5a00 - hp9470m: fix board name in smbios (5 weeks ago) <Riku Viitanen>
|/  
| * 8e378ee4 - (e6400nvidia_wip) dell/e6400nvidia: switch to "normal" config (5 weeks ago) <Leah Rowe>
| * a9f81e44 - Revert "Revert "coreboot/e6400nvidia: don't run vga rom"" (5 weeks ago) <Leah Rowe>
| * 499fa421 - seabios: do normal config, disable oprom in vgarom (5 weeks ago) <Leah Rowe>
| * 4ee5e2af - Revert "coreboot/e6400nvidia: don't run vga rom" (5 weeks ago) <Leah Rowe>
| * 2b2f5992 - coreboot/e6400nvidia: don't run vga rom (5 weeks ago) <Leah Rowe>
| * ba5a7cf9 - coreboot/e6400nvidia: debug level 8 (5 weeks ago) <Leah Rowe>
| *   f8db8519 - Merge pull request 'Add devicetree patch for E6400 with Nvidia GPU' (#51) from nic3-14159/lbmk:e6400-nvidia into e6400nvidia_wip (5 weeks ago) <Leah Rowe>
| |\  
| | * 9a7be395 - Add devicetree patch for E6400 with Nvidia GPU (5 weeks ago) <Nicholas Chin>
| * | 4e5fb60c - New board: Dell Latitude E6400 (Nvidia GPU model) (5 weeks ago) <Leah Rowe>
| | | * 4a6d2d2b - (wip_tianocore) wip tianocore (5 weeks ago) <Leah Rowe>
| |_|/  
|/| |   
* | | ee46c042 - update the makefile (5 weeks ago) <Leah Rowe>
|/ /  
* | 5a197b4f - blobutil: support downloading E6400 VGA ROM (5 weeks ago) <Leah Rowe>
* | 0729d6e6 - Merge pull request 'Add patches for bios_extract' (#49) from nic3-14159/lbmk:master into master (5 weeks ago) <Leah Rowe>
|\| 
| * 2e64f639 - Add patches for bios_extract (5 weeks ago) <Nicholas Chin>
|/  
* f5150f26 - remove e6400_8mb and e6400_16mb (keep e6400_4mb) (5 weeks ago) <Leah Rowe>
* 6d0ff028 - Import new util: bios_extract (5 weeks ago) <Leah Rowe>
* f820e304 - add e6400_flash_unlock binary to .gitignore (5 weeks ago) <Leah Rowe>
*   a52c9952 - Merge pull request 'Add fedora 38 other unifont dependencies' (#45) from MrArthegor/lbmk:master into master (6 weeks ago) <Leah Rowe>
|\  
| * bc85118c - add fedora 38 unifont dependencies (6 weeks ago) <Arthegor>
|/  
* f49eccee - util/e6400-flash-unlock: do void on ec_fdo_command (6 weeks ago) <Leah Rowe>
* 6588be67 - don't force console mode in grub (7 weeks ago) <Leah Rowe>
```


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
