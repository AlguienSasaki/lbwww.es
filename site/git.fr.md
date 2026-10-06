---
title: Comment utiliser Git et/ou envoyer des patchs à Libreboot
x-toc-enable: true
...

EN-TÊTES SPDX
-------------

Lorsque vous soumettez de nouvelles pages de documentation (par exemple des fichiers
Markdown), assurez-vous qu'elles sont explicitement sous licence. Vous pouvez le faire en ajoutant un en-tête
SPDX au bas du fichier.

Regardez les autres pages du dépôt pour voir comment cela se fait.

`GFDL-1.3-or-later` est supposée, car c'est la licence que la majeure partie du site utilise.
Vous pouvez l'ajouter sous forme de commentaire dans les fichiers Markdown. Les pages existantes
montrent comment faire (regardez le bas d'un fichier Markdown existant).

Une mention générique indiquant la GFDL est également placée dans les fichiers
`.include` de pied de page.

En cas de doute, placez simplement ceci au bas de votre nouveau fichier Markdown
lorsque vous le soumettez pour relecture :

```
[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
```

Si vous travaillez sur un fichier Markdown déjà présent dans le dépôt du
site web, cela y figure probablement déjà.

Dépôts de Libreboot
-------------------

Le projet `libreboot` possède 3 dépôts Git principaux :

* Système de compilation : <https://codeberg.org/libreboot/lbmk>
* Site web (+ docs) : <https://codeberg.org/libreboot/lbwww>
* Images (pour le site web) : <https://codeberg.org/libreboot/lbwww-img>
* Pico-serprog : <https://codeberg.org/libreboot/pico-serprog>
* Générateur de site statique de Libreboot : <https://codeberg.org/libreboot/lbssg>

Miroirs de lbmk.git
-------------------

Le dépôt `lbmk` contient le système de compilation automatisé de Libreboot, qui
produit les versions de Libreboot (y compris les images ROM compilées).

Vous pouvez exécuter `git clone` sur n'importe lequel de ces liens (les liens sont aussi cliquables,
pour consulter les modifications dans votre navigateur web) :

* <https://git.sr.ht/~libreboot/lbmk>
* <https://git.disroot.org/libreboot/lbmk>
* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://rocketgit.com/libreboot/libreboot>

<!-- will re-add 0xacab later * <https://0xacab.org/libreboot/lbmk/> -->

Miroirs de pico-serprog.git
---------------------------

### Miroir de lbwww.git

Le dépôt `lbwww` contient des fichiers Markdown (variante pandoc), destinés
au [générateur de site statique de Libreboot](docs/sitegen/) ; c'est
ce que Libreboot utilise pour fournir ses pages web HTML, *y compris* la page que
vous êtes en train de lire !

Vous pouvez exécuter `git clone` sur ces liens, et/ou cliquer pour consulter les modifications dans votre
navigateur web. Voir :

* <https://git.sr.ht/~libreboot/lbwww>
* <https://git.disroot.org/libreboot/lbwww>
* <https://gitea.treehouse.systems/libreboot/lbwww>
* <https://rocketgit.com/libreboot/lbwww>

NOTE : le dépôt `lbwww-img` n'est généralement pas fourni sur les miroirs, car
il ne contient que des fichiers image que vous pouvez trouver sur <https://av.libreboot.org>
et le projet Libreboot n'a pas l'intention d'alourdir le trafic des *miroirs*
en y hébergeant des images.


Plus d'informations
-------------------

Voir aussi : [Générateur de site statique de Libreboot](docs/sitegen/).

La page des [tâches](tasks/) liste certaines (pas toutes) des choses que nous souhaitons
implémenter, si elles ne le sont pas déjà. Les patchs sont les bienvenus !

Si les dépôts Git principaux sont hors service, des miroirs de `lbmk` et `lbwww` sont listés
plus bas dans cette page.

Libreboot utilisait auparavant NotABug, mais celui-ci souffrait de problèmes
de fiabilité persistants, avec des erreurs HTTP 500 renvoyées, surtout en soirée, très
probablement parce qu'il y avait trop de monde dessus ; il a été décidé que Libreboot
avait besoin de quelque chose de plus stable, et Libreboot est donc maintenant hébergé sur Codeberg. Voir :
[annonce du passage à Codeberg, 8 avril 2023](news/codeberg.md)

Il existe aussi ces programmes, hébergés par le projet Libreboot, que Libreboot
recommande ou utilise :

Vous pouvez télécharger n'importe lequel de ces dépôts, y apporter les modifications que vous souhaitez, puis
soumettre vos modifications en suivant les instructions ci-dessous.

Il est recommandé de compiler Libreboot (toutes ses parties) sous une distribution
Linux. Par exemple, le système de compilation (lbmk) n'est pas testé sur les systèmes BSD.
Installez `git` sur votre système Linux, et téléchargez l'un des dépôts.

