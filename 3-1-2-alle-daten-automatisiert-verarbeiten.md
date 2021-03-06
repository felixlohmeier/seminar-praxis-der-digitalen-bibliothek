# 3.1.2 Alle Daten automatisiert verarbeiten

Die Systemarchitektur von OpenRefine macht es möglich, die Anwendung nicht nur über die grafische Oberfläche, sondern auch über eine API "fernzusteuern". Für die [HTTP-API von OpenRefine](https://github.com/OpenRefine/OpenRefine/wiki/OpenRefine-API) gibt es Clients in den Programmiersprachen Python, Ruby, node.js, PHP und für R. Am ausgereiftesten ist der [Python-Client von Paul Makepeace](https://github.com/PaulMakepeace/refine-client-py/).

Darauf aufbauend habe ich ein Shell-Script geschrieben, das gespeicherte Transformationsregeln auf eine Vielzahl von Dateien anwenden kann. Es lädt sich alle benötigten Komponenten wie beispielsweise den Python-Client automatisch aus dem Internet. Die zu verarbeitenden Daten und die Transformationsregeln (als JSON-Dateien) müssen vorab in Dateiordnern bereitgestellt werden. Das Script ist bei GitHub frei verfügbar: [felixlohmeier/openrefine-batch](https://github.com/felixlohmeier/openrefine-batch).

## Schritt 1: Alle Daten automatisiert verarbeiten

Wir wenden jetzt die Transformationsregeln aus Kapitel 2.3.4 und 2.3.5 auf alle(!) MARCXML-Dateien an. Dazu sind folgende Teilschritte notwendig.

### 1.1 Download des Shell-Scripts

* Falls OpenRefine im Terminal noch läuft, beenden Sie es durch die Tastenkombination ```STRG```+```C``` und schließen Sie das Terminal.
* Öffnen Sie jetzt die Dateiverwaltung (Startmenü/Anwendungen/Systemwerkzeuge/Caja oder einen Ordner auf dem Desktop doppelklicken) und navigieren Sie in das Verzeichnis in dem der Ordner "download" mit den in [Kapitel 2.2.3](2-2-3-download-der-metadaten.md) heruntergeladenen MARCXML-Dateien liegt. Klicken Sie mit der rechten Maustaste in das Fenster und wählen Sie den Punkt "im Terminal öffnen" im Kontextmenü. Im Terminal befinden Sie sich jetzt im gewünschten Ordner, der entsprechende Pfad steht vor dem Dollarzeichen.
* Geben Sie im Terminal folgenden Befehl ein:

```wget https://github.com/felixlohmeier/openrefine-batch/raw/master/openrefine-batch.sh && chmod +x openrefine-batch.sh```

### 1.2 Zielverzeichnis anlegen

* Ordner anlegen: ```mkdir output```

### 1.3 Transformationsregeln bereitstellen

* Ordner anlegen: ```mkdir config```
* Transformationsregeln in Ordner config herunterladen: ```wget https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek/raw/master/openrefine/2-3-5.json -O config/2-3-5.json``` (alternativ könnten Sie auch Ihre selbst zwischengespeicherten Transformationsregeln verwenden und diese als Datei mit der Dateiendung ".json" in dem Ordner config speichern)

### 1.4 Quelldateien bereitstellen

Wenn Sie allen Schritten im Skript gefolgt sind, dann sollten die MARCXML-Dateien im Unterordner download liegen. Sie können das mit dem Befehl ```ls``` prüfen:
* Der Befehl ```ls``` sollte folgende Dateien und Verzeichnisse ausgeben: ```config  download  openrefine-batch.sh  output```
* Der Befehl ```ls download``` sollte nach einem Moment alle MARCXML-Dateien ausspucken.

Da es sehr zeitaufwendig ist, so viele kleine Einzeldateien zu verarbeiten, erstellen wir zunächst Zip-Archive mit jeweils 100 Dateien und stellen diese im Ordner input bereit:

```
find download/ -type f -name '*.xml' > marcxml
split -l 100 marcxml marcxml-
rm -f marcxml
for i in marcxml*; do cat $i | zip $i -@; done
mkdir input
mv marcxml*.zip input/
rm -f marcxml* 
```

### 1.5 Automatische Verarbeitung starten

Das Script benötigt die eine Reihe von Parametern, darunter das Quellverzeichnis, das Verzeichnis mit den Transformationsdateien und das Zielverzeichnis. Bei der Verarbeitung von XML Dateien ist zusätzlich das Format und der XML-Pfad (analog zum Klick in der GUI beim Erstellen der Projekte) anzugeben. Die abschließenden Parameter -m und -R sind technischer Natur und sorgen dafür, dass OpenRefine bis zu 3GB Arbeitsspeicher verwenden darf und unnötige Neustarts vermieden werden.

Geben Sie den folgenden Befehl im Terminal ein (läuft mehrere Stunden):

```
./openrefine-batch.sh -a input -b config -c output -f xml -i recordPath=zs:searchRetrieveResponse -i recordPath=zs:records -i recordPath=zs:record -i recordPath=zs:recordData -i recordPath=record -m 3G -R
```

Das Script lädt zunächst OpenRefine und den Python-Client in einen Unterordner und führt dann für jede Datei die Transformation aus und speichert die verarbeiteten Dateien im Zielverzeichnis im Format TSV (Tabellendokument mit Tabs als Trennzeichen).


## Schritt 2: Spalten einheitlich sortieren (und nicht benötigte MARC-Felder löschen)

Schauen Sie sich die ersten Zeilen der TSV-Dateien mit ```head -n1 *.tsv``` an. Die verschiedenen Pakete enthalten unterschiedliche Spalten und sie sind in unterschiedlicher Reihenfolge sortiert. Mit dem Befehl ```head -q -n1 *.tsv | tr "\t" "\n" | sort | uniq -c``` könnten Sie sich einen Überblick darüber verschaffen, wie oft eine Spalte in den verschiedenen TSV-Dateien vorkommt. Leider sind die Daten uneinheitlich codiert, so dass sehr viele unterschiedliche MARC-Felder belegt sind. Die daraus resultierende hohe Anzahl an Spalten stellt hohe Leistungsanforderungen an OpenRefine. Der Arbeitsspeicher wird vermutlich nicht ausreichen, um alle Daten in ein Projekt zu laden. Führen Sie die folgenden Schritte aus, um die Spalten einheitlich zu sortieren und die Anzahl der Felder zu reduzieren.

### 2.1 Anzahl der Werte pro MARC-Feld zählen

Download des Shell-Scripts:
```
wget https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek/raw/master/scripte/count-tsv.sh && chmod +x count-tsv.sh
```

Script starten (läuft mehrere Stunden):
```
./count-tsv.sh output/*.tsv | tee felder.tsv
```

Das Script gibt Ihnen für jede Datei nach und nach die Belegung aller enthaltenen Felder aus. Sie werden feststellen, dass viele Felder kaum belegt sind. Die dritte Spalte gibt an, wie häufig das Feld mehrfachbelegt ist (d.h. wie häufig das Zeichen ```␟``` vorkommt, das wir in Kapitel 2.3.5 als Trennzeichen für mehrfach belegte Felder festgelegt haben). Der letzte Teil des Befehls (```tee felder.tsv```) sorgt dafür, dass die Ergebnisse zusätzlich zur Ausgabe auf der Kommandozeile in der Datei "felder.tsv" gespeichert wurden.

### 2.2 Datei felder.tsv in OpenRefine öffnen

Erstellen Sie ein neues Projekt in Openrefine und laden Sie die Datei felder.tsv mit Standardeinstellungen hoch. 

Summen bilden:
* Spalte MARC-Feld / Sort / Sort... OK 
* Im neuen Menü "Sort" Reorder rows permanently wählen
* Spalte MARC-Feld / Edit cells / Blank down
* Spalte Vorkommen / Edit cells / Join multi-valued cells
* Spalte Vorkommen / Edit cells / Transform... forEach(value.split(','),v,v.toNumber()).sum()
* Spalte Mehrfachbelegung / Edit cells / Join multi-valued cells
* Spalte Mehrfachbelegung / Edit cells / Transform... forEach(value.split(','),v,v.toNumber()).sum()

Bei rund 580.000 Datensätzen können wir vermutlich diejenigen Felder vernachlässigen, die weniger als 100x belegt sind:
* Spalte Vorkommen / Facet / Customized Facet / Numeric log facet ... Regler auf 0.30 — 2.00 einstellen
* Spalte All / Edit rows / Remove all matching rows
* Facette schließen

### 2.3 Transformationsdatei für OpenRefine generieren

Wenn Sie die Funktion ```All / Edit Columns / Re-order / remove columns...``` über die grafische Oberfläche durchführen und anschließend die Funktion ```Undo / Redo / Extract ...``` aufrufen, können Sie sich anschauen, wie die Transformationsregel für diese Funktion in JSON definiert ist. Diese ist sehr einfach aufgebaut und sieht so aus (in diesem Beispiel werden nur die Spalten A, B, C erhalten):

```
[
  {
    "op": "core/column-reorder",
    "description": "Reorder columns",
    "columnNames": [
       "A",
       "B",
       "C"
    ]
  }
]
```

Das ermöglicht es uns die Transformationsdatei mit der Templating-Funktion von OpenRefine zu generieren:
* Löschen Sie alle nicht benötigten MARC-Felder (Zeilen). Je weniger Felder enthalten sind, desto übersichtlicher wird die weitere Bearbeitung in den folgenden Kapiteln und desto geringer wird der Bedarf an Arbeitsspeicher. Als Ausgangspunkt können Sie die Basisfelder in [Dublin Core (unqualified)](http://www.loc.gov/marc/marc2dc.html) heranziehen.
* Rufen Sie im Menü "Export" den Punkt Templating auf
* Geben Sie Folgendes in die Felder ein...

Prefix:
```
[
  {
    "op": "core/column-reorder",
    "description": "Reorder columns",
    "columnNames": [
```

Row Template:
```
       {{jsonize(cells["MARC-Feld"].value)}}
```

Row Separator (unverändert):
```
,
```

Suffix:
```

    ]
  }
]
```

Klicken Sie auf Export und speichern Sie die Datei im selben Ordner, in dem auch das Script openrefine-batch.sh liegt. Ändern Sie die Dateiendung in "json", z.B. mit folgendem Befehl: ```mv felder-tsv.txt felder-tsv.json```

Alternativ können Sie die folgenden vorbereiteten Dateien verwenden. Hier sind zwei Beispielkonfigurationen:

1. Alle Felder: [3-1-2_all.json](openrefine/3-1-2_all.json)
2. Feldauswahl auf Basis des Zielschemas [Dublin Core (unqualified)](http://www.loc.gov/marc/marc2dc.html): [3-1-2_minimal.json](openrefine/3-1-2_minimal.json)

### 2.4 Dateien erneut mit OpenRefine automatisiert verarbeiten

Geben Sie die folgenden Befehle im Terminal ein:

```
rm -f input/*
mv output/*.tsv input/
rm -rf output
rm -f config/*
wget https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek/raw/master/openrefine/3-1-2_minimal.json -O config/3-1-2_minimal.json
./openrefine-batch.sh -a input -b config -c output -m 3G -R
```

## Schritt 3: Daten in OpenRefine laden

OpenRefine führt unterschiedliche Datenstrukturen sinnvoll zusammen. Wenn die Dateien unterschiedlich viele Spalten oder eine andere Reihenfolge der Spalten haben, so ist das kein Problem. OpenRefine nimmt alle Spalten der ersten Datei auf und belegt diese mit neuen Zeilen. Sobald in einer weiteren Datei eine neue Spalte auftaucht, die OpenRefine noch nicht bekannt ist, so wird diese hinten angehängt.

Für das Laden der gesamten rund 580.000 Datensätze werden etwa 5 GB freier Arbeitsspeicher benötigt. Starten Sie OpenRefine mit dem zusätzlichen Parameter ```-m 5G```, damit OpenRefine über mehr Speicher verfügen kann.

```
~/openrefine-2.7-rc.2/refine -m 5G
```

Sollten Sie auf Ihrer virtuellen Maschine nicht über genügend freien Arbeitsspeicher verfügen, dann reduzieren Sie den Wert im Parameter ```-m``` und laden Sie nur einen Teil der Daten.

* OpenRefine beenden und alle Fenster schließen. Freien Arbeitsspeicher mit ```free -m``` anzeigen und Wert in Zeile "-/+ buffers/cache" in Spalte "free" notieren und auf Gigabyte abrunden.
* Zahl bei -m 3G im Startbefehl anpassen. Beispiel für 1 GB freien Arbeitsspeicher: -m 1G

Erstellen Sie ein neues Projekt und laden Sie die im vorigen Schritt erstellten TSV-Dateien aus dem Ordner ```output``` hoch.
* Create Project / Durchsuchen... / TSV Dateien auswählen / Next / Configure Parsing Options
* Parse data as CSV / TSV / separator-based files
* Checkbox "Store file source..." deaktivieren
* Projektnamen vergeben (z.B. "hsh-ksf" und Button "Create Project" drücken

Wenden Sie die Transformationsdatei aus dem vorigen Schritt noch einmal an, damit die Spalten erneut und abschließend alphabetisch sortiert werden.
* Menü oben links "Undo / Redo" aufrufen und Button "Apply..." drücken. Den Inhalt aus der Datei [3-1-2_minimal.json](openrefine/3-1-2_minimal.json) in die Zwischenablage kopieren und in das Textfeld von "Apply" einfügen und Button "Perform Operations" drücken.

Damit haben Sie nun endlich alle Daten in einem einzelnen Projekt und die Spalten alphabetisch sortiert. Das ist die Grundlage, auf der das nächste Kapitel aufsetzt.
