# TASKS

Laufende Aufgabenliste. Status: `offen`, `in Arbeit`, `blockiert`, `erledigt`.
Neue Aufgaben in der passenden Phase anfügen, erledigte nach „Erledigt" verschieben (mit Datum).
IDs bleiben stabil, auch wenn Aufgaben die Phase wechseln.

**Stand:** 19.11.2026

---

## In Arbeit

| ID | Aufgabe | Phase | Verantwortlich | Notiz |
|---|---|---|---|---|
| | | | | |

## Als Nächstes

| ID | Aufgabe | Phase | Abhängig von | Notiz |
|---|---|---|---|---|
| T-001 | Offene Entscheidungen E1, E2, E5, E6, E7, E12 treffen, ADRs auf „angenommen" setzen | 0 | | E3, E4, E8 entschieden |
| T-002 | Git-Repository anlegen (privat), `git status` darf keine Projektdaten zeigen | 0 | | |
| T-003 | Regelinventar aus dem Prompt-PDF füllen: Grundsätze, Schritt 1 bis 9, Anhang A bis H. Je Regel Ziel (Code, Konfig, Agent, Halt), Ort, Test | 0 | | `docs/architecture/regelinventar.md` |
| T-004 | Anhang E und H4 je Regel markieren: „gilt weiter" oder „hinfällig durch headless" | 0 | T-003 | Vorschlag in CLAUDE.md Abschnitt 8 |
| T-006 | Werkzeugordner nach `data/reference/werkzeug/` kopieren: `ENEO_Kennwerte.md`, `ENEO_Geodatenkatalog.md`, `Projektregister.csv`, `qs_zelle_v10.py`, `mkdocx.py`, `H2_Kernnetz_Nordwest.csv`, `H2_Kernnetz_Datenlage.md`, `ENEO_QGIS_Icons/` | 0 | | Fassung und Datum notieren |
| T-007 | Referenzlauf Narthauen-Süd sichern: GPKG, Projekt, Karten, Dokumente, `Uebergabe_Narthauen-Sued.md` nach `data/reference/narthauen-sued/` | 0 | | Grundlage für `tests/golden/` |
| T-009 | Referenzskripte aus `Kartendarstellung Arbeitsdateien/` sichern und sichten: `pb.py`, `stil.py`, `eneo_a3.py`, `nah.py`, `karte.py`, `eig.py`, `fest.py`, `final.py`, `verbund.py`, `doku.py`, `mkdocx.py`, `drive.py`, `beiblatt.py`, `teilnehmer.py`, `nordpfeil.svg`, Symbol-GeoPackages. Je Skript: Zweck, Ziel-Modul, QGIS-4.2-Tauglichkeit | 0 | | wichtigstes Ausgangsmaterial für `maps`, `report`, `publish` |
| T-008 | QGIS 4.2 LTR auf Zielrechner installieren, Punktversion festlegen, headless Testlauf: Projekt laden, Layout aus `.qpt`, A3-PDF exportieren mit `QT_QPA_PLATFORM=offscreen` | 0 | E4 | Windows: OSGeo4W, Interpreterpfad in `.env` |
| T-010 | Schriften prüfen: Calibri und Cambria im headless Export korrekt? | 0 | T-008 | E14 |
| T-011 | Google-API-Zugang einrichten (OAuth lokal, später Dienstkonto) für `.gsheet`, `.gslides`, Drive-Prüfung | 0 | E5 | |
| T-012 | Klären, ob `docs/html/ENEO-Prompt Schritt 2.html` gelöscht werden kann (identisch mit `ENEO_Prompt_Schritt2_Stammdaten.html`) | 0 | | |

## Backlog

