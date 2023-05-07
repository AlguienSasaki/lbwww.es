---
title: Code review
x-toc-enable: true
...

Libreboot Repositories
===================

Informationen darüber wer an Libreboot arbeitet und wer das Projekt betreibt
sind unter [who.de.md](who.de.md) zu finden.

Das `Libreboot` Projekt hat hauptsächlich 3 Git Repositories:

* Build system: <https://codeberg.org/libreboot/lbmk>
* Website (+Anleitungen): <https://codeberg.org/libreboot/lbwww>
* Bilder (für diw Website): <https://codeberg.org/libreboot/lbwww-img>

You can also *browse* lbmk on Libreboot's own cgit instance, though it is not
intended for development (use codeberg for that):\
<https://browse.libreboot.org/lbmk.git/>

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
Original Repository auf `stuge.se`nicht mehr verfügbar ist, seit wie dies
zuletzt geprüft haben. Das `bucts` Programm wurde von Peter Stuge geschrieben.
Du benötigst `bucts` sofern Du ein Libreboot ROM intern auf ein Thinkpad X60 
oder T60 flashen möchtest, welches (derzeit) noch ein nicht-freies Lenovo 
BIOS verwendet. Anleitungen hierfür findest Du hier:\
[Libreboot Installations Anleitungen](docs/install/)

The `ich9utils` Repository wird erheblich vom `lbmk` build system verwendet.
Du kannst `ich9utils` allerdings auch separat herunterladen und verwenden.
Es erzeugt ICH9M descriptor+GbE Images für GM45 ThinkPads welche die ICH9M 
Southbridge verwenden. Es könnte auch für andere Systeme funktionieren,
welche dieselbe Platform bzw. denselben Chipsatz verwenden.
Dokumentation für `ich9utils` ist hier verfügbar:\
[ich9utils Dokumentation](docs/install/ich9utils.md)

lbmk (libreboot-make)
---------------------

Dies ist das zentrale build system in Libreboot. Man könnte auch sagen `lbmk` *ist*
Libreboot! Das Git repository herunterladen:

	git clone https://codeberg.org/libreboot/lbmk
  
Der oben dargestellte `git` Befehl, wird das Libreboot build system `lbmk`
herunterladen.
Du kannst dann folgendermassen in das Verzeichnis wechseln:

	cd lbmk

Ändere dies nach deinen Vorstellungen oder ertelle einfach einen build. 
Für Anleitungen bzgl. `lbmk` build, siehe [build Anleitungen](docs/build/).

Informationen über das build system selbst und wie es funktioniert, ist
verfügbar unter dem [lbmk maintenance guide](docs/maintain/).

lbwww and lbwww-img
-------------------

Die *gesamte* Libreboot Website sowie Dokumentation befindet sich in einem
Git Repository.
Du kannst es folgendermassen herunterladen:

	git clone https://codeberg.org/libreboot/lbwww

Bilder befinden sich unter <https://av.libreboot.org/> und sind verfügbar
in einem separaten Git Repository:

	git clone https://codeberg.org/libreboot/lbwww-img

Du kannst alles nach deinen Vorstellungen ändern. Beachte die nachfolgenden
Informationen wie Du deine Änderungen zur Verfügung stellen kannst.

