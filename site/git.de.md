---
title: Use Git and/or send patches to Libreboot
x-toc-enable: true
...

SPDX HEADERS
------------

When submitting new documentation pages (e.g. Markdown files), make
sure they are explicitly licensed. You can do this by adding an SPDX
header at the bottom of the file.

Please look at other pages in the repository, to know how this is done.

`GFDL-1.3-or-later` is assumed, as that is what most of the site uses.
You can add this as a comment, in Markdown files. Existing pages show
how to do this (look at the bottom of an existing Markdown file).

A generic notice is also placed stating GFDL, in footer `.include`
files.

If in doubt, simply place this at the bottom of your new Markdown
file when submitting it for review:

```
[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
```

If you're working on an existing Markdown file present in the
website repository, this will probably already be there.

Libreboot Repositories
---------------------

Das `libreboot` Projekt hat hauptsächlich 3 Git Repositories:

* Build system: <https://codeberg.org/libreboot/lbmk>
* Webseite (+Anleitungen): <https://codeberg.org/libreboot/lbwww>
* Bilder (für die Webseite): <https://codeberg.org/libreboot/lbwww-img>
* Pico-serprog: <https://codeberg.org/libreboot/pico-serprog>
* Libreboot Static Site Generator: <https://codeberg.org/libreboot/lbssg>

Also see: [Libreboot Static Site Generator](docs/sitegen/).

Du kannst dir lbmk auch auf Libreboot's eigener cgit Instanz *ansehen*,
allerdings ist dies nicht für die Entwicklung gedacht (benutze hierfür bitte codeberg):\
<https://browse.libreboot.org/lbmk.git/>

The [tasks](tasks/) page lists some (not all) of the things we wish to
implement, if not already implemented. Patches welcome!

Weiter unten auf dieser Seite sind Mirror von `lbmk` und `lbwww` aufgelistet, 
sofern die Haupt Git Repositories nicht erreichbar sein sollten.

Zuvor hat Libreboot NotABug verwendet, aber es gab regelmässig
Probleme mit der der Zuverlässigkeit aufgrund von HTTP Error 500,
hauptsächlich in den Abendstunden, höchstwahrscheinlich weil zu viele Leute
darauf zugegriffen haben; daher wurde beschlossen, das Libreboot eine
stabilere Lösung benötigt, daher verwendet Libreboot nun codeberg. Siehe
[Ankündigung des Wechsels zu codeberg, 8. April 2023](news/codeberg.md)

Zudem gibt es noch diese vom Libreboot Projekt gehosteten Programme, welche
von libreboot entweder empfohlen oder verwendet werden:  

Das `ich9utils` Projekt ist nun unter `util/ich9utils` in lbmk verfügbar,
und lbmk verdendet *dies*, aber das alte standalone Repository ist nach
wie vor verfügbar unter notabug (bucts is also there):

* Bucts (Utility): <https://notabug.org/libreboot/bucts>
* ich9utils (Utility): <https://notabug.org/libreboot/ich9utils>

Du kannst diese Repositories herunterladen, sie nach deinen Wünschen ändern,
und dann deine Änderungen zur Verfügung stellen mithilfe der folgenden
Anleitungen.

Es wird empfohlen den libreboot build (alle zugehörigen Teile) in einer
Linux Umgebung herzustellen. Unter BSD Systemen ist das build system (lbmk)
beispielsweise nicht getestet.
Installiere `git` auf deinem Linux System und lade eines der Repositories
herunter.

