---
title: Projet Libreboot
x-toc-enable: true
...

Libreboot est un micrologiciel de démarrage [libéré](freedom-status.md)
qui initialise le matériel (càd le contrôleur mémoire, CPU,
périphériques) sur [des ordinateurs x86/ARM spécifiques](docs/hardware/)
et lance un chargeur d'amorçage pour votre système d'exploitation. [Linux](docs/linux/) et [BSD](docs/bsd/) sont bien supportés. C'est un
remplacement pour le micrologiciel UEFI/BIOS propriétaire.
Des canaux d'aide sont disponibles 
dans le canal [\#libreboot](https://web.libera.chat/#libreboot) sur le serveur IRC [Libera](https://libera.chat/).

<img tabindex=1 class="r" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

**NOUVELLE VERSION: La dernière version est [Libreboot 20230423](news/libreboot20230423.md), sortie
le 23 Avril 2023.**

Pourquoi devriez-vous utiliser Libreboot?
-----------------------------------

Libreboot vous donne des [libertés](https://en.wikipedia.org/wiki/Open_source)
que nous n'auriez pas autrement avec d'autre micrologiciel de démarrage. Il est
extremement [puissant](docs/linux/grub_hardening.md)
et [configurable](docs/maintain) pour  plein de cas d'utilisations.

Vous avez des droits. Un droit à la vie privée, liberté de pensée, liberté d'espression et le droit de lire. Dans ce contexte là, Libreboot vous permet d'avoir ces droits.
Votre liberté compte.
Le [Droit à la réparation](https://yewtu.be/watch?v=Npd_xDuNi9k) est important.
Beaucoup de personnes utilisent un micrologiciel de 
démarrage propriétare (non libre), même
si ils utilisent [un système d'exploitation libre](https://www.openbsd.org/).
Les micrologiciels propriétaires [contiennent](faq.html#intel) souvent 
des [portes dérobées](faq.html#amd) et peuvent être instable. Libreboot 
a été fondé en Décembre 2013 avec le but de rendre le libre
au niveau du micrologiciel accessible pour les utilisateurs non-techniques. 

Libreboot utilise [coreboot](https://www.coreboot.org) pour
[l'initialisation matérielle](https://doc.coreboot.org/getting_started/architecture.html)
Coreboot est renommé comme être difficilement installable par des utilisateurs 
non technique; il se charge seulement de l'initialisation basique
puis bascule sur un programme de [charge utile](https://doc.coreboot.org/payloads.html)
(par ex. [GRUB](https://www.gnu.org/software/grub/),
[Tianocore](https://www.tianocore.org/)), qui doit lui aussi être configuré.
*Libreboot règle ce problème*; c'est une *distribution de coreboot* avec
un [système de compilation automatisé](docs/builds/), crééant des 
*images ROM* complètes pour une installation plus robuste. De la documentation est disponible.

De quelle façon Libreboot diffère de Coreboot?
------------------------------------------------

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Contrairement à l'opinion populaire, le but principal de Libreboot n'est
pas de fournir un Coreboot déblobbé; ceci n'est simplement qu'une
des politiques de Libreboot, une importante certes, mais qui n'est qu'un
aspect mineur de Libreboot.

De la même façon que *Alpine Linux* est une distribution Linux, Libreboot 
est une *distribution coreboot*. Si vous voulez compilé une image ROM
en partant des bases, vous devez alors effectuer une configuration experte 
de Coreboot, GRUB et n'importe quel autre logiciel dont vous avez besoin 
afin de préparer la ROM. Avec *Libreboot*,
vous pouvez télécharger la source depuis Git ou une archive, exécuter
`make` etça compilera une image ROM entières. Le système de compilation 
automatisé de Libreboot nommé `lbmk` (Libreboot MaKe), compile ces images 
ROM automatiquement, sans besoin d'entrées utilisateur or intervention
requise. La configuration est faite à l'avance.

Si vous devriez compiler du coreboot classique sans utiliser le système 
de build automatisé de Libreboot, ça demanderait bien plus d'effort et 
de connaissances techniques décente pour écrire une configuration qui marche.

Les versions de Libreboot fournissent ces images ROM pré-compilés et vous 
pouvez les installez simplement, sans connaissance ou compétence particulière 
à savoir, sauf [suivre des instructions simplifiés écrite pour des utilisateurs non techniques](docs/install/).

Comment aider
-----------

The *single* biggest way you can help it to *add* new mainboards to Libreboot,
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

Vous pouvez allez voir les bugs listés sur le [traqueur de bugs](https://codeberg.org/libreboot/lbmk/issues).

Si vous trouvez un bug et avez un correctif, [voici les instructions pour envoyer des patchs](git.md), et vous pouvez aussi nous les signaler.
Par ailleurs, ce site est écrit en Markdown et hébergé dans un [dépôt séparé](https://codeberg.org/libreboot/lbwww) où
vous pouvez envoyer vos patchs.

La discussion sur le dévéloppement de Libreboot et le support utilisateur
se font toutes sur le canal IRC. Plus d'information est disponible sur 
la [page de contact](contact.md).