Le développement de Libreboot se fait avec le système de gestion de versions Git.
Consultez la [documentation officielle de Git](https://git-scm.com/doc) si vous ne
savez pas comment utiliser Git.

Le dépôt `bucts` est hébergé par le projet Libreboot, car le dépôt original
sur `stuge.se` n'est plus disponible, la dernière fois que nous avons vérifié. Le
programme `bucts` a été écrit par Peter Stuge. Vous avez besoin de `bucts` si vous flashez
en interne une ROM Libreboot sur un ThinkPad X60 ou T60 qui exécute actuellement
le BIOS Lenovo d'origine. Les instructions pour cela sont disponibles ici :\
[guides d'installation de Libreboot](docs/install/)

### lbmk (libreboot-make)

C'est le système de compilation principal de Libreboot. On pourrait dire que `lbmk` *est*
Libreboot ! Téléchargez le dépôt Git :

	git clone https://codeberg.org/libreboot/lbmk

La commande `git` ci-dessus télécharge le système de compilation de Libreboot, `lbmk`.
Vous pouvez ensuite vous y placer ainsi :

	cd lbmk

Apportez les modifications que vous voulez, ou compilez-le simplement. Pour savoir comment
compiler `lbmk`, consultez les [instructions de compilation](docs/build/).

Des informations sur le système de compilation lui-même et son fonctionnement sont disponibles dans
le [guide de maintenance de lbmk](docs/maintain/).

### lbwww et lbwww-img

Le site web et la documentation de Libreboot sont *entièrement* hébergés dans un dépôt Git.
Téléchargez-le ainsi :

	git clone https://codeberg.org/libreboot/lbwww

Les images sont hébergées sur <https://av.libreboot.org/> et disponibles dans un dépôt
séparé :

	git clone https://codeberg.org/libreboot/lbwww-img

Apportez les modifications que vous voulez. Voir les notes ci-dessous sur la façon d'envoyer des patchs.

Tout le site web est écrit en Markdown, plus précisément dans la variante Pandoc.
Les pages HTML statiques sont générées avec [lbssg](docs/sitegen/).
Leah Rowe, la fondatrice de Libreboot, est aussi la fondatrice du projet de générateur
de site statique de Libreboot.

Si vous le souhaitez, vous pouvez configurer un serveur HTTP local et compiler votre propre version
locale du site web. Notez que les images pointeront toujours vers celles
hébergées sur <https://av.libreboot.org/> ; ainsi, toute image que vous ajoutez à `lbwww-img`
n'apparaîtra pas sur votre site `lbwww` local si les liens des images (pour les
images que vous ajoutez) pointent vers `av.libreboot.org`. Toutefois, il est obligatoire que ces
images soient hébergées sur av.libreboot.org.

Par conséquent, si vous souhaitez ajouter des images au site web, soumettez-les aussi au
dépôt `lbwww-img`, avec des liens de la forme
<https://av.libreboot.org/chemin/vers/votre/nouvelle/image/dans/lbwww-img> pour chacune d'elles.
Lorsque votre contribution sera fusionnée sur le site de Libreboot, vos images apparaîtront en ligne.

Si vous ajoutez une photo, compressez-la pour une diffusion sur le web. Les images doivent faire environ
800 px de large, et généralement moins de 100 Kio :

Commencez par réduire votre image à environ 800 px de large, avec votre programme
de retouche d'image favori. Par exemple, avec `imagemagick`, vous pouvez faire
ce qui suit (assurez-vous que l'image n'est pas déjà plus petite ou égale à la taille souhaitée).

	convert original.jpg -resize 600000@ -quality 70% web.jpg

Vous devriez toujours exécuter `jpegoptim` sur les images jpg avant de les soumettre.
Il supprime les métadonnées inutiles et les optimise encore davantage *sans perte* en
réorganisant astucieusement les tables de Huffman qu'elles contiennent.

	jpegoptim -s --all-progressive web.jpg

Si l'image est un dessin (au trait), les graphiques vectoriels sont préférables aux images bitmap.
Par conséquent, si possible, enregistrez-les en SVG. Ils sont faciles à modifier
et faciliteront aussi sûrement le travail des traducteurs.

Les images PNG doivent être optimisées avec `zopfli` (également sans perte).
Par exemple, cela a réduit le logo de démarrage de Libreboot d'environ 11 Ko à 3 Ko :

	zopflipng -ym image.png image.png

Pour le développement, vous pouvez d'abord utiliser des liens locaux pour vos images, puis
ajuster les URL lorsque vous soumettez vos patchs de documentation/site web.

Les instructions pour configurer votre version locale du site web se trouvent dans le manuel de lbssg.
Téléchargez untitled, et dans votre répertoire `untitled`, créez
un répertoire nommé `www/`, puis allez dans le répertoire www et clonez-y le dépôt
`lbwww`. Configurez votre serveur HTTP local en conséquence.

