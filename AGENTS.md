# Anweisungen für Coding-Agents

Du arbeitest an einer Anwendung, die diesem Regelwerk folgt. Lies vor
UI-Arbeit die für deine Aufgabe relevanten Dateien:

1. `marke/token-kontrakt.md` — immer. Alle Farben/Maße kommen aus den Tokens
   des Markenprofils, nie hartkodieren.
2. `web/01-haltung.md` — immer beim ersten Kontakt mit dem Projekt.
3. Das Kapitel zum Baustein, den du baust oder änderst
   (`web/04-tabellen-und-listen.md` für Tabellen usw.).
4. `web/10-barrierefreiheit.md` — bei jeder UI, spätestens vor Abschluss.

Für Desktop-Apps gilt `desktop/` (siehe dortiges README), nicht `web/`.

## Harte Regeln (Web, nicht verhandelbar)

1. **Nur Token-Werte.** Farben, Radien, Schatten, Schrift kommen aus dem
   Token-Kontrakt (`marke/token-kontrakt.md`); fehlt ein Token, wird es im
   Markenprofil ergänzt — nicht im Code erfunden.
2. **Kein Komponenten-Framework, kein UI-Kit.** Kein shadcn/ui, kein
   Mantine/MUI/Chakra, kein Tailwind-UI — auch nicht vorschlagen. Native
   HTML-Elemente zuerst; Headless-Primitives nur für nachweislich komplexe
   Widgets (`web/11-komponenten.md`).
3. **Hell + Dunkel sind Pflicht** — drei Zustände (System als Default,
   explizit hell, explizit dunkel), Umschalter im Kopfbereich, Wahl wird
   gespeichert (`marke/token-kontrakt.md`).
4. **Der Inhalt ist die Oberfläche.** Keine Deko-Flächen im Arbeitsbereich;
   Markenmomente nur an den im Markenprofil definierten Orten, höchstens
   einer pro Sichtbereich (`web/01-haltung.md`).
5. **Datenlisten sind Tabellen** — mit Sortieren, Filtern, Suchen,
   Paginierung und Tastaturbedienung (`web/04-tabellen-und-listen.md`).
6. **Anlegen/Bearbeiten im Modal** (natives `<dialog>`, abgedunkelter und
   geblurrter Backdrop), nie inline neben der Liste. Browser-Dialoge
   (`alert`/`confirm`/`prompt`) sind verboten. Bestätigungs-Dialoge nur für
   Destruktives/Unumkehrbares (`web/05-formulare.md`, `web/06-dialoge.md`).
7. **Niemand bleibt im Unklaren.** Jede Aktion reagiert sofort, jeder
   Ladezustand endet garantiert, jeder Fehler ist sichtbar und nennt den
   nächsten Schritt, Doppel-Ausführung ist verhindert
   (`web/07-rueckmeldung.md`).
8. **Genau ein Primär-Button pro Sichtbereich.** Destruktives ist nie primär
   gestylt und bestätigt sich im Dialog (`web/11-komponenten.md`).
9. **Ein Outline-Icon-Set, einfarbig.** Icons ersetzen nie Beschriftungen;
   icon-only nur an den etablierten Orten und immer mit `aria-label` +
   sichtbarem Tooltip (`web/09-icons-und-bewegung.md`).
10. **WCAG 2.1 AA ist Pflicht, 2.2 AA das Ziel** — automatisiert geprüft
    (axe-core). Sichtbarer Fokus überall, Zeigerhand auf allem Klickbaren
    (`web/10-barrierefreiheit.md`).
11. **Wiedererkennbarkeit.** Gleiche Aktion = gleiches Aussehen an gleicher
    Stelle, in der ganzen Anwendung. Ansichtszustand (aktiver Tab, Filter,
    Seite) ist deep-linkbar in der URL (`web/02-app-shell.md`).
12. **Deutsch, ganze Sätze, Nutzersprache.** Buttons benennen die Aktion,
    Fehler erklären ohne Jargon und ohne Schuldzuweisung
    (`web/03-typografie-und-sprache.md`).

## Bei Widerspruch

Projekt-eigene Anweisungen (AGENTS.md/CLAUDE.md des Projekts) dürfen dieses
Regelwerk bewusst überstimmen — dann steht die Abweichung dort dokumentiert.
Stillschweigende Abweichungen im Code sind Bugs, keine Präzedenzfälle.
