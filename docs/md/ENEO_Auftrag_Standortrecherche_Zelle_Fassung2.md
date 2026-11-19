# Rechercheauftrag: Wasserstoff-Standortkonzept für eine neue Zelle

**Fassung 2, Stand 17.11.2026.** Ersetzt die Fassung vom 05.11.2026. Erweitert um
Quellenwege je Schritt, Tabellenvorlagen, Prüfregeln, Anfragetexte und ein
ausgefülltes Beispiel je Tabelle.

Vorlage. Vor dem Start die Platzhalter in spitzen Klammern ausfüllen. Alles, was
in Abschnitt 0 nicht ausgefüllt werden kann, bleibt als „unbekannt" stehen und
wird im Protokoll als Lücke geführt.

---

# 0 Vorab

## 0.1 Die Zelle

| Feld | Wert | Quelle des Werts |
|---|---|---|
| Name der Zelle | <Name> | |
| Projektnummer | <P-xxxx> | ENEO-Projektliste |
| Landkreis, Gemeinde, Gemarkung, Flurstücke | <...> | ALKIS, Pachtverträge |
| Anzahl WEA, Typ, Nennleistung je WEA | <n> x <Typ>, <MW> | Antragsunterlagen |
| Installierte Leistung Park gesamt | <MW> | |
| Jahresertrag netto nach Ertragsgutachten | <GWh/a>, Gutachter <Name>, Stand <Datum> | Ertragsgutachten, Seite <x> |
| Volllaststunden daraus | <h/a> = Jahresertrag / Leistung | [U] |
| Stundenzeitreihe vorhanden? | ja / nein / angefragt am <Datum> | |
| Netzverknüpfungspunkt, Spannungsebene, Netzbetreiber | <...> | Netzanschlusszusage oder Anfrage |
| Stand Genehmigung Park | <Vorbescheid / Antrag / Genehmigung, Datum> | |
| Geplante Inbetriebnahme Park | <Quartal/Jahr> | Projektplan |
| EEG-Ausschreibung geplant? | ja / nein / offen | Entscheidung GF |
| Nefino-Export für die Zelle vorhanden? | ja (Radius 10 km) / nein | |
| QGIS-Projekt, Layer `20_Biogas_CO2` befüllt? | ja / nein | |
| Flächen für Elektrolyse in ENEO-Hand oder in Verhandlung? | <Flurstücke> / nein | |

**Beispiel ausgefüllt (Hymendorf, P-3196):** 35 MW, 3.011 VLS, Zeitreihe nicht
vorhanden, EEG-Ausschreibung offen, Nefino 10 km vorhanden.

## 0.2 Was als Grundlage gilt

Die folgenden Dateien sind **Ground Truth** für alle Kennwerte. Aus ihnen wird
abgefragt, nicht neu recherchiert. Wer eine Zahl braucht, die dort nicht steht,
schreibt „nicht in den Kennwerten, zu erheben bei X" und legt die Lücke in
Protokoll Abschnitt 7 ab.

| Datei | Inhalt | Fassung | Wichtigste Abschnitte für diesen Auftrag |
|---|---|---|---|
| `ENEO_Kennwerte_Kostenbausteine.md` | Elektrolyse, Methanol, CO2-Abscheidung, CO2-Leitung gegen Lkw, H2-Leitung, Netzanschluss, Speicher | 4 | 1 Elektrolyse, 2.1 Wind gegen Synthese, 3.3 Feld `kW`, 3.4 Potenzialprüfung, 4.2 Kipppunktformel, 9 Prüfliste |
| `ENEO_Kennwerte_Anwendungspfade.md` | acht Pfade, Entscheidungsmatrix, Rangfolge | 2 | 0 Befund, 10 Entscheidungsmatrix |
| `ENEO_Kennwerte_Genehmigungsrecht.md` | BImSchG, Störfall-VO, Bauplanungsrecht, Wasserrecht, Ex-Schutz, Leitungen | 2 | 1 Verfahren, 2 Störfall, 3 Bauplanung, 4.3 Wasserrecht Systematik, 4.4 OVG-Urteil, 6 Leitungen |
| `ENEO_Kennwerte_Markt_Nebenprodukte_Foerderung.md` | Sauerstoff, Abwärme, Marktpreise, Förderung, Beihilfefrage | 2 | 3 Marktpreise, 4 Förderkulissen, 6 Beihilfefrage |
| `ENEO_Kennwerte_RFNBO.md` | Strombezugskriterien, Direktleitung, 36-Monats-Regel | 17.09.2026 | gesamt, **Änderungsliste `ENEO_Aenderungsliste_RFNBO.md` beachten, Freigabe offen** |
| `ENEO_Kennwerte_Beispielrechnung_Hymendorf.md` | Rechenmuster, Reihenfolge der Rechenschritte | 2 | 2 Wasserstoffkosten, 3.3 Fall C ungünstig, 5 Unstimmigkeiten |

**Belegstatus wie in den Kennwerten:** [P] Primärquelle gelesen, [S]
Sekundärquelle, [U] eigene Umrechnung mit Rechenweg, [offen] nicht belegbar.
Jede Zahl im Protokoll trägt einen dieser vier Marker.

## 0.3 Arbeitsregeln

