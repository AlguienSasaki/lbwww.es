% Libreboot 26.10 "Ominous Olive" released! With full UEFI support.
% Leah Rowe
% 15 October 2026

**You can find today's release in 
<https://rsync.libreboot.org/stable/26.10/> or you can use one of
the [mirrors](../download.md).**

Libreboot is free, open source boot firmware replacing proprietary BIOS/UEFI
firmware on supported Intel/AMD and ARM64 hardware. It initialises the hardware
inside your computer such as the CPU or memory controller, configuring the
machine in such a way as to enable use of an operating system like Linux or BSD
at boot - and now *Microsoft Windows* is also compatible because Libreboot has
added full UEFI support via MrChromebox EDK2 payload. Libreboot offers greater
security and customisation compared to proprietary firmware; its goal, plainly,
is to rid the world of proprietary software and to do that, we therefore try
to provide firmware that is extremely easy for non-technical users to understand.

Today's release changelog is relative to the previous Libreboot 26.01 release.

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>
<img tabindex=1 class="r" src="https://av.minifree.org/t580.jpg" /><span class="f"><img src="https://av.minifree.org/t580.jpg" /></span>

This is a very special release, with several major design changes made
regardless of hardware changes, so users should read this
changelog *very carefully* - and read it twice.

First thing's first. (and yes, you read the previous paragraph correctly)

A strongly anti-AI policy was added to a new file: `AGENTS.md` in lbmk. This
is the most important change in the release. Now everything else:

WARNING: MAJOR design changes were made to this release, in ways that may
affect lots of existing setups. We now use EDK2 as the default payload, with
GRUB/SeaBIOS provided in flash as EFI builds for backward compatibility.
Everything works, but there are some caveats so please read this release
changelog very carefully.

The first major change is:

233 Intel/AMD Chromebooks added
---

The full list of boards is quite long, so it's shown at the end of this
article. All 233 of them!

MrChromebox supports nearly every Chromebook, and this release imports all
boards from that project. This includes everything up to the most recent
Intel and AMD hardware.

Imported all 233 currently supported Chromebooks from the MrChromebox coreboot
distro. Baytrail (Kip) tested - others need testing. Source-only in this
release (must build from source, no binaries provided).

This was done with a utility I wrote at `util/mrimport/` which generates
configs for lbmk, including revisions and submodules. I made further tweaks
manually; further updates upstream can be synced in Libreboot by diffing again
the output of `mrimport` between revisions, adapted into Libreboot
irrespective of the latter's modifications which are numerous - and will be
covered next.

Libreboot makes these modifications to MrChromebox coreboot, in its version:

* Uses the EDK2 UEFI payload for coreboot, provided by MrChromebox; we
  set the background colour to Libreboot purple, and use the Libreboot rainbow
  logo instead of the coreboot logo. Our EDK2 build is otherwise currently
  identical except for the next changes:
* Set HAP bit by default, at build time, to disable Intel ME. It is not
  reliably disabled in MrChromebox coreboot by default as most people using it
  do so via software-based flashing scripts alongside disablement of Google's
  security as shown in MrChromebox documentation. We disable it here. NOTE:
  We also support numerous AMD Chromebooks, which are unrelated in this context.
* Unlock IFD regions via ifdtool (with quirks properly handled), setting them
  read-write at build time. This is done instead of modifying every Intel
  Flash Descriptor provided by MrChromebox, of which there are hundreds.
* CSMWrap added inside EDK2 (SeaBIOS CSM wrapped in EFI) for backward
  compatibility with bare metal SeaBIOS as in older Libreboot releases.
* GRUB EFI build provided inside EDK2, for backward compatibility also - it
  behaves, from a user's perspective, exactly like the coreboot-payload
  version of GRUB and even reads configs from CBFS in the same way.
* MemTest86+ added inside EDK2.
* Haswell chromebooks: native raminit used instead of MRC as in upstream.
* All Intel chromebooks up to Kabylake: use libgfxinit instead of GOP;
  upstream has since done this on newer gens as well, and this will be
  integrated post-release.

The additional payloads are, as above, available in the boot menu via ESC
keypress at startup. iPXE is also enabled by default, as in upstream, on
specific boards. EFI GRUB and CSM SeaBIOS have **replaced** the coreboot
payload versions of these; bare metal SeaBIOS/GRUB are now deprecated
and may be removed in a Libreboot future release, although they can be
re-enabled by those willing to build Libreboot from source.

This is NOT a fork of MrChromebox, which is to be regarded as Libreboot's
upstream on Intel/AMD Chromebooks. Efforts will be made, as with regular
coreboot, to remain as closely in sync with it as possible at all times. I
have worked extensively with MrChromebox, and even submitted a few minor fixes
to his project; he also implemented CBFS-based VBT and GOP handling per my
request, which I will integrate post-release.

Regardless, MrChromebox's own installation scripts should work, with Libreboot
images, so check out MrChromebox documentation for installation. Despite changes,
Libreboot's setup remains behaviourally compatible with the MrChromebox one
and, thus, MrChromebox tooling should still work alongside corresponding
documentation.

*This is made possible* by a new feature in Libreboot's build system: support
for an arbitrary number of upstreams per project. We still provide regular
coreboot on non-Chromebook mainboards.

Yes, 233. I counted. All automated. My scripting and my fastidiousness ensured
that what I imported and configured in lbmk closely matched upstream, and I'm
quite careful at maintaining my own divergence from it inside Libreboot.

Yes, Libreboot runs on Chromebooks now!

15h.org fork for Fam15h
----

E.g. KGPE-D16. We use the coreboot fork from 15h.org now, on AMD Fam15h
mainboards. This uses AMD AGESA source code instead of Timothy's custom
init code. It has much more reliable raminit and CPU initialisation; more RAM
also supported. Many bugs are fixed. IOMMU works, I also saw that they
mentioned TPM is fixed in their version, now our version also.

Regular binaries with SeaBIOS and GRUB payloads provided - we also have
experimental EDK2-based images, currently source-only (no binaries available).
EDK2 requires a framebuffer to be set up, and the integrated AST2050 chip
only starts in text mode on native initialisation; the vendor VGA ROM can set
up VESA, but we will not use the VGA ROM. The EDK2-based images are source-only
on these boards, intended for use with graphics cards.

Basically, these boards actually work for the first time.

EDK2 UEFI payload by default
---

EDK2 is Intel's reference implementation of the UEFI specification, for
booting. It's typically used by vendors, in proprietary forks of Intel's
BSD-licensed code, but Intel's reference code is Free Software. We use their
reference code, and full source code is provided in this Libreboot release.
EDK is short for: EFI Development Kit.

More specifically:

We use a fork of EDK2 provided by MrChromebox, which specifically offers use
of EDK2 as a generic coreboot payload. This is a complete, industry-standard
UEFI implementation capable of booting any compliant operating system.

All. Nearly all boards use it in Libreboot now, even older ones e.g. ThinkPad
X200. Tested on all platforms. Older, 32-bit-only mainboards still use the
setup from the previous release, while virtually all 64-bit-compatible hardware
now use EDK2. EFI builds of GRUB and SeaBIOS are provided in flash, accessible
from the EDK2 setup menu (press ESC at boot time when prompted), for backward
compatibility with existing Linux/BSD installations in Libreboot.

