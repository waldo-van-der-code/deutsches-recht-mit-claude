# BFSG Anforderungskatalog — Referenz für bfsg-check

Lade diese Datei nur in Modus A (URL-Scan) und B (Code-Audit). Nicht alles auf einmal laden.

---

## Gesetzlicher Rahmen

| Gesetz | Vollständiger Name | Gültig ab |
|---|---|---|
| BFSG | Barrierefreiheitsstärkungsgesetz | 28. Juni 2025 |
| EU-Richtlinie 2019/882 | European Accessibility Act (EAA) | — (DE-Umsetzung = BFSG) |
| DIN EN 301 549 v3.2.1 | Technische Norm (referenziert WCAG 2.1 AA) | — |

**Kleinstunternehmen-Ausnahme (§ 2 Abs. 4 BFSG):**
- Weniger als 10 Beschäftigte UND
- Jahresumsatz oder Jahresbilanzsumme ≤ 2 Millionen Euro
- → Ausnahme von BFSG-Pflichten (aber: freiwillige Compliance empfohlen)

**Geltungsbereich (§ 1 BFSG):**
- B2C Dienste: E-Commerce, Banking, E-Books, Telekommunikation, Verkehrsdienste
- NICHT: B2B, öffentliche Stellen (eigenes Recht: BITV 2.0), Kleinstunternehmen

---

## WCAG 2.1 AA — E-Commerce Kriterienkatalog

### Wahrnehmbar (Perceivable)

| ID | Kriterium | Anforderung | Häufiger Fehler |
|---|---|---|---|
| 1.1.1 | Nicht-Text-Inhalt | Alt-Text für alle informativen Bilder | `<img>` ohne `alt` oder `alt=""` für informative Bilder |
| 1.2.1 | Nur-Audio / Nur-Video | Transkript oder Audiodeskription | Videos ohne Untertitel |
| 1.2.2 | Untertitel (Aufgezeichnet) | Untertitel für alle Videos | Dekorative Clips ohne Untertitel aber mit Sprache |
| 1.3.1 | Info und Beziehungen | Semantisches HTML | `<div>` statt `<button>`, keine Tabellenheader |
| 1.3.2 | Bedeutungsvolle Reihenfolge | DOM-Reihenfolge = visuelle Reihenfolge | CSS-Reorder ohne ARIA-Anpassung |
| 1.3.3 | Sensorische Eigenschaften | Nicht nur durch Form/Farbe beschreiben | "Klicke den grünen Button" |
| 1.3.4 | Ausrichtung (AA) | Keine Zwangsausrichtung Portrait/Landscape | `orientation: portrait` in CSS |
| 1.3.5 | Eingabezweck (AA) | `autocomplete` auf Formularfeldern | Name/E-Mail-Felder ohne `autocomplete` |
| 1.4.1 | Verwendung von Farbe | Farbe nicht als einzige Info | Fehler nur durch rote Farbe markiert |
| 1.4.3 | Kontrast (Minimum) AA | 4,5:1 Normaltext, 3:1 Großtext | Hellgrauer Text auf weißem Hintergrund |
| 1.4.4 | Textgröße ändern | 200% ohne Scrolling und Inhaltsverlust | `user-scalable=no`, px-Schriftgrößen |
| 1.4.5 | Bilder von Text (AA) | Kein Text als Bild außer Logo | Überschriften als Grafiken |
| 1.4.10 | Reflow (AA) | 320px ohne horizontales Scrolling | Feste Breiten in px |
| 1.4.11 | Nicht-Text-Kontrast (AA) | 3:1 für UI-Komponenten und Grafiken | Hellgrauer Formularrand |
| 1.4.12 | Textabstand (AA) | Keine Inhaltsverluste bei erhöhtem Abstand | `overflow: hidden` bei erhöhtem Line-Height |
| 1.4.13 | Inhalt bei Hover/Focus (AA) | Popups schließbar/hoverable/persistent | Tooltips sofort verschwindend |

### Bedienbar (Operable)

| ID | Kriterium | Anforderung | Häufiger Fehler |
|---|---|---|---|
| 2.1.1 | Tastatur | Alle Funktionen per Tastatur bedienbar | Drag-only Interfaces, Custom JS-Buttons |
| 2.1.2 | Keine Tastaturfalle | Kein eingeschlossener Fokus | Modal ohne Escape-Handler |
| 2.1.4 | Zeichentasten-Kurzbefehle (AA) | Einzeltasten-Shortcuts abschaltbar | Globale Tastaturkürzel ohne Deaktivierung |
| 2.2.1 | Anpassbare Zeitvorgaben | Zeitlimits verlängerbar oder abschaltbar | Session-Timeout ohne Warnung |
| 2.2.2 | Bewegung pausierbar | Animationen/Scrollen pausierbar | Auto-play Slider ohne Pause |
| 2.3.1 | Dreimaliges Aufblitzen | Max 3 Blitze/Sekunde | Flashing Werbebanner |
| 2.4.1 | Blöcke überspringen | Skip-Link zu Hauptinhalt | Kein "Zum Inhalt springen" Link |
| 2.4.2 | Seitentitel | Aussagekräftiger `<title>` | Generischer Titel wie "Startseite" |
| 2.4.3 | Fokusreihenfolge | DOM-Reihenfolge = logische Reihenfolge | `tabindex > 0` Manipulation |
| 2.4.4 | Linkzweck (im Kontext) | Linktext beschreibt Ziel | "hier klicken", "mehr", "weiter" |
| 2.4.5 | Mehrere Wege (AA) | Navigation über Suche oder Sitemap | Nur Breadcrumb-Navigation |
| 2.4.6 | Überschriften und Beschriftungen (AA) | Beschreibende Überschriften | "Abschnitt 1", "Unterpunkt" |
| 2.4.7 | Fokus sichtbar (AA) | Sichtbarer Fokusring | `outline: none` in CSS |
| 2.5.1 | Zeigergesten | Einzelpunkt-Geste als Alternative | Nur Swipe/Multi-Touch |
| 2.5.2 | Zeiger-Abbruch | Aktion auf mouseup, nicht mousedown | Sofortaktion bei mousedown |
| 2.5.3 | Beschriftung im Namen (AA) | Zugänglicher Name enthält sichtbares Label | Button "Bestellen" mit aria-label "buy" |
| 2.5.4 | Bewegungsaktivierung (AA) | Alternative zu Gerätebewegung | Nur Schütteln zum Löschen |

