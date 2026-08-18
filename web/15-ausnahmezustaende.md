# Ausnahmezustände & Berechtigungen

`07-rueckmeldung.md` regelt den Fehlerfall einer einzelnen Aktion. Dieses
Kapitel regelt die Zustände **außerhalb des Normalbetriebs**: abgelaufene
Sitzung, verlorene Verbindung, Fehlerseiten, Deploys, Wartung — und was
die Oberfläche zeigt, wenn Rechte fehlen. Der gemeinsame Grundsatz ist
die Rückmeldepflicht: **Kein Zustand bleibt unerklärt, und keiner wirft
Eingaben weg.**

## Sitzungs-Ablauf: das Anmelde-Modal

- Antwortet der Server **401** (Sitzung abgelaufen), erscheint das
  **Anmelde-Modal** über der aktuellen Seite: dieselbe Karte wie auf der
  Anmeldeseite (`12-anmeldung.md`), der Titel nennt den Grund
  („Sitzung abgelaufen — bitte erneut anmelden").
- **Eingaben bleiben erhalten.** Die Seite darunter bleibt stehen, offene
  Formulare und Modals bleiben unangetastet. Nach erfolgreicher Anmeldung
  schließt das Modal und es geht an derselben Stelle weiter: Ladende
  Ansichten werden automatisch neu geladen; eine gescheiterte
  **schreibende** Aktion bleibt eingabebereit und wird vom Nutzer erneut
  ausgelöst — nie automatisch wiederholt.
- Das Modal lässt sich **nicht beiläufig schließen** (kein Schließen-X,
  kein Backdrop-Klick, `Esc` wirkungslos). Der einzige andere Weg ist der
  ausdrückliche Link „Zur Anmeldeseite" — der offene Eingaben verwirft
  und genau das vorher sagt (`06`-Nachfrage).
- **Keine Vorwarnung, kein Timer** — der Mechanismus ist bewusst reaktiv
  (Abwägung: `entscheidungen.md`). Drossel- und Auskunftsregeln im Modal
  wie in `12` (die Meldung verrät nicht, ob die E-Mail existiert).

## Verbindungsverlust

Wie die Trennung erkannt wird (fehlschlagende Aufrufe, `navigator.onLine`,
Heartbeat), entscheidet das Projekt — das Verhalten ist fix:

- **Banner unter der Topbar** im Warn-Schema: „Verbindung unterbrochen —
  es wird erneut verbunden …" (`role="status"`). Kein blockierendes
  Overlay: Lesen geht weiter. Kein bloßer Toast: Der Zustand betrifft
  alles und muss stehen bleiben.
- **Automatische Wiederholung** mit steigendem Abstand; im Banner
  zusätzlich eine manuelle Aktion („Jetzt erneut versuchen").
- Bei Rückkehr: kurze Erfolgsmeldung („Verbindung wiederhergestellt"),
  verschwindet von selbst; sichtbare Daten werden aktualisiert — ohne
  Layout-Sprung (`07`).
- Aktionen während der Trennung scheitern **sichtbar am Ort der Aktion**
  (`07`); nichts wird still verworfen, Eingaben bleiben erhalten.

## Fehlerseiten: 403, 404, 500

Fehlerseiten erscheinen **innerhalb der App-Shell** — Sidebar und Topbar
bleiben stehen, der Inhaltsbereich zeigt eine Leerzustands-Karte nach dem
Dreiklang aus `07` (was ist passiert · warum · nächster Schritt als
Aktion). Nur wenn die Shell selbst nicht laden kann (Bootstrap-Fehler),
steht die Meldung nackt im Stil der Anmeldeseite (Logo + Karte).

| Code | Titel (Vorlage) | Inhalt & Aktion |
| --- | --- | --- |
| **403** | „Kein Zugriff auf diesen Bereich" | Das Konto hat die nötige Berechtigung nicht; die zuständige Stelle wird genannt. Aktion: „Zur Übersicht" |
| **404** | „Diese Seite gibt es nicht mehr" | Datensatz gelöscht, Link veraltet oder vertippt. Aktion: zur passenden Liste („Zur Kontenübersicht") |
| **500** | „Unerwarteter Fehler" | Das lag nicht am Nutzer; erneut versuchen kann helfen. Aktionen: „Erneut versuchen" + Melde-Weg; technische Details eingeklappt (`07`) |

- Ton nach `03`: ohne Jargon, ohne Schuldzuweisung, ohne
  Entschuldigungsfloskeln.
- **Deep-Links auf Gelöschtes** führen zur 404-Variante mit dem Weg zur
  Liste — nie zu einer leeren Detailansicht.
- Ungültige **URL-Parameter** bleiben davon getrennt: Sie fallen weiter
  still auf den ersten gültigen Wert zurück (`02`). Fehlerseiten sind für
  ganze Routen und Objekte da, nicht für Parameter.

## Neue Version nach einem Deploy

- Die Anwendung erkennt eine neue Version (Versionskennung, beim
  Navigieren oder periodisch geprüft — Technik frei).
- **Beim nächsten vollständigen Seitenwechsel lädt sie unsichtbar neu** —
  dort geht nichts verloren, die URL trägt den Zustand (`02`).
- Bleibt ein Tab **lange ohne Navigation** (Monitoring-Ansichten!),
  erscheint ein dezenter Info-Hinweis mit Aktion: „Neue Version
  verfügbar — jetzt neu laden". Er bleibt stehen, bis er genutzt wird.
- **Nie ein Zwangs-Reload** über offenen Eingaben oder ohne Zutun des
  Nutzers außerhalb des Seitenwechsels.

## Geplante Wartung

- **Vorab** ein Info-Banner mit Zeitpunkt und Dauer („Am Samstag von
  6–8 Uhr nicht verfügbar") — angekündigt wird, was erschrecken würde
  (`03`).
- **Währenddessen** eine Wartungsseite im Stil der Anmeldeseite
  (Logo + Karte): was passiert, wann es weitergeht.

## Berechtigungen

- **Nie erlaubt → nicht vorhanden.** Die Navigation zeigt nur Erlaubtes;
  Aktionen, die eine Rolle nie ausführen darf, erscheinen gar nicht. Ein
  direkt aufgerufener Bereich ohne Recht zeigt die 403-Seite.
- **Gerade nicht möglich → sichtbar und erklärt.** Was nur am Zustand
  oder einer fehlenden Voraussetzung scheitert, bleibt sichtbar; beim
  Klick erklärt die Oberfläche, was fehlt — deckt sich mit „Disabled
  sparsam" (`05`, `11`).
- **Rechte ändern nie die Anordnung.** Was sichtbar bleibt, steht bei
  jeder Rolle an derselben Stelle und in derselben Reihenfolge —
  Wiedererkennbarkeit gilt über Rollen hinweg.
- **Rechte ändern nie die Form.** Dieselbe Entität hat für alle Rollen
  dieselbe Detail-Form (`14`); Rollen ohne Bearbeitungsrecht sehen die
  Anzeige ohne Bearbeiten-Aktionen — kein ausgegrautes Formular.

## Herkunft

Neues Kapitel (Lücken-Analyse; Abwägung: `entscheidungen.md`) — kaum ein
Vergleichssystem regelt diese Zustände. Sitzungs-Modal und die
Verstecken-vs.-Erklären-Norm nach SAP-Fiori-Vorbild (reaktiv statt Timer
vereinfacht); Fehlerseiten-Wordings angelehnt an Siemens iX
(Vorlagen je HTTP-Code) und GOV.UK (404-/500-Muster); Versions- und
Wartungs-Muster aus SPA-Betriebspraxis. Alles Weitere ist die
Rückmeldepflicht (`07`) zu Ende gedacht: kein stiller Datenverlust, jeder
Zustand erklärt sich am Ort des Geschehens.
