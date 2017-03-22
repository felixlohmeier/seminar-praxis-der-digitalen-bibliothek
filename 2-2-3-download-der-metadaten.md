# 2.2.3 Download der Metadaten

Die Verbundzentrale des Bibliotheksverbunds GBV bietet derzeit keinen einfachen Weg an, um regelmäßig vollständige Abzüge der Daten einer Bibliothek zu bekommen. Wenn wir also die Metadaten des Katalogs der Bibliothek im Kurt-Schwitters-Forum eigenständig laden wollen, müssen wir eine der angebotenen öffentlichen Schnittstellen benutzen.

## Wahl des Formats

Von den angebotenen [Formaten](https://www.gbv.de/wikis/cls/UnAPI#Formate) kommen für uns nur PICA+/PICAXML oder MARC21/MARCXML in Frage. In der vorigen Übung sollten Sie festgestellt haben, dass in den alternativen Formaten wie DC, MODS oder ISBD nicht alle Daten enthalten sind. **Wir verwenden MARC21/MARCXML**, weil der Standard wesentlich weiter verbreitet ist als PICA+/PICAXML und somit mehr Tools und Anleitungen dazu verfügbar sind.

Zu beachten ist, dass der Bibliotheksverbund diese Metadaten zuvor aus verschiedenen Quellen zusammengeführt hat, so dass wir hoffen müssen (und immer auch ein bisschen skeptisch sein sollten), dass die vorliegenden Metadaten alle einheitlich und standardkonform erfasst und in das gemeinsame Zielschema MARC21 ohne (großen) Informationsverlust "übersetzt" wurden.

## Wahl der Schnittstelle

Aus der [Dokumentation des GBV](https://www.gbv.de/wikis/cls/Schnittstellen):
> Folgende öffentliche, webbasierten Schnittstellen (APIs, Webservice ...) sind im GBV-Verbundwiki dokumentiert:
> * SRU und Z39.50 für Suche und Abruf kleiner Mengen von Datensätzen
> * unAPI für den Abruf einzelner Datensätze
> * SeeAlso für den Abruf von Links und Empfehlungen
> * DAIA für den Abruf von Verfügbarkeitsinformationen
> * PAIA für Zugriff auf Benutzerkonten

**Wir verwenden die [SRU-Schnittstelle](https://www.gbv.de/wikis/cls/SRU)**, die einen ähnlichen Funktionsumfang wie die Z39.50-Schnittstelle anbietet und etwas moderner ist. Über die SRU-Schnittstelle stehen nur XML-Formate zur Verfügung, so dass wir im Folgenden mit **MARCXML** arbeiten werden.

## Aufgabe 1: 100 Records über die SRU-Schnittstelle laden

Lesen Sie die [Dokumentation zur SRU-Schnittstelle im Wiki des GBV](https://www.gbv.de/wikis/cls/SRU) und stellen Sie auf Basis des dort aufgeführten Beispiels eine Abfrage mit folgenden Parametern zusammen:
* Katalog der Bibliothek im Kurt-Schwitters-Forum
* Suche über alle Felder
* Suchbegriff: ```open```
* Format: ```marcxml```
* Anzahl Records: ```100```

Laden Sie die Daten mit **curl**, wie in Kapitel 4.2 geübt.

## Lösung

Folgende Abfrage über die SRU-Schnittstelle des GBV liefert die ersten 100 Treffer im Format ```marcxml``` für den Suchbegriff ```open``` im Katalog der Bibliothek im Kurt-Schwitters-Forum:

{%s%}http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml{%ends%}

Speichern der Daten mit curl:

{%s%}curl "http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml" > 1-100.marcxml{%ends%}

## Aufgabe 2: 1.000 Records über die SRU-Schnittstelle laden

In Aufgabe 1 haben wir 100 Records über die SRU-Schnittstelle geladen. Wenn wir den Parameter maximumRecords einfach auf 1000 erhöhen, dann meldet die Schnittstelle einen Fehler zurück (probieren Sie es aus...). Suchen Sie nach einer Möglichkeit, mehr als 100 Records für den Suchbegriff ```open``` zu laden.

**Hinweise:**
* Stellen Sie mehrere Anfragen in 100er-Paketen.
* Nutzen Sie dazu den zusätzlichen Parameter ```startRecord```.

## Lösung

Download in 100er-Paketen:
* {%s%}curl "http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml&startRecord=1" > 1-100.marcxml{%ends%}
* {%s%}curl "http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml&startRecord=101" > 101-200.marcxml{%ends%}
* {%s%}curl "http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml&startRecord=201" > 201-300.marcxml{%ends%}
* usw.

## Aufgabe 3: Shell-Script zum Download von 1000 Records schreiben 

Shell-Scripte ermöglichen die Automatisierung von Befehlen auf der Kommandozeile. So müssen Sie nicht alle Befehle nacheinander selbst eintippen, sondern brauchen nur einmal das Script starten und der Computer arbeitet die Befehle selbstständig nacheinander ab. Es können auch Variablen und Schleifen definiert werden, so dass die Befehle dynamisch innerhalb der Laufzeit des Scripts angepasst werden können, was sehr weitreichende Möglichkeiten bietet. Shell-Scripte sind somit ein erster Einstieg in die Programmierung, woher übrigens auch das Schimpfwort "[Scriptkiddie](https://de.wikipedia.org/wiki/Scriptkiddie)" stammt ;-).

Wenn Sie noch keine Erfahrung mit Shell-Scripten haben und die Aufgabe selbst lösen wollen, dann arbeiten Sie zunächst den sehr empfehlenswerten [Bash-Skripting-Guide für Anfänger](https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger/) durch (wofür Sie sich 1-2 Stunden Zeit nehmen sollten). Alternativ probieren Sie einfach die untenstehende Lösung aus und schauen was passiert.

**Hinweise:**
* Suchen Sie in Foren nach fertigen Lösungen (und wenn Sie nicht fündig werden, schauen Sie z.B. [hier](http://stackoverflow.com/questions/16131321/shell-script-using-curl-to-loop-through-urls))
* Verwenden Sie die Lösung aus Aufgabe 2 als Grundlage und versuchen Sie die 10 Einzelschritte über eine Schleife abzubilden.
* Definieren Sie zu Beginn Variablen, das macht das Script aufgeräumter und erleichtert spätere Anpassungen.
* Das Script läuft nicht durch, obwohl die Befehle einzeln in der Kommandozeile funktionieren? Achten Sie auf das sogenannte [Quoting](http://wiki.bash-hackers.org/syntax/quoting), das sind [übliche Fehler](http://www.greenend.org.uk/rjk/tech/shellmistakes.html#missingquote).

## Lösung

```
#!/bin/bash

url="http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.all%3Dopen&maximumRecords=100&recordSchema=marcxml&startRecord="
startRecord=1
counter=100

while [ "$counter" -le 1000 ] ; do
    curl "${url}${startRecord}" > ${startRecord}-${counter}.marcxml
    let counter=counter+100
    let startRecord=startRecord+100
done
exit
```

**Ausführen:**
* Diesen Textinhalt in einer Datei abspeichern, z.B. mit ```nano download-1000.sh```
* Danach muss das Script noch ausführbar gemacht werden: ```chmod +x download-1000.sh```
* Script starten mit ```./download-1000.sh```

**Erläuterungen:**
* Anfangs werden die Variablen ```url```, ```startRecord``` und ```counter``` definiert, die später mit ```${url}``` usw. wieder abgerufen werden. Der Wert für die Variable ```url``` muss in Anführungszeichen gesetzt werden, weil sonst das &-Zeichen von der Shell missverstanden würde.
* Als Schleife wurde hier die Form ```while [ ] ; do``` (...) ```done``` gewählt. Die Testbedingung ```"$counter" -le 1000``` innerhalb der eckigen Klammern des ```while```-Kommandos bedeutet, dass die Schleife solange ausgeführt wird, bis die Variable ```counter``` den Wert 1000 erreicht.
* Der curl-Befehl lautet nach Auflösung der Variablen genauso wie aus den vorigen Aufgaben bekannt. Der Dateiname unter dem das jeweilige Ergebnis gespeichert werden soll, setzt sich aus den Variablen ```startRecord``` und ```counter``` zusammen.
* Bevor der curl-Befehl erneut ausgeführt wird, sorgen die beiden ```let```-Befehle dafür, dass ```startRecord``` und ```counter``` jeweils um 100 erhöht werden. Somit lädt der zweite Durchlauf der Schleife die Records 101 bis 200 (und so weiter).

## Aufgabe 4: Download der vollständigen Metadaten der Bibliothek im Kurt-Schwitters-Forum

In der vorigen Aufgabe haben Sie mit einem Script mehrere 100er-Pakete für den Suchbegriff ```open``` heruntergeladen. Um die Metadaten aus dem Bibliothekskatalog **vollständig** abzurufen, benötigen wir eine oder mehrere Suchabfragen, welche die gesamte Ergebnismenge zurückliefern und die wir dann mit dem Parameter ```startRecord``` in 100er-Paketen nach und nach durchgehen können.

**Aufgabe:**
1. Lesen Sie die Dokumentation der [SRU-Schnittstelle](https://www.gbv.de/wikis/cls/SRU) und des [PICA-Formats](https://www.gbv.de/wikis/cls/PICA-Format) und probieren Sie Abfragen mit verschiedenen Feldern.
2. Wenn Sie eine Möglichkeit gefunden haben, passen Sie das Script aus Aufgabe 3 entsprechend an.

**Hinweise:**
* Probieren Sie den Wert ```.*``` in Ihren Suchanfragen
* Falls Sie sich für die theoretischen Hintergründe interessieren, könnte der Bericht "[Guidelines for preparing a Z39.50/SRU target to enable metadata harvesting](http://cyberdoc.univ-lemans.fr/PUB/CfU/Journee_UNIMARC_Lyon/TELplus-D2.3_v1.0%5B1%5D.pdf)" des EU-Projekts eContentplus vom 30.6.2009 interessieren (dort ist aber keine praktische Lösung für die Aufgabe zu finden).

## Lösung

Folgende Anfrage an die SRU-Schnittstelle liefert die Gesamtmenge zurück:
* Suchanfrage: {%s%}pica.ppn=.*{%ends%}
* Beispiel: {%s%}http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.ppn=.*&maximumRecords=100&startRecord=1&recordSchema=marcxml{%ends%}
* entspricht folgender Suche im Katalog: {%s%}http://opac.tib.eu/DB=11/CMD?ACT=SRCH&TRM=PPN+.%3F{%ends%}
* Gesamtanzahl der Records: {%s%}612.152 (Stand: 22.03.2017){%ends%}

### Variante 1: Download-Script "minimal"

Mit dem Shell-Script aus Aufgabe 3 als Vorlage, der neuen Suchanfrage und der Gesamtanzahl der Records haben wir schon alle Bestandteile, die wir benötigen. Folgendes Script lädt alle Records herunter (vergleichen Sie es mit dem Script aus Aufgabe 3):

```
#!/bin/bash

url="http://sru.gbv.de/opac-de-960-3?operation=searchRetrieve&query=pica.ppn=.*&maximumRecords=100&recordSchema=marcxml&startRecord="
startRecord=1
counter=100

while [ "$counter" -le 612200 ] ; do
    curl "${url}${startRecord}" > ${startRecord}-${counter}.marcxml
    let counter=counter+100
    let startRecord=startRecord+100
done
exit
```

**Ausführen:**

* Diesen Textinhalt in einer Datei abspeichern, z.B. mit ```nano download-minimal.sh```
* Danach muss das Script noch ausführbar gemacht werden: ```chmod +x download-minimal.sh```
* Script starten mit ```./download-minimal.sh```

### Variante 2: Download-Script "comfort"

Üblicherweise werden in Shell-Scripten noch Tests oder Statistiken eingebaut, um gleich prüfen zu können, ob die Aktionen erfolgreich waren. Außerdem werden üblicherweise so viele Variablen wie möglich definiert, damit das Script leicht angepasst und in anderen Kontexten verwendet werden kann. Weiterhin sollten Kommentare mit ```#``` eingeführt werden, damit das Script ohne extra Dokumentation besser nachvollziehbar wird.

Ein komfortableres Script könnte wie folgt aussehen (probieren Sie es einfach mal aus...).

```
#!/bin/bash
# Script zum Download von Metadaten über SRU-Schnittstellen mit curl
# sru-download.sh, Felix Lohmeier, v0.2, 16.03.2017
# https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek

# Variablen (bei Bedarf hier anpassen)
url="http://sru.gbv.de/opac-de-960-3"
query="pica.ppn=.*"
format="marcxml"
outputdir="download"
filename="hsh_ksf"
recordlimitperquery=100
# Weitere technische Variablen
date="$(date +%F)"
datelog="$(date +%Y%m%d_%H%M%S)"
command="?operation=searchRetrieve"
startrecord=1
let counter=startrecord+recordlimitperquery-1

# Verzeichnis erstellen (falls nicht vorhanden)
mkdir -p $outputdir

# Ausgabe parallel in eine Logdatei schreiben
exec &> >(tee -a "$outputdir/${filename}_${datelog}.log")

# Anzahl der Datensätze auslesen
records=$(curl --silent "${url}${command}&query=${query}&recordSchema=${format}" | sed 's/</\n/g' | sed '/^\//d' | sed 's/:/\n/g' | grep numberOfRecords | cut -c 17-)

# Variablen ausgeben
echo "SRU-Schnittstelle:       ${url}"
echo "Suchabfrage:             ${query}"
echo "Format:                  ${format}"
echo "Anzahl Datensätze:       ${records}"
echo "Datensätze pro Datei:    ${recordlimitperquery}"
echo "Download in Verzeichnis: $(readlink -f ${outputdir})"
echo "Beispiel Dateiname:      ${filename}_${date}_$(printf "%.7i\n" ${startrecord})-$(printf "%.7i\n" ${counter}).xml"
echo "Logdatei:                ${filename}_${datelog}.log"
echo ""

# Startzeitpunkt ausgeben
echo "Startzeitpunkt: $(date)"
echo ""

# Schleife mit Aufruf von curl
while (("$counter" <= "$records")) ; do
    echo "Download Records "${startrecord}" bis "${counter}"..."
    curl "${url}${command}&query=${query}&maximumRecords=${recordlimitperquery}&recordSchema=${format}&startRecord=${startrecord}" > $outputdir/${filename}_${date}_$(printf "%.7i\n" ${startrecord})-$(printf "%.7i\n" ${counter}).xml
    # Sofortige Prüfung des Downloads, wenn Format marcxml
    if [ $format = "marcxml" ]; then
        echo "Ergebnis: "$(grep -c -H '<controlfield tag="001">' $outputdir/${filename}_${date}_$(printf "%.7i\n" ${startrecord})-$(printf "%.7i\n" ${counter}).xml)" records"
    fi
    echo ""
    let counter=counter+recordlimitperquery
    let startrecord=startrecord+recordlimitperquery
done

# Prüfung des Downloads, wenn Format marcxml
if [ "$format" = "marcxml" ]; then
    echo "Gesamtanzahl der Records im Ordner download:"
    grep '<controlfield tag="001">' $outputdir/*.xml | wc -l
    echo ""
    echo "Dateien, die weniger als 10 Records enthalten:"
    testfiles=($(find "$outputdir" -type f -name '*.xml'))
    for i in "${testfiles[@]}" ; do
        testfilerecords="$(grep -c -h '<controlfield tag="001">' ${i})"
        if (("${testfilerecords}" < "10")); then
            echo 1>&2 "${i}: ${testfilerecords}"
        fi
    done
    echo ""
fi

# Prüfung, ob sich während des Downloads die Datenbank geändert hat
recordsafterdownload=$(curl --silent "${url}${command}&query=${query}&recordSchema=${format}" | sed 's/</\n/g' | sed '/^\//d' | sed 's/:/\n/g' | grep numberOfRecords | cut -c 17-)
if [ "$records" != "$recordsafterdownload" ]; then
    echo 1>&2 "Warnung: Die Suchabfrage an die SRU-Schnittstelle hat vor Beginn des Downloads eine andere Gesamtanzahl an Datensätzen ergeben (${records}) als nach dem Download (${recordsafterdownload}). Das ist ein Indiz dafür, dass die Datenbank zwischenzeitlich verändert wurde. Es ist wahrscheinlich, dass dadurch einzelne Datensätze im Download fehlen."
fi

# Endzeitpunkt ausgeben
echo "Endzeitpunkt: $(date)"
echo ""
```

Script als Datei: [sru-download.sh](scripte/sru-download.sh)

**Ausführen:**

* Script mit ```curl``` auf den Server laden: ```curl -O https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek/raw/master/scripte/sru-download.sh```)
* Script ausführbar machen: ```chmod +x sru-download.sh```
* Script starten mit ```./sru-download.sh```

## Aufgabe 5: Grobe Prüfung der heruntergeladenen Dateien

Das Script benötigt für einen Komplettdurchlauf etwa 6 Stunden. Sie werden also bestimmt nicht jede Transaktion aufmerksam am Bildschirm verfolgt haben. So oder so ist es sinnvoll mit ein paar Tests die Plausibilität der heruntergeladenen Dateien zu prüfen. Bitte beantworten Sie folgende Fragen:

1. Wie groß sind a) alle heruntergeladenen Dateien und b) die kleinste(n) der Dateien? Gibt es c) Auffälligkeiten bei den Dateigrößen?
2. Wieviele Records wurden heruntergeladen? (Tipp: nutzen Sie dazu das Feld bzw. den Pfad ```<controlfield tag="001">```)
3. Gibt es Dubletten? Wenn ja, a) welche, b) wieviele und c) wo?

