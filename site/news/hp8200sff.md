% HP Elite 8200 SFF support added to Libreboot (plus more desktops coming soon)
% Leah Rowe
% 15 April 2023

Introduction
============

Today, Libreboot gains its *first* desktop machine for nearly 2 years. The
last one added was Acer G43T-AM3.

You can learn more about this on the [HP Elite 8200 SFF Libreboot installation
and hardware information page](../docs/install/hp8200sff.md).

The patch for Libreboot was done, courtesy of Riku Viitanen, IRC nick `Riku_V`
on Libreboot IRC. It's quite a nice Intel Sandybridge platform, with all libre
initialisation on the coreboot side, and Libreboot's build system automatically
runs `me_cleaner` while building for this board.

This board is significant because it's relatively simple to flash, cheap, and
readily available on merchant sites such as eBay. Desktop support has
traditionally been much weaker in Libreboot, and this is something that
should (can, and will) change.

HP EliteBook 2560p (laptop)
------------------

Riku is also interested in adding HP EliteBook 2560p support in Libreboot.
Coreboot has support for that board. For *that* board, I committed this
patch in a branch of Libreboot, to make handling EC firmware easier for Riku:

<https://browse.libreboot.org/lbmk.git/commit/?h=blobutil_kbc1126_ec&id=b9ee4e79c33365ede01fb7d2a0d5c8f3c1a1928c>

I initially wrote the logic in that patch as part of another experimental
branch of Libreboot, adding HP Elitebook 8560w (does not boot yet).

If my EC download script works for Riku, and 2560p support confirmed working
when Riku tests it, then both of them shall by merged into Libreboot's master
branch. The EC firmware is not on a separate IC, for that machine; instead,
it must be handled during the coreboot build process, for insertion into the
main boot flash. This is actually *better*, for similar reasons as explained
in the Libreboot [blobs reduction policy](policy.md), because it makes the EC
firmware easier to replace with libre code (based on reverse engineering,
perhaps).

Dell Optiplex 7020/9020 probably soon (testing needed)
-------------------------------------

I've purchased Dell Optiplex 7020 and 9020 workstations (Haswell gen), which is
available for coreboot with this gerrit patch:

<https://review.coreboot.org/c/coreboot/+/55232/>

Libreboot will eventually support *all* coreboot targets.

Want to help add boards yourself?
---------------------------------

Libreboot has the following documentation available:

* [lbmk maintenance manual](../docs/maintain/) (build system documentation)
* [porting guide](../docs/maintain/porting.md) (largely Intel-centric, at
  the time of writing this post)

Riku's work was inspired by reading these. The guides themselves are also in
need of constant maintenance and improvement, relative to the changes in
Libreboot.
