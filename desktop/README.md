# Desktop-Regelwerk

**Status: Gerüst.** Der Web-Bereich ist ausgearbeitet; die
Desktop-Ausarbeitung folgt als eigener Schritt. Bis dahin gilt für
Desktop-Apps direkt `torro-design/app-design.md` (für Torro-Apps) bzw. die
Plattform-Guidelines.

## Geltungsbereich

Native Desktop-Anwendungen, macOS zuerst (SwiftUI/AppKit). **Web-Regeln
gelten hier nicht** — die beiden Bereiche sind bewusst getrennt: Der Desktop
folgt dem Betriebssystem, das Web folgt der App-Shell dieses Regelwerks.

## Was bereits feststeht (aus `torro-design/app-design.md`, zu verallgemeinern)

- **Nativ zuerst.** Systemcontrols, Systemschrift, Systemverhalten — die
  Marke gestaltet das Betriebssystem nicht um, sie setzt Akzente.
- **Entscheidungen statt Optionen; Status in Nutzersprache** — identisch zur
  Web-Haltung.
- **Markenmoment als Slot:** höchstens ein lauter Ort pro Fenster; der Rest
  ruhiges System-Material. Buttons bleiben System-Buttons; die Markenfarbe
  ist Tint, nie Buttonfläche; `borderedProminent` genau einmal pro
  Wizard-Fuß.
- **Statusfarben sind Systemfarben** mit fester Bedeutung (grün/orange/
  rot/grau); Markenfarbe ist nie Status; Destruktives läuft über die
  System-Rolle.
- **Listen sind Kartenlisten** in einer ~720pt-Inhaltsspalte; Tabellen nur
  fürs Log (im Web ist es genau umgekehrt — `entscheidungen.md`).
- **Detail-/Einstellungsseiten als gruppierte System-Formulare**;
  Wizard-Sheets mit festem Rahmen und der Unterscheidung Wizard (Entwurf,
  Abbrechen möglich) vs. Commit-Sheet (schreibt sofort, nur „Fertig").
- **Fehler sind Diagnosen am Ort des Geschehens**, nicht modale Alerts;
  Statuspunkte immer mit Tooltip und Accessibility-Label.

## Geplante Kapitel

1. Haltung (gemeinsam mit Web, plattformspezifisch geschärft)
2. Fenster & Navigation (Split-View, Sidebar, Titelleiste)
3. Inhaltsspalte & Kartensystem
4. Formulare & Sheets (Wizard vs. Commit-Sheet)
5. Rückmeldung & Status
6. Sprache
7. Marken-Slots auf dem Desktop (Markenmoment, Icon-Familie)

Die Ausarbeitung übersetzt `app-design.md` in markenneutrale Regeln + 
Torro-Markenprofil — analog zur Web-Zusammenführung.
