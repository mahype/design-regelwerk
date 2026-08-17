# Suchen & Filtern auf Listenseiten

Kontextbezogene Suche/Filterung der aktuell angezeigten Liste — keine globale
Anwendungssuche (die wäre ein eigenes Muster im Kopfbereich).

## Standard: das Such-Popover in der Aktionsleiste

Die Suche sitzt in jeder Anwendung an derselben Stelle: als **Lupe-Icon-Button
erster Eintrag der Aktionsleiste** (Sub-Navigation, `02-app-shell.md`) — kein
dauerhaft sichtbares Suchfeld über der Liste. Hat eine Liste eine eigene
lokale Tab-Leiste, sitzt die Lupe an deren rechtem Ende (außerhalb des
`role="tablist"`).

- Button mit `aria-label="Suchen und filtern"`, Tooltip, `aria-expanded` +
  `aria-controls`; toggelt das Popover.
- **Aktiv-Zustand:** Sind Suchtext oder Filter gesetzt, zeigt der Button es —
  Icon/Fläche in Akzent, bei zählbaren Filtern ein kleiner Zähler-Badge. Der
  Nutzer sieht auch bei geschlossenem Popover, dass die Liste nicht
  vollständig ist.

### Das Popover

Öffnet unter dem Button, rechtsbündig; `--surface-raised`, 1px `--border`,
Radius `--radius-surface`, `--shadow-overlay`, ~360px (mit mehreren Filtern
bis ~520px, 1–2-spaltig), Innenabstand 16px. **Kein Backdrop** — die Liste
bleibt sichtbar und aktualisiert sich live.

Inhalt von oben nach unten:

1. **Suchfeld** (immer): volle Breite, führende Lupe, Platzhalter benennt,
   **wonach** gesucht wird („Suchen (Zweck, Gegenpartei …)"); beim Öffnen
   fokussiert; X zum Leeren, sobald Text steht. Das Platzhalter ersetzt kein
   Label (`sr-only`-Label oder `aria-label`).
2. **Filter** (optional): so wenige wie möglich. Native Selects mit
   „Alle …"-Option als Neutralwert; Mehrfachauswahl/Datumsbereich nur wenn
   fachlich nötig.
3. **Zurücksetzen** (nur bei aktiver Suche/Filtern): räumt alles. Kein
   „Anwenden"-Button — es wird live gefiltert.

### Verhalten

- **Clientseitig geladene Listen:** Live-Filterung bei jedem Tastendruck,
  Volltext über die fachlich sinnvollen Spalten, Filter UND-verknüpft.
- **Serverseitig paginierte Listen:** Suche/Filter wandern in die
  **Query-Parameter** (deep-linkbar), entprellt (~300 ms) ausgewertet.
- Trefferzahl sichtbar („N angezeigt", gefiltert) und als
  `aria-live="polite"`-Region; kein Treffer → Leerzustand der Tabelle mit
  „Keine Treffer für die aktuelle Suche." + Zurücksetzen-Aktion.
- Schließen per Lupe, `Escape`, Außenklick; Fokus zurück zum Button;
  gesetzte Filter bleiben erhalten (der Aktiv-Zustand zeigt es).

## Ausnahme: die dauerhafte Filterleiste

Für Seiten, deren **Hauptarbeit das iterative Filtern** ist (Umsatz-/
Buchungslisten mit Zeitraum, Betrag, Status, Volltext), darf statt des
Popovers eine dauerhafte Filterleiste über der Tabelle stehen (Abwägung:
`entscheidungen.md`):

- Als Karte über der Tabelle; Felder nach Formular-Regeln
  (`05-formulare.md`), aktive Filter erkennbar und einzeln entfernbar.
- Zustand vollständig in der URL; „Filtern" als expliziter Button, wenn
  serverseitig ausgewertet wird — live, wenn clientseitig.
- **Anwendungsweit konsistent:** Eine Anwendung wählt pro Listentyp einmal
  Popover oder Leiste — nicht mal so, mal so.

## Herkunft

Popover-Muster vollständig aus Enon (`patterns/suche.md`), deckungsgleich mit
anlagenmonitors `search-popover` (Aktiv-Badge, Panel-Breite). Tastatur- und
`aria-live`-Regeln: Enon + TorroConnect (Suche: incremental results, empty
state, keyboard). Die Filterleisten-Ausnahme ist neu entschieden
(`entscheidungen.md`).
