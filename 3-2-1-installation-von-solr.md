# 3.2.1 Installation von Solr

## Installation

* Geben Sie im Terminal folgende Befehle ein:

```
java -version
wget http://archive.apache.org/dist/lucene/solr/6.5.0/solr-6.5.0.tgz
tar zxf solr-6.5.0.tgz
```

siehe auch: [Offizielle Installationsanleitung](https://cwiki.apache.org/confluence/display/solr/Installing+Solr)

## Solr mit Beispieldaten starten

* Geben Sie im Terminal folgende Befehle ein:

```
cd solr-6.5.0
bin/solr -e techproducts
```

siehe auch: [Offizielle Anleitung "Running Solr"](https://cwiki.apache.org/confluence/display/solr/Running+Solr)


## Administrationsoberfläche

Nach einer kurzen Wartezeit (max. 1 Minute) sollten folgende Oberflächen im Browser erreichbar sein:

* Administrationsoberfläche: http://localhost:8983/
* Integrierte Suchoberfläche: http://localhost:8983/solr/gettingstarted/browse
