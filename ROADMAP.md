# ROADMAP

Phasen, Meilensteine und Entscheidungen. Zieltermine nach Kapazitätsplanung eintragen.

**Stand:** 19.11.2026

**Leitlinie:** Erst Fundament (Runner, Zustand, Speicherschicht) und Datenrückgrat (GeoPackage plus Prüfskript),
dann Schritt für Schritt nach vorne und hinten erweitern. Jede Phase endet mit einem Abnahmetest gegen den
Referenzlauf Narthauen-Süd (P-3241). Alles wird so gebaut, dass es ohne Umbau auf einem Server läuft.

---

## Phasen

| Phase | Ziel | Ergebnis (Abnahme) | Zieltermin |
|---|---|---|---|
| **0 Fundament** | offene Entscheidungen treffen, Umgebung, Regelinventar aus dem Prompt-PDF, Referenzskripte sichten | ADRs angenommen, QGIS 4.2 LTR headless exportiert ein Testlayout, Regelinventar vollständig | |
| **1 Gerüst** | Runner, `state.json`, Datenanforderungen, `decide` und `resume`, Speicherschicht lokal, Skript-Vertrag | Test: Lauf hält bei fehlender Datei an, Nutzer liefert nach, Lauf setzt ohne Verlust fort | |
| **2 Datenrückgrat** | Stufe 1 und 2, GeoPackage-Schema, Prüfskript portiert | Für Narthauen-Süd entsteht GPKG mit allen eigenen Layern, Prüfung grün | |
| **3 Sachdaten** | Stufe 3: Google-API für Tabellen, PDF per Agent mit Fundstelle | `facts.json` stimmt mit Stammdaten des Referenzlaufs überein | |
| **4 Fremdquellen** | Stufe 4: Geodatenkatalog, Kontrollabfragen, Cache, Snapshot | Umfeldlayer des Referenzlaufs reproduziert, Nullbefunde belegt | |
| **5 Analyse** | Stufe 5 mit Zwischenbericht | Layer 60 bis 64 und 96 reproduziert, Abweichungen erklärt | |
| **6 Karten** | Stufe 6: Vorlagen, headless Export, Kontaktbogen | sieben Blätter plus externe Fassungen, Sichtprüfung bestanden | |
| **7 Dokumente** | Stufe 7 und 8 | Konzeptbegründung, sechs Handouts, Zahlenabgleich und Namenssuche grün | |
| **8 Abschluss und Drive** | Stufe 9 und 10: QA, Projekt relativ und eingebettet, Änderungsliste, Archiv, Übertragung | Projektdatei öffnet aus dem Drive auf Windows und macOS, Ende-zu-Ende-Lauf mit nur H1 bis H5 | |
| **9 Zweite Zelle** | Lauf auf neuer Zelle (z. B. Hymendorf P-3196), Einordnung | Lauf ohne Codeänderung, Lücken sauber gemeldet | |
| **10 Server** | Linux-Server mit QGIS 4.2 headless, `gdrive_api`, Dienstkonto, `agent_mode: api`, Benachrichtigung per E-Mail oder Slack | derselbe Lauf wie lokal, Ergebnisse im Drive, identische Prüfergebnisse | |

## Meilensteine

* **M0** Entscheidungen getroffen, Umgebung steht (Ende Phase 0)
* **M1** Lauf übersteht Datenlücke ohne Verlust (Ende Phase 1)
* **M2** Erstes GeoPackage vollautomatisch, Prüfung grün (Ende Phase 2)
* **M3** Alle Daten ohne Handarbeit (Ende Phase 5)
* **M4** Erste vollständige Lieferung ins Drive (Ende Phase 8)
* **M5** Serverbetrieb (Ende Phase 10)

---

## Entscheidungen