EDK2 support was integrated as a prerequisite for Chromebooks, but then it
was extended for use on *all* previously existing supported mainboards in
Libreboot. It even runs well on really old machines e.g. ThinkPad T500.

All Libreboot boards now use an open source UEFI implementation by default,
where feasible. *All of them*, and it's *by default* - the bare metal GRUB
and SeaBIOS coreboot payloads have been removed, replaced by EFI versions
that are backward compatible.

We previously did not provide UEFI at all, except for U-Boot on a few select
mainboards. We now provide full UEFI, with all four freedoms intact, on nearly
every mainboard that Libreboot supports.

All of the following operating systems were tested and confirmed working,
on various Libreboot machines using EDK2:

* Windows 10 and 11
* 9front (fork of Plan 9)
* various other UEFI-compliant operating systems
* Linux UKI bootloaders, systemd-boot, etc. UKIs work perfectly now, booting
  from EDK2. Modern Linux distro setups now work (GRUB not required).
* OpenBSD with EFI bootloader and GPT partitioning (older SeaBIOS txtmode can
  only do MBR partitioning in Libreboot)
* FreeBSD in UEFI mode - txtmode no longer required!
* ReactOS *did not work*, unfortunately (no UEFI support)
* RedoxOS probably works too.

Modern computers and operating systems are all UEFI, and this new setup can
boot basically all of it reliably. More testing will be required, but this is
battle-tested code and it has been used extensively in coreboot by many users,
for many years. Libreboot finally has it as well.

This change alone is part of a much wider effort to modernise Libreboot, and
I'm only getting started. The next Libreboot releases will draw heavily on
this, as EDK2 is pretty much the expected payload on a lot of modern coreboot
setups these days. Integration of EDK2 removes all limitations in Libreboot,
allowing us to support much newer hardware and it basically makes Libreboot
much easier to use; I expect Libreboot to become much more popular and useful
over time, as a result.

Some caveats and extra notes are provided at the end of this release page,
for migration purposes. For example, despite boasting about *all*, older
32-bit-only hardware still use bare GRUB/SeaBIOS. Nearly all boards use
EDK2 however!

Modern GPU support (via EDK2)
---

Finally, some notes about graphics cards that may be of interest to the more
demanding users.

EDK2 alongside PCI-E relocatable BAR (rebar) means that you can now use modern
graphics cards that require allocations above 4GB. Rebar is *not enabled* by
default, but you can enable it in menuconfig (coreboot). Enabling it causes
breakage of Intel GPU video output on libgfxinit in testing, and no fix was
found yet, so rebar is disabled by default.

PCI-E Option ROMs are enabled by default where necessary, for EDK2 builds.
This means you can use it on desktops with graphics cards. Tested on
several desktops (Dell OptiPlex 9020 SFF and 5050 SFF).

On boards with only external GPU, rebar can be left enabled by default. On
systems with functioning integrated graphics, accidental enablement of rebar
can be remedied by inserting a graphics card; when the video output in the
payload does become broken, the Linux i915 video driver still initialises
video framebuffer properly in KMS/DRM.

This will be added to general documentation, pending fixes upstream. Note that
even with this issue, Linux video output works fine. This will be corrected and
rebar enabled by default, on a future Libreboot release.

Upstream revisions
---

Nearly all major upstream revisions updated:

* Flashprog: updated to 24 September 2026 revision
* EDK2: imported version 26.09.2
* GRUB: updated to version 2.16
* Coreboot (default tree): Updated to 8ce4c91c41da3f042979f92095149a85bc93227d
  commit ID as of 18 September 2026.
* SeaBIOS (non-CSM): Updated to 81ec9ec0bcf45df11fb7f98339ec9036b546fca0
  commit ID as of 28 July 2026.
* Coreboot (fam15h): removed (`4.11_branch` tree) and replaced with:
* Coreboot (fam15h_15h): 57e26bdcd959808c0171164f3c6197d4a347c3b2
  commit ID as of 27 May 2026. This is a heavily improved fork of coreboot
  from version `4.11_branch`, for improved initialisation reliability on
  AMD FAM15H hardware e.g. ASUS KGPE-D16.

The `4.11_branch` tree of coreboot was removed, because we now use the
15h.org fork of coreboot on AMD Fam15h hardware. This is much improved, with
proper AGESA-based initialisation which initialises CPUs and memory controller
more reliably. More RAM is supported, and several features were fixed too
e.g. IOMMU and TPM.

The use of multiple coreboot upstreams was enabled by a design improvement in
lbmk that makes all projects multi-tree, just with some projects ony having one
tree in use. We can have N number of upstreams per project now, enabling use
of forks.

The coreboot update brings in GCC 15.2, MPC 1.4.1, Nasm 3.02, acpica 20251212
and binutils 2.46.1 on crossgcc.

NEW hardware support
---

All 233 chromebooks currently supported by MrChromebox have been imported and
adapted for use in Libreboot. They *all* compile. All of them. I repeat:

* 233 Chromebooks imported from MrChromebox. Every Chromebook supported by
  the MrChromebox project can now also be built in lbmk, with lbmk's changes.
  Thank you Matt DeVillier for your amazing project!

More hardware added besides Chromebooks:

* Dell OptiPlex 5050 SFF support, courtesy of Ron Nazarov.
* Supermicro X11SSH-F mainboard support - improvements made by Ron Nazarow
  e.g. use of regular Intel ME neutered through `deguard` - no bootguard,
  but deguard can also be used generally to provide a pre-configured ME per board.
  This board otherwise uses SPS. With use of regular ME, we do need a larger
  ME region than with cleaned SPS. Use of regular ME means that the MRC
  caching works, and thus raminit training is much faster on reboots (no
  need to retrain on literally every boot).
* ThinkPad X280 support courtesy of AlguienSasaki
* ThinkPad X270 support courtesy of Kat Inskip
* Dell OptiPlex 3040 Micro support courtesy of Todd Baker
* Removed `release="n"` on the ThinkPad X301 target. It actually boots just
  fine, although it is quite slow. EDK2 even works.

We add SeaBIOS CSM and GRUB EFI builds
inside EDK2, for parity with Libreboot's older GRUB/SeaBIOS payload setups.
The EFI payloads work very much the same.

Use of EDK2 makes it easier to support newer machines, which will be the focus
from now on. EDK2 came first, as part of a general modernisation effort;
newer Intel and AMD hardware will be added over time, making exclusive use
of EDK2 payload by default.

Feature changes
---

This is very much a features release. A *lot* of feature changes were made!

* EDK2 payload support on all Intel/AMD boards with at least 4MB flash.
  This uses the MrChromebox version of EDK2, and Libreboot has made extensive
  modifications e.g. add GRUB EFI and SeaBIOS CSM (CSMwrap) builds to it; these
  are provided as backward compatibility with older lone SeaBIOS/GRUB payload
  setups and should still boot existing Linux/BSD installations.
* iPXE boot method added via EDK2 payload (ESC menu) - only EDK2 (not available
  in the lone SeaBIOS payload on older boards).
