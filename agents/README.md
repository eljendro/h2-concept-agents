# agents/

Rollenbeschreibungen, unabhängig vom Framework (Entscheidung E2). Jede Datei hat dieselben Abschnitte:
Auftrag, Eingänge, Ausgänge, Darf, Darf nicht, Haltepunkt, Erfolgskriterium.
Allgemeine Regeln stehen in `CLAUDE.md` Abschnitt 2 und gelten für alle Rollen.

| Rolle | Stufe | Kurz |
|---|---|---|
| `orchestrator` | alle | ruft Stufen auf, wertet `result.json` aus, behandelt Fehler nach Regeln, meldet an Haltepunkten |
| `sachdaten_leser` | facts | liest unstrukturierte Sachdaten (PDF) mit Fundstelle |
| `pruefer` | maps, qa | Sichtprüfung exportierter Karten, Zusammenfassung der Prüfergebnisse |
| `berichtsautor` | report | schreibt die zehn Kapitel der Konzeptbegründung |
| `handout_autor` | handouts | schreibt Zielgruppentexte für die Handouts |
