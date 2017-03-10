# 1.3.1 Installation der virtuellen Maschine

Wir verwenden die Virtualisierungssoftware [VirtualBox](http://www.virtualbox.org), um einen Linux-Webserver auf dem Computer zu installieren. VirtualBox ist für Windows, Mac OS und Linux kostenfrei erhältlich.

**Systemvoraussetzungen:**
* Computer mit Windows, Mac oder Linux
* mindestens 2 GB freien Arbeitsspeicher für die virtuelle Maschine
* 20 GB freien Speicher auf der Festplatte
* einigermaßen aktuelle CPU mit [Hardware-Unterstützung für Virtualisierung](http://www.sysprobs.com/disable-enable-virtualization-technology-bios) (Intel VT-X, AMD-V)

## Schritt 1: VirtualBox installieren

Laden Sie die Installationsdatei für ihr Betriebssystem unter https://www.virtualbox.org/wiki/Downloads und folgen Sie den Anweisungen des Installationsprogramms.

Eine Einführung in das Programm bietet das offizielle Handbuch (in englisch): https://www.virtualbox.org/manual/ch01.html

VirtualBox ermöglicht es Ihnen auf ihrem gewohnten Betriebssystem (Windows, Mac, Linux) beliebige weitere Gast-Betriebssysteme zu installieren und parallel auszuführen. Die Gast-Betriebssysteme laufen in einer gesicherten, virtuellen Arbeitsumgebung. So können sie lokal einen virtuellen Linux-Webserver auf ihrem Computer installieren und bei Bedarf starten.

Starten Sie den VirtualBox Manager, klicken Sie auf Neu, wählen Sie als Typ "Linux" und prüfen Sie, ob Ihnen bei "Version" Auswahlmöglichkeiten mit 64-bit angeboten werden. Falls ausschließlich 32-bit zur Verfügung steht, dann ist Ihr Rechner etwas älter oder ein anderes Virtualisierungsprogramm (manchmal ist Microsoft Hyper-V vorinstalliert) blockiert die 64-bit-Funktionen. Jedenfalls müssen Sie in Schritt 2 das passende Installationsmedium (32-bit oder 64-bit) herunterladen (bevorzugt 64-bit, wenn ihr Rechner das unterstützt).

## Schritt 2: Installationsdatei für Ubuntu MATE herunterladen

Laden Sie das Installationsmedium von der offiziellen Webseite unter https://ubuntu-mate.org/download herunter. Für ein stabiles System empfiehlt sich der Download der Long-Term-Support (LTS) Version, also beispielsweise Ubuntu MATE 16.04.2 LTS.

## Schritt 3: Virtuelle Maschine in VirtualBox erstellen und Ubuntu MATE installieren

Für die weitere Konfiguration orientieren Sie sich an der folgenden Screenshot-Galerie:

(folgt)