* Added new command `./mk import_uboot` for auto-importing U-Boot configs
  that can then build in lbmk. Not yet used, but this will be used to add
  tested U-Boot boards in future releases (U-Boot itself providing its own
  initialisation). Libreboot can already build U-Boot images from any config,
  without code changes, only config changes, but testing is needed on hardware.
* New util at `util/mrimport` that is used to automatically import/update new
  configs from MrChromebox coreboot. This was also used for the initial import
  of 233 chromebooks from MrChromebox, importing them into the Libreboot build
  system with our own modifications.
* Added `fwsetup` command in a GRUB menuentry on the EFI version, which boots
  back into the edk2 setup menu.
* Modified IFD on Kabylake boards, for supporting pin mod. This enables use of
  six- and eight-core Intel CPUs.
* Randomised MAC addresses at build time, only when `XBMK_RELEASE` is unset.
  You can export a MAC address via environmental variable `XBMK_MAC` to set
  it arbitrarily. This avoids the need to extract NVM post-build and re-insert,
  when working directly on `.rom` files instead of release tarballs, which
  already have this capability via `./mk inject`.
* GRUB `at_keyboard` driver: always force scancode set 2 with translation. This
  was already done, but only on the coreboot target; it is now also done on
  the EFI target, by just applying it universally to all targets. This is because
  it's possible to use it with PS/2 keyboards on EDK2. It mitigates buggy
  PS/2 controllers that might not report modes correctly even when set, as
  on the Dell Latitude E6400.
* I made Memtest86+ purple (on the EDK2 build). Libreboot purple. Red value
  0x28, green value 0x0b and blue value 0x22. This is Libreboot Purple.
* I made EDK2 purple too, and added Libreboot's rainbow logo to it.
* Scan every ESP directory in a given partition, via asterisks, on the GRUB
  payload (EDK2 and coreboot targets). This means that it will boot a larger
  number of non-UKI Linux distro setups if they use GRUB or Syslinux configs.
* TPM enabled where available, on all targets that use EDK2.
* Added `./mk cbtest` command which tests board names versus lbmk and coreboot
  defconfig, in case it changes upstream. Otherwise, we can invisibly change
  the actual board that builds for a given target if the default one is then
  chosen by Kconfig rules. There is no way to detect this automatically at
  build time in a way that is clean, so this command should be used when
  updating coreboot revisions.
* Added `./mk test` command which aggressively builds everything, much more
  than even `./mk release` - it intentionally disables use of caching, so that
  downloads are aggressively tested as well. Config-related commands are also
  tested. This is a brutal stress test command used to find build errors.
* Flashprog: always keep the flash IC hot during command loops, rather
  that doing it in a toggle. The `--workaround-mx` option is retained, but
  no longer does anything as its behaviour is always applied. This improves
  general reliability as a lot of users don't know about this option anyway.
  More people can reliably flash their boards now.
* New mkhelper type added: preconfig. This lets you run custom logic prior
  to configuration on a given project that uses config files. It also works
  on noconfig projects. This is used for example on coreboot, to build utils.
* Removed used of ccache everywhere in the build system, as it was causing
  subtle build errors (and possibly code bugs) in several stress test
  scenarios. We'd rather wait a bit longer for a reliable build. CCache is
  really only meant for development, not production.
* Removed the `-c` and `x` (clean and xgcc-clean) flags in lbmk, as they are
  largely superfluous; lbmk already auto-cleans everything before build
  anyway, to remove all build artifacts, and aggressively caches builds out
  of tree based on config metadata, deleting and re-fetching sources from
  local cache as required. These options were useless.
* Properly skip builds if makefile checks fail; this allows single-tree
  projects to build, by preventing error-prone use of copy_elf where no
  files exist.
* mk: Removed GCC/GNAT version pairing as upstream (coreboot) now does
  this its its buildgcc script. It's redundant in lbmk. The coreboot version
  is better because it simply errs and exits, instead of trying to continue;
  the latter is what we did, which might actually cause more bugs in some
  cases. Better to fail hard, than corrupt the build.
* Danish keymap added for GRUB.

Design changes
---

A lot of major design changes have been made!

* GRUB boot config simplified; only one menuentry now for boot, which boots
  everything, trying USB first. It selects the first config it finds, but you
  can press ESC to cycle through to the next one in the loop. Useful for
  dual-booting multiple Linux distros.
* Short defconfigs now used on coreboot, instead of full configs. This allows
  for easier maintenance, and lbmk has been modified to accomodate them. The
  inject scripts still resolve full configs automatically at runtime, needed
  for scanning paths at build time. The use of defconfigs reduces the chance
  of erroneous changes in config during updates, and makes use of olddefconfig
  possible instead of the less-reliable oldconfig command.
* New safety check: verify that a path prefix is set, when using it in logic.
  Otherwise, it's possible that we might have directory escape bugs in lbmk.
  This is checked relentlessly, throughout the build system.
* No longer export `XBMK_CACHE` as it's now hardcoded in config. The new
  handling is much stricter, and less likely to regress in future revision.
  This was done to reduce the possibility of a directory escape bug in lbmk.
* Fixed GRUB module list no longer used per GRUB tree. Instead, all modules
  are added per target but a blacklist is applied, configured per-tree.
* GRUB Syslinux and GRUB scanning now combined into one function, on GRUB
  configs. General simplifications to the GRUB config.
* GRUB is now *one* source tree, with multiple targets all re-using the
  very same tree where differing configurations are used.
* Modified MrChromebox coreboot to only add EDK2 GOP binaries if no video
  mode is selected in coreboot defconfig. This avoids adding it unnecessarily
  and thus saving space in the flash, where FSP init or libgfxinit is used.
* Always use `--mirror` git clones on `include/env/get.sh`. This makes cloning
  revisions more reliable on all projects, especially ones where we use revs
  inside branches other than main.
* Auto-unlock IFD regions at build time on all Intel IFD-based systems; disable
  ME automatically, by setting the HAP bit. This is how we do it on chromebooks,
  but it's done on all boards; this mitigates the possibility that we might
  accidentally forget to neuter the ME on specific targets in the future.
* When building EDK2, we use coreboot's build system, but when doing it this
  way, `custom_` prefix is not used in image file names. Proper edk2 prefix is
  used. This makes it work for Chromebooks, which build edk2 per-board in this
  release; generalised EDK2 builds aren't handled on Chromebooks. On non
  Chromebooks, we build a dummy ThinkPad T440p build with EDK2 and SMMSTORE,
  then we extract that for insertion into other boards in a more generalised
  way via shim logic in `include/fw/rom.sh`.
* Removed the `fspcc36ae2b5775fa7400cb3282680afc0f6cb37a3c` submodule in
  coreboot, used on Topton X2E N150. We now use the latest FSP, but this means
  breaking older builds when doing vendor injection. This is unavoidable.
* Added a `noconfig` variable that can be set by build trees/targets, where
  they are multi-tree but no configs are used. This makes it appear under
  a fake name called `noconfig`, e.g. `elf/grub/default/efi/noconfig/grub.elf`
  when compiled.
* `get.sh`: use `-L` in find command, because some projects now use symlinks
  to metadata from other projects to ensure rebuilds. For example, edk2
  coreboot targets (dummy targets) rely on grub and seabios efi builds. When
  these builds have to change, we need to ensure that the edk2 coreboot build
  is re-done. Here, we mean the find command used for finding files to hash
  when checking whether a given project has changed in build system setup.
