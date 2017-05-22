# 5.2 Relevanzranking mit TYPO3-find und Solr

TYPO3-find basiert auf dem Suchindex Solr, weshalb für das Relevanzranking die Konfigurationsmöglichkeiten von Solr genutzt werden können. Eine Verbesserung der Relevanzsortierung von Trefferlisten kann in der Praxis bereits durch eine einfache Gewichtung der Felder erzielt werden.

* Relevanz ist abhängig vom Nutzungskontext, d.h. von der Person, die sucht und von ihrem Erkenntnisinteresse zum jeweiligen Zeitpunkt.
* Discovery-Systeme versuchen eine subjektiv als "gut" empfundene Relevanzsortierung durch eine unterschiedliche Gewichtung der verschiedenen Metadatenfelder (Titel, UrheberIn, Beschreibungstext) herzustellen.
* Wenn die Daten uneinheitlich sind (z.B. wenn zu einem Objekt viele und zu einem anderen Objekt sehr wenige beschreibende Daten vorliegen), dann führt dies oft zu unerwarteten Rankings, weil der Suchindex in der Standardkonfiguration das Verhältnis der Suchtreffer in einem Dokument zur Gesamtlänge des Dokuments in die Berechnung der Relevanzsortierung einfließen lässt.
* Weil die Definition eines Algorithmus auf Basis von objektiven Kriterien so schwer fällt, wird in der Praxis die Gewichtung der Felder oft experimentell auf Basis von häufig durchgeführten Suchen austariert. Nutzerstudien sind beim Relevanzranking also besonders wichtig.

## 1. Solr Schema anpassen und neu indexieren

Die Suchabfragen werden in Solr durch einen Parser interpretiert. Der voreingestellte Parser erlaubt keine einfache Gewichtung von Feldern, daher nutzen wir den sogenannten "eDismax" Parser.

