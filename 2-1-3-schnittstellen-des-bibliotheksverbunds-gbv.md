# 2.1.3 Schnittstellen des Bibliotheksverbunds GBV

Das Bibliothekssystem der Bibliothek im Kurt-Schwitters-Forum ist ein "LBS"-System des Anbieters OCLC, das vom Bibliotheksverbund GBV betrieben wird.

Die Metadaten können über Schnittstellen des GBV abgerufen werden, sind aber nicht zur freien Nachnutzung gekennzeichnet. Siehe Datenbankinfo beim GBV: https://uri.gbv.de/database/opac-de-960-3

Das interne Datenformat ist [PICA+](https://www.gbv.de/wikis/cls/PICA-Format), das im Plain-Text-Format oder als XML-Format verfügbar ist. Darüberhinaus bietet der GBV die Metadaten auch in weiteren Formaten an, wie zum Beispiel:
* MARC 21
* Dublin Core (DC)
* MODS
* ISBD

Ausgehend vom internen Datenformat PICA+ werden die übrigen Formate automatisch hergestellt. Dazu werden Übereinstimmungen zwischen den Feldern definiert, die als Mapping oder [Crosswalk](https://en.wikipedia.org/wiki/Schema_crosswalk) bezeichnet werden. Wenn nicht alle Daten aus dem internen Datenformat in das Zielformat passen (wie zum Beispiel beim "kleinsten gemeinsamen Nenner" der Metadatenformate: Dublin Core), dann gehen bei diesem Prozess Informationen verloren.

Wenn alle Daten "verlustfrei" verarbeitet werden sollen, muss also grundsätzlich entweder mit dem internen (auch: [nativ](http://www.duden.de/rechtschreibung/nativ)en) Datenformat gearbeitet werden oder es muss genau (in der Dokumentation) geprüft werden, ob der Crosswalk zum Zielformat wirklich vollständig ist.

In den folgenden beiden Übungen laden wir daher Beispieldatensätze (Kap 2.2.1) und prüfen welche Daten in den verschiedenen Formaten enthalten sind (Kap 2.2.2). Anschließend entscheiden wir uns für ein Format und laden die gesamten Daten herunter.