* Properly sort submodules by directory name, so that recursive submodules can
  be handled in projects. This is in the submodule handling on `get.sh`.
* `get.sh`: support arbitrary submodule config path, for projects that use
  multiple versions of a submodule. For instance, EDK2 uses multiple versions
  of brotli.
* SeaGRUB removed; on boards that still have the coreboot-payload version
  of SeaBIOS and GRUB, we now only do images that have GRUB or SeaBIOS as the
  primary payload. SeaBIOS's payload loading isn't always reliable, and GRUB
  has improved its allocator considerably snice a few years ago. We also now
  use EDK2 on most boards, so testing GRUB is the minority of boards that still
  use it exclusively is much more feasible. This simplification is
  permanent, given that standalone SeaBIOS/GRUB are now obsolete; EDK2 is the
  preferred payload now, with EFI versions of SeaBIOS and GRUB spliced into
  the EDK menu for backward compatibility with older Libreboot setups.
* Stronger use of `make distclean` on projects before building, made feasible
  due to Libreboot's aggressive out-of-tree caching of project builds based
  on metadata checks in lbmk.
* Allow `noconfig=n` on a project, which allows it to be multi-tree and use
  the same tree on different targets, even when build configs aren't required;
  in some cases, configs are done exclusively in mkhelper settings. This is
  part of Libreboot's decision to make all projects multi-tree, alongside
  allowing use of N upstreams per project instead of just one upstream.
* Simplify `release=n` handling in lbmk; don't execute mkhelpers. Skip source
  fetches if release=n, so that the corresponding source won't be downloaded.
  This is necessary to exclude MrChromebox sources as well as builds in
  releases, because they aren't ready for stable release in Libreboot yet.
* `init.sh`: Portable `date` usage; git's own date handling now used. We
  no longer rely on GNU-specific extensions to the `date` command, for git
  repository initialisation on release builds.
* Allow running `mk` from outside the main work directory. The build system
  now changes work directory accordingly.
* Enable 3-way-merge when running git am in `get.sh` - this enables easier
  updates on project revisions.

Config changes
---

* Most boards still use CBFS or NVRAM option table (static option table, or
  modify CBFS). A few boards, like the HP EliteBook 820 G2 and Topton X2E N150,
  use SMMSTORE-based option config but it has not yet been set up in lbmk.
* Set `me_state` to `Disabled` by default, tree-wide in coreboot (default tree).
  We already set the HAP bit in IFD anyway, but this is an extra layer of
  disablement, to make sure that the Intel ME is deader than dead.
* Optimised GRUB modules; all FS modules now included, fewer errors shown in
  the GRUB shell at boot. Modules generally optimised to ensure that there is
  still not a lot of flash space used compared to the default upstream setup.
* Added `xxd` to dependencies, needed by CSMWrap when making SeaBIOS EFI builds
  as it runs `xxd -i` on SeaBIOS CSM builds to create header files that are
  then wrapped inside CSMWrapper's shim code.
* Corrected the Dell OptiPlex 5050 DMI string code to say 5050 SFF, not
  3050 Micro.
* Don't use dell.com as primary download mirrors in the vendor inject script.
  This reduces the number of download errors when people inject images that
  use deguard.
* Scan *all* GRUB configs on all devices, without using fixed device lists in
  GRUB. Instead, asterisks are used. More BTRFS subvolumes are also checked,
  also via use of asterisks.
* General cleanup on GRUB configs. Redundant features removed e.g. manipulation
  of gfxterm. The default setup works fine. GRUB never worked reliably for
  on serial output anyway and most modern computers don't support any of the
  interfaces that GRUB supports in this context.
* Added ImageMagick to dependencies (needed for EDK2 boot logo).
* SMMSTORE enabled by default, on all targets that use EDK2 as a payload.
  This means that Libreboot users can now enter a standard setup menu at boot
  time, configuring their machines (e.g. boot order) in a way that is both
  persistent and does not require re-building the firmware.
* Added 4MB and 8MB targets for KCMA-D8 and KGPE-D16 mainboards. This, along
  with use of the 15h.org fork of coreboot on these boards, for improved
  raminit and various bug fixes. It uses AMD AGESA source code instead of
  the custom source code written by Raptor Engineering. The modified AGESA
  code has better CPU and memory initialisation, supporting more hardware
  combinations and it supports higher amounts of RAM.
* Removed T440p targets that use 4MB CBFS. Only the full images are provided
  now. The 4MB images might have encouraged someone to skip overwriting the
  extra components in Intel ME, and there's no Soft ME Temporary Disable
  on Haswell. We don't want to encourage bad habits.

Bug fixes
---

* Merge patch from Alguien fixing IFD regions (set read-write) and blanking the
  OEM section on ThinkPad X280. HyperThreading was also disabled.
* Preventative build fixes: removed used of `-Werror` on all projects, where
  feasible.
* Fixed a hanging bug on ThinkPad X230 targets, caused by an optimisation - 
  now reverted for this fix - that optimised the CAR setup in coreboot. This
  fix reverses coreboot revision `97dbfd3098ae12ef3f51e5f95cd6d66c25214205`.
  This revert was done by HustlerOne in their new Snowboot project, and copied
  from Snowboot into the Libreboot project.
* Added fake menuconfig Make options to several projects, so that the related
  mk options don't cause build errors (instead, a harmless no-op now occurs).
* mk: Only copy defconfigs back when running config commands if they actually
  exist. This fixes `./mk -s seabios` which never saves a `defconfig` file.
* Backported several build fixes to our early 2025 revision of U-Boot, so that
  it builds on modern distros e.g. latest Arch Linux updates. (needed so
  that GRU Kevin/Bob chromebooks can continue to build).
* Removed `src/gpio-scripts` due to unclear licensing on the version used.
  (no license given on the changes to it, forked to standalone from coreboot
  where it fell back to coreboot's COPYING file, but the fork repo did not
  have any licensing in it whatsoever - what changes were made was also
  unclear as a result).
* Fixed several broken vendor file paths on Intel/AMD chromebook boards
  imported from MrChromebox. These fixes were submitted and merged upstream
  as well, in the MrChromebox project.
* vendor.sh: catch all make variables in coreboot defconfigs, running
  make-olddefconfig and resolving them all to their proper values, where
  paths are involved. they are converted in a way friendly to sh, processed
  in such a way as to avoid accidentally running errant subshells.
* Safer handling of defconfigs; stricter error checks, including on noconfig
  projects.
* Fixed missing infinite loop detection in `mk` when resolving trees/revisions
  in projects. Trees that refer to other trees infinitely are now detected.
* Fixed pico-serprog build errors on GCC 16 (tested on the latest Arch Linux)
* Use `std=gnu2x` on coreboot/default instead of `std=gnu23`; gnu2x is code
  compatible with gnu23 on newer GCC versions, but older versions used in
  e.g. 2024 Linux Mint release will fail to build otherwise. This fixes
  older distros with no behaviour change on newer ones.
* Don't build `bios_extract` in `src/biosutilities` - there was no build issue,
  but we don't actually use `bios_extract` at all in Libreboot. We use some
  scripting from that repository, that does not require compilation.
