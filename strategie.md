# Projektplan: Einstieg in mmWave-Radar

**Nebenberuflicher Aufbau einer Spezialisierung auf HF-Sensorik**

**Stand:** September 2026

---

## 1. Ausgangslage und Entscheidung

Der Einstieg erfolgt über **Radar im Haushalts- und Pflegeumfeld**: Präsenz- und
Sturzerkennung sowie Vitalparameter ohne Kamera. Ausschlaggebend ist nicht die Marktgröße,
sondern die **kurze Rückkopplungszeit**. Mit einem Evaluierungsboard für rund 200 Euro
entstehen innerhalb weniger Wochenenden eigene Messergebnisse. Alternativen wie
Sim-to-Real-Datengenerierung oder Sensorsimulation erfordern Monate Vorarbeit, bevor
überhaupt etwas Sichtbares vorliegt.

Das Thema ist **echte HF-Physik**: Wellenausbreitung, Mehrwegeffekte, Streuquerschnitte
und Mikro-Doppler-Signaturen. Der nicht-triviale Kern besteht darin, Reflexionen von Wänden
und Möbeln von denen realer Personen zu trennen. Genau hier liegt der fachliche Vorsprung
gegenüber reinen Softwareentwicklern.

---

## 2. Strategische Einordnung

Die drei betrachteten Optionen sind keine Alternativen, sondern bauen aufeinander auf:

1. **Radar verstehen** (Einstieg)
   Physikalisches und messtechnisches Verständnis realer Sensorik.
2. **Radar simulieren**
   Wer die Physik beherrscht, kann Sensormodelle mit realistischem Rauschen,
   Mehrwegeffekten und Streuverhalten aufbauen.
3. **Synthetische Trainingsdaten liefern**
   Simulierte Radardaten speisen das Training von Wahrnehmungs- und
   Robotikmodellen. Der Engpass der Branche sind Daten, nicht Architekturen.

Durch die Wahl von Punkt 1 wird nichts aufgegeben, sondern lediglich der Einstiegspunkt
mit der schnellsten Lernkurve gewählt.

---

## 3. Hardware und Werkzeuge

| Komponente | Zweck | Kosten (ca.) |
|---|---|---|
| Infineon DEMOBGT60TR13CTOBO1 | 60 GHz, 1 Tx / 3 Rx, Winkelauflösung | 170–220 EUR |
| Infineon Radar Development Kit | Python-Zugriff auf Rohdaten | kostenlos |
| Referenzsensor (Brustgurt) | Validierung der Atem-/Herzfrequenz | 50–100 EUR |
| Python, NumPy, SciPy | Signalverarbeitung | kostenlos |

Entscheidend ist die Arbeit mit **Rohdaten** (Chirps), nicht mit den vorgefertigten
Demo-Algorithmen des Herstellers. Infineon weist ausdrücklich darauf hin, dass
anwendungsspezifische Algorithmen separat zu entwickeln sind — genau dort liegt die
eigene Wertschöpfung.

---

## 4. Zeitplan

### Phase 1 — Inbetriebnahme (Wochen 1–2)

