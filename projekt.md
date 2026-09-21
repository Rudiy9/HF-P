# Projektkontext: mmWave-Radar für Haushalt und Pflege

Kontextdatei für Claude Code. Fasst Entscheidungen, Randbedingungen und offene
Punkte aus der bisherigen Planung zusammen.

**Stand:** September 2026
**Status:** Vor Hardwarebeschaffung, noch kein Code

---

## 1. Ausgangslage

- Physiker, Hintergrund aus Uni **und** Industrie
- Fachliche Nähe zu elektromagnetischer Simulation, seit ca. 2 Jahren nicht mehr
  aktiv darin, Wiedereinarbeitung nötig
- Gründung eines Einzelunternehmens, **nebenberuflich**, Hauptjob läuft weiter
- Kein bestehendes Kundennetzwerk, keine Referenzprojekte
- Vorgeschichte: ca. 6 Monate Ideensuche ohne Festlegung

**Wichtigste Randbedingung:** Der laufende Job erlaubt eine lange Einarbeitung ohne
finanziellen Druck. Das Risiko ist nicht Geld, sondern Themenwechsel vor Abschluss.

---

## 2. Entscheidung

Einstieg über **Radar im Haushalts- und Pflegeumfeld**: Präsenzerkennung,
Sturzerkennung, Vitalparameter ohne Kamera.

Auswahlkriterium war **kurze Rückkopplungszeit**, nicht Marktgröße. Board für ca.
200 EUR, erste eigene Messergebnisse innerhalb weniger Wochenenden.

### Dreistufiger Aufbau

| Stufe | Inhalt | Zeitpunkt |
|---|---|---|
| 1 | Radar messen und verstehen | jetzt |
| 2 | Radar simulieren (Ray-Tracing, Kanalmodell, Signalkette) | ab Monat 6 |
| 3 | Synthetische Trainingsdaten, Sim-to-Real für Robotik/VLA | ab Monat 18 |

Die Reihenfolge ist zwingend: Ohne eigene Messdaten ist simulierte Sensorik nicht
verkäuflich. **Validierung gegen reale Messungen ist der Wettbewerbsvorteil.**

---

## 3. Hardware

### Radar

- **Infineon DEMOBGT60TR13CTOBO1** — 60 GHz FMCW, 1 Tx / 3 Rx, Antenne im Chip
- Enthält RadarBaseboardMCU7 + BGT60TR13C-Shield + USB-Kabel
- Preis ca. 170–220 EUR (Farnell, DigiKey, Mouser, RS)
- 3 Rx-Kanäle → Winkelauflösung → Voraussetzung für Tracking und Sturzerkennung

### Zubehör (zusätzlich zu beschaffen)

- Stativ mit 1/4-Zoll-Gewinde (reproduzierbare Aufstellung, Querformat)
- Referenzsensor: Polar H10 (liefert RR-Intervalle, nicht nur geglättete BPM)
- Selbstgebauter Winkelreflektor (Aluplatten, Kantenlänge 10–15 cm) — berechenbarer
  Streuquerschnitt zur Validierung der gesamten Verarbeitungskette
- Maßband oder Laser-Entfernungsmesser für Range-Ground-Truth
- Optional: HF-Absorbermaterial zur gezielten Mehrwegkontrolle

### Host

- **RDK-Plattformen:** Windows 10/11, Ubuntu 22.04, Raspberry Pi (Raspbian Buster)
- **iPad wird nicht unterstützt** (kein nativer USB-Zugriff, keine Wheels)
- Empfehlung für Phase 1–3: Laptop (Windows/Ubuntu), interaktives Debuggen der
  Signalverarbeitung

### Raspberry-Pi-Fallstricke (wichtig)

- Python-Wheels sind für **32-Bit armhf (ARMv7)** gebaut
- **Pi 1 scheidet aus:** ARMv6, zu langsam (700 MHz, 512 MB, USB2 geteilt mit Ethernet)
- **Pi 5 problematisch:** Das armhf-Bookworm-Image bootet einen 64-Bit-Kernel mit
  32-Bit-Userspace, `uname -a` meldet aarch64 → Architekturprüfungen schlagen fehl.
  Bookworm ist Mindestversion, kein Ausweichen auf ältere Images.
- **Pi 4 ist die sichere Wahl:** echtes 32-Bit-System über Legacy-Image oder
  `arm_64bit=0` in `config.txt`
- Headless-Betrieb möglich: SSH + Jupyter Lab auf dem Pi, Browser auf dem iPad

---

## 4. Physikalische Randbedingungen

### Sichtverbindung

60 GHz erfordert praktisch LOS. Materialdämpfung (Größenordnungen, **pro Durchgang**,
Signal durchläuft hin und zurück):

