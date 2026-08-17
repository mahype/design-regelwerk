# Detail-Ansichten: Detailseite & Master-Detail-Split

Übersicht + Detail ist das Grundmuster jeder Fachanwendung; es ist nie
mehr als eine Entität gleichzeitig offen (`entscheidungen.md`). Für das
Detail gibt es drei Formen — **pro Entität wird einmal gewählt**, dann
gilt die Form überall:

| Form | Wann | Geregelt in |
| --- | --- | --- |
| **Modal** (Standard) | Anlegen/Bearbeiten überschaubarer Datensätze; die Liste bleibt als Kontext | `05-formulare.md`, `06-dialoge.md` |
| **Detailseite** | komplexe, verlinkbare Entität mit eigenen Unterbereichen | dieses Kapitel |
| **Master-Detail-Split** | sequenzielles **Abarbeiten** vieler Einträge ohne Kontextwechsel (Posteingangs-Muster) | dieses Kapitel |

Nicht verwechseln: Die **Formularseite** (Erfassen ab der Schwelle aus
`05`) ist ein Formular-Ort, keine Detail-Ansicht. Die Detailseite ist
primär **ruhige Anzeige** — bearbeitet wird in Modals (unten).

## Detailseite

### Kopf

- **Erste Inhaltszeile: der Zurück-Link.** Ein Fließtext-Link
  (`--accent-text`, `11-komponenten.md`) mit führendem Pfeil, der das
  **Ziel benennt**: „← Kontenübersicht", nie bloß „Zurück". Browser-Zurück
  funktioniert zusätzlich (`02-app-shell.md`); Breadcrumbs gibt es
  weiterhin nicht (Abwägung: `entscheidungen.md`).
- **Kopfzeile:** Entitätsname in der bestehenden 14 px/600-Rolle
  (`03` — es gibt auch hier keinen großen Seitentitel), daneben das
  Status-Badge (`04`-Schema). **Rechts die Aktionen:** höchstens ein
  Primär-Button, alles andere sekundär. **Löschen ist ein neutraler
  Sekundär-Button** — das Danger-Rot erscheint erst am Bestätigungs-Button
  des Dialogs (`entscheidungen.md`). **Kein „…"-Überlauf-Menü:** Aktionen
  sind sichtbar, oder es gibt sie nicht.
- **Metazeile** darunter: ID in `--font-mono`, Erstellt/Geändert als
  Sekundärtext (12 px). Technische Details, die selten gebraucht werden,
  gehören hierhin oder in einen eigenen Tab — nicht in die Kopfzeile.
