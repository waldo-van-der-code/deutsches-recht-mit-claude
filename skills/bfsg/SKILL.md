---
name: bfsg-check
description: This skill should be used when the user asks about "BFSG", "Barrierefreiheit", "Barrierefreiheitsstärkungsgesetz", "accessibility audit", "WCAG", "barrierefrei", "Web Content Accessibility", "EAA", "European Accessibility Act", "Barrierefreiheitserklärung", "ARIA", "Kontrast prüfen", "Screenreader", "keyboard navigation", "accessible e-commerce", "Beschwerdeverfahren Barrierefreiheit", or any question about whether a German e-commerce site or digital service meets accessibility law requirements.
version: 1.0.0
---

# /bfsg-check — BFSG Barrierefreiheits-Prüfung

Prüft, ob eine Website oder App die Anforderungen des **Barrierefreiheitsstärkungsgesetzes (BFSG)** erfüllt. Das BFSG setzt die EU-Richtlinie 2019/882 (European Accessibility Act) in deutsches Recht um und gilt seit **28. Juni 2025** für B2C-Anbieter digitaler Dienste.

Gesetzesquelle: BFSG via `rechtsinformationen-bund-de` MCP + WCAG 2.1 AA als technischer Standard.

---

## Step 0 — Route & Setup

Parse invocation args or user intent:

| Pattern | Mode |
|---|---|
| `scan <url>` or arg starts with `http` | → **URL-Scan** |
| `audit <path>` or a file/dir path given | → **Code-Audit** |
| `erklaerung` or `statement` | → **Barrierefreiheitserklärung generieren** |
| No args or natural-language question | → **Vollprüfung** (URL-Scan + Code-Audit + Erklärung-Angebot) |

Check MCP availability: attempt `intelligente_rechtssuche` with query `BFSG Barrierefreiheitsstärkungsgesetz`. If it responds, use it for all law lookups. If unavailable, fall back to built-in knowledge — note which source was used.

Prüfe Ausnahmetatbestand (§ 2 Abs. 4 BFSG Kleinstunternehmen):
- Kleinstunternehmen = weniger als 10 Beschäftigte UND Jahresumsatz/Bilanzsumme ≤ 2 Mio. €
- Falls Kleinstunternehmen → flag, aber trotzdem auf freiwillige Compliance prüfen

Display:
> **bfsg-check** | Modus: [X] | Ziel: [Y] | Gesetzesquelle: [MCP / Trainingsdaten]

---

## Modus A — URL-Scan

Ruft die Live-Website ab und prüft automatisch erkennbare Barrierefreiheitsprobleme.

### A1 — Abruf

`WebFetch` auf die Ziel-URL. Extrahiere:
- Vollständiges HTML (Head + Body)
- Alle `<script>` und `<link>` Tags
- Alle `<img>`, `<input>`, `<button>`, `<a>`, `<form>` Elemente
- Sprachattribut `<html lang="...">`
- Seitentitel `<title>`
- Meta-Tags (viewport, description)

### A2 — Automatisch prüfbare Kriterien

Prüfe jeden Punkt und klassifiziere: 🔴 Fehler / 🟡 Warnung / 🟢 Bestanden / ⚪ Nicht prüfbar (manuell erforderlich)

**Wahrnehmbar (Perceivable)**

| Kriterium | WCAG | Prüfung |
|---|---|---|
| Alt-Text für alle `<img>` | 1.1.1 | `<img>` ohne `alt` oder `alt=""` bei informativen Bildern → 🔴 |
| Kein Color-only Info | 1.4.1 | ⚪ (manuelle Prüfung) |
| Kontrast Text | 1.4.3 | ⚪ (CSS fehlt im HTML-Fetch — hinweis geben) |
| Text-Skalierung | 1.4.4 | Meta-viewport mit `user-scalable=no` oder `maximum-scale=1` → 🔴 |
| Reflow (320px) | 1.4.10 | Viewport-Meta vorhanden → 🟢 wenn `width=device-width` gesetzt |
| Nicht-Text-Kontrast UI | 1.4.11 | ⚪ (manuelle Prüfung) |

**Bedienbar (Operable)**

| Kriterium | WCAG | Prüfung |
|---|---|---|
| Seitentitel vorhanden | 2.4.2 | `<title>` leer oder fehlt → 🔴 |
| Skip-Links vorhanden | 2.4.1 | Kein `<a href="#main">` oder ähnlich → 🟡 |
| Sprachauszeichnung | 3.1.1 | `<html lang="">` fehlt oder leer → 🔴 |
| Links mit aussagekräftigem Text | 2.4.4 | `<a>hier klicken</a>`, `<a>mehr</a>`, `<a>weiter</a>` → 🔴 |

**Verständlich (Understandable)**

| Kriterium | WCAG | Prüfung |
|---|---|---|
| Formularfelder mit Labels | 3.3.2 | `<input>` ohne `<label>` oder `aria-label` → 🔴 |
| Fehlermeldungen identifizierbar | 3.3.1 | ⚪ (manuell: Formular absenden mit Fehler) |

