# Komponenten: Strategie & Basisinventar

## Strategie: selbst definieren statt Bibliothek

**Kein Komponenten-Framework und kein UI-Kit als Fundament** — kein
shadcn/ui, kein Mantine/MUI/Chakra, kein Tailwind-UI. Auch nicht vorschlagen.
(Abwägung und Vorgeschichte: `entscheidungen.md`.)

Die Rangfolge beim Bauen:

1. **Natives HTML zuerst.** `<dialog>` (Modals), das `popover`-Attribut
   (Popover/Menüs), `<details>` (Disclosure), native `<select>`, `<input
   type="date">`, Formular-Validierungs-API. Das System liefert Fokus-Fallen,
   Tastatur und Semantik gratis — Eigenbau darüber nur, wenn Natives fachlich
   nicht reicht.
2. **Eigene Komponenten aus dem Token-Kontrakt.** Klein, fokussiert,
   wiederverwendbar; Namen nach Zweck, nicht Aussehen. Vor jeder neuen
   Komponente: existiert schon eine? Keine Duplikate desselben Musters,
   keine One-off-Komponenten pro Seite.
3. **Headless-Primitives nur für nachweislich komplexe Widgets** — Combobox
   mit Typeahead, Mehrfachauswahl mit Chips, Datumsbereich, verschachtelte
   Menüs. Dort ist das Verhalten (Fokus-Management, ARIA, Portale) so
   fehlerträchtig, dass Eigenbau unverantwortlich ist. Die Optik kommt
   trotzdem zu 100 % aus unseren Tokens.
4. **Logik-Bibliotheken ohne eigene Optik sind erlaubt:** TanStack
   Table/Virtual (Tabellen-Logik), Floating UI (Positionierung), Zod,
   date-fns. Sie rendern nichts Eigenes und binden keine Ästhetik.

**Muster-Studium ist ausdrücklich erwünscht:** Verhalten aus dem
**WAI-ARIA Authoring Practices Guide (APG)** und aus guten Bibliotheken
übernehmen — Tastaturbelegung, Fokus-Reihenfolge, ARIA-Attribute. Die Optik
nie.

### Die Bibliotheksfrage im Einzelnen (Stand 2026-08)

| Kandidat | Einordnung | Entscheidung |
| --- | --- | --- |
| **shadcn/ui** | Kopier-Kit vor-gestylter Komponenten (Radix/Base UI + Tailwind-Optik) | **Nein.** Bringt eine fremde, sofort wiedererkennbare Ästhetik als Startpunkt mit — das Gegenteil des Marken-Slot-Modells; kopierte Komponenten altern im Repo |
| **Base UI** (MUI-/Ex-Radix-Team) | Headless-Primitives, ungestylt, v1.0 seit Ende 2025, aktiv gepflegt | **Erste Wahl**, wenn Stufe 3 greift |
| **React Aria** (Adobe) | Hook-basiert, strengste A11y/i18n, mehr Eigencode | **Alternative** zu Base UI bei höchsten A11y-Ansprüchen |
| **Radix Primitives** | Der frühere Standard; Pflege seit der WorkOS-Übernahme verlangsamt, Kernteam baut heute Base UI | Nicht für Neues; Bestand nicht panisch migrieren |
| **Headless UI, Ark UI** | schmaler bzw. jünger | keine Einwände, aber kein Grund, von Base UI abzuweichen |
| **Voll-Frameworks** (Mantine, MUI, Chakra, Flux …) | gestylte Komplettsysteme | **Nein** für Neues; bestehende Projekte damit (anlagenmonitor/Flux) bleiben |

CSS-Ansatz (Vanilla oder Tailwind) wählt das Projekt; Werte kommen in beiden
Fällen aus dem Token-Kontrakt.

## Basisinventar

Die Bausteine, die jede Anwendung braucht — mit ihren Kernregeln. Maße und
Farben immer aus dem Token-Kontrakt.

### Buttons

Alle: `inline-flex`, zentriert, Radius `--radius-control`, 14px semibold,
Padding 8px 16px (klein: 8px 12px, 12px Schrift), aktive Elemente mit
Zeigerhand, disabled 50 % Opazität ohne Zeigerhand.

