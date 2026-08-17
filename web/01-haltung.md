# Haltung

Die Prinzipien hinter allen Einzelregeln. Wenn ein Kapitel eine Frage nicht
beantwortet, wird sie in diesem Geist entschieden — und die Antwort
anschließend dokumentiert.

## Die drei Fragen jeder Seite

Jede Seite beantwortet auf einen Blick:

1. **Was ist das hier?**
2. **Was kann ich tun?**
3. **Was sollte ich als Nächstes tun?**

Ist das nicht in wenigen Sekunden offensichtlich, wird vereinfacht — nicht
erklärt. Ein Hinweistext, der eine unklare Seite erklärt, ist ein
Designfehler mit Untertitel.

## Ruhe: wenig gleichzeitig zeigen

Frustrationsfreiheit entsteht durch Ruhe. Eine Seite zeigt nur, was der
aktuelle Schritt braucht; alles Weitere liegt eine Detailstufe tiefer oder
hinter einer bewussten Aktion. „Alles ist fachlich relevant" ist kein Grund,
alles gleichzeitig zu zeigen. Layouts bleiben **stabil**: nichts springt bei
Interaktion, Tab-Wechsel oder Laden in Größe oder Position.

## Der Inhalt ist die Oberfläche

Der Arbeitsbereich gehört den Daten des Nutzers. Keine Deko-Flächen, keine
Zierbilder, keine Animationen ohne Funktion. Wo die Marke einmal laut sein
darf, bestimmt das Markenprofil (`marke/markenprofil.md`) — höchstens ein
lauter Ort pro Sichtbereich, und nie mitten in der Arbeit.

## Entscheidungen statt Optionen

Das Regelwerk (und jede Oberfläche) trifft Entscheidungen, statt sie als
Einstellungen an den Nutzer durchzureichen. Gleiches gilt für Statusanzeigen:
Sie formulieren die Antwort auf die Frage des Nutzers („Bereit", „Wartet auf
Beleg"), nie den Prozesszustand („Server: running"). Mechanik und
Fehlerdetails gehören ins Log.

## Rückmeldepflicht

Es gibt keine toten Klicks, keine stillen Aktionen, kein unerklärtes Warten.
Jede Interaktion erzeugt sofort sichtbare Rückmeldung; jeder Vorgang hat
einen sichtbaren Ausgang (Erfolg, Fehler oder Timeout). Details:
`07-rueckmeldung.md`. Das ist die wichtigste Einzelregel des Regelwerks.

## Wiedererkennbarkeit

Gleiche Aktion = gleiches Element, gleiche Farbe, gleiche Position — auf
jeder Seite und in jeder Anwendung, die diesem Regelwerk folgt. Für dasselbe
Konzept gibt es nie zwei Darstellungen. Wer eine Anwendung kennt, kann alle
bedienen.

## Monochrom benutzbar

Farbe trägt Bedeutung (Status, Primäraktion), ist aber nie der einzige
Träger: Jeder Status steht auch als Text, jeder Fehler wird benannt, jede
Markierung hat eine zweite Dimension (Position, Icon, Schrift). Eine
Oberfläche, die in Graustufen unbenutzbar wird, ist falsch gebaut.

## Handarbeit

Die Oberfläche soll wirken wie von Hand gebaut, nicht generiert — deshalb
kein Komponenten-Kit als Fundament (`11-komponenten.md`) und keine
Standard-Optik von der Stange. Qualitäts-Vorbilder: Linear, Notion, GitHub,
Apple-Systemanwendungen — als Maßstab für Hierarchie, Abstände und
Verlässlichkeit, nicht als Vorlage zum Kopieren.

## Herkunft

Die drei Fragen, Rückmeldepflicht, Monochrom-Regel, Vorbilder und
„handcrafted, not generated": TorroConnect. Ruhe/wenig gleichzeitig,
Wiedererkennbarkeit: Enon; Layout-Stabilität: anlagenmonitor.
„Entscheidungen statt Optionen" und Nutzersprache-Status: torro-design.
„Inhalt ist die Oberfläche" verbindet Enons Deko-Verbot mit dem
Markenmoment-Slot (Abwägung: `entscheidungen.md`).