**Robust**

| Kriterium | WCAG | Prüfung |
|---|---|---|
| Valides HTML (Basisprüfung) | 4.1.1 | Mehrfach gleiche `id`, nicht geschlossene Tags → 🟡 |
| Name, Rolle, Wert für UI | 4.1.2 | `<div onclick>`, `<span onclick>` ohne `role` und `tabindex` → 🔴 |
| ARIA-Verwendung | 4.1.2 | `aria-hidden="true"` auf fokussierbarem Element → 🔴 |

**E-Commerce-spezifisch (BFSG-relevant)**

| Kriterium | BFSG | Prüfung |
|---|---|---|
| Bestellprozess vollständig tastaturnavigierbar | § 3 i.V.m. Anhang | ⚪ (manuell) |
| Zahlungsformulare zugänglich | § 3 | `<input type="credit-card">` ohne Labels → 🔴 |
| Fehlermeldungen im Checkout identifizierbar | § 3 | ⚪ (manuell) |
| Produktbilder mit Alt-Text | 1.1.1 | alle `<img>` im Produktbereich prüfen |

### A3 — Pflichtdokumente prüfen

- **Barrierefreiheitserklärung** (§ 12 BFSG): Ist `/barrierefreiheit`, `/accessibility` oder eine verlinktes Dokument vorhanden? Fehlt → 🔴
- **Beschwerdeverfahren** (§ 17 BFSG): Muss in der Erklärung verlinkt sein

### A4 — Gesetzestext nachschlagen

Für jeden 🔴 Befund: `gesetz_per_abkuerzung_abrufen("BFSG")` → zitiere den relevanten Paragraphen. Falls MCP nicht verfügbar: *(Gesetzestext aus Trainingsdaten — verifizieren auf gesetze-im-internet.de)*

### A5 — Ausgabeformat

```
## BFSG URL-Scan — [url]  |  [Datum]  |  Gesetzesquelle: [MCP/Trainingsdaten]

Kleinstunternehmen-Ausnahme: [ja / nein / unklar]

🔴 Fehler (Verstoß gegen BFSG/WCAG AA) — [N]
  [WCAG X.X.X] [Problem] — Fundstelle: [HTML-Snippet]
  Rechtsgrundlage: § [X] BFSG i.V.m. WCAG 2.1 AA Erfolgskriterium [X.X.X]
  Behebung: [konkrete Aktion]

🟡 Warnungen (Best Practice / manuell prüfen) — [N]
  ...

🟢 Bestanden — [N]

⚪ Manuelle Prüfung erforderlich — [Liste]

Pflichtdokumente:
  Barrierefreiheitserklärung (§ 12 BFSG): [✓ vorhanden / ✗ fehlt]
  Beschwerdeverfahren (§ 17 BFSG): [✓ / ✗]

Gesamtbewertung: [konform / teilkonform / nicht konform]
Dringlichkeit: [sofort handeln / vor nächstem Release / mittelfristig]

*Dieser Scan prüft automatisch erkennbare Kriterien. Eine vollständige BFSG-Prüfung erfordert manuelle Tests mit Screenreader und Tastaturnavigation.*
```

---

## Modus B — Code-Audit

Scannt die Codebasis auf Barrierefreiheits-Anti-Pattern.

### B1 — Grep-Patterns

Lade `references/bfsg-requirements.md` für die vollständige Musterliste. Kernmuster:

```bash
# Interaktive Elemente ohne Semantik
grep -rE "<div[^>]+onClick|<span[^>]+onClick|<div[^>]+onKeyPress" \
  --include="*.tsx" --include="*.jsx" --include="*.html" .

# Bilder ohne Alt-Text
grep -rE "<img[^>]+>" --include="*.tsx" --include="*.jsx" --include="*.html" . \
  | grep -v "alt="

# Inputs ohne Labels
grep -rE "<input[^>]+(type=\"text\"|type=\"email\"|type=\"tel\"|type=\"password\")[^>]*>" \
  --include="*.tsx" --include="*.jsx" . | grep -vE "aria-label|aria-labelledby"

# Meta-viewport mit user-scalable=no
grep -rE "user-scalable\s*=\s*no|maximum-scale\s*=\s*1" \
  --include="*.tsx" --include="*.jsx" --include="*.html" .

# Farbkontrast-Hardcoding (verdächtige Kombinationen)
grep -rE "color:\s*(#fff|white|#ffffff).*background.*#[0-9a-fA-F]{6}|color:\s*#[0-9a-fA-F]{6}" \
  --include="*.css" --include="*.scss" .

# Fehlende aria-Attribute auf Buttons/Dialogen
grep -rE "<button[^>]*>" --include="*.tsx" --include="*.jsx" . \
  | grep -vE "aria-label|aria-labelledby|>.*</button>"

# autofocus ohne ARIA-Kontext
grep -rE "autofocus|autoFocus" --include="*.tsx" --include="*.jsx" .

# tabindex > 0 (Reihenfolge-Manipulation)
grep -rE "tabindex=\"[1-9]|tabIndex=\{[1-9]" --include="*.tsx" --include="*.jsx" .
```

