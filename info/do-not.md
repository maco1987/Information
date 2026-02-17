# Do-not-invent Review-Checkliste für KI-Agenten (Lastenheft → Req/Wirkkette/Architektur/Test)

## A) Ziel
Sicherstellen, dass der Agent **keine Inhalte erfindet**, sondern strikt zwischen **Quelle** und **Engineering-Ableitung** trennt – mit **Traceability**, **Testbarkeit** und **prüfbarer Evidence-Planung**.

---

## B) Verbindliche Tagging-Regeln (müssen IM Output sichtbar sein)

### B1. Kategorien (jede Aussage/Zeile bekommt genau 1 Tag)
- **Fakt (Quelle)**: direkt in LH/Artefakt belegt  
- **Ableitung (Engineering)**: technische Schlussfolgerung/Empfehlung, nicht im LH  
- **Annahme**: bewusst gesetzter Rahmen (muss separat gelistet sein)  
- **Offen**: Information fehlt in Quelle → als Klärfrage  

### B2. Belegformat (für jeden *Fakt*)
Jeder Fakt enthält **mindestens**:
- **Quelle**: Dokumentname/Version  
- **Locator**: Kapitel/Absatz/ID/Seite+Zeile (projektüblich)  
- Optional: kurzes Originalzitat (≤ 1 Satz)

**Beispiel:**  
`[Fakt|LH v1.3, Kap. 4.2, Abs. 3] Fahrzeug verriegelt bei Fahrtbeginn automatisch.`

### B3. Verbotene “Fakt-Sprache” ohne Beleg
Ohne Beleg sind **nicht erlaubt**:
- konkrete Werte: “max./min./≤/≥ … ms, km/h, °C, %”
- konkrete Technologien als Fakt: “CAN/Ethernet/FlexRay/SOME/IP”
- konkrete Komponenten als Fakt: “BCM/Zonencontroller/ECU X”
- Klassifikationen als Fakt: “ASIL …”, “Security Level …”

→ Stattdessen: **Offen** oder **Ableitung** (klar markiert).

---

## C) Output-Mindeststruktur (Gate 0 – Pflicht)
Output ist nur akzeptabel, wenn er enthält:
1. **Faktenliste** (nur belegt, mit Locator)  
2. **Requirement-Atoms Tabelle** (mit Beleg + Tagging)  
3. **Offen/Fragenliste** (priorisiert, mit Impact)  
4. **Annahmenliste** (separat, nummeriert)  
5. **Ableitungenliste** (Architektur/Test als Engineering, getrennt)  
6. Optional: **Konfliktliste** (widersprüchliche Stellen mit Locator)

---

## D) Review-Gates (Pass/Fail)

### Gate 1: Quellen-Disziplin (Traceability)
- [ ] **Jeder Fakt** hat einen **Locator** (Kap/Abs/ID/Seite).  
- [ ] Paraphrasen verändern **nicht** die Bedeutung (keine “Interpretations-Upgrades”).  
- [ ] Kombinierte Fakten nennen **alle** relevanten Locator.  
- [ ] Keine ECU-/Bus-/Signalnamen als Fakt ohne Quelle.

**Fail-Beispiele (Stop):**
- “Timing ist max. 200 ms” ohne Beleg → **Fail**  
- “Feature läuft auf Zonencontroller” ohne LH-Beleg → **Fail**

---

### Gate 2: Atomisierung & Requirement-Qualität
Für jeden Requirement-Atom:
- [ ] **Ein Atom = ein prüfbarer Inhalt** (keine Sammelsätze).  
- [ ] Atom ist **eindeutig** (keine weichen Wörter ohne Definition).  
- [ ] Atom ist **testbar** oder wird als **Offen** markiert (fehlendes Akzeptanzkriterium).  
- [ ] Trigger, Preconditions, Ergebnis, Exceptions sind enthalten oder **Offen**.

**Regel:** Fehlende Kriterien dürfen **nicht erfunden** werden. Nur:
- als **Offen** markieren + **Klärfrage** stellen, oder  
- als **Ableitung (Engineering)** vorschlagen (getrennt vom Fact-Set)

---

### Gate 3: Modes/States & Varianten (typische Halluzinationsquelle)
- [ ] Modes/States nur als Fakt, wenn **belegt** (Ignition, Sleep/Wake, Charging, Degraded …).  
- [ ] Wenn nicht belegt: **Offen** (Klärfrage) oder **Ableitung** (“sollte betrachtet werden”).  
- [ ] Varianten/Märkte/Ausstattung nur als Fakt, wenn belegt.

