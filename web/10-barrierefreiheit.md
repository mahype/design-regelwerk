# Barrierefreiheit

Verbindlich für **alle** Oberflächen — unabhängig davon, ob ein Produkt
rechtlich erfasst ist. Barrierefreiheit ist Grundlage jeder Seite, kein
Projekt-Feature, und wird nicht nachgerüstet, sondern mitgebaut.

## Zielniveau

- **Mindestens WCAG 2.1 AA** — der gesetzliche Maßstab (EAA seit 28.06.2025,
  EN 301 549 v3.2.1 verweist auf 2.1 AA).
- **Gebaut wird direkt gegen WCAG 2.2 AA** (Obermenge) — wer heute 2.2
  erfüllt, rüstet bei der Norm-Fortschreibung nicht nach.
- AAA punktuell, wo einfach machbar — kein generelles Ziel.

*(Stand 2026-08; bei Norm-Updates diesen Abschnitt aktualisieren.)*

## Was das konkret heißt

- **Kontrast:** Text ≥ 4.5:1 (normal), ≥ 3:1 (groß: ab 24px bzw. 18.66px
  bold); UI-Komponenten und Fokus-Indikatoren ≥ 3:1 — **in Hell UND
  Dunkel.** Neue Farbkombinationen vor Nutzung prüfen; `--accent-text` muss
  auf beiden Flächen beider Themes bestehen.
- **Tastatur:** Alles bedienbar, logische Tab-Reihenfolge, **sichtbarer
  Fokus-Ring** (`--focus-ring`) — nie `outline: none` ohne Ersatz. Modals
  fangen den Fokus, `Esc` schließt, der Fokus kehrt zum Auslöser zurück.
  Skip-Link zum Inhalt am Seitenanfang.
- **Zeigerhand:** Jedes aktive klickbare Element zeigt `cursor: pointer` —
  Buttons, Links, Zeilenziele, alles Interaktive. Deaktivierte Elemente sind
  ausgenommen (nicht klickbar).
- **Zielgrößen:** interaktive Ziele ≥ 24×24px (2.2 AA), besser ≥ 44px auf
  Touch-lastigen Flächen. Die 36px-Icon-Buttons und 48px-Nav-Zeilen erfüllen
  das.
- **Benennung:** Icon-only-Buttons haben `aria-label` + Tooltip; jedes Input
  ein echtes `<label>`; Bilder ein `alt`; `<html lang="de">`.
- **Struktur:** semantisches HTML (`<button>`, `<nav>`, `<table>`,
  `<th scope="col">`, Überschriften-Hierarchie, Landmarks). ARIA nur wo
  nötig — nie, um schlechtes HTML zu kaschieren.
- **Status & Feedback:** Fehler in Text, nie nur Farbe; Lade-, Fehler- und
  Trefferzahl-Änderungen per `aria-live` angekündigt (Fehler `role="alert"`);
  Statuspunkte und Badges mit zugänglichem Namen.
- **Bewegung:** `prefers-reduced-motion` respektieren
  (`09-icons-und-bewegung.md`).
- **Zoom:** 200 % Text-Zoom ohne Funktions- oder Inhaltsverlust; die Seite
  scrollt dabei nie horizontal.

## Automatischer Check (Pflicht)

Jede Oberfläche wird automatisiert gegen WCAG 2.x AA geprüft:

- Engine **axe-core** (geringste False-Positive-Rate, De-facto-Standard),
  Runner z. B. **pa11y** mit axe — gegen die **laufende** Anwendung.
- In CI mindestens: **Anmeldeseite + eine Listen- + eine Detailseite.**
  Rote Befunde brechen den Build.
- **Automatik ≠ vollständig:** axe findet grob die Hälfte der Probleme.
  Tastatur-Durchlauf und Screenreader-Stichprobe bleiben Pflicht vor jedem
  größeren Release.

Ein fertig eingerichtetes Prüf-Setup liegt in `enon-design/tools/a11y-check/`
und ist markenneutral wiederverwendbar.

## Herkunft

Vollständig aus Enon (`foundations/barrierefreiheit.md`, inkl. Normstand,
Prüf-Setup, Zeigerhand-Regel) — dort das strengste der vier Regelwerke;
anlagenmonitor deckungsgleich (A11y als „höchste Priorität", `aria-live`
für Paginierung). TorroConnect bestätigt die Prinzipien (Fokus, Labels,
verständliche Fehler). Nichts abgeschwächt übernommen.