| Variante | Aussehen | Verwendung |
| --- | --- | --- |
| **Primär** | Fläche `--accent`, Text `--text-on-accent`; Hover `--accent-hover` | Die Hauptaktion — **genau einer pro Sichtbereich.** Der Nutzer erkennt an der Akzentfarbe, wo es weitergeht |
| **Sekundär** | `--surface-raised`, 1px `--border`, Text `--text` | alles andere: Abbrechen, Export, Test … |
| **Danger** | Fläche `--status-error-surface`, 1px `--status-error-border`, Text `--status-error` | destruktive **Bestätigung** im Dialog-Fuß — nie als Primär-Stil, nie in Zeilen |
| **Icon-Button** | 36×36px, `--surface-hover`-Fläche, 1px `--border`, Icon sekundär | Topbar, Aktionsleiste, Tabellen-Aktionen, Paginierung — immer `aria-label` + Tooltip |

Mit Icon: 16px-Icon + 8px Abstand zum Text. Keine Ghost-/Link-Buttons
erfinden; Links im Fließtext sind Links (`--accent-text`, unterstrichen).
Disabled sparsam — besser aktiv lassen und beim Klick erklären, warum etwas
nicht geht.

### Eingaben

Siehe `05-formulare.md` (Feld-Anatomie, Select-Pfeil-Regel, Checkbox-Listen,
Datei-Input, Fokus) und `16-feldtypen.md` (Regeln je Datentyp:
Datum, Betrag, IBAN, Combobox-Schwelle, Passwort, Drop-Zone).

### Hinweisboxen

Ein Schema, fünf Bedeutungen: Fläche `--status-*-surface`, 1px
`--status-*-border`, Text im Vollton; Radius `--radius-control`, Padding
8px 12px, Text 14px (kompakt 12px). Icon optional (16px, Textfarbe).
**Eine Box pro Kontext**; Platzierung nach `07-rueckmeldung.md`. Ein
neutraler Hinweis ohne Dringlichkeit ist keine Box, sondern Sekundärtext.
Farbfamilien sind fest an ihre Bedeutung gebunden — nie Warnfarbe für
Fehler, nie Statusfarbe als Deko.

### Badges & Chips

Status-Badges nach `04-tabellen-und-listen.md`. Zähler (offene Posten) als
kleine Kapsel in Akzent oder neutral — zählbar Offenes darf am
Sidebar-Eintrag als Badge stehen. Chips für Auswahl/Presets: neutrale
Kapsel, aktiver Chip hebt sich über Fläche + Haarlinie ab, nie nur über
Farbe.

### Tooltips

Für Icon-Buttons Pflicht (Hover **und** Fokus, mit kleiner Verzögerung
~500 ms). Tooltips tragen nie Pflichtinformation — was der Nutzer wissen
muss, steht sichtbar.

### Karten

Karten gruppieren Zusammengehöriges (`--surface-raised`, 1px `--border`,
Radius `--radius-surface`, `--shadow-raised`, Innenabstand 20px). Keine
Karten in Karten; klare Abschnitte statt visueller Verschachtelung. Ist die
ganze Karte klickbar: Zeigerhand, Hover-Verstärkung (`--motion-fast`) und
ein benanntes Link-Ziel für die Zugänglichkeit.

### Code-Blöcke & Geheimnisse

`--font-mono` 12–13px auf `--surface-inset`, Radius `--radius-control`,
Innenabstand 12px; **scrollt horizontal statt umzubrechen**. Schlüssel sind
auf dem Schirm **maskiert**; „Schlüssel zeigen" ist ein bewusster Klick,
kopiert wird immer der echte Wert — angezeigter und kopierter Text sind
sonst identisch.

### Avatare

Initialen-Avatar 36×36px, Radius `--radius-control`, neutrale Fläche,
Initialen sekundär — kein Foto-Zwang.

## Herkunft

Rangfolge und Framework-Verbot: Enon Regel 1, verallgemeinert und mit der
Headless-Ausnahme präzisiert (`entscheidungen.md`); Bibliotheks-Einordnung
nach Recherche-Stand 2026-08. Button-Varianten, Hinweis-Schema,
Icon-Button-Maße: Enon (`komponenten/buttons.md`, `hinweise.md`).
Ein-Primär-Regel: Enon + torro-design (borderedProminent genau einmal).
Komponentendisziplin (klein, zweckbenannt, keine Duplikate): TorroConnect
(components.md). Code-Block/Secret-Regeln: torro-design, webifiziert.
Tooltip- und Icon-Button-Pflichten: anlagenmonitor.