1. Jede Zahl mit Datenquelle, Link und Stichtag. Bei Kennwerte-Dateien
   zitieren als `Kostenbausteine 4.2`, bei externen Quellen mit URL und
   Abrufdatum. Jede Tabelle im Protokoll hat eine Spalte Quelle. Eine Zeile
   ohne Quelle gilt als [offen].
2. Keine eigenen Schlussfolgerungen. Was nicht belegbar ist, heißt „unbekannt,
   zu verifizieren bei X". Das ist ein Ergebnis, kein Mangel.
3. Bandbreiten statt Punktwerte. Bei jeder Spanne sagen, wovon sie abhängt.
4. Bei Widersprüchen zwischen Zelle-Daten und Kennwerten: nicht auflösen,
   sondern als Rückfrage ins Protokoll Abschnitt 8.
5. Keine Gedankenstriche im Text, weder im Protokoll noch im Chat.
6. Reihenfolge einhalten: Teil A bis E erst vollständig, dann Teil F. Keine
   Konzeptbegründung vor Abschluss der Recherche.
7. In Außentexten (Protokoll Abschnitt 9) keine Abnehmer namentlich, keine
   LOIs, keine DCF-Werte, keine Wettbewerber. Im Protokoll Abschnitt 4 dürfen
   sie stehen.
8. Preise in Euro. Fremdwährungen umrechnen, Kurs und Datum nennen, [U].
9. Keine Zahl aus dem Gedächtnis. Kennwerte-Datei öffnen, Abschnitt zitieren.
10. Nach jedem Teil fünf Zeilen im Chat vorlegen, keine Datei. Erst nach
    Freigabe weiter.
11. Kennwerte-Dateien werden in diesem Auftrag nicht geändert. Wer dort einen
    Fehler findet, schreibt ihn in Protokoll Abschnitt 8 als Rückfrage.

## 0.4 Die vier Werte mit der größten Wirkung

Diese vier Werte entscheiden das Ergebnis. Sie sind zuerst zu klären oder als
Lücke zu kennzeichnen, bevor gerechnet wird.

| Nr | Wert | Warum | Woher | Anfragetext |
|---|---|---|---|---|
| 1 | **Stundenzeitreihe des Ertragsgutachtens** (8.760 Werte, MW je Stunde, P50) | Auslastung der Elektrolyse, Speichergröße, Reichweite bei Flaute, ab 2030 stündliche Korrelation. Ohne Zeitreihe nur Dauerlinienschätzung. | Ertragsgutachter, Anfrage je Zelle | Anhang 1 |
| 2 | **Leistungsangabe der Biogasanlagen im Umkreis** (installiert oder Bemessung) | Faktor 2,06 zwischen beiden (Kostenbausteine 3.3). Feld `kW` im Layer `20_Biogas_CO2` ist undokumentiert. Bis zur Klärung ungünstige Annahme: 1,07 bis 1,41 t CO2 je kW Feldwert, mit Vorbehalt. | Nefino-Ursprungsdatei (Spaltenkopf prüfen), Marktstammdatenregister (dort beide Werte getrennt) | intern |
| 3 | **Anlagentyp der Biogasanlagen** (BHKW oder Aufbereitung auf Biomethan) | Nur Aufbereitungsanlagen geben CO2 konzentriert ab (290 von 9.315 Anlagen, Kostenbausteine 3). Bei BHKW ist CO2 im Abgas verdünnt. | MaStR Feld „Einspeisung Biomethan" bzw. Gasnetzeinspeisung, Betreiberanfrage | Anhang 2 |
| 4 | **Quotenpflichtiger Abnehmer im Umkreis** | Rang 1 der Pfadreihenfolge (Anwendungspfade 10). Entscheidet, ob RFNBO-Qualität bezahlt wird. | Branchenrecherche, Kernnetz-Karte, Gewerbegebiete, IHK-Firmendatenbank | Protokoll 4 |

---

# TEIL A: Park und Elektrolyse

## A.1 Eingangswerte aus der Zelle

Aus 0.1 übernehmen: installierte Leistung, Jahresertrag, Volllaststunden,
Inbetriebnahme Park, Netzverknüpfungspunkt. Tabelle unverändert ins Protokoll
Abschnitt 1.

## A.2 Elektrolysefälle bilden

Vier Fälle wie in der Beispielrechnung Hymendorf Abschnitt 1 und 2,
Kostenbausteine Abschnitt 1:

| Fall | Definition | Zu berechnen | Vermerk |
|---|---|---|---|
| A | Elektrolyse auf volle Parkleistung | MW, Auslastung h/a, kg H2/a, Investition, EUR/kg | Auslastung = VLS des Parks |
| B | Elektrolyse auf Mittellast (Jahresertrag / 8.760 h) | dito | Auslastung ohne Zeitreihe nicht bestimmbar, nur Obergrenze 8.760 h |
| C | Elektrolyse auf Beispiel-Nennlast ohne DCF: Leistung, bei der der Elektrolyseur möglichst viele Stunden über Mindestlast läuft | dito | ohne Zeitreihe: Dauerlinienschätzung, Vermerk „Schätzung ohne Zeitreihe"; Hymendorf: 10,9 MW bei 35 MW Park |
| V | Fall C plus Batteriespeicher zur Glättung | Speichergröße MWh, Zusatzinvestition, EUR/kg | Speichergröße erst mit Zeitreihe, sonst Lücke |

**Rechenwerte verbindlich (Kostenbausteine Abschnitt 1, 7, 8):**

