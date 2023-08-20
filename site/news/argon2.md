% Encrypted /boot/ on LUKSv2 now possible in Libreboot GRUB (PHC argon2 imported)
% Leah Rowe
% 20 August 2023

<img tabindex=1 class="r" src="https://av.libreboot.org/grub/argon2.jpg" /><span class="f"><img src="https://av.libreboot.org/grub/argon2.jpg" /></span>

Introduction
============

The GRUB payload has supported LUKSv2 for a long time, but only with the
old-school PBKDF2 key derivation method; most Linux dm-crypt setups on LUKSv2
use argon2-based key derivation, which GRUB did not support. This mean either:
use LUKSv2 with PBKDF2 (less secure), or LUKSv1 (ditto), in GRUB.

Today, Libreboot now supports argon2 key derivation in its version of GRUB,
which is offered as a coreboot payload within the Libreboot build system, and
in Libreboot releases.

Libreboot's argon2 patches are based on [this AUR
repository](https://aur.archlinux.org/cgit/aur.git/tree/?h=grub-improved-luks2-git&id=1c7932d90f1f62d0fd5485c5eb8ad79fa4c2f50d)
which patched GRUB 2.06, and the patches were rebased for use with GRUB 2.12
which Libreboot uses; the rebase was performed by [Nicholas
Johnson](https://nicholasjohnson.ch/). Nicholas emailed me to tell me that this
had been done, and I then merged Nicholas's work into Libreboot. Thank you,
Nicholas!

Why does this matter?
---------------------

Libreboot previously documented how to boot distros from encrypted `/boot`,
which is a boon for security because it's harder to compromise a machine that
has encrypted `/boot` (containing the linux kernel) than if it were unencrypted
like on most LUKS setups. In addition to this, you may consider [GRUB
hardening](../docs/linux/grub_hardening.md) steps such as GPG signature checking
of your installed Linux kernel.

These patches import the PHC argon2 implementation into Libreboot's version
of GRUB:

* <https://browse.libreboot.org/lbmk.git/commit/?id=2c0c521e2f15776fd604f8da3bc924dec95e1fd1>
* <https://browse.libreboot.org/lbmk.git/commit/?id=fd6025321c4ae35e69a75b45d21bfbfb4eb2b3a0>
* <https://browse.libreboot.org/lbmk.git/tree/resources/scripts/build/payload/grub?id=fd6025321c4ae35e69a75b45d21bfbfb4eb2b3a0>

Argon2 is the newer key derivation preferred on modern LUKSv2 setups. It is
strongly recommended that you *upgrade* to argon2id, specifically, for your
setup.

How to get it
-------------

This is unavailable in the current Libreboot 20230625 release, but will be
available in the next Libreboot release. For now, you can compile a ROM image
yourself from the Libreboot build system, lbmk, like so:

[How to build Libreboot ROM images from source](../docs/build/)

Further reading
===============

PHC argon2 implementation
-------------------------

This is the reference argon2 implementation, now used by Libreboot,
and the upstream project for that is hosted here:

<https://github.com/P-H-C/phc-winner-argon2>

Article by Matthew Garrett
--------------------------

[PSA: upgrade your LUKS key derivation
function](https://mjg59.dreamwidth.org/66429.html) by Matthew Garret, talks
further about the importance of secure key derivation (specifically argon2id)
on encrypted Linux setups.

Always use encryption!