Là encore, les instructions sont disponibles dans le manuel de lbssg à cet effet.

### Nom non requis

Les contributions que vous faites sont enregistrées publiquement, dans un dépôt Git auquel
tout le monde peut accéder. Cela inclut le nom et l'adresse e-mail du
contributeur.

Dans Git, pour le nom de l'auteur et son adresse e-mail, vous n'êtes pas obligé d'utiliser des données
d'identification. Vous pouvez utiliser `libreboot Contributor` et votre adresse e-mail peut être
indiquée comme contributor@libreboot.org. Vous avez le droit de le faire, si
vous souhaitez préserver votre vie privée. Nous croyons en la vie privée. Si vous choisissez de rester
anonyme, nous respecterons ce choix.

Bien sûr, vous pouvez utiliser le nom et/ou l'adresse e-mail que vous voulez.

Légalement parlant, tout droit d'auteur est automatique en vertu de la Convention de Berne sur le droit
d'auteur international. Peu importe le nom utilisé, ni même si vous
déclarez un droit d'auteur (mais nous exigeons que certaines licences de droit d'auteur
soient utilisées - voir plus à ce sujet sur cette même page).

Si vous utilisez un autre nom et une autre adresse e-mail pour vos commits/patchs, vous
devriez être relativement anonyme. Utilisez
[git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
et [git show](https://git-scm.com/docs/git-show) pour le vérifier avant de
pousser des modifications vers un dépôt Git public.

### Licences (pour les contributeurs)

Veillez à placer votre travail sous une licence libre. Libreboot n'impose
plus de restrictions arbitraires sur les licences acceptées, et il en existe déjà beaucoup.
Nous auditerons votre contribution et vous signalerons s'il y a des problèmes
(par exemple l'absence de licence).

Déclarez *toujours* une licence sur votre travail ! Ne pas déclarer de licence signifie que
les lois restrictives par défaut sur le droit d'auteur s'appliquent, ce qui rendrait votre travail
non libre, soumis à toutes les mêmes restrictions.

La licence MIT est un bon point de départ, et c'est la licence préférée
pour tous les nouveaux travaux dans Libreboot, mais nous ne sommes pas pointilleux. Libreboot a historiquement
utilisé surtout des licences GPL ; une grande partie le reste, et le restera probablement.
C'est votre travail ; évidemment, si vous dérivez d'un travail existant,
il peut être judicieux d'utiliser la même licence pour votre contribution, afin d'assurer la compatibilité des
licences.

Vous trouverez des exemples courants de licences
[ici](https://opensource.org/licenses).

Si vous dérivez *effectivement* d'un travail existant, il est important que votre licence
(pour votre contribution) soit compatible avec la licence du travail dont
le vôtre est dérivé. La licence MIT est bonne car elle est largement compatible
avec de nombreuses autres licences, et elle permet de nombreuses libertés (comme celle de
sous-licencier) que d'autres licences n'accordent pas :

<https://opensource.org/licenses/MIT>

### Envoyer des patchs

Créez un compte sur <https://codeberg.org/> et rendez-vous (une fois connecté) sur le
dépôt sur lequel vous souhaitez travailler. Cliquez sur *Fork* et, dans votre compte,
vous disposerez de votre propre dépôt Libreboot. Clonez votre dépôt, apportez-y les
modifications que vous voulez, puis poussez-les vers votre dépôt, dans
votre compte Codeberg. Vous pouvez aussi le faire sur une nouvelle branche, si vous le souhaitez.

Dans votre compte Codeberg, vous pouvez ensuite aller sur le dépôt officiel de Libreboot
et soumettre une Pull Request. Le fonctionnement est similaire à celui des autres
plateformes Git web populaires utilisées aujourd'hui.

Vous pouvez soumettre vos patchs là. Sinon, vous pouvez vous connecter au canal IRC
de Libreboot et indiquer au canal quels patchs vous souhaitez faire relire, si
vous avez votre propre dépôt Git contenant les patchs.

Une fois que vous avez émis une Pull Request, les mainteneurs de Libreboot sont notifiés
par e-mail. Si vous ne recevez pas de réponse assez rapide de la part du projet, vous
pouvez aussi le notifier via le canal `#libreboot` sur Libera Chat.

Une autre façon de soumettre des patchs est d'écrire directement à Leah Rowe par e-mail :
[info@minifree.org](mailto:info@minifree.org) est l'adresse e-mail de projet de Leah.

Cependant, pour la transparence du processus de relecture du code, il est recommandé d'utiliser
Codeberg, pour le moment.

### Liste de diffusion

Libreboot dispose de cette liste de diffusion :
<https://lists.sr.ht/~libreboot/libreboot>

L'adresse e-mail est [~libreboot/libreboot@lists.sr.ht](mailto:~libreboot/libreboot@lists.sr.ht)


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"