| Größe | Wert | Quelle |
|---|---|---|
| Anlagenverbrauch | 57 kWh/kg (nicht 53, Abweichung zur alten Faustregel vermerken) | Kostenbausteine 1 |
| Mindestlast | AEL 30 %, PEM 15 % | Kostenbausteine 1 |
| Investition 10 MW | AEL 1.849 EUR/kW, PEM 2.477 EUR/kW | DEA, Kostenbausteine 1 |
| Investition 100 MW | AEL 1.254 EUR/kW, PEM 1.765 EUR/kW | DEA, Kostenbausteine 1 |
| Gesamtinvestition FfE | 3.120 EUR/kW | Kostenbausteine 1 |
| O&M | 5,5 % DEA, 3,4 % FfE | Kostenbausteine 1 |
| Stack | 54.000 h, Tausch 500 EUR/kW | Kostenbausteine 1 |
| Annuität | 8,72 % (20 Jahre, 6 %) | Kostenbausteine 8 |
| Speicher | ISE 400 bis 600 EUR/kWh, DEA 288 EUR/kWh, C-Rate 0,5 | Kostenbausteine 7 |

Zwischen 10 und 100 MW linear interpolieren und als [U] kennzeichnen. Unter
10 MW den 10-MW-Wert nehmen und vermerken, dass kleinere Anlagen teurer je kW
sind, Zahl dafür [offen].

## A.3 Schwellen prüfen

| Schwelle | Wert | Folge | Quelle |
|---|---|---|---|
| Vereinfachtes Verfahren 4. BImSchV Nr. 10.26.2 | ab 5 MW Elektrolyse | BImSchG-Genehmigung | Genehmigungsrecht 1 |
| TEHG | ab 5 t H2/d (~12 MW) | Emissionshandelspflicht, kostenlose Zuteilung prüfen | Markt 3.4 |
| Förmliches Verfahren Nr. 10.26.1 | ab 50 t H2/d (~83 MW) | Öffentlichkeitsbeteiligung | Genehmigungsrecht 1 |
| Lagerung H2, 4. BImSchV | 3 t vereinfacht, 30 t förmlich | eigene Anlagennummer | Genehmigungsrecht 1 |
| Störfall-VO untere Klasse | 5 t H2 gelagert | Betriebsbereich, Abstandsgebot | Genehmigungsrecht 2 |
| Störfall-VO obere Klasse | 50 t H2 gelagert | Sicherheitsbericht | Genehmigungsrecht 2 |
| Störfall Methanol | 500 t / 5.000 t | nur wenn Methanol | Genehmigungsrecht 2 |
| Störfall Sauerstoff | 200 t / 2.000 t | nur wenn O2 gelagert | Genehmigungsrecht 2 |

Tabelle je Fall A bis C: welche Schwellen werden überschritten. Tagesmenge
H2 = MW x 24 / 57 x 1.000 kg.

## A.4 RFNBO-Eignung des Parks

| Frage | Antwort | Folge |
|---|---|---|
| Geht der Park in die EEG-Ausschreibung? | aus 0.1 | Nach geltendem Stand der Änderungsliste RFNBO: Beihilfeverbot Art. 5 Buchst. b gilt nur bei Netzbezug; Direktleitung Art. 3 kennt kein Beihilfeverbot. Marktprämie für Direktleitungsstrom nach EEG-Lesart ohnehin nicht möglich (Markt 6.3). Beides als Lesart kennzeichnen, bis die Änderungsliste freigegeben ist. |
| Inbetriebnahme Park gegen Inbetriebnahme Elektrolyse | Abstand in Monaten | 36-Monats-Regel Art. 3, keine Übergangsfrist für Direktleitung (Art. 11 gilt nur Art. 5a/5b) |
| Direktleitung technisch möglich? | Entfernung Park zu Elektrolysestandort, Trasse | Variante 1 statt 2 |
| Park zusätzlich netzgekoppelt? | ja/nein | Smart-Meter-Nachweis, kein Netzstrom in die Elektrolyse |
| Speicher hinter demselben Netzanschlusspunkt? | ja/nein | Art. 2 Nr. 3: Speicher ist keine Erzeugungsanlage; Speicherförderung kein Ausschlussgrund bei Direktleitung (Markt 6.4) |

**Zeitschiene ausweisen:** Spätester Inbetriebnahmetermin Elektrolyse =
Inbetriebnahme Park + 36 Monate. Gegenrechnen in Teil D.2.

---

# TEIL B: CO2-Potenzial im Umkreis

Immer erheben, auch wenn Methanol nach Teil C voraussichtlich ausfällt. Die
CO2-Lage berührt die Standortwahl und ist später Abfrage statt Recherche.

## B.1 Erhebung