Die gesamte Website ist in Markdown geschrien, insbesondere die Pandoc Version. 
Die statischen HTML Seiten werden mit [Untitled](https://untitled.vimuser.org/)
generiert. Leah Rowe, die Gründerin von Libreboot, ist auch die Gründerin des Untitled static
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

Zu Entwicklungszwecken, könntest Du deine Bilder auch lokal verknüpfen, und
anschliesend die URLs anpassen sobald Du deine Patches für die Dokumentation/Webseite schickst.

Eine Anleitung wie Du eine lokale Version der Webseite herstellst kannst,
befinden sich auf der Untitled Webseite. Lade untitled
herunter, und erstelle in dem `untitled` Verzeichnis ein Verzeichnis mit
dem Namen `www/` dann wechsle in dieses Verzeichnis und klone das `lbwww`
Repository dorthin. Konfiguriere deinen lokalen HTTP Server entsprechend.

Nochmal, Anleitungen hierfür findest Du auf der Untitled Webseite.

Name nicht erforderlich
-----------------

Beiträge die Du hinzufügst, werden in einem für jeden zugänglichen Git
Repository öffentlich aufgezeichnet. Dies betrifft ebenso den Namen sowie
die email Adresse des Mitwirkenden.

Du musst bei Git keinen Autoren Namen bzw. keine email Addresse verwenden,
mithilfe derer Du identifizierbar bist. Du kannst `libreboot Contributor`
verwenden und deine email Addresse könnte als contributor@libreboot.org
spezifiert werden. Es ist Dir gestattet dies zu tun, sofern Du deine Privatsphäre
wahren möchtest. Wir glauben an Privatsphäre. Sofern Du anonym bleiben möchtest 
werden wir dies respektieren.

Natürlich, kannst Du jeglichen Namen und/oder jegliche email Aaddresse verwenden
die Du möchtest.

Rechtlich gesprochen, jegliches copyright is automatic under the Berne Convention of
international copyright law. It does not matter which name, or indeed whether
you even declare a copyright (but we do require that certain copyright
licenses are used - read more about that on this same page).

Legally speaking, all copyright is automatic under the Berne Convention of
international copyright law. It does not matter which name, or indeed whether
you even declare a copyright (but we do require that certain copyright
licenses are used - read more about that on this same page).

If you use a different name and email address on your commits/patches, then you
should be fairly anonymous. Use
[git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
and [git show](https://git-scm.com/docs/git-show) to confirm that before you
push changes to a public Git repository.

Licenses (for contributors)
--------

Make sure to freely license your work, under a libre license. Libreboot no
longer sets arbitrary restrictions on what licenses are accepted, and many
licenses out there already exist. We will audit your contribution and tell
you if there are problems with it (e.g. no license).

*Always* declare a license on your work! Not declaring a license means that
the default, restrictive copyright laws apply, which would make your work
proprietary, subject to all of the same restrictions.

The MIT license is a good one to start with, and it is the preferred license
for all new works in Libreboot, but we're not picky. Libreboot has historically
used GNU licensing such as GPL; much of that remains, and is likely to remain.
It's your work; obviously, if you're deriving from an existing work,
it may make sense to use the same license on your contribution, for license
compatibility.

You can find common examples of licenses
[here](https://opensource.org/licenses).

If you *are* deriving from an existing work, it's important that your license
(for your contribution) be compatible with the licensing of the work from which
yours was derived. The MIT license is good because it's widely compatible
with many other licenses, and permits many freedoms (such as the freedom to
sublicense) that other licenses do not:

<https://opensource.org/licenses/MIT>

Send patches
------------

Make an account on <https://codeberg.org/> and navigate (while logged in) to the
repository that you wish to work on. Click *Fork* and in your account,
you will have your own repository of libreboot. Clone your repository, make
whatever changes you like to it and then push to your repository, in your
account on NotABug. You can also do this on a new branch, if you wish.

In your Codeberg account, you can then navigate to the official libreboot
repository and submit a Pull Request. The way it works is similar to other
popular web-based Git platforms that people use these days.

You can submit your patches there. Alternative, you can log onto the libreboot
IRC channel and notify the channel of which patches you want reviewed, if you
have your own Git repository with the patches.

Once you have issued a Pull Request, the libreboot maintainers will be notified
via email. If you do not receive a fast enough response from the project, then
you could also notify the project via the `#libreboot` channel on Libera Chat.

Another way to submit patches is to email Leah Rowe directly:
[info@minifree.org](mailto:info@minifree.org) is Leah's project email address.

However, for transparency of the code review process, it's recommended that you
use Codeberg, for the time being.

Mirrors of lbmk.git
===================

The `lbmk` repository contains Libreboot's automated build system, which
produces Libreboot releases (including compiled ROM images).

You can run `git clone` on any of these links (the links are also clickable,
to view changes in your Web browser):

* <https://git.disroot.org/libreboot/lbmk>
* <https://gitea.treehouse.systems/libreboot/lbmk>
* <https://git.fosscommunity.in/libreboot/lbmk>
* <https://0xacab.org/libreboot/lbmk/>
* <https://framagit.org/libreboot/libreboot>
* <https://gitlab.com/libreboot/lbmk>
* <https://pagure.io/libreboot>
* <https://rocketgit.com/libreboot/libreboot>

lbwww.git mirror
----------------

The `lbwww` repository contains Markdown files (pandoc variant), for use
with the [Untitled Static Site Generator](https://untitled.vimuser.org/); this
is what Libreboot uses to provide HTML web pages, *including* the page that
you are reading right now!

You can run `git clone` on these links, and/or click to view changes in your
Web browser. See:

* <https://git.disroot.org/libreboot/lbwww>
* <https://gitea.treehouse.systems/libreboot/lbwww>
* <https://git.fosscommunity.in/libreboot/lbwww>
* <https://0xacab.org/libreboot/lbwww>
* <https://framagit.org/libreboot/lbwww/>
* <https://gitlab.com/libreboot/lbwww>
* <https://rocketgit.com/libreboot/lbwww>

NOTE: The `lbwww-img` repository is not generally provided, on mirrors, as
those are just image files which you can find on <https://av.libreboot.org>
and it is not the intention of the Libreboot project to bog down *mirrors*
with additional traffic by hosting images.

Notabug repositories
====================

Commits that go to codeberg are also still pushed to notabug, in addition to
the other mirrors. Notabug is considered a *mirror* since 8 April 2023, when
Libreboot's main development site moved to *Codeberg*.

OLD notabug repos are still pushed to as backup, but the codeberg mirror is
considered to be main/official now, as of the [announcement on 8
April 2023](news/codeberg.md). See:

* Build system: <https://notabug.org/libreboot/lbmk>
* Website (+docs): <https://notabug.org/libreboot/lbwww>
* Images (for website): <https://notabug.org/libreboot/lbwww-img>

For sending patches, it is now preferred that you use *codeberg*. Technically,
pull requests are still possible via Notabug. While Notabug still exists,
Libreboot patches will continue be pushed there, mirroring what gets pushed
on Notabug.

Because pull requests and issues were available on notabug in the past, it
makes sense to keep them open, though we ask that you send to codeberg. If
they were to be closed on notabug, existing PRs and issues won't be visible
anymore either, so they have to stay open.