- Gipskarton / dünne Zwischenwand: ~5–15 dB
- Holztür: ~5–20 dB
- Beschichtetes Glas: >20 dB
- Ziegel, Beton: >40 dB, faktisch undurchsichtig

Sauerstoffabsorption (~15 dB/km) ist auf Zimmerdistanz irrelevant — nicht der
limitierende Faktor.

→ **Ein Sensor pro Raum, freie Sicht.** Vitalparameter durch Wände sind mit
Submillimeter-Phasenauflösung aussichtslos.

### Mehrwege

- Reflexionen an glatten Flächen sind bei 60 GHz brauchbar (Verlust nur einige dB)
- Problem ist nicht die Dämpfung, sondern die **Winkelambiguität**: Gespiegelte Ziele
  erscheinen am Spiegelbildort (korrekte Weglänge, falsche Richtung)
- Rückrechnung mit Spiegelquellenmethode bei bekannter Raumgeometrie möglich
  (aktives Forschungsthema, vgl. Around-the-Corner-Radar im Automotive)
- Für Phase 3: Geisterziel-Filterung und statische Clutter-Unterdrückung nötig

### Falls Wanddurchdringung gefordert

Nicht 60 GHz, sondern UWB 3–10 GHz (z.B. Novelda XeThru, 7,3–8,7 GHz). Preis:
schlechtere Entfernungsauflösung, größere Antennen.

---

## 5. Arbeitsplan

### Phase 1 — Inbetriebnahme (Woche 1–2)

- Board in Betrieb nehmen, Toolchain aufsetzen, **Rohdaten** (Chirps) auslesen
- Range-Doppler-Karte selbst berechnen (FFT über Chirp und Rampe)
- Meilenstein: eigene Bewegung im Raum auf dem Bildschirm sichtbar

### Phase 2 — Vitalparameter (Woche 3–6)

- Phasenextraktion am Range-Bin einer sitzenden Person
- DC-Offset-Korrektur (Kreismittelpunkt-Tracking)
- Bandpassfilterung, Trennung Atmung / Herzschlag
- Validierung gegen Referenzsensor, Fehlerstatistik über mehrere Probanden
- Meilenstein: dokumentierte Atemfrequenzmessung mit Messvergleich

### Phase 3 — Sturzerkennung (Woche 7–12)

- Eigene Messreihe: Gehen, Setzen, Hinlegen, Sturz
- Mikro-Doppler-Signaturen (STFT) und Punktwolken-Tracking
- Trennung statischer Clutter (Wände, Möbel) von bewegten Zielen
- Meilenstein: funktionierender Klassifikator mit Falsch-Alarm-Rate

### Phase 4 — Sichtbarkeit (ab Woche 8, parallel)

- Ergebnisse öffentlich dokumentieren (Bilder, Messvergleiche)
- Kontaktliste aus Uni- und Industrienetzwerk: Ziel 40–80 Namen
- Gespräche führen, nicht verkaufen
- Subunternehmer-Anfragen bei Ingenieurbüros und Robotik-Startups

### Erfolgskriterien

| Zeitpunkt | Kriterium |
|---|---|
| Woche 2 | Board läuft, eigene Rohdatenverarbeitung funktioniert |
| Woche 6 | Erste validierte Messung, öffentlich dokumentiert |
| Woche 12 | Zwei Referenzprojekte, 10 geführte Gespräche |
| Monat 6 | Erster bezahlter Auftrag **oder** klare Absage des Themas |

---

## 6. Aufzeichnungsprotokoll für Messdaten

**Gilt ab der ersten Messung in Phase 1.** Jede Messreihe wird so aufgenommen, dass sie
später als Validierungsdatensatz für die Sensorsimulation (Ausbaustufe 2) taugt. Das
kostet zum Messzeitpunkt wenige Minuten; nachträglich ist es nicht rekonstruierbar und
die Messung wäre zu wiederholen.

### Immer mitschreiben

- **Roher ADC-Würfel** (Chirp × Rampe × Rx-Kanal), unverändert und ungefiltert.
  Abgeleitete Darstellungen (Range-Doppler, Punktwolke) werden nie anstelle der
  Rohdaten gespeichert, sondern höchstens zusätzlich.
- **Vollständige Chirp-Konfiguration**: Startfrequenz, Bandbreite, Rampendauer,
  Rampen pro Frame, Frame-Rate, Abtastrate, Anzahl Samples, Rx-Kanäle, Verstärkung.
  Direkt aus dem RDK auslesen, nicht aus der Erinnerung notieren.
- **Sensorpose**: Position im Raum (x, y, z gegen einen festen Raumursprung),
  Blickrichtung, Neigung, Stativhöhe. Bodenmarkierungen mit Kreppband.
- **Raumgeometrie**: Grundriss mit Maßen, Höhe, Position von Wänden, Fenstern, Türen,
  Möbeln und Metallflächen. Einmal pro Messort, als Skizze mit Maßen plus Foto.
