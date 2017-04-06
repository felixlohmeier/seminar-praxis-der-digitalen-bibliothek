# 3.1.1 Transformationshistorie anwenden

## Aufgabe: Speichern Sie die bisher durchgeführten Transformationsregeln und wenden Sie diese auf eine andere MARCXML-Datei an

OpenRefine verfügt über hilfreiche Undo/Redo-Funktionen, mit denen Sie auch alle bisher in einem Projekt durchgeführten Transformationsregeln speichern und auf ein anderes Projekt anwenden können.

Hinweise:

* Nutzen Sie die Funktion "Extract" im Bereich Undo/Redo und speichern Sie die Regeln in einer Textdatei zwischen (z.B. dem Texteditor Pluma, erreichbar im Startmenü unter Anwendungen/Zubehör/Pluma Text Editor).
* Erstellen Sie ein neues Projekt mit einer zweiten MARCXML-Datei (vgl. [Kapitel 2.3.2](2-3-2-openrefine-starten-und-daten-laden.md)).
* Wenden Sie die zwischengespeicherten Transformationsregeln über die Funktion "Apply" an.

## Lösung

Transformationsregeln extrahieren (altes Projekt):

* {%s%}Oben links Menü Undo / Redo den Button Extract... drücken{%ends%}
* {%s%}Alles im rechten Textfeld in die Zwischenablage kopieren (z.B. mit STRG+A und STRG+C){%ends%}

Transformationsregeln anwenden (neues Projekt):

* Neues Projekt erstellen wie in [Kapitel 2.3.2](2-3-2-openrefine-starten-und-daten-laden.md) beschrieben
* {%s%}Oben links Menü Undo / Redo den Button Apply... drücken{%ends%}
* {%s%}Den Inhalt der Zwischenablage einfügen (z.B. mit STRG+V) und den Button Perform Operations drücken.{%ends%}

## Literatur

* [History-Funktionen in OpenRefine Dokumentation](https://github.com/OpenRefine/OpenRefine/wiki/History)
* [JSON and my notepad or how to write script in google refine](http://kb.refinepro.com/2012/06/google-refine-json-and-my-notepad-or.html)
