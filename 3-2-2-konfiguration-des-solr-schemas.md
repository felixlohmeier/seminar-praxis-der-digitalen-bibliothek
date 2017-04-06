# 3.2.2 Konfiguration des Solr Schemas

Ab Solr Version 6.0 ist das sogenannte "managed schema" (auch "schemaless mode" genannt) voreingestellt. Solr analysiert bei der Indexierung die Daten und versucht das Schema selbst zu generieren. Felder können aber weiterhin zusätzlich manuell definiert werden.

## Aufgabe: Schema über Admin-Oberfläche konfigurieren

Hinweise:

* Prinzipiell muss für alle Spalten in den TSV-Daten ein Feld im Schema definiert werden.
* Im folgenden Abschnitt werden wir die Daten in Solr indexieren. Dabei erkennt Solr die allermeisten Felder automatisch. Es müssen nur die Felder ```ISBN``` und ```DDC``` manuell definiert werden, weil die automatische Erkennung hier Fehler produziert. Alle anderen Felder sollte Solr automatisch erkennen. Wenn Sie lieber auf Nummer sicher gehen wollen, dann legen Sie alle Felder manuell an.
* Anlegen von Feldern: Admin-Oberfläche aufrufen. Im Menü "Core Selector" den Index "gettingstarted" auswählen. Dann im zweiten Menü "Schema" aufrufen.
* Groß- und Kleinschreibung ist wichtig.

## Lösung

Minimal:

* Administrationsoberfläche: {%s%}http://localhost:8983/solr/#/gettingstarted/schema{%ends%}
* Feld ISBN ergänzen: {%s%}Button "Add Field" drücken, ISBN in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld DDC ergänzen: {%s%}Button "Add Field" drücken, DDC in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}

Vollständig:

* Feld Sprache ergänzen: {%s%}Button "Add Field" drücken, Sprache in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld LCC ergänzen: {%s%}Button "Add Field" drücken, LCC in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld ISSN ergänzen: {%s%}Button "Add Field" drücken, ISSN in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Urheber ergänzen: {%s%}Button "Add Field" drücken, Urheber in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Titel ergänzen: {%s%}Button "Add Field" drücken, Titel in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Medientyp ergänzen: {%s%}Button "Add Field" drücken, Medientyp in das Feld name eingeben, als field type "string" auswählen und NICHT als multiValued markieren{%ends%}
* Feld Ort ergänzen: {%s%}Button "Add Field" drücken, Ort in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Verlag ergänzen: {%s%}Button "Add Field" drücken, Verlag in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Jahr ergänzen: {%s%}Button "Add Field" drücken, Jahr in das Feld name eingeben, als field type "TrieLong" auswählen und NICHT als multiValued markieren{%ends%}
* Feld Datum ergänzen: {%s%}Button "Add Field" drücken, Datum in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Beschreibung ergänzen: {%s%}Button "Add Field" drücken, Beschreibung in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Schlagwoerter ergänzen: {%s%}Button "Add Field" drücken, Schlagwoerter in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Beitragende ergänzen: {%s%}Button "Add Field" drücken, Beitragende in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Reihe ergänzen: {%s%}Button "Add Field" drücken, Reihe in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Vorgaenger ergänzen: {%s%}Button "Add Field" drücken, Vorgaenger in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Nachfolger ergänzen: {%s%}Button "Add Field" drücken, Nachfolger in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}
* Feld Link ergänzen: {%s%}Button "Add Field" drücken, Link in das Feld name eingeben, als field type "string" auswählen und als multiValued markieren{%ends%}

## Literatur

* [Einführungsartikel zu "Managed Schema"](https://support.lucidworks.com/hc/en-us/articles/221618187-What-is-Managed-Schema-)
* [Einführungsartikel zur Definition von Feldern im Schema](http://www.solrtutorial.com/schema-xml.html)
