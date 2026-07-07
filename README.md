# Jobsearch-Routine

Automatisierte, wiederkehrende Jobrecherche als Claude-Code-Cloud-Routine.
Läuft 2× pro Woche (Di + Fr, 07:00) auf Anthropic-Cloud-Infrastruktur.

## Repo-Struktur

```
jobsearch/
├── README.md            # dieses Dokument
├── profil.md            # Kompetenzprofil Max Espelkott (v1.3) – hier einfügen
├── suchprofil.md        # Zielrollen, Keywords, Geografie, Scoring  [Pflege: Max]
├── firmen.md            # kuratierte Karriereseiten                 [Pflege: Max + Routine schlägt vor]
├── quellen.md           # Job-Boards + Query-Templates              [Pflege: Max]
├── routine-prompt.md    # Prompt der Cloud-Routine
├── gesehen.json         # Dedupe-Speicher                           [Pflege: nur Routine]
└── reports/             # ein Report pro Lauf                       [Pflege: nur Routine]
    └── 2026-KW28-di.md
```

**Verantwortlichkeiten (Single Source of Truth):** `suchprofil.md`, `firmen.md` und
`quellen.md` werden ausschließlich von Max gepflegt. Die Routine ändert nur
`gesehen.json` und `reports/`. Vorschläge der Routine (z. B. neue Firmen für
`firmen.md`) landen als Abschnitt im Report, nie direkt in den Stammdateien.

## Setup (Stand 07/2026, Routines = Research Preview)

Voraussetzungen: bezahlter Plan (Pro/Max/Team/Enterprise), Claude Code on the
web aktiviert, GitHub verbunden (sonst `/web-setup` in einer Claude-Code-Session).

1. GitHub-Repo `jobsearch` anlegen, diese Dateien + `profil.md` auf `main`
   committen. Jeder Routine-Lauf klont das Repo frisch vom Default-Branch.
2. Routine anlegen: claude.ai/code/routines → "New routine". Name vergeben,
   Inhalt von `routine-prompt.md` als Prompt einfügen, Modell: Sonnet
   (bei Bedarf später hochstufen).
3. Repository hinzufügen und **"Allow unrestricted branch pushes" aktivieren**
   (PFLICHT: sonst pusht Claude nur auf `claude/`-Branches, `gesehen.json`
   auf main bleibt stehen und die Dedupe-Logik bricht).
4. Cloud-Umgebung: Default-Environment mit offenem Netzwerkzugriff
   (Job-Boards + Karriereseiten müssen erreichbar sein).
5. **Connectoren ausmisten** (alle verbundenen sind standardmäßig drin):
   Indeed behalten, alles andere (insb. Trading-/Finanz-Connectoren, Strava,
   Google Drive) entfernen. Least Privilege – die Routine handelt als du.
6. Trigger "Schedule": Preset "weekly" wählen, dann per CLI
   `/schedule update` die Cron-Expression `0 7 * * 2,5` setzen
   (Di + Fr 07:00, lokale Zeitzone).
7. Mit "Run now" testen, Session live verfolgen, prüfen: Report + gesehen.json
   auf main? Negativ-Filter greift? Dann 2–3 Läufe kalibrieren.

Kontingent-Hinweis: Routine-Läufe zählen gegen die Plan-Usage; Tageslimits
(Pro: 5/Tag, Max: 15/Tag) sind bei 2 Läufen/Woche irrelevant.

## Vor dem ersten Lauf (einmalige Aufgaben)

- [ ] `firmen.md` validieren und vervollständigen: Karriere-URLs prüfen,
      IG-Metall-Tarifbindung recherchieren, weitere Hidden Champions ergänzen
      (Quellen: IHK Nord Westfalen, "Weltmarktführer NRW"-Listen,
      IG-Metall-Bezirk NRW).
- [ ] `profil.md` (Kompetenzprofil v1.3) ins Repo legen.
- [ ] Testlauf manuell auslösen und Report-Format prüfen.
