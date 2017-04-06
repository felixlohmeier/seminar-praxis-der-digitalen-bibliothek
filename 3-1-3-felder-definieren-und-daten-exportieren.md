# 3.1.3 Felder definieren und Daten exportieren

Jetzt wird es konkret: Welche Informationen wollen Sie in Ihrem Bibliothekskatalog anbieten? Welche Kurzinformationen sollen in der Trefferliste stehen? Welche Informationen sollen in der Vollanzeige dargestellt werden? Wir bilden jetzt aus den Rohdaten die gewünschten Felder.

## Aufgabe 1: Bilden Sie in OpenRefine das Feld "Titel" durch die Kombination verschiedener MARC-Felder

Hinweise:

* Die Expression zur Kombination von mehreren Feldern lautet (am Beispiel der Spalten 245 : a und 245 : b): ```cells["245 : a"].value + cells["245 : b"].value```. Die Werte der Zellen werden also schlicht mit einem ```+``` verbunden.
* Leider schlägt diese Expression fehl, wenn die zu ergänzende Zelle leer ist. Daher muss eine if-Abfrage ergänzt werden, damit die Transformation nur für nicht-leere Zellen durchgeführt wird: ```cells["245 : a"].value + if(isNonBlank(cells["245 : b"].value,cells["245 : b"].value,"")```
* Üblicherweise werden die Bestandteile des Titels durch unterschiedliche Trennzeichen getrennt. Die Trennzeichen sollten allerdings nur ergänzt werden, wenn danach auch ein Wert folgt. Daher muss die Ergänzung des Trennzeichens mit einer if-Abfrage von der folgenden Zelle abhängig gemacht werden. Beispiel für 245 : a und 245 : b mit Trennzeichen: ```cells["245 : a"].value + if(isNonBlank(cells["245 : b"].value),". ","") + if(isNonBlank(cells["245 : b"].value),cells["245 : b"].value,"")
* Versuchen Sie auf diese Weise alle relevanten MARC-Felder zu einem Titel-Feld zu kombinieren.

## Lösung

* {%s%}Spalte "245 : a" / Edit column / Add column based on this column...{%ends%}
* {%s%}New column name: Titel{%ends%}
* {%s%}Expression: value + if(isNonBlank(cells["245 : b"].value),". ","") + if(isNonBlank(cells["245 : b"].value),cells["245 : b"].value,"") + if(isNonBlank(cells["245 : n"].value)," - ","") + if(isNonBlank(cells["245 : n"].value),cells["245 : n"].value,"") + if(isNonBlank(cells["245 : p"].value)," - ","") + if(isNonBlank(cells["245 : p"].value),cells["245 : p"].value,"") + if(isNonBlank(cells["246 : a"].value)," - ","") + if(isNonBlank(cells["246 : a"].value),cells["246 : a"].value,""){%ends%}

** Als JSON-Datei: [3-1-3-1.json](3-1-3-1.json)**


## Aufgabe 2: Löschen Sie alle Datensätze, in denen das Feld "Titel" nicht belegt ist

Hinweise:

* In den rund 580.000 Datensätzen sind viele Fremd- und Normdaten enthalten, die wir zunächst nicht berücksichtigen wollen, um es nicht zu kompliziert zu machen.
* Vereinfachend lässt sich annehmen, dass alle Datensätze, die nach Erledigung der Aufgabe 4 noch keinen Titel haben, gelöscht werden können.

## Lösung

* {%s%}Spalte "Titel" / Facet / Customized facets / Facet by blank / true{%ends%}
* {%s%}All / Edit rows / Remove all matching rows{%ends%}
* {%s%}Facette schließen{%ends%}

** Als JSON-Datei: [3-1-3-2.json](3-1-3-2.json)**


## Aufgabe 3: Generieren Sie einheitliche ISBN-Nummern mit 13 Ziffern

Hinweise:

* Im MARC-Feld "020 : a" stehen teilweise 10-stellige, teilweise 13-stellige ISBN-Nummern.
* Suchen Sie nach einem Weg, um diese auf 13 Ziffern zu vereinheitlichen.
* Da es hier viele Mehrfachbelegungen gibt, müssen Sie die Zellen zunächst aufsplitten und nach erledigter Transformation wieder zusammenführen. Dieser Prozess ist speicherintensiv, also starten Sie OpenRefine vor und nach dieser Aufgabe besser neu.

## Lösung

* {%s%}Spalte "020 : a" / Edit cells / Split multi-valued cells... / ␟{%ends%}
* {%s%}Spalte "020 : a" / Facet / Custom text facet... / Expression: value.length() / 13{%ends%}
* {%s%}Spalte "020 : a" / Edit column / Add column based on this column... / New column name: ISBN /Expression: value{%ends%}
* {%s%}Spalte "020 : a" / Facet / Custom text facet... / Expression: value.length() / 10{%ends%}
* {%s%}Spalte "ISBN" / Edit cells / Transform... / Expression: with('978'+cells["020 : a"].value[0,9],v,v+((10-(sum(forRange(0,12,1,i,toNumber(v[i])*(1+(i%2*2)) )) %10)) %10).toString()[0] ){%ends%}
* {%s%}Facette schließen{%ends%}
* {%s%}Spalte "ISBN" / Edit cells / Join multi-valued cells... / ␟{%ends%}
* {%s%}Spalte "020 : a" / Edit cells / Join multi-valued cells... / ␟{%ends%}

Anmerkung: Alle Sonderfälle, in denen noch Text hinter den ISBN-Nummern steht, sind mit diesen Transformationsregeln noch nicht behandelt. Dafür liegt aber zumindest für einen Teil der Datensätze eine einheitliche ISBN13-Kodierung vor.

** Als JSON-Datei: [3-1-3-3.json](3-1-3-3.json)**


## Aufgabe 4: Ergänzen Sie ein Feld "id" für den Suchindex

Der Suchindex Solr erwartet ein Feld "id" mit eindeutiger Kennung in der ersten Spalte.

## Lösung

* {%s%}Spalte "001" /  Edit column / Add column based on this column... / New column name: id{%ends%}

** Als JSON-Datei: [3-1-3-4.json](3-1-3-4.json)**


## Aufgabe 5: Wenden Sie die vorbereitete Transformationsdatei zur Generierung weiterer Felder an

Das identifizieren wichtiger Felder wie Titel, Urheber, Ort, Erscheinungsjahr, Medientyp in den MARC-Daten ist mühsam und sprengt den Rahmen dieses Seminars. Ich habe daher eine Transformationsdatei erstellt, die Regeln zur Generierung weiterer Felder enthält. Diese unvollständige Empfehlung bildet auch die Grundlage für die folgenden Aufgaben. Diese haben nicht zum Ziel ein perfektes Mapping zu erstellen, sondern sollen ein paar Problemfelder illustrieren und sinnvolle Beispieldaten für den Suchindex (Kapitel 8) und die Kataloganzeige (Kapitel 9) bilden.

Hinweise:
* Verwenden Sie die **JSON-Datei ** Als JSON-Datei: [3-1-3-5.json](3-1-3-5.json)****

## Lösung

* {%s%}Menü oben links "Undo / Redo" aufrufen und Button "Apply..." drücken {%ends%}
* {%s%}Den Inhalt aus der Datei 3-1-3-5.json (siehe Link oben) in die Zwischenablage kopieren und in das Textfeld von "Apply" einfügen und Button "Perform Operations" drücken{%ends%}


## Aufgabe 6: Daten exportieren

OpenRefine bietet viele Möglichkeiten die Daten in verschiedene Formate zu exportieren. Wir verwenden das Format TSV, das sehr einfach ist und sich später gut in den Suchindex spielen lässt.

* Alle Spalten löschen außer diejenigen, die in Kapitel 7.6 angelegt wurden: id, ISBN, ISSN, Sprache, LCC, DDC, Urheber, Medientyp, Ort, Verlag, Jahr, Datum, Beschreibung, Schlagwoerter, Beitragende, Reihe, Vorgaenger, Nachfolger, Link, Titel
* Export / Tab-separated value

** Als JSON-Datei: ** Als JSON-Datei: [3-1-3-6.json](3-1-3-6.json)****

Zur Prüfung der exportierten Datei können Sie ```csvstat``` verwenden (vgl. [Kapitel 3.1.2, Schritt 2](3-1-2-alle-daten-automatisiert-verarbeiten.md)).


## Literatur

* Owen Stephens: [A worked example of fixing problem MARC data: Part 4 – OpenRefine](http://www.meanboyfriend.com/overdue_ideas/2015/07/worked-example-fixing-marc-data-4/)
* Library Carpentry OpenRefine: [Basic OpenRefine functions II](https://data-lessons.github.io/library-openrefine/04-basic-functions-II/)
* [Exporter in der OpenRefine Dokumentation](https://github.com/OpenRefine/OpenRefine/wiki/Exporters)
