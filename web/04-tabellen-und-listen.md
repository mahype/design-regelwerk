# Tabellen & Listen

Listen sind das Rückgrat jeder Fachanwendung. **Homogene Datensätze sind
Tabellen** — Karten(-raster) bleiben heterogenen Vorschau-Objekten
vorbehalten (Übersichts-Kacheln mit Zahl/Status/Link). Formular- oder
Kartenstapel mit allen Details aller Einträge sind keine Übersicht und
verboten.

## Pflicht-Fähigkeiten

Jede Datentabelle kann, sobald die Datenmenge es erfordert:

- **Sortieren** — klickbare Spaltenköpfe, Richtung sichtbar, `aria-sort`.
- **Filtern & Suchen** — nach `08-suche-und-filter.md`.
- **Paginierung** — nach dem Muster unten; Standard-Seitengröße 50.
- **Tastatur** — Zeilenziele fokussierbar, Aktionen per Enter erreichbar.

Eine kleine, immer vollständige Liste (z. B. fünf Konten) darf darauf
verzichten — aber sobald gescrollt oder gesucht wird, gilt die Pflicht.

## Anatomie

Die Tabelle ist eine Karte: `--surface-raised`, 1px `--border`, Radius
`--radius-surface`, `--shadow-raised`, volle Breite.

- **Scroll-Wrapper innen:** `overflow-x: auto` **innerhalb** des gerundeten
  Containers (Ecken clippen sauber; die Seite scrollt nie horizontal).
  Verhalten auf schmalen Screens — Spalten-Priorität, Footer-Umbruch:
  `13-responsive.md`.
- **Kopf (`th scope="col"`):** Fläche `--surface-inset`, 12px medium
  `--text-secondary` (versal je Marken-Slot), Padding 12px 20px,
  linksbündig; Zahlenspalten-Köpfe rechtsbündig wie ihre Werte.
- **Zellen (`td`):** 14px `--text`, Padding 12px 20px, `vertical-align: top`,
  Trennlinien 1px `--hairline` (letzte Zeile ohne).
- **Hover:** Zeile hebt sich mit `--surface-hover` ab (`--motion-fast`).
- **Ausrichtung:** Text links, Zahlen/Beträge rechts mit `tabular-nums`,
  Datumsspalten einheitlich.
- **Sekundärinfo** (IDs, E-Mail, Verwendungszweck-Auszug): 12px
  `--text-secondary` unter dem Hauptwert; technische Werte in `--font-mono`.
- **Zeile als Ziel:** Öffnet die Zeile ein Detail, ist die ganze Zeile
  klickbar (Zeigerhand) **und** es gibt ein explizites Ansehen-Icon — die
  Fläche ist Komfort, das Icon der zugängliche, benannte Weg. Welche
  Detail-Form eine Entität bekommt: `14-detail-ansichten.md`.

## Aktionsspalte

- Immer die **letzte Spalte, rechtsbündig** — gleiche Aktion in jeder Zeile
  an derselben Stelle.
- **Icon-Buttons, nie Text**, als Gruppe mit 6px Abstand; feste Reihenfolge
  **Ansehen → Bearbeiten → Löschen** (nur anbieten, was es gibt, aber stets
  in dieser Ordnung).
- Alle Aktions-Icons **neutral grau — auch Löschen.** Das Danger-Rot
  erscheint erst am Bestätigungs-Button des Dialogs
  (Abwägung: `entscheidungen.md`).
- Jeder Icon-Button: `aria-label` + sichtbarer Tooltip (`11-komponenten.md`).

## Status in Zeilen

- **Text-Badge ist der Standard:** kleine Kapsel, 12px, Fläche
  `--status-*-surface`, Text `--status-*`, 1px Ring `--status-*-border`.
  Der Text benennt den Status — keine Punkte, Icons oder Verläufe im Badge.
- **Der reine Statuspunkt** (8–9px Kreis in Statusfarbe) ist nur in dichten
  Übersichten zulässig, immer mit Tooltip + zugänglichem Namen, und die
  Bedeutung muss zusätzlich als Text erreichbar sein (Detail, Legende) —
  monochrom benutzbar bleibt Pflicht.
- Statusfarben sind semantisch fix (`marke/token-kontrakt.md`); die
  Markenfarbe ist nie Status.

## Überlange Inhalte: Kürzen & Umbrechen

- **Zahlen und Beträge werden nie abgeschnitten** — eher bekommt die
  Spalte mehr Platz oder eine andere weicht (`13-responsive.md`).
- **Spaltenköpfe** bleiben einzeilig und kürzen mit Ellipsis — sie
  brechen nie um.
