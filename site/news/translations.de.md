% Übersetzungen benötigt
% Leah Rowe
% 4 January 2022

Die Libreboot Webseite ist derzeit nur in Englisch verfügbar.

Ich habe kürzlich Unterstützung für Übersetzungen zum
[Untitled Static Site Generator](https://untitled.vimuser.org/) hinzugefügt,
welcher von der Libreboot Webseite verwendet wird. Die Seiten auf
libreboot.org sind in Markdown geschrieben, und diese Software erzeugt
HTML Seiten.

Die Seite die Du gerade liest wurde auf diese Weise erstellt!

Vorbereitung
--------------------

Die Libreboot Webseite ist verfügbar, in Markdown, über ein Git Repository:\
<https://codeberg.org/libreboot/lbwww>

Anleitungen zum senden von Patches sind hier verfügbar:\
[pagina Git](../git.de.html).

Wenn Du an einer Übersetzung arbeitest, notiere Dir die commit ID auf `lbwww.git`
und verfolge weitere Änderungen (an der englischen Webseite) in diesem
Repository. 

Wenn Du die Übersetzung sendest, gib bitte an mit welcher commit ID auf
`lbwww.git` diese übereinstimmt. Von dort an werde ich die Änderungen an
der englischen Seite nachverfolgen, an welcher ich arbeite. Meine
Muttersprache ist englisch. Wenn die erste Übersetzung auf libreboot.org
verfügbar ist, werde ich eine neue Seite erstellen (nur in englisch), und
dort jedesmal vermerken wenn ich Änderungen an der Seite vornehme, und
zeigen wo diese Änderungen dann auf übersetzten Seiten erledigt werden
müssen für jede Seite die ich ändere.
 
Wie übersetzt man libreboot.org
------------------------

Die Dokumentation auf <https://untitled.vimuser.org/> erläutert wie Du
Übersetzungen handhaben kannst. 

Ich empfehle das Du einen lokalen Nginx HTTP Server auf deinem Computer
einrichtest, und Untitled hierfür konfigurierst, mithilfe der Anweisungen
auf der Untitled Webseite. Dies wird es vereinfachen zu sehen wie deine
übersetzte Seite aussieht, bevor diese live geht.

**Es wird empfohlen das Du eine Firewall installierst, wenn Du Nginx
verwendest, sofern Du nicht tatsächlich möchtest das es öffentlich
zugänglich ist. Die `ufw` Software ist sehr nett**  

	sudo apt-get install ufw
	sudo ufw enable

Dies wird jeglichen unerwünscht eingehenden Datenverkehr blocken. Dies ist
ohnehin eine bewährte Praktik, für Workstations. Du musst nicht ufw verwenden,
aber es ist ein nettes Frontend für iptables/ip6tables auf Systemen die
den Linux Kernel verwenden. Weitere Informationen über `ufw` sind hier
verfügbar:

<https://help.ubuntu.com/community/UFW>

Wenn Du deine lokale Webseite betrachten möchtest, musst Du lediglich
`http://localhost/` in deinen Browser eingeben. Dies wird deine lokale
loopback Adresse aufrufen.

Grundsätzlich wirst Du mit `md` und `include` Dateien arbeiten.
Halte Ausschau nach Dateien mit `template`, `footer` und `nav` im Dateinamen.
Weitere Informationen darüber wie Untitled funktioniert ist verfügbar auf
der Untitled Webseite. Du solltest ebenso eine übersetzte `strings.cfg`
Datei zu Untitled hinzufügen für deine Übersetzung, sofern Untitled diese
nicht unterstützt. 

Du kannst Seiten in jeder Sprache zu Untitled hinzufügen, Die Software
generiert automatisch ein Menü zur Sprach-Auswahl, auf Seitenbasis sofern
eine Übersetzung für die jeweilige Seite vorliegt.


[//]: # "SPDX-License-Identifier: GFDL-1.3-or-later"
