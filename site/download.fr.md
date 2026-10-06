---
title: Téléchargement
x-toc-enable: true
...

Les nouvelles versions sont annoncées dans la [section principale des actualités](news/).

Si vous vous intéressez davantage au développement de Libreboot, rendez-vous sur la
[page de développement de Libreboot](../git.md), qui comprend également des liens vers les
dépôts Git. La page [/docs/maintain/](docs/maintain/) décrit la manière dont
Libreboot est assemblé et comment le maintenir. Si vous souhaitez compiler
Libreboot à partir du code source, [lisez cette page](docs/build/).

Acheter Libreboot préinstallé
--------------

Si vous souhaitez une installation professionnelle, Minifree Ltd vend
[Libreboot préinstallé](https://minifree.org/) sur du matériel sélectionné, et propose aussi
un [service de préinstallation de Libreboot](https://minifree.org/product/installation-service/)
si vous voulez envoyer votre machine pour qu'on y installe Libreboot à votre place.

Leah Rowe, la fondatrice et développeuse principale de Libreboot, est aussi propriétaire
et gérante de Minifree Ltd ; les ventes financent le projet Libreboot.

Avertissement de sécurité
--------------

**[VEUILLEZ LIRE CES INSTRUCTIONS AVANT D'INSTALLER](docs/install/ivy_has_common.md), SINON
VOUS RISQUEZ DE BRIQUER VOTRE MACHINE !! - Cliquez sur le lien et suivez les instructions
qui s'y trouvent avant de flasher. Pour mémoire,
[voici à nouveau le lien](docs/install/ivy_has_common.md).**

Clés de signature GPG
---------------------

**La dernière version est Libreboot 26.01, dans le répertoire `stable`.**

### NOUVELLE CLÉ

Empreinte complète de la clé : `8BB1 F7D2 8CF7 696D BF4F  7192 5C65 4067 D383 B1FF`

La clé ci-dessus est valable pour Libreboot 20240126 et les versions suivantes. Cette clé
s'applique à toute version publiée à partir du 28 décembre 2023. Elle
expirera le 26 décembre 2028.

Téléchargez la clé ici : [lbkey.asc](lbkey.asc)

Les versions de Libreboot sont signées avec GPG.

### ANCIENNE CLÉ

Empreinte complète de la clé : `98CC DDF8 E560 47F4 75C0  44BD D0C6 2464 FA8B 4856`

Cette clé concerne les versions de Libreboot *postérieures* à la version 20160907.
Cette clé a *expiré* en décembre 2023 ; vous devez donc utiliser la clé *plus récente*
(voir ci-dessus) pour les versions postérieures à Libreboot 20240126.

Téléchargez la clé ici : [lbkey.asc](lbkeyold.asc)

Les versions de Libreboot sont signées avec GPG.

### TRÈS ANCIENNE CLÉ :

Cette clé concerne Libreboot 20160907 et toutes les versions plus anciennes :

Empreinte complète de la clé : CDC9 CAE3 2CB4 B7FC 84FD  C804 969A 9795 05E8 C5B2

La clé GPG peut également être téléchargée avec cet export de la
clé publique : [lbkeyold.asc](lbkeyoldold.asc).

	sha512sum -c sha512sum.txt
	gpg --verify sha512sum.txt.sig

Dépôt Git
--------------

Les liens vers les archives des versions régulières sont indiqués sur cette page.

Cependant, pour obtenir la toute dernière version de Libreboot,
il existe des dépôts Git depuis lesquels vous pouvez la télécharger. Allez ici :

[Comment télécharger Libreboot via Git](git.md)

Miroirs HTTPS {#https}
-------------

**La dernière version est Libreboot 26.01, dans le répertoire `stable`.**

Ces miroirs sont recommandés, car ils utilisent le chiffrement TLS (https://).

Vous pouvez télécharger Libreboot depuis ces miroirs :

* <https://rsync.libreboot.org/> (miroir officiel de Libreboot.org, Royaume-Uni)
* <https://mirror.math.princeton.edu/pub/libreboot/> (Université de Princeton,
États-Unis)
* <https://mirror.shapovalov.website/libreboot/> (shapovalov.website, Ukraine)
* <https://www.mirrorservice.org/sites/libreboot.org/release/> (Université du
Kent, Royaume-Uni)
* <https://mirrors.mit.edu/libreboot/> (Université MIT, États-Unis)
* <https://mirror.koddos.net/libreboot/> (koddos.net, Pays-Bas)
* <https://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)
* <https://mirror.cyberbits.eu/libreboot/> (cyberbits.eu, France)
* <https://mirror.diyarciftci.xyz/libreboot/> (diyarciftci.xyz, Allemagne)
* <http://5gxsihf4vjdhimiotrhchihrfujoanondrplizofwparrtmgbwsafhid.onion/libreboot/> (miroir caché de diyarciftci.xyz)
* <http://iue4ubnjvskey5gop7n3odkylyjvelb5sl7z4cx6xjwdnthv2hba.b32.i2p/libreboot/> (miroir caché de diyarciftci.xyz)
* <https://ca.mirrors.cicku.me/libreboot/> (cicku, Canada)
* <https://au.mirrors.cicku.me/libreboot/> (cicku, Australie)
* <https://in.mirrors.cicku.me/libreboot/> (cicku, Inde)
* <https://za.mirrors.cicku.me/libreboot/> (cicku, Afrique du Sud)

NOTE : les miroirs « cachés » sont chiffrés du fait du réseau sur lequel ils se trouvent,
c'est pourquoi ils figurent dans la section HTTPS (chiffré), bien qu'ils utilisent
en réalité du HTTP simple, car c'est i2p et/ou Tor qui assurent le chiffrement ; sur ces
réseaux, il est normal d'utiliser du HTTP simple.

Miroirs RSYNC {#rsync}
-------------

Les miroirs rsync suivants sont accessibles publiquement :

* <rsync://rsync.mirrorservice.org/libreboot.org/release/> (Université du Kent,
Royaume-Uni)
* <rsync://mirror.math.princeton.edu/pub/libreboot/> (Université de Princeton, États-Unis)
* <rsync://rsync.shapovalov.website/libreboot/> (shapovalov.website, Ukraine)
* <rsync://ftp.linux.ro/libreboot/> (linux.ro, Roumanie)
* <rsync://mirror.koddos.net/libreboot/> (koddos.net, Pays-Bas)
* <rsync://mirror-hk.koddos.net/libreboot/> (koddos.net, Hong Kong)
* <rsync://mirror.diyarciftci.xyz/libreboot/> (diyarciftci.xyz, Allemagne)
* <rsync://rsync.libreboot.org/mirrormirror/> (Libreboot.org)

Vous hébergez un miroir ? Contactez le projet Libreboot et le lien sera
ajouté à cette page !

Vous pouvez rendre votre miroir rsync accessible via votre serveur web, et également configurer
votre *propre* miroir pour qu'il soit accessible via rsync. De nombreuses ressources en ligne
expliquent comment configurer un serveur rsync.

Comment créer votre propre miroir rsync :

C'est utile pour répliquer l'ensemble des archives des versions de Libreboot. Vous pouvez placer
la commande rsync dans une crontab et récupérer les fichiers dans un répertoire de
votre serveur web.

Si vous comptez répliquer l'ensemble, il est recommandé de prévoir
au moins 25 Go. Le rsync de Libreboot pèse actuellement environ 12 Go ; en prévoir 25 Go
vous laisse donc beaucoup de marge pour l'avenir. Au minimum, assurez-vous que
15 à 20 Go d'espace sont disponibles pour votre miroir Libreboot.

*Il est vivement recommandé d'utiliser le miroir libreboot.org* si vous souhaitez
héberger un miroir officiel. Sinon, si vous voulez simplement créer votre propre miroir
local, vous devriez utiliser l'un des autres miroirs, qui se synchronisent avec
libreboot.org.

Avant de créer le miroir, créez un répertoire sur votre serveur web. Par
exemple :

	mkdir /var/www/html/libreboot/

Vous pouvez maintenant exécuter rsync, par exemple :

	rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

Vous pourriez le placer dans une crontab horaire. Par exemple :

	crontab -e

Puis, dans la crontab, ajoutez cette ligne et enregistrez/quittez (crontab horaire) :

	0 * * * * rsync -avz --delete-after rsync://rsync.libreboot.org/mirrormirror/ /var/www/html/libreboot/

**Il est extrêmement important de mettre une barre oblique (/) à la fin de chaque chemin
dans la commande rsync ci-dessus. Sinon, rsync se comportera de façon très étrange.**

Si vous souhaitez maintenir régulièrement vos miroirs rsync à jour, vous pouvez l'ajouter à
la crontab. Cette page explique comment utiliser crontab :
<https://man7.org/linux/man-pages/man5/crontab.5.html>

Miroirs HTTP {#http}
------------

**La dernière version est Libreboot 26.01, dans le répertoire `stable`.**

ATTENTION : ces miroirs ne sont pas en HTTPS, ce qui signifie qu'ils sont
non chiffrés. Votre trafic peut faire l'objet d'une interception
par des adversaires. Veillez tout particulièrement à vérifier les signatures GPG, en supposant que
vous disposez de la bonne clé. Bien sûr, vous devriez le faire de toute façon, même
en utilisant HTTPS.

* <http://mirror.linux.ro/libreboot/> (linux.ro, Roumanie)
* <http://mirror.helium.in-berlin.de/libreboot/> (in-berlin.de, Allemagne)

Miroirs FTP {#ftp}
-----------

**La dernière version est Libreboot 26.01, dans le répertoire `stable`.**

ATTENTION : FTP est lui aussi non chiffré, comme HTTP. Les mêmes risques s'appliquent.

* <ftp://ftp.mirrorservice.org/sites/libreboot.org/release/> (Université du
Kent, Royaume-Uni)
* <ftp://ftp.linux.ro/libreboot/> (linux.ro, Roumanie)

Liés statiquement
------------------

Libreboot inclut, dans certaines versions, des exécutables liés statiquement, compilés à partir du
code source disponible. Ces exécutables embarquent certaines bibliothèques
afin de fonctionner sur de nombreuses distributions Linux.

Pour respecter la GPL v2, des ISO de sources sont fournies
par le projet Libreboot. Vous pouvez trouver ces ISO de sources dans le répertoire `ccsource`
des miroirs `rsync`.

Les versions antérieures, jusqu'à Libreboot 20160907, ne distribuent pas de binaires
liés statiquement. Ces versions ne contiennent que du code source, hormis les images
ROM précompilées, pour lesquelles les archives de code source habituelles de Libreboot suffisent. Les versions plus récentes
automatisent à la place l'installation des dépendances de compilation, avec des instructions
dans la documentation pour compiler les différents utilitaires à partir du code source.

Ces exécutables sont des utilitaires comme `flashprog`.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/download.uk.html>