- Board bestellen ([Produktseite](https://at.farnell.com/en-AT/infineon/demobgt60tr13ctobo1/demo-eval-board-radar-sensor-60ghz/dp/4035678))
- Toolchain aufsetzen, Rohdaten auslesen
- Erste Range-Doppler-Karte selbst berechnen (FFT über Chirp und Rampe)
- **Meilenstein:** eigene Bewegung im Raum auf dem Bildschirm sichtbar

### Phase 2 — Vitalparameter (Wochen 3–6)

- Phasenextraktion am Range-Bin einer sitzenden Person
- Bandpassfilterung, Trennung von Atmung und Herzschlag
- Validierung gegen Referenzsensor, Fehlerstatistik über mehrere Probanden
- **Meilenstein:** dokumentierte Atemfrequenzmessung mit Messvergleich

### Phase 3 — Sturzerkennung (Wochen 7–12)

- Aufbau einer eigenen Messreihe: Gehen, Setzen, Hinlegen, Sturz
- Mikro-Doppler-Signaturen und Punktwolken-Tracking
- Trennung statischer Clutter (Wände, Möbel) von bewegten Zielen
- **Meilenstein:** funktionierender Klassifikator mit Falsch-Alarm-Rate

### Phase 4 — Sichtbarkeit und Akquise (ab Woche 8, parallel)

- Veröffentlichung der Ergebnisse mit Bildern und Messvergleichen
- Kontaktliste aus Uni- und Industrienetzwerk aufbauen (Ziel: 40–80 Namen)
- Gespräche führen, nicht verkaufen: Wo bestehen HF- und Sensorikprobleme?
- Erste Subunternehmer-Anfragen bei Ingenieurbüros und Robotik-Startups

---

## 5. Erfolgskriterien

- **Woche 2:** Board läuft, eigene Rohdatenverarbeitung funktioniert
- **Woche 6:** Erste validierte Messung, öffentlich dokumentiert
- **Woche 12:** Zwei belastbare Referenzprojekte, 10 geführte Gespräche
- **Monat 6:** Erster bezahlter Auftrag oder klare Absage des Themas

---

## 6. Risiken

- **Themenwechsel vor Abschluss.** Das größte Risiko ist nicht fachlicher Natur.
  Gegenmaßnahme: feste Termine für Phase 1 und 2, kein neues Thema vor Woche 12.
- **Regulatorik.** Vitalparameter für Pflegeanwendungen fallen potenziell unter
  die Medizinprodukteverordnung. Für Prototyp und Lernphase irrelevant, vor jeder
  Vermarktung jedoch früh zu klären.
- **Einarbeitungsaufwand.** Der HF-Hintergrund liegt rund zwei Jahre zurück.
  Realistisch sind zwei bis drei Monate bis zur Arbeitsfähigkeit.
- **Verfügbarkeit der Hardware.** Lieferzeiten schwanken, daher mehrere
  Distributoren vergleichen.

---

## 7. Ausblick: Ausbaustufen 2 und 3

Die Radararbeit ist der Einstieg, nicht das Ziel. Sobald Phase 3 belastbare Ergebnisse
liefert, öffnet sich der Weg zur Sensorsimulation und von dort zur Erzeugung synthetischer
Trainingsdaten. Die Reihenfolge ist nicht beliebig: Ohne eigene Messdaten ist simulierte
Sensorik nicht verkäuflich, weil der Kunde keine Möglichkeit hat, ihr zu vertrauen.
Die Validierung gegen reale Messungen ist der eigentliche Wettbewerbsvorteil.

### Ausbaustufe 2 — Sensorsimulation (ab Monat 6, parallel zum Dienstleistungsgeschäft)

Ziel ist ein physikalisch korrektes Vorwärtsmodell des Radarkanals: von der Szene über
Wellenausbreitung und Streuung bis zum quantisierten ADC-Signal.

- **Kanalmodell:** Ray-Tracing bzw. Shooting-and-Bouncing-Rays für Mehrwege,
  frequenzabhängige Materialparameter von Wand, Glas, Möbeln, Streuquerschnitte
  (RCS) pro Zielklasse
- **Signalkette:** FMCW-Rampen, Phasenrauschen, Kopplung zwischen Tx und Rx,
  Quantisierung, Antennendiagramme des realen Bauteils
- **Bewegte Ziele:** animierte Körpermodelle (z. B. SMPL/AMASS-Bewegungsdaten)
  mit segmentweisen Streueigenschaften, daraus synthetische Mikro-Doppler-Signaturen
- **Werkzeuge:** eigener Ray-Tracer in Python/C++ für den Kern, ergänzend
  Sionna RT (GPU, differenzierbar), MATLAB Radar Toolbox, kommerziell Remcom
  Wireless InSite oder Ansys HFSS SBR+
- **Validierung:** direkter Vergleich simulierter und gemessener
  Range-Doppler-Karten aus Phase 2 und 3 — dieser Schritt ist das
  Unterscheidungsmerkmal gegenüber reinen Softwareanbietern

**Erstes vermarktbares Ergebnis:** ein Datensatzgenerator für Sturzerkennung.
Das Verkaufsargument liegt auf der Hand — echte Stürze lassen sich nicht in
ausreichender Zahl und Variation aufnehmen, simulierte schon.

### Ausbaustufe 3 — Sim-to-Real und synthetische Trainingsdaten (ab Monat 18)

Vision-Language-Action-Modelle benötigen Datenmengen, die real nicht aufzunehmen sind.
Der Engpass der Branche sind derzeit Daten, nicht Modellarchitekturen. Die dafür nötige
Simulation ist klassische Mechanik plus Numerik.

- **Physikseite:** Kontaktdynamik und Reibungsmodelle, deformierbare und
  artikulierte Objekte, Materialparameter-Identifikation aus Messdaten
- **Plattformen:** Isaac Sim, MuJoCo, Genesis — der Wert liegt nicht in der
  Bedienung dieser Werkzeuge, sondern darin, ihre physikalischen Grenzen zu kennen
- **Domain Randomization:** systematische statt zufällige Variation,
  Quantifizierung des verbleibenden Sim-to-Real-Gaps als eigenständige Leistung
- **Multisensor-Szenen:** Radar, ToF-Kameras und Lidar mit realistischem
  Rauschen und Mehrwegeffekten in derselben simulierten Umgebung — hier fließen
  Ausbaustufe 2 und der EM-Hintergrund unmittelbar ein

### Geschäftsmodelle in diesen Stufen

- Datensätze als Lizenzprodukt (einmal erzeugt, mehrfach verkauft)
- Generator-Toolchain beim Kunden, angepasst an dessen Sensorik
- Beratung zur Reduktion des Sim-to-Real-Gaps
- **Validierungsdienstleistung** — die Frage, ob synthetische Daten den
  realen entsprechen, kann kaum jemand beantworten, und sie wird mit jedem
  Robotikprodukt im Markt drängender
