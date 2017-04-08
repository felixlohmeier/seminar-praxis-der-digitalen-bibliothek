# 4.1.2 Erweiterung TYPO3-find installieren

Normalerweise können Erweiterungen für TYPO3 ganz einfach über die Administrationsoberfläche installiert werden, so wie bei einem App Store. Die EntwicklerInnen legen ihre Erweiterungen dazu im offiziellen TYPO3 Extension Repository ab. Leider ist zum Zeitpunkt der Erstellung des Skripts (April 2017) von der Erweiterung TYPO3-find nur eine uralte Version im Extension Repository verfügbar. Dort liegt Version 1.0.1 (Nov 2013), während bei GitHub Version 3.1.1 (Jan 2017) zur Verfügung steht.

## Schritt 1: Installation von TYPO3-find aus Quellcode bei GitHub

Wir installieren den Code aus dem GitHub-Repository https://github.com/subugoe/typo3-find

### 1.1 Code aus GitHub Repository herunterladen

```
cd ~
wget https://github.com/subugoe/typo3-find/archive/master.zip
unzip master.zip
cd typo3-find-master
```

### 1.2 Abhängigkeiten der TYPO3-Erweiterung mit Composer nachladen

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
php composer.phar install
```

### 1.3 Daten in Extension-Verzeichnis kopieren

```
sudo mkdir /var/www/html/typo3/ext
sudo mkdir /var/www/html/typo3/ext/find
sudo cp -r . /var/www/html/typo3/ext/find
sudo chown :www-data -R /var/www/html/typo3/ext
sudo chmod 775 ext
```

## Schritt 2: Extension in Administrationsoberfläche aktivieren

Login in Administrationsoberfläche unter http://localhost/typo3 (localhost durch IP-Adresse ersetzen)

### 2.1 Menü Extensions

* Neben Extension ```Find``` auf den Würfel klicken, um die Extension zu aktivieren

### 2.2 Menü Page

* Seite auswählen, auf welcher der Katalog eingefügt werden soll, hier ```Congratulations``` (Startseite)
* Folgende Kästen löschen (Mülleimer-Symbol): "Start browsing", "Example Pages", "Test the CMS", "Divider", "Make it your own"
* Button +Content in Spalte "Normal" drücken und im Reiter ```Plugins / General Plugin``` unten auf der Seite im Punkt ```Selected Plugin``` die Erweiterung ```Find``` auswählen und anschließend oben den Save-Button betätigen.

### 2.3 Menü Template

* Gleiche Seite auswählen, auf der vorhin das Plugin eingefügt wurde (müsste noch vorausgewählt sein)
* Im Pulldown oben ```Info/Modify``` auswählen
* Button ```Edit the whole template record```
* Reiter ```Includes```: Rechts bei ```available items``` die Option ```Find (find)``` anklicken
* Oben den Save-Button betätigen
* Reiter ```General```: In Textfeld ```Setup``` Folgendes einfügen

```
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