| Schritt | Was | Quelle | Weg |
|---|---|---|---|
| 1 | Alle Biogasanlagen im Ring bis 15 km um den Parkmittelpunkt | Nefino-Export (nur 10 km), Marktstammdatenregister für 10 bis 15 km | MaStR: Erweiterte Suche, Energieträger Biomasse, Umkreis um Koordinate oder PLZ-Liste der Gemeinden im Ring; Export CSV |
| 2 | Je Anlage: Feldwert `kW`, Herkunft (installiert / Bemessung / unbekannt) | Layer `20_Biogas_CO2`, Nefino-Ursprungsdatei, MaStR (Bruttoleistung und Nettonennleistung getrennt) | Spaltenkopf der Nefino-Datei abschreiben, ins Protokoll |
| 3 | Je Anlage: Anlagentyp BHKW oder Aufbereitung | MaStR (Biomethaneinspeisung), Betreiberanfrage | Anhang 2 |
| 4 | Je Anlage: Entfernung Luftlinie und Straße zum Elektrolysestandort | QGIS | Luftlinie aus Layer, Straße aus Routing oder Schätzung Luftlinie x 1,3 [U] |
| 4a | Je Anlage: Lage in Wasserschutzgebiet und Zone | Umweltkarten Niedersachsen (NLWKN), Layer Wasserschutzgebiete | SchuVO Nds. verbietet Biogasanlagen in Zone II/III; OVG Lüneburg 13 KN 67/14 (Genehmigungsrecht 4.4). Bestehende Anlagen in Zone III: Erweiterung um Abscheidung fraglich, als Rückfrage führen |
| 5 | Je Anlage: Inbetriebnahmejahr, EEG-Restlaufzeit | MaStR | Anlagen vor 2010 enden bis 2030 in der Förderung, Kandidaten für Umrüstung oder Stilllegung |
| 6 | Vorhandene CO2-Nutzung oder Verflüssigung | Betreiberanfrage, Nachrichtenlage | |

## B.2 Anlagentabelle (Vorlage)

| Nr | Betreiber | Ort | Feldwert kW | Herkunft | Typ | km Luft | km Straße | WSG Zone | IBN | t CO2/a ungünstig | t CO2/a günstig | Kipppunkt km | Lkw oder Leitung |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | <...> | <...> | 750 | unbekannt | BHKW | 4,2 | 5,5 | keine | 2009 | 803 bis 1.058 | 1.650 bis 2.175 | 2,1 | Lkw |

Ungünstig: Feldwert x 1,07 bis 1,41. Günstig: Feldwert x 2,2 bis 2,9 (nur
nach Klärung, Kostenbausteine 3). Faustregel 3 t/kW als Obergrenze.

Summen je Ring: 0 bis 5 km, 5 bis 10 km, 10 bis 15 km, getrennt nach BHKW und
Aufbereitung.

## B.3 Leitung gegen Lkw je Anlage

Formel Kostenbausteine 4.2:

Kipppunkt km = (V + 3,80) / (26.150 / M − 0,14)

mit V = Verflüssigungskosten je t (Seeger 117 EUR/t als Referenz, Markt und
Kostenbausteine 3), M = Jahresmenge in t. Lkw-Modell DEA 3,80 EUR/t + 0,14
EUR/t·km, Leitung 6 Zoll 300 EUR/m.

Je Anlage: Entfernung über oder unter Kipppunkt. Dazu zwei Vermerke:

- Leitung unterliegt seit 28.11.2025 dem KSpTG (Planfeststellung oder
  Plangenehmigung, Genehmigungsrecht 6), wirkt zugunsten des Lkw.
  Bagatellschwelle KSpTG [offen].
- Bündelung mehrerer Anlagen auf eine Sammelleitung verschiebt den Kipppunkt
  (bis über 30 km bei großen Mengen, Kostenbausteine 4.2). Nur rechnen, wenn
  drei oder mehr Anlagen in einer Richtung liegen.

## B.4 Methanolbedarf gegenprüfen

Bedarf je Fall A bis C: 1,4 t CO2 je t Methanol, 0,192 t H2 je t Methanol
(Kostenbausteine 2). Tabelle: Fall, t H2/a, t Methanol/a, t CO2/a Bedarf,
Potenzial ungünstig im Ring 0 bis 10 km, Deckungsgrad. Wenn Deckung unter
100 %: Fallgröße, die gedeckt wäre (Hymendorf: 4,2 bis 5,6 MW statt 10,9 MW,
Beispielrechnung 3.3).

---

# TEIL C: Abnehmer und Anwendungspfade

Prüfreihenfolge nach Anwendungspfade Abschnitt 10 (Fassung 2):

