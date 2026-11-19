# ADR-0001: Deterministischer Runner, Agenten an definierten Stellen

* **Status:** vorgeschlagen
* **Datum:** 19.11.2026
* **Entscheidung Nr. in ROADMAP:** E1

## Kontext

Der bisherige Ablauf ist vollständig prompt-gesteuert (Schritt 1 bis 9). Viele Regeln sind aus
stillen Fehlern im Referenzlauf entstanden. Die Reihenfolge der Schritte ist fest. Gewünscht ist,
dass Agenten Skripte aufrufen und bei Fehlern eingreifen.

## Entscheidung (Vorschlag)

1. Der Runner ist ein Python-Skript (CLI `h2konzept`). Er führt die Stufen nach `config/pipeline.yaml` aus,
   hält den Zustand und braucht selbst kein Sprachmodell.
2. Agenten werden an drei Arten von Stellen gerufen: Textarbeit (Bericht, Handouts), Lesen
   unstrukturierter Sachdaten, Diagnose bei Fehlern und Sichtprüfung.
3. Agenten bekommen Aufgaben als Dateien (ADR-0005). Damit kann ein Agent auch den Runner bedienen
   (interaktiv: `run`, `status`, `resume`), ohne dass die Reihenfolge vom Modell abhängt.
4. Erlaubte Eingriffe sind abschließend aufgezählt (CLAUDE.md Abschnitt 2). Zahlen gelangen nur über Platzhalter in Texte.

## Folgen

* Läufe sind reproduzierbar und billiger, weil kein LLM für reine Taktung läuft.
* Derselbe Runner läuft lokal und auf dem Server.

## Verworfene Alternativen

* Agent als alleiniger Orchestrator: flexibel, aber nicht reproduzierbar, teuer, schwer zu testen.
