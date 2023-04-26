---
title: Libreboot project
x-toc-enable: true
...

The *Libreboot* project provides
[libre](freedom-status.md) *boot
firmware* that initializes the hardware (e.g. memory controller, CPU,
peripherals) on [specific Intel/AMD x86 and ARM targets](docs/hardware/), which
then starts a bootloader for your operating system. [Linux](docs/linux/)
and [BSD](docs/bsd/) are well-supported. It replaces proprietary BIOS/UEFI
firmware. Help is available
via [\#libreboot](https://web.libera.chat/#libreboot)
on [Libera](https://libera.chat/) IRC.

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

**NEUESTE VERSION: Die neueste Version von Libreboot ist 20230423, veröffentlicht am
23 April 2023.
Siehe auch: [Libreboot 20230423 release announcement](news/libreboot20230423.md).**

Warum solltest Du *Libreboot* verwenden?
----------------------------

Libreboot gibt dir [Freiheit](https://en.wikipedia.org/wiki/Open_source) welche
Du mit den meisten Boot Firmwares nicht hast, und zusätzlich schnellere Boot Geschwindigkeiten
und [höhere Sicherheit](docs/linux/grub_hardening.md). Es ist extrem leistungsfähig
und [konfiguerierbar](docs/maintain/) für viele Einsatzzwecke.

Du hast Rechte. Das Recht auf Privatsphäre, freedom of thought, freedom of speech
and the right to read. In diesem Zusammenhang, Libreboot gibt dir diese Rechte.
Deine Freiheit ist wichtig.
[Das Recht auf Reparatur](https://yewtu.be/watch?v=Npd_xDuNi9k) matters.
Viele Menschen verwenden proprietäre (non-libre)
Boot Firmware, sogar wenn Sie ein [a libre OS](https://www.openbsd.org/) verwenden.
Proprietäre Firmware [enthält](faq.html#intel) häufig [Hintertüren](faq.html#amd),
und kann fehlerhaft sein. Das Libreboot Projekt wurde im Dezember 2013 gegründet, mit dem
Ziel Coreboot Firmware auch für technisch unerfahrene Nutzer verfügbar zu machen.

Das Libreboot Projekt verwendet [Coreboot](https://www.coreboot.org/) für [die Initialiserung
der Hardware](https://doc.coreboot.org/getting_started/architecture.html).
Die Coreboot Installation ist für unerfahrene Benutzer überaus schwierig; sie
übernimmt lediglich die Basis Initialisierung und springt dann zu einem separaten
[payload](https://doc.coreboot.org/payloads.html) Programm (z.B.
[GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), welche zusätzlich konfiguriert werden muss.
*Libreboot löst dieses Problem*; es ist eine *Coreboot Distribution* mit
einem [automatisierten Build System](docs/build/) welches vollständige *ROM images* 
für eine robustere Installation erstellt. 
Dokumentation ist verfügbar.

Libreboot ist kein Coreboot Fork
-----------------------------------

<img tabindex=1 class="l" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

In fact, Libreboot tries to stay as close to *stock* coreboot as possible,
for each board, but with many different types of configuration provided
automatically by the Libreboot build system.

In the same way that *Alpine Linux* is a *Linux distribution*, Libreboot is
a *coreboot distribution*. If you want to build a ROM image from scratch, you
otherwise have to perform expert-level configuration of coreboot, GRUB and
whatever other software you need, to prepare the ROM image. With *Libreboot*,
you can literally download from Git or a source archive, and run `make`, and it
will build entire ROM images. An automated build system, named `lbmk`
(Libreboot MaKe), builds these ROM images automatically, without any user input
or intervention required. Configuration has already been performed in advance.

If you were to build regular coreboot, without using Libreboot's automated
build system, it would require a lot more intervention and decent technical
knowledge to produce a working configuration.

Regular binary releases of Libreboot provide these
ROM images pre-compiled, and you can simply install them, with no special
knowledge or skill except the ability to
follow [simplified instructions, written for non-technical
users](docs/install/).

How to help
-----------

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

The *single* biggest way you can help is to *add* new mainboards in Libreboot,
by submitting a config. Anything coreboot supports can be integrated in
Libreboot, with ROM images provided in releases. See:

* [Apply to become a board maintainer/tester](docs/maintain/testing.md)
* [Porting guide for new mainboards](docs/maintain/porting.md)
* [Libreboot build system documentation](docs/maintain/)

After that, there is build system maintenance (see above), and *documentation*
which we take seriously. Documentation is critical, in any project.

*User support* is also critical. Stick around on IRC, and if you're competent
to help someone with their issue (or wily enough to learn with them), that is
a great service to the project. A lot of people also ask for user support
on the `r/libreboot` subreddit.

Eine Liste mit Bugs gibt es
unter [Bug Tracker](https://codeberg.org/libreboot/lbmk/issues).

Sofern Du einen Bug findest oder einen Fix hast, [hier sind Anleitungen um Patches zu
schicken](git.md), oder Du kannst davon berichten. Diese Website ist zudem
in Markdown geschrieben und verfügbar in einem [separaten
Repository](https://codeberg.org/libreboot/lbwww) für welches Du auch Patches schicken kannst.

Sämtliche Diskussionen über Entwicklung sowie Hilfe für Nutzer findet im IRC
Kanal statt. Mehr Informationen gibt es unter [Kontakt](contact.md).
