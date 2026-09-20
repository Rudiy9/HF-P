# CLAUDE.md — Projektgedächtnis

Diese Datei wird von Claude Code zu Beginn jeder Sitzung automatisch gelesen.
Sie hält fest, worum es im Projekt geht und wie weit es ist, damit nicht jedes Mal
der gesamte Kontext neu erklärt werden muss.

---

## Projekt in drei Sätzen

Nebenberuflicher Aufbau einer Spezialisierung auf **mmWave-Radar (60 GHz FMCW)** für
Haushalt und Pflege: Präsenzerkennung, Sturzerkennung und Vitalparameter ohne Kamera.
Einstieg über eigene Messungen mit dem Infineon-Evalboard, später Sensorsimulation und
synthetische Trainingsdaten. Der Wettbewerbsvorteil ist die **Validierung gegen reale
Messungen**, nicht das Modell.

## Dateien im Repo

| Datei | Inhalt |
|---|---|
| `projekt.md` | Vollständiger Projektkontext: Entscheidungen, Hardware, Physik, Literatur, Geschäftsmodell, Risiken |
| `strategie.md` | Projektplan und strategische Einordnung der drei Ausbaustufen |
| `todo.md` | Einkaufsliste und Arbeitsliste mit Checkboxen |
| `CLAUDE.md` | Diese Datei: Projektgedächtnis und Fortschrittsprotokoll |

## Arbeitsprinzipien (gelten für jede Codeänderung)

- **Immer mit Rohdaten arbeiten**, nie mit den Demo-Algorithmen des Herstellers.
  Dort sitzt die Wertschöpfung.
- **Sensorunabhängig implementieren.** Range-Doppler-Verarbeitung ist Physik, nicht
  Herstellerlogik. Kein Infineon-spezifischer Code im Kern, nur in einer dünnen
  Treiberschicht.
- **Gegen Theorie validieren, nicht nur gegen Trainingsdaten.** Der Winkelreflektor mit
  berechenbarem RCS ist der Prüfstein für die gesamte Kette.
- Dokumentation auf **Deutsch**, Code und Bezeichner auf Englisch.
- Zielplattform Raspberry Pi 4 mit **32-Bit armhf (ARMv7)** — das RDK liefert nur dafür
  Wheels. Kein Pi 5, kein 64-Bit-Userspace. Beim Pi 4 `arm_64bit=0` in `config.txt`
  oder Legacy-Image.

---

## Pflegeregel für diese Datei

**Claude aktualisiert diese Datei selbstständig**, ohne dass danach gefragt werden muss.
Konkret:

1. **Nach jeder inhaltlichen Änderung am Repo** (neue Datei, neuer Code, erledigter
   Meilenstein, geänderte Entscheidung) einen Eintrag im Fortschrittsprotokoll ergänzen —
   im selben Commit wie die Änderung.
2. **Format:** `### JJJJ-MM-TT — kurzer Titel`, darunter zwei bis fünf Stichpunkte.
   Nur Fakten, keine Absichtserklärungen.
3. **Abschnitt „Aktueller Stand" oben im Protokoll überschreiben**, nicht ergänzen —
   er beschreibt immer nur das Jetzt.
4. **`todo.md` synchron halten:** erledigte Punkte abhaken, neue Erkenntnisse als neue
   Punkte eintragen.
5. **Offene Fragen und Entscheidungen** im Abschnitt „Offene Entscheidungen" führen und
   dort streichen, sobald sie entschieden sind — mit Datum und Ergebnis im Protokoll.
6. Wird in einer Sitzung nichts Inhaltliches geändert, gibt es auch keinen Eintrag.
   Das Protokoll ist kein Tagebuch, sondern ein Zustandsspeicher.

---

## Aktueller Stand

**Phase:** vor Hardwarebeschaffung
**Code:** noch keiner
**Hardware:** noch nicht bestellt
**Nächster Schritt:** Board bestellen, Termin für Phase 1 im Kalender fixieren; parallel
TI-Application-Note und Sensors-Paper lesen und die Pipeline gegen den öffentlichen
arXiv-Datensatz vorbereiten.

## Fortschrittsprotokoll

### 2026-09-20 — ToDo-Liste und Projektgedächtnis angelegt
- `todo.md` erstellt: Einkaufsliste (Radar, Raspberry Pi 4, Kabel, Messzubehör) mit
  Budgetrahmen 475–660 EUR, dazu Arbeitspakete für Phase 1–4 und offene Punkte
- Steuerrechner auf **Raspberry Pi 4** festgelegt (Pi 5 scheidet wegen des
  64-Bit-Kernels im armhf-Image aus)
- `CLAUDE.md` als Projektgedächtnis mit Fortschrittsprotokoll angelegt
- Noch keine Bestellung ausgelöst

### 2026-09-14 — Strategie dokumentiert
- `strategie.md` angelegt: Projektplan, strategische Einordnung der drei Ausbaustufen,
  Zeitplan mit Meilensteinen, Risiken, Ausblick auf Sensorsimulation und Sim-to-Real
- Vorlage lag als LaTeX-Quelle vor, inhaltsgleich nach Markdown übertragen

### 2026-09-14 — Projektkontext festgehalten
- `projekt.md` angelegt: Ausgangslage, Entscheidung für Radar im Haushalts- und
  Pflegeumfeld, Hardwareauswahl, physikalische Randbedingungen, Arbeitsplan,
  Literatur, Geschäftsmodell, Risiken
- Repository `HF-P` initialisiert, erste Commits

---

## Offene Entscheidungen

- **Bestelldatum und Startdatum Phase 1** — noch offen, größtes Projektrisiko
- **Pi 4 mit 4 GB oder 8 GB** — 4 GB vorgesehen, 8 GB nur bei größeren Datensätzen im RAM
- **Aktiver USB-Hub** — erst nach Messung der Stromaufnahme des Radarboards entscheiden
- **Laser-Entfernungsmesser oder Maßband** — Laser reproduzierbarer, Maßband reicht zum Start
- **Ab Woche 12:** Ausbaustufe 2 (Sensorsimulation) starten oder Thema abbrechen