| Nr | Frage | Stand | ADR |
|---|---|---|---|
| E1 | Wer steuert den Ablauf? | Vorschlag: Python-Runner, Agenten an definierten Stellen | [ADR-0001](docs/decisions/ADR-0001-skript-rechnet-agent-schreibt.md) |
| E2 | Agentenlaufzeit | Vorschlag: Aufgaben als Dateien, `interactive` (Desktop, Claude Code) jetzt, `api` auf dem Server | [ADR-0005](docs/decisions/ADR-0005-agentenaufgaben-als-dateien.md) |
| E3 | Laufumgebung | **entschieden:** lokal jetzt, Server später, Code plattformneutral. Offen: welcher lokale Rechner für Produktivläufe | [ADR-0004](docs/decisions/ADR-0004-speicherschicht-server.md) |
| E4 | QGIS-Version | **entschieden:** aktuelle LTR (4.2) headless. Offen: Punktversion | [ADR-0003](docs/decisions/ADR-0003-qgis-headless-ltr.md) |
| E5 | Zugriff auf Drive und Google-Dateien | Vorschlag: Speicherschicht, Google-API für `.gsheet` und `.gslides` auch lokal | ADR-0004 |
| E6 | Dokumenterzeugung | offen: `mkdocx.py` übernehmen oder docx-Vorlagen mit `docxtpl`. Auf Server ist python-docx verfügbar | |
| E7 | Datenschutz gegenüber LLM | offen. Vorschlag: Namen nur als IDs an Agenten, Auflösung beim Setzen | |
| E8 | Grad der Automatisierung | **entschieden:** Datenlücken entscheidet immer der Nutzer. Haltepunkte H1 bis H5 | [ADR-0002](docs/decisions/ADR-0002-lauf-zustand-datenluecken.md) |
| E9 | Reproduzierbarkeit externer Daten | Vorschlag: Snapshot je Lauf mit Abrufdatum und Hash | |
| E10 | Pipeline-Werkzeug | Vorschlag: eigener schlanker Runner auf Basis `pipeline.yaml` | |
| E11 | Speicherschicht | Vorschlag: logische Pfade, Backends `local` und `gdrive_api` | ADR-0004 |
| E12 | Wo läuft der Runner bei interaktiven Läufen? | offen: Claude Code auf Windows (direkter Zugriff auf QGIS) oder Desktop-App mit eigener Linux-Umgebung für QGIS | ADR-0005 |
| E13 | Benachrichtigung | offen: lokal Konsole und Datei, Server E-Mail oder Slack | |
| E14 | Schriften auf dem Server | offen: Calibri und Cambria lizenziert installieren oder Hausstil auf freie Schriften umstellen | |
| E15 | Zuschnitt der Teilschritte | Vorschlag in `pipeline.yaml`, wird beim Bau je Stufe festgelegt | ADR-0002 |

## Risiken

| Risiko | Wirkung | Gegenmaßnahme |
|---|---|---|
| Implizites Wissen aus Prompt und Referenzskripten geht verloren | stille Fehler wie im Referenzlauf | Regelinventar mit Verweis je Regel, Test je Regel, Referenzskripte als Ausgangsmaterial |
| Referenzskripte für QGIS 3.x (Qt5), Ziel 4.2 (Qt6) | Portierungsaufwand, geänderte APIs | früh ein Blatt headless unter 4.2 bauen |
| Fremddienste ändern sich oder antworten fehlerhaft | Nullbefund statt Fehler | Kontrollabfrage, Zweitquelle, Snapshot, Datenanforderung an Nutzer |
| LLM erfindet oder verändert Zahlen | falsche Aussagen in Außendokumenten | Platzhalter, Zahlenabgleich, Fakten-IDs |
| Interne Daten in externer Fassung | Vertrauensschaden | Filter in den Daten, Namenssuche als harte Prüfung |
| Lange Pfade und Umlaute (Windows 260 Zeichen, NFD auf macOS) | Dateien nicht gefunden, Archiv scheitert | kurzer Run-Ordner, lange Pfade aktivieren, NFC-Vergleich |
| Lauf bleibt lange in `waiting_user` | Eingänge im Drive ändern sich zwischendurch | Eingangshashes beim Fortsetzen prüfen, Änderungen melden |
| Kennwerte ändern sich (z. B. RFNBO-Änderungsliste) | Ergebnisse nicht vergleichbar | Fassung und Standdatum im Lauf-Manifest |
| Schriften fehlen auf dem rendernden Rechner | Karten und Dokumente weichen vom Hausstil ab | Schriftprüfung als Teil der Umgebungsprüfung (E14) |