- **Topbar:** Der Titel bleibt der Bereich, das Kontext-Suffix trägt den
  **Datensatznamen** („Konten – Sparkonto Nord", trunkiert). Die
  Detail-Tabs spiegeln sich nicht ins Suffix — der Datensatz ist der
  Kontext.

### Aufbau

- **Unterbereiche als Tabs** der Sub-Navigation (deep-linkbar, `02`);
  der erste Tab zeigt das Wichtigste (Stammdaten/Zusammenfassung).
  Sekundäre Tabs, die leer sein können, folgen den Modal-Tab-Regeln aus
  `06` (nur zeigen, wenn Daten da sind — dann mit Anzahl-Badge).
- **Inhalt in Abschnitts-Karten** (`05`: nichts sitzt nackt auf dem
  Seitengrund). **Anzeige-Felder** folgen der Formular-Anatomie ohne
  Eingaberahmen: Label 12 px sekundär (versal je Marken-Slot) über dem
  Wert 14 px; leere Werte zeigen „—", nie eine Lücke.
- **Verwandte Datensätze** sind normale Tabellen nach `04` — bevorzugt
  **eine Hauptliste pro Tab** (dann gelten Aktionsleiste und Plus der
  Sub-Navigation). Mehrere kleine Listen auf einem Tab: je Karte ein
  Titel und ein Plus-Icon-Button im Kartenkopf.

### Bearbeiten: abschnittsweise im Modal

- Jede Abschnitts-Karte trägt eine **Bearbeiten-Aktion** (Stift-Icon-
  Button im Kartenkopf, `aria-label` + Tooltip). Sie öffnet ein
  Formular-Modal **nur mit den Feldern dieses Abschnitts** — Regeln aus
  `05`/`06` gelten unverändert; nach Erfolg schließt das Modal, die Karte
  aktualisiert sich, der Erfolg wird bestätigt (`07`).
- Es gibt **keinen Seiten-Bearbeiten-Modus** und **kein
  Klick-auf-den-Wert-Editieren** — das Modal bleibt der einzige
  Formular-Ort des Regelwerks (Abwägung: `entscheidungen.md`).
- **Workflow-Aktionen** (Freigeben, Stornieren, Archivieren …) sitzen im
  Kopf; Destruktives und Unumkehrbares bestätigt sich im Dialog (`06`).

### Zurück & Zustand

- Zurück-Link und Browser-Zurück führen zur Liste **im alten Zustand**:
  Filter, Seite und Scrollposition sind wiederhergestellt. Die URL trägt
  den Zustand ohnehin (`02`) — die Wiederherstellung ist Pflicht, kein
  Komfort.
- **Kein Vor/Zurück-Blättern** zwischen Datensätzen auf der Detailseite:
  Wer viele Einträge nacheinander durchgeht, bekommt dafür den Split.

### Responsive

Unter „schmal" (`13-responsive.md`) stapelt der Kopf: Zurück-Link, dann
Name + Badge, dann die Aktionen (umbrechend, volle Breite ist erlaubt);
Karten einspaltig, Tabs scrollen horizontal.

## Master-Detail-Split

### Wann — und wann nicht

Der Split ist die Form fürs **Abarbeiten**: viele gleichartige Einträge,
die nacheinander gesichtet, zugeordnet, freigegeben werden (Posteingang,
Beleg-Zuordnung). Für gelegentliches Nachschlagen ist er die falsche
Form — dort Modal oder Detailseite.

### Anatomie

- Zwei Flächen unter der Sub-Navigation (`02`, zweispaltige
  Arbeitsflächen): **Listen-Spalte links, fest ~360 px**, Detail rechts
  flexibel. Beide scrollen **intern**; die Seite selbst scrollt nicht.
- **Listen-Zeilen sind reduziert:** Titel (14 px) + eine Sekundärzeile
  (12 px, z. B. Betrag/Datum) + Status-Badge. **Keine Aktionsspalte** —
  alle Aktionen wohnen im Detail-Kopf. Die aktive Zeile markiert sich wie
  die aktive Navigation: Fläche `--accent-soft`, Text in Akzent
  (`aria-selected`).
- **Detail-Kopf** wie auf der Detailseite: Name + Status-Badge, rechts
  die Aktionen — der eine Primär-Button ist die Abarbeiten-Aktion.
  Der Inhalt folgt den Detailseiten-Regeln (Anzeige-Felder,
  Abschnitts-Karten, Bearbeiten im Modal).
- **Suche/Filter** laufen wie überall über die Aktionsleiste (`08`) und
  wirken auf die Listen-Spalte; die Trefferzahl bleibt sichtbar.
  **Erledigte Einträge** bleiben in der Liste sichtbar, werden aber
  optisch sekundär (Text `--text-secondary`, Badge neutral), bis ein
  Filter sie ausblendet.

### Auswahl, Tastatur, Fluss

- **Die Auswahl lebt in der URL** (deep-linkbar, teilbar). Laden ohne
  Auswahl wählt automatisch den **ersten offenen Eintrag** — kein leerer
  Rechtsbereich als Begrüßung. Ist die Liste leer, füllt der
  Leerzustands-Dreiklang (`07`) die ganze Fläche.
- **`↑`/`↓` wechseln die Auswahl direkt** (Auswahl folgt dem Fokus in der
  Liste); das Detail-Panel ist eine benannte Region
  (`aria-labelledby` auf den Entitätsnamen) und stiehlt beim Wechsel nie
  den Fokus.
- **Auto-Weiter:** Nach einer **erledigenden** Aktion (Zuordnen,
  Freigeben …) springt die Auswahl zum **nächsten offenen Eintrag** in
  Listenreihenfolge; der erledigte Eintrag zeigt seinen neuen Zustand
  sichtbar in der Liste (`07`). **Bloßes Speichern springt nie.** Ist
  nichts mehr offen, bleibt die Auswahl stehen — und der Leerzustand der
  „Offen"-Sicht darf den Abschluss benennen („Alles erledigt").
- Doppel-Ausführung der Abarbeiten-Aktion ist gesperrt wie überall
  (`07`): Button disabled + Ladezustand, bevor gesprungen wird.

### Responsive

Unter „schmal" arbeitet der Split **nacheinander** (`13-responsive.md`):
Die Liste füllt die Fläche, eine Zeile öffnet das Detail als eigene
Ansicht mit Zurück-Link; Auto-Weiter gilt dort genauso — nach der Aktion
zeigt das Detail direkt den nächsten offenen Eintrag.

## Herkunft

Drei-Formen-Entscheidung und Split-Kriterien: `entscheidungen.md`
(2026-08-17). Detailseiten-Anatomie orientiert an Polaris' „Resource
details" (Kopf mit Rückweg, Status, Aktionen) und Fioris Object Page
(Abschnitte, Tabs) — reduziert auf unsere Shell; **bewusst gegen**
Polaris' Blätter-Pfeile und Fioris Seiten-Bearbeiten-Modus entschieden
(Abwägung: `entscheidungen.md`). Split-Fluss aus der
Posteingangs-Praxis (TorroMail, torro-design); Responsive-Fallback nach
Material 3 „List-detail". Anzeige-Felder: Enon-Formularanatomie, auf die
Anzeige übertragen.
