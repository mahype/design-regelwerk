# Feldtypen & Upload

`05-formulare.md` regelt Ort, Anatomie und Ablauf von Formularen — dieses
Kapitel ist das **Nachschlagewerk je Datentyp**: welches Element, welche
Eingabe-Toleranz, welches Format. Es gilt überall, wo der Typ vorkommt
(Formulare, Filter, Anmeldung). Grundlinie bleibt `11-komponenten.md`:
natives HTML zuerst, Headless nur für nachweislich komplexe Widgets.

## Grundregeln für alle Typen

- **Anatomie aus `05`:** Label über dem Feld, Hilfetext darunter, Fokus-
  Ring, Fehler-Markierung + Fehlerblock.
- **`autocomplete`-Attribute sind Pflicht**, wo ein definierter Zweck
  existiert (`name`, `email`, `tel`, `street-address`,
  `current-password`, `new-password`, `one-time-code` …) — der Browser
  füllt schneller, und WCAG 2.1 AA (1.3.5) verlangt es.
- **`inputmode` passend setzen** (`decimal`, `numeric`, `email`) — auf
  Touch erscheint die richtige Tastatur.
- **Read-only ist nicht disabled.** Nicht-Editierbares wird als
  Anzeige-Feld gezeigt (`14-detail-ansichten.md`: Label über Wert) —
  voll lesbar, kopierbar, nie ausgegraut. Disabled bleibt dem „geht
  gerade nicht"-Fall vorbehalten (`05`).
- **Eingaben sind tolerant, Anzeigen sind normiert:** Was der Nutzer
  eintippt, wird großzügig geparst (Leerzeichen, Komma/Punkt,
  Groß/Klein); beim Verlassen des Felds erscheint der normierte Wert
  (`03`-Formate). Gespeichert wird immer das interne Format.

## Text & Kennungen

- **Einzeiler** für Namen und Bezeichnungen; **Textarea** mit fester
  Zeilenzahl (scrollt) für Freitext.
