---
title: Installer Libreboot, firmware de démarrage BIOS/UEFI open source
x-toc-enable: true
...

Firmware de démarrage BIOS/UEFI open source
------------------------------

**N'oubliez TOUJOURS de faire une sauvegarde de la flash actuelle avant de l'écraser,
quel que soit le firmware que vous avez actuellement et celui avec lequel vous
la reflashez ; cela inclut les mises à jour entre versions de Libreboot. Utilisez
l'option `-r` de flashprog au lieu de `-w`, pour lire la flash.**

AVERTISSEMENT DE SÉCURITÉ !
---------------------------

**[VEUILLEZ LIRE CES INSTRUCTIONS AVANT D'INSTALLER](ivy_has_common.md), SINON
VOUS RISQUEZ DE BRIQUER VOTRE MACHINE !!

Si vous [compilez à partir des sources](../build/), vous pouvez ignorer les conseils ci-dessus.
Veillez tout de même à sauvegarder le firmware actuel avant de flasher !

Appareils achetables directement chez [Minifree](https://minifree.org) ;
-------------------------------------------------------------------

### Portables et ordinateurs de bureau de 8e génération (Kabylake Refresh)

-   **[Lenovo ThinkPad X280](https://minifree.org/product/libreboot-x280.html)**

-   **[Lenovo ThinkPad T480 et ThinkPad T480s](https://minifree.org/product/libreboot-t480.html)**

-   **[Lenovo ThinkPad T580](https://minifree.org/product/libreboot-t580.html)**

-   **[Dell OptiPlex 3050 Micro](https://minifree.org/product/libreboot-3050-micro/)**

Pris en charge par Libreboot ;
-----------------------

**Si vous souhaitez une installation professionnelle de Libreboot,
un [service d'installation](https://minifree.org/product/installation-service.html)
est disponible ; il propose aussi d'autres firmwares coreboot en plus de Libreboot. Par
exemple, l'installation sur Chromebook via la distribution UEFI coreboot MrChromebox est
également disponible.**

### Portables (Intel, x86)

#### 8e génération (Kabylake Refresh).

-   [Lenovo ThinkPad X280](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-x280.html)**
-   [Lenovo ThinkPad T480 et ThinkPad T480s](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-t480.html)**
-   [Lenovo ThinkPad T580](t480.html) (Complexe, DDR4) - Aussi **[disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-t580.html)**

#### 5e génération.

-   [HP EliteBook 820 G2](hp820g2.md) (Complexe, DDR3L)

#### 4e génération.

-   [Lenovo ThinkPad W541](w541_external.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad T440p](t440p_external.md) (Complexe, DDR3L)

#### 3e génération.

-   [HP EliteBook 2170p](hp2170p.md) (Modéré, DDR3L)
-   [HP EliteBook 2570p](hp2570p.md) (Modéré, DDR3L)
-   [HP EliteBook 8470p](hp8470p.md) (Modéré, DDR3L)
-   [HP EliteBook Folio 9470m](hp9470m.md) (Complexe, DDR3L)
-   Lenovo ThinkPad T430 (**mais pas le T430S**) (Complexe, DDR3L)
-   Lenovo ThinkPad T530 (Complexe, DDR3L)
-   [Lenovo Thinkpad X230 / X230 Tablet](x230_external.md) (Complexe, DDR3L)
-   [HP EliteBook 2560p](hp2560p.md) (Modéré, DDR3L)
-   [HP EliteBook 8460p](hp8460p.md) (Modéré, DDR3L)
-   [HP EliteBook 8560w](hp8560w.md) (Modéré, DDR3L)
-   [Dell Latitude E5420, E5520, E5530, E6420, E6430, E6520, E6530, E6230, E6330, E6220, E7240](latitude.md) (Simple, DDR3L)
    (variantes à GPU Intel uniquement)
-   [Lenovo Thinkpad T420/T520](t420_external.md) (Complexe, DDR3L)
-   Lenovo ThinkPad T420s (Complexe, DDR3L)
-   [Lenovo ThinkPad X220/X220](x230_external.md) (Complexe, DDR3L)

#### Core 2

-   [Dell Latitude E4300, E6400, E6400 XFR et E6400 ATG, tous avec GPU Nvidia ou Intel](latitude.md) (Simple, DDR2L + DDR3L pour l'E4300)
-   [Lenovo ThinkPad R400](r400.md) (Complexe, DDR3L)
-   Lenovo ThinkPad R500 (Complexe, DDR3L)
-   [Lenovo ThinkPad T400 / T400S](t400.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad T500 / W500](t500.md) (Complexe, DDR3L)
-   [Lenovo ThinkPad X200 / X200S / X200 Tablet](x200.md) (Complexe, DDR3L)
-   [Apple MacBook1,1 et MacBook2,1](macbook21.md) (Complexe pour le 1.1 / Simple pour le 2.1 - 2007, DDR2L)
-   [Lenovo ThinkPad T60](t60_unbrick.md) (avec GPU Intel) (Complexe, DDR2L)
-   [Lenovo ThinkPad X60](x60_unbrick.md) (avec GPU Intel) (Complexe, DDR2L)
-   Lenovo ThinkPad X60S (avec GPU Intel) (Complexe, DDR2L)
-   [Lenovo ThinkPad X60 Tablet](x60tablet_unbrick.md) Tablet (avec GPU Intel) (Complexe, DDR2L)

### Consoles de jeu

-   [Sony Playstation](playstation.md) (Complexe) (PS1/PSX)

### Serveurs (AMD, x86 avec prise en charge ECC)

#### Opteron 6300

-   [Carte mère ASUS KGPE-D16](kgpe-d16.md) (Modéré, DDR3)

#### Opteron 4300/4200

-   [Carte mère ASUS KCMA-D8](kcma-d8.md) (Modéré, DDR3)

#### Opteron 2000/8000

-   [Carte mère ASUS KFSN4-DRE](kfsn4-dre.md) (Complexe, DDR2)

### Ordinateurs de bureau/stations de travail (Intel, x86)

#### 12e génération

-   [Topton X2E N150 alias BKHD H30W-N150](x2e-n150.md) (Simple, DDR5)

#### 8e génération (Kabylake Refresh).

(Les processeurs d'autres générations sont aussi compatibles avec les cartes mères plus anciennes et plus récentes ; les 4 cœurs sont confirmés, les processeurs 6/8 cœurs pourraient fonctionner, non testé.)

-   [Dell OptiPlex 3050 Micro](dell3050.html) (Simple, DDR4) - **Aussi [disponible à l'achat avec Libreboot préinstallé](https://minifree.org/product/libreboot-3050-micro/)**
-   [Dell OptiPlex 5050 SFF](dell5050.html) (Simple, DDR4)

#### 5e -> 4e génération (Core + Xeon v3-4)

(Les processeurs de 5e génération sont compatibles avec certaines cartes de 4e génération ; C222/4/6 avec initialisation native de la RAM uniquement, et chipsets H/Z97
                  https://en.wikipedia.org/wiki/LGA_1150#Haswell_chipsets )

-   [Dell OptiPlex 7020/9020/XE2/T1700 MT et SFF](dell9020.md) (Simple, DDR3 + ECC pour le T1700)

#### 3e -> 2e génération (Core + Xeon v1-2)

(Les processeurs sont entièrement compatibles avec les cartes mères d'autres générations)

-   [Dell OptiPlex 7010/9010 SFF/MT +T1650](dell7010.md) (Simple, DDR3 + ECC pour le T1650)
-   [HP Elite 8300 USDT](hp8300usdt.md) (Simple, DDR3)
-   [HP Elite 8200 SFF/MT](hp8200sff.md) (Simple, DDR3) (le HP 6200 Pro Business fonctionne probablement aussi)

#### Core 2

-   [Acer G43T-AM3](acer_g43t-am3.md) (Simple, DDR3)
-   [Dell OptiPlex 780 variantes, par ex. MT, USFF](dell780.md) (Simple, DDR3)
-   [Carte mère Gigabyte GA-G41M-ES2L](ga-g41m-es2l.md) (Simple, DDR2)
-   [Apple Apple iMac 5,2](macbook21.md) (Simple, DDR2) (Même procédure de flash que le Macbook 2.1)

**Bonnel (Atom)**
-   [Intel D945GCLF](d945gclf.md) (Modéré, DDR2)
-   Cartes mères Intel D510MO et D410PT (Modéré, DDR2)

### Portables (ARM, avec charge utile U-Boot)

-   [ASUS Chromebook Flip C101 (gru-bob)](chromebooks.md) (Complexe)
-   [Samsung Chromebook Plus (v1) (gru-kevin)](chromebooks.md) (Complexe)

### Émulation

-   [Qemu x86 et arm64](../misc/emulation.md)

Plus d'informations sur le système de compilation de Libreboot
se trouvent dans le [manuel de maintenance de lbmk](../maintain/),
et des informations sur le portage de nouveaux systèmes dans
le [manuel de portage](../maintain/porting.md).

Installer Libreboot par flashage externe
---------------------------------------

Consultez l'article suivant :\
[Réécrire en externe une flash NOR 25xx via le protocole SPI](spi.md)

Il est vivement conseillé d'*avoir* une configuration de flashage externe, et de vérifier
qu'elle fonctionne, avant de tenter un flashage interne. Cela s'ajoute à la
sauvegarde du contenu actuel de la flash avant de flasher, que vous la lisiez
en externe ou en interne - si seul le flashage externe est disponible, alors
en général seule la lecture externe l'est aussi.

Un problème ?
----------

Vous pouvez poser vos questions sur l'[IRC de Libreboot](../../contact.md) et d'autres canaux.

Si vous souhaitez une installation professionnelle, Minifree Ltd vend
[Libreboot préinstallé](https://minifree.org/) sur du matériel sélectionné, et propose aussi
un [service de préinstallation de Libreboot](https://minifree.org/product/installation-service/)
si vous voulez envoyer votre machine pour qu'on y installe Libreboot à votre place.

Leah Rowe, la fondatrice et développeuse principale de Libreboot, est aussi propriétaire
et gérante de Minifree Ltd ; les ventes financent le projet Libreboot.

Désactiver la sécurité avant de flasher
--------------------------------

**Avant un flashage interne, vous devez d'abord désactiver les protections de `/dev/mem`.
Veillez à les réactiver une fois terminé.**

**Voir : [Désactiver la protection de /dev/mem](devmem.md)**

Noms des fichiers d'image ROM
--------------------

Les images ROM de Libreboot sont nommées ainsi :
`payload_board_inittype_displaytype_keymap.rom`

L'option `payload` peut être SeaBIOS, SeaGRUB ou U-Boot. Si GRUB est disponible
sur une carte donnée, dans la flash, SeaBIOS et SeaGRUB sont tous deux fournis ; les images SeaBIOS
donnent quand même accès à GRUB via le menu de SeaBIOS, et SeaGRUB signifie que
SeaBIOS charge automatiquement GRUB depuis la flash en premier (mais vous pouvez toujours choisir
autre chose en appuyant sur ÉCHAP dans SeaBIOS lorsque c'est proposé).

Inittype peut être `libgfxinit`, `vgarom` ou `normal`. L'option `libgfxinit`
signifie que coreboot fournit l'initialisation vidéo native, pour les graphismes intégrés.
L'option `vgarom` signifie que coreboot exécute une option ROM VGA pour l'initialisation
vidéo. L'option `normal` signifie que coreboot ne fournit aucune initialisation
vidéo, ni par ROM VGA ni par code natif.

Displaytype peut être `txtmode` ou `corebootfb` - si inittype est `normal`, cette
option est ignorée car `txtmode` est supposé.

Si `payload` vaut `seabios` au lieu de `seagrub`, aucune disposition de clavier n'est insérée dans la
flash et seul le QWERTY US est supposé ; sinon, la disposition de clavier correspond à celle utilisée
dans GRUB avec les configurations de charge utile `seagrub`.

Si vous utilisez une image libgfxinit sur un ordinateur de bureau, vous pouvez quand même insérer une
carte graphique et cela fonctionnera très bien ; sa propre option ROM VGA sera
exécutée à la place, si la charge utile principale est SeaBIOS, qu'il s'agisse de
SeaBIOS pur ou d'une configuration SeaGRUB.

Mises à jour du firmware EC
-------------------

Évidemment, un firmware EC libre serait préférable, mais ce n'est pas le cas sur
toutes les machines. Nous aimerions avoir un firmware EC libre sur davantage de machines, mais
pour l'instant, nous devons nous fier au fabricant dans beaucoup de cas. L'EC se trouve généralement sur une
flash séparée, donc on n'y penserait pas si l'on ne savait pas qu'il existe ;
c'est précisément pour cela qu'on le mentionne, pour que vous y pensiez,
[parce que les logiciels propriétaires sont mauvais](../../news/policy.md).

Dans de nombreux cas, le firmware EC doit être mis à jour sur un circuit intégré distinct de la
flash de démarrage principale, ce qui ne peut généralement se faire qu'avec l'outil du fabricant,
exécuté depuis le firmware de démarrage du fabricant, et généralement uniquement sous Windows, car
les mises à jour EC et BIOS/UEFI sont fournies dans le même utilitaire. Renseignez-vous sur ce que vous devez
faire pour votre machine avant d'installer Libreboot.

Il est recommandé de mettre à jour vers la dernière version du firmware EC. Le
[firmware EC](../../faq.md#ec-embedded-controller-firmware)

Mettre à jour l'EC peut parfois apporter un bénéfice selon le fabricant. Par
exemple, il peut corriger des problèmes d'alimentation, ce qui peut ensuite améliorer l'autonomie de la batterie.

### ThinkPad

Voir : <http://www.thinkwiki.org/wiki/BIOS_update_without_optical_disk>

Sinon, consultez le site web de Lenovo pour trouver l'utilitaire de mise à jour de votre
carte mère.

### HP EliteBook

Le firmware EC est requis dans la flash de démarrage principale, mais le système de compilation de Libreboot
le télécharge automatiquement depuis HP pour chaque machine, et l'insère, donc
vous n'avez pas à vous en soucier. Assurez-vous simplement que les [fichiers propriétaires sont
insérés](ivy_has_common.md) si vous utilisez des images de version.

### Autres

Le même principe s'applique aux autres fabricants de portables.

Les machines qui ne sont pas des portables n'ont généralement pas de contrôleur embarqué.

Instructions d'installation de Libreboot
-----------------------------------

En général, si Libreboot fonctionne déjà, vous pouvez passer directement
à la dernière section de cette page, qui fournit des instructions générales de flashage
interne. Le flashage interne consiste à flasher la machine cible depuis la machine cible elle-même,
depuis un système d'exploitation qui y tourne.

Certaines cartes nécessitent des étapes particulières, même si Libreboot fonctionne déjà,
par exemple si vous avez [verrouillé la flash](../linux/grub_hardening.md), ou
autre exemple, [SureStart sur le HP EliteBook 820 G2](hp820g2.md)

Par conséquent, avant de suivre les guides génériques, vérifiez d'abord si
votre carte a des instructions particulières ; sinon, utilisez le guide générique à la
fin de cet article.

### Adresse MAC Intel GbE (systèmes basés sur IFD)

Sur toutes les plateformes Intel sauf X4X (par ex. Gigabyte GA-G41M-ES2L) et i945
(par ex. ThinkPad X60, ThinkPad T60, MacBook2,1), un Intel Flash Descriptor est
utilisé. Si la carte dispose d'un Ethernet gigabit Intel, l'adresse MAC est incluse dans
la flash, et peut (doit) être modifiée avant l'installation.

Vous pouvez utiliser [nvmutil](nvmutil.md) pour changer l'adresse MAC. Vous effectuerez
cette modification sur l'image ROM, avant de la flasher.

### Verrouillage de la flash / sécurité du démarrage

On l'appelle de façon informelle *Secure libreBoot*.

Un verrouillage complet de la flash est possible, avec vérification cryptographique de votre
noyau Linux et d'autres fichiers, grâce à des fonctionnalités spéciales de la charge utile GRUB.

Voir : [Durcissement de GRUB / Secure libreBoot](../linux/grub_hardening.md)

Si vous l'avez déjà fait, il est possible que vous ne puissiez plus
flasher en interne. Dans ce cas, vous devez [flasher en externe](spi.md).

### Mettre à jour une installation existante

Sauf indication contraire, dans les sections propres à chaque carte mère ci-dessous,
une installation Libreboot existante peut être mise à jour par flashage interne,
sans étape particulière ; suivez simplement le guide général de flashage interne,
dans la dernière section plus bas dans cette page.

Si vous avez une installation Libreboot existante mais que vous avez *verrouillé la flash*,
la mettre à jour nécessitera un flashage externe.

Si vous avez actuellement le firmware d'origine, vous devrez probablement flasher
en externe ; sur *certaines* machines, le flashage interne est possible, avec en général
des étapes particulières différentes de celles d'une mise à jour d'une installation existante.

Les sections suivantes concernent des cartes mères précises, là où c'est indiqué,
suivies d'instructions générales de flashage interne le cas échéant.

### ThinkPad X200/T400/T500/W500/R400/R500

Si vous utilisez l'une de ces machines avec le BIOS Lenovo, vous devez flasher Libreboot
en externe, car le firmware d'origine restreint les écritures dans la flash.

Ces machines utilisent toutes des circuits de flash SOIC8/SOIC16. Consultez les pages propres à
chaque machine :

* [ThinkPad X200](x200.md)
* [ThinkPad T400](t400.md)
* [ThinkPad R400](r400.md)
* [ThinkPad T500/W500](t500.md) (le R500 est similaire)

NOTE : les T400S, X200S et X200 Tablet nécessitent des étapes différentes, car ils ont
des circuits de flash WSON8, ce qui nécessitera de la soudure. Veuillez lire
le [guide de flashage externe](spi.md) dans la section consacrée au WSON.

Vous pouvez trouver en ligne des sondes WSON8, similaires à une pince SOIC8/SOIC16. Votre
expérience peut varier, mais le WSON8 a le même brochage que le SOIC8, donc vous aurez peut-être
un peu de chance avec ça.

Échangez à chaud le circuit de flash avec un autre pendant qu'il fonctionne, et flashez-le
en interne.

### ThinkPad X220/X220T/T420/T420s/T520

T420/T520 : [instructions de flashage du T420](t420_external.md)

Les T420s/T520 sont similaires, en termes de montage/démontage.

Quant aux X220/X220 Tablet :

Le X220/X220 Tablet est essentiellement identique au X230, mais ne peut pas être flashé
en interne ; vous devez utiliser [un flasheur externe](spi.md).

Sinon, consultez le [démontage du X230](x230_external.md). Notez que le X220
a une seule flash de 8 Mo, au lieu de 8 Mo et 4 Mo.

### ThinkPad X230/T430/T530/W530/X230T

Le flashage interne depuis le BIOS d'origine vers Libreboot est possible, mais déconseillé.
Voir : [Flashage interne des ThinkPad IvyBridge](ivy_internal.md).

Le flashage externe est recommandé, mais documenté uniquement pour le X230.
Voir : [guide d'installation du ThinkPad X230](x230_external.md).

Sinon, consultez le [flashage SPI externe](spi.md).

### ThinkPad T60/X60/X60Tablet/X60S

Seul le GPU Intel est compatible. Ne flashez pas les modèles à GPU ATI.

Guides de flashage externe :

* [ThinkPad X60](x60_unbrick.md)
* [ThinkPad X60 Tablet](x60tablet_unbrick.md)
* [ThinkPad T60](t60_unbrick.md)

Ces machines peuvent aussi être flashées en interne, en exploitant un bogue
du BIOS Lenovo d'origine. S'il y a un mot de passe BIOS au démarrage, vous devriez
simplement flasher en externe.

Instructions de flashage interne :

D'abord, assurez-vous que votre pile CR2032/CMOS fonctionne. C'est elle qui
alimente la SRAM contenant les réglages du BIOS, ainsi que l'horloge temps réel.
Elle conserve aussi la valeur BUC.TS - c'est ce dont nous avons besoin.

Le registre BUC (Backup Control) contient un bit appelé Top Swap (TS). Le bootblock de 64 Ko
situé en haut de la flash est complété par un Top Swap de secours juste au-dessus.
Celui de la fin ne peut pas être flashé en interne pendant que le BIOS Lenovo tourne,
mais le reste peut l'être (tout ce qui se trouve au-dessus du bootblock principal).

En activant le bit TS, vous pouvez faire démarrer la machine depuis le bootblock de secours.

Téléchargez l'archive d'utilitaires de Libreboot 20160907, et vous y trouverez
ces binaires :

* `flashrom`
* `flashrom_i945_sst`
* `flashrom_i945_mx`

Vous y trouverez aussi l'outil bucts. Exécutez-le en root :

	./bucts 1

Exécutez maintenant ces deux commandes en root :
	
	./flashrom_i945_sst -p internal -w coreboot.rom
	./flashrom_i945_mx -p internal -w coreboot.rom

Vous verrez beaucoup d'erreurs. C'est normal. Vous devriez voir quelque chose comme :

```
Reading old flash chip contents... done.
Erasing and writing flash chip... spi_block_erase_20 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
spi_block_erase_52 failed during command execution at address 0x0
Reading current flash chip contents... done. Looking for another erase function.
Transaction error!
spi_block_erase_d8 failed during command execution at address 0x1f0000
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_60 failed during command execution
Reading current flash chip contents... done. Looking for another erase function.
spi_chip_erase_c7 failed during command execution
Looking for another erase function.
No usable erase functions left.
FAILED!
Uh oh. Erase/write failed. Checking if anything has changed.
Reading current flash chip contents... done.
Apparently at least some data has changed.
Your flash chip is in an unknown state.
```

Si vous voyez cela, réjouissez-vous ! Cela signifie que le flashage a réussi. Ne
paniquez pas. Éteignez maintenant, attendez quelques secondes, puis rallumez.

Le bootblock principal n'est toujours pas flashé, mais vous pouvez éteindre, attendre quelques
secondes et redémarrer. Quand vous le ferez, vous aurez Libreboot. Veillez
à flasher une seconde fois, comme ceci :
	
	flashprog -p internal -w coreboot.rom

Libreboot recommande désormais `flashprog`, un fork de flashrom, mais nous avons utilisé
flashrom dans la version de 2016. Les binaires flashrom macronix/sst qui s'y trouvent sont
spécialement modifiés ; consultez le code source de Libreboot 20160907 pour les patchs
exacts. Les patchs modifient certaines définitions de puces flash dans flashrom, afin d'exploiter
le bogue du BIOS Lenovo qui permet le flashage interne.

Vous devez vous assurer que le second flashage est effectué, au redémarrage, car
sinon, si la pile CR2032 meurt, bucts sera réinitialisé et la machine ne
démarrera plus.

Quand vous avez fait le second flashage, qui inclut l'écrasement du bootblock
principal, remettez bucts à zéro :

	./bucts 0

Le second flashage peut se faire simplement en suivant le guide général de flashage interne
plus bas dans cette page.

### Chromebooks ARM

Voir : [Instructions de flashage des Chromebooks](chromebooks.md)

NOTE : les instructions générales de flashage (plus loin dans cette page) ne s'appliquent qu'aux
machines x86, car les Chromebooks utilisent toujours flashrom avec
l'argument `-p host` au lieu de `-p internal` lors du flashage, et vous devez généralement
flasher en externe, à cause du modèle de sécurité de Google.

### QEMU (arm64 et x86)

Libreboot peut être utilisé sur QEMU (machine virtuelle), ce qui est utile pour déboguer
des charges utiles et pour essayer Libreboot en général, sans matériel réel.

Voir : [Guide Libreboot QEMU](../misc/emulation.md)

Installer via le CPU hôte (flashage interne)
----------------------------------------

NOTE : cela s'applique principalement aux machines x86.

Veuillez consulter les autres sections listées ci-dessus, pour voir s'il y a quelque chose
concernant votre carte mère. Le flashage interne signifie que vous démarrez Linux ou
BSD sur la machine cible, et que vous y exécutez `flashprog`, flashant ainsi la machine
directement.

**Si vous ne pouvez pas flasher en interne, vous devez [flasher en externe](spi.md).**

Le flashage interne est souvent indisponible avec le firmware d'origine, mais il est
généralement possible lorsque Libreboot fonctionne (sauf circonstances particulières).

### Exécuter flashprog sur le CPU hôte

**Lorsque vous utilisez des images de version, n'oubliez pas de toujours
[insérer les fichiers propriétaires](ivy_has_common.md). Lorsque vous compilez à partir des sources,
les fichiers propriétaires sont ajoutés automatiquement à la compilation (ils ne sont pas
présents dans les images de version).**

#### Taille de la puce flash

Utilisez ceci pour la connaître :

	flashprog -p internal

La sortie contiendra des informations sur votre flash de démarrage.

#### Mode d'emploi : lire/écrire/effacer la flash de démarrage

Comment lire le contenu actuel de la puce :

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -r dump.bin

Vous devriez quand même faire plusieurs dumps, même si vous flashez en interne, afin de
vous assurer d'obtenir les mêmes sommes de contrôle. Vérifiez chaque dump avec `sha1sum`

Comment effacer et réécrire le contenu de la puce :

	sudo flashprog -p internal:laptop=force_I_want_a_brick,boardmismatch=force -w libreboot.rom

NOTE : `force_I_want_a_brick` n'est pas effrayant. N'ayez pas peur ! Cela désactive simplement
les contrôles de sécurité de flashprog. Flashprog et coreboot évoluent beaucoup au fil des années,
et il est parfois nécessaire d'utiliser cette option. Si cela vous fait peur, suivez simplement
les instructions ci-dessus, mais retirez cette option. Utilisez donc simplement `-p internal`.
Si cela ne fonctionne pas, essayez ensuite `-p internal:boardmismatch=force`. Si cela ne
fonctionne pas, essayez `-p internal:boardmismatch=force,laptop=force_I_want_a_brick`. Tant
que vous *vous assurez* d'utiliser la bonne ROM pour votre machine, exécuter flashprog
sera sans danger. Ces options supplémentaires désactivent simplement les contrôles de sécurité de flashprog.
Il n'y a rien à craindre.

En cas de succès, il affichera soit `VERIFIED`, soit que le contenu de la puce
est identique à l'image demandée.

NOTE : il existe des exceptions où ce qui précède n'est pas possible. Lisez à leur sujet dans
les sections ci-dessous :

Cartes retirées
--------------

Ces cartes étaient dans Libreboot, mais ont été retirées avec l'intention de
les rajouter ultérieurement. Elles ont été retirées en raison de problèmes. Liste :

-   [Acer Chromebook 13 (CB5-311, C810) (nyan-big)](chromebooks.md)
-   [ASUS Chromebit CS10 (veyron-mickey)](chromebooks.md)
-   [ASUS Chromebook C201PA (veyron-speedy)](c201.md)
-   [ASUS Chromebook Flip C100PA (veyron-minnie)](chromebooks.md)
-   [Hisense Chromebook C11 et autres (veyron-jerry)](chromebooks.md)
-   [HP Chromebook 11 G1 (daisy-spring)](chromebooks.md)
-   [HP Chromebook 14 G3 (nyan-blaze)](chromebooks.md)
-   [Samsung Chromebook 2 11" (peach-pit)](chromebooks.md)
-   [Samsung Chromebook 2 13" (peach-pi)](chromebooks.md)
-   [Samsung Chromebook XE303 (daisy-snow)](chromebooks.md)
-   Lenovo ThinkPad X301 (toujours dans lbmk, mais avec `release="n"`)

### NOTES sur les cartes retirées

**AVERTISSEMENT : les cartes veyron speedy (par ex. C201) ont une initialisation vidéo non fonctionnelle
au 19 février 2023, et aucun correctif n'est encore disponible à cette date. Voir :
<https://notabug.org/libreboot/lbmk/issues/136> - la dernière révision testée
de 2021.01 est connue pour fonctionner, pour u-boot sur cette carte. Voir :\
<https://wiki.postmarketos.org/wiki/ASUS_Chromebook_C201_(google-veyron-speedy)>
(alpernebbi sur IRC s'y intéresse, pour bissecter uboot et mettre à jour les dernières
révisions) - pour l'instant, les images ROM ont été supprimées des versions Libreboot 20221214
et 20230319.**

**AVERTISSEMENT : les cartes daisy- et peach- nécessitent un firmware de chargeur d'amorçage BL1, mais
celui de coreboot 3rdparty est un faux fichier/fichier de substitution. Il nous faut une logique dans le
système de compilation de Libreboot pour les récupérer/extraire correctement, ainsi que de la documentation pour
l'expliquer. Pour l'instant, considérez-les comme non fonctionnelles - les images ROM sont exclues,
pour l'instant, et ont été supprimées des versions Libreboot 20221214 et 20230319.
- voir : <https://review.coreboot.org/plugins/gitiles/blobs/+/4c0dcf96ae73ba31bf9aa689768a5ecd47bac19e>
et <https://review.coreboot.org/plugins/gitiles/blobs/+/b36cc7e08f7337f76997b25ee7344ab8824e268d>**

D945GCLF : ne démarre pas du tout, d'après le dernier rapport. Le D510MO est toujours dans
lbmk mais a aussi été signalé comme problématique ; les autres cartes devraient fonctionner (voir la liste
ci-dessus).

AVERTISSEMENT : la prise en charge des Chromebooks ARM est au stade de preuve de concept. Consultez
[docs/uboot/](../uboot/) pour plus d'informations sur la charge utile U-Boot.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/docs/install/>