### Verständlich (Understandable)

| ID | Kriterium | Anforderung | Häufiger Fehler |
|---|---|---|---|
| 3.1.1 | Sprache der Seite | `lang` Attribut auf `<html>` | Fehlendes oder falsches `lang` |
| 3.1.2 | Sprache von Teilen (AA) | `lang` auf anderssprachigen Abschnitten | Englische Abschnitte ohne `lang="en"` |
| 3.2.1 | Bei Fokus | Kein unerwarteter Kontextwechsel | Fokus auf `<select>` navigiert direkt |
| 3.2.2 | Bei Eingabe | Kein Auto-Submit | Form submittet bei Eingabe ohne Button |
| 3.2.3 | Konsistente Navigation (AA) | Gleiche Navigation auf allen Seiten | Menüpositionen ändern sich |
| 3.2.4 | Konsistente Erkennung (AA) | Gleiche Funktionen gleich benannt | "Warenkorb" und "Shopping Cart" für gleiche Funktion |
| 3.3.1 | Fehlermeldung | Fehler identifiziert und beschrieben | Feld nur rot markiert, keine Fehlermeldung |
| 3.3.2 | Beschriftungen oder Anweisungen | Labels + Pflichtfeld-Kennzeichnung | Kein Label, nur Placeholder |
| 3.3.3 | Fehlerkorrektur (AA) | Fehler + Korrekturvorschlag | "Ungültige Eingabe" ohne Hinweis |
| 3.3.4 | Fehlervermeidung (AA) | Überprüfbar, korrigierbar, bestätigbar | Keine Checkout-Bestätigung |

### Robust

| ID | Kriterium | Anforderung | Häufiger Fehler |
|---|---|---|---|
| 4.1.1 | Syntaxanalyse | Valides HTML (keine Parsing-Fehler) | Doppelte IDs, nicht geschlossene Tags |
| 4.1.2 | Name, Rolle, Wert | Alle UI-Elemente haben ARIA-Informationen | `<div>` als Button ohne role/aria |
| 4.1.3 | Statusmeldungen (AA) | `aria-live` für dynamische Inhalte | "Artikel hinzugefügt" ohne live-Region |

---

## E-Commerce-spezifische BFSG-Anforderungen

Diese Bereiche werden durch BFSG Anhang, Abschnitt VI explizit für E-Commerce-Dienste gefordert:

| Bereich | Anforderung | Prüfung |
|---|---|---|
| Produktsuche | Suchergebnisse per Tastatur navigierbar | Manuell: Tab durch Suchergebnisse |
| Produktdetails | Alle Produktinfos (Preis, Varianten) per Screenreader lesbar | ARIA-Labels auf Varianten-Selects |
| Warenkorb | Warenkorb-Aktionen (hinzufügen, entfernen, Menge) per Tastatur | `aria-live` für Warenkorb-Updates |
| Checkout | Vollständiger Checkout-Prozess per Tastatur abschließbar | Manuell: Gesamter Ablauf mit Tab |
| Zahlungsformular | `autocomplete` auf Kreditkarten-/Zahlungsfeldern | `autocomplete="cc-number"` etc. |
| Bestellbestätigung | Bestätigungsmeldung per Screenreader angekündigt | `aria-live="polite"` auf Bestätigungsbereich |
| Fehlermeldungen | Formularfehler mit Feldverweis und Korrekturhinweis | `aria-describedby` auf Fehlertext |

---

## Barrierefreiheitserklärung (§ 12 BFSG)

Pflichtangaben:

1. Konformitätsstatus (voll / teilweise / nicht konform)
2. Nicht barrierefreie Inhalte + Begründung (unverhältnismäßige Belastung nach § 3a)
3. Barrierefreie Alternativen (falls vorhanden)
4. Kontaktstelle für Rückmeldungen
5. Beschwerdeverfahren (§ 17 BFSG) — Link auf zuständige Durchsetzungsstelle

---

## Zuständige Behörden und Beschwerdeverfahren (§ 17 BFSG)

Durchsetzungs- und Überwachungsstellen nach Bundesland (Stand 2025):

| Bundesland | Zuständige Stelle |
|---|---|
| Bayern | Bayerisches Staatsministerium für Digitales |
| NRW | Ministerium für Arbeit, Gesundheit und Soziales NRW |
| Berlin | Senatsverwaltung für Digitalisierung |
| Hamburg | Behörde für Arbeit, Soziales, Familie und Integration |
| Bundesebene / alle | Marktüberwachungsbehörden der Länder |

→ Aktuelle Liste und Beschwerde-Links: [BMAS Barrierefreiheit](https://www.bmas.de/DE/Soziales/Teilhabe-und-Inklusion/Barrierefreiheit/) prüfen, da die Zuweisungen sich 2025 noch konkretisieren.

**Sanktionen (§ 23 BFSG):** Bußgeld bis zu **100.000 Euro** pro Verstoß.
