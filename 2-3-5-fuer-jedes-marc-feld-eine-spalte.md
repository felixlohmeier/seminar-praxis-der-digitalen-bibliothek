# 2.3.5 Für jedes MARC-Feld eine Spalte

Aktuell sind die Inhalte eines Datensatzes über viele Zeilen verteilt. Die MARC-Felder stehen untereinander in Zeilen. Wir transformieren die Daten nun so, dass die Daten wie in einer üblichen Tabelle strukturiert sind, d.h.:

* Die MARC-Felder in den Spaltenüberschriften.
* Jeder Datensatz nur über eine Zeile.

Das Vorgehen ist insbesondere bei Schritt 3 nicht besonders intuitiv. Das liegt daran, dass OpenRefine das MARC21-Format nicht "versteht" und wir jeden einzelnen Transformationsschritt selbst vorgeben müssen. Gleichzeitig demonstriert es Ihnen einige Funktionen von OpenRefine und zeigt, wie vielfältig das Werkzeug einzusetzen ist.

## Vorgehen

Schritt 1: Vorerst nicht benötigte Spalten löschen

* All / Edit columns / Re-order / remove columns...
* Spalten "record", "record - datafield", "record - datafield - ind1", "record - datafield - ind2" "record-controlfield" und "record-controlfield-tag" nach rechts schieben

Schritt 2: MARC-Felder durchgängig belegen

* Spalte "record - datafield - tag" / Edit cells / Fill down

Schritt 3: PPN aus Spalte record-leader in Spalte mit MARC-Feldern verschieben (und dafür eine neue Zeile einfügen)

* Spalte "record - leader" / Add Column based on this column...; Name für neue Spalte: ```NEU```
* Spalte "record - leader" / Transpose / Transpose cells across columns into Rows; In der zweiten Spalte ("To Column") ```NEU``` auswählen, rechts die Option "One column" auswählen und Name ```PPN``` eingeben
* Spalte "record - datafield - tag" / Facet / Customized facets / Facet by blank / Wert ```true``` auswählen und in Modus "rows" wechseln
* Spalte "record - datafield - tag" / Edit cells / Transform... / Wert ```"001"``` (also mit Anführungszeichen) eingeben und Facette schließen
* Spalte "record - datafield - tag" / Facet / Text facet / Wert ```001``` auswählen
* Spalte "record - datafield - subfield" / Edit cells / Transform... / Wert ```cells["PPN"].value``` eingeben und Facette schließen

Schritt 4: MARC-Feld mit Feld MARC-Code zusammenfassen

* Spalte "record - datafield - tag" / Edit cells / Transform... den Wert ```value + " : " + cells["record - datafield - subfield - code"].value``` eingeben
* Spalte "record - datafield - subfield - code" / Edit column / Remove this column

Schritt 5: Sortieren und Aufräumen

* Spalte "PPN" / Edit cells / Fill down
* Spalte "PPN" / Sort...
* Spalte "record - datafield - tag" / Sort...
* Im neu verfügbaren Menü "Sort" den Menüpunkt "Reorder rows permanently" auswählen
* Spalte "PPN" / Edit column / Remove this column

Schritt 6: Felder mit Mehrfachbelegungen zusammenführen

* Spalte "record - datafield - tag" / Edit cells / Blank down
* Spalte "record - datafield - subfield" / Edit cells / Join multi-valued cells und als Trennzeichen ```␟``` angeben (das Trennzeichen [Unit Separator ␟](http://unicode-table.com/en/241F/) aus dem Unicode-Zeichensatz kommt mit Sicherheit nicht in den Daten vor, daher ist dieses gut geeignet. Das Zeichen ist am einfachsten per copy & paste einzufügen).

Schritt 7: Transpose

* Spalte "record - datafield - tag" / Transpose / Columnize by key/value columns...

## Literatur zu den genutzten Funktionen

* [How can I join two datasets using a key in OpenRefine, with the secondary table having more than one value?](http://www.devsplanet.com/question/35776263) und [Cells to columns in OpenRefine](http://stackoverflow.com/questions/15187543/cells-to-columns-in-openrefine)
* [Zellen zusammenführen](http://kb.refinepro.com/2011/07/merge-2-columns-that-have-both-blank.html)
* [Trick, um neue Zeilen einzufügen](http://kb.refinepro.com/2011/12/add-extra-rows-records-in-google-refine.html)
