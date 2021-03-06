# 4.1.3 TYPO3-find anpassen

Über das Template im Textfeld ```Setup``` (vgl. den letzten Schritt aus dem vorigen Kapitel) kann TYPO3-find konfiguriert werden.

## Aufgabe: Ergänzen Sie eine weitere Facette

Hinweise:

* Verwenden Sie das Feld ```Jahr``` für die Facette

Literatur:

* Dokumentation von TYPO3-find: http://typo3-find.readthedocs.io/en/latest/index.html

## Lösung

```
plugin.tx_find.settings {
        connections {
                default {
                        options {
                                host = 127.0.0.1
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
}
```

## Bonus: Darstellung der Trefferliste und Detailseite anpassen

Für die Anpassung der Detailseiten müssen gemäß der Dokumentation von TYPO3-find die Dateien ```Result.html``` und ```Detail.html``` im Ordner ```Resources/Private/Partials/Display/``` der Extension bearbeitet werden.

Die Dateien liegen bei unserer Installation im Ordner ```/var/www/katalog/web/typo3conf/ext/find/Resources/Private/Partials/Display```.