- **Zellwerte** sind standardmäßig einzeilig mit Ellipsis; der volle
  Wert ist immer **eine Interaktion entfernt** (Tooltip bei Hover und
  Fokus, oder das Detail). Mehrzeilig nur für ausgewiesene Textspalten
  (Verwendungszweck), dann mit festem Zeilen-Limit (`line-clamp`).
- **Kennungen** (Hashes, lange IDs) dürfen mittig gekürzt werden, wenn
  Anfang und Ende die Erkennung tragen; kopiert wird immer der Rohwert.
- Die **Identitätsspalte** (was den Datensatz benennt) bekommt beim
  Platzverteilen Vorrang — sie kürzt zuletzt.

## Export

Auf Listen, zu deren **Arbeit der Export gehört** (Buchungen, Umsätze,
Auswertungen), sitzt ein Export-Icon-Button in der Aktionsleiste
(`02-app-shell.md`) — nicht auf jeder Liste.

- Der Button öffnet ein kleines Dropdown mit **„CSV" und „Excel
  (XLSX)"** — beide Formate immer gemeinsam.
- **Exportiert wird, was die Liste zeigt:** aktuelle Filterung und
  Sortierung, sichtbare Spalten in Listenreihenfolge — nie heimlich
  alles. Die Trefferzahl steht im Dropdown („1.234 Zeilen").
- **CSV:** Semikolon als Trenner, UTF-8 **mit BOM** (sonst zerlegt
  Excel die Umlaute), Formate wie angezeigt (`1.234,56`, `TT.MM.JJJJ`).
  **XLSX:** echte Typen — Zahlen als Zahlen, Daten als Datum.
- **Dateiname:** `entitaet_JJJJ-MM-TT.csv` / `.xlsx`.
- Große Mengen laufen **asynchron** mit Fortschritt (`07`); der
  Abschluss startet den Download und beziffert das Ergebnis
  („1.234 Zeilen exportiert").

## Paginierung / Listen-Footer

Bei größeren Datenmengen sitzt die Steuerung **immer unten im Footer** der
Tabelle, dreigeteilt (`justify-between`):

1. **Links:** Navigation (erste / vorige / Seiten / nächste / letzte).
2. **Mitte:** Standortanzeige „Zeige 1–50 von 1.234" — als
   `aria-live="polite"`-Region, damit Screenreader Seitenwechsel mitbekommen.
3. **Rechts:** Ansichts-Einstellungen — Seitengröße (perPage-Dropdown) und,
   falls vorhanden, der Umschalter zwischen Ansichten.

Alle Footer-Steuerelemente in **derselben Größe und Optik** (quadratische
40px-Buttons, `--surface-raised`, 1px `--border`, `--shadow-raised`,
Fokus-Ring); Seiten-/Filterzustand in der URL.

## Leere Listen

Nie eine leere Tabelle (oder eine kommentarlos leere Fläche) rendern.
Der Leerzustand erklärt in der Karte: **was hier stünde**, **warum es leer
ist** (noch nichts angelegt? Filter zu eng?) und **was der nächste Schritt
ist** — als echte Aktion („Konto anlegen") bzw. „Filter zurücksetzen" bei
leerem Suchergebnis.

## Auswahl & Massenaktionen

Wenn fachlich nötig: Checkbox-Spalte ganz links, „alle auswählen" im Kopf
(indeterminate bei Teilauswahl), Auswahl sichtbar markiert; Massenaktionen
erscheinen als Leiste über der Tabelle mit Zähler („3 ausgewählt") — nicht
als dauerhaft sichtbare Buttonreihe.

## Herkunft

Karten-Anatomie, Kopf-/Zellen-Maße, Aktionsspalte, Badge-Schema,
Leerzustand: Enon (`komponenten/tabellen.md`). Pflicht-Fähigkeiten:
TorroConnect (interaction-patterns). Innerer Scroll-Wrapper,
Aktions-Reihenfolge, neutrale Aktions-Icons, Paginierungs-Anatomie,
Footer-Button-Disziplin: anlagenmonitor (Abschnitte 6, 6a, 7).
Statuspunkt-mit-Tooltip: torro-design, auf den Web-Kontext eingeschränkt.
Tabellen-statt-Karten: Abwägung in `entscheidungen.md`. Kürzungs-Regeln
nach Fiori („Wrapping and Truncation": Zahlen nie kürzen, Köpfe nie
umbrechen) und Carbon („Overflow content"); Export neu entschieden
(`entscheidungen.md`), CSV-Details aus Excel-Praxis.