- **Materialien** der dominierenden Flächen (Gipskarton, Ziegel, Glas, Holz, Metall) —
  Eingangsgröße für die späteren frequenzabhängigen Materialparameter.
- **Ziel-Ground-Truth**: exakte Entfernungen und Winkel der Ziele, bei Personen
  zusätzlich Körpergröße, Haltung, Kleidung und der Bewegungsablauf.
- **Umgebungsbedingungen**: Temperatur, grob die Luftfeuchte, Datum und Uhrzeit.
  Temperatur, weil die Frequenzrampe und das Phasenrauschen davon abhängen.
- **Referenzsensor-Rohsignal** (Polar H10: RR-Intervalle) mit gemeinsamer Zeitbasis
  zum Radar. Ohne belastbare Synchronisation ist der Messvergleich wertlos.

### Ablage

Pro Messung ein Verzeichnis: Rohdatendatei plus eine maschinenlesbare Metadatendatei
(JSON oder YAML) mit allen obigen Feldern, dazu Skizze und Fotos. Das Metadatenschema
wird **vor** der ersten Messung festgelegt, damit alle Messreihen vergleichbar bleiben.
Lückenlose laufende Nummerierung, keine Messung überschreiben — auch Fehlmessungen
bleiben erhalten und werden als solche markiert.

### Winkelreflektor als absoluter Referenzfall

Der Winkelreflektor ist nicht nur Prüfstein der Verarbeitungskette, sondern der spätere
Ankerpunkt der Simulation: Sein Streuquerschnitt ist analytisch berechenbar. Nur an
diesem Fall lässt sich zeigen, dass das Vorwärtsmodell **absolut** stimmt und nicht bloß
relativ plausibel aussieht.

Deshalb wird er systematisch vermessen, nicht nur einmal zur Kalibrierung:

- Entfernungssweep in definierten Schritten (z. B. 0,5 m bis 5 m)
- Winkelsweep in Azimut, bei bekannter Orientierung des Reflektors
- Kantenlänge, Winkelfehler und Materialstärke des Reflektors dokumentieren —
  der berechnete RCS gilt nur für die tatsächliche Geometrie
- je eine Referenzmessung des leeren Raums (nur Clutter) vor und nach jedem Sweep
- möglichst eine Wiederholung des gleichen Sweeps in einem zweiten Raum, um
  Clutter-Einfluss von Sensoreigenschaften zu trennen

---

## 7. Technische Prinzipien

- **Immer mit Rohdaten arbeiten**, nie mit den Demo-Algorithmen des Herstellers.
  Infineon weist selbst darauf hin, dass anwendungsspezifische Algorithmen separat
  zu entwickeln sind — dort sitzt die Wertschöpfung.
- **Sensorunabhängig implementieren.** Range-Doppler-Verarbeitung ist Physik, nicht
  Herstellerlogik. Schützt vor Abhängigkeit von einem Zulieferer.
- **Gegen Theorie validieren, nicht nur gegen Trainingsdaten.** Winkelreflektor mit
  berechenbarem RCS als Prüfstein für die gesamte Kette.
- **Jede Messung nach dem Aufzeichnungsprotokoll dokumentieren** (Abschnitt 6).
  Rohdaten ohne Metadaten sind für die spätere Simulationsvalidierung wertlos.
- Zu lernende Konzepte: FMCW-Beat-Frequenz, Range-FFT, Doppler-FFT, MIMO/Winkel-FFT,
  CFAR, MTI und statische Clutter-Unterdrückung, Phasenextraktion, DACM,
  Mikro-Doppler/STFT

---

## 8. Literatur

### Bücher

- Mark Richards, *Fundamentals of Radar Signal Processing* — Standardwerk
  (Range-Doppler, CFAR, Clutter)
- Victor C. Chen, *The Micro-Doppler Effect in Radar* — Grundlage der Sturzerkennung
- Moeness Amin (Hrsg.), *Radar for Indoor Monitoring*

### Sofort verfügbar

- TI Application Note: *The Fundamentals of Millimeter Wave Radar Sensors*
  (Iovescu & Rao) — beste kompakte FMCW-Einführung, ca. 20 Seiten

### Papers Vitalparameter

- *Remote Monitoring of Human Vital Signs Based on 77-GHz mm-Wave FMCW Radar*
  (Sensors) — sauberste Darstellung der Verarbeitungskette, erweitertes DACM.
  **Kandidat zum Nachimplementieren.**
- *A high precision vital signs detection method* (Sci Rep) — Unterdrückung von
  Atmungsharmonischen, Medianfilterung + RLS-MUSIC
- *Comprehensive mmWave FMCW Radar Dataset for Vital Sign Monitoring*
  (arXiv 2405.12659) — öffentlicher Rohdatensatz, **nutzbar bevor Hardware ankommt**

