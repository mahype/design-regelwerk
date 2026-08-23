# Lücken im Web-Regelwerk — Analyse und Fahrplan

Abgleich des Bestands (Kapitel 01–12 plus `marke/`) mit sieben etablierten
Designsystemen: **SAP Fiori** (Fachanwendungs-Referenz), **IBM Carbon**,
**Shopify Polaris** (Admin-Oberflächen), **GOV.UK Design System**
(Formular-/Muster-Referenz), **Atlassian**, **Material 3**, **Siemens iX**
(Industrie/Monitoring). Stand 2026-08-17.

Zweck: entscheiden, was `web/` noch definieren muss, bevor es sich
„vollständig" nennen darf. Dieses Dokument ist Arbeitsgrundlage, kein
Regelkapitel — abgearbeitete Punkte werden gestrichen, am Ende wird die
Datei gelöscht.

## Befund in Kürze

Auf Komponenten- und Interaktionsebene ist der Bestand bereits dichter als
die meisten verglichenen Systeme (siehe „Bereits gedeckt"). Die Lücken
liegen eine Ebene darüber und daneben:

1. **Responsive-Verhalten fehlt komplett.** Kein Breakpoint, keine Regel,
   was Shell, Tabellen, Formulare und Modals auf schmalen Screens tun.
   Jedes verglichene System hat hier ein Modell.
2. **Zwei der drei beschlossenen Detail-Formen sind unspezifiziert.**
   Detailseite und Master-Detail-Split existieren nur als Entscheidung in
   `entscheidungen.md` — ohne Anatomie kann niemand regelkonform bauen.
3. **Ausnahmezustände fehlen.** Session-Ablauf, Verbindungsverlust,
   403/404/500, Wartung, neue Version nach Deploy. Die Rückmeldepflicht
   (07) verspricht „niemand bleibt im Unklaren" — genau diese Fälle lösen
   das bisher nicht ein.
4. **Fachmuster sind angerissen, aber nicht ausdefiniert:** Upload,
   Diagramme/Kennzahlen, Wizard, Feldtypen, Export, Live-Daten,
   ungespeicherte Änderungen, Berechtigungen.

---

## Bereits gedeckt — kein Handlungsbedarf

Zur Einordnung, was der Abgleich **bestätigt** hat (Auswahl):

| Standardthema (bei den Vergleichssystemen) | Bei uns geregelt in |
| --- | --- |
| Leerzustände (Dreiklang was/warum/nächster Schritt) | `07` — deckungsgleich mit Carbon/Fiori |
| Meldungs-Taxonomie (Inline-Box vs. Toast, Fehler bleiben) | `07`, `11` — entspricht Atlassians „Messages"-Entscheidungsbaum |
| Lade-/Skeleton-Regeln, garantiertes Ende | `07` — strenger als alle Vergleichssysteme (Timeout-Pflicht) |
| Statusanzeige nie nur über Farbe | `01`, `04` — entspricht Carbons „color + shape" |
| Destruktiv: Bestätigung, Tat benennen, Tipp-Bestätigung | `06` — entspricht Carbons Lösch-Stufenmodell |
| Formular-Validierung (Block + Feldmarkierung, früh) | `05` — Kombination aus GOV.UK-Protokoll und Fiori |
| Hell/Dunkel mit drei Zuständen, Token-Architektur | `marke/` — expliziter als Carbon/Atlassian |
| Sprache/Wortliste/Datumsformate | `03` + Markenprofil — entspricht Atlassian „Content design" |
| Barrierefreiheit mit Prüfpflicht in CI | `10` — nur Fiori/GOV.UK sind vergleichbar verbindlich |
| Breadcrumbs | bewusst keine: Topbar-Kontext + max. zwei Nav-Ebenen ersetzen sie — bleibt so |

Bemerkenswert: **Session-Ablauf und Verbindungsverlust definiert fast kein
großes System** (nur Fiori am Rand). Für uns sind sie trotzdem Pflicht —
wenige eigene Anwendungen, identischer Bedarf überall, und die
Rückmeldepflicht verlangt es logisch.

---

## A — Neue Kapitel (strukturelle Lücken)

### A1 · `13-responsive.md` — Breakpoints und schmale Screens

**✅ Erledigt 2026-08-17** — Kapitel `13-responsive.md` angelegt:
drei Klassen (Grenzen 768/1280), benutzbar ab 320 px, Rail-Automatik in
„mittel", Drawer unter „schmal", Tabellen mit Spalten-Priorität,
Vollbild-Modals, Container Queries für Bausteine, Touch-Regeln.
Breakpoint-Konstanten im Token-Kontrakt, harte Regel 13 in `AGENTS.md`,
Abwägung samt verworfener Alternativen in `entscheidungen.md`.

### A2 · `14-detail-ansichten.md` — Detailseite und Master-Detail-Split

**✅ Erledigt 2026-08-17** — Kapitel `14-detail-ansichten.md` angelegt:
Formen-Wahltabelle, Detailseiten-Anatomie (benannter Zurück-Link statt
Breadcrumb, Kopfzeile mit Badge und Aktionen, Anzeige-Felder,
abschnittsweises Bearbeiten im Modal, kein Blättern zwischen
Datensätzen), Split-Anatomie (360er-Liste ohne Aktionsspalte, Auswahl in
der URL, `↑`/`↓`, Auto-Weiter zum nächsten Offenen). Abwägung samt
verworfener Alternativen in `entscheidungen.md`; Querverweise in
`02`/`04`/`05`/`13`.

### A3 · `15-ausnahmezustaende.md` — Session, Verbindung, Fehlerseiten, Berechtigungen

**✅ Erledigt 2026-08-17** — Kapitel `15-ausnahmezustaende.md` angelegt:
reaktives Anmelde-Modal bei 401 (Eingaben bleiben, kein Timer),
Verbindungsverlust-Banner mit Auto-Retry, Fehlerseiten 403/404/500 in
der App-Shell mit Wording-Vorlagen, neue Version (unsichtbarer Reload
beim Seitenwechsel + Hinweis für Dauer-Tabs), Wartungs-Muster,
Berechtigungs-Norm (nie Erlaubtes existiert nicht, Gesperrtes erklärt
sich; Rechte ändern nie Anordnung oder Form). Abwägung samt verworfener
Alternativen in `entscheidungen.md`; Querverweise in `07`/`12`, Ergänzung
der harten Regel 7 in `AGENTS.md`.

---

## B — Ausbau bestehender Kapitel (fachliche Muster)

### B1 · Feldtypen-Katalog

**✅ Erledigt 2026-08-17** — als eigenes Kapitel `16-feldtypen.md`:
Grundregeln (`autocomplete`, `inputmode`, read-only ≠ disabled, tolerant
eingeben / normiert anzeigen), Text/Kennungen/IBAN, E-Mail/Telefon,
Beträge (kein `type="number"`), Datum/Zeitraum/Uhrzeit,
Auswahl mit Combobox-Schwelle ~15, Passwörter und Einmal-Codes.
Querverweise in `03`/`05`/`11`/`12`; Abwägung in `entscheidungen.md`.

### B2 · Wizard-Anatomie

**✅ Erledigt 2026-08-17** — als Abschnitt „Mehrstufige Abläufe" in
`05-formulare.md`: 2–8 Schritte, Schrittanzeige, Zurück/Weiter,
Validierung je Schritt, Zusammenfassung „Prüfen & Bestätigen" **ab 4
Schritten Pflicht** (kurze Wizards schließen direkt ab), Eingaben-Schutz
beim Abbruch. Abwägung in `entscheidungen.md`.

### B3 · Datei-Upload

**✅ Erledigt 2026-08-17** — in `16-feldtypen.md`: zweistufig
(beiläufige Einzeldatei = Datei-Input aus `05`; wiederkehrend/mehrfach =
Drop-Zone mit Sofort-Upload, Fortschritt/Abbrechen/Fehler je Datei-Zeile,
Vorab-Nennung von Typen und Größen, Tastatur-/Touch-Weg).

### B4 · `04` — Truncation und Umbruch

Ungeregelt, und die Vergleichssysteme sind sich einig (Carbon „Overflow
content", Fiori „Wrapping and Truncation"): **Zahlen werden nie
abgeschnitten**; Spaltenköpfe truncaten mit Ellipsis statt umzubrechen;
Zellwerte: einzeilig mit Ellipsis + voller Wert per Tooltip/Detail —
der volle Text ist immer eine Interaktion entfernt; mehrzeilig nur für
definierte Textspalten (Verwendungszweck), dann mit Zeilen-Limit.

### B5 · `04` — Export

„Export" existiert nur als Button-Beispiel. Festlegen (Fiori als Vorbild,
bewusst kleiner): CSV als Standardformat, Ort in der Aktionsleiste
(Zahnrad? eigener Button nur wo Export Kernarbeit ist), **exportiert wird
die aktuelle Filterung** (nicht heimlich alles), große Mengen asynchron
mit Fortschritt (`07`), Dateiname `entitaet_JJJJ-MM-TT.csv`, Formate wie
angezeigt (deutsches Datum, Beträge mit Komma). Druck-Stylesheets bleiben
Nicht-Ziel (C).

