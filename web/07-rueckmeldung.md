# Rückmeldung: Laden, Erfolg, Fehler, Leere

**Der Nutzer wird nie im Unklaren gelassen.** Alles, was im Hintergrund
passiert, ist sichtbar. Nichts schlägt still fehl, kein Spinner lädt sich
tot, kein Klick verpufft. Das ist die wichtigste Einzelregel des Regelwerks.
Zustände außerhalb des Normalbetriebs — Sitzungs-Ablauf,
Verbindungsverlust, Fehlerseiten, neue Version, Wartung, fehlende
Rechte — regelt `15-ausnahmezustaende.md`.

## Jede Aktion reagiert sofort

- **Button mit Hintergrundarbeit:** sofort disabled (50 % Opazität) plus
  Spinner oder Textwechsel („Speichern…"). Nie einen zweiten Klick zulassen,
  nie kommentarlos warten — Doppel-Ausführung ist zusätzlich serverseitig
  (Idempotenz) abgesichert, wo sie Schaden anrichtet.
- **Laden von Listen/Seiten:** Ladezustand zeigen (kurzer Hinweistext oder
  Skeleton der Zielfläche) — nie eine leere Fläche, die wie „fertig, aber
  leer" aussieht. Skeletons nur für ganze Flächen beim Erstladen; danach
  bleibt das Layout stabil (kein Springen bei Aktualisierung).
- **Längere Vorgänge** (Import, Abruf, Prüfung): Fortschritt, wenn messbar
  (Balken, „Datei 2 von 5"); sonst Spinner **mit dem aktuellen Schritt als
  Text** („Verbindung wird geprüft…") — nie ein nackter Spinner für einen
  mehrstufigen Vorgang.
- **Fortlaufend entstehende Inhalte** (KI-Antworten, Logs) werden gestreamt,
  nicht am Ende geliefert — lesen beginnt sofort.

## Kein Spinner ohne Ausgang

Jeder Ladezustand hat genau drei mögliche Enden, jedes sichtbar:

1. **Erfolg** → Ergebnis erscheint; ohne sichtbares Ergebnis eine kurze
   Bestätigung.
2. **Fehler** → Fehlermeldung am Ort der Aktion: was schiefging + was der
   Nutzer tun kann.
3. **Timeout** → Netzwerkaufrufe haben ein Zeitlimit; danach endet der
   Ladezustand als Fehler („Server antwortet nicht"). Ein ewig drehender
   Spinner ist ein Bug.

`try/catch/finally` ist Pflicht: Der Ladezustand endet in `finally`, nie nur
im Erfolgspfad. Kein `console.error` als einzige Reaktion, kein stilles
Verschlucken.

## Erfolg bestätigen

- Der Nutzer darf nie rätseln, ob eine Aktion durchlief. Ergebnis sichtbar
  machen (aktualisierte Liste, neuer Eintrag markiert) — und wo das Ergebnis
  nicht von selbst sichtbar ist, kurz bestätigen („Gespeichert").
- **Zahlen sagen mehr:** Mengen-Vorgänge melden ihr Ergebnis beziffert —
  „4 neu importiert, 2 Dubletten übersprungen" schlägt „Import erfolgreich".
- Erfolgsmeldungen verschwinden von selbst; Fehler bleiben, bis sie behoben
  oder quittiert sind.

## Wo Meldungen erscheinen

- **Am Ort der Aktion** — im Modal über den Abschluss-Buttons, auf Seiten als
  Block unter der Sub-Navigation/Toolbar (Fehler-Variante), am Feld bei
  Feldfehlern. Eine Meldung pro Kontext; die neue ersetzt die alte.
- Hinweisboxen folgen dem Status-Schema (`--status-*-surface` / `-border` /
  Vollton als Text); ein neutraler Hinweis ohne Dringlichkeit ist keine Box,
  sondern schlichter Sekundärtext.
- **Toasts** nur für Erfolge nach Kontextwechsel (das Modal ist schon zu, die
  Seite gewechselt). Fehler erscheinen **nie** ausschließlich als Toast — sie
  müssen stehen bleiben und auffindbar sein.
- Lade- und Fehlerzustände werden für Screenreader angekündigt
  (`aria-live="polite"`, Fehler `role="alert"`).

## Fehlertexte

Was schiefging + was zu tun ist, in Nutzersprache, ohne Schuldzuweisung
(`03-typografie-und-sprache.md`). Technische Details, die beim Melden helfen,
dürfen dazu — eingeklappt oder als Sekundärzeile, nicht als Hauptsatz.

## Leere Zustände

Jede leere Fläche erklärt drei Dinge:

1. **Was hier stünde** („Hier erscheinen deine Umsätze.")
2. **Warum es leer ist** (noch nichts importiert? Filter ohne Treffer?)
3. **Der nächste Schritt als Aktion** („Erste Datei einlesen" /
   „Filter zurücksetzen")

Leere durch Filterung sagt das ausdrücklich — sonst hält der Nutzer den
Bestand für leer.

## Herkunft

Sofort-Reaktion, garantiertes Ende, `finally`-Pflicht, Timeout-Regel,
Meldungs-Orte, Ein-Box-Regel: Enon (`patterns/feedback.md`,
`komponenten/hinweise.md`). Keine toten Klicks/stillen Aktionen, Schritt
statt Spinner, Streaming, Erfolgspflicht, Leerzustands-Anatomie,
Toast-Verhalten: TorroConnect. Layout-Stabilität beim Laden: anlagenmonitor.
Bezifferte Mengen-Rückmeldung: Torro-Billing-Praxis, verallgemeinert.
