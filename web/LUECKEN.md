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

### B4 · Truncation und Umbruch

**✅ Erledigt 2026-08-17** — Abschnitt „Überlange Inhalte" in `04`:
Zahlen nie kürzen, Köpfe einzeilig mit Ellipsis, Zellwerte einzeilig mit
Voll-Wert eine Interaktion entfernt, `line-clamp` nur für ausgewiesene
Textspalten, Mitte-Kürzung für Kennungen, Vorrang der Identitätsspalte.

### B5 · Export

**✅ Erledigt 2026-08-17** — Abschnitt „Export" in `04`: eigener
Icon-Button in der Aktionsleiste (nur wo Export zur Arbeit gehört),
Dropdown mit **immer CSV und XLSX**, exportiert wird die aktuelle
Filterung/Sortierung, CSV mit Semikolon + UTF-8-BOM, XLSX mit echten
Typen, `entitaet_JJJJ-MM-TT`-Dateinamen, große Mengen asynchron.
Aktionsleisten-Reihenfolge in `02` erweitert.

### B6 · Live-Daten und Aktualisierung

**✅ Erledigt 2026-08-17** — Abschnitt „Live-Daten & Aktualisierung" in
`07`: Auto-Refresh nur für ausgewiesene Monitoring-Ansichten (festes
Intervall, Stand-Anzeige, Tab-Pause, störungsfrei), manuelle
Aktualisieren-Aktion in der Aktionsleiste.

### B7 · Ungespeicherte Änderungen und Konflikte

**✅ Erledigt 2026-08-17** — „Ungespeicherte Änderungen auf Seiten" in
`05` (Guard + `beforeunload`, Autosave nur als fachliche Ausnahme) und
„Bearbeitungskonflikte" in `07` (Versionsstempel Pflicht, kein
last-write-wins, zwei benannte Wege).

### B8 · Uhrzeit und Zeitzonen

**✅ Erledigt 2026-08-17** — in `03`: `HH:MM` (24 h), Sekunden nur in
Logs, kombiniert `TT.MM.JJJJ, HH:MM`, Anzeige lokal (Europe/Berlin),
Speicherung UTC/ISO 8601.

### B9 · Diagramme und Kennzahlen

**✅ Erledigt 2026-08-17** — Kapitel `17-diagramme.md` angelegt:
operative-Frage-Pflicht, Typen-Katalog (Linie/Fläche, Balken, Donut ≤5,
Gauge), Chart-Tokens `--chart-1…6` als Marken-Slot, Statusfarben nur für
Schwellen, Tabellen-Alternative als Pflicht, KPI-Kachel mit neuer
Kennzahl-Rolle (24 px in `03`), Apache ECharts als Default-Bibliothek
(Token-Theme, Ausnahme zur Kit-Regel in `AGENTS.md` dokumentiert).
Abwägung in `entscheidungen.md`.

### B10 · Kleinteile

**✅ Erledigt 2026-08-17** — Toasts (unten rechts, ~5 s, höchstens
einer, `role="status"`) und Copy-Button als Bausteine in `11`;
Abstands-Skala als dokumentierte Festwerte und Chart-Tokens im
Token-Kontrakt; Ebenen-Skala in `02` (Top-Layer nativ, 10 sticky,
20 Tooltips, Ad-hoc-z-index verboten).

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