---

### Gate 4: Wirkketten (E2E) korrekt getrennt
- [ ] **Primary Flow** basiert auf LH-Fakten (Trigger/Output).  
- [ ] Technische Kette (ECUs/Netze/Services) ist **Ableitung** oder **Annahme**, wenn nicht belegt.  
- [ ] Nebenpfade separat: **Diagnose**, **Degradation/Fallback**, **Security** (falls relevant).  
- [ ] Keine Signalperioden, Busauslastungen, Timeout-Werte ohne Beleg.

**Erlaubt:** generische Platzhalter (“ECU A”, “Gateway”, “Netzwerk”) + Annahmenliste.

---

### Gate 5: Architektur- & SW-Statements (AUTOSAR etc.)
- [ ] Architekturentscheidungen sind **nie** als Fakt dargestellt, wenn nicht in Quelle.  
- [ ] Architektur wird als **Optionen** formuliert (z.B. zentral vs. verteilt) + Trade-offs.  
- [ ] AUTOSAR-Zuordnung (Classic/Adaptive, SWC/Service) nur als Fakt mit Beleg, sonst Annahme/Ableitung.

---

### Gate 6: Teststrategie (V&V) ohne Erfindung
- [ ] Teststufen (MIL/SIL/HIL/Fahrzeug) dürfen als **Ableitung** vorgeschlagen werden.  
- [ ] **Keine** konkreten Grenzwerte/Timings/Setups ohne Beleg.  
- [ ] Für jedes Atom existiert ein V&V-Mapping **oder** Atom ist “noch nicht testbar” → **Offen**.  
- [ ] Fault-Injection: Fehlerreaktionen nur als Fakt, wenn belegt; sonst **Offen** (“Was ist erwartete Reaktion?”).

---

### Gate 7: Klärfragen-Qualität (Offen-Liste)
Jede Klärfrage muss:
- [ ] konkret sein (welcher Parameter fehlt)  
- [ ] **Impact** nennen (Testbarkeit, Safety, Timing, Integration, Kosten)  
- [ ] gewünschtes Antwortformat nennen (z.B. “Schwelle + Hysterese + Zeitbedingung”)

---

## E) Stop-the-line Kriterien (sofortiger Rework)
Output ist **nicht verwendbar**, wenn:
1. Zahlen/Schwellen/Timing **ohne Beleg** und nicht als **Offen** markiert  
2. ECU-/Bus-/Signal-/AUTOSAR-Details als Fakt **ohne Quelle**  
3. Vermischung von Fakt und Ableitung (keine klaren Tags)  
4. Fehlende Locator/Traceability bei Fakten  
5. “Finale” Anforderungen ohne Testbarkeit (kein Akzeptanzkriterium, kein Offen-Status)

---

## F) Minimaler Self-Check am Ende (Pflichtblock des Agenten)
Der Agent muss am Ende ausgeben:
- **#Fakten:** n  
- **#Ableitungen:** n  
- **#Annahmen:** n  
- **#Offen/Fragen:** n  
- Top-3 **Risiken** (jeweils mit Tag: Fakt/Ableitung/Offen)  
- Bestätigung: „**Keine unbelegten Fakten enthalten**“ + Hinweis auf Gate-Compliance

---

## G) Tabellen-Template (für DOORS/Jama/Excel)

### G1. Requirement-Atoms Tabelle (Pflichtspalten)
- Atom-ID  
- Beleg (Locator)  
- Originaltext (kurz)  
- Typ (Functional/NF/Interface/Safety/Security/Diag/Variant)  
- Atom (Inhalt)  
- Tag (Fakt/Ableitung/Annahme/Offen)  
- Akzeptanzkriterium (oder Offen)  
- Modes/States (Fakt/Offen)  
- Schnittstellen (Fakt/Offen)  
- V&V Mapping (Stufe + Evidence oder Offen)  
- Offene Punkte  

### G2. V&V-Mapping Tabelle (optional, empfohlen)
- Req/Atom-ID  
- Verifikationsmethode (Review/Analyse/Simulation/Test)  
- Teststufe (MIL/SIL/HIL/Fahrzeug/Regression/Endurance)  
- Stimuli/Setup  
- Messgrößen/Logs  
- Pass/Fail Kriterien  
- Evidence (Report/Trace/Log/Coverage)  