### Phase 1: Gerüst (Runner, Zustand, Speicher)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-101 | `common/config`: `.env`, `config/*.yaml`, Umgebung über `H2_ENV` | |
| T-102 | `common/contract`: `result.json`, Exitcodes, JSON-Logging, atomares Schreiben | CLAUDE.md Abschnitt 4 |
| T-150 | `orchestration/state`: `state.json` je Teilschritt mit Eingangshashes, Lauf-Manifest (Versionen, Kennwerte-Fassung) | ADR-0002 |
| T-151 | `orchestration/runner`: CLI `h2konzept run`, `status`, `resume`, Haltepunkte H1 bis H5, Neuauslösung über Hashes | ADR-0001 |
| T-152 | Datenanforderungen: Schema, Sammelmeldung je Stufe, `h2konzept decide`, `decisions.jsonl` | ADR-0002 |
| T-153 | „Weglassen" durchreichen: `skipped_by_user`, Eintrag Layer 97, Vorbehalt in Dokumenten, Abschnitt im Übergabebericht | ADR-0002 |
| T-154 | `common/storage`: logische Pfade, Backend `local`, `fetch` in den Run, NFC-Normalisierung, Glob mit Platzhaltern wie `Projektanalyse*` | ADR-0004 |
| T-155 | `common/notify`: Konsole und Datei, Schnittstelle für E-Mail und Slack | E13 |
| T-156 | `orchestration/agent_tasks`: `request.json`, `response.json`, Schemaprüfung, Modus `interactive` | ADR-0005 |
| T-157 | `common/qgis_runner`: QGIS-Interpreter als Unterprozess starten (Pfade aus `.env`, PROJ_DATA prüfen, offscreen) | ADR-0003, Falle Anhang C: falscher proj-Pfad |
| T-158 | Abnahmetest Gerüst: Datei fehlt, Lauf hält an, Nachlieferung, Fortsetzen ohne erneute Ausführung erledigter Teilschritte | M1 |

### Phase 2: Datenrückgrat (Stufe 1, 2)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-103 | `project`: Projektordner über `Projektregister.csv` auflösen | Prompt 1.1 |
| T-104 | `project`: maßgebliche .qgz per XML bestimmen, Arbeitskopie im Run | Prompt 1.3, Anhang E |
| T-105 | `project`: Projektstand 1 bis 5 mit Beleg, Eingangsvertrag nach Anhang A, Lücken als Datenanforderung | `config/inputs.yaml` |
| T-106 | `config/layer_schema.yaml` gegen Anhang D vervollständigen | |
| T-107 | `project`: GeoPackage aus Schema, `00_Metadaten`, QGIS-Projekt headless mit drei CRS | Prompt 6 |
| T-108 | `internal_data`: Nefino-Export finden und laden | Anhang B |
| T-109 | `internal_data`: Planung, Pool- und Potenzialflächen, Flurstücke | |
| T-110 | `internal_data`: PostGIS-Anbindung klären (welche Tabellen, wofür), Abfragen als SQL in `config/` | |
| T-111 | `internal_data`: Parkverbund bis 5 km, nachrichtliche Flächen | Prompt 1.7, 1.8 |
| T-112 | `qa`: `qs_zelle_v10.py` portieren, bekannte Grenzen als Test | Prompt 8.6 |
| T-113 | `qa`: Schemaprüfung | |
| T-118 | `config/inputs.yaml` gegen Anhang A und echte Ordner prüfen | |

### Phase 3: Sachdaten (Stufe 3)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-201 | Quellenkarte Sachdaten: Größe, Datei, Zelle oder Seite | Prompt Schritt 2 |
| T-202 | `facts`: Google-Tabellen per Zellbezug (Skript) | E5 |
| T-203 | Schema `facts.json` | |
| T-204 | Agent `sachdaten_leser` für PDF mit Fundstelle und Zitat | |
| T-205 | Abgleich statt Korrektur: Widersprüche als `intern_unplausibel` oder Rückfrage an H2 | Prompt 2.5, 2.6 |
| T-206 | Kennwerte-Standdatum prüfen, älter als zwölf Monate melden | Grundsätze |

### Phase 4: Fremdquellen (Stufe 4)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-301 | `config/sources/geodatenkatalog.yaml` aus Anhang C und `ENEO_Geodatenkatalog.md` vervollständigen | Endpunkte stehen teils im PDF |
| T-302 | WFS-Client mit Kontrollabfrage, Paging, Cache, Abrufdatum, Datenanforderung bei Ausfall | |
| T-303 | WMS GetFeatureInfo (NIBIS) | |
| T-304 | MaStR-Abfrage (EinheitJson, Header, lokal filtern) | Anhang C |
| T-305 | OSM mit Spiegeln | |
| T-306 | Basis-DLM aus Landesdownload | |
| T-307 | Bauleitplanung und Ratsinformationssysteme als Agentenrecherche mit Nullbefund-Regel | Anhang C |
| T-308 | Prüfregeln 3.4 als Code | |
| T-309 | Snapshot externer Daten je Lauf | E9 |

