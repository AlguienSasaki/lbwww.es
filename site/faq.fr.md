---
title: Foire aux questions
x-toc-enable: true
...

NOTE : Libreboot s'est standardisé sur [flashprog](https://flashprog.org/wiki/Flashprog)
depuis le 27 janvier 2024. Il s'agit d'un fork de flashrom.

Également connue sous le nom de FAQ (Foire aux questions)

Acheter Libreboot préinstallé
==========

Si vous souhaitez une installation professionnelle, Minifree Ltd vend du matériel avec [Libreboot
préinstallé](https://minifree.org/) sur certains modèles, et propose également un
[service de préinstallation de Libreboot](https://minifree.org/product/installation-service/)
si vous souhaitez envoyer votre machine pour qu'on y installe Libreboot à votre place.

Leah Rowe, fondatrice et développeuse principale de Libreboot, est également propriétaire et
gérante de Minifree Ltd ; les ventes financent le projet Libreboot.

Questions importantes
================

Comment compiler libreboot à partir du code source
------------------------------------

Consultez les [instructions de compilation de lbmk](docs/build/index.uk.md).

Comment fonctionne le système de compilation ?
-------------------------------

Consultez le [manuel de maintenance de lbmk](docs/maintain/).

N'utilisez pas le CH341A !
------------------

Ce programmateur SPI endommagera votre puce ainsi que la carte mère à laquelle il est
connecté.

Lisez les notes sur le CH341A dans [docs/install/spi.md](docs/install/spi.md) pour
en savoir plus.

Flashrom se plaint de l'accès à DEVMEM
--------------------------------------

Si vous exécutez `flashprog -p internal` pour un flashage logiciel et que
vous obtenez une erreur liée à l'accès à /dev/mem, vous devez redémarrer le système avec
le paramètre noyau `iomem=relaxed` avant d'exécuter flashprog, ou utiliser un noyau
sur lequel `CONFIG_STRICT_DEVMEM` et `CONFIG_IO_STRICT_DEVMEM` ne sont pas activés.

Exemple de sortie de flashprog avec `CONFIG_STRICT_DEVMEM` et `CONFIG_IO_STRICT_DEVMEM` tous deux activés :
```
flashprog v0.9.9-r1955 on Linux 4.11.9-1-ARCH (x86_64)
flashprog is free software, get the source code at https://flashprog.org

Calibrating delay loop... OK.
Error accessing high tables, 0x100000 bytes at 0x000000007fb5d000
/dev/mem mmap failed: Operation not permitted
Failed getting access to coreboot high tables.
Error accessing DMI Table, 0x1000 bytes at 0x000000007fb27000
/dev/mem mmap failed: Operation not permitted
```

Le rétroéclairage s'assombrit sur le côté gauche de l'écran
---------------------------------------------------------------------------------------------------------------

Nous ne savons pas comment déterminer la bonne valeur de PWM à utiliser dans
coreboot, nous utilisons donc simplement celle par défaut de coreboot, qui présente
ce problème sur certaines dalles CCFL, mais pas sur les dalles LED.

Vous pouvez résoudre ce problème dans votre distribution en suivant les notes de
la [documentation : réglage du rétroéclairage](../docs/misc/#finetune-backlight-control-on-intel-gpus).

L'Ethernet ne fonctionne pas sur mon X200/T400/X60/T60 quand je le branche
-------------------------------------------------------------------

Ce problème a été observé sur certains systèmes utilisant network-manager. Il se produit
aussi bien avec le BIOS d'origine qu'avec libreboot. C'est une bizarrerie du
matériel. Sur les systèmes Debian, le contournement consiste à redémarrer le service
réseau lorsque vous branchez le câble Ethernet :

	sudo service network-manager restart

Avec systemd, vous pouvez essayer :

	sudo systemctl restart network-manager

(le nom du service peut différer selon votre
configuration)

Mon KCMA-D8 ou KGPE-D16 ne démarre pas avec un module PIKE2008 installé
-----------------------------------------------------------------------

**Les cartes mères ASUS KGPE-D16, KCMA-D8 et KFSN4-DRE ont été supprimées le 19
novembre 2022. Vous pouvez toujours utiliser d'anciennes versions de Libreboot, ainsi que les versions
publiées de ces anciennes versions.**

Le chargement de l'Option ROM du module PIKE2008 sur l'ASUS KCMA-D8
ou KGPE-D16 provoque un blocage du système au démarrage. Il est possible de l'utiliser
dans la charge utile (si vous utilisez une charge utile à noyau Linux, comme linuxboot),
ou de démarrer (avec SeaGRUB et/ou SeaBIOS) depuis du SATA classique, puis de l'utiliser
sous Linux. Le noyau Linux est capable d'utiliser le module PIKE2008
sans charger l'Option ROM.

Comment enregistrer les journaux de panique du noyau sur les ordinateurs portables Thinkpad ?
--------------------------------------------------

Le moyen le plus simple consiste à utiliser netconsole du noyau
et à reproduire la panique. Netconsole nécessite deux machines : celle qui
panique (la source) et celle qui recevra les journaux de plantage (la cible). La source
doit être connectée par câble Ethernet, et la cible doit également être accessible
pendant la panique. Pour configurer ce système, exécutez les commandes
suivantes en tant que root sur la source (`source#`) et en tant qu'utilisateur normal
sur la cible (`cible$`) :

1.  Lancez un serveur d'écoute sur la machine cible (netcat convient très bien) :

	`cible$ nc -u -l -p 6666`

2.  Montez configfs (une seule fois par démarrage ; vous pouvez vérifier s'il est déjà monté
    avec `mount | grep /sys/kernel/config`. Cette commande ne renverra rien
    s'il ne l'est pas).

	`source# modprobe configfs`

	`source# mkdir -p /sys/kernel/config`

	`source# mount none -t configfs /sys/kernel/config`

3.  Trouvez le nom de l'interface Ethernet de la source, de la forme `enp*` ou
    `eth*` ; voyez la sortie de `ip address` ou `ifconfig`.

	`source# iface="enp0s29f8u1"` modifiez ceci

    Renseignez ici l'adresse IPv4 de la machine cible :

	`source# tgtip="192.168.1.2"` modifiez ceci


4.  Créez une cible de journalisation netconsole sur la machine source :

	`source# modprobe netconsole`

	`source# cd /sys/kernel/config/netconsole`

 	`source# mkdir target1; cd target1`

	`source# srcip=$(ip -4 addr show dev "$iface" | grep -Eo '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+')`

	`source# echo "$srcip" > local_ip`

	`source# echo "$tgtip" > remote_ip`

	`source# echo "$iface" > dev_name`

	`source# arping -I "$iface" "$tgtip" -f | grep -o '..:..:..:..:..:..' > remote_mac`

	`source# echo 1 > enabled`

5.  Changez le niveau de journalisation de la console en debug :

	`source# dmesg -n debug`

6.  Vérifiez que la journalisation fonctionne, par exemple en branchant ou en débranchant des
    périphériques USB sur la source.
    Quelques lignes devraient apparaître dans le terminal où vous avez lancé netcat (nc), sur l'hôte cible.

7.  Essayez de reproduire la panique du noyau.

Compatibilité matérielle
======================

Quels systèmes sont compatibles avec libreboot ?
-----------------------------------------------------------------------------------

N'importe quel système peut être ajouté facilement ; la *compatibilité* désigne donc toute
carte intégrée au système de compilation `lbmk` que libreboot utilise.

La [page d'installation](docs/install/) liste les machines compatibles.

Les pièges de la liberté avec le matériel Intel moderne {#intel}
----------------------------------------------------

Coreboot est nominalement un logiciel libre, mais la plupart des cibles x86
qu'il prend en charge nécessitent des blobs binaires, aussi bien sur Intel que sur AMD.

### Intel Management Engine (ME) {#intelme}

NOTE : Les informations ci-dessous sont un peu obsolètes. L'Intel ME ne
s'exécute désormais plus sur un coprocesseur ARC, mais sur une architecture de processeur Intel 486 modifiée,
avec un micrologiciel ME écrit pour x86 et basé sur le système d'exploitation Minix.
Toutefois, la philosophie générale de conception et de fonctionnement reste en grande partie la
même.

Sur *la plupart* des plateformes Intel modernes dotées d'un Intel ME, il est désormais possible de neutraliser
l'Intel ME après le BringUp. Voir :

<https://github.com/corna/me_cleaner>\

Sur tous les ordinateurs portables GM45+ICH9M dotés d'un Intel ME (cela signifie aussi les ordinateurs de bureau
X4X+ICH10), le micrologiciel ME n'est pas nécessaire dans la mémoire flash de démarrage. Soit un
descripteur modifié est utilisé, qui désactive le ME et supprime la région qui lui est dédiée
dans la mémoire flash de démarrage, soit une configuration sans descripteur est utilisée. En revanche, toutes les plateformes
Intel modernes nécessitent par ailleurs la présence d'une image Intel ME dans la mémoire flash de démarrage
principale.

Passons maintenant au sujet principal :

Introduit en juin 2006 avec la famille de chipsets Intel 965 Express pour les contrôleurs
concentrateurs (graphiques et) mémoire, ou (G)MCH, et la famille de contrôleurs d'E/S ICH8,
l'Intel Management Engine (ME) est un environnement
informatique distinct, physiquement situé dans la puce (G)MCH. Au troisième trimestre 2009,
la première génération de processeurs Intel Core i3/i5/i7 (Nehalem) et la cinquième série de
chipsets contrôleurs concentrateurs de plateforme, ou PCH, ont apporté un
ME plus étroitement intégré (désormais en version 6.0) à l'intérieur de la puce PCH,
qui a elle-même remplacé l'ICH. Ainsi, le ME ***est présent sur tous les systèmes de bureau,
mobiles (portables) et serveurs depuis la mi-2006***.

Le ME se compose d'un cœur de processeur ARC (remplacé par d'autres cœurs de processeur
dans les générations suivantes du ME), d'un cache de code et de données, d'un minuteur
et d'un bus interne sécurisé auquel sont connectés des périphériques supplémentaires,
notamment un moteur de cryptographie, des ROM et RAM internes, des contrôleurs
mémoire et un ***moteur d'accès direct à la mémoire (DMA)*** pour accéder à la
mémoire du système d'exploitation hôte, ainsi que pour réserver une région
de mémoire externe protégée afin de compléter la RAM interne limitée du ME.
Le ME dispose également d'un ***accès au réseau*** avec sa propre adresse MAC via le
contrôleur Gigabit Ethernet d'Intel. Son programme de démarrage, stocké dans la ROM
interne, charge un « manifeste » de micrologiciel depuis la puce flash SPI du PC.
Ce manifeste est ***signé par une clé cryptographique de confiance***,
qui diffère selon les versions du micrologiciel ME. Si le manifeste
n'est pas signé par une clé Intel spécifique, la ROM de démarrage ne chargera ni
n'exécutera le micrologiciel, et le cœur de processeur du ME sera arrêté.

Le micrologiciel ME est compressé et composé de modules énumérés dans le
manifeste, accompagnés des empreintes cryptographiques sécurisées de leur contenu.
L'un des modules est le noyau du système d'exploitation, qui repose sur un
***système d'exploitation temps réel (RTOS) propriétaire*** appelé
« ThreadX ». Son développeur, Express Logic, vend des licences et le code
source de ThreadX. Les clients, comme Intel, ont interdiction de divulguer
ou de sous-licencier le code source de ThreadX. Un autre module est le
Dynamic Application Loader (DAL), qui comprend une ***machine virtuelle Java***
et un ensemble de classes Java préinstallées pour la cryptographie, le stockage sécurisé,
etc. Le module DAL peut charger et exécuter des modules ME supplémentaires depuis le
disque dur ou le SSD du PC. Le micrologiciel ME contient aussi plusieurs modules d'applications
natives dans son espace flash, notamment Intel
Active Management Technology (AMT), des implémentations de Trusted
Platform Module (TPM), Intel Boot Guard, ainsi que des systèmes DRM audio et vidéo.

L'application Active Management Technology (AMT), qui fait partie de la marque
« vPro » d'Intel, est un serveur web et un code applicatif qui permettent à des utilisateurs
distants d'allumer, d'éteindre, de consulter des informations et, plus généralement,
de gérer le PC. Elle peut ***être utilisée à distance même lorsque le PC
est éteint*** (via Wake-on-LAN). Le trafic est chiffré à l'aide de bibliothèques SSL/TLS,
mais n'oubliez pas que toutes les principales implémentations SSL/TLS ont connu des vulnérabilités
très médiatisées. L'application AMT elle-même présente des ***[vulnérabilités
connues](https://fr.wikipedia.org/wiki/Active_Management_Technology#Probl%C3%A8mes_de_s%C3%A9curit%C3%A9)***,
qui ont été exploitées pour développer des rootkits et des keyloggers, ainsi que pour obtenir
secrètement un accès chiffré aux fonctions de gestion du PC.
N'oubliez pas que le ME a un accès complet à la RAM du PC. Cela signifie qu'un
attaquant exploitant l'une de ces vulnérabilités pourrait accéder à
tout ce qui se trouve sur le PC : tous les fichiers ouverts, toutes les applications
en cours d'exécution, toutes les frappes au clavier, etc.

[Intel Boot Guard](https://mjg59.dreamwidth.org/33981.md) est une application ME
introduite au deuxième trimestre 2013 avec la version 9.0 du micrologiciel ME, sur la quatrième
génération de processeurs Intel Core i3/i5/i7 (Haswell). Elle permet aux fabricants d'équipement d'origine (OEM) de PC
de générer une paire de clés cryptographiques asymétriques, de graver la clé publique
dans le CPU et d'empêcher le CPU d'exécuter un micrologiciel de démarrage qui n'est pas signé
avec leur clé privée. Cela signifie que ***coreboot et libreboot
ne peuvent pas être portés*** sur de tels PC sans la clé de signature privée de
l'OEM. Notez que les systèmes assemblés à partir d'une carte mère et d'un
CPU achetés séparément ne sont pas concernés, car le fournisseur de la carte mère
(sur laquelle est stocké le micrologiciel de démarrage) ne peut pas influer sur la
clé publique stockée dans le CPU.

Les versions 4.0 et ultérieures du micrologiciel ME (chipsets Intel de série 4 et suivants)
incluent une application ME pour le ***DRM
[audio et vidéo](https://defectivebydesign.org/what_is_drm_digital_restrictions_management)***
appelée « Protected Audio Video Path » (PAVP). Le ME reçoit du système d'exploitation hôte
un flux multimédia chiffré et une clé chiffrée,
déchiffre la clé et envoie la clé déchiffrée au
GPU, qui déchiffre ensuite le média. Le PAVP est également utilisé par une autre application ME
pour dessiner directement à l'écran le clavier de saisie du code PIN d'authentification.
Dans ce cas, l'application PAVP contrôle directement les graphismes qui
apparaissent à l'écran du PC, d'une manière que le système d'exploitation hôte ne peut pas détecter. La version 7.0 du
micrologiciel ME sur le PCH des processeurs Intel Core i3/i5/i7 de 2e génération
(Sandy Bridge) remplace le PAVP par une application DRM similaire appelée
« Intel Insider ». Comme l'application AMT, ces applications DRM,
défectueuses par conception, illustrent la toute-puissance du
ME : ce matériel et son micrologiciel propriétaire peuvent accéder à tout ce qui se trouve en RAM
et le contrôler, et même ***tout ce qui est
affiché***.

L'Intel Management Engine, avec son micrologiciel propriétaire, a un
accès complet au PC et un contrôle total sur celui-ci : il peut allumer ou éteindre le PC,
lire tous les fichiers ouverts, examiner toutes les applications en cours d'exécution, suivre toutes les frappes
au clavier et tous les mouvements de souris, et même afficher ou enregistrer des images à
l'écran. Et il dispose d'une interface réseau manifestement non sécurisée,
qui pourrait permettre à un attaquant sur le réseau d'injecter des rootkits
compromettant totalement le PC et rapportant à l'attaquant toute
l'activité qui s'y déroule. C'est une menace pour la liberté, la sécurité et
la vie privée qui ne peut être ignorée.

Avant la version 6.0 (c'est-à-dire sur les systèmes de 2008/2009 et antérieurs),
le ME peut être désactivé en définissant deux valeurs dans la mémoire flash SPI.
Le micrologiciel ME peut alors être entièrement supprimé de l'espace de la mémoire
flash. Le projet libreboot [le fait](docs/install/ich9utils.md) sur
les systèmes Intel de série 4 qu'il prend en charge, comme le [ThinkPad
X200](../docs/install/x200.uk.md) et le [ThinkPad
T400](../docs/install/t400.md). Le micrologiciel ME version 6.0 et
ultérieures, présent sur tous les systèmes dotés de processeurs Intel Core i3/i5/i7
et de PCH, inclut un micrologiciel « ME Ignition » qui effectue une certaine
initialisation matérielle et gère l'alimentation. Si la ROM de démarrage du ME ne trouve pas
dans la mémoire flash SPI un manifeste de micrologiciel ME avec une signature Intel valide,
l'ensemble du PC s'éteindra au bout de 30 minutes.

En raison de la vérification de signature, le développement d'un remplacement libre
du ME est en principe impossible. La seule entité capable de remplacer
le micrologiciel ME est Intel. Comme indiqué précédemment, le micrologiciel ME contient du
code propriétaire sous licence de tiers, de sorte qu'Intel ne pourrait pas
publier le code source même si elle le souhaitait. Et même si elle développait un tout
nouveau micrologiciel ME sans code propriétaire tiers et
en publiait le code source, la ROM de démarrage du ME rejetterait un micrologiciel modifié
non signé par Intel. Le micrologiciel ME est donc à la fois
irrémédiablement propriétaire et « tivoïsé ».

**En résumé, l'Intel Management Engine et ses applications sont
une porte dérobée disposant d'un accès complet au reste du PC et d'un contrôle total sur celui-ci.
Le ME est une menace pour la liberté, la sécurité et la vie privée, et le projet libreboot
recommande vivement de l'éviter complètement. Dans les versions récentes,
il ne peut pas être supprimé, ce qui signifie qu'il faut éviter toutes les générations récentes de matériel
Intel.**

Le paragraphe *ci-dessus* concerne l'utilisation du micrologiciel Intel ME *complet*,
qui contient du code réseau et, en particulier, l'*Active Management
Technology* (AMT).

On considère que l'utilisation de l'utilitaire `me_cleaner` minimise tout risque de sécurité
lié à l'utilisation de ces plateformes Intel, et coreboot *contient* du code entièrement libre pour les plateformes
sandybridge/ivybridge. Du point de vue de la liberté, elles sont comparables aux ThinkPad
compatibles avec libreboot, et ce sont de très bonnes machines.

De plus amples informations sur le Management Engine se trouvent sur divers sites web,
dont [me.bios.io](http://me.bios.io/Main_Page),
[unhuffme](http://io.netgarage.org/me/), le [wiki de
coreboot](http://www.coreboot.org/Intel_Management_Engine) et
[Wikipédia](https://fr.wikipedia.org/wiki/Intel_Active_Management_Technology).
Le livre ***[Platform Embedded Security Technology
Revealed](https://www.apress.com/9781430265719)*** décrit en détail et de façon
remarquable l'architecture matérielle du ME et les modules applicatifs du micrologiciel.

Si vous êtes bloqué avec un ME (système non libreboot), vous trouverez peut-être ceci
intéressant :
<http://hardenedlinux.org/firmware/2016/11/17/neutralize_ME_firmware_on_sandybridge_and_ivybridge.html>

### Firmware Support Package (FSP) {#fsp}

Sur tous les systèmes Intel récents, la prise en charge de coreboot repose sur
l'intégration d'un blob (propre à chaque système) appelé *FSP* (Firmware Support Package, paquet de support
du micrologiciel), qui gère toute l'initialisation du matériel, y compris
celle de la mémoire et du CPU. La rétro-ingénierie et le remplacement de ce
blob sont quasiment impossibles tant il est complexe. Même pour le développeur le plus
qualifié, il faudrait des années pour le remplacer. Intel distribue
ce blob aux développeurs de micrologiciels, sans le code source.

Comme le FSP est responsable de l'initialisation précoce du matériel, cela signifie
qu'il prend également en charge le SMM (System Management Mode, mode de gestion du système). C'est un mode
particulier qui s'exécute en dessous du niveau du système d'exploitation. **Il est possible que des rootkits
y soient implémentés, qui pourraient exécuter de nombreuses
attaques contre l'utilisateur (la liste est infinie). Tout système Intel comportant
un blob FSP propriétaire ne peut absolument pas être considéré comme digne de confiance.** En fait, plusieurs
rootkits SMM ont été démontrés dans la nature (utilisez un moteur de recherche
pour les trouver).

### Mises à jour du microcode du CPU {#microcode}

Le microcode configure les portes logiques de votre CPU afin d'implémenter l'architecture
du jeu d'instructions. Votre CPU en contient déjà un, mais il offre aussi un moyen de mettre à jour le
microcode au démarrage, ce qui corrige des bogues et améliore considérablement la
fiabilité globale de votre système.

Le microcode a déjà été abordé très en détail dans la [politique sur les blobs
binaires](news/policy.md).

Cette vidéo intéressante explique comment un groupe de personnes
a fait de la rétro-ingénierie du microcode sur des processeurs AMD :

* <https://yewtu.be/watch?v=W3FbTMqYi4U>

* <https://yewtu.be/watch?v=I6dQfnb3y0I>

Le dépôt git de ce projet se trouve ici :

<https://github.com/RUB-SysSec/Microcode>

La vidéo comme le dépôt donnent des informations supplémentaires sur le microcode
des CPU. Son fonctionnement sur AMD sera très similaire à celui d'Intel.

Les pièges de la liberté à prendre en compte avec le matériel AMD {#amd}
----------------------------------------------------------------------------

NOTE : Il existe aujourd'hui openSIL <https://github.com/openSIL/openSIL>, une
tentative d'AMD de fournir à nouveau du code source que des projets comme coreboot
peuvent utiliser, mais AMD reste problématique ; par exemple, le PSP (voir plus bas) ne peut pas
être « neutralisé » (il n'existe encore rien de semblable à `me_cleaner`, ni de *psp\_cleaner*).

AMD a plus ou moins le même problème qu'Intel en matière
de liberté logicielle.

### AMD Platform Security Processor (PSP)

C'est essentiellement la version d'AMD de l'[Intel Management
Engine](#intelme). Il présente les mêmes problèmes fondamentaux de sécurité et de liberté,
bien que l'implémentation soit radicalement différente.

Le Platform Security Processor (PSP) est intégré aux processeurs AMD dont
l'[architecture](https://en.wikipedia.org/wiki/List_of_AMD_CPU_microarchitectures) est de la famille 16h (Puma) tardive, Zen 17h ou ultérieure (ainsi qu'aux
GPU AMD GCN de 5e génération (Vega) ou ultérieurs). Sur les processeurs dotés d'un PSP, celui-ci
contrôle le démarrage du cœur x86 principal. Le micrologiciel du PSP est signé cryptographiquement
par une clé de confiance, comme pour l'Intel ME. Si le micrologiciel PSP intégré
est absent ou si la clé de signature AMD est absente, les cœurs x86 ne
seront pas libérés de la réinitialisation, ce qui rend le système inutilisable.

Le PSP est un cœur ARM avec la technologie TrustZone intégré au processeur
principal. Il a donc la capacité de dissimuler son propre code,
d'effacer la RAM et toutes les données qu'il aurait pu prendre et stocker depuis
la RAM système x86 moins privilégiée (clés de chiffrement du noyau, données de connexion,
historique de navigation, frappes au clavier, qui sait !). Pire encore,
le PSP a théoriquement accès à tout l'espace mémoire du système (AMD
ne le nie pas, ou ne le peut pas, et cela semble nécessaire
pour que les « fonctionnalités » DRM fonctionnent correctement), ce qui signifie qu'il a
au moins un accès par MMIO aux contrôleurs réseau et à tous les autres
périphériques PCI/PCIe installés dans le système.

Théoriquement, tout attaquant ayant accès à la clé de signature AMD pourrait
installer un logiciel malveillant persistant impossible à éradiquer
sans un équipement de flashage externe et une image PSP saine connue. De plus,
de nombreuses vulnérabilités de sécurité ont été démontrées par le passé dans le micrologiciel d'AMD,
et il y a tout lieu de supposer qu'une ou plusieurs vulnérabilités zero-day
se cachent dans le micrologiciel du PSP. Étant donné le niveau de privilège extrême
(anneau -2 ou -3) du PSP, ces vulnérabilités permettraient
de surveiller et de contrôler à distance toute machine dotée d'un PSP,
totalement à l'insu de l'utilisateur.

À l'instar d'Intel Boot Guard (une application de l'Intel
Management Engine), le PSP d'AMD peut aussi agir en tyran en vérifiant les
signatures de tout micrologiciel de démarrage que vous flashez, rendant impossible le remplacement du
micrologiciel de démarrage (par exemple libreboot, coreboot) sur certaines cartes. Des
rapports officieux précoces indiquent qu'un équivalent AMD de boot guard serait utilisé
sur la plupart du matériel OEM, désactivé seulement sur les processeurs dits
« pour passionnés ».

### Micrologiciel AMD IMC

Lisez <https://www.coreboot.org/AMD_IMC>.

NOTE : Cette section est obsolète et doit être nettoyée.

### Micrologiciel AMD SMU

NOTE : Cette section est obsolète et doit être nettoyée.

Il gère certaines fonctions de gestion de l'alimentation pour les périphériques PCIe (sans lui, votre portable
ne fonctionnerait pas correctement) ainsi que plusieurs autres fonctions
liées à la gestion de l'alimentation.

Le micrologiciel est signé, bien que sur le matériel AMD plus ancien il s'agisse d'une clé
symétrique, ce qui signifie qu'avec l'accès à la clé (en cas de fuite), vous pouvez signer
votre propre version modifiée et l'exécuter. Rudolf Marek (hacker de coreboot)
a découvert comment extraire cette clé [dans cette démonstration
vidéo](https://media.ccc.de/v/31c3_-_6103_-_en_-_saal_2_-_201412272145_-_amd_x86_smu_firmware_analysis_-_rudolf_marek),
et sur la base de ce travail, Damien Zammit (un autre hacker de coreboot)
l'a [partiellement remplacé](https://github.com/zamaudio/smutool/) par un micrologiciel
libre, mais le système concerné (ASUS F2A85-M) contenait encore
d'autres blobs (Video BIOS et autres).

### Micrologiciel AMD AGESA

NOTE : Il faudrait en écrire davantage à ce sujet pour refléter la réalité actuelle.
La situation d'AMD a changé ces dernières années. Les informations de cette FAQ
sont périmées depuis plusieurs années.

Il est responsable de pratiquement toute l'initialisation du matériel principal dans
les systèmes AMD modernes. En 2011, AMD a commencé à collaborer avec le projet coreboot,
en publiant ce code en source sous licence libre. En 2014, elle
a cessé de publier le code source et a commencé à publier AGESA sous forme de
blobs binaires. Cela fait d'AGESA l'équivalent de l'[Intel FSP](#fsp).

### Mises à jour du microcode des CPU AMD

Lisez la section Intel,
c'est à peu près la même chose, bien qu'on ait constaté qu'avec du matériel
AMD bien plus récent, il est possible de fonctionner sans mises à jour du microcode. On ne sait pas si les mises à jour sont nécessaires sur toutes les
cartes AMD (cela dépend du CPU).

Le projet libreboot ne considère pas les mises à jour du microcode comme un problème, et il
les active par défaut sur tout le matériel
pris en charge.

### AMD ne coopère pas

AMD semblait être sur la bonne voie en 2011, lorsqu'elle a commencé à
collaborer et à publier le code source de plusieurs composants
critiques pour le projet coreboot. Cela n'a pas duré. Pour des raisons dites
économiques, elle a décidé qu'il ne valait plus la peine de consacrer du temps à investir
dans le projet coreboot. Malheureusement, elle n'a même pas partagé le code source
de la bibliothèque AGESA pour les architectures de la famille 15h Steamroller/Excavator
(qui, comme les fam15h Bulldozer/Piledriver précédentes, n'ont
pas de PSP) et l'a publiée pour le projet coreboot uniquement sous forme binaire.

Le passage de cette entreprise de si bonne à si mauvaise en seulement 3 ans
laisse penser que quelque chose cloche gravement chez AMD. Comme Intel, elle
ne mérite pas votre argent.

Compte tenu de l'état actuel du matériel Intel avec Management Engine, nous
considérons que tout matériel x86 de production plus récent que les CPU AMD
de la famille 15h (côté AMD) ou que tout ce qui est sorti après 2009 côté Intel
est défectueux par conception et ne peut pas être utilisé en toute sécurité pour stocker, transmettre
ou traiter des données sensibles. Les données sensibles sont toutes les données dont la
violation pourrait causer un préjudice économique important à l'entité qui les a créées
ou qui était chargée de les conserver ; cela inclut donc les banques,
les sociétés émettrices de cartes de crédit ou les détaillants (dossiers de comptes clients), en
plus des cabinets d'ingénierie et de développement logiciel « ordinaires ».
Cela s'applique aussi aux lanceurs d'alerte ou à toute personne ayant besoin d'une véritable confidentialité et
sécurité.

Salut, j'ai &lt;insérez ici un système au hasard&gt;, est-il pris en charge ?
--------------------------------------------------------------------------------------------------------

S'il est pris en charge par coreboot, vous pouvez l'ajouter immédiatement.
Lisez le [guide de portage](/docs/maintain/porting.html) pour porter une nouvelle carte.
Si vous parvenez à générer une ROM fonctionnelle pour votre système, veuillez
lire le [manuel de maintenance de lbmk](docs/maintain/) pour l'ajouter à libreboot.

Si coreboot ne prend pas en charge votre matériel, vous devrez y ajouter cette prise en charge.
Veuillez consulter le projet coreboot pour être accompagné.

Questions générales
=================

Comment installer libreboot ?
-------------------------------------------------------

Consultez le [guide d'installation](docs/install/)

Comment programmer une puce flash SPI ?
---------------------------------------------------------------------------------

Reportez-vous à :\
[Réécriture externe de mémoire flash NOR 25xx via le protocole SPI](docs/install/spi.md)

Vous pouvez utiliser une pince SOIC à 16 broches sur une puce SOIC à 8 broches si
vous alignez correctement les broches. En général, la connexion est plus solide.

Comment protéger la puce flash en écriture ?
----------------------------------------------------------------------------

Par défaut, il n'y a aucune protection en écriture sur un système libreboot. C'est
par souci de facilité d'utilisation, car la plupart des gens n'ont pas facilement accès à un
programmateur externe pour reflasher leur micrologiciel, ou trouvent
peu pratique d'utiliser un programmateur externe.

Sur certains systèmes, il est possible de protéger le micrologiciel en écriture afin qu'il soit
en lecture seule au niveau du système d'exploitation (le flashage externe reste
possible avec du matériel spécial). Par exemple, sur les portables GM45 modernes
(tels que les ThinkPad X200 et T400), il est possible d'activer la protection en écriture (voir
l'[utilitaire ICH9 gen](docs/install/ich9utils.md#ich9gen)).

La protection en écriture peut être configurée sur tous les systèmes libreboot, mais les instructions
restent à écrire. La documentation se trouve dans le dépôt git principal, vous pouvez donc
envoyer des correctifs ajoutant ces instructions.

À FAIRE : Documenter la protection de la mémoire flash basée sur PRx sur les plateformes Intel et étudier d'autres
méthodes sur les systèmes AMD.

Comment modifier les paramètres du BIOS ?
------------------------------------------------------------------------

La plupart des paramètres de libreboot utilisent en réalité la [charge utile
GRUB](http://www.coreboot.org/GRUB2). Plus d'informations sur les charges utiles
se trouvent sur
[coreboot.org/Payloads](http://www.coreboot.org/Payloads). SeaBIOS est également
disponible. La configuration *CMOS* est codée en dur dans libreboot.

Le projet libreboot hérite du concept modulaire de charge utile de coreboot, ce qui
signifie que les programmes de *configuration du BIOS* ne sont pas vraiment
pratiques. Coreboot (et libreboot) inclut un utilitaire appelé
*nvramtool*, qui peut servir à modifier certains paramètres. Vous trouverez
nvramtool dans *coreboot/util/nvramtool/*, dans les archives de code source
de libreboot.

Le paramètre *-a* de nvramtool affichera la liste des paramètres disponibles, et *-w*
permet de les modifier. Consultez la documentation de nvramtool sur le
wiki de coreboot pour plus d'informations.

En pratique, dans la plupart des cas, vous n'avez besoin de modifier aucun de ces
paramètres.

Les paramètres par défaut de libreboot verrouillent la table CMOS afin de garantir un fonctionnement cohérent
pour tous les utilisateurs. Vous pouvez utiliser :

	nvramtool -C votreROM.rom -w unparametre=unevaleur

Pour obtenir la liste complète des options disponibles, faites ceci :

	nvramtool -C votreROM.rom -a

Cela modifiera les valeurs par défaut à l'intérieur de l'image ROM, que vous pourrez ensuite
reflasher.

Comment remplir (padder) la ROM avant le flashage ?
--------------------------------------

Il est préférable d'utiliser simplement une image ROM plus grande. Cette section a été écrite surtout pour
les cartes mères ASUS KCMA-D8 et KGPE-D16, pour lesquelles nous ne fournissions auparavant dans libreboot que
des images ROM de 2 Mo, mais nous fournissons désormais des images ROM de 16 Mo.
Les autres tailles ne sont pas fournies car, en pratique, quelqu'un qui mettrait à niveau l'une de
ces puces n'en utiliserait que 16 Mo. Des tailles plus grandes existent, mais 16 Mo
est le maximum utilisable sur tous les systèmes libreboot pris en charge qui
utilisent de la mémoire flash SPI.

C'est nécessaire pour une ROM dont l'image est plus petite que la puce flash
(par exemple, écrire une ROM de 2 Mo sur une puce flash de 16 Mo).

Créez un fichier vide (octets 00) de la taille de la différence entre
la ROM et la puce flash. Dans l'exemple ci-dessus, par exemple :

	truncate -s +14MiB pad.bin

Pour les images x86 sans descripteur, il faut ajouter le remplissage *au début* de la ROM :

	cat pad.bin votrerom.rom > votrerom.rom.new

Pour ARM et pour x86 avec descripteur flash Intel, il faut l'ajouter après l'image :

	cat votrerom.rom pad.bin > votrerom.rom.new

Flashez le fichier obtenu. Notez que cbfstool ne pourra pas fonctionner
avec des images remplies de cette façon ; veillez donc à effectuer toutes les modifications de l'image, y compris
la configuration à l'exécution, avant le remplissage.

Pour retirer le remplissage, par exemple après avoir lu l'image depuis la puce flash,
utilisez simplement dd(1) pour extraire uniquement la partie non remplie. En poursuivant
les exemples ci-dessus, pour extraire une ROM x86 sans descripteur de 2 Mo à partir d'une
image remplie de 16 Mo, exécutez ce qui suit :

	dd if=flashprogread.rom of=votrerom.rom ibs=14MiB skip=1

Une fois le remplissage retiré, cbfstool pourra travailler normalement avec l'image.

Dois-je installer un chargeur d'amorçage lors de l'installation d'une distribution ?
---------------------------------------------------------------------------------------------------

La plupart des configurations de libreboot intègrent déjà le chargeur d'amorçage GRUB comme
*[charge utile](http://www.coreboot.org/Payloads)*. Cela signifie que le chargeur d'amorçage GRUB
est en fait *flashé* dans le cadre du micrologiciel de démarrage
(libreboot). Vous n'avez donc pas besoin d'installer de chargeur d'amorçage sur votre
disque dur ou SSD lors de l'installation d'une nouvelle distribution. Vous pourrez démarrer normalement
en utilisant le chargeur d'amorçage (GRUB) qui se trouve dans la puce flash.

Cela signifie que même si vous retirez votre disque dur ou SSD, vous disposerez toujours
d'un chargeur d'amorçage fonctionnel, utilisable pour démarrer le programme d'installation
d'une distribution depuis une clé USB. Voir
[Comment installer Linux sur un système libreboot](../docs/linux/grub_boot_installer.md)

D'autres charges utiles sont également proposées à l'heure actuelle. Si vous utilisez la charge utile SeaBIOS,
alors un secteur d'amorçage MBR normal est utilisé sur votre disque dur ou SSD, comme on pouvait s'y
attendre. Les paragraphes ci-dessus ne concernent donc que la charge utile GRUB.

Dois-je reflasher quand je réinstalle une distribution ?
-------------------------------------------------------------------------------------------

Plus maintenant. Les versions récentes de libreboot (avec la charge utile GRUB)
basculeront automatiquement vers la configuration GRUB présente sur le disque dur ou le SSD, si
elle existe. Vous pouvez aussi charger une autre configuration GRUB depuis n'importe quel périphérique pris en charge par
GRUB (par exemple une clé USB). Pour
plus d'informations, voir
[Modifier la configuration de GRUB sur les systèmes libreboot](../docs/linux/grub_cbfs.md)

Si vous utilisez la charge utile SeaBIOS, c'est encore plus simple. Cela fonctionne comme vous
vous y attendez. SeaBIOS implémente l'interface BIOS x86 habituelle.

À quoi ressemble une puce flash ?
-----------------------------------------------------------------

Vous trouverez des photos de différents types de puces sur la page suivante :\
[Guide du flashage externe de mémoire NOR 25xx](docs/install/spi.md)

Impossible d'exécuter modprobe thinkpad\_acpi sur Haswell
===============================================

Ce problème a été signalé par un utilisateur de Debian 11 avec le
noyau `5.19.0-0.deb11.2-amd64`. Le module `thinkpad_acpi` ne se chargeait pas,
avec ce message :

```
modprobe: ERROR: could not insert 'thinkpad_acpi': "No such device"
```

Les informations sur la batterie dans `/sys` étaient absentes pour cette raison. L'utilisateur a indiqué que
le contournement suivant était efficace (sous Debian).

Ajoutez cette ligne à `/etc/modprobe.d/thinkpad_acpi.conf` :

```
options thinkpad_acpi force_load=1
```

tlp
---

Vous pouvez installer le paquet `tlp` et démarrer ce service. Par exemple, sous
Debian :

```
apt-get install tlp tlp-rdw
systemctl enable tlp
systemctl start tlp
```

Lisez maintenant la documentation :

```
man tlp-stat
```

En tant que root, vous pouvez exécuter :

```
tlp-stat -b
```

Cela fournira des informations sur la batterie.

Quels autres micrologiciels existent en dehors de libreboot ?
==================================================

Vous pouvez aussi lire des informations à leur sujet dans la [politique de minimisation des blobs
binaires de libreboot](news/policy.uk.md), qui en détaille plusieurs
plus en profondeur.

### Cartes graphiques externes

Un BIOS vidéo est présent sur la plupart des cartes graphiques. Pour les graphiques intégrés,
le VBIOS (un type particulier d'OptionROM) est généralement intégré
au micrologiciel de démarrage principal. Pour une carte graphique externe, le VBIOS se trouve généralement
sur la carte graphique elle-même. Il est généralement propriétaire ; la seule
différence est que SeaBIOS peut l'exécuter (sinon, vous l'intégrez
à l'image ROM de coreboot et coreboot l'exécute si vous avez
une autre charge utile, comme GRUB).

*Le projet coreboot* fournit du code d'initialisation libre sur de nombreuses cartes, et
libreboot utilisera ce code lorsqu'il est disponible, selon la configuration.

Dans les configurations où SeaBIOS et l'initialisation GPU native sont utilisés ensemble,
une couche d'adaptation VBIOS spéciale est ajoutée, qui utilise le framebuffer linéaire de coreboot.

### Micrologiciel de l'EC (contrôleur embarqué)

On le trouve sur la plupart des ordinateurs portables (tous ?). L'EC (contrôleur embarqué) est un petit
processeur distinct qui gère principalement les entrées/sorties propres
aux portables. Par exemple :

-   Lorsque vous appuyez sur l'interrupteur radio, l'EC active/désactive
    les périphériques sans fil (wifi, bluetooth, etc.) et allume/éteint la
    LED qui indique si c'est activé ou non.
-   Il écoute une autre puce qui effectue des mesures de température, et ajuste
    la vitesse du ventilateur en conséquence (ou l'allume/l'éteint).
-   Il reçoit certaines entrées du clavier, par ex. augmenter/diminuer la luminosité,
    augmenter/diminuer le volume.
-   Il détecte l'ouverture ou la fermeture du capot et envoie un signal
    l'indiquant.
-   Etc.

L'EC est présent sur presque tous les ordinateurs portables. D'autres appareils utilisent, selon leur complexité,
soit un EC, soit une variante à micrologiciel en Mask ROM : le SuperIO.

### Micrologiciel des disques durs/SSD

Les disques durs et les SSD contiennent un micrologiciel embarqué
chargé de gérer le fonctionnement interne du périphérique tout en exposant une interface simple et standard (comme
AHCI/SATA) que le logiciel du système d'exploitation peut généralement utiliser. Ce micrologiciel est invisible
pour l'utilisateur du lecteur.

Les disques durs et les SSD sont assez complexes et contiennent aujourd'hui du matériel
suffisamment sophistiqué pour exécuter un système d'exploitation entier (c'est-à-dire que le disque lui-même est capable d'exécuter son propre
système d'exploitation embarqué), voire
Linux ou BusyBox/Linux.

Les SSD et les disques durs sont un cas particulier, car ce sont des périphériques de stockage persistant
tout en étant aussi des ordinateurs.

Exemple d'attaque qu'un logiciel malveillant pourrait mener : remplacer vos clés SSH,
ce qui permettrait à un attaquant inconnu d'obtenir un accès distant non autorisé. Ou peut-être
remplacer vos clés GPG. Les disques SATA peuvent aussi avoir du DMA (via le
contrôleur), ce qui signifie qu'ils peuvent lire la mémoire système ;
en théorie, un disque pourrait avoir son propre stockage caché dans lequel il pourrait lire vos clés
LUKS et les conserver en clair pour une extraction ultérieure
par l'attaquant.

Avec une IOMMU appropriée et l'utilisation de l'USB au lieu du SATA, il serait possible d'atténuer
tout problème lié au DMA qui pourrait survenir.

Quelques preuves de concept ont été démontrées. Pour les disques durs :
<https://spritesmods.com/?art=hddhack&page=1> Pour les SSD :
<http://www.bunniestudios.com/blog/?p=3554>

On ne connaît actuellement aucun remplacement libre viable du micrologiciel. Pour les
SSD, le projet
[OpenSSD](https://web.archive.org/web/20220425071606/http://www.openssd-project.org/wiki/The_OpenSSD_Project)
pourrait être intéressant.

Apparemment, les disques SATA n'ont pas eux-mêmes de DMA, mais peuvent l'utiliser
via le contrôleur. Voir
<http://web.archive.org/web/20170319043915/http://www.lttconn.com/res/lttconn/pdres/201005/20100521170123066.pdf>
(pages 388-414, 420-421, 427, 446-465, 492-522, 631-638) et
<http://www.intel.co.uk/content/dam/www/public/us/en/documents/technical-specifications/serial-ata-ahci-spec-rev1_3.pdf>
(pages 59, 67, 94, 99).

Ce qui suit est tiré d'une discussion avec Peter Stuge (CareBear\\) sur le
canal IRC de coreboot le vendredi 18 septembre 2015, lors de
recherches visant à déterminer si un disque SATA lui-même peut utiliser le DMA. Ce qui suit
repose sur les fiches techniques dont les liens figurent ci-dessus :

D'après ces documents, le type de FIS 39h est *« DMA Activate FIS
- Device to Host »*. Il y est question d'un *« transfert de données de l'hôte vers le
périphérique », et il est précisé : à la réception d'un DMA Activate,
si le contrôleur DMA de l'adaptateur hôte a été programmé et activé, l'adaptateur
hôte lance la transmission d'un Data FIS et transmettra dans
ce FIS les données correspondant aux régions de mémoire hôte indiquées par le
contexte du contrôleur DMA. »* FIS est une unité de protocole (Frame Information
Structure). Sur cette base, il semble que le disque puisse dire au
contrôleur hôte qu'il souhaite qu'un DMA ait lieu, mais tant que le logiciel hôte n'a pas
configuré, ou ne souhaite pas configurer à l'avenir, un transfert DMA,
rien ne se passera. **Le disque peut aussi envoyer un DMA Setup**. Si un DMA
Setup FIS est envoyé en premier, avec le bit Auto-Activate positionné, alors cela
est déjà configuré et le disque peut initier un DMA. Le document poursuit :
*« À la réception d'un DMA Setup, le destinataire du FIS doit
valider la demande de DMA Setup reçue. »* — autrement dit, l'hôte
doit vérifier ; mais il y a peut-être une faille ici. Le document dit encore :
*« L'implémentation spécifique de l'identifiant de tampon et de la validation
du tampon/de l'adresse n'est pas définie »* — donc personne ne s'en soucie
vraiment. *« le destinataire du FIS »* — dans le cas qui nous
occupe, il s'agit du matériel contrôleur hôte dans le chipset et/ou du
pilote du noyau (très probablement le pilote du noyau). Tous les périphériques SATA ont un
micrologiciel, avec une mémoire flash mise à jour,
qu'il est généralement possible de mettre à jour en exécutant un logiciel dans votre système d'exploitation ; **un logiciel malveillant exécuté en tant que root
pourrait mettre à jour ce micrologiciel, ou le micrologiciel
pourrait déjà être malveillant**. Votre disque dur ou SSD est l'endroit idéal pour qu'un attaquant
installe un logiciel malveillant, car c'est un stockage persistant
tout en étant aussi un ordinateur.

À partir de là, on peut dire sans risque qu'utiliser l'USB à la place du SATA est
judicieux si la sécurité vous préoccupe. L'USB 2.0 offre une bande passante suffisante pour
de nombreux disques durs (quelques disques haut de gamme peuvent utiliser plus de bande passante que l'USB 2.0),
mais pour les SSD cela peut poser problème (sauf si vous utilisez
l'USB 3.0, qui n'est pas encore utilisable en toute liberté. Voir

L'utilisation de l'USB n'est pas non plus une garantie de sécurité absolue ; soyez donc prudent.
La surface d'attaque devient bien plus réduite, mais un disque malveillant pourrait quand même
tenter une attaque par « fuzzing » (par exemple en envoyant des descripteurs USB
malformés, ce qui a permis de briser la tyrannie DRM de la PlayStation 3,
afin que les utilisateurs puissent exécuter leur propre système d'exploitation et du code
non signé). (Vous êtes probablement en sécurité, sauf s'il existe une faille de sécurité
dans la bibliothèque/le pilote USB utilisé par votre système d'exploitation. L'USB est généralement considéré comme
l'un des protocoles les plus sûrs, précisément parce que les périphériques USB n'ont pas de DMA.)

Autres liens :

-   <https://www.vice.com/en_us/article/ypwkwk/the-nsas-undetectable-hard-drive-hack-was-first-demonstrated-a-year-ago>

Il est recommandé d'utiliser le chiffrement complet du disque sur les disques durs
connectés par USB. Il existe en ligne plusieurs adaptateurs permettant de connecter des disques durs
SATA via USB, et le projet Libreboot est capable de démarrer depuis ceux-ci
de la manière habituelle. Consultez la documentation de votre système d'exploitation Linux/BSD
pour savoir comment l'installer avec un *chiffrement complet du disque* :


La théorie actuelle (non prouvée) est que cela empêcherait au moins
un disque malveillant de manipuler correctement les données lues depuis le disque
ou écrites sur celui-ci, puisqu'il ne peut pas accéder à votre clé LUKS
si elle n'est présente qu'en RAM, à condition que le disque dur n'ait pas de DMA (les périphériques USB n'ont
pas de DMA). Le pire qu'il puisse faire dans ce cas est de détruire vos données.
Bien sûr, vous devez veiller à ne jamais placer de fichiers de clé dans l'en-tête LUKS.
**Prenez le contenu de ce paragraphe avec des pincettes. Cela fait encore l'objet de discussions
et rien de tout cela n'est prouvé.**

### NIC (contrôleur Ethernet)

Les cartes réseau Ethernet exécutent généralement un micrologiciel embarqué chargé
de l'initialisation interne du périphérique. En théorie, il pourrait être configuré
pour abandonner des paquets, voire les modifier.

Avec une IOMMU appropriée, il serait possible d'atténuer les problèmes liés au DMA.
Il est également possible d'utiliser un adaptateur réseau USB, qui n'a pas de DMA.

### Microcode du processeur

Le microcode configure les réseaux de portes logiques du microprocesseur afin d'implémenter
l'architecture du jeu d'instructions. Des *décodeurs* spéciaux dans le microprocesseur configurent
les circuits en fonction de ce microcode.

La [politique de réduction des blobs de libreboot](news/policy.uk.md) décrit en détail le
microcode.

### Carte son

Le matériel audio (intégré ou dédié) possède généralement un micrologiciel embarqué
(DSP) pour gérer les entrées/sorties. Là encore, un DAC USB est un bon
contournement.

### Webcam

Les webcams ont un micrologiciel embarqué qui traite l'image captée
par la caméra : mise au point, balance des blancs, etc. Il est possible d'utiliser du matériel
de webcam USB pour résoudre d'éventuels problèmes de DMA ; les webcams intégrées
(par exemple sur les portables) sont déconseillées par le projet libreboot pour
des raisons de sécurité.

### Contrôleur hôte USB

Les contrôleurs hôtes USB nécessitent un micrologiciel. Il faut parfois le fournir
dans coreboot lui-même.

### Micrologiciel WWAN

Certains portables peuvent disposer d'un lecteur de carte SIM avec une carte WWAN,
pour se connecter à un réseau 3G/4G (par exemple GSM). C'est la
même technologie que celle utilisée dans les téléphones mobiles pour l'accès réseau à distance (par exemple
Internet).

NOTE : à ne pas confondre avec le wifi. Le wifi est une autre technologie qui n'a
absolument aucun rapport.

Le processeur de bande de base à l'intérieur de la puce WWAN aura son propre système
d'exploitation embarqué, très probablement propriétaire. L'utilisation de cette technologie
entraîne aussi les mêmes problèmes de confidentialité que pour les téléphones mobiles (suivi à distance
via le réseau GSM, par triangulation du signal).

Sur certains portables, ces cartes utilisent l'USB (en interne) et n'ont donc pas de DMA,
mais cela reste un gros problème de liberté et de vie privée. Si vous avez une
puce/carte WWAN interne, le projet libreboot recommande de la désactiver et
(idéalement, si possible) de retirer physiquement le matériel. Si vous avez absolument
besoin d'utiliser cette technologie, un adaptateur USB externe est bien
préférable, car il peut être facilement retiré lorsque vous n'en avez pas besoin, empêchant ainsi
toute entité extérieure de suivre votre position.

Il est recommandé d'utiliser l'Ethernet ou le wifi plutôt que les réseaux mobiles,
car ils sont généralement bien plus sûrs.

Systèmes d'exploitation
=================

Puis-je utiliser Linux ?
--------------------------------------------------

Absolument ! Il est bien testé dans libreboot et fortement recommandé. Voir
[installation de Linux](../docs/linux/grub_boot_installer.md) et
[exécution de Linux](../docs/linux/grub_cbfs.md).

Toute distribution moderne devrait fonctionner, tant qu'elle utilise le KMS (kernel mode
setting) pour les graphismes.

Fedora ne démarre pas ? (peut aussi s'appliquer à Redhat/CentOS)
-----------------------------------------------------------

Sous Fedora, le grub.cfg par défaut tente de démarrer Linux en mode 16 bits.
Il vous suffit de modifier la configuration GRUB de Fedora.
Reportez-vous à la [page Linux](docs/linux/).

Puis-je utiliser BSD ?
----------------------------------

Absolument ! Le micrologiciel libreboot offre une bonne prise en charge de FreeBSD, NetBSD et
OpenBSD. Les autres systèmes n'ont pas été testés mais devraient fonctionner normalement.

Voir :
[docs/bsd/](docs/bsd/index.uk.md)

Windows ??
---------

Oui, vous pouvez utiliser Windows 10 et 11. Ils ne sont pas officiellement pris en charge et le
projet Libreboot vous recommande de *l'éviter*, car Windows est un
logiciel propriétaire. Cela signifie que vous n'avez *pas* la liberté
d'utiliser, d'étudier, d'adapter et de partager le logiciel de manière raisonnable. Vous êtes
entièrement à la merci de Microsoft, dont le mobile est le profit, et non
le progrès général de l'informatique ni le bien-être de tous.

Voir : [Qu'est-ce que le logiciel libre ?](https://writefreesoftware.org/learn)

En plus d'être un logiciel propriétaire, Windows est connu pour être truffé de bogues,
y compris de *portes dérobées*. Lorsque vous *utilisez* Windows, il envoie des informations vous
concernant à des tiers, utilisées à diverses fins comme la publicité, mais
il est tout à fait possible que des agences à trois lettres reçoivent aussi vos données si
vous utilisez Windows.

Pour votre liberté, vous ne devriez jamais, au grand jamais, utiliser Windows. Utilisez Linux ou
les systèmes BSD, qui sont bien pris en charge. Quoi qu'il en soit :

[Vidéo de Windows 10 en cours de démarrage](https://yewtu.be/watch?v=BWq6XnWKQnM)

[Vidéo de Windows 11 en cours de démarrage](https://yewtu.be/watch?v=OFHiMfVNNeA)

À noter : Windows 11 exige officiellement la prise en charge d'au moins TPM 1.2 et
l'utilisation d'UEFI SecureBoot. Pour contourner cette exigence, MajorGeeks propose
un guide, voir : <https://www.majorgeeks.com/content/page/bypass_tpm.html>

La personne qui a effectué ce test a également indiqué qu'elle n'avait pas pu réaliser une
mise à niveau sur place de 10 vers 11 ; elle a donc dû effacer le disque et effectuer une
installation propre (note : pas si propre, car Windows est plein de logiciels espions de la NSA).

Dans les deux cas, comme indiqué ci-dessus, le système d'exploitation Windows démarrait depuis
SeaBIOS, avec le framebuffer de coreboot initialisé au démarrage, sur un GPU Intel
initialisé via *libgfxinit* de coreboot, avec les versions de Libreboot de novembre 2023.
Nous ne prenons pas encore en charge le démarrage en UEFI sur les machines x86.

D'autres systèmes d'exploitation sont-ils pris en charge ?
-------------------------------------------------------------------

On ne sait pas. C'est possible, mais impossible à dire sans tests supplémentaires.

Quel niveau de liberté logicielle libreboot m'apporte-t-il ?
===================================================

Veuillez lire la [politique de minimisation des blobs binaires de libreboot](news/policy.uk.md).

Le micrologiciel libreboot fournit l'initialisation du matériel hôte à l'intérieur de fichiers ROM
pouvant être écrits sur la mémoire flash NOR, mais sur de nombreux systèmes il existe
bien d'autres petits ordinateurs sur la carte mère qui exécutent des micrologiciels binaires.
Certains d'entre eux ne peuvent pas être remplacés car ils se trouvent en Mask ROM.
Par exemple, la plupart des portables ont un micrologiciel EC (contrôleur embarqué).

Au-delà du logiciel lui-même (embarqué en ROM ou non), la plupart du matériel
(des puces aux circuits imprimés) n'est pas publié sous des licences libres.
Nous n'avons aucun appareil que l'on puisse considérer comme « 100 % libre »,
et de tels absolus sont presque impossibles à atteindre.

Blobs propriétaires connus (liste non exhaustive) :

* Tous les appareils
	* Micrologiciel des disques durs SATA/PATA/lecteurs optiques
  ([a souvent un puissant ordinateur basé sur ARM](
  http://spritesmods.com/?art=hddhack&page=1))
	* Clés USB et tous les périphériques USB — ils contiennent un ordinateur
  exécutant du code, au minimum pour gérer le protocole USB
* ThinkPad :
	* Micrologiciel de l'EC (H8S jusqu'à Sandy Bridge inclus, puis basé sur ARC MEC16xx)
	* Micrologiciel du TrackPoint (8051)
	* Les appareils Penabled contiennent un microcontrôleur µPD78F0514 sur la carte fille wacom,
  et un Atmega (AVR) sur la carte LED/boutons
	* BMS de la batterie, bq8030 (CoolRISC C816)
* Chromebooks C201PA/C100PA :
	* BMS de la batterie, bq30z55
	* Pavé tactile Elan
	* Micrologiciel du [contrôleur de mémoire flash](https://fr.wikipedia.org/wiki/M%C3%A9moire_flash) eMMC

Un jour, nous vivrons dans un monde où n'importe qui pourra fabriquer ses propres puces, y compris
des processeurs, ainsi que tout autre type de circuit intégré. Les efforts pour rendre la fabrication de puces à domicile réalité
n'en sont qu'à leurs balbutiements, mais ils
existent, par exemple le travail de Sam Zeloof et du projet Libre Silicon :

* <https://www.youtube.com/channel/UC7E8-0Ou69hwScPW1_fQApA>
* <http://sam.zeloof.xyz/>
* <https://libresilicon.com/>

(Sam fabrique littéralement des processeurs dans son garage)

Où en apprendre davantage sur l'électronique
==========================================

* Bases de la soudure et du reworking par PACE
    Les deux séries de vidéos sont à voir absolument, quel que soit votre niveau de soudure.
    * [Soudure de base](https://yewtu.be/playlist?list=PL926EC0F1F93C1837)
    * [Reworking et réparation](https://yewtu.be/playlist?list=PL958FF32927823D12)
    Les séries de PACE ci-dessus couvrent les techniques classiques, mais pas
    l'électronique *moderne*. Pour cela, voir :
    * [Leçons de microsoudure d'iFixit avec Jessa
	Jones](https://yewtu.be/playlist?list=PL4INaL5vWobD_CltiZXr7K46oJ33KvwBt)
    * Consultez aussi les liens YouTube ci-dessous, en particulier les vidéos de Louis Rossmann, pour en apprendre
      (beaucoup) plus.
* [Cours edX sur les bases de l'électronique](https://www.edx.org/course/circuits-and-electronics-1-basic-circuit-analysi-2)
    Dans la plupart des pays, le contenu de ce cours est abordé au collège et au lycée.
    Il vous sera aussi utile pour rafraîchir votre mémoire
    si vous n'avez pas utilisé ces connaissances depuis.
* Introduction à l'impédance
    * [Similitudes dans le comportement des ondes](https://yewtu.be/watch?v=DovunOxlY1k)
    * [Réflexions dans une ligne de transmission](https://yewtu.be/watch?v=y8GMH7vMAsQ)
    * Stubs :
        * [Article de Wikipédia sur les stubs](https://en.wikipedia.org/wiki/Stub_(electronics))
        * [Article de Polar Instruments sur les stubs](http://www.polarinstruments.com/support/si/AP8166.html)
        Avec le flashage externe SPI, seuls les stubs de circuits imprimés imprévus nous intéressent
* [Comment mesurer précisément le pas d'un connecteur](https://www.microcontrollertips.com/accurately-measure-headerconnector-pitch/)
* Autres chaînes YouTube proposant du contenu utile sur l'électronique
    * [EEVblog](https://yewtu.be/channel/UC2DjFE7Xf11URZqWBigcVOQ)
	(électronique en général, tests de matériel, etc., quelques
	vidéos de réparation)
    * [Louis Rossmann](https://yewtu.be/channel/UCl2mFZoRqjw_ELax4Yisf6w)
	(défense du droit à la réparation, nombreuses vidéos de réparation de MacBook)
    * [mikeselectricstuff](https://yewtu.be/channel/UCcs0ZkP_as4PpHDhFcmCHyA)
    * [bigclive](https://yewtu.be/channel/UCtM5z2gkrGRuWd0JQMx76qA)
    * [ElectroBOOM](https://yewtu.be/channel/UCJ0-OtVpF0wOKEqT2Z1HEtA)
	(il fait exploser des choses et montre comment ne pas faire)
    * [Jeri Ellsworth](https://yewtu.be/user/jeriellsworth/playlists)
	(a des vidéos montrant comment fabriquer soi-même un *transistor*)
    * [Sam Zeloof](https://yewtu.be/channel/UC7E8-0Ou69hwScPW1_fQApA)
	(Sam fabrique littéralement des processeurs dans son garage, inspiré par le travail de Jeri Ellsworth
	sur les transistors)
    * [Ben Eater](https://eater.net/) (montre comment construire un CPU 8 bits à partir de zéro,
	fait aussi des choses avec le MOS 6502)
	(montre aussi comment faire d'autres choses, comme des puces graphiques, et enseigne des
	concepts réseau) — regardez les vidéos de Ben ! <https://redirect.invidious.io/beneater>
    * [iPad Rehab with Jessa Jones](https://yewtu.be/channel/UCPjp41qeXe1o_lp1US9TpWA)
	(soudure de très haute précision. Elle répare des téléphones mobiles et appareils similaires, ce qui
	est aussi couvert dans la série d'iFixit sur la réparation au niveau des composants)
* Les fichiers Boardview peuvent être ouverts dans [OpenBoardview](https://github.com/OpenBoardView/OpenBoardView),
qui est un logiciel libre sous licence MIT.

L'utilisation de `yt-dlp` (un fork amélioré de `youtube-dl`) est recommandée pour les liens
`youtube.com`. Voir : <https://github.com/yt-dlp/yt-dlp>

Enfin, le message le plus important pour quiconque se lance dans ce merveilleux nouveau passe-temps : [Le secret pour apprendre l'électronique](https://yewtu.be/watch?v=xhQ7d3BK3KQ)