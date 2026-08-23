# Formulare

## Ort: Modal als Standard, Seite ab Schwelle

Anlegen und Bearbeiten von Datensätzen geschieht **im Modal**
(`06-dialoge.md`) — nie inline über, unter oder neben einer Liste, auch nicht
als feste Formular-Spalte. Die Übersicht listet, das Detail bearbeitet genau
eine Entität; der Plus-Button der Aktionsleiste öffnet das Anlegen-Modal, die
Bearbeiten-Aktion der Zeile das Einzel-Detail.

Eine **eigene Seite** (oder ein mehrstufiger Ablauf) ist vorgeschrieben,
sobald eines zutrifft (Abwägung: `entscheidungen.md`):

- mehr als ~8 Felder trotz fachlicher Reduktion,
- mehrere inhaltliche Abschnitte, die Tabs bräuchten,
- Datei-Verwaltung mit Nachbearbeitung (Hochladen + Zuordnen + Verlauf),
- der Vorgang muss unterbrechbar oder per URL teilbar sein.

Welche **Ansichts**-Form eine Entität bekommt (Modal, Detailseite,
Master-Detail-Split) und wie auf Detailseiten bearbeitet wird
(abschnittsweise im Modal): `14-detail-ansichten.md`.

## Weniger Felder, bewusste Reihenfolge

Ein Modal ist kein Freibrief für Feldmengen. Vor der Umsetzung wird fachlich
reduziert:

- Felder nach Aufgabe und Häufigkeit priorisieren; Seltenes und Technisches
  in eine nachgelagerte Detailstufe.
- Wenige, klar benannte Abschnitte; abhängige Felder erst zeigen, wenn die
  auslösende Auswahl getroffen ist.
- Umfangreiche, trennbare Aufgaben als Schritte (Wizard, Abschnitt unten):
  pro Schritt nur die nötigen Felder.
- Unnötige Pflichtfelder vermeiden; **Optionales als „(optional)"
  kennzeichnen**, nicht Pflicht mit Sternchen-Wald markieren.

## Felder

Regeln je **Datentyp** — Datum/Zeitraum, Betrag, IBAN, Auswahl/Combobox,
Passwort, Upload, `autocomplete`-Pflichten: `16-feldtypen.md`.

- **Label über dem Feld**, 12px medium `--text-secondary` (versal je
  Marken-Slot), 4px zum Input, 16px zum vorherigen Feld. Jedes Eingabefeld
  hat ein echtes `<label>` — Platzhalter ersetzen keine Labels.
- **Input / Select / Textarea:** volle Breite, `--surface-raised`, 1px
  `--border-input`, Radius `--radius-control`, Padding 8px 12px, 14px
  (ergibt ~36px Kontrollhöhe — einheitlich für alle Controls).
- **Fokus:** Rahmen + Ring in `--focus-ring`; nie `outline: none` ohne
  Ersatz.
- **Hilfetext:** 12px `--text-secondary` unter dem Feld — Erklärungen
  gehören ans Feld, nicht als Intro-Text auf die Seite.
- **Select:** nativ belassen (kein Custom-Dropdown), aber der
  Browser-Pfeil wird ersetzt: `appearance: none` + eigenes Chevron-SVG als
  `background-image` (16px, 12px vom rechten Rand, `padding-right: 36px`,
  Farbe `--text-secondary` je Theme). Als **globale `select`-Regel** im
  Stylesheet, nicht pro Element — nur so wird keins vergessen.
  `select[multiple]`/`[size]` ausnehmen.
- **Checkbox/Radio:** nativ mit `accent-color: var(--accent)`; Label 14px
  normal. Mehrere Checkboxen als gerahmte Liste (1px `--border`, Radius
  `--radius-control`, Padding 12px, 8px Abstand).
- **Datei-Input:** wie ein Input (Höhe 40px), der Datei-Knopf darin als
  graue Pille (`--surface-hover`, 12px Text) — für die beiläufige
  Einzeldatei; wiederkehrender oder Mehrfach-Upload nutzt die Drop-Zone
  (`16-feldtypen.md`).
- **Technische Eingaben** (JSON, Schlüssel): `--font-mono`, 12–13px.
- **Disabled:** 50 % Opazität, keine Zeigerhand — aber sparsam: besser aktiv
  lassen und beim Klick erklären, warum etwas gerade nicht geht.

## Layout in Modals und auf Seiten

