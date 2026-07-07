# Jobquellen & Query-Templates – v1.3 (Stand: 07.07.2026)

## Schicht 1 – Job-Boards (Breitensuche)

| Quelle | Methode | Hinweis |
|---|---|---|
| Indeed DE + NL | Websuche UND Indeed-Connector parallel | Connector liefert z. T. dünne Ergebnisse (Test 07/2026) – Websuche ist gleichwertiger Kanal, kein Fallback |
| Arbeitsagentur-Jobbörse | Websuche/direkt | offen zugänglich, gut strukturiert, Umkreissuche |
| StepStone | Websuche (Bot-Schutz auf Direktzugriff) | Query: Keywords + Ort |
| LinkedIn Jobs | Websuche | auch EN-Keywords für NL/Remote |
| ingenieur.de (VDI) | Websuche/direkt | ingenieurspezifisch, wenig Rauschen |
| Jobware | Websuche/direkt | stark bei Mittelstand/Ingenieuren |
| Google Jobs | Websuche | Meta-Ebene / Gegenprobe |
| Fraunhofer-Stellenportal | direkt | Filter: Münster (FFB), NRW |

## Query-Matrix

Keyword-Cluster (Nummern = Themencluster aus `suchprofil.md`):

**Jeden Lauf (Kern):**
- Q1: `Simulink Regelungstechnik` (K1)
- Q2: `Funktionsentwickler modellbasierte Entwicklung` (K1/K2)
- Q3: `FOC PMSM Motorsteuerung elektrische Antriebe` (K1)
- Q4 (EN): `model-based development motor control` (K1, NL/Remote)

**Rotation Dienstag:**
- Q5: `Systems Engineering Systemarchitektur E/E` (K3)
- Q6: `Anforderungsmanagement Requirements Engineer DOORS Polarion` (K4)
- Q7: `BMS Batteriemanagement Software Ladesystem` (K5)
- Q8 (EN): `battery management system embedded` (K5, NL/Remote)

**Rotation Freitag:**
- Q9: `Wärmepumpe Regelung Energiemanagement HEMS` (K6)
- Q10: `KI-gestützte Entwicklung Toolchain Automatisierung MBSE` (K7)
- Q11 (EN): `control systems engineer mechatronics` (K1/K2, NL)
- Q12: `Teamleiter modellbasierte Entwicklung Embedded` (K8/[TL])
- Q13: `Funktionale Sicherheit Steuergerät Embedded` (K1/K2)

Ortsanker je Query: `Münster` · `Dortmund` · `Osnabrück` · `Gütersloh` ·
`Enschede/Hengelo/Almelo` (nur EN-Queries) · `remote Deutschland`.
Nicht jede Kombination stumpf abarbeiten – pro Query die 3 plausibelsten
Ortsanker wählen, bei dünnen Ergebnissen erweitern.

Zeitfenster immer explizit: **nur Anzeigen der letzten 4 Tage** (bei 2 Läufen/
Woche), sofern die Quelle Datumsfilter unterstützt; sonst gegen `gesehen.json`
deduplizieren.

## Schicht 1b – Direkte Kanäle am Board vorbei

- **Bundesagentur-Jobsuche** (jobsuche.arbeitsagentur.de, offene API):
  Umkreissuche 100 km um 48727 mit den Kern-Keywords – technisch robusteste
  automatisierbare Quelle, jeden Lauf.
- **ATS-Subdomain-Queries** (Mittelstands-Stellen, die nie auf Boards landen):
  pro Lauf 2–3 Websuchen nach Muster `{Keyword} jobs.personio.de`,
  `{Keyword} softgarden`, `{Keyword} d.vinci` mit rotierenden Kern-Keywords
  (Simulink, Regelungstechnik, Funktionsentwickler, BMS).

## Schicht 2 – Kuratierte Karriereseiten

Alle Einträge aus `firmen.md` mit Status `aktiv`: Karriereseite abrufen, offene
Stellen gegen ALLE Cluster K1–K7 matchen (nicht nur Kern – gerade RE-/SE-Rollen
tauchen bei fast jeder Firma der Liste auf), nur Deltas seit letztem Lauf melden.
Bei Bot-Blockern: Quelle überspringen, im Report unter "Quellenprobleme" vermerken.

## Schicht 3 – Ergänzende Quellen (nur Di-Lauf)

- FH Münster / Uni Münster: Transfer-/Laborstellen (inkl. MEET, K5)
- Westfälische Nachrichten Stellenmarkt (regionale Mittelständler)
- Kununu/Glassdoor: KEINE Jobquelle – nur Kurzcheck (Bewertung, Trend) je A-Treffer

## Vorschlagspflicht der Routine

Stößt die Routine auf einen regional passenden Arbeitgeber, der nicht in
`firmen.md` steht (Industriebetrieb, Hidden Champion, Institut, Batterie-/
Energie-Player), nimmt sie ihn als Vorschlag in den Report auf (Abschnitt
"Firmenvorschläge").

## Changelog

| Version | Datum | Änderung |
|---|---|---|
| v1.0 | 06.07.2026 | Erstversion, 6 Query-Cluster |
| v1.1 | 06.07.2026 | Query-Matrix auf 12 Queries erweitert (K3–K7, TL) mit Di/Fr-Rotation; Schicht 2 matcht explizit alle Cluster |
| v1.2 | 06.07.2026 | Q2 auf "Funktionsentwickler" umgestellt, Q13 FuSi ergänzt; Indeed-Connector nach Test als gleichwertiger Kanal neben Websuche eingestuft |
| v1.3 | 07.07.2026 | Neue Schicht 1b: Bundesagentur-Jobsuche (API) + ATS-Subdomain-Queries (Personio/softgarden/d.vinci) |