Weil dieser nicht mit Feldern funktioniert, die als "string" konfiguriert sind, müssen wir zunächst das Schema ändern. Dafür nutzen wir die [Schema API](https://cwiki.apache.org/confluence/display/solr/Schema+API).

### 1.1 Solr starten

```
~/solr-6.5.0/bin/solr start -s ~/solr-6.5.0/example/schemaless/solr
```

### 1.2 Schema ändern

```
curl -X POST -H 'Content-type:application/json' --data-binary '{
  "replace-field":[
     { "name":"ISBN",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"DDC",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Datum",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"LCC",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"ISSN",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Urheber",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Titel",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Medientyp",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": false },
     { "name":"Sprache",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Ort",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Verlag",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Jahr",
       "type":"tlong",
       "stored":true,
       "indexed": true,
       "docValues": false,
	   "multiValued": false },
     { "name":"Beschreibung",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Schlagwoerter",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Beitragende",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Reihe",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Vorgaenger",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Nachfolger",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true },
     { "name":"Link",
       "type":"text_general",
       "stored":true,
       "indexed": true,
	   "multiValued": true }]
}' http://localhost:8983/solr/gettingstarted/schema
```

### 1.3 Core neu laden, damit die Konfigurationsänderung wirksam wird

```
curl "http://localhost:8983/solr/admin/cores?action=RELOAD&core=gettingstarted"
```

### 1.4 Index leeren

```
curl "http://localhost:8983/solr/gettingstarted/update?commit=true&stream.body=%3Cdelete%3E%3Cquery%3E*%3A*%3C/query%3E%3C/delete%3E"
```

### 1.5 Neu indexieren (dauert 5-10 Minuten...)

```
curl "http://localhost:8983/solr/gettingstarted/update/csv?commit=true&separator=%09&f.ISBN.split=true&f.ISBN.separator=%E2%90%9F&f.ISSN.split=true&f.ISSN.separator=%E2%90%9F&f.Sprache.split=true&f.Sprache.separator=%E2%90%9F&f.LCC.split=true&f.LCC.separator=%E2%90%9F&f.DDC.split=true&f.DDC.separator=%E2%90%9F&f.Urheber.split=true&f.Urheber.separator=%E2%90%9F&f.Ort.split=true&f.Ort.separator=%E2%90%9F&f.Verlag.split=true&f.Verlag.separator=%E2%90%9F&f.Datum.split=true&f.Datum.separator=%E2%90%9F&f.Beschreibung.split=true&f.Beschreibung.separator=%E2%90%9F&f.Schlagwoerter.split=true&f.Schlagwoerter.separator=%E2%90%9F&f.Beitragende.split=true&f.Beitragende.separator=%E2%90%9F&f.Reihe.split=true&f.Reihe.separator=%E2%90%9F&f.Vorgaenger.split=true&f.Vorgaenger.separator=%E2%90%9F&f.Nachfolger.split=true&f.Nachfolger.separator=%E2%90%9F&f.Link.split=true&f.Link.separator=%E2%90%9F&f.Titel.split=true&f.Titel.separator=%E2%90%9F" --data-binary @hsh-ksf.tsv -H 'Content-type:text/plain; charset=utf-8'
```

## 2. TYPO3-Find Konfiguration erweitern

Auch hier muss der Parser eDismax in der Konfiguration aktiviert werden. Rufen Sie dazu die TYPO3-Administrationsoberfläche in der virtuellen Maschine auf http://localhost/typo3/ und wählen Sie im Menü List / Seite "Home" das Template "Main TypoScript Rendering". Ersetzen Sie dort den vorhandenen Eintrag im Textfeld Setup durch folgenden Abschnitt und drücken Sie anschließend den Save-Button:

```
page = PAGE
page.100 < styles.content.get
page.javascriptLibs.jQuery = 1
page.includeJS.find = EXT:find/Resources/Public/JavaScript/find.js
plugin.tx_find.features.requireCHashArgumentForActionArguments = 0
plugin.tx_find.settings {
      connections {
              default {
                      options {
                              host = localhost
                              port = 8983
                              path = /solr/gettingstarted
                      }
              }
      }
      standardFields {
              title = Titel
              snippet = Urheber
      }
      facets {
              10 {
                      id = Medientyp
                      field = Medientyp
                      sortOrder = count
              }
              20 {
                      id = Sprache
                      field = Sprache
                      sortOrder = count
              }
              30 {
                      id = Jahr
                      field = Jahr
                      sortOrder = count
              }
      }
      features {
              eDisMax = 1
      }
}
```

## 3. Relevanzranking einstellen

Im folgenden Befehl sind alle Felder mit Faktor 1 gewichtet. Variieren Sie die Zahlen und entfernen Sie Felder, die Sie aus dem Relevanzranking ausschließen wollen. Die Änderungen werden erst wirksam, wenn Sie den Index neu laden. Prüfen Sie die Ergebnisse im Katalog, machen Sie Beispielsuchen und nähern Sie sich Stück für Stück einem effektiven Relevanzranking an.

### 3.1 Konfiguration ändern

```
curl http://localhost:8983/solr/gettingstarted/config -H 'Content-type:application/json'  -d '{
  "update-requesthandler": {
    "name": "/select",
    "class":"solr.SearchHandler",
    "defaults":{
      "echoParams":"explicit",
      "rows":"10"
    },
    "appends":{
      "defType":"edismax",
      "qf":"ISBN^1 DDC^1 Datum^1 LCC^1 ISSN^1 Urheber^1 Titel^1 Medientyp^1 Sprache^1 Ort^1 Verlag^1 Jahr^1 Beschreibung^1 Schlagwoerter^1 Beitragende^1 Reihe^1 Vorgaenger^1 Nachfolger^1 Link^1"
    }
  }
}'
```

### 3.2 Core neu laden, damit die Konfigurationsänderung wirksam wird

```
curl "http://localhost:8983/solr/admin/cores?action=RELOAD&core=gettingstarted"
```

### 3.3 Ergebnis prüfen

http://localhost

## Literatur

* Präsentation von Elmar Haake: Relevanzranking als Erfolgsfaktor für Discoverysysteme. http://docplayer.org/3530893-Relevanzranking-als-erfolgsfaktor-fuer-discoverysysteme-elmar-haake-staats-und-universitaetsbibliothek-bremen.html
* Hajo Seng: Relevance-Ranking auf dem VuFind-Anwendertreffen (siehe die dort verlinkten Papiere und Vortragsfolien). http://beluga-blog.sub.uni-hamburg.de/blog/2015/10/02/relevance-ranking-auf-dem-vufind-anwendertreffen-2015/
* Solr/Lucene Score Tutorial: http://www.openjems.com/solr-lucene-score-tutorial/
* The Extended DisMax Query Parser: https://cwiki.apache.org/confluence/display/solr/The+Extended+DisMax+Query+Parser
