# Responsive-Verhalten & Breakpoints

Unsere Anwendungen sind Desktop-Werkzeuge — optimiert wird für große
Fenster. Aber **jede Funktion bleibt auf jedem Gerät benutzbar**: mobil
darf es unbequemer sein, nie unmöglich. Dieses Kapitel definiert die
Größenklassen und was Shell und Bausteine je Klasse tun. Das oberste
Gesetz aus `02-app-shell.md` gilt in jeder Klasse: **Die Seite scrollt
nie horizontal** — nur innere Wrapper scrollen.

## Grundsatz: benutzbar ab 320 px

- Jede Funktion ist ab **320 px Viewport-Breite** erreichbar und
  bedienbar. Es gibt keine „Desktop-only"-Funktionen und keine Hinweise
  „bitte am Desktop fortsetzen" — so ein Text wäre ein Designfehler mit
  Untertitel (`01-haltung.md`).
- Damit ist WCAG-Reflow (320 px entsprechen 400 % Zoom,
  `10-barrierefreiheit.md`) strukturell erfüllt. Die zulässige Ausnahme
  bleibt zweidimensionaler Inhalt: Datentabellen scrollen in ihrem
  Wrapper.
- Schmal wird **mitgebaut, nicht nachgerüstet**: Jede neue Ansicht wird
  vor Abschluss einmal bei 360 px Breite durchlaufen (nichts
  abgeschnitten, kein horizontales Seiten-Scrollen, alles erreichbar).

## Drei Größenklassen, zwei Grenzen

| Klasse | Breite | Typische Geräte |
| --- | --- | --- |
| **schmal** | < 768 px | Telefon, halbes Tablet |
| **mittel** | 768–1279 px | Tablet, halbes Notebook-Fenster |
| **voll** | ≥ 1280 px | Notebook, Desktop (ab ~1400 px greift die Zentrier-Regel aus `02`) |

- Es gibt **genau diese zwei Grenzen** — keine weiteren
  Viewport-Breakpoints erfinden. Braucht ein Baustein feineres Verhalten,
  reagiert er per Container Query auf seinen Behälter (siehe Technik),
  nicht über neue Fensterbreiten.
- Die Werte 768/1280 liegen auf den Sweet Spots der Branche (Tailwind
  `md`/`xl`, Polaris, Bootstrap) und auf unserer Shell-Arithmetik:
  256 px Sidebar + Seitenabstände + ~950 px Tabelle ≈ 1280 px.
- Die Grenzen stehen als Konstanten im Token-Kontrakt
  (`marke/token-kontrakt.md`) — Marken ändern sie nicht.

## Shell je Klasse

### Sidebar

- **voll:** wie `02-app-shell.md` — volle Sidebar, die gespeicherte
  Nutzerwahl (Sidebar ⇄ Icon-Rail) gilt.
- **mittel:** startet **immer als Icon-Rail**. Der Menü-Toggle öffnet die
  volle Sidebar **überlagernd** über dem Inhalt (Breite
  `--sidebar-width`, `--shadow-overlay`, kein Backdrop); Außenklick,
  `Escape` oder Navigieren lässt sie zurückschnappen. Die Nutzerwahl wird
  hier nicht gespeichert — sie gehört zur Klasse „voll".
- **schmal:** die Sidebar ist ein **Drawer**: fährt von links über einen
  Backdrop (`--backdrop` + Blur wie bei Modals), volle Höhe, Breite
  `--sidebar-width` (höchstens 85 vw). Öffner ist der Menü-Toggle der
  Topbar; `Escape`, Backdrop-Klick und Navigieren schließen; der Fokus
  wandert in den Drawer und kehrt zum Toggle zurück (Popover-Regeln,
  `06-dialoge.md`). Eine Icon-Rail gibt es unter schmal nicht.
- Übergänge mit `--motion-slow`, `prefers-reduced-motion` respektieren
  (`09-icons-und-bewegung.md`).

### Topbar

