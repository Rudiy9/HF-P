# ToDo — Nächste Schritte

Arbeitsliste zum Projekt. Grundlage: [projekt.md](projekt.md) und [strategie.md](strategie.md).

**Stand:** 21. September 2026
**Aktueller Status:** vor Hardwarebeschaffung, noch kein Code

> Preise sind Richtwerte (Stand September 2026) und vor der Bestellung zu prüfen.
> Die Summen dienen der Budgetplanung.

---

## 1. Einkaufsliste

### 1.1 Radar

- [ ] **Infineon DEMOBGT60TR13CTOBO1** — 60 GHz FMCW, 1 Tx / 3 Rx, Antenne im Chip. Das Kernstück.
      Enthält RadarBaseboardMCU7 + BGT60TR13C-Shield + USB-Kabel. **170–220 EUR**
      [Farnell-Produktseite](https://at.farnell.com/en-AT/infineon/demobgt60tr13ctobo1/demo-eval-board-radar-sensor-60ghz/dp/4035678)
- [ ] **Infineon Radar Development Kit (RDK)** — Software mit Python-Zugriff auf Rohdaten. **kostenlos**
- [ ] Bezugsquellen vergleichen (Lieferzeiten schwanken): Farnell, DigiKey, Mouser, RS Components
- [ ] Vor der Bestellung prüfen: Lieferzeit, Versandkosten, Zoll/Einfuhrumsatzsteuer (DigiKey und Mouser versenden aus den USA)
- [ ] Rechnung für die Buchhaltung des Einzelunternehmens ablegen

### 1.2 Raspberry Pi 4 als Steuerrechner

> **Wichtig:** Das RDK liefert Python-Wheels für **32-Bit armhf (ARMv7)**.
> Deshalb Pi 4 und **nicht** Pi 5: dessen armhf-Bookworm-Image bootet einen 64-Bit-Kernel,
> `uname -a` meldet aarch64, und Architekturprüfungen schlagen fehl.
> Auf dem Pi 4 entweder das Legacy-32-Bit-Image verwenden oder `arm_64bit=0` in `config.txt` setzen.

- [ ] **Raspberry Pi 4 Model B, 4 GB** — 8 GB erst nötig, wenn größere Datensätze im RAM verarbeitet werden. **60–80 EUR**
- [ ] **Original-Netzteil USB-C, 5,1 V / 3 A (15,3 W)** — kein Handy-Ladegerät. Unterspannung bei zusätzlicher USB-Last ist die häufigste Fehlerquelle am Pi. **10–12 EUR**
- [ ] **microSD-Karte 32 GB, Klasse A2** (SanDisk Extreme, Samsung Evo Plus) — A2 wegen der Zugriffszeiten; 32 GB reichen für System und erste Messreihen. **10–15 EUR**
- [ ] **Gehäuse mit aktiver Kühlung** oder Kühlkörper + Lüfter — der Pi 4 drosselt unter Dauerlast, die Signalverarbeitung läuft am Limit. **10–15 EUR**
- [ ] microSD-Kartenleser (USB) — falls der Laptop keinen Slot hat. **8–10 EUR**
- [ ] Zweite microSD-Karte (optional) — Sicherungs-Image eines funktionierenden Systems. Spart bei einer Fehlkonfiguration Stunden. **10 EUR**
- [ ] Aktiver USB-Hub mit eigenem Netzteil (nur falls nötig) — erst kaufen, wenn sich zeigt, dass der Pi das Radarboard nicht stabil versorgt. **20–25 EUR**

**Ersteinrichtung:** headless empfohlen. Der Raspberry Pi Imager schreibt SSH, WLAN und Benutzer
vorab auf die Karte, danach Zugriff per SSH und Jupyter Lab vom Laptop aus. Monitor, Tastatur und
Maus werden dann nicht gebraucht — ein micro-HDMI-Kabel ist trotzdem für den Notfall sinnvoll
(kein Netzwerk, kaputte Konfiguration).

### 1.3 Kabel (nicht vergessen)

- [ ] **USB-A → Micro-USB, Datenkabel, 1 m** — Verbindung Pi 4 zum Radar-Baseboard.
      **Kein reines Ladekabel**, es muss Daten führen. Steckertyp am gelieferten Board gegenprüfen. **6–8 EUR**
- [ ] **USB-Verlängerung A-Buchse/A-Stecker, 2–3 m, aktiv oder gut geschirmt** — das Radarboard steht auf dem
      Stativ, der Pi daneben am Boden. Ohne Verlängerung diktiert die Kabellänge den Messaufbau. **8–12 EUR**
- [ ] **Ethernet-Kabel Cat 6, 3–5 m** — stabiler Headless-Zugriff und schnelles Kopieren der Rohdaten.
      WLAN am Pi bricht bei großen Transfers gerne ab. **5–8 EUR**
- [ ] **Micro-HDMI → HDMI, 2 m** — der Pi 4 hat **micro**-HDMI, nicht Standard- oder Mini-HDMI.
      Ein normales HDMI-Kabel passt nicht. **8–10 EUR**
- [ ] USB-C-Kabel als Ersatz für die Stromversorgung — nur falls nicht fest am Netzteil. **5 EUR**
- [ ] Kabelbinder oder Klettbinder — reproduzierbarer Aufbau und Zugentlastung am Board. **5 EUR**

### 1.4 Messzubehör

- [ ] **Stativ mit 1/4-Zoll-Gewinde** — reproduzierbare Aufstellung, Querformat, Höhe bis ca. 1,5 m. **25–50 EUR**
- [ ] **Halterung/Adapter Board → Stativ** — Klemme oder kleine Platte mit 1/4-Zoll-Gewinde, ggf. selbst gedruckt. **10–15 EUR**
- [ ] **Polar H10 Brustgurt** — Referenzsensor für den Herzschlag. Liefert **RR-Intervalle** statt nur geglätteter BPM,
      deshalb H10 und kein günstigeres Modell. **70–90 EUR**
- [ ] **Aluplatten für den Winkelreflektor**, Kantenlänge 10–15 cm, 1–2 mm stark — berechenbarer Streuquerschnitt (RCS)
      als Prüfstein für die gesamte Verarbeitungskette. **15–25 EUR**
- [ ] Winkelmesser, Metallwinkel, Kleber oder Schrauben — der Reflektor muss exakt rechtwinklig sein,
      sonst stimmt der berechnete RCS nicht. **10–15 EUR**
- [ ] **Laser-Entfernungsmesser** (25–40 EUR) oder Maßband 5 m (8 EUR) — Range-Ground-Truth. Der Laser ist reproduzierbarer.
- [ ] Kreppband — Bodenmarkierungen für wiederholbare Positionen. **5 EUR**
- [ ] **Thermometer/Hygrometer** — Temperatur beeinflusst Frequenzrampe und Phasenrauschen, gehört ins Messprotokoll. **10–15 EUR**
- [ ] Weiche Matte oder Matratze für Phase 3 — Sturzmessungen ohne Verletzungsrisiko. Vorhandenes verwenden.
- [ ] HF-Absorbermaterial (optional, später) — gezielte Mehrwegkontrolle. Erst kaufen, wenn Mehrwege nachweislich stören. **30–80 EUR**

### 1.5 Budget

| Block | Summe (ca.) |
|---|---|
| Radar | 170–220 EUR |
| Raspberry Pi 4 komplett | 110–150 EUR |
| Kabel | 35–50 EUR |
| Messzubehör (ohne Absorber) | 170–255 EUR |
| **Gesamt Grundausstattung** | **485–675 EUR** |

Minimalvariante für den Start — Phase 1 läuft auch ohne Pi direkt am Laptop:
Radar + Kabel + Stativ ≈ 230–290 EUR. Pi und Referenzsensor lassen sich nachkaufen.

---

## 2. Vorbereitung, solange die Hardware unterwegs ist

- [ ] TI Application Note *The Fundamentals of Millimeter Wave Radar Sensors* (Iovescu & Rao) lesen — ca. 20 Seiten, beste kompakte FMCW-Einführung
- [ ] Sensors-Paper *Remote Monitoring of Human Vital Signs Based on 77-GHz mm-Wave FMCW Radar* durcharbeiten (Kandidat zum Nachimplementieren)
- [ ] Öffentlichen Rohdatensatz **arXiv 2405.12659** herunterladen (*Comprehensive mmWave FMCW Radar Dataset for Vital Sign Monitoring*)
- [ ] Python-Umgebung aufsetzen (NumPy, SciPy, Matplotlib, Jupyter) und Repo-Struktur anlegen
- [ ] **Metadatenschema für Messungen festlegen** (JSON/YAML) nach [projekt.md](projekt.md), Abschnitt 6 — vor der ersten Messung, sonst sind die frühen Messreihen später nicht vergleichbar
- [ ] Range-FFT und Doppler-FFT gegen den öffentlichen Datensatz implementieren — **Ziel: am Liefertag existiert bereits Code**
- [ ] Verarbeitungskette sensorunabhängig anlegen, kein Infineon-spezifischer Code im Kern
- [ ] Richards, *Fundamentals of Radar Signal Processing* besorgen (Bibliothek oder Kauf)

---

## 3. Phase 1 — Inbetriebnahme (Woche 1–2)

- [ ] Board auspacken, Lieferumfang prüfen, Steckertyp des USB-Anschlusses dokumentieren
- [ ] RDK auf dem Laptop installieren, Board-Erkennung prüfen
- [ ] Pi 4 aufsetzen: 32-Bit-Image bzw. `arm_64bit=0`, SSH, Jupyter Lab
- [ ] Architektur mit `uname -a` prüfen, RDK-Wheels auf dem Pi installieren
- [ ] **Rohdaten** (Chirps) auslesen und abspeichern — nicht die Demo-Algorithmen verwenden
- [ ] Chirp-Konfiguration programmatisch aus dem RDK auslesen und mit jeder Messung ablegen
- [ ] Grundriss des Messraums mit Maßen aufnehmen (Skizze + Fotos + Materialien der Wände und Möbel)
- [ ] Raumthermometer aufstellen, Temperatur je Messung protokollieren
- [ ] Range-Doppler-Karte selbst berechnen (FFT über Chirp und Rampe)
- [ ] Winkelreflektor bauen und in bekannter Entfernung messen, Range-Achse kalibrieren
- [ ] **Referenzsweep mit dem Winkelreflektor** aufnehmen: Entfernung 0,5–5 m in festen Schritten, dazu Azimutsweep, Leerraummessung davor und danach — Ankerpunkt für die spätere Simulationsvalidierung
- [ ] Kantenlänge, Winkelfehler und Materialstärke des gebauten Reflektors dokumentieren (der berechnete RCS gilt nur für die tatsächliche Geometrie)
- [ ] **Meilenstein:** eigene Bewegung im Raum auf dem Bildschirm sichtbar

## 4. Phase 2 — Vitalparameter (Woche 3–6)

- [ ] Phasenextraktion am Range-Bin einer sitzenden Person
- [ ] DC-Offset-Korrektur (Kreismittelpunkt-Tracking)
- [ ] Bandpassfilterung, Trennung Atmung / Herzschlag
- [ ] Gemeinsame Zeitbasis zwischen Radar und Polar H10 herstellen und prüfen — ohne belastbare Synchronisation ist der Messvergleich wertlos
- [ ] Validierung gegen Polar H10, Fehlerstatistik über mehrere Probanden
- [ ] **Meilenstein:** dokumentierte Atemfrequenzmessung mit Messvergleich

## 5. Phase 3 — Sturzerkennung (Woche 7–12)

- [ ] Eigene Messreihe aufnehmen: Gehen, Setzen, Hinlegen, Sturz
- [ ] Mikro-Doppler-Signaturen (STFT) und Punktwolken-Tracking
- [ ] Statische Clutter (Wände, Möbel) von bewegten Zielen trennen
- [ ] **Meilenstein:** funktionierender Klassifikator mit Falsch-Alarm-Rate

## 6. Phase 4 — Sichtbarkeit (ab Woche 8, parallel)

- [ ] Ergebnisse öffentlich dokumentieren (Bilder, Messvergleiche)
- [ ] Kontaktliste aus Uni- und Industrienetzwerk aufbauen (Ziel 40–80 Namen)
- [ ] Gespräche führen, nicht verkaufen
- [ ] Subunternehmer-Anfragen bei Ingenieurbüros und Robotik-Startups

---

## 7. Offene Punkte

- [ ] **Termin für Phase 1 fest im Kalender eintragen** — das größte Risiko ist der Themenwechsel vor Abschluss, nicht die Technik
- [ ] Stromaufnahme des Radar-Baseboards am Pi prüfen; bei Instabilität aktiven USB-Hub nachrüsten
- [ ] Einwilligung der Probanden für Messreihen klären (Vitaldaten), auch im privaten Umfeld
- [ ] MDR-Relevanz vor einer Vermarktung juristisch klären — für den Prototyp irrelevant
- [ ] Ab Woche 12 entscheiden: Ausbaustufe 2 (Sensorsimulation) starten oder Thema abbrechen
