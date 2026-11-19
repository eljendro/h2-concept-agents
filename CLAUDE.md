# CLAUDE.md

Leitdatei für Claude und alle anderen Agenten, die in diesem Repository arbeiten oder von ihm
gesteuert werden. Kurz halten. Bei jeder Architekturentscheidung nachziehen.

**Stand:** 19.11.2026 · **Status:** Entwurf. Entscheidungen in `ROADMAP.md`, Begründungen in `docs/decisions/`.

---

## 1 Zweck und Regelquellen

Automatisierung des ENEO Wasserstoff-Standortkonzepts je Zelle (ein Windparkprojekt mit Umfeld):
vom QGIS-Projekt über eigene und fremde Geodaten, Analyse und Karten bis zur Konzeptbegründung
und den Handouts für Kommune, Abnehmer und Investor (je intern und extern). Ergebnis für den Nutzer:
eine fertige QGIS-Projektdatei zum Öffnen, Karten als A3-PDF, Dokumente als DOCX, alles im Drive.

| Bestand | Ort | Rolle |
|---|---|---|
| **Ursprünglicher Prompt mit Grundsätzen und Anhang A bis H**, Fassung 2026-11-11-Z | `docs/pdf/ENEO_Konzept_Prompt_2026-11-11-Z.pdf` | **maßgebliche Regelquelle** |
| Aufbereitete Prompts Schritt 1 bis 9 | `docs/html/` | Lesehilfe, bei Abweichung gilt das PDF |
| Rechercheauftrag Fassung 2 (17.11.2026) | `docs/md/ENEO_Auftrag_Standortrecherche_Zelle_Fassung2.md` | Rechenregeln, Schwellen |
| Datenquellenliste | `docs/md/ENEO_Datenquellen.md` | Ausgangspunkt für `config/sources/` |
| Werkzeugordner im Drive (`ENEO_Kennwerte.md`, `ENEO_Geodatenkatalog.md`, `Projektregister.csv`, `qs_zelle`, `mkdocx.py`, Kernnetzdaten, Icons) | Drive, siehe `config/storage.example.yaml` | Ground Truth für Kennwerte und Dienste |
| Referenzskripte des Laufs Narthauen-Süd (`pb.py`, `stil.py`, `eneo_a3.py`, `nah.py`, `karte.py`, `verbund.py`, `doku.py`, `drive.py` u. a.) | Drive, Projektordner Narthauen-Süd, `Kartendarstellung Arbeitsdateien/` | Ausgangsmaterial für `maps`, `report`, `publish` |
| Ergebnisse Referenzlauf Narthauen-Süd (P-3241) | `docs/docx/` | Sollergebnis für Abnahmetests. **Vertraulich, nicht in Git** |

## 2 Grundprinzip

**Skript rechnet, Agent urteilt und schreibt, Mensch entscheidet.**

| Art der Arbeit | Wer | Beispiele |
|---|---|---|
| deterministisch | Python-Skript | Projektdatei finden, Daten laden, Dienste abfragen, verschneiden, Mengenmodell, Karten, Dokumente setzen, Prüfskript, Übertragung |
| Urteil, Sprache | Agent | unstrukturierte Sachdaten lesen, Fehler diagnostizieren, Berichtstext, Handouttexte, Sichtprüfung |
| Entscheidung | Mensch | Haltepunkte, Datenlücken (nachliefern, Anbindung prüfen oder weglassen), Änderungsliste vor Drive |

### Verbindliche Regeln für Agenten

1. **Agenten verändern keine Geodaten und keine Zahlen direkt.** Sie rufen Skripte mit Parametern auf.
2. **Keine Zahl im Fließtext ohne Datenquelle.** Agenten schreiben Platzhalter (`{{ h2_t_a }}`) oder
   Fakten-IDs. Werte setzt das Rendering aus Layer 96, 97 oder `facts.json` ein. Die Prüfung gleicht
   jede Zahl im Text gegen die Daten ab.
3. **Datenlücken entscheidet immer der Nutzer** (Abschnitt 5). Ein Agent darf eine Lücke beschreiben
   und Optionen erläutern, aber nie selbst „weglassen" wählen oder einen Ersatz beschaffen.
