---
title: Freie und Open Source BIOS/UEFI Firmware
x-toc-enable: true
...

*Libreboot* ist ein [Coreboot-Distribution](docs/maintain/) (coreboot distro),
so wie Debian eine Linux-Distribution ist. Das *Libreboot* Projekt bietet
eine [freie](https://writefreesoftware.org/) *Boot
Firmware* welche auf [bestimmten Intel/AMD x86 und ARM Geräten](docs/install/#which-systems-are-supported-by-libreboot)
die Hardware initialisiert (z.b. Speicher-Controller, CPU, Peripherie),
und dann einen Bootloader für dein Betriebssystem startet. [Linux](docs/linux/)
sowie [BSD](docs/bsd/) werden gut unterstützt. Es ersetzt proprietäre BIOS/UEFI
Firmware. Hilfe ist verfügbar
via [\#libreboot](https://web.libera.chat/#libreboot)
und [Libera](https://libera.chat/) IRC.

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

Libreboot provides GNU [GRUB](docs/linux/) boot loader and SeaBIOS payloads on
x86/x86\_64 Intel/AMD motherboards, and
a [U-Boot UEFI payload](docs/uboot/) *for coreboot* on ARM64(Aarch64) motherboards.
An [x86/x86\_64 U-Boot UEFI payload](docs/uboot/uboot-x86.md) is also available
on some boards. The x86, x86\_64 and arm64 U-Boot payloads provide a lightweight
UEFI boot implementation, which can boot many Linux distros and BSD systems.
The SeaBIOS and GRUB payloads also boot Linux/BSD systems. Which one you use
depends on your preferences. Libreboot's design incorporates all of these boot
methods in a single image, so you can choose which one you use at boot time,
and more payloads (e.g. Linux kexec payload) are planned for future releases.

**NEUESTE VERSION: Die neueste Version von Libreboot ist 20241206, veröffentlicht
am 6 December 2024.
Siehe auch: [Libreboot 20241206 release announcement](news/libreboot20241206.md).**

You can also [buy Libreboot preinstalled](https://minifree.org/) from Minifree Ltd,
on select hardware, as well as send your compatible hardware
for [Libreboot preinstallation](https://minifree.org/product/installation-service/).
The founder and lead developer of Libreboot, Leah Rowe, also owns and operates
Minifree; sales provide funding for Libreboot.

Warum solltest Du *Libreboot* verwenden?
----------------------------

<img tabindex=1 class="l" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot gibt dir [Freiheit](https://writefreesoftware.org/) welche
Du mit den meisten Boot Firmwares nicht hast, und zusätzlich schnellere Boot
Geschwindigkeiten sowie [höhere Sicherheit](docs/linux/grub_hardening.md).
Es ist extrem leistungsfähig und für viele Einsatzzwecke [konfigurierbar](docs/maintain/).

Du hast Rechte. Das Recht auf Privatsphäre, Gedankenfreiheit, Meinungsäußerungsfreiheit,
und Informationsfreiheit. In diesem Zusammenhang, Libreboot gibt dir diese Rechte.
Deine Freiheit ist wichtig.
[Das Recht auf Reparatur](https://en.wikipedia.org/wiki/Right_to_repair) ist wichtig.
Viele Menschen verwenden proprietäre (non-libre)
Boot Firmware, sogar wenn Sie ein [Libre OS](https://www.openbsd.org/) verwenden.
Proprietäre Firmware [enthält](faq.html#intel) häufig [Hintertüren](faq.html#amd),
und kann fehlerhaft sein. Das Libreboot Projekt wurde im Dezember 2013 gegründet, 
mit dem Ziel, Coreboot Firmware auch für technisch unerfahrene Nutzer verfügbar 
zu machen.

Das Libreboot Projekt verwendet [Coreboot](https://www.coreboot.org/) für
[die Initialiserung der Hardware](https://doc.coreboot.org/getting_started/architecture.html).
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

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

Tatsächlich versucht Libreboot so nah am regulären Coreboot zu bleiben wie möglich,
für jedes Board, aber mit vielen automatisch durch das Libreboot Build System zur 
Verfügung gestellten verschiedenen Konfigurationstypen. 

Ebenso wie *Alpine Linux* eine *Linux Distribution* ist, ist Libreboot eine
*Coreboot Distribution*. Sofern Du ein ROM Image von Grund auf herstellen möchtest,
musst Du zunächst Konfigurationen auf Experten Level durchführen,
und zwar für Coreboot, GRUB sowie sämtliche Software die Du sonst noch verwenden 
möchtest um das ROM Image vorzubereiten. Mithilfe von *Libreboot* kannst Du 
sprichwörtlich von Git oder einem anderen Quell-Archiv herunterladen, anschliessend 
ein script ausführen, und es wird komplette ROM Images herstellen, ohne das Benutzer 
Eingaben oder Eingreifen von Nöten sind. Die Konfiguration wurde bereits im 
Vorfeld erledigt.

Sofern Du das reguläre Coreboot herstellen wollen würdest, ohne hierfür das automatisierte
Libreboot Build System zu verwenden, würde dies deutlich mehr Eingreifen und ein 
sehr tiefgreifendes technisches Verständnis voraussetzen um eine funktionsfähige 
Konfiguration herzustellen. 

Reguläre Binär Veröffentlichungen bieten diese ROM Images vor-kompiliert,
und Du kannst dies einfach installieren ohne spezielle technische 
Kenntnisse oder Fertigkeiten abgesehen von der Fähigkeit einer 
[vereinfachten Anleitung, geschrieben für technisch unerfahrene Benutzer](docs/install/) zu folgen.

Wie kann ich helfen
-----------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.vimuser.org/uboot.png" /><span class="f"><img src="https://av.vimuser.org/uboot.png" /></span>

Der beste Weg wie Du helfen kannst, ist das *hinzufügen* neuer Motherboards in 
Libreboot, indem Du eine Konfiguration zur Verfügung stellst. Alles was von 
Coreboot unterstützt wird kann auch in Libreboot integriert werden, mithilfe
von ROM Images in den Veröffentlichungen. Siehe auch: 

* [Bewerbe dich um Boards zu testen oder zu pflegen](docs/maintain/testing.md)
* [Anleitung um neue Motherboards hinzuzufügen](docs/maintain/porting.md)
* [Libreboot Build System Dokumentation](docs/maintain/)

Zudem ist da noch Pflege des Build Systems (siehe oben), sowie *Dokumentation*
welche wir sehr ernst nehmen. Dokumentation ist wichtig, in jedem Projekt.

*Hilfe für Benutzer* ist ebenso wichtig. Bleibe im IRC Chat, und falls Du 
kompetent genug bist jemandem bei seinem Problem zu helfen (oder bereit mit
der Person gemeinsam zu lernen), dann ist dies ein wichtiger Beitrag zum 
Projekt. Viele Leute fragen zudem unter dem Subreddit `r/libreboot` nach Hilfe.

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Eine Liste mit Bugs gibt es
unter [Bug Tracker](https://codeberg.org/libreboot/lbmk/issues).

Sofern Du einen Bug findest oder einen Fix hast, [hier sind Anleitungen um Patches zu
schicken](git.de.md), oder Du kannst davon berichten. Diese Website ist zudem
in Markdown geschrieben und verfügbar in einem [separaten
Repository](https://codeberg.org/libreboot/lbwww) für welches Du auch Patches schicken kannst.

Sämtliche Diskussionen über Entwicklung sowie Hilfe für Nutzer findet im IRC
Kanal statt. Mehr Informationen gibt es unter [Kontakt](contact.de.md).

Übersetzungen für libreboot.org benötigt
--------------------------------------

Libreboot hat derzeit übersetzte Webseiten in ukrainisch und französisch (aber bislang
nicht für alle Seiten für keine der Sprachen)

Sofern Du mit Übersetzungen helfen möchtest, kannst Du Seiten übersetzen, 
existierende Übersetzungen überarbeiten oder deine übersetzten Versionen schicken.
Für Anleitungen, siehe bitte hier:

[Wie man Übersetzungen für libreboot.org bereitstellt](news/translations.de.md)

Auch wenn jemand bereits an einer Übersetzung in einer bestimmten Sprache arbeitet,
so können wir immer mehrere Leute gebrauchen. Desto mehr desto besser!