* Updated Dell OptiPlex 3040 Micro `ME_bin_hash` so that auto-download
  works, as it was broken on initial import.
* `vendor.sh`: Use `-w` and not `--whitelist` on `me_cleaner` commands, since
  the latter failed for some reason in some cases. Use of `-w` was reliable.
* ONLY skip `make` when `if_skip_build` is set in the main `mk` script,
  for more thorough mkhelper execution on all projects.
* Stricter preventation of build routine in GRUB when dry build mode is used;
  in some cases, it may have tried to add a non-existent GRUB payload to a
  non-existent ROM that was never compiled. Checks like these have been
  tightened up and generalised everywhere in lbmk.
* Added missing error handling on `command -v` usage in a few places when
  setting up lbmk.
* Preventative fix: call `x_` in mk, wrapping `xbmk_init`. The latter never
  returns non-zero status, so this fix is purely preventative in case the
  function is ever redesigned at some point to do that.
* mk: Fix globbing issue in multi-tree command args. We previously populated
  a string with arguments, but now we use dollar-ampersand and cycle through
  arguments using `shift` which allows for proper double-quoting to be handled.
* Fully export git author/commit date when re-initialising lbmk on release
  builds. This is done on every related git command, for deterministic
  operation.
* Fam15h coreboot: patch out `__unused` variables and cast them to `(void)`
  instead, where unused, to prevent build errors in GCC 16.
* Gentoo Linux dependencies setup added to lbmk.
* Fix offline GRUB builds. The build system caches sources and repos, but
  GRUB had custom logic in it for downloading unversioned PO files from
  translationproject.org - so when I did my FOSDEM talk, my demo failed
  because the internet was down, while I wrongly believed I had everything
  cached. GRUB was modified to not download the files, and lbmk caches them
  instead. They are also now version-checked, by virtue of checksum; no
  versioning is actually provided upstream, but their contents were checked
  and further updates will inspect the diffs between them. Basically, whatever
  is on translationproject.org pertains to the current HEAD in GRUB. It's fine
  because the way gettext works is that if a literal string is not defined,
  it defaults back to original language. In this context then, use of the
  wrong PO files will just result in some sentences not being translated.
  Still, a bug in the way the files are handled might allow for a... buffer
  overflow, on unversioned files that GRUB was downloading without checking
  hashes. Yes. Libreboot fixed this reproducibility problem by forking the
  repository containing the files, keeping it static. Next time I do a talk,
  my demo won't fail. I fixed this literally a few hours after my talk.

Nvmutil changes
---------------

A lot of changes were made to `util/nvmutil/` to the extent that it deserves
its own release log:

* Generally safer use of macros (use brackets).
* Removed errno-based error handling. Just return zero or one, based on logical
  errors, but do not rely on the status of errno for error exits. Only use
  return values as appropriate to POSIX in libc functions. However, errno reset
  is handled more reliably accordingly, after return from libc functions; not
  required but also not counterindicated by POSIX. - dell-flash-unlock errno
  handling also fixed in the same way. We previously relied heavily on errno
  for exit, in ways that were not portable. Not all libc will properly reset
  it, and may set it randomly even when a given function logically returns with
  success. Any number of things could set it, so it's not a reliable mechanism
  for general error handling.
* Don't use `-pedantic` flag - also remove this on `spkmodem-recv`. Reduces
  the chance of pedantic build errors based on warnings.
* General correctness fixes; bound checks, proper handling of word alignment,
  use of uintptr when converting pointers, etc. The code is generally much
  safer, in terms of memory management, as a result of extensive auditing.
* EINTR-safe wrappers for various functions; loop until EINTR clears, where
  error occurs, otherwise err-exist on non-EINTR errors. Properly ignore EINTR
  on `close()` if function returns with error (EINTR on close is not an error
  condition in POSIX, and the descriptor should be disregarded in such scenario).
* More testing and fixes for OpenBSD, especially on memory management (much
  stricter libc in OpenBSD).
* Generalised I/O functions for libc functions like `read`, write`, `pread`
  and `pwrite` - EINTR-safe, with stricter (than libc) checks against incomplete
  writes and such.

NOTE: This applies to the standalone version of nvmutil. There was an attempt
to rewrite it in a much larger utility, mentioned in the following section
about canceled changes for this release (see notes below about libreboot-utils).

General cleanup
--

Roughly one thousand lines of dead code were cleaned out of the build system,
but several hundred more were added due to new feature changes. Overall, the
amount of code in lbmk has been reduced, in spite of its advances in this
release.

Specific cleanup includes:

* Projects under `config/` centralised better, e.g. `config/module/coreboot/`
  instead of `config/coreboot`. The directory `config/ifd/` is moved
  to `config/data/coreboot/ifd/` as it's only ever used from coreboot.
* The `config/` directory is generally better organised now, with more
  unified placement of files per-project.
* Moved all vendor files into `vendorfiles/` e.g. `vendorfiles/mrc/` to
  de-clutter the main work directory.
* Removed cbfstoolref config; it's now hardcoded as we always use the
  same coreboot 4.11 tree for that version of cbfstool (refcode extraction
  on broadwell boards).
* Systematically removed unused variables in the build system.
* mk: new `forx` function allows macro-like shorthand for loops, greatly
  simplifying much of the code.
* Removed several redundant checks in the build system, and generally tightened
  up all error handling.
* Various random and minor code cleanups; minor dead code removal in the build
  system.
* Generalised several macro'd mkhelper checks in lbmk, refactoring and reducing
  overall code complexity.
* `lib.sh`: Made `build_sbase` function a macro instead, so that it can be
  changed all in one place. It's literally just one
  command (`make -C util/sbase`).
* Simplified `add_uboot` logic as it is only needed now for ARM64. The
  conditional logic for x86 was removed as we no longer use U-Boot on x86
  at all; EDK2 is now the preferred payload on x86, for UEFI.
* Removed ccache from dependencies as we no longer use ccache, for reliability
  reasons.
* `inject.sh`: removed end message confirming successful insertion, and
  generally simplified all variable initialisation.
* `release.sh`: Removed the `-m` option as it's useless. There is no context
  in which we want to generate a source archive without also building binaries.
  What's the point in doing half a release? This is dead code. Knobs are for
  nobs.
* Cleanup in `vendor`sh`.
* Unrolled several loops in lbmk, generalising them into macro-like functions.
  This allowed for a decent code size reduction while also increasing code
  readability. Macro-like programming is quite feasible in sh when you do it
  right.
* Used `awk` heavily in certain parts of the build system, to further unroll
  a few loops, especially in the `err` function.
* Removed u-boot-specific make-clean handling in mk; mk is more generalised
  now anyway. U-Boot was making use of git-clean but only on lbmk Git, not
  releases; so what's the point? We want the behaviour on either to be in as
  much parity as possible, and doing unnecessary work is, well, unnecessary.