4. **Dieselbe Operation wird nicht wiederholt** (Grundsatz „Meldung statt Stillstand"). Ausnahme:
   kurzzeitige Netzfehler (Zeitüberschreitung, HTTP 5xx) wiederholt das Skript selbst, höchstens
   zweimal mit Wartezeit. Der Agent wiederholt nicht.
5. **Erlaubte Eingriffe des Agenten bei Fehlern:** Diagnose aus `result.json` und Logs, Ausweichquelle
   aus `config/sources/` vorschlagen oder (falls dort als automatisch freigegeben markiert) wählen,
   Meldung an den Nutzer. **Verboten:** Code während eines Laufs ändern, Ergebnisse von Hand nachbauen,
   Prüfungen abschalten, Schwellen lockern, Widersprüche auflösen.
6. **Bestand bleibt unberührt.** Nichts im Drive wird verändert, gelöscht, umbenannt oder überschrieben,
   außer über die Stufe `publish` nach Änderungsliste und Freigabe.
7. **Leer heißt nicht kartiert.** Ein Nullbefund gilt erst nach erfolgreicher Kontrollabfrage.
8. **Prüfen heißt ansehen.** Karten werden am exportierten Bild geprüft, nicht an Rückgabewerten.
9. **Erst fragen, dann annehmen.** Mehrdeutigkeiten (mehrere Dateistände, unklare Zuordnung, neue Zahl
   gegen alte Aussage) gehen als Rückfrage mit Auswahl an den Nutzer. Dateinamenszusätze wie v2 oder
   final sagen nichts über Aktualität.
10. **Keine Zahl aus dem Gedächtnis.** Kennwerte-Datei öffnen, Abschnitt zitieren, Standdatum prüfen
    (älter als zwölf Monate: melden).

## 3 Bausteine

| Baustein | Was | Ausführung |
|---|---|---|
| **Runner** | Python-CLI `h2konzept` (`run`, `status`, `resume`, `decide`, `publish`). Führt Stufen nach `config/pipeline.yaml` aus, hält Zustand, hält an Haltepunkten und Datenlücken | Skript, kein LLM |
| **Stufen** | ein Modul je Stufe unter `src/h2konzept/` | Skript (venv oder QGIS headless) |
| **Agentenaufgaben** | Runner schreibt `agent_tasks/<id>/request.json`, Agent liefert `response.json` nach Schema | Claude interaktiv (Desktop, Claude Code) oder per API |
| **Speicherschicht** | logische Pfade (`projekt://`, `werkzeug://`, `ablage://`), Backends lokal und Google Drive | Skript |
| **Benachrichtigung** | Meldungen an den Nutzer (Haltepunkt, Datenlücke, Fehler) | lokal Konsole und Datei, später E-Mail oder Slack |

### Agentenaufgaben als Dateien

Der Runner ruft kein Modell direkt, sondern legt eine Aufgabe ab und setzt den Lauf auf `waiting_agent`.
Wer sie bearbeitet, bestimmt der Ausführungsmodus:

* `interactive`: Claude in der Desktop-App oder Claude Code liest die Aufgabe, bearbeitet sie nach der
  Rolle in `agents/<rolle>.md`, schreibt `response.json` und ruft `h2konzept resume`. Kein API-Schlüssel nötig.
* `api`: der Runner reicht die Aufgabe selbst an die Claude API (Agent SDK) weiter. Für Serverbetrieb.

Rolle, Eingänge und Ausgabeschema sind in beiden Modi identisch. Der Runner prüft `response.json`
gegen das Schema, bevor er weitermacht.

## 4 Skript-Vertrag

* **Aufruf:** `python -m h2konzept.<stufe> --run-dir <run> [--substep <id>] [--param wert ...]`
* **Eingänge** nur über die Speicherschicht und nur die in `config/` deklarierten. **Ausgänge** nur im Run-Verzeichnis.
* **Idempotent** und **atomar**: jede Ausgabe erst als temporäre Datei, dann umbenennen.
* **Teilschritte** (`substeps`): jede Stufe ist in Teilschritte zerlegt (z. B. je Quelle, je Layer, je Kartenblatt).
  Jeder Teilschritt ist ein eigener Sicherungspunkt.
* **Exitcodes:**

  | Code | Bedeutung | Lauf danach |
  |---|---|---|
  | 0 | ok | weiter |
  | 10 | ok mit Warnungen | weiter, Warnungen im Lauf |
  | 20 | eigener Bestand fehlt oder unplausibel | Datenanforderung, `waiting_user` |
  | 30 | externe Quelle nicht erreichbar oder Kontrollabfrage fehlgeschlagen | Datenanforderung, `waiting_user` |
  | 40 | Prüfung fehlgeschlagen | Diagnose durch Agent, Meldung, `waiting_user` |
  | 50 | interner Fehler | anhalten, Fehlerbericht für Entwicklung |

* **Ergebnisdatei** `<stufe>.result.json`: `status`, `exit_code`, `substeps` (je Status, Ausgaben mit Hash
  und Objektzahl), `warnings`, `errors` (`code`, `message`, `hint`), `data_requests`, `open_questions`,
  `duration_s`, `versions`.
* **Logs** als JSON-Zeilen nach `<run>/logs/`.

## 5 Lauf-Zustand, Datenlücken, Fortsetzen

**Nichts Erledigtes geht verloren.** Der Zustand liegt in `<run>/state.json` (Stufen und Teilschritte mit
Status `pending`, `done`, `waiting_user`, `waiting_agent`, `skipped_by_user`, `failed` und Eingangshashes).
`h2konzept resume` setzt genau dort fort und führt nur aus, was nicht `done` ist oder dessen Eingänge sich geändert haben.

**Ablauf bei einer Datenlücke:**

1. Das Skript arbeitet alle davon unabhängigen Teilschritte der Stufe zu Ende.
2. Es sammelt alle Lücken der Stufe und schreibt je Lücke `<run>/data_requests/<id>.json`:
   was fehlt, wo es erwartet wurde (logischer und aufgelöster Pfad), wo gesucht wurde, wofür es gebraucht
   wird (Stufen, Layer, Kapitel), Folgen beim Weglassen, ob blockierend (Eingangsvertrag, Anhang A).
3. Der Runner hält an, bevor eine weitere Stufe oder ein Agent läuft, und schickt **eine** Sammelmeldung.
4. Der Nutzer entscheidet je Lücke:

   | Option | gilt für | Wirkung |
   |---|---|---|
   | `nachgeliefert` | eigener Bestand | Nutzer legt Daten an den erwarteten Ort, Teilschritt läuft neu |
   | `anderer_ort` | eigener Bestand | Nutzer nennt Pfad, wird im Lauf gespeichert, Vorschlag zur Übernahme in `config/` |
   | `anbindung_geprueft` | externe Quelle | Nutzer hat Dienst, Zugang oder Netz geprüft, Teilschritt läuft neu |
   | `weglassen` | beide | Teilschritt `skipped_by_user`, Vermerk in Layer 97 und in den Vorbehalten der Dokumente: „nicht erhoben, auf Entscheidung des Nutzers vom <Datum> ausgelassen" |

5. Entscheidung per `h2konzept decide <run> <id> --option ... [--note ...]` oder im Chat. Ein Agent trägt
   eine Entscheidung nur ein, wenn der Nutzer sie ausdrücklich getroffen hat. Jede Entscheidung landet
   mit Zeitpunkt in `<run>/decisions.jsonl` und im Übergabebericht.
6. Bei blockierenden Lücken zeigt die Meldung, welche Stufen beim Weglassen nicht laufen können.
   Entscheiden darf trotzdem nur der Nutzer.

## 6 Pipeline und Haltepunkte

Reihenfolge, Teilschritte, Abhängigkeiten und Datenanforderungen stehen in `config/pipeline.yaml` und
`config/inputs.yaml`. Sachdaten werden **vor** der Analyse gelesen, weil das Mengenmodell P50,
Parkleistung und Volllaststunden braucht.

| Stufe | Modul | Inhalt | Laufzeit | Prompt |
|---|---|---|---|---|
| 1 | `project` | Projektordner auflösen, maßgebliche .qgz per XML, Projektstand, Eingangsvertrag, GeoPackage-Schema | venv + QGIS | 1, 6 |
| 2 | `internal_data` | eigene Geodaten: Planung, Geodaten, Nefino-Export, PostGIS, Parkverbund | venv | 1.4, 1.7, 1.8 |
| 3 | `facts` | Sachdaten: DCF, Angebot, Vertrieb, Ertragsprognose, Eigentümer | venv, Agent für PDF | 2 |
| 4 | `external_data` | Fremdquellen nach Geodatenkatalog | venv | 3 |
| 5 | `analysis` | Verschneidung, Distanzen, Schwellen, Standortvarianten, Mengenmodell | venv, Agent schlägt vor | 4 |
| 6 | `maps` | sieben Kartenblätter, Export, Kontaktbogen, Sichtprüfung | QGIS headless, Agent prüft Bild | 7 |
| 7 | `report` | Konzeptbegründung, zehn Kapitel | Agent schreibt, Skript setzt | 5, 8.2 |
| 8 | `handouts` | sechs Handouts, Begleitdokumente | Skript filtert, Agent schreibt | 8.1, 8.3 |
| 9 | `qa` | Prüfskript, Zahlenabgleich, Namenssuche, Ladetest Projekt, Übergabebericht | venv + QGIS | 8.6, 8.7, 9 |
| 10 | `publish` | Abschluss GeoPackage und Projekt, Änderungsliste, Archiv, Übertragung, Prüfung | venv + QGIS | Anhang H |

| Halt | nach | Mensch prüft |
|---|---|---|
| H1 | Stufe 1 | Eingangsvertrag, Projektstand |
| H2 | Stufe 3 | Stammdaten, Widersprüche, Mehrdeutigkeiten |
| H3 | Stufe 5 | Zwischenbericht, Standortvariante, Auslegungsfall |
| H4 | Stufe 6 | Kontaktbogen der Karten vor dem Dokumentenbau |
| H5 | vor Stufe 10 | Änderungsliste (Anhang H2) mit externen Fassungen, Go für das Drive |

Datenlücken (Abschnitt 5) halten zusätzlich an jeder Stelle an.

## 7 Pfade, Speicher, Server

* **Pfade nur mit `pathlib`**, nie als zusammengesetzte Zeichenketten, nie hart kodiert. Dateinamen
  Unicode-normalisiert (NFC) vergleichen, macOS und Drive liefern teils NFD (das ü in „Süd" kann ein oder zwei Zeichen sein).
* **Logische Pfade** in Konfiguration und Code: `projekt://Projektanalyse/24h Analyse/Planung/`,
  `werkzeug://ENEO_Kennwerte.md`, `ablage://Media/01 Logo/SVG/Logo Green.svg`. Die Speicherschicht löst sie je
  Umgebung auf (`config/storage.example.yaml`): lokal über den Drive-Desktop-Ordner, auf dem Server über die Drive-API.
* **Drive ist Ablage, nicht Werkbank** (Anhang E, H1). Eingänge werden in den Run-Ordner kopiert und von dort gelesen.
  Gebaut wird im Run-Ordner, Ergebnisse liegen in `<run>/out/drive_set/` als Spiegel des Zielordners. Nur `publish` schreibt ins Drive.
* **Google-Dateien** (`.gsheet`, `.gslides`) sind im Drive-Desktop-Ordner nur Verweise. Sie werden immer über die
  Google-API gelesen, auch lokal.
* **Projektdatei portabel:** relative Pfade, Projekt in die GeoPackage eingebettet, keine `-wal` und `-shm` im Ziel.
  Sie muss nach der Übertragung auf Windows und macOS ohne Nacharbeit öffnen (Ladetest in `qa`).
* **Serverbetrieb von Anfang an mitdenken:** keine Abhängigkeit von Benutzerprofil, Laufwerksbuchstaben oder offener
  Anwendung. Zugangsdaten nur aus Umgebungsvariablen. Run-Ordner auf persistentem Speicher. Meldungen über
  austauschbare Benachrichtigung. Schriften (Calibri, Cambria) müssen auf dem Rechner installiert sein, der rendert.

## 8 QGIS

* **QGIS 4.2 LTR, headless.** Kein geöffnetes QGIS nötig. Stufen mit QGIS starten den QGIS-Interpreter als Unterprozess
  mit `QgsApplication([], False)` und `QT_QPA_PLATFORM=offscreen`, für Processing-Algorithmen `qgis_process`.
  Genaue Punktversion steht in `config/settings` und wird nur bewusst angehoben.
* Der QGIS-Interpreter hat kein pip: dort nur stdlib und `qgis`. Alles andere läuft in der venv. Übergabe über Dateien.
* Weiter gültig aus Anhang E und H4: keine Netzzugriffe in QGIS, Projektdateien über die XML lesen, Renderer und
  Beschriftungen neu bauen statt auslesen, kein `embeddedRenderer()` bei Cluster- und Versatzrenderern, Symbollayer als
  GeoPackage, je Blatt eigene Layernamen, Vorlagen ohne Undo-Historie laden, nach jedem Schreiben speichern, drei CRS
  (EPSG:25832) setzen, Ladetest in zweiter `QgsProject()`-Instanz.
* Durch headless hinfällig: Zeitlimit von `execute_code`, `print(1)`-Prüfung, Mac-Pfade des Interpreters.

## 9 Fachliche Festlegungen (Kurzfassung)

* GeoPackage `Zelle_<Projekt>.gpkg`, EPSG:25832, Layerpräfixe und Pflichtfelder nach `config/layer_schema.yaml` (Anhang D).
  Bandbreiten als `_min` und `_max`, numerisch.
* Belegstatus `[P]`, `[S]`, `[U]`, `[offen]`. Konfidenz in Layer 97: `belegt`, `indikativ`, `zu erheben`.
  Herkunft und Prüfquelle sind verschieden.
* Was im Radius liegt, kommt vollständig in den Datensatz. Auswahlregeln steuern nur Bewertung und Darstellung.
* Kartenblätter 1, 2, 3, 4, 5, 7, 8 (6 nicht belegt), A3 quer, `<Projekt>_<Nr>_<Name>[_extern]`.
* Hausstil (Anhang C): Calibri 11 Blocksatz, Überschriften Cambria fett, Logo auf jeder Seite, **keine Gedankenstriche,
  keine Semikolons**, Preise in Euro. Gilt für Dokumente, Legenden, Beschriftungen und Chat.
* **Externe Fassungen** ohne Abnehmer, Partner, Wettbewerber, Ansprechpartner, Flächen- oder Pachtdaten, DCF-Werte, LOIs.
  Gefiltert wird in den Daten. Namenssuche gegen Layer 50 in jedem externen Dokument, 0 Treffer ist Pflicht.

## 10 Verzeichnisse

```
agents/            Rollen der Agenten
config/            pipeline.yaml, inputs.yaml, layer_schema.yaml, storage.example.yaml, sources/
data/              lokal, nicht in Git: raw/, cache/, reference/
docs/              pdf/ (maßgeblicher Prompt), html/, md/, docx/ (nicht in Git), architecture/, decisions/
runs/              <projekt>/<run_id>/: state.json, data_requests/, agent_tasks/, logs/, work/, out/drive_set/
scripts/           Hilfsskripte
src/h2konzept/     ein Modul je Stufe plus common/, qa/, orchestration/, publish/
templates/         qgis/layouts (.qpt), qgis/styles (.qml), report/, handouts/
tests/             unit/, integration/, golden/, fixtures/
```

## 11 Vertraulichkeit

* Repository **privat**. Keine Projektdaten, Namen, Pachtdaten oder DCF-Werte in Git.
* Eigentümer und Ansprechpartner sind natürliche Personen. Agenten bekommen personenbezogene Daten nur,
  wenn die Aufgabe sie braucht (Entscheidung E7).

## 12 Arbeitsweise in diesem Repo

* Vor Arbeitsbeginn `TASKS.md` lesen, danach aktualisieren.
* Entscheidungen als ADR in `docs/decisions/`, in `ROADMAP.md` verlinken.
* Neue Regel aus dem Prompt: zuerst ins Regelinventar, dann umsetzen, dann abhaken. Eine Regel lebt an genau einer Stelle.
* Neue Datenquelle: Eintrag in `config/sources/`, Kontrollabfrage, Test, Fallen notieren.
