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

Das Script benötigt eine Reihe von Parametern, darunter das Quellverzeichnis, das Verzeichnis mit den Transformationsdateien und das Zielverzeichnis. Bei der Verarbeitung von XML Dateien ist zusätzlich das Format und der XML-Pfad (analog zum Klick in der GUI beim Erstellen der Projekte) anzugeben. Die abschließenden Parameter -m und -R sind technischer Natur und sorgen dafür, dass OpenRefine bis zu 3GB Arbeitsspeicher verwenden darf und unnötige Neustarts vermieden werden.

Geben Sie den folgenden Befehl im Terminal ein:

```
./openrefine-batch.sh -a input -b config -c output -f xml -i recordPath=zs:searchRetrieveResponse -i recordPath=zs:records -i recordPath=zs:record -i recordPath=zs:recordData -i recordPath=record -m 3G -R
```

Das Script lädt zunächst OpenRefine und den Python-Client in einen Unterordner und führt dann für jede Datei die Transformation aus und speichert die verarbeiteten Dateien im Zielverzeichnis im Format TSV.


## Schritt 2: Spalten einheitlich sortieren (und nicht benötigte MARC-Felder löschen)

Prinzipiell könnten die TSV-Daten jetzt auch direkt in OpenRefine geladen werden, aber die Spalten wären dann nicht alphabetisch sortiert und es wären auch zahlreiche Spalten enthalten, die nur extrem selten (weniger als 10 Werte in 580.000 Datensätzen) belegt sind. Das macht die Arbeit mit den Daten unübersichtlich und langsam. Also sollten wir als erstes nicht benötigte Spalten löschen und die Spalten sortieren.

Wir nutzen dazu die Software [csvkit](https://csvkit.readthedocs.io).

Installation:
* ```sudo apt-get install python-dev python-pip python-setuptools build-essential```
* ```pip install csvkit```

Dateien zusammenführen:
* ```csvstack -t *.tsv > stacked.csv```

Spalten sortieren:
* ```columns=$(head -n1 stacked.csv | sort)```
* ```csvcut -c $columns stacked.csv > sorted.csv```

Spalten mit sehr vielen leeren Werten ausgeben:
* ```csvstat --nulls```
* Sie können die [Dokumentation des MARC21 Formats](http://www.dnb.de/DE/Standardisierung/Formate/MARC21/marc21_node.html) konsultieren, um zu prüfen, ob Sie die Informationen aus den Spalten mit sehr vielen (>570.000) Null-Werten wirklich benötigen.

Nicht benötigte Spalten löschen (hier ein Vorschlag):
* ```csvcut -C spalte1,spalte2 -x > hsh-ksf.csv```


## Schritt 3: Daten in OpenRefine laden

Für das Laden der gesamten rund 580.000 Datensätze werden etwa XXX GB freier Arbeitsspeicher benötigt.

Starten Sie OpenRefine mit dem zusätzlichen Parameter ```-m 4G```, damit OpenRefine über mehr Speicher verfügen kann:
```
~/openrefine-2.7-rc.2/refine -m 4G
```

Sollten Sie auf Ihrer virtuellen Maschine nicht über genügend freien Arbeitsspeicher verfügen, dann reduzieren Sie den Wert im Parameter ```-m``` und laden Sie nur einen Teil der Daten.

Erstellen Sie ein neues Projekt und laden Sie die im vorigen Schritt präparierte CSV-Datei ```hsh-ksf.csv``` hoch. Damit haben Sie nun endlich alle Daten in einem einzelnen Projekt und die Spalten alphabetisch sortiert. Das ist die Grundlage, auf der das nächste Kapitel aufsetzt.
