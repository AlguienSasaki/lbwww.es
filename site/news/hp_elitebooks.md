% HP EliteBook 2560p and Folio 9470m support added to Libreboot
% Riku Viitanen
% 23 April 2023

Free software BIOS
------------------

![Two HP EliteBooks side by side, both running Libreboot](https://av.libreboot.org/hp9470m/9470m+2560p.jpg)

Support for [HP EliteBook Folio 9470m](../docs/install/hp9470m.md) and
[HP EliteBook 2560p](../docs/install/hp2560p.md)
has now been merged into Libreboot. These are very nice Sandy/Ivy
Bridge laptops. Libreboot's build system now builds coreboot automatically
for them, making it simple to install libre firmware on these laptops and
take control of your own devices. Check out the links above for more details,
including installation details.

Additionally, a [long-standing bug](https://browse.libreboot.org/lbmk.git/plain/resources/grub/patches/0005-at-keyboard-timeout.patch?id=6ff0284a510e8d7a8721dd769ba6a773610c2cad)
with GRUB on coreboot now has a functional workaround
which is applied automatically by Libreboot.

### More to come!

Leah Rowe has recently been on a buying spree, so more Sandy/Ivy Bridge and
Haswell EliteBooks are making their way to Libreboot soon. Libreboot's goal
is to eventually support everything coreboot does.

Do you want to help add new boards yourself? You totally could give it a try,
it's fun! The Libreboot [developers](https://libreboot.org/contact.html)
are very welcoming too.

Check these documents out:

* [lbmk maintenance manual](../docs/maintain/) (build system documentation)
* [porting guide](../docs/maintain/porting.md) (largely Intel-centric, at
  the time of writing this post)



[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
