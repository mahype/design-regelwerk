# Icons & Bewegung

## Ein Set, ein Stil

- **Ein einziges Outline-Icon-Set pro Anwendung** — welches, bestimmt das
  Markenprofil (Default: **Lucide**). Nie Sets mischen, auch nicht für ein
  „passenderes" Einzelsymbol.
- Ausschließlich Outlines: `fill="none"`, einfarbig über `currentColor`,
  einheitliche Strichstärke (Lucide-Default 1.8). Keine Filled-, Duotone-,
  3D-, Gradient- oder mehrfarbigen Icons; **keine Emojis oder
  Unicode-Zeichen als Icon-Ersatz** (`✕`, `⚙`, `🔍`).
- Größen: **16px** in Buttons, Tabellen, Hinweisen; **24px** in
  Sidebar-Hauptnavigation und als Seiten-Icon in der Topbar. Andere Größen
  nur, wo ein Kapitel sie festlegt.
- Farbe aus den Tokens des jeweiligen Bausteins. Nur
  `background-image`-SVGs (Select-Pfeil) dürfen den Tokenwert URL-kodiert
  enthalten — je Theme.
- Logos/Markenzeichen sind keine UI-Icons (behalten ihre Markenfarben);
  Diagramme/Datenvisualisierungen ebenfalls nicht.

## Wiedererkennbarkeit

- Für dieselbe Funktion überall dasselbe Symbol: Suche = Lupe, Anlegen =
  Plus, Einstellungen = Zahnrad, Bearbeiten = Stift, Löschen = Papierkorb,
  Ansehen = Auge.
- Fehlt ein fachliches Symbol: allgemeines Symbol + verständliche
  Beschriftung. Ein eigenes SVG nur als letzte Möglichkeit, exakt im
  24×24-Raster und Strichstil des Sets — und anschließend im Markenprofil als
  kanonische Ausnahme dokumentiert.

## Icons ersetzen nie Beschriftungen

- Jede wichtige Aktion bleibt ohne Icon verständlich — das Icon unterstützt
  die Erkennung, der Text trägt die Bedeutung.
- **Icon-only ist nur an den etablierten Orten zulässig** (Topbar,
  Aktionsleiste der Sub-Navigation, Tabellen-Aktionsspalte,
  Paginierungs-Footer) — und dort immer mit `aria-label` **und** sichtbarem
  Tooltip. Ein nacktes Icon ohne Button-Rahmen ist nirgends eine Aktion.
- Icons neben sichtbarem Text sind dekorativ: `aria-hidden="true"`, nicht
  fokussierbar. Klickfläche und Fokus gehören zum Element, nie zum SVG.

## Bewegung

- **Nur funktional:** Bewegung erklärt einen Zustandswechsel (Panel öffnet,
  Sidebar klappt, Zeile erscheint) — nie Dekoration, nie Verzögerung des
  Nutzers. Im Zweifel: keine Animation.
- Dauern aus den Tokens: `--motion-fast` (120 ms) für Hover und kleine
  Wechsel, `--motion-slow` (200 ms) für Panels, Sidebar und Theme-Wechsel;
  Kurve ease-out.
- Icons werden nicht gedreht, beschattet oder dekorativ animiert; die einzige
  zulässige Icon-Animation ist der Lade-Spinner.
- **`prefers-reduced-motion` ist Pflicht:** Übergänge entfallen dann
  (Zustandswechsel geschieht sofort), Spinner werden statisch angedeutet
  oder durch Text ersetzt.
- Layout-Stabilität geht vor Effekt: Nichts schiebt beim Laden oder Hover
  Inhalte umher.

## Herkunft

Set-Exklusivität, Outline-Stil, Größen, Emoji-Verbot, Ausnahme-Prozess,
A11y-Regeln: Enon (`komponenten/icons.md`) — Set als Marken-Slot
verallgemeinert (`entscheidungen.md`, anlagenmonitor nutzt Heroicons
Outline). „Icons ersetzen nie Labels", Funktions-Pflicht für Animation:
TorroConnect. 120 ms ease-out: torro-design (Karten-Hover). Icon-only nur
mit Rahmen + Tooltip: anlagenmonitor (6a).
