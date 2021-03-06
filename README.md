# Skript zum Seminar "Praxis der digitalen Bibliothek" \(Sommersemester 2017\)

Das Skript basiert auf einer vorigen [Lehrveranstaltung an der HAW Hamburg im Wintersemester 2016/17](https://www.gitbook.com/book/felixlohmeier/seminar-wir-bauen-uns-einen-bibliothekskatalog/) mit 4 SWS. Diese Fassung ist weiterentwickelt und für eine Lehrveranstaltung mit 2 SWS auf die Hälfte komprimiert.

## Formate
* Lesefassung bei GitBook (HTML): https://www.gitbook.com/read/book/felixlohmeier/seminar-praxis-der-digitalen-bibliothek
* Druckfassung bei GitBook (PDF): https://www.gitbook.com/download/pdf/book/felixlohmeier/seminar-praxis-der-digitalen-bibliothek
* Repository bei GitHub (zum Nachnutzen): https://github.com/felixlohmeier/seminar-praxis-der-digitalen-bibliothek

## Lehrveranstaltung

Dieses Skript entsteht in der Zeit von März bis Mai 2017 im Rahmen der folgenden Lehrveranstaltung:

* Seminar "Praxis der digitalen Bibliothek"
* Dozent: [Felix Lohmeier](http://felixlohmeier.de)
* Sommersemester 17
* Lehrauftrag an der [Hochschule Hannover, Fakultät III - Medien, Information und Design](http://f3.hs-hannover.de)
* Studiengänge Informationsmanagement und Informationsmanagement berufsbegleitend
* Bachelor 6. Semester

## Inhalte

Kapitel 1: Einführung ins Thema und Installation der Arbeitsumgebung
* [1.1 Einführung in Discovery-Systeme in Bibliotheken](1-1-0-einfuehrung-in-discovery-systeme-in-bibliotheken.md)
* [1.2 Open-Source-Software für Bibliothekskataloge](1-2-0-open-source-software-fuer-bibliothekskataloge.md)
* [1.3 Grundinstallation der Arbeitsumgebung](1-3-0-grundinstallation-der-arbeitsumgebung.md)

Kapitel 2: Metadaten laden und mit OpenRefine transformieren
* [2.1 Einführung in Metadatenformate und Schnittstellen](2-1-0-einfuehrung-in-metadatenformate-und-schnittstellen.md)
* [2.2 Metadaten über eine Schnittstelle laden](2-2-0-metadaten-ueber-eine-Schnittstelle-laden.md)
* [2.3 Metadaten transformieren mit OpenRefine](2-3-0-metadaten-transformieren-mit-openrefine.md)

Kapitel 3: Transformierte Metadaten mit Solr indexieren
* [3.1 Daten mit OpenRefine für Indexierung vorbereiten](3-1-0-daten-mit-openrefine-fuer-indexierung-vorbereiten.md)
* [3.2 Suchmaschinenindex Solr](3-2-0-suchmaschinenindex-solr.md)

Kapitel 4: Prototyp eines Katalogs mit TYPO3-find bauen
* [4.1 Katalogoberfläche TYPO3-find](4-1-0-katalogoberflaeche-typo3-find.md)
* [4.2 Zwischenfazit](4-2-0-zwischenfazit.md)

Kapitel 5: Erweiterungsmöglichkeiten
* [5.1 Zusätzliche Daten über OAI-Schnittstelle](5-1-zusaetzliche-daten-ueber-oai-schnittstelle.md)
* [5.2 Relevanzranking mit TYPO3-find und Solr](5-2-relevanzranking-mit-typo3-find-und-solr.md)

## Lerntagebücher

Als eine mögliche [Prüfungsleistung](pruefungsleistungen.md) haben Studierende öffentliche Lerntagebücher geschrieben, in denen sie von ihren Erkenntnissen berichten und sich mit dem Inhalt des Seminars auseinandersetzen.

Die Blogs der Studierenden sind unter der Rubrik [Lerntagebücher](lerntagebuecher.md) aufgeführt. Thematisch relevante Beiträge sind in den jeweiligen Kapiteln im Skript verlinkt.

## Beschreibung

Zeitgemäße Recherchesysteme bedienen sich moderner Suchmaschinentechnologien. So haben die meisten größeren Bibliotheken in den letzten 10 Jahren ihre klassischen Online-Kataloge (OPACs) auf modernere Discovery-Systeme umgestellt. Diese Systeme sind allerdings trotz standardisierter Webtechnologien keine Selbstläufer. Egal ob gekauft oder selbst entwickelt, das Ranking nach Relevanz bringt nur dann richtig gute Ergebnisse, wenn die Bestände der Bibliothek für die jeweilige Zielgruppe klug aufbereitet werden. Dafür müssen Bibliothekarinnen und Bibliothekare sich immer intensiver mit der zugrundeliegenden Technik und den (Meta)daten auseinandersetzen.

Der Einstieg in die mysteriös anmutende Discovery-Technik fällt oft schwer, die Zusammenhänge sind in der Theorie manchmal unverständlich. Daher ist es am besten, die Technik selbst auszuprobieren und einen Blick unter die Haube zu werfen. Dank der freien Verfügbarkeit von Open-Source-Anwendungen für Metadatenmanagement und Katalogpräsentation ist das möglich geworden.

In diesem Seminar werden wir in praktischen Übungen reale Bibliotheksdaten verarbeiten und einen Prototyp eines modernen Discovery-Systems aufsetzen. Programmierkenntnisse sind dafür nicht erforderlich. Die praktischen Übungen werden im PC-Pool der Hochschule Hannover durchgeführt.

## Intention Openness

Soweit möglich werden die Materialien für die Lehrveranstaltung an dieser Stelle online gestellt, damit sie vielleicht auch über den Kreis der SeminarteilnehmerInnen hinaus nützlich sind.

## Literaturempfehlungen zum Einstieg

* Anne Christensen (2013): Warum BibliothekarInnen bei Discovery mitmischen sollten, trotz allem. In: Blog "A growing organism - Bibliothekarische An- und Aussichten von Anne Christensen", 20.4.2013: https://xenzen.wordpress.com/2013/04/20/discovery-mitmischen/
* Prof. Magnus Pfeffer (2016): Open Source Software zur Verarbeitung und Analyse von Metadaten. Präsentation auf dem 6. Bibliothekskongress. http://nbn-resolving.de/urn/resolver.pl?urn:nbn:de:0290-opus4-24490
* Christof Rodejohann & Felix Lohmeier (2016): Schlanke Discovery-Lösung auf Basis von TYPO3. Der neue Bibliothekskatalog der SLUB Dresden. Präsentation im Rahmen des "Bibcast", Live-Webcast im Vorfeld des Bibliothekskongresses, 9.3.2016: http://bibcast.openbiblio.eu/schlanke-discovery-loesung-auf-basis-von-typo3-der-neue-bibliothekskatalog-der-slub-dresden/
* Felix Lohmeier & Jens Mittelbach (2014): Offenheit statt Bündniszwang. In: Zeitschrift für Bibliothekswesen und Bibliographie 61, H. 4-5, S. 209-214. http://dx.doi.org/10.3196/1864295014614554 

## Lizenz

Dieses Werk ist lizenziert unter einer [Creative Commons Namensnennung 4.0 International Lizenz](http://creativecommons.org/licenses/by/4.0/)

[![Creative Commons Lizenzvertrag](https://i.creativecommons.org/l/by/4.0/88x31.png)](http://creativecommons.org/licenses/by/4.0/)