- **Zweispaltig ab vielen Feldern** (Grid, `gap` 24/16px); breite Felder
  (Notizen, Auswahllisten) über beide Spalten. Einspaltig bleibt der
  Default. Der Umbruch auf einspaltig folgt dem **Behälter** (Container
  Query, `13-responsive.md`), nicht dem Viewport.
- **Auf Seiten sitzen Felder nie nackt auf dem Seitengrund:** pro
  inhaltlichem Abschnitt eine Karte (`--surface-raised`, Radius, Rahmen,
  Schatten) mit Abschnittstitel **in** der Karte. In Modals übernimmt der
  Modal-Body diese Rolle.

## Mehrstufige Abläufe (Wizard)

Für umfangreiche, fachlich trennbare Aufgaben — im Modal (mit konstanter
Höhe, `06-dialoge.md`) oder als Seite (ab der Schwelle oben).

- **2–8 Schritte.** Wer mehr braucht, schneidet die Aufgabe neu.
- **Schrittanzeige im Kopf:** „Schritt 2 von 4 — Zuordnung". Besuchte
  Schritte sind anklickbar, kommende nicht.
- **Fußzeile:** „Zurück" (sekundär, links neben dem Primär-Button) +
  „Weiter" (der eine Primär-Button). Im letzten Schritt benennt der
  Primär-Button die Tat („Konto anlegen") — nie „Fertig".
- **Validierung je Schritt** beim Weiter-Klick (Fehlerblock im Schritt,
  Regeln unten) — Fehler stauen sich nie bis zum Ende auf.
- **Zusammenfassung ab 4 Schritten Pflicht:** Der letzte Schritt ist dann
  „Prüfen & Bestätigen" — alle Angaben als Anzeige-Felder
  (`14-detail-ansichten.md`) mit „Ändern"-Links zurück in den jeweiligen
  Schritt; **geschrieben wird erst bei der Bestätigung.** Kurze Wizards
  (2–3 Schritte) dürfen direkt im letzten Schritt abschließen.
- **Abbruch** verhält sich wie beim Modal (`06`): Eingaben-Schutz,
  Nachfrage vor dem Verwerfen. Serverseitige Zwischenstände gibt es
  nicht — ein Wizard schreibt am Ende.

## Validierung & Fehler

- **Ein Fehlerblock pro Kontext** (Hinweisbox in der Fehler-Variante) über
  den Abschluss-Buttons trägt die Meldung: was schiefging + was zu tun ist.
- Betroffene Felder werden zusätzlich **markiert**: Rahmen in
  `--status-error`, `aria-invalid="true"`, Zuordnung per
  `aria-describedby`. Fehler nie nur über Farbe.
- Format-prüfbare Felder validieren beim Verlassen des Felds; die
  Vollprüfung beim Absenden. Eine neue Meldung ersetzt die alte; beim
  erneuten Absenden verschwindet die alte sofort.
- **Eingaben bleiben erhalten** — ein Fehler wirft nie Eingegebenes weg.

## Absenden

- Genau **ein Primär-Button**; Sekundär („Abbrechen") links davon,
  rechtsbündig unter dem Formular, 16px Abstand nach oben.
- **Doppel-Ausführung verhindern:** beim Absenden Button disablen (50 %
  Opazität) + Ladezustand im Button („Speichern…"); zusätzlich serverseitige
  Idempotenz, wo Doppelklicks Schaden anrichten.
- Erfolg wird bestätigt und der Kontext aktualisiert (`07-rueckmeldung.md`);
  das Modal schließt erst nach Erfolg.
- **Einmalig sichtbare Geheimnisse** (frisch erzeugte Tokens): in einer
  Hinweisbox mit `--font-mono`, `word-break: break-all` und dem Satz, dass
  der Wert nur jetzt sichtbar ist; kopiert wird immer der echte Wert.

## Herkunft

Modal-Pflicht, Feld-Anatomie, Select-Pfeil-Regel, Checkbox-Liste,
Datei-Input, Fehlerblock, Secret-Box, Abschluss-Zeile: Enon
(`komponenten/formulare.md`). Feld-Markierung + frühe Validierung +
Eingaben-erhalten: TorroConnect — Zusammenführung mit dem Fehlerblock in
`entscheidungen.md`. Zweispalten-Grid und Karten-Pflicht für Seitenformulare:
anlagenmonitor (5, 5a). Seiten-Schwelle: neu entschieden
(`entscheidungen.md`). Wizard-Anatomie: neu, nach den Vorbildern Fiori
(Wizard-Floorplan, Schrittgrenzen) und GOV.UK („Check answers");
Zusammenfassungs-Schwelle ab 4 Schritten in `entscheidungen.md`.
