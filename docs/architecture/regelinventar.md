# Regelinventar

Jede Regel aus dem Prompt-PDF (`docs/pdf/`) und dem Rechercheauftrag genau einmal. Ziel: Code (C), Konfiguration (K),
Agentenanweisung (A), Haltepunkt (H). Erst wenn „umgesetzt" und „Test" gefüllt sind, gilt die Regel als übertragen.

| Nr | Quelle | Regel (kurz) | Ziel | Ort im Repo | Test | umgesetzt |
|---|---|---|---|---|---|---|
| R-001 | Prompt 6.1 | CRS an drei Stellen setzen: Projekt, Kartenfenster, jedes Kartenelement | C | maps | | |
| R-002 | Prompt 3.4 | Leer heißt nicht kartiert, Kontrollabfrage Pflicht | C, K | external_data, geodatenkatalog.yaml | | |
| R-003 | Prompt 6.8 | Symbollayer im GeoPackage, nie im Speicher | C | project, maps | | |
| R-004 | Prompt 8.3 | Externe Fassung ohne Namen, Prüfung per Suche gegen Layer 50 | C | handouts, qa | | |
| R-005 | Auftrag 0.3 Nr. 5 | Keine Gedankenstriche in Texten | A, C | agents/, qa | | |
| R-006 | Prompt 1.3, Anhang E | Projektdatei nur per XML wählen, nicht in QGIS öffnen | C | project | | |
| R-007 | Grundsätze | Bestand bleibt unberührt, nur `publish` schreibt ins Drive | C | common/storage, publish | | |
| R-008 | Grundsätze | Meldung statt Stillstand, dieselbe Operation nicht wiederholen | C, A | orchestration, agents/ | | |
| R-009 | Anhang H2 | Änderungsliste vor jeder Dateiänderung im Drive | H | publish, H5 | | |
| R-010 | Anhang E | Drive ist Ablage, nicht Werkbank: Eingänge in den Run kopieren | C | common/storage | | |
| | | | | | | |
