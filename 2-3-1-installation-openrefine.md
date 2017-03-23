# 2.3.1 Installation OpenRefine

[OpenRefine](http://www.openrefine.org) bietet eine grafische Oberfläche zur Analyse und Transformation von Daten, die ähnlich wie eine klassische Tabellenverarbeitungssoftware (MS Excel, LibreOffice Calc, usw.) aufgebaut ist. Wir verwenden diese Software im Seminar um die Ausgangsdaten aus dem Bibliothekssystem zu manipulieren und in ein passendes Format für den Suchmaschinenindex zu transformieren.

## OpenRefine herunterladen und entpacken

Auf der Webseite von OpenRefine werden verschiedene Varianten zum [Download](http://openrefine.org/download.html) angeboten. Wir laden die neueste Version (Stand 22.3.2017: OpenRefine 2.7-rc2) für das Betriebssystem Linux. Die Installationsanleitung auf der Webseite ist simpel: "Download, extract, then type ./refine to start."

Wir erledigen dies wieder mit der Kommandozeile (MATE-Terminal):
* Download: ```wget https://github.com/OpenRefine/OpenRefine/releases/download/2.7-rc.2/openrefine-linux-2.7-rc.2.tar.gz```
* Extract (entpacken): ```tar -xzf openrefine-linux-2.7-rc.2.tar.gz```

Im Ordner ```openrefine-2.7-rc.2``` finden Sie jetzt das Programm OpenRefine.

## OpenRefine starten

```
~/openrefine-2.7-rc.2/refine
```

Die Tilde (```~```) ist ein Kürzel für ihr Benutzerverzeichnis. Dieser Befehl funktioniert immer, egal in welchem Verzeichnis Sie sich gerade befinden. Wenn Sie sich im Ordner von OpenRefine befinden (```cd ~/openrefine-2.7-rc.2```) reicht ein simples ```refine```

Ist der Startvorgang erfolgreich, dann öffnet sich der Browser (Firefox) automatisch und Sie bekommen das Programm direkt angezeigt. OpenRefine ist in der Standardeinstellung unter der IP-Adresse http://127.0.0.1:3333 erreichbar.

Rufen Sie den Menüpunkt "Open Project" auf und klicken Sie ganz unten auf den Link "Browse workspace directory". Es öffnet sich der Ordner, in dem OpenRefine die Daten speichert. In der Standardkonfiguration unter Linux ist dies das Verzeichnis ```~/.local/share/openrefine```

### OpenRefine beenden

OpenRefine ist nur solange verfügbar, wie der oben verwendete Befehl in der Kommandozeile läuft.

1. Beenden Sie OpenRefine mit ```STRG``` und ```C``` auf der Kommandozeile. Der Browser schließt sich automatisch.
2. Starten Sie OpenRefine erneut, indem Sie auf der Kommandozeile mit der ```Pfeiltaste nach oben``` den vorigen Befehl auswählen und mit ```Enter``` ausführen.

Auf der Kommandozeile können Sie übrigens mitverfolgen, wie der Browser und OpenRefine miteinander kommunizieren. Beim Aufruf von OpenRefine im Browser erscheinen beispielsweise die folgenden POST und GET Befehle in der Kommandozeile:

```
15:10:34.819 [                   refine] POST /command/core/load-language (19332ms)
15:10:34.940 [                   refine] POST /command/core/load-language (121ms)
15:10:35.223 [                   refine] POST /command/core/get-importing-configuration (283ms)
15:10:35.509 [                   refine] GET /command/core/get-all-project-metadata (286ms)
15:10:35.632 [                   refine] GET /command/core/get-languages (123ms)
15:10:35.721 [                   refine] GET /command/core/get-version (89ms)
```

Doch dazu später mehr.
