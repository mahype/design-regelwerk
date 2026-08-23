# Typografie & Sprache

## Schrift

- **Eine UI-Schrift** (`--font-ui`), wenige Gewichte (400/500/600) — keine
  zusätzlichen Schnitte, keine Kursive laden. Welche Familie, entscheidet die
  Marke; Systemschrift ist ein legitimer Wert.
- **Monospace** (`--font-mono`) nur für technische Werte: IDs, Tokens, JSON,
  Pfade, Logs — dann 12–13px.
- Eine **Display-/Markenschrift** existiert nur, wenn die Marke eine hat,
  und erscheint nur an Markenmoment-Orten — nie als UI-Schrift.

## Größenskala — Rollen fix, Werte als Default

Die Oberfläche ist bewusst kompakt; es gibt nur wenige Stufen. Die Werte sind
die Empfehlungs-Defaults (aus Enon); die Marke darf innerhalb ±1–2px
verschieben, die **Rollen und Verhältnisse** stehen fest:

| Rolle | Default | Gewicht |
| --- | --- | --- |
| Karten-Sondertitel (nur Anmeldekarte) | 18px | 600 |
| Topbar-Seitentitel (einzige Verwendung) | 16px | 600 |
| Topbar-Kontext-Suffix | 16px | 400, sekundär |
| Überschriften im Inhalt: Karten-, Modal-, Abschnittstitel | 14px | 600 |
| Bedienflächen: Buttons, Nav, Tabs | 14px | 500–600 |
| Fließtext, Tabellenzellen, Eingaben | 14px | 400 |
| Labels, Tabellenköpfe | 12px | 500 |
| Sekundärtext: Hints, Untertitel, Badges, Meta | 12px | 400 |

- **Es gibt keine großen Seitentitel.** Der Seitenkontext steht einzeilig in
  der Topbar; im Inhalt beginnt keine `h1` mit 24px+. Große Typografie ist
  Markenmoment-Material, kein Seitenbaustein.
- **Versal-Labels** (Labels + Tabellenköpfe in Großbuchstaben mit leichtem
  `letter-spacing`) sind ein Marken-Slot: einmal entscheiden, dann überall —
  nie gemischt.
- Zeilenhöhe: Engine-Standard; in Text-/Chatblöcken 1.5.

## Zahlen, Beträge, Daten

- **Tabellarische Ziffern** (`font-variant-numeric: tabular-nums`) überall,
  wo Zahlen untereinander stehen: Tabellen, Beträge, Zähler, Zeiten.
- **Beträge rechtsbündig**, mit deutschem Format (`1.234,56 €`), echtes
  Minus, Cent immer zweistellig. Negative Werte nie nur über Farbe
  unterscheiden.
- **Datumsformat** `TT.MM.JJJJ`; relative Angaben („vor 3 Minuten") nur
  zusätzlich zu einem exakten Wert (Tooltip oder Sekundärzeile).
- Intern sind Beträge Integer-Cents und Kalendertage `JJJJ-MM-TT`-Strings —
  die Formatierung ist reine Darstellungsschicht. Die **Eingabe** dieser
  Typen (tolerantes Parsen, kein `type="number"` für Beträge):
  `16-feldtypen.md`.

## Sprache

- **Deutsch, durchgängig** — UI, Fehlermeldungen, leere Zustände, CLI-Ausgaben.
  Fachbegriffe, die im Feld etabliert sind (IBAN, MCP, API), bleiben stehen.
- **Ganze Sätze, Nutzerperspektive.** „Bereit für den Abruf" statt
  „Sync-Status: idle". Interne Ereignisnamen werden übersetzt; Unbekanntes
  zeigt seinen rohen Namen — besser als nichts.
- **Buttons benennen die Aktion:** „Einlesen", „Konto anlegen", „Löschen" —
  nie „OK", „Ja", „Absenden". Der destruktive Bestätigungs-Button nennt die
  Tat („Konto löschen").
- **Fehler sind Diagnosen:** was schiefging + was der Nutzer tun kann, ohne
  Jargon, ohne Schuldzuweisung, ohne Entschuldigungsfloskeln. Technische
  Details (Statuscode, Servertext) dürfen dazu, wenn sie beim Melden helfen.
- **Destruktive Dialoge nennen die Konsequenz.** „Das entfernt nur die
  Konfiguration. Deine Daten auf dem Server bleiben erhalten."
- **Warnungen vorwegnehmen:** Wenn ein Folgeschritt erschrecken wird
  (Fremd-Dialog, lange Wartezeit), kündigt die Oberfläche ihn an — ein
  erwarteter Schritt statt eines gefühlten Fehlers.
- **Anrede** (Du/Sie) und Wortliste kommen aus dem Markenprofil; dieselbe
  Sache heißt überall gleich.

## Herkunft

Skala, Kompaktheit, „keine großen Seitentitel", Uppercase-Regel: Enon
(`foundations/typografie.md`) — Uppercase zum Marken-Slot verallgemeinert
(`entscheidungen.md`). Typografie-als-Hierarchie und Rollen-Erkennbarkeit:
TorroConnect. Nutzersprache, Konsequenz-Nennung, Warnung vorwegnehmen,
monospaced Ziffern: torro-design (`Sprache`). Zahlen-/Datumsformate:
Torro-Billing-Praxis, verallgemeinert.
