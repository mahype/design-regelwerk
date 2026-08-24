# Entscheidungslog

Wo die vier Quell-Regelwerke (Enon, TorroConnect, Anlagenmonitor,
torro-design) einander widersprachen oder eine Frage offen ließen, steht hier
die Abwägung: was übernommen wurde, was nicht, und warum. Neue Einträge werden
datiert angehängt; eine Entscheidung wird durch einen neuen Eintrag revidiert,
nicht durch stilles Umschreiben.

---

## 2026-08-17 — Komponenten-Bibliotheken: keine als Fundament

**Konflikt:** Enon verbietet Komponenten-Frameworks kategorisch (entschieden
2026-07-09); anlagenmonitor (als bestehendes Projekt) setzt Flux UI ein;
Torro Billing hatte shadcn/ui im Projekt (nur Farbtokens genutzt).

**Entscheidung:** Kein Komponenten-Framework und kein UI-Kit als Fundament
neuer Anwendungen — auch kein shadcn/ui. Native HTML-Elemente zuerst
(`<dialog>`, `popover`, `<details>`, native Selects). Headless-Primitives
(Base UI, alternativ React Aria) sind erlaubt, aber nur für Widgets, deren
Interaktions- und Barrierefreiheits-Komplexität den Eigenbau unverantwortlich
macht (Combobox mit Typeahead, Mehrfachauswahl, Datumsbereich).
Logik-Bibliotheken ohne eigene Optik (TanStack Table, Floating UI, Zod,
date-fns) sind erlaubt. Muster-Studium ausdrücklich erwünscht: Verhalten aus
WAI-ARIA APG und guten Bibliotheken übernehmen, Optik nie.