* Greatly simplified the `findpath` function in mk. It's now one line of code,
  using `forx`. (wrapper function for `readlink -f` and `realpath`.
* Added error check (stricter one) on `xbmkpwd` initialisation in mk.
* Removed `xprintf` in mk; we now just printf ampersand into awk. Easy.
* Simplified `xbmkpwd` initialisation in mk; we now find the path to mk and cd
  to its base directory, which thus becomes `xbmkpwd`.
* mk: run `xbmk_init` from main script, not `init.sh`. This makes it clearer
  on first reading, where xbmk initialisation is called from. We do it just
  below the include of `init.sh`.
* Moved tree.sh into mk (merger). Everything centralised there, with custom
  functions now shimmed.
* All includes now done in bulk, and everything called from the main script.
  We previously included `tree.sh` (later merged into mk) conditionally. Now
  absolutely all state is global, which enabled further code simplication
  and variable cleanup.
* Simplified `get.sh` considerably. A lot of logic, especially submodule
  handling, was aggressively generalised.

Canceled changes
--

A few changes were worked on, but did not land in this release. They pertain
to two major projects:

### util/libreboot-utils

This was an extension of nvmutil that later became a generalised set of
utilities alongside a library for handling things like atomic writes, and
userspace mitigation against various types of TOCTOU attacks. Not really
critical for nvmutil itself, but...

An attempt was made to implement a hardened version of mktemp called mkhtemp.
It implemented several mitigations in userspace against TOCTOU attacks e.g.
directory replacement and symlink attacks.

Check lbmk git log for `util/libreboot-utils` to see more (might have
been `util/libreboot_utils`.

Mkhtemp was started when implementing atomic writes for nvmutil, but then I
naturally made use of tmpfiles for that, and that led to me auditing every
implementation of mktemp. I concluded that I could do better - and I did.

Examples of how mkhtemp innovated:

* Still provided libc-like behaviour when requested, but:
* Stricter checks against use of world-writeable non-sticky directories.
* Generally stricter error-exit handling.
* Relentless checks against changes to files during operation/check, inode
  changes and so on. Checking that a file wasn't operated on by another
  program - basically, openat2-like security was attempted to implement in
  userspace, though openat2 was also enabled as a fast method on Linux.
* Own implementation of rand, wrapping around Linux getrandom and BSD
  arc4random. It provided various functions for rand, including one that
  matched the semantics of arc4random_buf (rset function in libreboot-utils).
* Descriptorless operation in general, on files and directories.
* Pathless operation where possible - including userspace directory walking
  and sandboxing.
* Unveil-like restrictions in userspace (and pledge/unveil usage in OpenBSD).
* Use of symlinks disabled by default.

...it was an over-engineered piece of crap, and had several memory issues
related to stack memory usage, which tripped up a lot on BSD. The could could
be tidied up and finished, but I came to a conclusion:

Distros would never accept it. Mkhtemp worked fine on Linux, but it had much
stricter operation that would trip up most build systems that use it. I tried
it in lbmk to replace mkhtemp, symlinking it in PATH, but for example GRUB
had virtually no error checking on mktemp. Most programs that use mktemp
wrongly assume that it can never fail, so it doesn't; implementations try
to mitigate errors and continue operation, in ways that are extremely complex.
Mkhtemp was a clean attempt at stricter TMPDIR usage.

Because I decided it was a waste of time, I don't think I care to write more
about it in this changelog. Check lbmk git log if you're interested.

### util/spkmodem-decode

A heavily extended version of `util/spkmodem-recv` that implemented much
stronger noise protection, attempted automatic tone detection and frequency
selection, plus more. I went a bit overboard with it and decided that it was
an overengineering effort. It will not be resumed at any point. You can
find it in lbmk git log. (might have been `util/spkmodem_decode`).

Pointless effort. Spkmodem the protocol is unusable in most setups regardless.

The most pointless effort ever, such as:

* Edge detection along with amplitude, for more reliable operation on noisy
  signals
* Recalibrate timing to signals periodically, adjusting for tone. The idea
  was ta allow for the tone separator to drift over time, and have our code
  adapt to subtle changes to reduce the amount of noise. Right now, spkmodem
  shows a lot of noise on poor signals.
* A bunch of other equally useless changes.

Completely pointless. The simpler code works just fine, so the effort
was abandoned and thus spkmodem-recv is still present; this is a cleaned up
version of what was inherited from GNU GRUB several years ago.

### Miscallaneous canceled work

Just two canceled works:

* Updated U-Boot to a late 2026 revision on ARM64 chromebooks, but the bootflow
  menu looks ugly now and barely works. Upstream completely broke it. Therefore,
  we still use the early 2025 revision of U-Boot that was there in the
  Libreboot 26.01 (January 2026) release.
* GRUB keymap support via parsing of `keymap.gkb` in CBFS when booting with
  EDK2. This was canceled because a better method will be implemented post
  release; use of HiiDatabase and a graphical config menu in EDK2 to change
  the keyboard layout and locale. This will be written to SMMSTORE and also
  used by payloads. For example, the GRUB EFI payload could read from SMMSTORE
  to know locale and then change its language accordingly, using its existing
  gettext integration (GRUB already supports internationalisation).

Hidden third cancelation, because there always is one:

Linuxboot. Now with EFI support, we can easily use any number of UKIs added
to flash if we want to - and we will, in a future release. The term Linuxboot
refers to any flash-based Linux distro that can boot other Linux kernels via
Kexec - my preference is ZFSBootMenu, or at least something inspired by it
as ZFSBootMenu's build system is thoroughly incompatible with Libreboot's
strict design principles.

(which is a polite way of saying that I hate podman/docker with a passion)

Linux UKIs are common now, for boot, and Libreboot EDK2 can easily integrate it.

U-Boot x86 payload REMOVED
---------

The ARM64 payload is still present, but EDK2 has replaced U-Boot on x86.

This deserves its own section, in this release, as it is a major design change.
EDK2 was not merely added as an option; it has *replaced* U-Boot on x86. I
could make this its own article. It deserves one. It deserves my judgement,
publicly as I'm now about to articulate in no uncertain terms:

U-Boot was used for lightweight UEFI implementation on Intel/AMD boards, but the
person chiefly in charge of the x86 coreboot payload for U-Boot has largely
forked U-Boot into a newly vibe-coded project; Libreboot opposes heavily the
use of AI, as per new policy. AI is no good, and upstream x86 U-Boot support
has largely bitrotted although several interesting patches were merged.

U-Boot is thus a liability on x86. EDK2 is more fully functional already, so
this is used instead. Think AI makes you a super hacker? Think again:

* <https://arxiv.org/abs/2507.09089>

U-Boot was always buggy on x86 anyway. It's really just not ready at all. It is
better used on RISCV and ARM64(where we continue to use it).

U-Boot upstream itself is staunchly anti-AI, at least at the time of this
release, and much discussion of the above was had on their mailing list. It
would not be productive to re-ligitate that on this release page, but it should
suffice to say that the Libreboot project considers U-Boot to be effectively
dead on x86. The ARM-based development is still ongoing strongly, upstream,
and we will continue to use U-Boot for UEFI on ARM64 (chromebooks for example).

I will not say his name, or the name of his fork, because I don't want to
promote him or his U-Boot fork. If you know his name or his project, you should
not say it or promote it either. AI is bad, very bad, for reasons here:

* <https://blog.codeberg.org/protecting-our-floss-commons-from-llms.html>

The fork in question, of U-Boot, is heavily AI vibe-coded and thus useless to
Libreboot. This is not mere conjecture; in said aforementioned U-Boot mailing
list thread, the author stated thus. We will not, in any way, encourage or
facilitate use of AI. It has been made clear that several projects Libreboot
uses also have some AI-assisted commits; GCC, GRUB, etc. This is not our fault,
but it is my intention that I
and others will audit these projects in the future, de-slopping them and possibly
even forking them entirely for that very reason. The author of the fork basically
went insane with it, proposing to the U-Boot upstream that his work be "custodian",
and merged slowly into the upstream U-Boot tree. He was basically pressuring
them to accept AI code against their wills. I say in my capacity as leader of
the Libreboot project that U-Boot should never allow AI to taint its project.
When you accept AI, you become deskilled and you erode the trust that exists
between you and your users. You remove the human connection between maintainers
and users, and your project will start to become dependent on what is - yes -
proprietary technology, paid for in blood. AI is one of the single biggest
consumers of electricity and water on the planet, leading to resource
shortages all over the world and its increasing control over what information
people see in the news is even a threat to democracy worldwide. This is before
even arguing about unclear licensing, copyright violations and so on. It was
decided that Libreboot would join the crusade against all artificial
intelligence. The countries chiefly in charge of AI training and infrastructure
are the United States (led by several companies currently attacking democracy
worldwide) and the People's Republic of China; even if all the other problems
with AI didn't exist, it would be a very bad idea to give these countries even
more power over our computing.

NO. Libreboot says no to AI. Thus, we will continue using upstream U-Boot as
they say no also, and we will not use any vibecoded forks of it. Even if that
causes me to have to spend weeks integrating a totally new system to replace
it. That is precisely, exactly and indubitably how far I will go to prove my
point.

That is why I went to such extreme effort integrating EDK2. Rather than
replace U-Boot with yet another incomplete UEFI implementation, I simply
added EDK2 which is already feature-complete as it is the reference,
industry-standard implementation of the UEFI spec.

CrabEFI was what I initially considered, to replace U-Boot. This is a new
payload written by Arthur Heymans, and the project itself is excellent, but
rustc/cargo is inherently unreliable for several reasons:

* Unpredictable toolchain setup. Most people use e.g. rustup and they set up
  all sorts of custom environments. Much less predictive than e.g. LLVM/GCC
  provided in distros. Libraries and such are provided by Cargo, instead of
  distros, and users often leave everything out of date, leading to breakage.
* Rust is not standardised; the specification is implementation-defined, and
  code written today may still compile in the future when the spec changes, but
  code behaviour would naturally change as a result of the latter.
* Given the above, we would have to bootstrap rustc/cargo ourselves, and we
  would have to do it from source (from a compiler like GCC which already
  has source bootstrapping courtesy of the Guix project and others).

Rust is awesome, but in my opinion not production-ready yet. Give it another
ten years. EDK2 is infinitely more complex, but the size of the code is not
a concern as it is generally stable, and written in C (some small bits of it
are in C++). It has the most amount of eyes on it, and regularly receives
security audits. It's one of the most battle-tested codebases on the internet,
and works well.

CrabEFI will likely be merged in Libreboot at some time in the future, when
Rust tooling has stabilised across distros (including BSDs). It has a much
better design, even if it's not yet feature-complete, and use of Rust means
that it will have fewer memory bugs. For now, Libreboot uses EDK2 to replace
U-Boot and it has been enabled by default on all boards.

On memory safety, we can say contrarily that EDK2 is at least stable; bugs
are also stable. That's the point. We have stable C/C++ compilers, properly
tested according to formalised standards (not really, because GCC and LLVM
do all sorts of whacky things too, but they also try to be compatible with
each other behaviourally). You want your code, when compiled, to behave
exactly the same, bugs included. This is more strongly guaranteed with
projects written in C and C++ (especially pure C projects, and most of EDK2
is in fact pure C).

It was originally intended that U-Boot would become Libreboot's primary payload
on x86, but alas. I bit the Intel-flavoured bullet and went with EDK2.

Say no to AI. AI makes you stupid. AI is the reason we can't have nice things.

Errata
------

### EDK2 issues

No ability to change the keyboard layout yet - a graphical menu option can
be added for this later in EDK2, saved to SMMSTORE. The EFI build of GRUB
uses `terminal_input console` only, and input is thus handled by the EDK2
input drivers (ditto USB mass storage and other storage devices); GRUB's
nativedisk driver is removed for example, and its native NVMe/xHCI drivers
are no longer needed as in previous Libreboot releases.

EDK2 input drivers work with both USB and PS/2 keyboards in Libreboot. GRUB
is made to use `console` input instead of `at_keyboard` as it otherwise
lacks USB keyboard support in the EFI build. Thus, layouts must be handled
by EDK2. There is code for this in EDK2, not currently handled by either
Libreboot or MrChromebox so it all defaults to UK QWERTY in this release.
Upstream (MrChromebox) uses US QWERTY by default, but Libreboot is a British
project, so EDK2 was patched accordingly. Work will be done post-release.

Keymapped images no longer provided. There is now only one ROM image per
config on each mainboard. This simplifies builds, and users can insert
a `keymap.gkb` file inside CBFS from `config/data/grub/keymap/` if they wish;
if you add a grub.cfg to flash with `terminal_input` set to `at_keyboard`,
this will work on devices with PS/2 keyboards if wanting to use the GRUB EFI
payload (a command in grub.cfg to load the keymap is also needed,
specifically `keymap (cbfsdisk)/keymap.gkb`.

EDK2 has been tested extensively on all Intel hardware from GM45 e.g.
ThinkPad X200/T400, all the way to Intel Kabylake hardware supported by
Libreboot e.g. ThinkPad T480, X280, Dell Optiplex 3050/5050.

Want old GRUB/SeaBIOS back?
----------------------

The new compatibility modules in EDK2 should more than suffice for most
users, who may still want GRUB/SeaBIOS for booting their existing Linux
and BSD setups. It is recommended that you convert these to EFI or just
reinstall them - use of UEFI is strongly encouraged as the default from
now on, but Libreboot must (will) continue supporting older setups.

FreeBSD users booting with SeaBIOS text mode may be interested in this,
as BIOS-mode FreeBSD bootloaders do not boot with anything else. A UEFI-based
FreeBSD installation will work just fine in EDK2 though, so new FreeBSD
installations are best recommended via EDK2 (do not use the compatibility
modules of GRUB/SeaBIOS).

Some users might want it. Here are some brief notes. They won't be added
to the main documentation as they pertain to a configuration that Libreboot
now regards as obsolete, but some users may nonetheless find it useful in
the short term:

Older mainboards that are 32-bit only or with limited flash space still use
the standalone GRUB/SeaBIOS payloads without EDK2; some newer ones with
weird graphics setups also still use lone SeaBIOS/GRUB e.g. HP EliteBook
8560w.

To re-enable use of standalone GRUB/SeaBIOS (without edk2), modify `target.cfg`
as follows for your board: remove `build_depend`, `payload_edk2` and `grubtree`
variables. Then add these lines:

```
payload_grub="y"
payload_seabios="y"
payload_memtest="y"
```

This is obsolete now in Libreboot and may even be removed in a future release.
EDK2 is much better as it boots nearly everything, and supports more
hardware setups. It is simply much more versatile than Libreboot's old setup
as in the previous release.

Then you can just re-build. NOTE: In the above guidance, `build_depend`
is assumed to be `grub/default seabios/build memtest86plus/default` by default.
You may want to set them explicitly:

```
build_depend="seabios/build grub/default memtest86plus/default"
```

You may also wish to set the GRUB tree explicitly:

```
grubtree="default"
```

Example config: `config/module/coreboot/t480_vfsp_16mb/target.cfg`

Just do this and re-build if you want these old setups back. This will be
supported for as long as feasible, but will probably be removed at some point,
maybe years from now, maybe a week from now. Use it while it's there. The
xHCI patches are still in there, and everything is configured as it should be.

In setups with EDK2, please note: txtmode not provided. If wanting e.g.
SeaBIOS txtmode, look at e.g.:

* `config/module/coreboot/t480_vfsp_16mb/config/libgfxinit_corebootfb`

Renamed it to `libgfxinit_txtmode` in this example and do:

```
./mk -m coreboot t480_vfsp_16mb
./mk -s coreboot t480_vfsp_16mb
```

In the menu that appears, go into `Devices -> Display -> Framebuffer mode`
and select `Legacy VGA text mode` - this is not recommended for any new
setups, but the FreeBSD bootloader starts in text mode and the framebuffer
modes in coreboot can't switch back to it. Booting in text mode is recommended
when booting the FreeBSD BIOS-mode bootloader, and then FreeBSD can be set up
for KMS in Xorg/Wayland. Otherwise, new installations should be done in EFI
mode directly via EDK2 (no compatibility modules). More information about
this is available in Libreboot's BSD documentation.

List of Chromebooks added
-----------

* Google Anahera
* Google Aurash
* Google Banshee
* Google Bujia
* Google Caboc
* Google Constitution
* Google Crota
* Google Dochi
* Google Felwinter
* Google Gaelin
* Google Gimble
* Google Gladios
* Google Jubilant
* Google Kano
* Google Kinox
* Google Kuldax
* Google Lisbon
* Google Lotso
* Google Marasov
* Google Mithrax
* Google Moli
* Google Moxie
* Google Anraggar
* Google Craaskov
* Google Craask
* Google Dirks
* Google Domika
* Google Epic
* Google Glassway
* Google Gothrax
* Google Guren
* Google Joxer
* Google Kaladin
* Google Meliks
* Google Nereid
* Google Nivviks
* Google Nova
* Google Pujjocento
* Google Pujjogatwin
* Google Pujjoga
* Google Pujjolo
* Google Pujjoniru
* Google Pujjo
* Google Quandiso2
* Google Quandiso
* Google Riven
* Google Rull
* Google Sundance
* Google Teliks
* Google Telith
* Google Tereid
* Google Tivviks
* Google Uldrenite
* Google Uldren
* Google Xivu
* Google Yaviks
* Google Yavilla
* Google Omnigul
* Google Osiris
* Google Primus
* Google Redrix4ES
* Google Redrix
* Google Taeko
* Google Taniks
* Google Vell
* Google Volmar
* Google Xol
* Google Coral
* Google Pyro
* Google Reef
* Google Sand
* Google Snappy
* Google Auron_Paine
* Google Auron_Yuna
* Google Buddy
* Google Gandof
* Google Guado
* Purism Librem 13 v1
* Purism Librem 15 v2
* Google Lulu
* Google Rikku
* Google Samus
* Google Tidus
* Google Banon
* Google Celes
* Google Cyan
* Google Edgar
* Google Kefka
* Google Reks
* Google Relm
* Google Setzer
* Google Terra
* Google Ultima
* Google Wizpig
* Google Banjo
* Google Candy
* Google Clapper
* Google Enguarde
* Google Expresso
* Google Glimmer
* Google Gnawty
* Google Heli
* Google Kip
* Google Ninja
* Google Orco
* Google Quawks
* Google Squawks
* Google Sumo
* Google Swanky
* Google Winky
* Google Akemi
* Google Ambassador
* Google Dooly
* Google Drallion
* Google Dratini
* Google Duffy
* Google Faffy
* Google Genesis
* Google Helios
* Google Jinlon
* Google Kaisa
* Google Kindred
* Google Kohaku
* Purism Librem 14
* Purism Librem Mini v2
* Google Moonbuggy
* Google Nightfury
* Google Noibat
* Google Scout
* Google Wyvern
* Google Dewatt
* Google Nipperkin
* Google Ampton
* Google Bloog
* Google Bobba
* Google Casta
* Google Dood
* Google Fleex
* Google Foob
* Google Garg
* Google Lick
* Google Meep
* Google Phaser
* Google Falco
* Google Leon
* Google Mccloud
* Google Monroe
* Google Panther
* Google Peppy
* Google Tricky
* Google Wolf
* Google Zako
* Google Awasuki
* Google Beadrix
* Google Blipper
* Google Boten
* Google Boxy
* Google Bugzzy
* Google Cret
* Google Dexi
* Google Dibbi
* Google Dita
* Google Drawcia
* Google Galtic
* Google Kracko
* Google Lantis
* Google Madoo
* Google Magolor
* Google Metaknight
* Google Pirika
* Google Sasukette
* Google Sasuke
* Google Storo
* Google Taranza
* Google Atlas
* Google Endeavour
* Google Eve
* Google Fizz
* Google Karma
* Purism Librem 13 v4
* Purism Librem 15 v4
* Google Nami
* Google Nautilus
* Google Nocturne
* Google Rammus
* Google Soraka
* Google Crystaldrift
* Google Frostflow
* Google Markarth
* Google Skyrim
* Google Winterhold
* Google Kanix
* Google Karis
* Google Screebo
* Google Berknip
* Google Dirinboz
* Google Ezkinil
* Google Gumboz
* Google Morphius
* Google Shuboz
* Google Vilboz
* Google Woomax
* Google Asuka
* Google Caroline
* Google Cave
* Google Chell
* Google Lars
* Purism Librem 13 v2
* Purism Librem 15 v3
* Google Sentry
* Google Butterfly
* Google Link
* SAMSUNG Lumpy
* Google Parrot
* Google Stout
* SAMSUNG Stumpy
* Google Aleena
* Google Careena
* Google Liara
* Google Treeya
* Google Chronicler
* Google Collis
* Google Copano
* Google Delbin
* Google Drobit
* Google Eldrid
* Google Elemi
* Google Lindar
* Google Voema
* Google Volet
* Google Voxel
* Purism Librem Mini
* Google Sarien

CSMWrap errata
---------

CSMWrap is the SeaBIOS CSM build provided inside EDK2. It hogs an entire CPU
core at boot, so e.g. quad-core will show up as three-core inside Linux. It's
a known limitation that will be corrected in a future release, due to the design
of CSMWrap. If you need backward compatibility, it's better to use the GRUB EFI
build instead which does not have this problem. Both GRUB EFI and SeaBIOS CSM
are available in the EDK2 menu via ESC keypress at boot time.

Users are best served just switching to native UEFI boot with ESP, using
Libreboot's EDK2 payload.
