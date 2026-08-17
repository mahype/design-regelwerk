# Markenprofil — was eine Marke festlegen muss

Das Regelwerk ist markenneutral: Es sagt, **wo** Farbe, Schrift und Marke
wirken — die **Werte** liefert die Marke. Jede Marke pflegt ihr Profil in
ihrem eigenen Repo (z. B. `torro-design`, `enon-design`) und liefert zwei
Artefakte:

1. **`tokens.css`** nach [`token-kontrakt.md`](token-kontrakt.md) — die Werte
   unter den kanonischen Namen, hell und dunkel.
2. **Ein ausgefülltes Markenprofil** (diese Datei als Vorlage) — die
   Entscheidungen, die sich nicht als CSS ausdrücken lassen.

Ohne ausgefülltes Profil ist ein Projekt nicht startklar; ein Agent, der
einen fehlenden Slot vorfindet, fragt nach statt zu raten.

## 1. Identität

- **Name & Logo/Wortmarke:** Dateien, erlaubte Varianten (hell/dunkel),
  Mindestgrößen, Schutzraum.
- **Markenmoment:** Darf die Marke einmal laut sein — und wo? Zulässige Orte:
  Anmeldeseite, Erstlauf-/Leerzustand, definierter Kopfbereich. Höchstens
  **ein** lauter Ort pro Sichtbereich; keiner ist genauso zulässig
  (siehe `entscheidungen.md`). Der Arbeitsbereich bleibt in jedem Fall ruhig.
- **Stille Präsenz:** Wo die Marke leise auftritt (z. B. Wortmarke am
  Sidebar-Fuß, Logo-Mark im Sidebar-Kopf).

## 2. Farbe

Das Regelwerk kennt vier Farb-Rollen; die Marke füllt sie:

- **Akzent** (`--accent`-Familie): die eine Markenfarbe für Primäraktion,
  aktive Markierung und Fokus. Einsatzorte sind fix (genau ein Primär-Button
  pro Sichtbereich, aktive Navigation, Fokus-Ring, Links) — die Marke wählt
  nur den Ton. Pflicht: eine Textvariante (`--accent-text`) mit
  AA-Kontrast auf hellen **und** dunklen Flächen; ein kräftiges Rot etwa
  braucht im Dunkeln eine aufgehellte Textvariante.
- **Neutrale Flächen** (`--surface-*`, `--border-*`, `--text-*`): die
  Grau-Familie der Marke, hell und dunkel. Ein leichter Farbstich ist
  erwünscht (gewählt wirkt besser als default), Kontraste nach
  `web/10-barrierefreiheit.md`.
- **Statusfarben** (`--status-*`): ok / Warnung / Fehler / Info / neutral.
  Semantik ist fix und überall gleich. **Marke und Status sind strikt
  getrennt:** Ähnelt die Markenfarbe einer Statusfarbe (Rot, Orange, Grün),
  gilt besonders streng — die Markenfarbe ist nie Statusfarbe, der Status
  nimmt den eigenen Ton (Torro-Regel „Rot ist Marke, nie Status",
  verallgemeinert).
- **Deko ist keine Rolle.** Farbe trägt Bedeutung; monochrom muss die
  Oberfläche benutzbar bleiben.

## 3. Typografie

- **UI-Schrift** (`--font-ui`): eine Familie, wenige Gewichte (400/500/600).
  Muss tabellarische Ziffern können oder mit `font-variant-numeric:
  tabular-nums` sauber rendern. Systemschrift ist ein legitimer Wert.
- **Display-/Markenschrift:** nur wenn die Marke eine hat — und dann nur an
  den Markenmoment-Orten, nie als UI-Schrift (Torro: Frutiger nur in der
  Wortmarke).
- **Skala-Abweichungen:** Das Regelwerk gibt Rollen und Default-Werte vor
  (`web/03-typografie-und-sprache.md`); die Marke darf Werte innerhalb der
  dokumentierten Spannen verschieben und entscheidet **einmalig**, ob Labels
  und Tabellenköpfe versal gesetzt werden (Enon: ja).

## 4. Material

- **Radien** (`--radius-control`, `--radius-surface`): z. B. Enon 6/8,
  Torro 12 für Karten. Zwei Stufen genügen.
- **Schatten & Kanten** (`--shadow-*`): die Material-Sprache der Marke —
  von Enons flachem `shadow-sm` bis zur Torro-Karte mit Doppelschatten und
  Verlaufs-Randlinie. Höchstens zwei Schatten-Stufen (Fläche + Overlay).
- **Backdrop** (`--backdrop`): Abdunklung hinter Modals, mit Blur.

## 5. Ton

- **Anrede:** Du oder Sie — einmal entscheiden, überall gleich.
- **Wortliste:** die Fachbegriffe des Produkts (was heißt „Konto", „Beleg",
  „Vorgang" …), damit UI, Fehlermeldungen und Doku dieselben Wörter benutzen.
- **Stimme:** in ganzen Sätzen, Nutzerperspektive, ohne Prozess-Jargon —
  Grundregeln in `web/03-typografie-und-sprache.md`, hier nur
  markenspezifische Schärfungen.

## 6. Bewegung

- Dauern und Kurven (`--motion-fast`, `--motion-slow`): Defaults 120 ms /
  200 ms, ease-out. Die Marke darf justieren, nicht dramatisieren — Bewegung
  bleibt funktional (`web/09-icons-und-bewegung.md`).

## 7. Theme-Default

- Default ist **System**. Eine Marke darf einen festen Start-Default setzen
  (Enon: dunkel), nie die drei Zustände reduzieren oder den Umschalter
  weglassen.

## Checkliste

Ein Markenprofil ist vollständig, wenn:

- [ ] `tokens.css` alle Pflicht-Tokens des Kontrakts definiert, hell + dunkel
- [ ] Kontraste in beiden Themes geprüft sind (AA)
- [ ] Logo-/Wortmarken-Dateien mit Einsatzregeln vorliegen
- [ ] Markenmoment-Orte benannt sind (oder ausdrücklich „keine")
- [ ] Anrede + Wortliste festgelegt sind
- [ ] Icon-Set benannt ist (Default: Lucide Outline)
