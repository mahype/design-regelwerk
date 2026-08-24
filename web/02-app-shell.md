# App-Shell

Jede Anwendung nutzt dasselbe Grundgerüst: Sidebar links, Topbar oben,
darunter die Sub-Navigation, Inhalt auf dem Seitengrund.

```
┌──────────┬──────────────────────────────────────────────┐
│ ▉ Marke  │ ⌂ Titel – Unterbereich        [☰] [◐] [⚙]    │ ← Topbar, 64px
│──────────├──────────────────────────────────────────────┤
│ ▣ Bereich│ Tab · Tab · Tab              [🔍] [＋] [⚙]   │ ← Sub-Navigation
│ ◦ Seite  ├──────────────────────────────────────────────┤
│ › Bereich│                                              │
│          │  Inhalt (--surface-page, 24px Padding)       │
│  (Marke) │                                              │
└──────────┴──────────────────────────────────────────────┘
  ← 256px →
```

Alle Flächen, Maße und Farben aus dem Token-Kontrakt; die hier genannten
Pixelwerte sind die Defaults (`--sidebar-width`, `--topbar-height`,
`--space-page`). Wie sich das Gerüst auf schmalen Screens verhält
(Drawer, Rail-Automatik, Vollbild-Modals): `13-responsive.md`.

## Sidebar — die einzige Seiten-Navigation

- Fix links, volle Höhe, `--surface-raised`, rechts 1px `--border`.
  Breite `--sidebar-width` (Default 256px).
- **Kopf (64px):** Logo-Mark + Produktname; unten 1px `--border`.
- **Einträge:** ganze Zeile klickbar (Default 48px hoch, Radius
  `--radius-control`), Icon 24px + Label 14px medium. Inaktiv sekundär,
  Hover `--surface-hover`. **Aktiv: Fläche `--accent-soft`, Text und Icon in
  Akzent** — die aktive Navigation ist einer der festen Akzent-Orte.
  `aria-current="page"` am aktiven Link.
- **Höchstens zwei Ebenen:** direkte Ziele als Links; Fachbereiche mit
  Unterseiten als Disclosure (Chevron rechts, `aria-expanded`), Untereinträge
  ohne eigenes Icon, eingerückt unter dem Label. Der Bereich der aktuellen
  Seite ist beim Laden geöffnet; der Auf/Zu-Zustand wird gemerkt. Keine
  überlagernden Untermenüs, keine dritte Ebene — tiefere Struktur gehört in
  die Sub-Navigation der Seite.
- **Gruppen-Reihenfolge, in jeder Anwendung gleich:** 1. Übersicht (falls
  vorhanden) · 2. fachliche Arbeitsbereiche · 3. Einstellungen/Verwaltung ·
  4. Dokumentation. Nicht Vorhandenes entfällt ersatzlos; kein Eintrag
  doppelt.
- **Eingeklappt = Icon-Rail (64px), nie weg.** Der Menü-Toggle verkleinert
  die Sidebar auf Icons mit sichtbaren Tooltips (Hover **und** Fokus) +
  `aria-label`; Fachbereiche öffnen ein Popover mit ihren Untereinträgen
  (Tastatur-bedienbar, `Escape` schließt, Fokus kehrt zurück). Zustand wird
  über Reload gemerkt; Übergang animiert (`--motion-slow`,
  `prefers-reduced-motion` respektieren).
- **Stille Markenpräsenz** (Wortmarke am Fuß o. ä.): nach Markenprofil.

## Topbar — Kontext und globale Steuerung

- Sticky oben, `--topbar-height` (64px), `--surface-raised` mit leichter
  Transparenz + `backdrop-blur`, unten 1px `--border`, `--shadow-raised`.
- **Links der Seitenkontext, immer genau eine Zeile:** Seiten-Icon (24px,
  sekundär) + Titel (16px semibold) + Kontext-Suffix „– Unterbereich" (16px
  regular, sekundär — der aktive Tab der Sub-Navigation). **Keine großen
  Seitentitel im Inhalt, kein Subheading als zweite Zeile.**
- **Rechts die globalen Icon-Buttons, feste Reihenfolge in jeder Anwendung:**
  1. **Menü-Toggle** (Sidebar ⇄ Icon-Rail)
  2. **Theme-Umschalter** (hell / dunkel / System)
  3. **Benutzermenü**
- Die Topbar ist **kein** Seitenmenü und trägt **keine** Seiten-Aktionen —
  die gehören in die Sub-Navigation.

### Benutzermenü

Dropdown unter dem Benutzer-Button, rechtsbündig: `--surface-raised`, 1px
`--border`, Radius `--radius-surface`, `--shadow-overlay`, min. 280px.
Kopf: Initialen-Avatar (36×36, Radius `--radius-control`) + Name + E-Mail,
Trennlinie; dann Einträge (Icon 16px + Label 14px); **Abmelden immer als
letzter Eintrag**, durch Trennlinie abgesetzt. Abmelden ist nie ein eigener
Topbar-Button.

## Sub-Navigation — auf jeder Seite