| Rang | Pfad | Zu klären je Zelle | Quelle | Ausschluss, wenn |
|---|---|---|---|---|
| 1 | Direktabnahme quotenpflichtiger Abnehmer | Industrie im Umkreis mit Wasserstoffbedarf: Raffinerie, Chemie, Stahl, Glas, Düngemittel, Lebensmittel (Fetthärtung), Metallverarbeitung (Schutzgas). Entfernung, Trailer gegen Leitung | IHK-Firmendatenbank, Gewerbegebietslisten der Gemeinden, Branchenverzeichnisse, Unternehmenswebsites; Kostenbausteine 5 (Trailer 183 EUR/t + 3,11 EUR/t·km, Leitung 8 Zoll 0,92 Mio. EUR/km + 35 %) | kein Abnehmer in <x> km; Platzhalter bleibt, kein belegter Radius |
| 2 | H2-Kernnetz | Entfernung zum nächsten Kernnetz-Abschnitt, Inbetriebnahmejahr des Abschnitts, nächster möglicher Einspeisepunkt, Betreiber | Kernnetz-Karte FNB Gas (fnb-gas.de, Stand prüfen), Antrag und BNetzA-Genehmigung 10/2024; Anwendungspfade 5; Entgelt 25 EUR je kWh/h·a (0,20 bis 0,33 EUR/kg) | Anschlussleitung länger als wirtschaftlich (Kostenbausteine 5), Inbetriebnahme Abschnitt nach 36-Monats-Frist |
| 3 | Mobilität | Tankstellen bestehend oder geplant, Flottenbetreiber (ÖPNV, Logistik, kommunaler Fuhrpark, Hafen) im Umkreis, Bedarf t/a, Preisniveau ~8 EUR/kg netto | H2.LIVE Tankstellenkarte, ÖPNV-Betreiber, Kreisverwaltung; Markt 3 | Bedarf unter <x> t/a, kein belegter Schwellwert, Lücke |
| 4 | Methanol | nur wenn Teil B deckt und Rang 1 bis 3 nicht tragen; Mindestlast 40 %, Kaltstart 1 bis 2 Tage, Wind gegen Synthese (Kostenbausteine 2.1) | Anwendungspfade 2, Kostenbausteine 2 | CO2 fehlt oder Methanolabnehmer fehlt |
| 5 | Gasnetz-Überlauf | Netzbetreiber Gas, zulässige Beimischung, Einspeisepunkt | Gasnetzbetreiber | nur als Restweg |
| aus | Ammoniak-Eigensynthese | fällt aus (kleinstes DEA-Datenblatt 229 t/d ≈ 90 MW) | Anwendungspfade 3 | immer |
| aus | e-Kerosin | fällt aus (Referenz 588 MW) | Anwendungspfade 7 | immer |
| aus | Rückverstromung | fällt aus (~30 %) | Anwendungspfade 8 | immer |

## C.1 Abnehmertabelle (Vorlage)

| Nr | Name | Ort | Branche | km | Bedarf t H2/a | Quelle Bedarf | Quotenpflicht | RFNBO nötig | Status Kontakt |
|---|---|---|---|---|---|---|---|---|---|
| 1 | <...> | <...> | Chemie | 18 | unbekannt | keine | ja (RED III Industriequote) | ja | kein Kontakt |

Bedarf nur mit Quelle, sonst „unbekannt". Quotenpflicht: Industrie nach RED
III Art. 22a (42 % bis 2030), Verkehr nach THG-Quote (Markt 3); beschlossene
deutsche Umsetzung der Industriequote [offen], Stand prüfen.

Abnehmer dürfen im Protokoll Abschnitt 4 stehen, in Abschnitt 9 nicht.

## C.2 Kernnetz-Prüfung (Vorlage)

| Frage | Antwort | Quelle |
|---|---|---|
| Nächster Kernnetz-Abschnitt, Name, Betreiber | | FNB-Karte |
| Luftlinie Park zu Abschnitt | km | QGIS |
| Inbetriebnahme Abschnitt laut Plan | Jahr | FNB |
| Neubau oder Umstellung | | FNB |
| Einspeisepunkt vorgesehen? | ja / nein / unbekannt | Betreiberanfrage |
| Anschlussleitung Kosten | km x 0,92 Mio. EUR x 1,35 [U] | Kostenbausteine 5 |
| Entgelt je kg bei Fall C | 0,20 bis 0,33 EUR/kg | Anwendungspfade 5 |
| Mindestmengen, Druck, Reinheit | [offen], Betreiberanfrage | |

## C.3 Entscheidungsmatrix

| Pfad | trägt / bedingt / fällt aus | Begründung in einem Satz | Quelle | offene Punkte |
|---|---|---|---|---|

---

# TEIL D: Standort der Elektrolyse

## D.1 Kandidatenflächen

Zwei bis drei Flächen im Umkreis des Parks. Vorrang: Fläche am
Netzverknüpfungspunkt oder am Parkrand mit kürzester Direktleitung, dann
Gewerbegebiet mit bestehendem Bebauungsplan.

Je Fläche:

| Kriterium | Prüfung | Quelle |
|---|---|---|
| Lage, Flurstücke, Eigentümer, Verfügbarkeit | | ALKIS, ENEO-Pachtstand |
| Entfernung zum Park, Direktleitung | km, Trasse, Querungen (Straße, Bahn, Gewässer) | QGIS |
| Bauplanungsrecht | Außenbereich: § 249a BauGB deckt nur 100 m² und 3,5 m Höhe, also Bebauungsplan nötig (1,5 bis 2 Jahre); Gewerbegebiet: Festsetzungen prüfen (Störfallbetrieb zulässig?) | Genehmigungsrecht 3; Bauleitpläne der Gemeinde |
| Wasserschutzgebiet | Zone I/II/III A/III B, Folgen für Elektrolyse und Lager | Umweltkarten Nds.; Genehmigungsrecht 4.3 |
| Störfall-Abstände | KAS-18 und KAS-63: 50 bis 180 m zu Schutzobjekten je Lagermenge | Genehmigungsrecht 2 |
| Schutzobjekte | Wohnbebauung, Schule, Kita, Krankenhaus, Pflegeheim, Verkehrswege, Naturschutz, Entfernung je Objekt | ALKIS, Ortsbegehung, Schutzgebietskarten |
| Wasserbezug | Menge je Fall (Verbrauch je t H2 nach Kostenbausteine 1), Anschluss, Verfahren § 11c WHG 7 Monate; WasserstoffBG gilt nicht für Wasserentnahme | Wasserversorger; Genehmigungsrecht 4 |
| Abwasser, AwSV | Gefährdungsstufe § 39 AwSV, § 49 AwSV in Schutzgebieten; WGK Methanol, Kalilauge, Trafoöl [offen] | Genehmigungsrecht 4.3 |
| Netzanschluss | Entfernung Umspannwerk, Netzbetreiber, Kosten nur beim Netzbetreiber [offen]; Netzanschlussanfrage Anhang 3 | Kostenbausteine 6 |
| Abwärme-Abnehmer | Nahwärmenetz, Gewerbe, Schwimmbad, Gewächshaus; Temperaturniveau AEL/PEM | Markt 2; Gemeinde, Wärmeplanung |
| Sauerstoff-Abnehmer | Kläranlage, Industrie; Erlös oder Entsorgung | Markt 1 |
| Erschließung | Zufahrt für Trailer und Lkw, Tragfähigkeit | Ortsbegehung |
| Hochwasser, Überschwemmungsgebiet | | Umweltkarten Nds. |