## Lösung

Wechseln Sie in das Verzeichnis download mit ```cd download```

(1) Prüfung Dateigrößen:
* a) alle: {%s%}du -a -h{%ends%}
* b) die kleinste(n): {%s%}ls -1 -s -S{%ends%}
* c) Auffälligkeiten: {%s%}achten Sie auf kleine und gleiche Dateigrößen, ebenfalls mit ls -1 -s -S{%ends%}

(2) Prüfung Anzahl Records:
* alle: ```grep -h "<controlfield tag=\"001\">" *.marcxml | wc -l```
* ohne Dubletten: ```grep -h "<controlfield tag=\"001\">" *.marcxml | sed 's/<[^>]*>//g; s/^ *//' | uniq | wc -l```

(3) Dubletten ausgeben:
* a) welche: ```grep -h "<controlfield tag=\"001\">" *.marcxml | sed 's/<[^>]*>//g; s/^ *//' | uniq -D```
* b) wieviele: ```grep -h "<controlfield tag=\"001\">" *.marcxml | sed 's/<[^>]*>//g; s/^ *//' | uniq -c -d```
* c) wo: ```grep -h "<controlfield tag=\"001\">" *.marcxml | sed 's/<[^>]*>//g; s/^ *//' | uniq -D | grep -f - *```