Eine Leiste direkt unter der Topbar, auf **wirklich jeder Seite** — auch mit
nur einem (dann aktiven) Tab. Der Nutzer findet Tabs und Aktionen überall an
derselben Stelle; das ist Kern der Wiedererkennbarkeit.

- Leiste min. 56px, Padding 0 `--space-page`, unten 1px `--border`;
  links Tabs, rechts die Aktionsleiste.
- **Tabs** (14px medium): inaktiv sekundär mit Hover; **aktiv in Akzent**
  mit 2px-Unterstreichung bündig auf der Leisten-Unterkante. Der aktive Tab
  erscheint zusätzlich als Suffix im Topbar-Titel. Tab-Zustand ist
  deep-linkbar (URL).
- **Aktionsleiste des aktiven Bereichs** — Icon-Buttons, feste Reihenfolge
  (nicht Vorhandenes entfällt ersatzlos):
  1. **Suche/Filter** (Lupe → Popover, `08-suche-und-filter.md`)
  2. **Aktualisieren** (nur Monitoring-Ansichten, `07-rueckmeldung.md`)
  3. **Export** (nur wo Export zur Arbeit gehört,
     `04-tabellen-und-listen.md`)
  4. **Plus** (öffnet das Anlegen-Modal des Bereichs)
  5. **Zahnrad** (optional: selten geänderte Bereichs-Grundeinstellungen)
- Wächst der Einstellungsbereich hinter dem Zahnrad, gliedert er sich in
  Bereiche mit **vertikaler** Bereichs-Navigation links (bewusst anders
  orientiert als die horizontalen Tabs; aktiver Eintrag mit Akzent-Balken,
  `aria-current`, Zustand in der URL).

## Inhalt

- `--surface-page`, Padding `--space-page` (24px), Seitenblöcke mit 24px
  vertikalem Abstand, innerhalb eines Blocks 16px. Keine individuellen
  Margins an Einzelelementen — der Rhythmus kommt vom Raster.
- Der Inhaltsbereich braucht `min-width: 0` (Flex), damit breite Tabellen in
  ihren Scroll-Wrappern bleiben, statt die Seite zu sprengen. Die Seite
  scrollt **nie** horizontal.
- **Textspalten** (Erklärseiten, Doku): max. ~720px Zeilenbreite.
  **Datenflächen** (Tabellen, Karten-Raster) dürfen die volle Breite nutzen;
  ab ~1200–1400px Inhaltsbreite zentrieren mit Maximalbreite, statt endlos zu
  wachsen.
- **Zweispaltige Arbeitsflächen** (Werkzeugseiten, Master-Detail-Split):
  Hauptfläche flexibel + feste Spalte (~360px), Abstand 24px; Höhe begrenzen,
  Inhalte scrollen intern. Anatomie des Splits: `14-detail-ansichten.md`.

## Navigation beantwortet drei Fragen

**Wo bin ich?** (Sidebar-Aktivzustand + Topbar-Kontext) · **Wohin kann ich?**
(Sidebar + Tabs) · **Wie komme ich zurück?** (Browser-Zurück funktioniert
immer). Deshalb: Ansichtszustand — aktiver Tab, Filter, Seite, geöffneter
Bereich — lebt in der URL und ist teil- und wiederherstellbar. Ungültige
URL-Werte fallen serverseitig auf den ersten gültigen Wert zurück.

## Dashboards

Erlaubt, wenn sie **operative Fragen beantworten** und in die Arbeit
verlinken (Kacheln mit Zahl, Status und Ziel — „4 offene Posten →"); verboten
als Deko oder Pflicht-Startseite ohne Inhalt. Eine Anwendung, deren Kern eine
einzige Arbeitsliste ist, startet auf dieser Liste. Anatomie der
KPI-Kacheln und Diagramm-Regeln: `17-diagramme.md`.

## Ebenen (Stapelreihenfolge)

Natives `<dialog>` (`showModal()`) und das `popover`-Attribut liegen im
**Browser-Top-Layer** — sie brauchen keinen `z-index` und liegen immer
über allem. Für den Rest gilt eine kleine feste Skala, mehr gibt es
nicht: **10** sticky Leisten (Topbar, Sub-Navigation, sticky
Modal-Kopf/-Fuß) · **20** schwebende Hilfen ohne Popover-Attribut
(Tooltips). Ad-hoc-Werte (`z-index: 9999`) sind verboten — wer höher
muss, nutzt das Top-Layer.

## Herkunft

Gerüst, Maße, Sidebar-Verhalten, Icon-Rail, Benutzermenü, Sub-Navigation:
Enon (`layout/app-shell.md`), Topbar-Anatomie und Aktionsleisten-Disziplin
deckungsgleich mit anlagenmonitor (Header/Subtab-Bar; dort auch der
Drei-Wege-Theme-Umschalter und die URL-Bindung des Bereichszustands).
Die drei Navigationsfragen: TorroConnect. Dashboard-Regel: Abwägung zwischen
Enons Verbot und gelebter Praxis (`entscheidungen.md`, Markenmoment).