## D.2 Zeitschiene

Rückwärts von der spätesten Inbetriebnahme Elektrolyse (Park + 36 Monate):

| Schritt | Dauer | Quelle | Parallel möglich? |
|---|---|---|---|
| Bebauungsplan | 1,5 bis 2 Jahre | Genehmigungsrecht 3 | ja, parallel zu BImSchG-Antrag |
| BImSchG-Verfahren vereinfacht | nicht veröffentlicht [offen], gesetzliche Frist 3 Monate ab Vollständigkeit, § 10 Abs. 6a BImSchG, in der Praxis länger | Genehmigungsrecht 1 | |
| Wasserrecht § 11c WHG | 7 Monate | Genehmigungsrecht 4 | ja |
| Netzanschluss | [offen], Netzbetreiber | | |
| Lieferzeit Elektrolyseur | [offen], Herstelleranfrage | | |
| Bauzeit | [offen] | | |
| Zertifizierung ISCC EU, Erstaudit | [offen] | RFNBO 7 | vor erster Lieferung |

Ergebnis in einem Satz: Frist haltbar, knapp oder nicht haltbar, und woran es
hängt. Wenn unklar, so sagen.

---

# TEIL E: Rechnung

Nach Muster `ENEO_Kennwerte_Beispielrechnung_Hymendorf.md`, Reihenfolge:

1. Eingangswerte (Teil A) tabellieren
2. Wasserstoffkosten je Fall A, B, C: AEL, PEM, FfE-Variante, in EUR/kg, mit
   Rechenweg (Investition, Annuität, O&M, Stack, Strom zu 0 EUR Opportunität
   oder zu <x> EUR/MWh, beides ausweisen)
3. Sensitivität Auslastung: Fall C mit VLS der Zelle, mit plus 50 % und plus
   75 % (Hymendorf als Vergleich: 3.011 h → 8,82 EUR/kg AEL; 4.632 h → 7,08;
   5.365 h → 6,63)
4. Nur wenn Methanol nicht ausgeschlossen: Methanolkosten je t (Hymendorf
   1.850 bis 2.580 EUR/t), Fall C ungünstig mit CO2-Begrenzung
5. Wind gegen Synthese (Kostenbausteine 2.1): Tagesbedarf H2, Reichweite
   Puffer 3 t und 5 t, drei Auswege benennen
6. Einordnung gegen Marktpreise mit Stichtag aus Markt 3: Hydex grau ~4,1
   EUR/kg (07/2026), grün 4,9 bis 5,5, Tankstelle ~8 EUR/kg netto,
   Wasserstoffbank Auktion 3 Prämie 0,44 bis 3,49 EUR/kg, Auktion 4 Ende 2026
7. Kernnetz-Entgelt und Transportkosten zum Abnehmer auf EUR/kg aufschlagen
8. Unstimmigkeiten zwischen Zelle-Daten und Kennwerten auflisten

Keine DCF, keine Erlösprognose. Gestehungskosten und Vergleich mit Markt
genügen.

## E.1 Ergebnistabelle (Vorlage)

| Fall | MW | h/a | t H2/a | AEL EUR/kg | PEM EUR/kg | FfE EUR/kg | plus Transport/Entgelt | Marktvergleich | Quelle je Spalte |
|---|---|---|---|---|---|---|---|---|---|

---

# TEIL F: Protokoll

Datei `Recherche_Zelle_<Name>.md`, 15 bis 25 KB, Aufbau:

| Nr | Abschnitt | Inhalt |
|---|---|---|
| 0 | Befunde | fünf bis acht Sätze: was die Zelle trägt, was nicht, der eine Wert, an dem es hängt |
| 1 | Eingangswerte | Tabelle 0.1 ausgefüllt, Lücken markiert |
| 2 | Park und Elektrolyse | Teil A mit Schwellentabelle und RFNBO-Tabelle |
| 3 | CO2-Potenzial | Teil B mit Anlagentabelle und Ringsummen |
| 4 | Abnehmer und Pfade | Teil C mit Abnehmertabelle, Kernnetz-Prüfung, Entscheidungsmatrix |
| 5 | Standort | Teil D je Kandidatenfläche, Zeitschiene |
| 6 | Rechnung | Teil E mit Ergebnistabelle |
| 7 | Offene Punkte | Lückenliste „zu erheben bei X", sortiert nach Wirkung |
| 8 | Klärungsliste für ENEO | Fragen, die nur ENEO beantworten kann: EEG-Ausschreibung, Flächen, Netzanschlussauskunft, Freigabe Anfragen Anhang 1 bis 3 |
| 9 | Formulierungsvorschlag Konzeptbegründung | ohne Abnehmernamen, ohne DCF; Pflichtabsätze: Direktleitung und Art. 3, 36-Monats-Frist, Bebauungsplan parallel zum BImSchG-Verfahren, Speicher und Stundenkorrelation ab 2030, Rechtsrahmen in Bewegung; Absatz Wind gegen Synthese nur wenn Methanol nicht ausgeschlossen |
| 10 | Quellen | Tabelle Quelle, Inhalt, Stichtag, Belegstatus |
| 11 | Belegstand | Anteil [P]/[S]/[U]/[offen] in Prozent der Zahlen |

