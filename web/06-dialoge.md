# Dialoge & Modals

## Grundsatz

Dialoge unterbrechen — deshalb gibt es genau drei legitime Einsätze:

1. **Formular-Modal** (Standard): Anlegen/Bearbeiten einer Entität
   (`05-formulare.md`).
2. **Bestätigungs-Modal** (klein): destruktive oder unumkehrbare Aktionen —
   und **nur** die. Routine-Aktionen werden nie bestätigt, sie werden
   rückgemeldet (`07-rueckmeldung.md`).
3. **Anzeige-Modal:** einmalig sichtbare Werte (Secrets) oder ein
   Einzel-Detail ohne Bearbeitung.

**Browser-Dialoge (`alert` / `confirm` / `prompt`) sind verboten** — auch
für die kleinste Rückfrage. Alles bekommt ein eigenes Modal im Look der
Anwendung.

## Technik

Natives `<dialog>`, geöffnet mit `showModal()` — nicht selbstgebaut mit
Overlay-Divs. Fokus-Falle, `Esc` und Backdrop sind damit nativ gelöst;
`Esc` und das Schließen-X entsprechen „Abbrechen". Nach dem Schließen kehrt
der Fokus zum auslösenden Element zurück.

## Aussehen

- **Zentriert**; Breite `min(640px, calc(100vw - 2rem))` — klein (Bestätigung)
  `min(480px, …)`, breit (mehrspaltig, Vorschauen) `min(960px, …)`. Höhe max.
  `calc(100vh - 4rem)`, Inhalt scrollt vertikal.
- `--surface-raised`, 1px `--border`, Radius `--radius-surface`,
  `--shadow-overlay`.
- **Backdrop:** `--backdrop` (abgedunkelt) **plus** `backdrop-filter:
  blur(4px)` — bei jedem Modal, auch dem kleinsten. Das Modal steht im
  Vordergrund, der Hintergrund tritt spürbar zurück.

## Aufbau

- **Kopf:** Padding 16px 20px, unten 1px `--border`, sticky beim Scrollen.
  Links Titel (14px semibold, optional Untertitel 12px sekundär), rechts das
  Schließen-X (Icon-Button ohne Rahmen).
- **Body:** Padding 20px — Formular oder Inhalt.
- **Fuß, feste Anordnung:** **links** Destruktives (z. B. „Löschen", nur im
  Bearbeiten-Modus — räumlich weit weg vom Primär-Button), **rechts**
  „Abbrechen" (sekundär) + der eine Primär-Button. Nie mehr als diese drei.

## Verhalten

- **Keine verschachtelten Modals** — höchstens eine Ebene. Braucht ein Modal
  ein zweites, ist der Ablauf falsch geschnitten (Schritte oder eigene Seite).
- **Backdrop-Klick schließt nur, solange nichts eingegeben wurde.** Ein Modal
  mit ungesicherten Eingaben schließt nur explizit (Abbrechen/X) — und fragt
  dann nach, ob die Eingaben verworfen werden sollen.
- Nach Abschluss: Modal schließen, Liste/Kontext aktualisieren, Erfolg
  bestätigen.

## Tabs im Modal & konstante Höhe

- Inhaltliche Gruppen eines Bearbeiten-Modals dürfen Tabs sein; sekundäre
  Tabs nur einblenden, wenn sie Daten haben — dann mit Anzahl-Badge.
- Die Tab-Leiste läuft full-bleed über die Modal-Breite (Trennlinie über
  volle Breite), Tab-Texte bündig zum Inhalt.
- **Ein Dialog behält immer dieselbe Höhe** — auch bei Tab-Wechsel oder als
  Wizard. Das Auge darf nicht springen. Technik: alle Panels in einen
  CSS-Grid-Stapel legen (`col-start-1 row-start-1`) und inaktive per
  `visibility: hidden` ausblenden — nicht per `display: none` (das kollabiert
  die Höhe). Der Container ist so hoch wie das höchste Panel.

## Bestätigungs-Modal

- Klein (480px), Titel + ein bis zwei Sätze — die **Konsequenz** steht im
  Text („Das entfernt nur die Konfiguration. Deine Daten bleiben erhalten.").
- Buttons: „Abbrechen" (sekundär) + der bestätigende Button im
  **Danger-Stil**, der die Tat benennt („Konto löschen" — nie „OK").
- Extrem Unumkehrbares (Datenbank leeren) darf zusätzlich eine
  Tipp-Bestätigung verlangen (Name eintippen) — sparsam einsetzen.

## Popover sind keine Modals

Popover (Suche, Benutzermenü, Rail-Untermenüs) haben keinen Backdrop, liegen
mit `--shadow-overlay` über dem Inhalt und schließen mit `Escape`,
Außenklick und Auslöser-Klick; der Fokus kehrt zum Auslöser zurück.
Positionierung darf eine Logik-Bibliothek übernehmen (`11-komponenten.md`).

## Herkunft

`<dialog>`-Pflicht, Browser-Dialog-Verbot, Maße, Backdrop, Kopf/Body,
Bestätigungs-Modal: Enon (`komponenten/modals.md`). Fuß-Anordnung mit
Destruktiv-links, Tab-Regeln, Anzahl-Badges und die Konstante-Höhe-Technik:
anlagenmonitor (Abschnitt 5). „Nur wenn nötig" + Bestätigung nur destruktiv:
TorroConnect — Zusammenführung in `entscheidungen.md`. Konsequenz-Nennung:
torro-design. Eingaben-Schutz beim Backdrop-Klick: neu (aus der
Rückmeldepflicht abgeleitet — stiller Datenverlust ist die schlimmste
stille Aktion).