Die Entwicklung von Libreboot findet mithilfe der Versionskontrolle von
Git statt. Sieh in der [offiziellen Git Dokumentation](https://git-scm.com/doc)
nach, sofern Du nicht weisst wie man Git verwendet. 

Das `bucts` Repository wird auch vom Libreboot Projekt gehostet, da das
Original Repository auf `stuge.se` nicht mehr verfügbar ist, seit wie dies
zuletzt geprüft haben. Das `bucts` Programm wurde von Peter Stuge geschrieben.
Du benötigst `bucts` sofern Du ein Libreboot ROM intern auf ein Thinkpad X60 
oder T60 flashen möchtest, welches (derzeit) noch ein nicht-freies Lenovo 
BIOS verwendet. Anleitungen hierfür findest Du hier:\
[Libreboot Installations Anleitungen](docs/install/)

Das `ich9utils` Repository wird erheblich vom `lbmk` build system verwendet.
Du kannst `ich9utils` allerdings auch separat herunterladen und verwenden.
Es erzeugt ICH9M descriptor+GbE Images für GM45 ThinkPads welche die ICH9M 
Southbridge verwenden. Es könnte auch für andere Systeme funktionieren,
welche dieselbe Platform bzw. denselben Chipsatz verwenden.
Dokumentation für `ich9utils` ist hier verfügbar:\
[ich9utils Dokumentation](docs/install/ich9utils.md)

### lbmk (libreboot-make)

Dies ist das zentrale build system in Libreboot. Man könnte auch sagen `lbmk` *ist*
Libreboot! Das Git repository herunterladen:

	git clone https://codeberg.org/libreboot/lbmk
  
Der oben dargestellte `git` Befehl, wird das Libreboot build system `lbmk`
herunterladen.
Du kannst dann folgendermassen in das Verzeichnis wechseln:

	cd lbmk

Ändere dies nach deinen Vorstellungen oder erstelle einfach einen build. 
Für Anleitungen bzgl. `lbmk` build, siehe [build Anleitungen](docs/build/).

Informationen über das build system selbst und wie es funktioniert, sind
verfügbar unter dem [lbmk maintenance guide](docs/maintain/).

### lbwww and lbwww-img

Die *gesamte* Libreboot Website sowie Dokumentation befindet sich in einem
Git Repository.
Du kannst es folgendermassen herunterladen:

	git clone https://codeberg.org/libreboot/lbwww

Bilder befinden sich unter <https://av.libreboot.org/> und sind verfügbar
in einem separaten Git Repository:

	git clone https://codeberg.org/libreboot/lbwww-img

Du kannst alles nach deinen Vorstellungen ändern. Beachte die nachfolgenden
Informationen wie Du deine Änderungen zur Verfügung stellen kannst.

Die gesamte Website ist in Markdown geschrieben, insbesondere die Pandoc Version. 
Die statischen HTML Seiten werden mit [lbssg](docs/sitegen/)
generiert. Leah Rowe, die Gründerin von Libreboot, ist auch die Gründerin des Libreboot static
site generator Projekts.

Wenn Du möchtest, kannst Du einen lokalen HTTP Server einrichten und eine
lokale Version der Website herstellen. Bitte bedenke, dass alle Bilder nach
wie vor mit den Bildern auf <https://av.libreboot.org/> verknüpft werden,
daher werden jegliche Bilder die Du `lbwww-img` hinzugefügt hast nicht auf
deiner lokalen `lbwww` Seite dargestellt, sofern Du die Bilder (die Du
hinzugefügt hast) mit `av.libreboot.org` verknüpfst. Es ist jedoch erforderlich, 
dass sich diese Bilder auf av.libreboot.org befinden.

Sofern Du der Webseite Bilder hinzufügen möchtest, füge diese ebenso
dem `lbwww-img` Repository hinzu, indem Du diese dann jeweils mit diesem Link verknüpfst
<https://av.libreboot.org/path/to/your/new/image/in/lbwww-img>.
Wenn dein Patch der Libreboot Webseite hinzugefügt wird, werden erscheinen deine Bilder live.

If adding a photo, compress it for web distribution. Images should be about
800px wide, and usually under 100KiB in size:

First, scale your image down to approximately 800px width, using your favourite
image manipulation program. For example, with `imagemagick` you can do the
following (make sure the image isn't already smaller or equal than preferred).

        convert original.jpg -resize 600000@ -quality 70% web.jpg

You should always run `jpegoptim` on jpg images before submitting them.
It strips useless metadata and *losslessly* optimises them further by cleverly
rearranging the huffman tables used in them.

        jpegoptim -s --all-progressive web.jpg

If the image is a (line) drawing, vector graphics are preferable to bitmaps.
Therefore, if possible, save them as SVGs. Those are easy to modify,
and will surely make translators' work easier as well.

PNG images should be optimised with `zopfli` (this is lossless as well).
For example, this reduced the Libreboot boot logo from around 11k to 3k:

        zopflipng -ym image.png image.png

Zu Entwicklungszwecken, könntest Du deine Bilder auch lokal verknüpfen, und
anschliesend die URLs anpassen sobald Du deine Patches für die Dokumentation/Webseite schickst.

Eine Anleitung wie Du eine lokale Version der Webseite herstellen kannst,
befinden sich auf der lbssg dokumentation. Lade untitled
herunter, und erstelle in dem `untitled` Verzeichnis ein Verzeichnis mit
dem Namen `www/` dann wechsle in dieses Verzeichnis und klone das `lbwww`
Repository dorthin. Konfiguriere deinen lokalen HTTP Server entsprechend.

Nochmal, Anleitungen hierfür findest Du auf der lbssg dokumentation.

### Name nicht erforderlich

Beiträge die Du hinzufügst, werden in einem für jeden zugänglichen Git
Repository öffentlich aufgezeichnet. Dies betrifft ebenso den Namen sowie
die email Adresse des Mitwirkenden.

Du musst bei Git keinen Autoren Namen bzw. keine email Addresse verwenden,
mithilfe derer Du identifizierbar bist. Du kannst `libreboot Contributor`
verwenden und deine email Addresse könnte als contributor@libreboot.org
spezifiert werden. Es ist Dir gestattet dies zu tun, sofern Du deine Privatsphäre
wahren möchtest. Wir glauben an Privatsphäre. Sofern Du anonym bleiben möchtest 
werden wir dies respektieren.

Natürlich kannst Du jeglichen Namen und/oder jegliche email Adresse verwenden
die Du möchtest.

Rechtlich gesprochen, jegliches Urheberrecht fällt automatisch unter die
Berner Übereinkunft zum Schutz von Werken der Literatur und Kunst. Es spielt
keine Rolle welchen Namen, oder ob Du tatsächlich überhaupt ein Urheberrecht
deklariert hast (aber wir setzen voraus das bestimmte Lizenzen für das
Urheberrecht verwndet werden - lies mehr darüber auf dieser Seite).

Sofern Du einen anderen Namen sowie eine andere email Adresse für deine 
Commits/Patches verwendest dann solltest Du anonym sein. Verwende 
[git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
und [git show](https://git-scm.com/docs/git-show) um dies zu überprüfen
bevor Du einem öffentlichen Git Repository Änderungen hinzufügst.

### Lizenzen (für Mitwirkende)

Stelle sicher, dass deine Beiträge mit einer libre Lizenz frei lizensiert
sind. Libreboot schreibt nicht mehr vor, welche Lizenzen akzeptiert werden,
und es existieren einige Lizenzen. Wir werden deinen Beitrag prüfen und
dir mitteilen sofern es ein Problem damit gibt (z.B. keine Lizenz).

Gib *immer* eine Lizenz an für deine Arbeit! Keine Lizenz anzugeben bedeutet
das deine Arbeit unter die Standard Urheberrechte fällt, was deine Arbeit 
proprietär macht und somit von denselben Einschränkungen betroffen ist.

Die MIT Lizenz ist ein guter Start, und sie ist die bevorzugte Lizenz
für sämtliche Arbeit an Libreboot, aber wir sind nicht pingelig.

[Hier](https://opensource.org/licenses) findest Du übliche Beispiele für Lizenzen.

*Wenn* deine Arbeit auf bestehender Arbeit basiert, dann ist es wichtig
(für deinen Beitrag) das die Lizenz deiner Arbeit kompatibel ist mit der
Lizenz der Arbeit auf der sie beruht. Die MIT Lizenz ist hierfür gut geeignet,
weil sie mit vielen anderen Lizenen kompatibel ist, und Freiheit zulässt
(wie zum Beispiel die Freiheit einer SubLizenz) was bei vielen anderen
Lizenzen nicht der Fall ist:

<https://opensource.org/licenses/MIT>

### Patches senden

Erstelle einen Account unter <https://codeberg.org/> und navigiere (während 
Du eingeloggt bist) zu dem Repository das Du bearbeiten möchtest. Klicke 
auf *Fork* und Du wirst ein eigenes Libreboot Repository in deinem Account 
erhalten. Erstelle einen Clone dieses Repository, füge alle gewünschten Änderungen hinzu
und führe anschliessend einen Push in dein Repository in deinem Account
auf Codeberg durch. Du kannst dies auch in einem neuen Branch erledigen,
sofern Du magst.

In deinem Codeberg Account kannst Du nun zum offiziellen Libreboot
Repository navigieren und dort einen Pull Request erstellen. Die Art und 
Weise wie dies funktioniert ist vergleichbar mit anderen populären Web basierten
Git Plattformen die heutzutage verwendet werden. 

Du kannst dort deine Patches bereitstellen. Alternativ kannst Du dich in
den Libreboot IRC Kanal einloggen und dort bekannt geben welche deiner Patches 
geprüft werden sollen, sofern Du ein eigenes Git repository mit den Patches
hast.

Sobald Du einen Pull Request erstellt hast, werden die Libreboot Maintainer
per email informiert. Sofern Du nicht schnell genug eine Antwort erhälst,
kannst Du das Projekt ebenso mithilfe des `#libreboot` Kanals auf Libera
Chat kontaktieren.

Ein weiterer Weg Patches zu senden ist Leah Rowe direkt eine email zu senden:
[info@minifree.org](mailto:info@minifree.org) ist Leah's Projekt email Addresse.

Um den Prozess der Quelltext Überprüfung transparent zu gestalten,
wird jedoch empfohlen künftig Codeberg zu verwenden.

### Mailing list

Libreboot has this mailing list:
<https://lists.sr.ht/~libreboot/libreboot>

The email address is [~libreboot/libreboot@lists.sr.ht](mailto:~libreboot/libreboot@lists.sr.ht)

Mirrors für lbmk.git
--------------------

Das `lbmk` Repository enthält Libreboot's automatischess build system, welches
Libreboot Veröffentlichungen  herstellt (inklusive kompilierter ROM Images).

Du kannst `git clone` für alle diese Links ausführen (die Links können auch
angeklickt werden, um Änderungen in deinem Web Browser anzusehen):

* <https://git.sr.ht/~libreboot/lbmk>
* <https://git.disroot.org/libreboot/lbmk>
* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://git.fosscommunity.in/libreboot/lbmk>
* <https://0xacab.org/libreboot/lbmk/>
* <https://framagit.org/libreboot/libreboot>
* <https://gitlab.com/libreboot/lbmk>
* <https://pagure.io/libreboot>
* <https://rocketgit.com/libreboot/libreboot>

Mirrors fur pico-serprog.git
----------------------------

* <https://notabug.org/libreboot/pico-serprog>

### lbwww.git Mirror

Das `lbwww` Repository enthält Markdown Dateien (Pandoc Variant), für die
Verwendung mit dem [Libreboot Static Site Generator](docs/sitegen/);
dies wird von Libreboot verwendet um HTML Web Seiten bereitzustellen, *inklusive*
der Seite die Du gerade liest!

Du kannst `git clone` für diese Links ausführen und/oder die Links
anklicken um Änderungen in deinem Web Browser anzusehen). Siehe:

* <https://git.sr.ht/~libreboot/lbwww>
* <https://git.disroot.org/libreboot/lbwww>
* <https://gitea.treehouse.systems/libreboot/lbwww>
* <https://git.fosscommunity.in/libreboot/lbwww>
* <https://0xacab.org/libreboot/lbwww>
* <https://framagit.org/libreboot/lbwww/>
* <https://gitlab.com/libreboot/lbwww>
* <https://rocketgit.com/libreboot/lbwww>

HINWEIS: Das `lbwww-img` Repository wird generell nicht auf einem Mirror
zur Verfügung gestellt, weil dies lediglich Bilder sind die Du unter 
<https://av.libreboot.org> finden kannst und es ist nicht die Intention
des Libreboot Projektes *Mirror* mit zusätzlichen Datenverkehr durch
Bilder zu belasten.

Notabug Repositories
--------------------

Commits die zu codeberg gepusht werden, werden ebenso zu notabug gepusht, 
zusätzlich zu den anderen Mirrors.
Notabug wird seit dem 8. April 2023 als *Mirror* betrachtet, seitdem
Libreboot's Haupt Entwicklung zu *Codeberg* gewechselt hat.

In die ALTEN notabug Repositories wird zu Backup Zwecken nach wie vor gepusht, 
aber der codeberg mirror wird nun als der hauptsächliche/offizielle betrachtet, 
wie in dieser [Ankündigung vom 8. April 2023](news/codeberg.md). Siehe:

* Build system: <https://notabug.org/libreboot/lbmk>
* Webseite (+Dokumentation): <https://notabug.org/libreboot/lbwww>
* Bilder (für die Webseite): <https://notabug.org/libreboot/lbwww-img>

Um Patches zu senden, wird nun bevorzugt wenn Du *codeberg* verwendest. 
Technisch gesehen sind pull requests via Notabug nach wie vor möglich. 
Während Notabug nach wie vor existiert, werden Libreboot Patches nach wie
vor dorthin gepushed, als Mirror für Änderungen die auf Notabug gepushed werden.

Weil pull requests und issues in der Vergangenheit auf notabug verfügbar waren, 
macht es Sinn diese offen zu lassen, dennoch würden wir dich bitten an codeberg
zu schicken. Sofern diese auf notabug geschlossen werden, dann werden diese PRs
und issues ohnehin nicht mehr sichtbar, daher sollten diese offen bleiben.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"


The Canoeboot version of this page is: <https://canoeboot.org/git.de.html>
