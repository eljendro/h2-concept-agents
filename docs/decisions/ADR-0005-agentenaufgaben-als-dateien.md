# ADR-0005: Agentenaufgaben als Dateien, interaktiv oder per API

* **Status:** vorgeschlagen
* **Datum:** 19.11.2026
* **Entscheidung Nr. in ROADMAP:** E2, E12

## Kontext

Agenten sollen heute über die Claude-Desktop-App (oder Claude Code) arbeiten können, später auf einem Server
ohne Desktop-App.

## Entscheidung

1. Der Runner legt je Agentenaufgabe `<run>/agent_tasks/<id>/request.json` an: Rolle (`agents/<rolle>.md`),
   Eingangsdateien, Ausgabeschema, Grenzen. Status des Laufs: `waiting_agent`.
2. **Modus `interactive`:** Claude (Desktop-App mit verbundenem Ordner oder Claude Code im Terminal) bearbeitet die
   Aufgabe, schreibt `response.json` und ruft `h2konzept resume`. Abrechnung über das Abo, kein API-Schlüssel.
3. **Modus `api`:** der Runner reicht dieselbe Aufgabe an die Claude API (Agent SDK) weiter. Für Serverbetrieb.
4. Der Runner validiert jede Antwort gegen das Schema. Ungültig heißt: Meldung, kein Weiterlaufen.

## Folgen

* Rollenbeschreibungen und Schemas sind die eigentliche Agentenlogik und werden versioniert und getestet.
* Wo der Runner lokal läuft, muss Claude ihn aufrufen können: Claude Code läuft direkt in der Windows-Shell.
  Die Desktop-App (Cowork) arbeitet in einer Linux-Umgebung, die nur die verbundenen Ordner sieht, nicht das
  Windows-QGIS. Dort startet entweder der Nutzer den Runner oder die QGIS-Stufen laufen in einer eigenen Linux-Umgebung.
