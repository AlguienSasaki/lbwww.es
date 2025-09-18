---
title: Libero e Open Source BIOS/UEFI firmware
x-toc-enable: true
...

Il progetto *Libreboot* fornisce avvio [libero e open source](https://writefreesoftware.org/)
grazie al firmware basato su coreboot, sostituendo cosi', firmware BIOS/UEFI proprietario
su [alcune schede madri basate su Intel/AMD x86 o ARM](docs/install/#which-systems-are-supported-by-libreboot),
in computer fissi e portatili. Inizializza l'hardware (controller di
memoria, CPU, periferiche) e avvia un bootloader per il tuo sistema operativo.
[Linux](docs/linux/) e [BSD](docs/bsd/) sono ben supportati.
L'aiuto e' disponibile sul canale IRC [\#libreboot](https://web.libera.chat/#libreboot)
su [Libera](https://libera.chat/).

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

On [8 September 2025](news/spi.html), Libreboot became an official [Associated
Project](https://www.spi-inc.org/projects/libreboot/) at SPI ([Software in
the Public Interest](https://www.spi-inc.org/)).

Libreboot provides GNU boot loader "[GRUB](docs/linux/)" and SeaBIOS payloads
on x86/x86\_64
Intel/AMD motherboards, and a [U-Boot UEFI payload](docs/uboot/) *for coreboot*
on ARM64(Aarch64) motherboards.
An [x86/x86\_64 U-Boot UEFI payload](docs/uboot/uboot-x86.md) is also available
on some boards. The x86, x86\_64 and arm64 U-Boot payloads provide a lightweight
UEFI boot implementation, which can boot many Linux distros and BSD systems.
The SeaBIOS and GRUB payloads also boot Linux/BSD systems. Which one you use
depends on your preferences. Libreboot's design incorporates all of these boot
methods in a single image, so you can choose which one you use at boot time,
and more payloads (e.g. Linux kexec payload) are planned for future releases.

**ULTIMO RILASCIO: L'ultimo rilascio e' Libreboot 25.06, rilasciato il 30 June 2025.
Vedi: [Libreboot 25.06 annuncio di rilascio](news/libreboot2506.md).**

You can also [buy Libreboot preinstalled](https://minifree.org/) from Minifree Ltd,
on select hardware, as well as send your compatible hardware
for [Libreboot preinstallation](https://minifree.org/product/installation-service/).
The founder and lead developer of Libreboot, Leah Rowe, also owns and operates
Minifree; sales provide funding for Libreboot.

A parallel [Libreboot fork, Canoeboot](https://canoeboot.org), also exists.
Both Canoeboot and Libreboot are maintained by the same developer (Leah Rowe).

Per quale ragione utilizzare *Libreboot*?
-----------------------------------------

<img tabindex=1 class="l" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot ti permette [liberta'](https://writefreesoftware.org/) che non potresti ottenere
con altri firmware di boot, velocita' di avvio maggiori
e [migliore sicurezza](docs/linux/grub_hardening.md).
E' estremamente flessibile e [configurabile](docs/maintain/) per la maggior parte dei casi.

*Noi* crediamo nella liberta' di [studiare, condividere, modificare and usare
il software](https://writefreesoftware.org/), senza restrizione alcuna,
in quanto e' uno dei fondamentali diritti umani che chiunque deve avere.
In questo contesto, *il software libero* conta. La tua liberta' conta. La formazione personale conta.
[Il diritto di riparare](https://en.wikipedia.org/wiki/Right_to_repair) conta.
Molte persone usano firmware di boot proprietario (non-libero), anche se usano
[un sistema operativo libero](https://www.openbsd.org/).
Firmware proprietari spesso [contengono](faq.html#intel) [vulnerabilita'](faq.html#amd),
e possono essere difettosi. Il progetto libreboot venne fondato nel Dicembre 2013, con lo scopo
prefissato di permettere che il firmware coreboot sia accessibile anche
per utenti con scarsa formazione tecnica.

Il progetto Libreboot fa uso di [coreboot](https://www.coreboot.org/) per
[l'inizializzazione hardware](https://doc.coreboot.org/getting_started/architecture.html).
Coreboot e' notoriamente difficile da installare per utenti che hanno una scarsa formazione tecnica;
gestisce solo l'inizializzazione di base e successivamente carica un programma come
[payload](https://doc.coreboot.org/payloads.html) (ad esempio.
GRUB,
[Tianocore](https://www.tianocore.org/)), i quali possono essere configurati a piacere.
*Libreboot risolve questo problema*; e' una *distribuzione di coreboot* con un
[sistema di compilazione automatizzato](docs/build/) che produce *immagini ROM* complete, per una
installazione piu' robusta. Viene fornito con apposita documentazione.

Libreboot non deriva da coreboot
--------------------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

In effetti, Libreboot tenta di essere il piu' possibile simile alla versione *ufficiale* di coreboot,
per ogni scheda, ma con diversi tipi di configurazione forniti automaticamente dal sistema di
compilazione automatico di Libreboot.

Esattamente come *Alpine Linux* e' una *distribuzione Linux*, Libreboot e' una
*distribuzione coreboot*. Per fare un immagine ROM da zero, hai bisogno di esperienza necessaria
nel configurare coreboot, GRUB e qualunque altra cosa ti serve. Con *Libreboot*,
che puoi scaricare da Git o da un archivio di codici sorgenti, puoi far partire *a script*,
e questo mettera' su automaticamente le immagini ROM richieste. Un sistema di compilazione automatico,
chiamato `lbmk` (Libreboot MaKe), mettera' su quelle immagini ROM automaticamente, senza troppi
interventi da parte dell'utente. Le configurazioni di base sono gia' state previste in precedenza.

Se avresti voluto compilare coreboot normalmente senza il sistema di compilazione
automatico di Libreboot, ti troveresti ad affrontare molte piu' difficolta senza adeguate competenze
tecniche per produrre una configurazione funzionante.

I rilasci binari di Libreboot forniscono immagini ROM precompilate,
che puoi semplicemente installare senza troppe conoscenze tecniche o abilita'
particolari ad eccezione del seguire [semplici istruzioni scritte per chiunque](docs/install/).

Come essere d'aiuto
-------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.vimuser.org/uboot.png" /><span class="f"><img src="https://av.vimuser.org/uboot.png" /></span>

Il *modo migliore* col quale puoi aiutarci e' quello di *aggiungere* nuove schede condividendone
la configurazione. Qualunque cosa sia supportata da coreboot puo' essere integrata in Libreboot,
con immagini ROM fornite nei rilasci. Vedi anche:

* [Richiedi di collaudare o mantenere una scheda](docs/maintain/testing.md)
* [Guida per rendere nuove schede compatibili](docs/maintain/porting.md)
* [Documentazione del sistema di compilazione automatica di Libreboot](docs/maintain/)

Dopo di che c'e' da tenere seriamente in considerazione la manutenzione della compilazione
automatica (guarda sopra), e la *documentazione*. La documentazione e' critica in ogni progetto.

*Il supporto utente* e' inoltre un fattore critico. Resta collegato su IRC, e se hai sufficiente
esperienza puoi aiutare qualcuno in difficolta' (o anche imparare insieme a loro), in questo modo
sarai di grande aiuto al progetto. Un sacco di persone chiedono inoltre aiuto nel subreddit
`r/libreboot`.

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Puoi controllare l'elenco dei bugs sul
[bug tracker](https://codeberg.org/libreboot/lbmk/issues).

Se hai notato un difetto (bug) e hai trovato una soluzione, [qui puoi trovare istruzioni su come inviare
delle correzioni (patches)](git.md) o puoi semplicemente farne rapporto.
Inoltre tutto questo sito internet e' scritto in Markdown e ospitato su un
[repository separato](https://codeberg.org/libreboot/lbwww), dove puoi inviare correzioni (patches).

Qualunque discorso relativo a sviluppo e supporto utente viene fatto sul canale IRC.
Puoi avere piu' informazioni sulla [pagina dei contatti](contact.md).

Servono traduzioni per libreboot.org
------------------------------------

Libreboot attualmente ha pagine Web tradotte in Ucraino and Francese
(ma non ancora in ogni pagina e per ogni lingua).

Se vuoi essere d'aiuto con le traduzioni, puoi tradurre le pagine, aggiornare traduzioni esistenti
e condividere le tue versioni tradotte. Per saperne di piu' puoi leggere:
[come condividere traduzioni per libreboot.org](news/translations.md)

Anche se qualcuno sta gia' lavorando sulle traduzioni in una determinata lingua,
possiamo sempre avvalerci di piu' contributori. Piu' siamo e meglio e'!


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