### Phase 5: Analyse (Stufe 5)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-401 | Radien als Konfiguration | |
| T-402 | Distanzen Luftlinie und Straße | |
| T-403 | Schwellenprüfung je Auslegungsfall | Auftrag A.3 |
| T-404 | Mengenmodell Layer 96 | |
| T-405 | Standortvarianten, Vorschlag durch Agent, Entscheidung H3 | |
| T-406 | Methanol-3-km-Grenze, Direktleitungen, Verbundabstände | |
| T-407 | Zwischenbericht für H3 erzeugen | Prompt Schritt 5 |

### Phase 6: Karten (Stufe 6)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-501 | Layouts als `.qpt` aus der Kartenvorlage ableiten (A3 durch Klonen) | Anhang F1, H4 |
| T-502 | Stile als `.qml`, Themenfarben | Anhang F5 |
| T-503 | `maps` headless: Blatt je Konfiguration, Legende gebaut, CRS vor Export ausgeben | Anhang F, aus `pb.py`, `stil.py`, `eneo_a3.py` |
| T-504 | Icon-Versatz ohne Überschneidung | aus `nah.py` |
| T-505 | Kontaktbogen, Sichtprüfung durch Agent, Mensch an H4 | G2 |

### Phase 7: Dokumente (Stufe 7, 8)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-601 | Vorlagen im Hausstil | Anhang C, E6 |
| T-602 | Kapitelplan Konzeptbegründung als Konfiguration | Prompt 8.2 |
| T-603 | Agent `berichtsautor` | |
| T-604 | Agent `handout_autor` | |
| T-605 | Zahlenabgleich, Namenssuche extern, keine Gedankenstriche und Semikolons | G3 |
| T-606 | Begleitdokumente | aus `beiblatt.py`, `teilnehmer.py` |

### Phase 8: Abschluss und Drive (Stufe 9, 10)

| ID | Aufgabe | Notiz |
|---|---|---|
| T-607 | Übergabebericht aus Lauf-Manifest, `result.json`, `decisions.jsonl` | H5 |
| T-703 | `publish`: Projekt relativ, GPKG abschließen und einbetten, Änderungsliste, Archiv, Übertragung, MD5 und Größe prüfen, Ladetest, `-wal` und `-shm` prüfen | Anhang H1 bis H3, aus `drive.py` |
| T-704 | Einordnung gegen andere Zellen | Prompt Schritt 9 |
| T-705 | Ladetest der übertragenen Projektdatei auf Windows und macOS | |

### Phase 9 und 10: Zweite Zelle, Server

| ID | Aufgabe | Notiz |
|---|---|---|
| T-801 | Lauf auf zweiter Zelle | |
| T-901 | Server-Umgebung: Linux, QGIS 4.2 headless gleiche Punktversion, Schriften, venv | E14 |
| T-902 | Backend `gdrive_api` mit Dienstkonto, Rechte auf geteilte Ablage | ADR-0004 |
| T-903 | Modus `agent_mode: api` über Agent SDK | ADR-0005 |
| T-904 | Benachrichtigung per E-Mail oder Slack mit Link auf Datenanforderung | E13 |
| T-905 | Entscheidungen ohne Terminal: einfacher Weg für `decide` (z. B. Antwort per Chat oder Formular) | |
| T-906 | Start eines Laufs auf dem Server (manuell, geplant oder per Auftrag) | |

## Erledigt

| ID | Aufgabe | Datum |
|---|---|---|
| T-000 | Verzeichnisstruktur, CLAUDE.md, ROADMAP.md, TASKS.md, Konfigurationsentwürfe angelegt | 19.11.2026 |
| T-005 | Anhänge A bis H beschafft (`docs/pdf/ENEO_Konzept_Prompt_2026-11-11-Z.pdf`) | 19.11.2026 |
| T-013 | Datenlücken-Protokoll, QGIS headless, Speicherschicht und Agentenaufgaben als ADR-0002 bis 0005 entworfen, `inputs.yaml` und `storage.example.yaml` angelegt | 19.11.2026 |
