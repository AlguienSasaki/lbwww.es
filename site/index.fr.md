---
title: Projet Libreboot
x-toc-enable: true
---

Le projet Libreboot fournit un firmware [libre et Open-Source](https://writefreesoftware.org/) basé sur Coreboot, remplaçant le BIOS/UEFI propriétaire sur des [cartes mères Intel/AMD x86 et ARM](docs/install/#which-systems-are-supported-by-libreboot), pour des ordinateurs de bureau et des ordinateurs portables. 
Il initialise le matériel (le contrôleur de mémoire, le CPU, les périphériques) et lance un bootloader pour votre système d’exploitation. Les systèmes [Linux](docs/linux/) et [BSD](docs/bsd/) sont bien pris en charge. De l’aide est disponible sur [\#Libreboot](https://web.libera.chat/#Libreboot) via [Libera](https://libera.chat/). La page des tâches répertorie les projets en cours (les contributions sont les bienvenues si vous souhaitez y travailler vous-même).

<img tabindex=1 class="r" src="https://av.libreboot.org/t480/t480.jpg" /><span class="f"><img src="https://av.libreboot.org/t480/t480.jpg" /></span>

Vous pouvez également acheter du matériel avec [Libreboot pré-installé](https://minifree.org/) via l'entreprise Minifree Ltd,
ou envoyer votre propre ordinateur [pour y installer Libreboot](https://minifree.org/product/installation-service/).

Leah Rowe, fondateur et principal développeur du projet Libreboot, est également propriétaire de Minifree Ltd. Les ventes contribuent à financer le développement du projet Libreboot.

**NOUVELLE VERSION: La dernière version est [Libreboot 20241206](news/Libreboot20241206.md), sortie
le 6 December 2024.**

Pourquoi devriez-vous utiliser *Libreboot*?
-----------------------------------

<img tabindex=1 class="l" src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /><span class="f"><img src="https://av.libreboot.org/hp9470m/9470m+2560p.jpg" /></span>

Libreboot vous offre des [libertés](https://writefreesoftware.org/) que vous n’auriez pas avec d’autres micrologiciels de démarrage. [Puissant](docs/linux/grub_hardening.md) et hautement [configurable](docs/maintain), il répond à de nombreux cas d’utilisation.

Vous avez des droits : droit à la vie privée, liberté de pensée, liberté d’expression et droit à l’information. Dans ce cadre, Libreboot vous permet de préserver ces droits. Votre liberté est essentielle. Le [droit à la réparation](https://fr.wikipedia.org/wiki/Droit_%C3%A0_la_r%C3%A9paration) est également crucial. Beaucoup de personnes utilisent des micrologiciels propriétaires (non libres), même lorsqu’elles utilisent [un système d'exploitation libre](https://www.openbsd.org/). Ces micrologiciels sont souvent instables et [contiennent](faq.html#intel) parfois des [portes dérobées](faq.html#amd). 


Libreboot, fondé en décembre 2013, a pour objectif de rendre le micrologiciel libre accessible même aux utilisateurs non techniques.

Libreboot s’appuie sur  [Coreboot](https://www.Coreboot.org) pour
[l'initialisation matérielle](https://doc.Coreboot.org/getting_started/architecture.html). Coreboot est réputé complexe à installer, en particulier pour les utilisateurs non techniques ; il se limite à l’initialisation matérielle de base avant de transmettre le contrôle à une [charge utile](https://doc.Coreboot.org/payloads.html) (comme [GRUB](https://www.gnu.org/software/grub/) ou [Tianocore](https://www.tianocore.org/)), qui nécessite également une configuration. Libreboot simplifie ce processus : il s’agit d’une distribution de Coreboot dotée d’un [système de compilation automatisé](docs/builds/), capable de créer des images ROM complètes pour une installation plus simple et fiable. 

Une documentation détaillée est disponible.

Quelles sont les différences entre Libreboot et Coreboot?
------------------------------------------------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /><span class="f"><img src="https://av.libreboot.org/thinkpadcollection/thinkpadcollection1-min.jpg" /></span>

Contrairement à l'opinion populaire, le but principal de Libreboot n'est pas de fournir un Coreboot sans blobs propriétaires ; ceci n'est qu'une des politiques de Libreboot, certes importante, mais qui reste un aspect mineur de son projet.

De la même façon qu’Alpine Linux est une distribution Linux, Libreboot est une distribution Coreboot. Si vous voulez compiler une image ROM à partir de zéro, vous devrez effectuer une configuration experte de Coreboot, GRUB et de tout autre logiciel nécessaire pour préparer la ROM. Avec Libreboot, vous pouvez télécharger le code source depuis Git ou une archive, exécuter un script, et cela générera une image ROM complète. Le système de compilation automatisé de Libreboot, nommé `lbmk` (Libreboot MaKe), compile ces images ROM automatiquement. Aucune intervention n'est requise, car la configuration est préparée à l'avance.

Si vous deviez compiler Coreboot classique sans utiliser le système de compilation automatisé de Libreboot, cela nécessiterait bien plus d'efforts et de connaissances techniques pour créer une configuration fonctionnelle.

Les versions de Libreboot fournissent des images ROM pré-compilées que vous pouvez installer facilement, [sans connaissances ou compétences particulières](docs/install/).

Comment aider
-----------

<img tabindex=1 class="r" style="max-width:25%;" src="https://av.libreboot.org/t1650/t1650_grub.jpg" /><span class="f"><img src="https://av.libreboot.org/t1650/t1650_grub.jpg" /></span>

La meilleure façon d'aider le projet est d’ajouter de nouvelles cartes mères au projet Libreboot en soumettant une configuration. Tout ce qui est pris en charge par Coreboot peut être intégré à Libreboot, avec des images ROM fournies dans les versions publiées. Voir :

* [Postuler pour devenir mainteneur/testeur de cartes mères](docs/maintain/testing.md)
* [Guide de portage pour nouvelles cartes mèress](docs/maintain/porting.md)
* [Documentation du système de compilation Libreboot](docs/maintain/)

Ensuite, il y a la maintenance du système de compilation (voir ci-dessus) ainsi que la **documentation**, que nous prenons très au sérieux. La documentation est essentielle dans tout projet.

Le support utilisateur est également crucial. Restez actif sur IRC et, si vous êtes dans la capacité d'aider quelqu'un avec son problème, cela rendra un grand service au projet. De nombreuses personnes demandent également du support utilisateur sur le subreddit r/Libreboot.

Vous pouvez allez voir les bugs listés sur le [traqueur de bugs](https://codeberg.org/Libreboot/lbmk/issues).

<img tabindex=1 class="l" style="max-width:15%;" src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /><span class="f"><img src="https://av.libreboot.org/hp8200sff/grub_open.jpg" /></span>

Vous pouvez nous signaler les bugs que vous trouverez. Vous pouvez également proposer un correctif en [suivant ces instructions pour envoyer des patchs](git.md),
Par ailleurs, ce site est écrit en Markdown et hébergé dans un [dépôt séparé](https://codeberg.org/Libreboot/lbwww) où
vous pouvez envoyer vos patchs.

La discussion sur le dévéloppement de Libreboot et le support utilisateur
se font toutes sur le canal IRC. Plus d'informations sont disponibles sur 
la [page de contact](contact.md).

Aide pour la traduction
--------------------------------------

Libreboot dispose actuellement de pages Web traduites en ukrainien et en français.

Si vous souhaitez participer à le traduction, vous pouvez traduire des pages, mettre à jour les traductions existantes et soumettre vos versions traduites. Pour les instructions, veuillez lire :

[Comment proposer une traduction pour Libreboot.org](news/translations.md)

Et même si quelqu'un travaille déjà sur des traductions dans une langue donnée, nous pouvons travailler à plusieurs. Plus on est de fous, plus on rit !
