# ADR-0004: Speicherschicht mit logischen Pfaden, Drive als Ziel, Server mitgedacht

* **Status:** vorgeschlagen
* **Datum:** 19.11.2026
* **Entscheidung Nr. in ROADMAP:** E3, E5, E11

## Kontext

Heute laufen Skripte lokal (Windows, Referenzlauf auf dem Mac). Später läuft alles auf einem Server und
schreibt Ergebnisse ins Drive. Anhang E und H verlangen: Drive ist Ablage, nicht Werkbank, Übertragung
nur nach Änderungsliste, mit Archiv und Prüfung.

## Entscheidung

1. Code arbeitet nur mit **logischen Pfaden** (`projekt://`, `werkzeug://`, `ablage://`, `vorlagen://`) und `pathlib`.
2. **Backends:** `local` (Drive-for-Desktop-Ordner) und `gdrive_api` (Server, Dienstkonto). Gleiche Schnittstelle:
   `list`, `fetch` (in den Run kopieren), `put` (nur aus `publish`), `stat` (Größe, MD5, Titel).
3. **Eingänge** werden immer in den Run kopiert und dort gelesen. **Ausgänge** entstehen in `<run>/out/drive_set/`.
4. **Google-Dateien** (`.gsheet`, `.gslides`) immer über Google-API, in beiden Umgebungen.
5. **`publish`** setzt Anhang H1 bis H3 um, unabhängig vom Backend.
6. **Benachrichtigung** über austauschbare Kanäle (Konsole, Datei, später E-Mail oder Slack).

## Folgen

* Lokal braucht es trotzdem einen Google-API-Zugang (OAuth) für Tabellen und Prüfungen.
* Windows: lange Pfade aktivieren, Run-Ordner kurz halten. Dateinamen NFC-normalisiert vergleichen.
* Projektdatei mit relativen Pfaden und eingebettet in die GeoPackage, damit sie nach der Übertragung überall öffnet.
