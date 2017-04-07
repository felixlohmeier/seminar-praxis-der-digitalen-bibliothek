# 3.2.1 Installation von Solr

## Installation

* Geben Sie im Terminal folgende Befehle ein:

```
wget http://archive.apache.org/dist/lucene/solr/6.5.0/solr-6.5.0.tgz
tar zxf solr-6.5.0.tgz
```

siehe auch: [Offizielle Installationsanleitung](https://cwiki.apache.org/confluence/display/solr/Installing+Solr)


## Solr mit Beispielkonfiguration starten

* Geben Sie im Terminal folgenden Befehl ein, um Solr mit der Beispielkonfiguration "schemaless" zu starten:

```
~/solr-6.5.0/bin/solr -e schemaless
```

* Laden Sie anschließend ein paar mitgelieferte Beispieldaten, damit in der integrierten Suchoberfläche schon einmal etwas zu sehen ist:

```
~/solr-6.5.0/bin/post -c gettingstarted ~/solr-6.5.0/example/exampledocs/books.csv
```

siehe auch: [Offizielle Anleitung "Running Solr"](https://cwiki.apache.org/confluence/display/solr/Running+Solr)


## Administrationsoberfläche

Nach einer kurzen Wartezeit (max. 1 Minute) sollten folgende Oberflächen im Browser (nur innerhalb der virtuellen Maschine! Menü Anwendungen/Internet/Firefox Web Browser) erreichbar sein:

* Administrationsoberfläche: http://localhost:8983/
* Integrierte Suchoberfläche: http://localhost:8983/solr/gettingstarted/browse
