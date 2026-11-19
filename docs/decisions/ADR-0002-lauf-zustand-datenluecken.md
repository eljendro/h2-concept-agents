# ADR-0002: Lauf-Zustand, Datenlücken und Fortsetzen

* **Status:** angenommen (Vorgabe Nutzer 19.11.2026)
* **Datum:** 19.11.2026
* **Entscheidung Nr. in ROADMAP:** E8, E15

## Kontext

Daten aus eigenem Bestand liegen nicht immer am erwarteten Ort oder fehlen noch. Externe Dienste
fallen aus oder liefern fehlerhaft. Bis dahin erledigte Arbeit darf nicht verloren gehen.
Ob fehlende Daten nachgeliefert oder weggelassen werden, entscheidet immer der Nutzer.

## Entscheidung

1. **Zustand** in `<run>/state.json`, feinkörnig je Teilschritt, mit Eingangshashes. Ausgaben atomar.
2. **Weiterarbeiten, dann anhalten:** Eine Stufe arbeitet alle unabhängigen Teilschritte zu Ende,
   sammelt alle Lücken und hält dann an. Keine weitere Stufe und kein Agent läuft, bis entschieden ist.
3. **Datenanforderung** je Lücke als `<run>/data_requests/<id>.json`:

   ```json
   {
     "id": "dr-003",
     "stage": "internal_data", "substep": "poolflaeche",
     "kind": "intern_fehlt",
     "what": "Poolfläche als GeoPackage",
     "expected": {"logical": "projekt://Projektanalyse*/24h Analyse/Planung/*Poolflaeche*.gpkg",
                  "resolved": "G:/.../Planung/"},
     "searched": ["G:/.../Planung/", "G:/.../Geodaten/"],
     "needed_for": ["Layer 05", "Blatt 1", "Blatt 4", "Kapitel 2"],
     "if_omitted": "Blatt 4 ohne Poolfläche, Vermerk in Vorbehalten",
     "blocking": false,
     "options": ["nachgeliefert", "anderer_ort", "weglassen"],
     "decision": null
   }
   ```

   `kind`: `intern_fehlt`, `intern_unplausibel`, `intern_mehrdeutig`, `extern_nicht_erreichbar`, `extern_kontrolle_fehlgeschlagen`.
4. **Optionen:** intern `nachgeliefert`, `anderer_ort`, `weglassen`. Extern `anbindung_geprueft`, `weglassen`.
5. **Entscheidung** per `h2konzept decide` oder im Chat, protokolliert in `decisions.jsonl` (wer, wann, Option, Notiz).
   Agenten tragen nur ausdrücklich getroffene Nutzerentscheidungen ein.
6. **Weglassen** wird sichtbar: Teilschritt `skipped_by_user`, Eintrag in Layer 97 (Konfidenz `zu erheben`,
   Aussage „nicht erhoben, auf Entscheidung des Nutzers vom <Datum> ausgelassen"), Vorbehalt in den Dokumenten,
   Abschnitt im Übergabebericht.
7. **Fortsetzen** mit `h2konzept resume`. Es laufen nur Teilschritte, die nicht `done` sind oder deren Eingänge sich geändert haben.

## Folgen

* Ein Lauf kann über Tage offen bleiben, ohne Arbeit zu verlieren.
* Teilschritte müssen sauber getrennt und idempotent sein. Das bestimmt den Zuschnitt der Module.
* Der Grundsatz „Kein nicht erhoben, solange eine Quelle die Antwort führt" bleibt gewahrt, weil die Auslassung als Nutzerentscheidung belegt ist.
