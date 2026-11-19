# h2-concept-agents

Automatisierung des ENEO Wasserstoff-Standortkonzepts je Zelle (Windparkprojekt):
QGIS-Projekt, eigene und fremde Geodaten, Analyse, Karten, Sachdaten, Konzeptbegründung, Handouts.

Python-Skripte erledigen alles Deterministische. Agenten rufen die Skripte auf, behandeln
Fehler nach festen Regeln, lesen unstrukturierte Sachdaten und schreiben Bericht und Handouts.

* `CLAUDE.md`: Arbeitsregeln für Agenten und Entwickler, Architektur in Kurzform
* `ROADMAP.md`: Phasen, Meilensteine, offene Entscheidungen
* `TASKS.md`: laufende Aufgabenliste

**Vertraulich.** Repository privat halten. Projektdaten (`data/`, `runs/`, `docs/docx/`) werden nicht eingecheckt.

## Schnellstart (Entwurf)

```bash
python -m venv .venv && . .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -e ".[dev]"
cp .env.example .env                           # ausfüllen
pytest
```
