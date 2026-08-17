# Anmeldung

Login-, Registrierungs- und Passwort-Seiten stehen **außerhalb der
App-Shell** (keine Sidebar, keine Topbar) — sind aber optisch **dieselbe
Anwendung**: dieselben Felder, Radien, Abstände und Tokens wie jedes
Formular im Inneren. Kein neutraler Sonderstil „vor der Anmeldung".

## Seite

- Hintergrund `--surface-page`; über die volle Viewport-Höhe zentriert eine
  schmale Spalte: **Logo oben, darunter die Karte** — mehr steht nicht auf
  der Seite.
- Die Anmeldeseite ist einer der zulässigen **Markenmoment-Orte**
  (`marke/markenprofil.md`): Hier darf die Marke lauter auftreten als im
  Arbeitsbereich — ob sie es tut, entscheidet ihr Profil.

## Logo & Karte

- Logo/Wortmarke der Anwendung zentriert **über** der Karte (nicht darin),
  24px Abstand; nie breiter als der Karten-Inhalt, `height: auto`.
- Karte: `--surface-raised`, 1px `--border`, Radius `--radius-surface`,
  `--shadow-raised`; Breite max. **384px**, Padding 24px.
- Kartenkopf: Titel 18px/600 (die einzige Verwendung dieser Größe),
  optional Untertitel 12px sekundär.

## Felder & Button

- Felder exakt nach `05-formulare.md` (Label darüber, 16px Feldabstand,
  Fokus-Ring).
- **Vollbreiter Primär-Button** — die volle Breite ist die einzige
  Abweichung vom Standard-Button. 16px Abstand darüber.
- Beim Anmelden: Button disabled + Ladezustand; der Ladezustand endet
  garantiert (`07-rueckmeldung.md`). Fehlversuche werden serverseitig
  gebremst; die Meldung verrät nicht, ob die E-Mail existiert.

## Fehler & Nebenlinks

- Anmeldefehler als Fehler-Hinweisbox **in der Karte über dem Button** —
  konkret, ohne Schuldzuweisung. Keine Feld-Inline-Fehler, solange der
  Block reicht; Pflichtfelder über `required`.
- „Passwort vergessen?" u. ä. als Fließtext-Link (`--accent-text`,
  unterstrichen) unter Button oder Feld.

## Theme-Umschalter vor der Anmeldung

Der Hell/Dunkel/System-Wechsel muss **schon vor der Anmeldung** möglich sein
— mit demselben Icon-Button wie in der Topbar, **fixiert oben rechts**
(24px Abstand), also in derselben Ecke wie in der App. Wahl in
`localStorage` (vor dem Login gibt es kein Konto) mit demselben Key wie in
der App — die Wahl wandert mit durch den Login.

## Herkunft

Vollständig aus Enon (`patterns/login.md`) — markenneutral übersetzt
(Karte/Maße/Logo-Regel/Toggle-Position). Markenmoment-Öffnung für die
Anmeldeseite: `entscheidungen.md`. Drossel- und Auskunfts-Regel beim Login:
Torro-Billing-Praxis (Sicherheits-Grundhygiene), verallgemeinert.
