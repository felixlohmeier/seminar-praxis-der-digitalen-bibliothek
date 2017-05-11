# 4.1.2 Erweiterung TYPO3-find installieren

Bevor wir die Erweiterung TYPO3-find installieren und konfigurieren, starten wir zunächst erneut den Suchindex Solr (vgl. Kapitel 3.2.3):
```
~/solr-6.5.0/bin/solr start -s ~/solr-6.5.0/example/schemaless/solr
```

Die Administrationsoberfläche von TYPO3 ist unter folgender URL verfügbar: http://localhost/typo3/.

## Menü Extensions

* In der Liste neben dem Eintrag ```Find``` auf den Würfel klicken, um die Extension zu aktivieren

## Menü Page

* Seite ```Home``` auswählen
* Button +Content in Spalte "Normal" drücken und im Reiter ```Plugins``` das Plugin ```TYPO3 Find``` auswählen
* Oben den Save-Button betätigen.

## Menü List

* Gleiche Seite auswählen, auf der vorhin das Plugin eingefügt wurde (müsste noch vorausgewählt sein)
* Das Template ```Main TypoScript Rendering``` bearbeiten
* Reiter ```General```: In Textfeld ```Setup``` den vorhandenen Inhalt durch Folgendes ersetzen
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
        }
}
```
* Reiter ```Includes```: Rechts bei ```available items``` das Item ```Find (find)``` anklicken.
* Oben den Save-Button betätigen

Rufen Sie anschließend die Webseite http://localhost auf. Der Katalog sollte erscheinen.
