# ADR-0003: QGIS 4.2 LTR headless

* **Status:** angenommen (Vorgabe Nutzer 19.11.2026), Punktversion offen
* **Datum:** 19.11.2026
* **Entscheidung Nr. in ROADMAP:** E4

## Kontext

Der Referenzlauf steuerte ein geöffnetes QGIS auf dem Mac über `execute_code` (Zeitlimit 55 s,
Blockaden, `print(1)`-Prüfung). Gewünscht: alle QGIS-Operationen ohne geöffnetes QGIS, am Ende eine
fertige Projektdatei. QGIS 4.2 ist die erste LTR der 4.x-Reihe (Qt6). In den LTR-Paketquellen wird
3.44 noch so lange ausgeliefert, bis die nächste reguläre Version erscheint. Stand Mitte November 2026
ist 4.4 noch nicht erschienen.

## Entscheidung

1. Zielversion **QGIS 4.2 LTR**, Punktversion festgeschrieben in der Konfiguration, auf allen Rechnern gleich.
2. QGIS-Stufen laufen als Unterprozess im QGIS-Interpreter mit `QgsApplication([], False)` und
   `QT_QPA_PLATFORM=offscreen`. Processing-Algorithmen über `qgis_process` oder `processing.run` im selben Prozess.
3. Ein Prozess je Teilschritt, Übergabe über Dateien. Abbruch eines Prozesses gefährdet keine anderen Teilschritte.
4. Referenzskripte (3.x, Qt5) werden beim Übertrag auf 4.2 geprüft.

## Folgen

* Die Betriebsregeln aus Anhang E werden im Regelinventar je Regel als „gilt weiter" oder „hinfällig durch headless" markiert.
* Schriften des Hausstils müssen auf jedem rendernden Rechner installiert sein, sonst weichen Karten ab.
* Server: Linux mit QGIS 4.2 headless (Paket oder Container), gleiche Version wie lokal.
