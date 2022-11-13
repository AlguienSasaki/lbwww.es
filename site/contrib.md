---
title: Project contributors
x-toc-enable: true
...

This list does not necessarily reflect who is currently working on the project,
but it lists some people who have contributed to the project in meaningful ways.

If we forgot to mention you here, let us know and we'll add you. (or if
you don't want to be mentioned, let us know and we'll remove your
entry)

Information about who works on osboot, and how the project is run, can
be found on this page: [who.md](who.md)

You can know the history of the osboot project, simply by reading this page.
It goes into detail about all of the major contributions to the project, and in
general how the project was created (and who helped create it).

Leah Rowe
---------

**Founder of the osbootboot project, and currently the lead developer.** Leah
works on all aspects of osboot, such as:

* General management. Leah handles all outside contributions to osboot,
  reviews pull requests, deals with bug reports, delegates tasks when necessary
  or desirable. Leah controls the osboot.org server infrastructure, hosted
  in her lab.
* Leah has the final say on all decisions, taking input via discussion with
  members of the public, mostly on IRC. Leah oversees releases of osboot,
  and generally keeps the project going. Without Leah, there would be no Osboot!
* The build system (osbmk, short for osboot Make). This is the automated build
  system that sits at the heart of osboot; it downloads, patches, configures
  and compiles the relevant components like coreboot, GNU GRUB and generates
  the osboot ROM images that you can find in release archives (as of 23 March
  2022, there are not yet any binary releases, it's rolling release, built from
  source. see: <https://osboot.org/docs/build/>)
* Upstream work on coreboot, when necessary (and other projects that osboot
  uses). This means also working with people from outside of the osboot
  project, to get patches merged (among other things) on the upstream projects
  that osboot uses
* Providing user support on IRC

Leah is also responsible for [libreboot.org](https://libreboot.org/) which is heavily
based on Osboot, but with different project goals.

Caleb La Grange
---------------

**Secondary developer, number two to Leah.** Caleb is a full time osboot developer
with a narrower focus. Caleb focuses on several areas of development:

* Build system. Caleb is responsible for improving and fixing the osboot Make build
system. Specifically: binary blob management, automation, and reproducibility. 
* Hardware modification. Caleb has a passion for hardware alteration; soldering,
desoldering, and testing osboot software on the resulting hardware.
* Board porting. Anything supported in Coreboot can be ported to osboot, Caleb
will test and port any board he can get his hands on. Additionally, anyone can
contact Caleb to generate osboot roms for testing on their board.
* Documentation. Caleb actively maintains documentation on the above areas of
interest. Additionally, Caleb is responsible for disassembly guides with his own
pictures and diagrams for several boards.
* User support. Caleb is active on irc and willing to help any user interested in
using osboot or in need of help.
* Project goals. Caleb collaborates with Leah on determining project goals.
Leah has the final say in every decision.

Coreboot project
----------------

Without coreboot, the osboot project simply would not be possible.

The people and companies that work on coreboot are numerous, and they make the
osboot project what it is. The osboot project makes heavy use of coreboot, to
provide hardware initialization.

GNU GRUB
--------

GRUB is the bootloader used by osboot. It goes without saying that the GRUB
developers enable osboot, through their work.

SeaBIOS
-------

The osboot firmware provides SeaBIOS as a payload option. SeaBIOS provides a
legacy x86 BIOS implementation.

Libreboot contributors
----------------------

Since osboot is a fork of Libreboot, it goes without saying that many of the
contributions to Libreboot have also helped shape the osboot project.

There is a separate list of contributors is maintained by the Libreboot
project.

Please read: <https://libreboot.org/contrib.html>
