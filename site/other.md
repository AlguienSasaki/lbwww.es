---
title: Other coreboot distributions providing Open Source BIOS/UEFI firmware
x-toc-enable: true
...

What is a coreboot distro?
--------------------------

Libreboot is a *coreboot distribution* or *coreboot distro*, in the same way
that Debian is a *Linux distro*. Its purpose is to provide free/opensource boot
firmware, replacing proprietary BIOS/UEFI firmware, and it
supports [many machines](docs/install/#which-systems-are-supported-by-libreboot).

It is a coreboot distro precisely because of its [design](docs/maintain/).
Libreboot's build system automatically downloads, patches and builds all the
various upstream sources such as coreboot, GRUB, SeaBIOS, U-Boot and so on.
This automation is used to provide [binary releases](download.md), which the
user can [easily install](docs/install/). Coreboot is notoriously difficult
to configure and install, for most people, and you need a high degree of
technical skill to use it; distros like Libreboot bridge this gap, making
coreboot accessible to non-technical users.

Libreboot was the *first* coreboot distro,
existing [since December 2013](news/10.md). Many Libreboot users exist today,
who would otherwise not use coreboot at all.

Coreboot is highly flexible for many configurations. It is quite possible build
to [your own coreboot image](https://doc.coreboot.org/getting_started/index.html)
but most non-technical users should probably use a coreboot distro.

It's thanks to the various coreboot distros that many people use coreboot today;
without them, many otherwise non-technical users might not use coreboot at all.

List of coreboot distros
------------------------

Not all distros are listed; only those of high quality or otherwise of interest;
quality, not quantity. In alphabetical order:

### Canoeboot

Website: <https://canoeboot.org/>

Git repositories: <https://canoeboot.org/git.html>

Canoeboot is a *special fork* of Libreboot; both Canoeboot and Libreboot are
maintained in parallel by the same developer, Leah Rowe. Canoeboot supports
far less hardware than Libreboot, but provides a *pure* free software coreboot
distribution, due to its [policy](https://canoeboot.org/news/policy.html). As
a result of Canoeboot's policy, it currently only supports very old hardware.

It otherwise has the exact same design as Libreboot, and is kept in relative
sync [at all times](https://canoeboot.org/about.html), often doing releases side
by side on the same days as Libreboot.

*Libreboot* supports more hardware than Canoeboot, due to its more
pragmatic [Binary Blob Reduction Policy](news/policy.md) adopted on 17
November 2022; Canoeboot is a continuation of Libreboot from prior to this,
since Libreboot initially used the same dogmatic policy as Canoeboot. A small
minority of users demanded it post-November 2022, so Canoeboot was born.

If you're an absolute Free Software fanatic, Canoeboot is for you. Otherwise,
if you want to use much newer hardware, Libreboot is a worthy choice. Since
Canoeboot only supports much older hardware, and uses Libreboot's *old* policy,
you could consider Canoeboot to be *legacy Libreboot*. Libreboot adopted the
Binary Blob Reduction Policy in November 2022, as part of a general desire to
support more - and newer - hardware.

### Dasharo

Website: <https://docs.dasharo.com/>

Git repositories: <https://github.com/dasharo>

Supports many machines, with a choice of EDK2(UEFI) or Heads(Linuxboot)
payload in the flash. Some older machines may provide a SeaBIOS payload
instead. A lot of work that goes into the upstream coreboot project came
from the Dasharo developers.

Dasharo provides their own fork of coreboot, with a specific tree *per board*.
Several coreboot ports (e.g. MSI Z690-A PRO) were implemented directly by
the Dasharo project, and later upstreamed into the regular coreboot project.

Dasharo has a special emphasis on commercial application, providing tailored
coreboot images for each supported motherboard, with an emphasis on stability.
It's a very different approach than Libreboot's approach; Libreboot provides
a more generalised design in its build system and infrastructure.

### Firmware-action

Website: <https://github.com/9elements/firmware-action>

Documentation: <https://9elements.github.io/firmware-action/>

**TODO: Link their FOSDEM 2025 talk when it becomes available on archives.**

Launched at the start of 2023, this project by 9elements provides
a source-based package manager similar conceptually to lbmk; it downloads,
patches and compiles various upstream sources, specifically coreboot,
Linux, Tianocore/edk2, u-root and others.

It currently does not provide specific build targets pre-configured, nor
binary releases, but it could theoretically be fashioned that way, and this
was asked of them at their FOSDEM 2025 talk, during questions and answers - and
much of what it's trying to do could inspire aspects of Libreboot's design in
the future.

Their build system design is interesting, and has many of the same sort of
features lbmk has, such as tracking of build artifacts / re-use of build
artifacts to speed up re-builds. I (Leah Rowe, Libreboot maintainer) learned
of Firmware-action because I attended their FOSDEM 2025 talk. It is listed on
this page, because their build system design *can* be used to provide a fully
fledged coreboot distribution, with binary releases.

The goal of the firmware-action project is to provide a fully reproducible
build environment, for integration with a CI/CD setup during coreboot
development. This build system has great potential to provide a useful
competitor versus, say, Heads or LinuxBoot projects. The design of this build
system could also be adapted to provide many different coreboot payloads,
even ones that it currently doesn't support, such as GNU GRUB.

### Heads

Website: <https://osresearch.net/>

Git repositories: <https://github.com/linuxboot/heads>

Heads provides a LinuxBoot payload using U-Root, and has many advanced features
such as TPM-based MeasuredBoot. With combined use of a FIDO key, you can easily
and more reliably determine whether you boot firmware has been tampered with.

The Linux-based payload in flash uses kexec to boot another Linux kernel. It
provides an easy to use boot menu, highly configurable and supports many
Linux distros easily.

If you're the sort of person who needs full disk encryption and you have a
focus on security, Heads is for you. Perfect for use with something like Qubes.

Libreboot provides its own [security mechanisms](docs/linux/grub_hardening.html),
but Heads is much more flexible and complete, in this regard. The only downside
to Heads's Linux-based flash setup, is that it's basically a Linux-only
coreboot distro (whereas Libreboot can boot BSD and even *Windows*, in some
cases).

Another focus of the heads project is on *reproducible builds*. Its build
system bootstraps a toolchain that then compiles everything else, including
the coreboot crossgcc toolchain. The purpose of this is to provide matching
ROM hashes on every build; for this purpose, it also auto-downloads vendor
files such as Intel ME at build time, instead of requiring you to dump from
the original boot firmware.

Heads's vendorcode auto-download logic inspired Libreboot's
own [vendorcode inject](docs/install/ivy_has_common.md) design; Libreboot
greatly expanded on it, on more machines.

We in Libreboot have an affinity for the Heads project, and have worked with
them in the past, and they with us, helping each other back and forth. Many of
the machines supported in Libreboot are also supported in Heads, and vice versa,
but they target different kinds of users and use-case scenarios, with Libreboot
targeting a more general audience (while providing security hardening options),
whereas Heads specifically targets security-conscious users.

### MrChromeBox

Website: <https://docs.mrchromebox.tech/>

Git repositories: <https://github.com/MrChromebox/>

Provides a tailored EDK2(UEFI) payload on supported *Chromebooks*. You can use
this to replace ChromeOS with a regular Linux distro or BSD system - even
Windows - if you wish.

The benefit of using *MrChromebox* is that it provides up to date EDK2, unlike
proprietary vendors who often provide old, CVE-ridden versions of EDK2 forks
such as InsydeH2O.

With MrChromebox's guidance, you can have a completely up to date UEFI firmware
on your machine, and get good use out of your Chromebook for many more years,
with regular security updates.

You can also use the [Chrultrabook Docs](https://docs.chrultrabook.com/) which
make use of MrChromebox and might prove useful.

One of Chultrabook's maintainers, Elly, did this talk at 37C3 conference,
demonstrating Chultrabook:
<https://www.youtube.com/watch?v=7HFIQi835wY> - and also did this more general
talk about coreboot at 38C3: <https://www.youtube.com/watch?v=LD9tOcf4OkA>. It's
very good reference material if you want to know more about coreboot, and
coreboot distros more generally.

Elly also did this interview with Brodie Robertson, about coreboot, and
explains the concept of a coreboot distro in more detail in one part of
the interview:
<https://www.youtube.com/watch?v=4Am_1MzJ6ZA>

Libreboot largely avoids supporting Chromebooks, precisely because
MrChromebox is a perfectly viable option on these machines.

### Skulls

Git repositories: <https://github.com/merge/skulls>

Skulls provides simple coreboot images with SeaBIOS payload, on a handful of
Thinkpads. Libreboot *also* provides similar SeaBIOS configurations, on all
of the same machines, but Libreboot's design does mean that there are a few
additional steps for installation.
If you just want the simplest, most barebones setup, Skulls is a great choice.

Libreboot *also* provides U-Boot and GRUB, and has other ambitions. Libreboot
aims to provide ease of use while also providing great power and flexibility.
So Libreboot is aimed specifically at power users, while also trying to
accomodate non-technical users; Skulls largely targets the latter.

### System76 Open Firmware

Git repository: <https://github.com/system76/firmware-open>

Other repositories e.g. EC firmware: <https://github.com/system76>

System76 provides their own special coreboot fork, that they tailor for
specific machines that they sell; they also provide free EC firmware. Jeremy
Soller of System76 maintains this firmware, and the work is regularly
upstreamed into the regular coreboot project.

System76 provides the coreboot firmware, along with EDK2 UEFI payload. It can
boot Linux distros, BSD systems and even Windows perfectly.

Is your distro unlisted?
------------------------

Please get in touch! We'd love to link your project here.

The coreboot project also maintains its own list of coreboot distros:

<https://coreboot.org/users.html>

Libreboot maintains its own list, because it is felt that distros should also
link to each other, since many people who find coreboot for the first time may
find it through a distro (such as Libreboot) and not check coreboot's own
website. We in the Libreboot project wish to see everyone using free boot
firmware, which was the primary motivation behind this page, in promoting the
various projects.

Organisations
-------------

This list will be populated over time. Several organisations exist out there
that pertain to *Free and Open Source firmware* development, supporting its
existence in general. Here are just a few of them:

### Open Source Firmware Foundation (OSFF)

Website: <https://opensourcefirmware.foundation/>

The OSFF is a non-profit organisation of Oregon, USA, whose mission it is to
coordinate communication between the various free boot projects (projects like
coreboot). Several coreboot developers are a part of
its [governance](https://opensourcefirmware.foundation/team/).

The OSFF also hosts an annual [Open Source Firmware
Conference](https://www.osfc.io/), where various projects, companies and
peoples can talk about their work in this field.

The OSFF exists precisely because nothing like it has existed before; the
world of boot firmware is largely dominated by proprietary IBVs (independent
BIOS vendors), the likes of e.g. AMI, Phoenix.

Organisations like OSFF are critical, if we ever wish to see *libre* boot
firmware become the default firmware, on computers that normal people actually
purchase. All the separate projects out there do great work, but it is critical
that we have a central *sorting ground*, a *point of contact* if you will,
to represent us, and the OSFF's mission is to be exactly that.

OSFF also has their own list of projects, similar to the list on *this* page
that you are reading now. See:

<https://opensourcefirmware.foundation/projects/>

Libreboot is currently not affiliated with the OSFF in any official capacity,
but it has our *full* support, and we will do what we can to help it, when we
can.

### Software Freedom Conservancy (SFC)

Although not strictly related to free/opensource firmware, the coreboot project
is an SFC member, which you can see here:

<https://sfconservancy.org/projects/current/>

The SFC is a non-profit organisation of New York, USA, dedicated to the
cause of [software freedom](https://writefreesoftware.org/learn), with a view
towards Copyleft advocacy, especially [copyleft
compliance enforcement](https://sfconservancy.org/copyleft-compliance/). 

SFC provides services to *member projects*, such as coreboot, which you
can read about here:

* <https://sfconservancy.org/projects/>
* <https://sfconservancy.org/projects/services/>

Services such as donations infrastructure, legal services (including things
like copyright assignements and enforcement), advice/assistance about project
governance, help with fundraising and outreach, etc.

SFC also hosts an annual conference, called *FOSSY*, which projects, companies
and peoples can go to to promote their work on free/opensource projects. See:

<https://sfconservancy.org/fossy/>

Although Libreboot is not affiliated with SFC in any official capacity, it is
otherwise an excellent organisation, it is listed here in honour of the
excellent work it does, including its support of the coreboot project.

### Software in the Public Interest (SPI)

Website: <https://www.spi-inc.org/>

The SPI is a non-profit organisation of New York, USA, dedicated to the
cause of [software freedom](https://writefreesoftware.org/learn). It provides
fiscal sponsorship and promotion of numerous Free and Open Source projects,
which you can read about here:

<https://www.spi-inc.org/projects/>

It provides services to member projects such as donations infrastructure,
legal services, advice/assistance and oversight of project governance and
general help with fundraising.

SFC most notably provides services to Debian, which is a member:

<https://www.spi-inc.org/projects/debian/>

Although SPI does not currently (at this time) sponsor any coreboot or
firmware-related projects, several of the projects that it *does* support
are critical for Libreboot development. For example, it also supports the
Arch Linux project:

<https://www.spi-inc.org/projects/archlinux/>

Libreboot development, especially build testing, happens largely on computers
running Debian Linux and Arch Linux, because these cover a large number of
users; we also test many other distros such as Fedora or Ubuntu.

Organisations like SPI (and indeed SFC, OSFF) are critical to the general
health of the Free Software movement. Without them, we would not be able to
as effectively coordinate projects, especially in terms of funding.

Libreboot is not currently affiliated with the SPI in any official capacity,
but they have our respect and they are listed here.

Non-coreboot firmware projects
--------------------------

Several other projects besides coreboot provide free hardware initialisation,
such as [U-Boot](https://www.u-boot.org/) (as own firmware, distinct from U-Boot
as a coreboot payload), [Trusted Firmware](https://www.trustedfirmware.org/) and
so on.

Here are a few such projects:

### fwupd

fwupd is essentially a centralised repository of firmware images, that
can be used to provide updates for your mainboard. Although not officially
supported nor endorsed by the Libreboot project, many Linux distros make
use of this to provide UEFI firmware updates for example.

Libreboot doesn't use this, due to the many idiosyncrasies of Libreboot on
various motherboards; however, we may use it in a limited capacity, in the
future.

### LinuxBoot

Website: <https://www.linuxboot.org/>

LinuxBoot can be provided as a UEFI DXE, or as a U-Boot SPL, coreboot payload
or Slim Bootloader Stage 1B, to provide a Linux kernel at boot time, which
kexecs into another Linux kernel.

The benefit of using *Linux* to *boot Linux* is that then the bootloader part
of your firmware doesn't need to rewrite every driver, because Linux already
provides excellent drivers, and it also affords you many advanced
configuration possibility such as hardened encryption setups with things
like Measured Boot, and it could also be used to boot over a network.

### OpenBMC

Website: <https://github.com/openbmc/docs>

Linux distro for management controllers (BMCs) used on servers,
rack switches and RAID appliances. This provides a remote management
feature, most useful (for example) on colocated server hosting.

### Oreboot

Website: <https://github.com/oreboot/oreboot>

Oreboot is a special fork of coreboot, re-written in the Rust programming
language, focusing specifically on the *LinuxBoot* payload configuration.

### Trusted Firmware

Website: <https://www.trustedfirmware.org/>

Trusted Firmware provides boot firmware for ARMv8-A, ARMv9-A
and ARMv8-M. Specifically tailored for use on embedded systems, and parts of
it are also used by the coreboot project on some boards.

### U-Boot

Website: <https://www.u-boot.org/>

U-Boot runs on a large number of embedded systems, with support for a variety
of architectures such as ARM, x86, RISC-V and others. U-Boot provides its own
small kernel and operating system, with drivers and applications designed to
boot your operating system using various methods. It has an advanced *shell*,
with excellent networking and file system support - most notably, we use it
in Libreboot as a UEFI payload for *coreboot*, but U-Boot can also provide its
own boot initialisation independently of coreboot.

One of the nice features of U-Boot is its *licensing* (GPLv2 for the most part)
and similar coding style to Linux; the licensing and technical design means
that it's much easier to port existing Linux drivers, when something needs to
be done in the U-boot project.

-----------------------------------------

We would like to list various distros of these too. If you know of a cool
project, please get in touch with the Libreboot project!