### B6 · `07` — Live-Daten und Aktualisierung

Für Monitoring (anlagenmonitor) nötig: Auto-Refresh-Intervall je Seite
festgelegt (keine Nutzer-Option — „Entscheidungen statt Optionen");
sichtbarer Stand („Stand: 12:03" + relative Angabe); Pause bei
verborgenem Tab; Aktualisierung ohne Layout-Sprung und ohne
Interaktionsverlust (offene Menüs/Auswahl bleiben); manuelle
Aktualisieren-Aktion dort, wo Aktualität entscheidend ist; Wertwechsel
dürfen kurz markieren, nie blinken.

### B7 · `05`/`07` — Ungespeicherte Änderungen und Konflikte

Das Modal ist geschützt (`06`, Backdrop-Regel) — **Seitenformulare nicht**:
Navigations-Guard + Browser-`beforeunload` („Änderungen verwerfen?").
Polaris löst das mit einer kontextuellen Speicherleiste, Fiori mit
Entwürfen — beides für uns zu schwer; der Warn-Dialog genügt als Standard,
**Entwurfs-Speicherung nur als begründete fachliche Ausnahme**.
Dazu **Bearbeitungskonflikt** (zwei Personen, ein Datensatz): beim
Speichern erkennen („inzwischen von X geändert"), Optionen: neu laden /
eigene Eingaben behalten und vergleichen. Kein stilles Überschreiben.

### B8 · `03` — Uhrzeit und Zeitzonen

`03` regelt Datum, aber nicht Zeit: **24-Stunden-Format `HH:MM`**,
Sekunden nur in Logs/Technik; kombiniert `TT.MM.JJJJ, HH:MM`; Zeitzone:
Anzeige in lokaler Zeit (Europe/Berlin), Speicherung UTC/ISO 8601 —
abweichende Zeitzone nur nennen, wenn fachlich relevant.

### B9 · `17-diagramme.md` — Diagramme und Kennzahlen (eigenes Kapitel)

Nichts vorhanden; anlagenmonitor braucht es sicher, Auswertungen in
Billing absehbar. Carbon (eigene Dataviz-Sektion), Fiori (Chart-Katalog +
semantische Muster) und iX (ECharts-Theme, KPI, Gauge) zeigen den Umfang —
wir brauchen die **kleine, strenge Teilmenge**:

- Ein Diagramm beantwortet eine operative Frage (`02`-Dashboard-Regel),
  sonst Tabelle.
- Wenige Typen erlaubt: Linie (Zeitreihe), Balken (Vergleich),
  gestapelt sparsam; keine 3D-, Donut-Deko.
- **Farben:** kategoriale Palette als neue Tokens (`--chart-1 … -6`,
  Marken-Slot, in beiden Themes AA-tauglich); Statusfarben nur für echte
  Schwellen/Zustände; Markenfarbe ist nie Datenreihe „aus Deko".
- **Monochrom benutzbar:** direkte Beschriftung/Legende + unterscheidbare
  Marker, Werte zusätzlich als Tabelle erreichbar (A11y).
- Leerer/ladender Zustand nach `07`; Tooltip-Format nach `03`
  (tabellarische Ziffern).
- **KPI-Kachel** (Wert + Einheit + Status + Ziel-Link) als Baustein
  definieren — die `02`-Dashboards bekommen damit ihre Anatomie.
- Bibliotheksfrage klären: Chart-Bibliotheken rendern eigene Optik →
  Ausnahme zur Kit-Regel nötig, analog Headless-Stufe 3 (`11`): erlaubt,
  wenn vollständig über unsere Tokens gethemt (iX macht das mit ECharts
  vor).

### B10 · `11`/`marke/` — Kleinteile

- **Toast präzisieren:** Position (unten rechts), Dauer (~5 s), höchstens
  einer sichtbar, `role="status"`, nie mit Pflichtinformation (Regel aus
  `07` steht schon).
- **Copy-Button** als Mini-Baustein (Icon + „Kopiert"-Bestätigung) — bei
  IDs, IBAN, Secrets ohnehin gelebte Praxis.
- **Token-Kontrakt:** Abstands-Skala explizit machen (4/8/12/16/20/24 als
  `--space-1…6` oder dokumentierte Festwerte — heute stehen die Werte
  verstreut in Kapiteln); Breakpoint-Tokens (A1); Chart-Tokens (B9).
- **Ebenen-Absatz** in `02` oder Kontrakt: natives `<dialog>`/`popover`
  liegen im Top-Layer, es gibt genau eine z-index-Skala für Rest
  (Topbar < Dropdown < Toast) — nie Ad-hoc-`z-index: 9999`.

---

## C — Klein halten oder ausdrücklich zum Nicht-Ziel erklären

Jeweils ein Satz in einem passenden Kapitel bzw. ein datierter Eintrag in
`entscheidungen.md`, damit die Frage nicht wiederkommt:

| Thema | Vorschlag |
| --- | --- |
| Globale Suche / Command-Palette (`⌘K`) | In `08` bewusst offen gelassen. Entscheiden: vorerst **Nicht-Ziel**; Kriterien nennen, ab wann doch (mehrere Entitätstypen, Springen als Hauptarbeit — Fiori „Enterprise Search" wäre das Vorbild) |
| Tastaturkürzel-Schema | Nicht-Ziel über Browser-Standards + definierte Muster (`Esc`, `Enter`, `↑↓` im Split) hinaus; keine eigenen Chords ohne sichtbare Übersicht |
| Onboarding-Touren / Spotlights / „Neu"-Badges | Nicht-Ziel — Leerzustände tragen die Einführung (`07`); Overlays-über-der-Arbeit widersprechen `01` |
| Kontextmenüs (Rechtsklick) | Nicht-Ziel — Browser-Menü nie überschreiben; Aktionen sind sichtbar (`04`) |
| Drag & Drop (Sortieren, Kanban) | Nicht-Ziel außer Upload-Drop-Zone (B3) |
| Infinite Scroll | explizit verboten — Paginierung ist die Regel (`04`, ein Satz ergänzen) |
| Tabellen-Personalisierung (Spaltenwahl, Resize, Dichte-Umschalter) | Nicht-Ziel — „Entscheidungen statt Optionen" (`01`); Fiori/Carbon bieten es, wir bewusst nicht |
| Gespeicherte Ansichten/Filter (Polaris „saved views") | vertagt — Deep-Links (`02`) decken den Bedarf; erst bei echtem fachlichem Zug |
| KI-Inhalte kennzeichnen | Mini-Regel in `07` (Streaming erwähnt KI bereits): generierte Inhalte sind als solche erkennbar (Carbon „AI label" als Referenz, ohne dessen Optik) |
| Windows-Kontrastmodi (`forced-colors`) | prüfen, nicht regeln — ein Satz in `10`, dass native Elemente + echte Rahmen das meiste tragen |
| Browser-Baseline | ein Absatz in `11`: evergreen, native `<dialog>`/`popover` setzen Baseline ~2024 voraus; kein IE-/Alt-Support |
| Mehrsprachigkeit | Entscheidung dokumentieren: Deutsch als einzige UI-Sprache ist Absicht (`03`), i18n-Umbau wäre eine Regelwerks-Revision |
| E-Mail-/PDF-Ausgaben (Rechnungen, Benachrichtigungs-Mails) | Nicht-Ziel dieses Web-Regelwerks; falls Bedarf → eigener Bereich (iX „operational emails" zeigt, wie das aussähe) |
| `web/README.md` | kleines Inhaltsverzeichnis mit Lesepfad anlegen (Desktop hat eins, Web nicht) |

**Bewusst nicht übernehmen** (von den Vergleichssystemen gesehen, gegen
unsere Haltung): Illustrationen/Maskottchen in Leerzuständen (Fiori/iX),
Dichte-Umschalter cozy/compact (Fiori), Sound-Feedback (Polaris),
Token-Dreischicht-Architektur (Material — Overkill für unsere Größe),
Onboarding-Journeys in S/M/L (Atlassian).

---

## Empfohlene Reihenfolge

1. **A1 Responsive** — größte Lücke, berührt alle anderen Kapitel; danach
   sind A2/B-Muster „responsive mitzudenken" statt nachzurüsten.
2. **A2 Detail-Ansichten** — löst die offene Schuld aus `entscheidungen.md`.
3. **A3 Ausnahmezustände & Berechtigungen** — macht die Rückmeldepflicht rund.
4. **B1–B3** Feldtypen, Wizard, Upload (ein Ausbau-Schub für `05`).
5. **B4–B8** Tabellen-Feinschliff (Truncation, Export), `07`-Ausbau
   (Live, Konflikte, ungespeichert), Uhrzeit.
6. **B9 Diagramme & Kennzahlen** + Token-Ergänzungen (B10).
7. **C-Sammelrunde** — Nicht-Ziele in `entscheidungen.md` datieren,
   Ein-Satz-Ergänzungen, `web/README.md`.

Begleitend bei jedem Schritt: `AGENTS.md` (harte Regeln ggf. ergänzen —
Responsive-Pflicht gehört vermutlich hinein), Haupt-README
(Status „vollständig" → präzisieren, solange der Ausbau läuft),
`entscheidungen.md` (strittige Abwägungen datieren).