## F.1 Abschlussfragen

Vor Abgabe diese sechs Fragen in je einem Satz beantworten, am Anfang von
Abschnitt 0:

1. Welche der vier Werte aus 0.4 sind geklärt, welche offen?
2. Trägt die Zelle einen Pfad in Rang 1 bis 3, oder bleibt nur Methanol?
3. Ist die 36-Monats-Frist nach heutigem Stand haltbar? Wenn unklar: woran
   hängt es?
4. Gibt es einen Standort-Ausschluss (Wasserschutz, Störfall-Abstand,
   Bauplanungsrecht, Hochwasser)?
5. Welche Zahl in der Rechnung hat die größte Unsicherheit?
6. Welche Rückfrage an ENEO ist die erste, die gestellt werden muss?

## F.2 Prüfregeln vor Abgabe

- Jede Zahl trägt [P], [S], [U] oder [offen].
- Kein Gedankenstrich in der Datei (Suche nach Geviertstrich U+2014 und Halbgeviertstrich U+2013).
- Abschnitt 9 enthält keinen Firmennamen außer ENEO.
- Jede Tabelle hat mindestens die Spalten der Vorlage und eine Spalte Quelle.
- Jede externe Quelle steht mit URL und Abrufdatum in Abschnitt 10.
- Alle Platzhalter in spitzen Klammern sind ersetzt oder als „unbekannt"
  ausgefüllt.
- Die sechs Abschlussfragen sind beantwortet.

---

# Vorgehen

Teil A bis E nacheinander, nach jedem Teil fünf Zeilen im Chat vorlegen, keine
Datei. Teil F erst, wenn A bis E freigegeben sind. Bei jeder Mehrdeutigkeit
zuerst fragen. Was nicht belegbar ist, bleibt Lücke. Anfragen aus Anhang 1 bis
3 werden nur als Entwurf vorgelegt, nicht versendet.

---

# Anhang 1: Anfrage Stundenzeitreihe an den Ertragsgutachter (Entwurf)

Nur Entwurf, Versand durch ENEO.

**Deutsch**

Betreff: Windpark <Name>, Ertragsgutachten vom <Datum>: Stundenzeitreihe

Sehr geehrte Damen und Herren,

für den Windpark <Name> (<n> WEA, <Typ>, <MW>) liegt uns Ihr Ertragsgutachten
vom <Datum> vor. Für eine Auslegungsprüfung benötigen wir ergänzend die
Stundenzeitreihe der Parkleistung für ein repräsentatives Jahr (8.760 Werte,
P50, in MW oder kWh je Stunde, nach Abzug der im Gutachten angesetzten
Verluste), als CSV oder XLSX.

Bitte teilen Sie uns mit, ob die Zeitreihe aus den Gutachtendaten erstellt
werden kann, welche Kosten anfallen und in welchem Zeitraum sie geliefert
werden kann.

Mit freundlichen Grüßen
<Name>, ENEO Gruppe GmbH

**English**

Subject: Wind farm <Name>, yield assessment dated <date>: hourly time series

Dear Sir or Madam,

For the wind farm <Name> (<n> turbines, <type>, <MW>) we hold your yield
assessment dated <date>. For a sizing study we additionally require the hourly
time series of park output for a representative year (8,760 values, P50, in
MW or kWh per hour, net of the losses applied in the assessment), as CSV or
XLSX.

Please let us know whether the time series can be produced from the assessment
data, what the cost would be and within what period it could be delivered.

Kind regards
<Name>, ENEO Gruppe GmbH

---

# Anhang 2: Anfrage an Biogasbetreiber (Entwurf)

Nur Entwurf, Versand durch ENEO. Erst nach Freigabe der Anlagentabelle.

**Deutsch**

Betreff: Biogasanlage <Ort>: Frage zur Anlagentechnik

Sehr geehrte Frau / Sehr geehrter Herr <Name>,

die ENEO Gruppe GmbH entwickelt im Raum <Gemeinde> ein Windenergieprojekt und
prüft, ob sich in der Umgebung Kohlendioxid aus Biogasanlagen nutzen lässt.
Dafür hätten wir drei Fragen zu Ihrer Anlage in <Ort>:

1. Wird das Biogas vor Ort in einem Blockheizkraftwerk verstromt oder zu
   Biomethan aufbereitet und eingespeist?
2. Wie hoch ist die installierte elektrische Leistung und wie hoch die
   Bemessungsleistung nach EEG?
3. Bis wann läuft die EEG-Vergütung Ihrer Anlage?