### B2 — Klassifizieren

Für jeden Befund: Schweregrad (🔴 / 🟡 / 🟢) + WCAG-Erfolgskriterium.

Lade `references/bfsg-requirements.md` für die vollständige Kriterien-Tabelle.

Häufige Muster:

- `<div onClick>` ohne `role="button"` + `tabIndex={0}` → 🔴 — WCAG 4.1.2, 2.1.1
- `<img src="...">` ohne `alt` → 🔴 — WCAG 1.1.1
- `user-scalable=no` im Viewport → 🔴 — WCAG 1.4.4
- `tabIndex > 0` → 🟡 — WCAG 2.4.3 (Fokusreihenfolge gestört)
- `aria-hidden="true"` auf fokussierbarem Element → 🔴 — WCAG 4.1.2
- Hartkodierte kleine Schriftgröße < 16px → 🟡 — WCAG 1.4.4

### B3 — Ausgabeformat

```
## BFSG Code-Audit — [Projekt]  |  [Datum]

🔴 [N] Fehler  |  🟡 [N] Warnungen  |  🟢 [N] Bestanden

🔴 [Datei:Zeile] [Problem]
  WCAG [X.X.X]: [Kriterium]
  Rechtsgrundlage: BFSG § 3 i.V.m. Anhang Abschnitt [X]
  Fix: [konkreter Code-Vorschlag]

...

Gesamteinschätzung: [X] 🔴-Befunde müssen vor dem 28. Juni 2025 behoben sein.
```

---

## Modus C — Barrierefreiheitserklärung generieren

Generiert eine BFSG-konforme Barrierefreiheitserklärung (§ 12 BFSG).

Erforderliche Pflichtangaben (§ 12 Abs. 2 BFSG):
1. Erklärung zum Grad der Konformität (voll konform / teilweise konform / nicht konform)
2. Nicht barrierefreie Inhalte + Begründung
3. Barrierefreie Alternativen (falls vorhanden)
4. Kontaktstelle für Rückmeldungen
5. Beschwerdeverfahren (§ 17 BFSG) — Verlinkung auf zuständige Stelle

```markdown
---
# Barrierefreiheitserklärung

Diese Erklärung gilt für die Website **[URL]** und wurde am **[Datum]** erstellt.

## Konformitätsstatus

Diese Website ist **[voll konform / teilweise konform / nicht konform]** mit dem
Barrierefreiheitsstärkungsgesetz (BFSG) und dem technischen Standard DIN EN 301 549 
(WCAG 2.1 Konformitätsstufe AA).

## Nicht barrierefreie Inhalte

[Liste der bekannten Barrieren]
- **[Bereich]**: [Problem] — Geplante Behebung: [Datum / Begründung für Ausnahme]

## Kontakt und Feedback

Bei Problemen mit der Barrierefreiheit dieser Website wenden Sie sich bitte an:
- **Name/Rolle**: [Ansprechpartner]
- **E-Mail**: [E-Mail-Adresse]
- **Telefon**: [Telefonnummer]
- **Antwortzeit**: Wir bemühen uns, innerhalb von 2 Werktagen zu antworten.

## Beschwerdeverfahren

Wenn Sie mit unserer Antwort nicht zufrieden sind, können Sie sich an die zuständige
Durchsetzungs- und Überwachungsstelle wenden: [Zuständige Behörde + Link, § 17 BFSG]

*Dieser Erklärungstext wurde mit KI-Unterstützung erstellt. 
Kein Ersatz für eine vollständige Barrierefreiheitsprüfung durch Fachleute.*
```

---

## Ausgabestandards

- **Schweregrad:** 🔴 Fehler (Pflichtanforderung verletzt) / 🟡 Warnung (Best Practice) / 🟢 Bestanden / ⚪ Manuell prüfen
- **Zitierweise:** Jeder Befund nennt WCAG-Erfolgskriterium + BFSG-Paragraph
- **Gesetzesquelle:** MCP-Abfrage oder *(Gesetzestext aus Trainingsdaten — verifizieren auf gesetze-im-internet.de)*
- **Haftungsausschluss:** Jeder Bericht endet mit — *Dieser Bericht dient der technischen Orientierung und ersetzt keine Barrierefreiheitsprüfung durch zertifizierte Fachkräfte oder Rechtsberatung.*

---

## Referenzmaterialien

Lade nach Bedarf — nicht alle gleichzeitig:

- **`references/bfsg-requirements.md`** — WCAG 2.1 AA Kriterienkatalog, BFSG-Paragraphen, E-Commerce-Checkliste, zuständige Behörden, Beschwerdeverfahren-Links