**Warum:** Ein Kit bestimmt die Ästhetik mit („jede shadcn-App sieht gleich
aus") — das Gegenteil von „handcrafted, not generated" (TorroConnect) und vom
Marken-Slot-Modell dieses Regelwerks. Dazu Update-Kopplung und
Abstraktionsschichten über Dingen, die HTML inzwischen selbst kann. Der
Geschmacks-Befund („hat mir bis jetzt nicht gefallen") deckt sich mit der
Enon-Vorentscheidung. Bestehende Projekte mit Framework (anlagenmonitor/Flux)
bleiben unangetastet — die Regel gilt für Neues. Details und
Alternativen-Vergleich: `web/11-komponenten.md`.

---

## 2026-08-17 — Datenlisten: Tabellen im Web, Kartenlisten auf dem Desktop

**Konflikt:** torro-design (macOS): „Listen sind Kartenlisten, Tabellen nur
fürs Log." Enon, TorroConnect und anlagenmonitor: Tabellen sind das Rückgrat,
mit Sortieren/Filtern/Suchen/Paginierung.

**Entscheidung:** Im Web sind homogene Datensätze **immer Tabellen** (mit den
Pflicht-Fähigkeiten aus `web/04-tabellen-und-listen.md`). Karten(-raster)
bleiben heterogenen Vorschau-Objekten vorbehalten — etwa Konten-Kacheln einer
Übersicht. Die Kartenlisten-Regel ist eine **Desktop-Regel** (macOS-Idiom,
schmale 720er-Inhaltsspalte) und wandert nach `desktop/`.

**Warum:** Web-Fachanwendungen arbeiten mit dichten, gleichförmigen
Datenmengen; Scanbarkeit, Spalten-Ausrichtung und tabellarische Ziffern
schlagen dort jede Kartenoptik. Der Widerspruch war keiner — die beiden
Regeln galten für verschiedene Plattformen und werden jetzt auch so geführt.

---

## 2026-08-17 — Formulare: Modal als Standard, Seite ab definierter Schwelle

**Konflikt:** Enon: „Erstellen/Bearbeiten immer im Modal, nie inline."
TorroConnect: „Dialoge unterbrechen — nur einsetzen, wenn nötig."

**Entscheidung:** Das Modal ist der **Standard** für Anlegen und Bearbeiten
von Datensätzen — der Nutzer behält die Liste als Kontext und kehrt exakt
dorthin zurück. Eine **eigene Seite** (oder ein mehrstufiger Ablauf) ist
vorgeschrieben, sobald eines zutrifft: mehr als ~8 Felder trotz fachlicher
Reduktion, mehrere inhaltliche Abschnitte mit Tabs, Datei-Verwaltung mit
Nachbearbeitung, oder der Vorgang muss unterbrechbar/verlinkbar sein.
Bestätigungs-Dialoge bleiben Destruktivem und Unumkehrbarem vorbehalten;
Browser-Dialoge sind verboten.

**Warum:** Beide Quellen haben recht — auf verschiedenen Ebenen.
TorroConnects Satz richtet sich gegen *unterbrechende* Dialoge (Rückfragen,
Bestätigungen), Enons Regel für *Erfassungs*-Modale erhält den Kontext. Ein
scrollendes 20-Felder-Modal wäre trotzdem Miss­brauch — deshalb die explizite
Schwelle, die Enons „lange Formulare fachlich reduzieren" operationalisiert.

---

## 2026-08-17 — Drei Detail-Formen statt Dogma

**Konflikt:** Enon kennt Detail-Modal (Standard) oder Detailseite. Torro
Billing nutzt ein seitliches Detail-Panel neben der Liste; TorroMail
(Desktop) ist ein Master-Detail-Split.

**Entscheidung:** Übersicht + Detail bleibt das Grundmuster (nie mehrere
Entitäten gleichzeitig offen). Für das Detail gibt es drei zulässige Formen:
**(1) Modal** — Standard für CRUD; **(2) Detailseite** — für komplexe,
verlinkbare Entitäten mit eigenen Unterbereichen; **(3) Master-Detail-Split**
(Liste + festes Detail-Panel) — für das *sequenzielle Abarbeiten* vieler
Einträge (Posteingangs-Muster: nächster/vorheriger Eintrag ohne
Kontextwechsel). Die Form wird pro Entität einmal gewählt und dann überall
gleich verwendet.

**Warum:** Das Abarbeits-Szenario (Belege zuordnen, Posteingang durchgehen)
ist real und ein Modal pro Zeile wäre dort spürbar langsamer. Statt die
Praxis heimlich von der Regel abweichen zu lassen, bekommt sie definierte
Kriterien.

---

## 2026-08-17 — Markenmoment: Slot statt Hero-Pflicht oder Hero-Verbot

**Konflikt:** Enon: „Keine Dashboards, Hero-Flächen oder Deko — der Inhalt
ist die Liste." torro-design: „Ein lauter Markenmoment pro Fenster" (Hero)
ist Pflichtbestandteil jeder Torro-App.

**Entscheidung:** Der **Arbeitsbereich gehört dem Inhalt** — dort gibt es
keine Deko-Flächen, das ist generelle Regel. Wo die Marke einmal laut sein
darf, definiert das **Markenprofil** (zulässige Orte: Anmeldeseite,
Erstlauf-/Leerzustand, definierter Kopfbereich) — höchstens ein lauter Ort
pro Sichtbereich, alles andere bleibt ruhig. Eine Marke ohne lauten Moment
(Enon) ist genauso regelkonform wie eine mit (Torro).

**Warum:** Beide Regeln schützen dasselbe: Ruhe im Arbeitsfluss. Ob eine
Marke sich einen Auftritt gönnt, ist Markenidentität, keine
Bedienbarkeitsfrage — also gehört es in den Slot, nicht ins Regelwerk.
Dashboards sind davon getrennt zu bewerten: erlaubt, wenn sie echte
operative Fragen beantworten und in die Arbeit verlinken; verboten als Deko
(`web/02-app-shell.md`).

---

## 2026-08-17 — Suche/Filter: Popover als Standard, Filterleiste als begründete Ausnahme

**Konflikt:** Enon: Suche/Filter **immer** als Lupe-Popover in der
Aktionsleiste, „nie ein dauerhaft sichtbares Suchfeld über der Liste."
Fachanwendungen wie eine Buchungsliste leben aber vom iterativen Filtern
mit vielen Kriterien.

**Entscheidung:** Das Such-Popover (Lupe in der Aktionsleiste, Live-Filterung,
Aktiv-Indikator) ist der Standard. Eine **dauerhafte Filterleiste** über der
Tabelle ist als Ausnahme zulässig, wenn das iterative Filtern die Hauptarbeit
der Seite ist (z. B. Umsatz-/Buchungslisten mit Zeitraum, Betrag, Status,
Volltext) — dann anwendungsweit konsistent, nicht mal so, mal so.
Serverseitig paginierte Listen tragen Suche/Filter in den Query-Parametern
(deep-linkbar, entprellt).

**Warum:** Das Popover hält Seiten ruhig und die Suche an einer festen
Stelle — richtig für die Mehrzahl der Verwaltungslisten. Bei
Dauerfilter-Seiten versteckt es aber den Kern der Arbeit hinter einem Klick
und verliert den Überblick über aktive Kriterien. Die Ausnahme ist eng
definiert, damit sie nicht zum Schlupfloch wird.

---

## 2026-08-17 — Zeilenaktionen neutral grau, Rot erst am Ort der Entscheidung

**Konflikt:** Enon: Löschen-Icon in der Aktionsspalte im Danger-Stil
(rötlich). Anlagenmonitor: „Keine bunten Aktions-Icons" — alle Zeilenaktionen
neutral grau, rote CTAs nur als Footer-Button im Dialog.

**Entscheidung:** Anlagenmonitor-Linie. Zeilenaktionen (Ansehen → Bearbeiten
→ Löschen, feste Reihenfolge) sind neutrale graue Icon-Buttons. Das
Danger-Rot erscheint erst am Bestätigungs-Button des Lösch-Dialogs — dem Ort,
an dem die Entscheidung tatsächlich fällt.

**Warum:** Ein rotes Icon in jeder Zeile lässt die ganze Tabelle Alarm
schreien und stumpft ab („ruhiges Erscheinungsbild"). Die Sicherung gehört an
den Moment des Commitments, nicht an den Einstiegspunkt. Da Löschen ohnehin
immer bestätigt wird, geht keine Sicherheit verloren.

---

## 2026-08-17 — Feldfehler: ein Fehlerblock plus markierte Felder

**Konflikt:** Enon: „Keine Feld-für-Feld-Inline-Fehler, solange der
Fehlerblock reicht." TorroConnect: „highlight invalid fields, validate
early."

**Entscheidung:** Beides, arbeitsteilig: **Eine** Meldung pro Kontext (der
Fehlerblock über den Abschluss-Buttons) trägt den Text; betroffene Felder
werden zusätzlich **markiert** (Fehler-Rahmen + `aria-invalid`), damit das
Auge sie findet. Format-prüfbare Felder validieren beim Verlassen, nicht erst
beim Absenden. Eingaben bleiben bei Fehlern immer erhalten.

**Warum:** Der Block allein lässt den Nutzer bei längeren Formularen suchen;
Feld-Texte allein zersplittern die Meldung. Die Kombination ist Standard
guter Formulare und widerspricht keiner der beiden Quellen im Kern.

---

## 2026-08-17 — Theme-Handling: drei Zustände, `data-theme`, Default System

**Konflikt:** Enon schaltet per `.dark`-Klasse mit Standard Dunkel;
anlagenmonitor bietet Hell/Dunkel/System; Torro Billing folgte nur dem
System ohne Umschalter.

**Entscheidung:** Drei Zustände sind Pflicht: **System** (Default, kein
Attribut gesetzt, `prefers-color-scheme` entscheidet), **explizit hell**,
**explizit dunkel** (`data-theme="light|dark"` auf `<html>`). Umschalter im
Kopfbereich und bereits auf der Anmeldeseite; Wahl wird gespeichert
(localStorage) und überlebt Reload und Login. Eine Marke darf einen anderen
*Default* setzen (Slot), nie die Zustände reduzieren. Das CSS-Muster steht im
Token-Kontrakt.

**Warum:** Nur-System (ohne Umschalter) bevormundet; Nur-Klasse (ohne
System-Default) ignoriert die Betriebssystem-Einstellung beim Erstbesuch.
Das Drei-Zustands-Muster ist die einzige Variante, bei der beides stimmt —
und der un-gestempelte Default-Zustand ist erfahrungsgemäß die häufigste
Fehlerquelle, deshalb ist er im Kontrakt explizit ausformuliert.

---

## 2026-08-17 — Icon-Set: ein Outline-Set pro Anwendung, Default Lucide

**Konflikt:** Enon: ausschließlich Lucide. Anlagenmonitor: Outline-Heroicons.

**Entscheidung:** Die generelle Regel ist: **ein** einziges Outline-Icon-Set
pro Anwendung, einfarbig über `currentColor`, nie gemischt, keine
Filled-/Duotone-/Emoji-Icons. Welches Set, ist ein Marken-Slot mit **Default
Lucide** (größter Umfang, ISC-Lizenz, alle Stacks). Für dieselbe Funktion
überall dasselbe Symbol.

**Warum:** Die Wirkung (Einheitlichkeit, Ruhe, Wiedererkennbarkeit) hängt an
der Konsistenz, nicht am konkreten Set. Anlagenmonitor müsste sonst
grundlos migrieren.

---

## 2026-08-17 — Typografie-Skala: Rollen sind Regelwerk, Werte sind Marke

**Offene Frage:** Wie viel Typografie darf das markenneutrale Regelwerk
festlegen, wenn Schrift und Überschriftgrößen die Marke darstellen?

**Entscheidung:** Das Regelwerk definiert die **Rollen** und ihre
Verhältnisse (kompakte Skala, keine großen Seitentitel, Basis 14 px für
Bedienflächen, 12 px für Sekundäres — mit den Enon-Werten als
Empfehlungs-Default). Die Marke füllt die Slots: Schriftfamilie, exakte
Größen innerhalb der dokumentierten Spannen, Versal-Labels ja/nein.

**Warum:** Lesbarkeit und Dichte sind Bedienbarkeit (Regelwerk), die Stimme
der Schrift ist Marke (Slot). Die Grenze verläuft zwischen Rolle und Wert.

---

## 2026-08-17 — Responsive: drei Größenklassen (768/1280), benutzbar ab 320 px

**Offene Frage:** Keines der vier Quell-Regelwerke enthielt
Responsive-Regeln — es gab weder Breakpoints noch Regeln für Sidebar,
Tabellen oder Modals auf schmalen Screens (Lücken-Analyse A1, Abgleich mit
Fiori, Material 3, Carbon, Polaris, Tailwind, Bootstrap, Siemens iX).

**Entscheidung:** Drei Klassen **schmal < 768 ≤ mittel < 1280 ≤ voll** mit
genau zwei Grenzen (Kontrakt-Konstanten; Tailwind-`md`/`xl`). Anspruch:
**jede Funktion bleibt ab 320 px benutzbar** — optimiert für den Desktop,
mobil ggf. unbequemer, nie abgeschaltet. Sidebar: voll = gespeicherte
Nutzerwahl, mittel = automatisch Icon-Rail (Aufklappen überlagert den
Inhalt), schmal = Drawer über Backdrop. Tabellen: Scroll-Wrapper bleibt die
Grundantwort, ergänzt um **Spalten-Priorität** (sekundäre Spalten entfallen
unter schmal, die Information bleibt im Detail erreichbar; Identitäts- und
Aktionsspalte nie). Formular-/Anzeige-Modals werden unter schmal Vollbild,
Bestätigungs-Modals bleiben klein zentriert. Bausteine brechen per
Container Query (Behälter, nicht Viewport); Touch wird über
`pointer: coarse` erkannt (Ziele ≥ 44 px, Eingabeschrift 16 px).
Details: `web/13-responsive.md`.

**Verworfene Alternativen:** Fiori-Raster 600/1024/1440 (vier Klassen ohne
vierten Verhaltensunterschied; bei 1024–1440 quetscht die volle Sidebar
breite Tabellen). Komplette Tailwind-Skala als offizielle Grenzen (drei
Grenzen ohne Regelbedeutung laden zu Wildwuchs ein). „Kernpfade mobil,
Werkzeuge nur am Desktop" (zwei Funktionsklassen plus Grauzone; der
„am Desktop fortsetzen"-Hinweis wäre genau der Erklärtext, den die Haltung
verbietet — und WCAG-Reflow gilt ohnehin). Karten-Transformation für
Tabellen unter schmal (zweite Darstellung derselben Liste, Spannung zur
Tabellen-Entscheidung, hoher Aufwand je Liste). Nutzerwahl der Sidebar auch
in mittel (vorhersehbarer, aber ein 1024er-Fenster mit voller Sidebar
quetscht genau die Tabellen, für die das Kapitel da ist).

**Warum:** So wenige Grenzen wie möglich, jede mit echtem
Verhaltensunterschied; 768 ist der branchenweite Tablet-Sweet-Spot, 1280
folgt aus der Shell-Arithmetik (256 px Sidebar + Abstände + ~950 px
Tabelle). Die 320-px-Untergrenze spricht nur aus, was WCAG-Reflow ohnehin
verlangt — als Anspruch formuliert statt als Compliance-Fußnote.

---

## 2026-08-17 — Detail-Ansichten: Anatomie von Detailseite und Split

**Offene Frage:** Die Drei-Formen-Entscheidung (oben) ließ offen, wie
Detailseite und Master-Detail-Split konkret aussehen (Lücken-Analyse A2;
Vorbilder: Polaris „Resource details", Fiori Object Page, Material
„List-detail").

**Entscheidung:** (a) Rückweg ist ein **benannter Zurück-Link**
(„← Kontenübersicht") als erste Inhaltszeile — Breadcrumbs bleiben
ausgeschlossen; das Topbar-Suffix trägt auf Detailseiten den
Datensatznamen. (b) **Kein Vor/Zurück-Blättern** auf der Detailseite —
sequenzielle Arbeit ist die Aufgabe des Splits. (c) Bearbeitet wird
**abschnittsweise im Formular-Modal** (Stift je Abschnitts-Karte); es
gibt keinen Seiten-Bearbeiten-Modus und kein Inline-Editieren. (d) Im
Split springt die Auswahl nach einer **erledigenden** Aktion automatisch
zum nächsten offenen Eintrag; bloßes Speichern springt nie. Details:
`web/14-detail-ansichten.md`.

**Verworfene Alternativen:** Breadcrumbs (drittes Navigationssystem neben
Topbar-Kontext und zwei Sidebar-Ebenen — bei Polaris/Carbon Standard, für
unsere flache Hierarchie überdimensioniert). Blätter-Pfeile im
Detail-Kopf à la Polaris (Listen-Kontext müsste mitgeführt werden, tote
Pfeile bei Deep-Links, und die Split-Abgrenzung verwischt). Fioris
Seiten-Bearbeiten-Modus (zweiter Formular-Ort neben dem Modal samt
Ungespeichert-Logik auf Seiten). Inline-Klick-Editieren (dritter Ort,
schwer zugänglich, die Seite verliert ihre Ruhe). Stehenbleiben nach der
Abarbeiten-Aktion (vorhersehbarer, aber der Split existiert für den
Durchsatz — ein Extra-Schritt pro Eintrag summiert sich bei 50 Belegen).

**Warum:** Ein Formular-Ort, ein Rückweg-Muster, scharfe Abgrenzung der
drei Formen — Wiedererkennbarkeit schlägt Einzelfall-Komfort. Der Kopf
der Detailseite bleibt in der kompakten Typo-Skala (weiterhin kein
großer Seitentitel).

---

## 2026-08-17 — Ausnahmezustände: reaktives Anmelde-Modal, Fehlerseiten in der Shell

**Offene Frage:** Sitzungs-Ablauf, Verbindungsverlust, HTTP-Fehlerseiten,
Deploys, Wartung und Berechtigungen waren ungeregelt (Lücken-Analyse A3).
Auffällig: Fast kein Vergleichssystem definiert diese Zustände — nur
Siemens iX (Fehlerseiten-Vorlagen je HTTP-Code), SAP Fiori
(Sitzungs-Warnung, Verstecken-Norm) und GOV.UK (404/500-Muster) liefern
Teile.

**Entscheidung:** (a) **401 → reaktives Anmelde-Modal** über der Seite
(Karte aus `12`); Eingaben bleiben erhalten, schreibende Aktionen werden
nie automatisch wiederholt; keine Vorwarnung per Timer. (b) **Fehlerseiten
403/404/500 in der App-Shell** als Leerzustands-Karte nach dem
`07`-Dreiklang; nackt nur bei Bootstrap-Fehlern. (c) **Neue Version:**
unsichtbarer Reload beim nächsten Seitenwechsel plus dezenter Hinweis für
navigationslose Dauer-Tabs; nie Zwangs-Reload über Eingaben.
(d) **Berechtigungen:** nie Erlaubtes existiert in der UI nicht (403 bei
Direktaufruf); nur vorübergehend Gesperrtes bleibt sichtbar und erklärt
sich; Rechte ändern nie Anordnung oder Form. Verbindungsverlust als
Warn-Banner mit automatischer Wiederholung. Details:
`web/15-ausnahmezustaende.md`.

**Verworfene Alternativen:** Redirect zur Anmeldeseite mit Rücksprung
(wirft offene Eingaben weg — genau der stille Datenverlust, den `06`/`07`
sonst überall verhindern). Vorwarnungs-Timer à la Fiori (lohnt nur bei
kurzen Sitzungen; zusätzlicher Meldungstyp und Timer-Logik). Nackte
Fehlerseiten (werfen den angemeldeten Nutzer aus dem Kontext). Nur-Banner
beim Versionswechsel (alte Versionen stehen tagelang) bzw. Nur-Automatik
(Dauer-Tabs ohne Navigation bekommen nie eine neue Version).
Alles-sichtbar-aber-gesperrt bei Rechten (transparent, aber tote Elemente
gegen die Ruhe-Haltung aus `01`).

**Warum:** Die Rückmeldepflicht endet nicht am Rand des Normalbetriebs.
Alle Muster folgen denselben Grundsätzen: kein stiller Datenverlust,
jeder Zustand erklärt sich am Ort des Geschehens, der nächste Schritt ist
eine Aktion.

---

## 2026-08-17 — Feldtypen-Katalog, Wizard-Anatomie, zweistufiger Upload

**Offene Frage:** `05` regelte die Feld-Anatomie, aber kaum Datentypen;
der Wizard existierte nur als Stichwort; Upload nur als gestylter
Datei-Input (Lücken-Analyse B1–B3; Vorbilder: GOV.UK „Ask users for …",
Fiori Wizard/Upload Set, Polaris Drop zone und Formatregeln, Carbon File
uploader).

**Entscheidung:** (a) Die Datentyp-Regeln stehen als **eigenes Kapitel
`web/16-feldtypen.md`** (Nachschlagewerk, inkl. Upload); `05` bleibt der
Ablauf und erhält die **Wizard-Anatomie** (2–8 Schritte, Schrittanzeige,
Validierung je Schritt, Tat-benennender Abschluss-Button).
(b) **Zusammenfassungs-Schritt „Prüfen & Bestätigen" ab 4 Schritten
Pflicht** — kurze Wizards (2–3 Schritte) schließen direkt ab; geschrieben
wird in beiden Fällen erst am Ende. (c) **Combobox-Schwelle:** natives
Select bis ~15 stabile Optionen; darüber, bei wachsender Menge oder
Suchbedarf die Headless-Combobox. (d) **Upload zweistufig:** beiläufige
Einzeldatei = Datei-Input; wiederkehrender/Mehrfach-Upload = Drop-Zone
mit Sofort-Upload, Fortschritt und Fehlern je Datei-Zeile. Dazu als
Typregeln u. a.: kein `type="number"` für Beträge, `autocomplete`-Pflicht,
kein Kalender-Zwang für erinnerte Daten, Einmal-Codes als ein Feld.

**Verworfene Alternativen:** Alles in `05` (eine sehr lange Datei, die
Ablauf- und Nachschlage-Charakter mischt). Zusammenfassung **immer**
Pflicht (GOV.UK-/Fiori-Linie — bei 2–3 Schritten ein Extra-Klick ohne
Prüf-Mehrwert, weil alles eben erst eingegeben wurde; die Schwelle hält
beide Fälle definiert). Immer Drop-Zone (in kleinen Modals sperrig,
für Nebenfälle überdimensioniert). Combobox-Schwelle ~30 (native Selects
werden ab wenigen Dutzend mühsam, besonders auf Touch) bzw. ganz ohne
Zahl (jede Diskussion beginnt von vorn).

**Warum:** Datentypen sind Nachschlage-Wissen — sie gehören in einen
Katalog, den man je Feld aufschlägt, nicht in den Ablauftext. Die
quantifizierten Schwellen (15 Optionen, 4 Schritte, 2–8 Schritte) folgen
dem Fiori-Prinzip messbarer Regeln: Sie beenden Einzelfall-Debatten und
bleiben trotzdem begründet abweichbar.

---

## 2026-08-17 — Tabellen-Feinschliff und Rückmeldungs-Ausbau (B4–B8)

**Offene Frage:** Kürzungs-Regeln, Export, Live-Aktualisierung,
ungespeicherte Änderungen auf Seiten, Bearbeitungskonflikte und das
Uhrzeit-Format waren ungeregelt (Lücken-Analyse B4–B8).

**Entscheidung:** (a) **Ungespeicherte Änderungen auf Seitenformularen:**
Navigations-Guard + `beforeunload` mit der bekannten Verwerfen-Nachfrage —
dieselbe Mechanik wie im Modal; Autosave/Entwürfe bleiben eine begründete
fachliche Ausnahme. (b) **Export immer in beiden Formaten CSV und XLSX**
(Dropdown unter einem Export-Button), Ort: **eigener Icon-Button in der
Aktionsleiste**, aber nur auf Listen, zu deren Arbeit Export gehört.
Exportiert wird die aktuelle Filterung und Sortierung; CSV mit Semikolon
und UTF-8-BOM, XLSX mit echten Typen. (c) **Auto-Refresh nur für
ausgewiesene Monitoring-Ansichten** — dort Pflicht mit festem Intervall,
Stand-Anzeige, Tab-Pause und störungsfreier Aktualisierung; normale
Listen aktualisieren bei Aktionen und Navigation. (d) **Konflikte:**
Versionsstempel Pflicht, kein last-write-wins; Meldung mit den zwei
benannten Wegen „Aktuelle Version laden" / „Meine Version speichern"
(Letzteres nur wo vertretbar). (e) **Kürzen:** Zahlen nie, Spaltenköpfe
einzeilig mit Ellipsis, Zellwerte einzeilig mit Voll-Wert eine
Interaktion entfernt, `line-clamp` nur für ausgewiesene Textspalten.
(f) **Uhrzeit** `HH:MM` (24 h), lokale Anzeige (Europe/Berlin), UTC
intern. Die Aktionsleisten-Reihenfolge wächst auf: Suche → Aktualisieren
→ Export → Plus → Zahnrad.

**Verworfene Alternativen:** Kontextuelle Speicherleiste à la Polaris
(sehr sichtbar, aber ein zweiter prominenter Speichern-Ort neben dem
Formular-Fuß) und Fiori-Draft als Standard (serverseitige
Entwurfs-Logik je Entität — zu schwer als Grundregel). Nur-CSV
(deutsche Fachbereiche landen praktisch immer in Excel; die
CSV-Stolperfallen sind real) bzw. XLSX nur bei Bedarf — **bewusst gegen
die ursprüngliche Empfehlung**: beide Formate immer, damit die Frage nie
wieder pro Liste diskutiert wird. Export im Zahnrad (versteckt eine
Aktion in einem Einstellungs-Ort) oder im Footer (außerhalb des
Blickfelds, auf kurzen Listen nicht vorhanden). Auto-Refresh für alle
Listen (Dauer-Requests, Konfliktpotenzial, kaum Nutzen) oder nur manuell
(für Monitoring unbrauchbar — jede Anwendung erfände wieder eigene
Automatik).

**Warum:** Alles Verlängerungen bestehender Grundsätze: kein stiller
Datenverlust (Guard, Konflikte), Ruhe und feste Orte (Aktionsleiste,
Intervalle ohne Nutzer-Schalter), messbare Regeln statt
Einzelfall-Debatten (Formate, Kürzungs-Rangfolge).

---

## 2026-08-17 — Diagramme: ECharts als Default, kleiner Typen-Katalog plus Donut/Gauge

**Offene Frage:** Diagramme und Kennzahlen waren komplett ungeregelt
(Lücken-Analyse B9/B10) — inklusive der Spannung zur Kit-Regel: eine
Chart-Bibliothek rendert zwangsläufig eigene Optik.

**Entscheidung:** Neues Kapitel `web/17-diagramme.md`. (a) Ein Diagramm
braucht eine operative Frage; die Werte sind immer zusätzlich als Tabelle
erreichbar (A11y + Monochrom-Regel). (b) Typen-Katalog: Linie/Fläche,
Balken (gestapelt sparsam), **plus Donut (≤ ~5 Segmente) und Gauge** für
Monitoring-Auslastungen; alles Weitere erst nach dokumentiertem Bedarf.
(c) **Apache ECharts als Default-Bibliothek** (Marken-Slot wie das
Icon-Set), Theme zu 100 % aus Tokens, geladen nur auf Diagramm-Seiten —
die dokumentierte Ausnahme zur Kit-Regel. (d) Neue Marken-Slot-Tokens
`--chart-1…6`; Statusfarben nur für echte Schwellen. (e) Neue Typo-Rolle
**Kennzahl-Wert** (24 px/600, `tabular-nums`, nur KPI-Kachel) — die
einzige große Zahl der Skala. Dazu aus B10: **Toasts unten rechts**
(~5 s, höchstens einer, `role="status"`), **Copy-Button** mit Häkchen-
Rückmeldung im Button, **Abstands-Skala als dokumentierte Festwerte**
(4/8/12/16/20/24 — keine Space-Tokens), **Ebenen-Skala** in `02`
(Top-Layer nativ; 10 sticky, 20 Tooltips; `z-index: 9999` verboten).

**Verworfene Alternativen:** Chart.js (kleiner, aber Gauge nur per
Plugin und bei großen Zeitreihen schwächer — für Monitoring der wunde
Punkt); Observable Plot (beste Token-/A11y-Integration, aber ohne
Donut/Gauge — hätte den beschlossenen Katalog wieder beschnitten); freie
Projektwahl mit Token-Pflicht (jede Neuwahl kostet die Diskussion
erneut, Apps sähen im Detail verschieden aus); Eigenbau-SVG zuerst
(Achsen/Tooltips/Resize/A11y selbst bauen ist teuer und fehleranfällig);
streng-kleiner Katalog ohne Donut/Gauge (für Monitoring-Dashboards zu
eng); echte `--space-*`-Tokens (Umbau quer durch alle Kapitel ohne
praktischen Gewinn — Abstände sind kein Marken-Slot).

**Warum:** Ein benannter Default beendet die Bibliotheksfrage genauso,
wie Lucide sie fürs Icon-Set beendet hat; der Typen-Katalog bleibt so
klein, dass jedes Diagramm begründbar ist, und die Tabellen-Pflicht
sichert Zugänglichkeit und Monochrom-Benutzbarkeit unabhängig von der
Canvas-Technik.