### Papers Sturzerkennung

- Kittiyanpunya et al., IEEE Access 2023 — 1D-Punktwolke + Doppler + LSTM,
  60–64 GHz, 3,44 GHz Bandbreite, 4,4 cm Auflösung (direkt übertragbar)
- *Radar-Based Fall Detection Using Micro-Doppler Signatures* (Sensors 2026)
- Sci-Rep-Paper zu Mehrpersonen-Sturzerkennung, Validierung gegen Video-Ground-Truth

**Hinweis:** ML-Papers veralten schnell und sind austauschbar. Der bleibende Vorteil
liegt in Richards und Chen — zu verstehen, *warum* eine Signatur so aussieht.

---

## 9. Geschäftsmodell

### Kostenstruktur (Korrektur eines Missverständnisses)

- Nacktes Bauteil BGT60TR13CE6327XUMA1: **19,72 USD** einzeln bei DigiKey, in
  Stückzahlen deutlich darunter
- Die ~200 EUR des Evalkits sind Entwicklungswerkzeug, kein Vorprodukt
- Realistische Serienstückliste: 25–50 EUR

### Günstigere Alternativen

| Baustein | Leistung | Preis |
|---|---|---|
| Acconeer XM125 (A121) | gepulst, stromsparend, **ein Kanal, kein Winkel** | 23,53 USD |
| RFbeam V-LD1 | Radarmodul SMD | 35,60 USD |
| BGT60LTR11AIP | reines Doppler, keine Entfernung | Evalboard ~149 USD |
| TI IWRL6432 | 60 GHz Low-Power, gleicher Zielmarkt | Evalboard ~174–194 USD |

### Empfohlenes Modell: keine eigene Hardware

Die Stückliste ist nicht das Kostenproblem. Die Brocken sind RED/CE-Funkzulassung,
EMV-Prüfung, Gehäusewerkzeug, Firmwarepflege, Support, Haftung, ggf. MDR.

**Abstufung:**

1. **Algorithmus lizenzieren** ← Einstieg. Null Zulassungsaufwand, null Haftung,
   null Kapitalbindung. Geringere Marge, Partnerabhängigkeit.
2. **Gemeinsames Produkt** — Partner baut und zertifiziert, Erlösteilung
3. **White Label unter eigenem Namen** — beste Marge, aber volle Herstellerpflichten

Bei Weg 3 gilt man rechtlich als Hersteller, auch ohne eigene Fertigung: CE,
Funkanlagenrichtlinie, technische Doku, EU-Konformitätserklärung, Produkthaftung.
Softwareänderungen an einer Funkanlage können eine erneute Konformitätsbewertung
auslösen. Zusätzlich: neue Produkthaftungsrichtlinie (schließt Software ein),
Cyber Resilience Act. **Vor dem ersten Verkauf juristisch klären.**

### Marktzugang ohne Referenzen

- Subunternehmer bei Ingenieurbüros (fragen nach Verfügbarkeit, nicht nach Portfolio)
- Selbst erzeugte Referenzen: öffentlich dokumentierte Messungen
- Publikationen und Institutskollegen als erster Verteiler
- Fachmessen und Anwendertreffen (NAFEMS, Herstellerevents), Vortrag als beste Akquise
- Partnerprogramme der Softwarehersteller

**Funktioniert nicht:** Freelancer-Plattformen, breite Kaltmailings, Website bauen
bevor die Zielgruppe klar ist.

---

## 10. Risiken

| Risiko | Gegenmaßnahme |
|---|---|
| **Themenwechsel vor Abschluss** (größtes Risiko, nicht fachlicher Natur) | Feste Termine Phase 1–2, kein neues Thema vor Woche 12 |
| Fertige Geräte liefern keine Rohdaten | Module statt Geräte kaufen, Rohdatenzugriff vertraglich klären — **im ersten Gespräch, nicht im letzten** |
| Regulatorik (MDR bei Vitalparametern für Pflege) | Für Prototyp irrelevant, vor Vermarktung klären |
| Einarbeitungsaufwand (HF-Wissen ~2 Jahre alt) | 2–3 Monate bis Arbeitsfähigkeit einplanen |
| Hardwareverfügbarkeit | Mehrere Distributoren vergleichen |
| Plattformkonkurrenz NVIDIA (Stufe 3) | Nische in Sensormodellen und Validierung, nicht im Simulator |
| GPU-Bedarf für Ray-Tracing (Stufe 2) | Cloud-Kapazität einplanen |

---

## 11. Nächster Schritt

**Board bestellen. Datum für Phase 1 festlegen.**

Parallel TI-Application-Note und das Sensors-Paper lesen, mit dem öffentlichen
arXiv-Datensatz die Pipeline vorbereiten — dann existiert am Liefertag bereits Code.