Bleibt in jeder Klasse **eine Zeile, 64 px**. Wird es eng, fällt zuerst
das Kontext-Suffix („– Unterbereich") weg, dann trunkiert der Titel mit
Ellipsis. Die drei Icon-Buttons rechts (Menü, Theme, Benutzer) bleiben in
fester Reihenfolge — sie sind der Anker der Wiedererkennbarkeit.

### Sub-Navigation

Die Leiste bleibt auf jeder Seite (auch schmal). Die **Tabs scrollen
horizontal** in einer Zeile (kein Umbruch, kein „Mehr"-Menü); der aktive
Tab wird beim Laden in Sicht gescrollt. Die **Aktionsleiste bleibt rechts
stehen** und scrollt nicht mit — Suche, Plus und Zahnrad sind
Kernzugriffe.

## Bausteine je Klasse

### Tabellen: Scrollen plus Spalten-Priorität

- Die Grundantwort in jeder Klasse bleibt der **innere Scroll-Wrapper**
  (`04-tabellen-und-listen.md`).
- Zusätzlich darf jede Tabelle Spalten als **sekundär** markieren:
  Sekundäre Spalten entfallen unter **schmal**. Was entfällt, bleibt über
  Ansehen/Detail erreichbar — der Wegfall ist Verdichtung, nie
  Datenverlust. Eine aktive Sortierung nach einer entfallenen Spalte
  bleibt wirksam (die Reihenfolge ändert sich beim Verkleinern nicht).
- **Nie entfallen:** die Identitätsspalte (das, was den Datensatz
  benennt) und die Aktionsspalte.
- Der Paginierungs-Footer darf unter schmal **zweizeilig** umbrechen
  (Navigation oben; Standortanzeige und Ansichts-Einstellungen darunter);
  die Buttons behalten ihre Größe.

### Formulare

- Das Zweispalten-Grid (`05-formulare.md`) bricht **per Container Query**
  auf einspaltig, sobald sein Behälter schmaler als ~36 rem (576 px)
  ist — dieselbe Regel gilt damit im Modal, auf der Seite und in der
  Split-Spalte automatisch richtig.
- Auf Touch-Geräten (siehe unten): Eingabefelder wachsen auf ≥ 44 px
  Höhe, die **Eingabeschrift auf 16 px** — sonst zoomt iOS beim Fokus in
  die Seite.

### Modals

- **schmal:** Formular- und Anzeige-Modals werden **Vollbild**
  (`100dvw × 100dvh`, Radius 0); Kopf bleibt sticky (`06-dialoge.md`),
  der Fuß wird es auch — Abschluss-Buttons sind ohne Scrollen erreichbar.
  `dvh` statt `vh`, damit mobile Browserleisten nicht verdecken.
- **Bestätigungs-Modals bleiben in jeder Klasse klein und zentriert**
  (`min(480px, calc(100vw - 2rem))`) — eine Rückfrage soll klein wirken.

### Arbeitsflächen, Raster, Popover

- **Zweispaltige Arbeitsflächen** (`02`, feste ~360er-Spalte): unter
  mittel stapelt die feste Spalte unter die Hauptfläche.
- **Master-Detail-Split:** unter schmal nacheinander statt nebeneinander
  (Liste → Detail mit Zurück); Details im Kapitel Detail-Ansichten.
- **Karten-Raster** definieren Mindest-Kartenbreiten
  (`grid: minmax(…)`) und fließen selbst — nie feste Spaltenzahlen je
  Viewport.
- **Such-Popover** (`08`): unter schmal volle Inhaltsbreite (16 px
  Seitenrand), begrenzte Höhe, scrollt intern. Die **Filterleiste**
  bricht ihre Felder wie das Formular-Grid.
- **Anmeldeseite** (`12`): die 384er-Karte funktioniert unverändert bis
  320 px (Seiten-Padding bleibt).

## Touch

Touch wird über **`pointer: coarse` / `hover: none`** erkannt — nie über
die Viewport-Breite (es gibt Touch-Notebooks und Tablets mit Maus).

- **Ziele ≥ 44 px:** Icon-Buttons (36 px) wachsen per Padding auf
  mindestens 44 px Trefferfläche; Nav-Zeilen (48 px) erfüllen es schon.
- **Hover ist nie Voraussetzung:** Alles ist per Tap erreichbar;
  Hover-Effekte bekommen ein `:active`-Äquivalent als Rückmeldung.
- **Tooltips entfallen bei Touch.** Icon-only bleibt trotzdem nur an den
  etablierten Orten zulässig (`09`) — `aria-label` bleibt Pflicht, die
  feste Position trägt die Erkennung. Neue oder erklärungsbedürftige
  Aktionen zeigen unter schmal **sichtbaren Text statt icon-only**.

## Technik

- **Shell-Verhalten** hängt an den zwei Viewport-Grenzen (Media Queries).
  In `@media`-Bedingungen funktioniert kein `var()` — die Werte stehen
  als Literale im Code, der Kontrakt ist ihre einzige Quelle.
- **Tailwind v4:** Skala auf genau unsere zwei Grenzen beschränken
  (`--breakpoint-*: initial;` dann `--breakpoint-md: 768px;
  --breakpoint-xl: 1280px;`). Basis-Styles = schmal, `md:` = mittel,
  `xl:` = voll — andere Präfixe existieren dann gar nicht.
- **Bausteine** (Formular-Grid, Karten-Raster, Filterleiste) reagieren
  per **Container Query** (`container-type: inline-size` am Behälter) —
  ein Formular im breiten Fenster, aber schmalen Modal bricht damit
  richtig um.
- `--space-page` reduziert sich unter schmal von 24 auf 16 px (der
  Kontrakt setzt den Tokenwert per Media Query um).
- Höhenbezüge auf Mobilgeräten mit `dvh`, nie `vh`.

## Prüfung

- Jede neue Ansicht: ein Durchlauf bei **360 px** (Standard-Telefonmaß)
  und bei **200 % Zoom** (`10`) — nichts abgeschnitten, kein
  horizontales Seiten-Scrollen, alle Aktionen erreichbar.
- Der automatisierte A11y-Check (`10`) läuft zusätzlich in einem
  schmalen Viewport (z. B. 360×740), nicht nur in Desktop-Größe.

## Herkunft

Neues Kapitel — keines der vier Quell-Regelwerke enthielt
Responsive-Regeln (Abwägung: `entscheidungen.md`). Vorbilder aus dem
Systemvergleich: Größenklassen-Denken und Spalten-Wichtigkeit („Pop-in")
von SAP Fiori, Navigations-Wechsel je Klasse (Leiste → Rail → Drawer)
von Material 3, Breakpoint-Werte auf den Sweet Spots von
Tailwind/Polaris/Bootstrap. Die 320-px-Untergrenze folgt aus
WCAG-Reflow; `dvh`, 16-px-Eingabeschrift gegen iOS-Zoom und
`pointer: coarse` sind gängige Mobile-Web-Praxis.
