# 5.1 Zusätzliche Daten über OAI-Schnittstelle

Über das Protokoll [OAI-PMH](http://www.openarchives.org/OAI/openarchivesprotocol.html) (Open Archives Initiative Protocol for Metadata Harvesting) können Metadaten vieler Repositorien heruntergeladen werden. Eine standardkonforme OAI-Schnittstelle unterstützt mindestens das Ausgabeformat Dublin Core, oft werden zusätzliche Formate wie MARC oder MODS unterstützt. Über weitere Variablen kann die Abfrage nach Datum oder Teilbeständen gesteuert werden.

In dieser Aufgabe laden Sie Daten via OAI-PMH aus einem Repositorium, bearbeiten Sie mit OpenRefine und indexieren die Daten im Suchindex.

Das internationale Forschungsdatenrepository [Zenodo](http://www.zenodo.org) bietet eine gut implementierte OAI-Schnittstelle, die auch über den Browser "erkundet" werden kann:
* Schnittstelle: https://zenodo.org/oai2d?verb=Identify
* Dokumentation: http://developers.zenodo.org/#oai-pmh

Beispiel "European Commission Funded Research (OpenAIRE)":
* In dieser Community liegen etwa 3.800 Dokumente, wie eine Suche zeigt: https://zenodo.org/communities/ecfunded/search?page=1&size=20
* Auf der [Übersichtsseite der Community](https://zenodo.org/communities/ecfunded/) steht ein Link zu Informationen für das Harvesting über OAI-PMH. In der URL selbst sind die Parameter abzulesen, die benötigt werden. Beispiel: https://zenodo.org/oai2d?verb=ListRecords&set=user-ecfunded&metadataPrefix=oai_dc. Daran können Sie ablesen, dass das Set "user-ecfunded" heißt.

## 1. OAI Harvesting

Es gibt sehr viele Clients, um OAI-Schnittstellen abzufragen. VuFind hat beispielsweise einen umfangreichen [OAI-Harvester](https://github.com/vufind-org/vufindharvest) integriert, der auch eigenständig benutzt werden kann. Wir nutzen für diese Aufgabe einen schlanken kommandozeilenbasierten Client, der in Python geschrieben wurde: https://github.com/bloomonkey/oai-harvest. Der Entwickler heißt John Harrison und arbeitet an der Universität von Liverpool.

### 1.1 Installation

```
sudo apt-get install sqlite3 python3-pip
sudo python3 -m pip install git+https://github.com/infrae/pyoai.git
sudo python3 -m pip install git+http://github.com/bloomonkey/oai-harvest.git#egg=oaiharvest
```

### 1.2 Download

```
mkdir zenodo
oai-harvest https://zenodo.org/oai2d -s user-ecfunded -p oai_dc -d zenodo
```

### 1.3 Dateien in Zip-Archiv packen

Die Dateien sollten nun im "Persönlichen Ordner" im Unterverzeichnis "zenodo" liegen. 3800 Einzeldateien in OpenRefine zu laden schlägt leicht fehl, daher packen wir diese Dateien in ein Zip-Archiv und laden dann (im nächsten Schritt) dieses Zip-Archiv in OpenRefine:

```
zip -r zenodo.zip zenodo
```

## 2. Verarbeitung mit OpenRefine

### 2.1 OpenRefine starten

```
~/openrefine-2.7-rc.2/refine
```

### 2.2 Projekt erstellen

Zip-Datei hochladen und bei der Auswahl des Pfads auf die erste Zeile (<oai_dc...>) klicken

### 2.3 Spalten umbenennen

Die Spalten müssen umbenannt werden, so dass sie auf Felder im Schema des Suchindex passen (z.B. "oai_dc:dc - dc:title" in "Titel" umbenennen). Im Schema haben wir unter anderem folgende Bezeichnungen verwendet:

* Titel
* Urheber
* Medientyp
* Sprache
* Jahr
* Link

Für den Import in Solr ist zudem eine Spalte "id" zwingend technisch notwendig. Diese kann aus der Spalte "File" gebildet werden. Weitere mögliche Felder können Sie dem Schema unter http://localhost:8983/solr/#/gettingstarted/schema entnehmen.

### 2.4 Überflüssige Spalten löschen

### 2.5 Mehrfachbelegungen 

* Spalte id (ehemals File) / Blank down
* Spalten mit Mehrfachbelegungen zusammenführen mit Edit cells / Join multi-valued cells.... Wählen Sie dabei ein Trennzeichen, dass nicht in den Daten vorkommt. Am besten das Sonderzeichen Unit Separator ␟
http://unicode-table.com/en/241F/
* Leere Zeilen löschen

### 2.6 Daten exportieren als TSV

* Es sollten jetzt 3805 Zeilen sein.
* Menü Export / Tab-separated value. Datei wird vom Firefox Browser im Ordner Downloads gespeichert.
* OpenRefine beenden: ```STRG```+```C``` im Terminal, in dem OpenRefine läuft

## 3. Daten in Suchindex laden

### 3.1 Solr starten

```
~/solr-6.5.0/bin/solr start -s ~/solr-6.5.0/example/schemaless/solr
```

### 3.2 Daten laden

Wechseln Sie in der Kommandozeile in das Verzeichnis mit den gespeicherten Daten (z.B. mit ```cd Downloads```) und geben Sie dann den folgenden Befehl zum Laden der Daten ein. In dem Befehl muss der Dateiname der zu importierenden Datei angegeben werden, also: ```test.tsv``` durch den Dateinamen der mit OpenRefine exportierten Datei ersetzen.

```
curl "http://localhost:8983/solr/gettingstarted/update/csv?commit=true&separator=%09" --data-binary @test.tsv -H 'Content-type:text/plain; charset=utf-8'
```

Falls Sie eine Fehlermeldung "Error adding field 'Jahr'='2014-09-21'" erhalten, dann liegt das daran, dass in unserem Schema im Feld Jahr nur Nummern zugelassen sind. In diesem Fall müssen Sie das Projekt noch einmal in OpenRefine laden, die Trennzeichen im Feld Jahr entfernen (z.B. mit der Transformation ```value[0,4]``` und die Datei nochmal exportieren.

Wenn Sie mehrfachbelegte Felder aufsplitten wollen, müssen Sie die URL innerhalb der Anführungszeichen (mehrfach) ergänzen. Beispiel für Titel:

```
&f.Titel.split=true&f.Titel.separator=%E2%90%9F
```

Vollständiger Befehl für Beispiel Titel:

```
curl "http://localhost:8983/solr/gettingstarted/update/csv?commit=true&separator=%09&f.Titel.split=true&f.Titel.separator=%E2%90%9F" --data-binary @test.tsv -H 'Content-type:text/plain; charset=utf-8'
```

## 3.3 Ergebnis prüfen

http://localhost in der virtuellen Maschine aufrufen, um den Katalog anzuzeigen

Die Facetten funktionieren nicht, wenn dort Sonderzeichen wie ```:``` oder ```&``` enthalten sind. Diese müssen in OpenRefine entfernt oder ersetzt werden.

Der Befehl zum Leeren des gesamten Suchindex lautet:

```
curl "http://localhost:8983/solr/gettingstarted/update?commit=true&stream.body=%3Cdelete%3E%3Cquery%3E*%3A*%3C/query%3E%3C/delete%3E"
```

Anschließend können Sie die Daten neu indexieren (siehe oben Schritt 3.2)

## Literatur

* Open Archives Initiative Protocol for Metadata Harvesting: http://www.openarchives.org/OAI/openarchivesprotocol.html
* OAI-Schnittstelle der Deutschen Nationalbibliothek: http://www.dnb.de/DE/Service/DigitaleDienste/OAI/oai_node.html
* OAI-Schnittstelle von Zenodo: http://developers.zenodo.org/#oai-pmh