- **Zeichenzähler nur bei harten fachlichen Limits**, live sichtbar
  („Noch 20 Zeichen", `aria-live="polite"`). Ein `maxlength` ohne
  sichtbaren Zähler ist verboten — es schneidet Eingefügtes still ab.
- **Technische Kennungen** (IDs, Schlüssel, JSON): `--font-mono`
  (`05`).
- **IBAN:** Eingabe tolerant (Leerzeichen und Kleinschreibung erlaubt),
  beim Verlassen normalisiert und **in Vierergruppen** angezeigt
  (`--font-mono`); Prüfsummen-Validierung beim Verlassen, Fehlertext
  konkret („Diese IBAN ist ungültig — bitte prüfen, DE… hat 22
  Stellen").

## E-Mail & Telefon

- `type="email"` bzw. `type="tel"` mit passendem `autocomplete`.
- **Telefonnummern nicht in Formate zwingen:** ein Feld, tolerant
  angenommen; nur grob geprüft (Ziffern/+ / Leerzeichen).

## Zahlen, Beträge, Prozente

- **Beträge: kein `type="number"`.** Stattdessen `type="text"` +
  `inputmode="decimal"` — `number` scheitert am deutschen Komma und
  ändert Werte per Scrollrad. Eingabe mit Komma (Punkt wird toleriert),
  rechtsbündig, Währung als Suffix **neben** dem Feldwert; beim
  Verlassen normiert formatiert („1.234,56"). Intern Integer-Cents
  (`03`).
- **Mengen/Ganzzahlen:** `inputmode="numeric"`; `type="number"` nur,
  wenn Schrittlogik (min/max/step) wirklich gebraucht wird.
- **Prozente** wie Beträge, mit %-Suffix und genannter Spanne.
- **Einheiten** (kWh, Stück) stehen als Suffix am Feld, nie im Wert.

## Datum & Zeit

- **Einzeldatum:** natives `<input type="date">` — zeigt lokal
  TT.MM.JJJJ, der Wert bleibt `JJJJ-MM-TT` (`03`). `min`/`max` setzen,
  wo fachlich klar.
- **Erinnerte Daten** (Geburtsdatum, Belegdatum aus einem Dokument):
  **nie einen Kalender erzwingen** — getippt ist schneller als
  geblättert (GOV.UK-Linie); die native Eingabe erlaubt beides.
- **Zeitraum:** zwei Datumsfelder „Von" / „Bis" nebeneinander;
  Von ≤ Bis wird beim Verlassen geprüft. Ein Headless-Range-Picker erst,
  wenn das Nebeneinander nachweislich nicht reicht (Stufe 3, `11`).
  Wo iteratives Filtern die Hauptarbeit ist (`08`-Filterleiste), dürfen
  **Presets als Chips** daneben stehen („Dieser Monat", „Letzte 30
  Tage").
- **Uhrzeit:** `<input type="time">`, 24-Stunden-Format.

## Auswahl

- **Natives Select bis ~15 stabile Optionen** (mit Neutral-Option
  „Alle …" bzw. „Bitte wählen", je Kontext; Pfeil-Regel aus `05`).
- **Darüber — oder bei dynamisch wachsender Menge (Konten, Kontakte)
  oder Suchbedarf — die Combobox** mit Typeahead: Headless-Primitive
  (Stufe 3, `11`), Optik komplett aus den Tokens. Verhalten: filtert ab
  dem ersten Zeichen, Pfeiltasten + Enter wählen, „Keine Treffer" wird
  angezeigt (nie leere Liste), X leert das Feld; ARIA nach APG.
- **Mehrfachauswahl:** Checkbox-Liste (`05`) bis ~15 Optionen; darüber
  Multi-Combobox mit Chips (`11`).
- **Radio vs. Select:** Bis ~5 stabile Optionen, die der Nutzer
  vergleichen soll → Radios (alle sichtbar); sonst Select.
- **Ja/Nein ist eine Checkbox** — nie ein Zwei-Optionen-Select, kein
  eigener Switch-Baustein.

## Passwörter & Codes

- **Anzeigen-Umschalter** (Auge-Icon-Button im Feld, `aria-pressed`,
  Tooltip); **Einfügen nie blockieren** (Passwort-Manager!).
- `autocomplete="current-password"` beim Anmelden,
  `"new-password"` beim Setzen; Regeln fürs neue Passwort stehen
  **vorab** als Hilfetext, nicht erst als Fehler.
- **Einmal-Codes:** ein einziges Feld mit
  `autocomplete="one-time-code"` + `inputmode="numeric"` — keine sechs
  Einzelkästchen.

## Upload

Zwei Stufen — das Kriterium ist die Rolle der Dateien:

1. **Beiläufige Einzeldatei** (Logo, Avatar, ein Anhang in einem
   Formular): der schlichte **Datei-Input** aus `05`. Die Datei wird mit
   dem Formular gespeichert.
2. **Wiederkehrender oder Mehrfach-Upload** (Belege einlesen, Anhänge
   verwalten): die **Drop-Zone**.

### Drop-Zone

- **Fläche:** volle Breite, 1px gestrichelter Rahmen `--border-input`,
  Radius `--radius-control`, Innenabstand 24px; zentriert Upload-Icon
  (24px, sekundär) + Satz „Dateien hierher ziehen oder **auswählen**".
  Die ganze Fläche ist klickbar und ein echtes
  `<input type="file" multiple>` (per Label) — fokussierbar mit
  Fokus-Ring; Drag-over färbt die Fläche `--accent-soft`.
- **Zulässiges vorab nennen:** erlaubte Typen und Maximalgröße stehen
  als Hilfetext unter der Zone — nicht erst in der Fehlermeldung.
- **Sofort-Upload:** Dateien laden beim Ablegen sofort hoch, nicht erst
  beim Speichern. Jede Datei wird eine **Zeile in der Liste darunter**:
  Name (14px) + Größe (12px sekundär), währenddessen Fortschrittsbalken
  + Abbrechen-X; danach Status und eine neutrale Entfernen-Aktion
  (Grau-Linie aus `04`; endgültiges Löschen bestätigt sich nach `06`).
- **Fehler je Datei, in der Zeile:** was + warum + was tun („zu groß —
  höchstens 10 MB"); andere Dateien laufen weiter. Duplikate werden
  gemeldet, nie still ersetzt. Der Fortschritt insgesamt ist
  `aria-live="polite"` („2 von 5 hochgeladen"), Datei-Fehler
  `role="alert"` (`07`).
- **Tastatur und Touch:** Ziehen ist nur der Zusatzweg — Auswahl-Dialog
  per Klick/Enter ist der vollwertige Weg; auf Touch öffnet Tippen den
  Dialog.
- **Nachbearbeitung** (Hochladen + Zuordnen + Verlauf) bleibt der
  Seiten-Fall aus der `05`-Schwelle — die Drop-Zone ersetzt keine
  Verwaltungsseite.

## Herkunft

Neues Kapitel (Lücken-Analyse B1/B3; Abwägung: `entscheidungen.md`).
Datentyp-Regeln nach dem Vorbild von GOV.UK „Ask users for …"
(Datum-ohne-Kalenderzwang, Zeichenzähler, Passwort-Anzeigen,
`autocomplete`) und Polaris' Formatregeln; Combobox-/Auswahl-Schwellen
und Zwei-Stufen-Upload neu entschieden. Drop-Zone-Verhalten angelehnt an
Polaris Drop zone, Carbon File uploader und Fiori Upload Set — Optik und
Meldungswege aus diesem Regelwerk (`04`–`07`). Betrags- und IBAN-Regeln:
Torro-Billing-Praxis, verallgemeinert (`03`).