Es handelt sich um eine unverbindliche Vorprüfung. Für ein kurzes Telefonat
stehen wir gern zur Verfügung.

Mit freundlichen Grüßen
<Name>, ENEO Gruppe GmbH

**English**

Subject: Biogas plant <place>: question on plant technology

Dear Ms / Mr <Name>,

ENEO Gruppe GmbH is developing a wind energy project in the <municipality>
area and is assessing whether carbon dioxide from biogas plants in the
vicinity could be used. We would have three questions about your plant in
<place>:

1. Is the biogas converted to electricity on site in a CHP unit, or upgraded
   to biomethane and injected into the grid?
2. What is the installed electrical capacity and what is the rated capacity
   (Bemessungsleistung) under the EEG?
3. Until when does the EEG tariff for your plant run?

This is a non-binding preliminary assessment. We would be happy to arrange a
short call.

Kind regards
<Name>, ENEO Gruppe GmbH

---

# Anhang 3: Netzanschlussanfrage Elektrolyse (Entwurf)

Nur Entwurf. Vor Versand mit dem Stand der Netzanschlussanfrage des Parks
abgleichen, damit nicht zwei Anfragen zum selben Punkt laufen.

**Deutsch**

Betreff: Anfrage Netzanschluss Elektrolyseanlage <Gemeinde>, <MW>

Sehr geehrte Damen und Herren,

die ENEO Gruppe GmbH prüft am Standort <Flurstück, Gemarkung, Gemeinde> die
Errichtung einer Elektrolyseanlage mit einer elektrischen Anschlussleistung von
<MW> (Bezug). Der Strom soll vorrangig über eine Direktleitung aus dem
Windpark <Name> (Netzverknüpfungspunkt <UW>) bezogen werden; ein
Netzanschluss ist für Hilfsenergie, Anfahrvorgänge und als Rückfallebene
vorgesehen.

Wir bitten um Auskunft, an welchem Netzverknüpfungspunkt und auf welcher
Spannungsebene ein Anschluss mit <MW> Bezugsleistung möglich wäre, mit welchen
Netzanschlusskosten und Baukostenzuschüssen zu rechnen ist und welche
Bearbeitungsdauer für eine verbindliche Anfrage gilt.

Mit freundlichen Grüßen
<Name>, ENEO Gruppe GmbH

**English**

Subject: Grid connection enquiry, electrolysis plant <municipality>, <MW>

Dear Sir or Madam,

ENEO Gruppe GmbH is assessing the construction of an electrolysis plant with an
electrical connection capacity of <MW> (consumption) at <plot, district,
municipality>. Power is to be sourced primarily via a direct line from the
<Name> wind farm (grid connection point <substation>); a grid connection is
intended for auxiliary power, start-up and as a fallback.

We would be grateful for information on the grid connection point and voltage
level at which a connection with <MW> consumption capacity would be possible,
the expected connection costs and construction cost contributions, and the
processing time for a binding enquiry.

Kind regards
<Name>, ENEO Gruppe GmbH

---

# Anhang 4: Quellenwege

URLs Stand 17.11.2026, bei Abruf prüfen und Abrufdatum im Protokoll vermerken.

| Quelle | Wofür | Zugang | Hinweis |
|---|---|---|---|
| Marktstammdatenregister | Biogasanlagen, Leistung, Typ, Inbetriebnahme | https://www.marktstammdatenregister.de/MaStR/Einheit/Einheiten/OeffentlicheEinheitenuebersicht , erweiterte Filter, CSV-Export | Bruttoleistung und Nettonennleistung getrennt ausgewiesen |
| Umweltkarten Niedersachsen (NLWKN) | Wasserschutzgebiete, Überschwemmungsgebiete, Schutzgebiete | https://www.umweltkarten-niedersachsen.de | WMS in QGIS einbindbar |
| FNB Gas, Kernnetz | Trassen, Inbetriebnahmejahre, Betreiber | https://fnb-gas.de/wasserstoffnetz-wasserstoff-kernnetz/ , Antrag 07/2024, BNetzA-Genehmigung 10/2024 | Stand der Karte prüfen, Änderungen seit 2024 möglich |
| H2.LIVE | H2-Tankstellen bestehend und geplant | https://h2.live | Betreiberangabe |
| DEA Technology Data (ens.dk) | Elektrolyse, Methanol, Ammoniak, CO2-Transport | https://ens.dk/en/analyses-and-statistics/technology-data , PDFs über ens.dk/media/<id>/download | Hauptquelle der Kennwerte, dort schon ausgewertet |
| publications.europa.eu, CELEX 32023R1184 | Delegierte VO RFNBO im Wortlaut | https://publications.europa.eu/resource/celex/32023R1184 | EUR-Lex direkt oft blockiert |
| LAI-Leitfaden Elektrolyse 12/2025 | Genehmigungspraxis | https://www.lai-immissionsschutz.de | schon in Genehmigungsrecht ausgewertet |
| Bauleitpläne der Gemeinde | Bebauungspläne, Flächennutzungsplan | Gemeinde, Landkreis, Geoportal | je Zelle neu |
| IHK-Firmendatenbank, Branchenverzeichnisse | Abnehmerrecherche | IHK Stade / Elbe-Weser, IHK Bremerhaven, je nach Zelle | Bedarf steht dort nie, nur Branche |
