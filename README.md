# Design-Regelwerk

Generelles, markenneutrales Regelwerk für die Oberflächen unserer Anwendungen.
Es beantwortet die Fragen, die sich in jedem Projekt wiederholen — Aufbau,
Navigation, Tabellen, Formulare, Dialoge, Rückmeldung, Barrierefreiheit —
genau einmal, damit nicht jede Anwendung sie neu entscheidet.

## Zwei Geltungsbereiche, klar getrennt

| Bereich | Für | Stand |
| --- | --- | --- |
| [`web/`](web/) | Web-Anwendungen: Backends, Admin-Oberflächen, Fachanwendungen im Browser | vollständig |
| [`desktop/`](desktop/) | native Desktop-Anwendungen (macOS zuerst) | Gerüst, Ausarbeitung folgt |

Web- und Desktop-Regeln werden nie vermischt: Eine Web-App folgt `web/`,
eine macOS-App `desktop/`. Was beide teilen (Haltung, Statusfarben-Logik,
Sprache), steht in beiden Bereichen ausformuliert — bewusst redundant, damit
jeder Bereich für sich vollständig lesbar ist.

## Marken füllen Slots

Farben, Schrift, Radien, Schatten und Logo-Einsatz sind **bewusst nicht
festgelegt** — sie gehören der Marke, nicht dem Regelwerk. Das Regelwerk
definiert stattdessen:

- **welche Slots** eine Marke füllen muss ([`marke/markenprofil.md`](marke/markenprofil.md)),
- **unter welchen Token-Namen** die Werte bereitstehen
  ([`marke/token-kontrakt.md`](marke/token-kontrakt.md)),
- **wo und wie** die Werte eingesetzt werden (die Kapitel in `web/`).

So teilen alle Anwendungen dieselbe Struktur und Bedienung, während Torro,
Enon & Co. ihr eigenes Gesicht behalten. Die Markenprofile selbst leben bei
der jeweiligen Marke (z. B. `torro-design`, `enon-design`), nicht hier.

## Herkunft

Dieses Regelwerk führt vier gewachsene Regelwerke zusammen. Jedes Kapitel
nennt am Ende, was woher stammt und was neu entschieden wurde
([`entscheidungen.md`](entscheidungen.md) hält die strittigen Abwägungen fest):

| Quelle | Beitrag |
| --- | --- |
| `enon-design` | das Gerüst: App-Shell, Tabellen, Modals, Formulare, Suche, Login, Barrierefreiheits-Prüfung — das ausgereifteste der vier |
| `TorroConnect` (`axis/docs/frontend/`) | die Haltung: Rückmeldepflicht, die drei Fragen jeder Seite, ruhige visuelle Sprache, Tabellen-Fähigkeiten |
| `anlagenmonitor` (`docs/Design-Guide.md`) | Detailmuster: Paginierung, Dialoge mit konstanter Höhe, Icon-Buttons, Formularfeld-Container, Teleport-Aktionen |
| `torro-design` (`app-design.md`) | Desktop-Grundlage und Markenmoment-Denke; fließt vor allem in `desktop/` und in die Marken-Slots ein |

## Verwendung in Projekten

In die `AGENTS.md` / `CLAUDE.md` des Projekts:

```markdown
## Design
Verbindlich ist das Regelwerk `mahype/design-regelwerk`, Bereich `web/`
(Einstieg und harte Regeln: dessen `AGENTS.md`).
Markenwerte: `<marken-repo>/tokens.css` nach `marke/token-kontrakt.md`.
```

Für **neue** Anwendungen gilt dieses Regelwerk. Die bestehenden Regelwerke
(`enon-design`, TorroConnect `axis/docs/frontend/`, anlagenmonitor
`Design-Guide.md`) bleiben für ihre Projekte gültig, bis sie ausdrücklich
hierauf umgestellt werden — dieses Repo ersetzt sie nicht rückwirkend.

## Pflege

- Strittige oder richtungsweisende Entscheidungen landen **datiert** in
  [`entscheidungen.md`](entscheidungen.md) — mit Begründung, nicht nur Ergebnis.
- Die Chronologie ist die **Git-Historie**; Commit-Messages beschreiben, was
  sich fachlich geändert hat. Es gibt bewusst kein separates CHANGELOG.
- Regeln werden geändert, nicht umgangen: Wenn ein Projekt eine Regel nicht
  einhalten kann, wird zuerst hier diskutiert und entschieden.
