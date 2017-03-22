# 2.3.2 OpenRefine starten und Daten laden

Die folgenden Übungen in diesem Kapitel führen wir zunächst mit einer einzigen Datei aus dem Download durch. Diese Datei beinhaltet "nur" 100 Datensätze, mit denen wir beispielhaft arbeiten. Im nächsten Kapitel werden wir dann die gelernten Transformationen auf alle rund 600.000 Datensätze anwenden.

## Aufgabe: Daten konfigurieren und in ein neues Projekt laden

Starten Sie OpenRefine, laden Sie eine der in Kapitel 2.2.3 heruntergeladenen MARCXML-Dateien in OpenRefine und legen Sie damit ein neues Projekt an.

Hinweise:

* Wählen Sie die Option "XML-Files". Die Option "MARC" ist nicht für XML gedacht und [funktioniert derzeit ohnehin nicht](https://github.com/OpenRefine/OpenRefine/issues/794).

## Lösung

* OpenRefine starten: {%s%}~/openrefine-linux-2.7-rc.2/refine{%ends%}
* Projekt erstellen: {%s%}Im Menüpunkt "Create Project" auf den Button "Durchsuchen" klicken und eine der MARCXML Dateien auswählen. Im nächsten Bildschirm unten links bei Parse data as "XML files" auswählen, dann im Vorschaubildschirm auf den Pfad record xmlns="http://www.loc.gov/MARC21/slim" klicken und dann oben rechts den Button "Create Project" drücken.{%ends%}

Stören Sie sich nicht zu sehr an der zunächst unübersichtlichen Darstellung der Daten, das werden wir in den nächsten Übungen bereinigen.
