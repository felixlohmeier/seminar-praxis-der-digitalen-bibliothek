# 4.1.1 Installation von TYPO3

Für Ubuntu gibt es derzeit kein Installationspaket, daher müssen wir die für TYPO3 benötigten Komponenten (Webserver, Datenbank, PHP) manuell installieren.

## Benötigte Pakete (Apache, MySQL, PHP) installieren

```
sudo apt-get install apache2 libapache2-mod-php7.0 php7.0 php7.0-mysql mysql-server php-gd php-json php-imagick php-mbstring php-curl php-apcu php-soap php-xml php-zip
```

Während der Installation müssen Sie ein Root-Passwort für MySQL vergeben. Denken Sie sich eins aus und notieren Sie dies.

## Konfiguration MySQL

Wenn die Installation abgeschlossen ist, müssen wir eine Datenbank und eine/n Nutzer/in anlegen:
* ```mysql -u root -p```
* MySQL-root-Passwort eingeben
* Anschließend folgende Befehle eingeben (_secretpassword_ durch ein eigenes Passwort ersetzen):
```
CREATE DATABASE typo3 DEFAULT CHARACTER SET utf8;
CREATE USER typo3_db_user@localhost IDENTIFIED BY 'secretpassword';
GRANT ALL PRIVILEGES ON typo3.* TO typo3_db_user@localhost;
FLUSH PRIVILEGES;
quit;
```

## Konfiguration PHP

Optimieren Sie die Einstellungen von PHP für TYPO3:
```
sudo sed -i 's/max_execution_time = 30/max_execution_time = 240/' /etc/php/7.0/apache2/php.ini
sudo sed -i 's/; max_input_vars = 1000/max_input_vars = 1500/' /etc/php/7.0/apache2/php.ini
sudo sed -i 's/upload_max_filesize = 2M/upload_max_filesize = 8M/' /etc/php/7.0/apache2/php.ini
```

Abschließend ist ein Neustart des Webservers erforderlich:
```
sudo /etc/init.d/apache2 restart
```

## TYPO3 installieren

Geben Sie folgende Befehle ins Terminal ein, um die neueste Version von TYPO3 7 zu installieren.

```
cd /var/www/
sudo wget get.typo3.org/7 --content-disposition
sudo tar xzf typo3_src-7*
sudo rm -f typo3_src-7*.tar.gz
sudo chown root:www-data -R html
sudo chmod 775 html
cd html
sudo ln -s $(find .. -name typo3_* -type d) typo3_src
sudo ln -s typo3_src/typo3 typo3
sudo ln -s typo3_src/index.php index.php
sudo rm index.html
sudo touch FIRST_INSTALL
```

## Konfiguration von TYPO3 mit dem Installationsassistent

Nach der Installation erreichen Sie TYPO3 unter der Adresse http://localhost. Dort treffen Sie zunächst auf den Installationsassistenten.
* In Schritt 2 muss als Username ```typo3_db_user``` und das von Ihnen für den Nutzer typo3_db_user gesetzte Passwort (secretpassword) eingetragen werden.
* In Schritt 3 wählen Sie "use an existing empty database"
* In Schritt 4 müssen Sie einen weiteren Account anlegen, diesmal für die Administration von TYPO3. Notieren Sie sich Benutzername und Passwort.
* Wählen Sie in Schritt 5 die Option ```Yes, download the list of distributions.``` und installieren Sie nach dem Login das Paket ```The official Introduction Package```
