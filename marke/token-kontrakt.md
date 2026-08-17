# Token-Kontrakt

Der Kontrakt zwischen Regelwerk und Marke: Das Regelwerk formuliert alle
Regeln gegen diese Token-Namen; die Marke liefert die Werte (hell + dunkel).
Komponenten verwenden **ausschließlich** Tokens — nie Hex-Werte, nie
Ad-hoc-Radien oder -Schatten. Fehlt ein Token, wird es im Markenprofil
ergänzt, nicht im Code erfunden.

Token-Namen sind englisch (CSS-Konvention), Dokumentation deutsch.
Markeneigene Zusatz-Tokens sind erlaubt und tragen das Präfix `--brand-*`.

## Pflicht-Tokens

```css
:root {
  color-scheme: light dark;

  /* Schrift */
  --font-ui: /* UI-Schrift mit System-Fallbacks */;
  --font-mono: ui-monospace, SFMono-Regular, monospace;

  /* Flächen */
  --surface-page:   /* Seitenhintergrund hinter allem */;
  --surface-raised: /* Karten, Tabellen, Modals, Sidebar, Topbar */;
  --surface-inset:  /* vertiefte Flächen: Tabellenkopf, Code-Blöcke */;
  --surface-hover:  /* Hover von Zeilen, Nav-Einträgen, Icon-Buttons */;

  /* Linien */
  --border:       /* Standard-Rahmen: Karten, Tabellen, Modals */;
  --border-input: /* Rahmen von Eingabefeldern (etwas kräftiger) */;
  --hairline:     /* feine Trennlinien in Tabellen und Listen */;

  /* Text */
  --text:           /* Primärtext */;
  --text-secondary: /* Sekundärtext, Hints, Tabellenköpfe, Labels */;
  --text-on-accent: /* Text auf Akzentfläche (meist Weiß) */;

  /* Akzent — die eine Markenfarbe */
  --accent:       /* Fläche: Primär-Button, aktive Markierung */;
  --accent-hover: /* Hover des Primär-Buttons */;
  --accent-soft:  /* helle Akzentfläche: aktive Navigation, Tints */;
  --accent-text:  /* Akzent als Text-/Linkfarbe, AA-Kontrast auf
                     --surface-page UND --surface-raised im jeweiligen Theme */;
  --focus-ring:   /* sichtbarer Fokus, meist = Akzentton */;

  /* Status — Semantik fix, Töne von der Marke.
     Je Familie: Vollton (Text/Icon), -surface (Fläche), -border (Rahmen). */
  --status-ok:            ; --status-ok-surface:      ; --status-ok-border:      ;
  --status-warn:          ; --status-warn-surface:    ; --status-warn-border:    ;
  --status-error:         ; --status-error-surface:   ; --status-error-border:   ;
  --status-info:          ; --status-info-surface:    ; --status-info-border:    ;
  --status-neutral:       ; --status-neutral-surface: ; --status-neutral-border: ;

  /* Geometrie & Material */
  --radius-control: /* Buttons, Inputs, Badges — z. B. 6px */;
  --radius-surface: /* Karten, Tabellen, Modals — z. B. 8–12px */;
  --shadow-raised:  /* Karten/Tabellen/Topbar — flach */;
  --shadow-overlay: /* Modals, Popovers, Dropdowns — deutlich */;
  --backdrop:       /* Abdunklung hinter Modals, z. B. rgb(… / 0.35);
                       immer kombiniert mit backdrop-filter: blur(4px) */;

  /* Maße (Regelwerk-Defaults; Marke ändert nur mit Grund) */
  --space-page: 24px;    /* Seiten-Padding, Abstand zwischen Seitenblöcken */
  --space-block: 16px;   /* Abstand innerhalb eines Blocks */
  --sidebar-width: 256px;
  --topbar-height: 64px;

  /* Bewegung */
  --motion-fast: 120ms;  /* Hover, kleine Zustandswechsel */
  --motion-slow: 200ms;  /* Panels, Sidebar, Theme-Wechsel */
}
```

## Hell/Dunkel: das Drei-Zustands-Muster (verbindlich)

Es gibt drei Zustände, nicht zwei: **System** (Default — kein Attribut
gesetzt), **explizit hell**, **explizit dunkel**. Umschaltung per
`data-theme="light" | "dark"` auf `<html>`; Abwesenheit des Attributs heißt
„System entscheidet". Die Wahl wird in `localStorage` gespeichert und vor dem
ersten Paint angewendet (Inline-Script im `<head>`, sonst blitzt das falsche
Theme).

```css
/* 1) Heller Basissatz: IMMER auf :root — nie nur hinter einem Selektor */
:root { /* … alle Pflicht-Tokens, helle Werte … */ }

/* 2) System dunkel — greift nur, solange keine explizite Wahl hell erzwingt */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) { /* … dunkle Werte … */ }
}

/* 3) Explizit dunkel — gewinnt auch bei hellem System */
:root[data-theme="dark"] { /* … dieselben dunklen Werte … */ }
```

Regeln dazu:

- Eine Farbe, deren **einzige** Definition hinter `@media` oder
  `[data-theme]` liegt, ist ein Bug — der ungestempelte Default-Zustand sieht
  sie nie. Jedes Token hat seinen hellen Basiswert auf `:root`.
- Der dunkle Wertesatz steht **zweimal identisch** (Media-Query + Attribut);
  bei Änderungen beide Stellen pflegen — oder per Präprozessor/Mixin aus
  einer Quelle erzeugen.
- Komponenten kennen nur Tokens. Kein `dark:`-Sonderweg an einzelnen
  Elementen für Farben, die der Kontrakt abdeckt.
- Bestehende Projekte mit `.dark`-Klasse (Enon) erfüllen den Zweck; neue
  Projekte verwenden `data-theme`. Der Umschalter bietet Hell / Dunkel /
  System (System = Attribut entfernen).

## Verwendung mit Tailwind oder Vanilla-CSS

Der Kontrakt ist stack-neutral. Vanilla-CSS bindet die Datei direkt ein;
Tailwind v4 mappt die Tokens im `@theme`-Block auf Utilities
(`--color-accent: var(--accent);` usw.). In beiden Fällen bleibt die
Marken-`tokens.css` die einzige Wertequelle — Utilities sind nur ein anderer
Schreibweg zum selben Token.

## Herkunft

Struktur und Flächen-/Linien-Rollen aus `enon-design/tokens/tokens.css`
(verallgemeinert und markenneutral benannt); `--accent-text` und das
Drei-Zustands-Muster aus den Torro-Billing-Erfahrungen (rote Schrift auf
dunklem Grund, System-Default ohne Stempel); Status-Fünffaltigkeit aus Enon,
Marke-nie-Status aus torro-design.
