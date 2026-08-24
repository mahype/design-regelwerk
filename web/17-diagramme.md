# Diagramme & Kennzahlen

Diagramme sind Werkzeuge, keine Deko. Ein Diagramm existiert nur, wenn es
eine **operative Frage** beantwortet („Wie entwickelt sich der Umsatz?",
„Wo steht die Auslastung?") und in die Arbeit verlinkt — dieselbe Regel
wie für Dashboards (`02-app-shell.md`). Wer exakte Werte vergleichen
will, bekommt eine Tabelle (`04`); das Diagramm zeigt Verlauf, Verhältnis
und Ausreißer.

## Erlaubte Typen

| Typ | Für | Grenzen |
| --- | --- | --- |
| **Linie / Fläche** | Zeitreihen, Verläufe | Fläche nur für einzelne Reihen, nicht gestapelt |
| **Balken** (auch horizontal) | Vergleiche zwischen Kategorien | gestapelt sparsam, höchstens ~4 Teile |
| **Donut** | Anteile am Ganzen | höchstens ~5 Segmente, kleinere als „Weitere" gebündelt; der Gesamtwert steht in der Mitte |
| **Gauge/Tacho** | Auslastungs- und Füllgrade gegen feste Schwellen (Monitoring) | Schwellen-Zonen in Statusfarben, Wert als Zahl daneben |

Alles andere (Streudiagramm, Treemap, 3D, …) ist erst erlaubt, wenn der
Bedarf dokumentiert und entschieden wurde (`entscheidungen.md`). Keine
3D-Effekte, keine Verläufe als Deko, keine Einschwing-Shows
(`09-icons-und-bewegung.md`; `prefers-reduced-motion` schaltet
Diagramm-Animationen ab).

## Farben

- **Kategoriale Reihen** nutzen die Chart-Tokens `--chart-1` … `--chart-6`
  (`marke/token-kontrakt.md`) — ein Marken-Slot: in beiden Themes gegen
  die Kartenfläche ≥ 3:1, untereinander auch bei Farbfehlsicht
  unterscheidbar (Helligkeit staffeln). Einzelne Reihen nehmen
  `--chart-1`; die Marke darf ihn auf ihren Akzent legen.
- **Statusfarben nur für echte Zustände und Schwellen** (Gauge-Zonen,
  Fehler-Reihen) — nie als hübsche Abwechslung. Die Markenfarbe ist auch
  im Diagramm nie Status (`marke/markenprofil.md`).
- **Monochrom benutzbar:** Reihen werden zusätzlich zur Farbe
  unterschieden — bevorzugt **direkte Beschriftung** (am Linienende, am
  Balken), sonst Marker-Formen; eine Legende allein trägt nicht. Werte
  sind außerdem als Tabelle erreichbar (unten).

## Anatomie & Verhalten

- Ein Diagramm lebt in einer **Karte** (`11`): Der Kartentitel ist die
  Frage („Umsatz je Monat"), daneben sichtbar Zeitraum/Filter; bei
  Live-Daten die Stand-Anzeige (`07`).
- **Achsen:** beschriftet mit Einheit, deutsche Formate und tabellarische
  Ziffern (`03`); lieber weniger Ticks als schräge Labels.
- **Tooltip** nach `03`-Formaten (Betrag, Datum); er trägt Komfort, keine
  Pflichtinformation.
- **Zustände nach `07`:** Skeleton der Fläche beim Erstladen, danach
  stabil; Leerzustand mit Dreiklang statt leerer Achsen („fertig, aber
  leer" gibt es nicht); Fehler mit nächstem Schritt.
- **Interaktion sparsam:** Hover/Fokus-Tooltip; ein Klick auf ein Element
  darf in die Arbeit führen (die passend gefilterte Liste). Zeitraum-Zoom
  nur auf Monitoring-Zeitreihen, wo er gebraucht wird — kein
  Brush/Zoom-Standard.

## Die Tabellen-Alternative (Pflicht)

Jede Diagramm-Karte macht ihre Werte **als Tabelle erreichbar** — ein
Umschalter in der Karte („Als Tabelle anzeigen", Icon-Button mit Tooltip)
oder ein benannter Ziel-Link. Das Canvas selbst ist für Screenreader
verborgen (`aria-hidden`) und durch einen benannten Ersatz vertreten
(`aria-label` mit der Kernaussage); die Tabelle ist der zugängliche Weg.
Das erfüllt zugleich die Monochrom-Regel (`01`).

## KPI-Kachel

Der Baustein, mit dem Dashboards (`02`) ihre Zahlen zeigen:

- **Karte** (`11`) mit: Label oben (12 px sekundär), **Wert** in der
  Kennzahl-Rolle (24 px/600, `tabular-nums` — die einzige große Zahl der
  Typo-Skala, `03`), Einheit daneben (14 px sekundär).
- Optional: **Status-Badge** (`04`-Schema) oder **Trend** (Pfeil + Delta
  mit echtem Vorzeichen; Farbe nur zusätzlich zum Zeichen).
- **Ziel-Link:** Die Kachel verlinkt in die Arbeit („4 offene Posten →")
  — als klickbare Karte nach `11` (Zeigerhand, Hover, benanntes Ziel).
- KPI-Raster fließen als Karten-Raster (`13-responsive.md`).

## Bibliothek: Apache ECharts als Default

- **Default ist Apache ECharts** — ein Marken-/Projekt-Slot wie das
  Icon-Set: Abweichung nur mit dokumentiertem Grund im Markenprofil.
  ECharts deckt alle erlaubten Typen (inkl. Gauge) nativ ab und bleibt
  bei großen Zeitreihen (Monitoring) flüssig; Apache-2.0.
- **Das Theme kommt zu 100 % aus den Tokens:** einmal zentral als
  ECharts-Theme registriert (Farben `--chart-*` und Statusfarben,
  `--font-ui`, Linien `--hairline`, Text `--text-secondary`) — nie
  Farben in einzelnen Chart-Configs (Siemens-iX-Vorbild).
- Die Bibliothek wird **nur auf Seiten geladen, die Diagramme zeigen**
  (Bundle-Disziplin).
- Die Kit-Regel (`11`, AGENTS Regel 2) bleibt unberührt: Die
  Chart-Bibliothek ist deren dokumentierte Ausnahme für Diagramme —
  sie rendert Canvas-Grafik, keine UI-Bausteine.

## Herkunft

Neues Kapitel (Lücken-Analyse B9) — kein Quell-Regelwerk enthielt
Diagramm-Regeln. Disziplin und Palette-Rollen nach Carbon
(Data-Visualization-Sektion), Typ-Auswahl-Strenge nach Fiori („Choosing
the Correct Chart Type", semantische Muster), ECharts-Token-Theme, KPI-
und Gauge-Muster nach Siemens iX. Bibliotheks- und Typen-Abwägung:
`entscheidungen.md`.
