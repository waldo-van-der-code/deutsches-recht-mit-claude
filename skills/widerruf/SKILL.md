---
name: widerruf-check
description: This skill should be used when the user asks about "Widerruf", "consent withdrawal", "Einwilligung widerrufen", "Widerruf-Button", "Opt-out", "Art. 7(3)", "Cookie Widerruf", "Einwilligung zurückziehen", "so einfach wie die Einwilligung", "Ablehnen-Button", "Cookie-Banner Ablehnen", or asks whether consent withdrawal on a website is as easy as giving consent, or wants a UX checklist for consent management in e-commerce.
version: 1.0.0
---

# /widerruf-check — Widerruf-Button & Einwilligungs-Entzug

Prüft, ob die Widerrufsmöglichkeit für erteilte Einwilligungen **genauso einfach** ist wie die Einwilligung selbst — die Kernpflicht aus Art. 7(3) DSGVO.

Gesetzesquellen: DSGVO Art. 7(3), TTDSG § 25 via `rechtsinformationen-bund-de` MCP.

> **Lücke:** Die wichtigsten Umsetzungshinweise stammen aus den **DSK-Orientierungshilfen** (Datenschutzkonferenz) — diese sind nicht im MCP verfügbar. Relevante Dokumente: [DSK Orientierungshilfe Telemedien](https://www.datenschutzkonferenz-online.de/), [DSK Beschluss zu Cookie-Bannern 2022](https://www.datenschutzkonferenz-online.de/). Beim Nachschlagen von Implementierungsdetails darauf hinweisen und empfehlen, diese Dokumente manuell zu prüfen.

---

## Step 0 — Route & Setup

Parse invocation args or user intent:

| Pattern | Modus |
|---|---|
| `scan <url>` oder URL gegeben | → **URL-Scan** (Cookie-Banner + Widerruf-UX) |
| `audit <path>` oder Dateipfad | → **Code-Audit** (Einwilligungs-Implementierung) |
| `checklist` | → **UX-Checkliste ausgeben** (vollständige Prüfliste) |
| Keine Args oder Freitext | → **Vollprüfung** (Scan + Checkliste + Empfehlungen) |

Check MCP availability: attempt `intelligente_rechtssuche` with query `DSGVO Artikel 7 Widerruf Einwilligung`. If it responds, use it for law lookups. If unavailable, fall back to built-in knowledge.

Display:
> **widerruf-check** | Modus: [X] | Ziel: [Y] | Gesetzesquelle: [MCP / Trainingsdaten]

---

## Rechtliche Grundlage

**Art. 7(3) DSGVO — Widerruf der Einwilligung (Kernsatz):**
> *"Die betroffene Person hat das Recht, ihre Einwilligung jederzeit zu widerrufen. Durch den Widerruf der Einwilligung wird die Rechtmäßigkeit der aufgrund der Einwilligung bis zum Widerruf erfolgten Verarbeitung nicht berührt. Die betroffene Person wird vor Abgabe der Einwilligung hiervon in Kenntnis gesetzt. Der Widerruf der Einwilligung muss so einfach wie die Erteilung der Einwilligung sein."*

**TTDSG § 25 — Cookies & Tracking:**
Der gleiche Grundsatz gilt für Einwilligungen zum Setzen/Auslesen von Cookies und Tracking-Technologien. Der Widerruf muss jederzeit möglich und so einfach wie die ursprüngliche Einwilligung sein.

**DSK-Interpretation (nicht im MCP — manuell prüfen):**
- Gleichwertigkeit der Schritte: Widerruf ≤ so viele Klicks wie Einwilligung
- Gleiche visuelle Prominenz: "Alle ablehnen" = gleiche Größe/Farbe wie "Alle akzeptieren"
- Kein Consent-by-Default nach Widerruf
- Widerruf ohne Nachteil (keine Strafe, kein Funktionsentzug als Druckmittel)

---

## Modus A — URL-Scan: Cookie-Banner & Widerruf-UX

Analysiert den Live-Auftritt auf Widerrufbarkeit der Einwilligung.

### A1 — Abruf

`WebFetch` auf die Ziel-URL. Suche nach:
- Cookie-Consent-Banner / CMP (Consent Management Platform)
- "Alle ablehnen" / "Ablehnen" / "Reject all" Button
- "Einstellungen" / "Einstellungen ändern" Link
- "Datenschutz" / "Cookie-Einstellungen" im Footer
- Vorhandensein von `/privacy`, `/datenschutz`, `/cookies` Seiten

Falls vorhanden: auch `/datenschutz` oder `/privacy` abrufen und prüfen, ob die Seite erklärt, WIE man widerruft.

### A2 — Widerruf-Prüfmatrix

Prüfe jeden Punkt und klassifiziere: 🔴 Verstoß / 🟡 Risiko / 🟢 Konform / ⚪ Nicht automatisch prüfbar

**Primärer Widerruf (Cookie-Banner / CMP)**

| Prüfpunkt | Rechtsgrundlage | Klassifizierung |
|---|---|---|
| "Alle ablehnen" Button vorhanden (gleichrangig zu "Alle akzeptieren") | Art. 7(3) DSGVO, TTDSG § 25 | 🔴 falls fehlt |
| "Ablehnen" nicht versteckt in Untermenü, wenn "Akzeptieren" direkt sichtbar | DSK Orientierungshilfe | 🔴 falls asymmetrisch |
| Gleiche oder ähnliche Schriftgröße / Kontrast beider Buttons | BGH I ZR 186/17 "Cookie-Einwilligung" | 🟡 falls visuell deutlich unterschiedlich |
| Kein Pre-Check von optionalen Cookies | Art. 6(1)(a) DSGVO, Planet49 EuGH | 🔴 falls vorausgewählt |
| Banner blockiert Seite bis zur Entscheidung (kein "X" als Einwilligung) | TTDSG § 25 | 🟡 falls X = Akzeptieren |

**Widerruf nach erteilter Einwilligung**

| Prüfpunkt | Rechtsgrundlage | Klassifizierung |
|---|---|---|
| Einstellungen nachträglich änderbar (z.B. über Footer-Link) | Art. 7(3) DSGVO | 🔴 falls nicht möglich |
| Widerruf mit ≤ gleich vielen Klicks wie ursprüngliche Einwilligung | Art. 7(3) DSGVO | 🔴 falls mehr Schritte |
| Widerruf ohne Anmeldung / Konto möglich | Art. 7(3) DSGVO | 🔴 falls Login erforderlich |
| Nach Widerruf: Seite bleibt zugänglich (kein "Pay or OK") ohne legitime Grundlage | Art. 7(3) + ErwGr. 42 DSGVO | 🟡 — komplex, manuell prüfen |

**Information über Widerruf**

| Prüfpunkt | Rechtsgrundlage | Klassifizierung |
|---|---|---|
| Datenschutzseite erklärt, wie man Einwilligung widerruft | Art. 13(2)(c) DSGVO | 🔴 falls fehlt |
| Cookie-Banner informiert über Widerrufsmöglichkeit | Art. 7(3) S.3 DSGVO | 🟡 falls nicht erwähnt |

### A3 — Gesetzestext nachschlagen

Für jeden 🔴 Befund: `intelligente_rechtssuche("DSGVO Artikel 7 Absatz 3 Widerruf")` oder `gesetz_per_abkuerzung_abrufen("DSGVO")` → zitiere Art. 7(3) wörtlich.

Hinweis: DSK-Implementierungsdetails sind nicht im MCP → empfehle manuelle Prüfung der DSK-Orientierungshilfe Telemedien (aktuelle Version unter datenschutzkonferenz-online.de).

### A4 — Ausgabeformat

```
## Widerruf-Check — [url]  |  [Datum]  |  Gesetzesquelle: [MCP/Trainingsdaten]

Cookie-Banner/CMP erkannt: [ja/nein/unklar]

🔴 Verstöße — [N]
  [Prüfpunkt] — Fundstelle: [HTML/Screenshot-Beschreibung]
  Rechtsgrundlage: [Art. X(X) DSGVO / § X TTDSG]
  Wortlaut: "[zitierter Gesetzestext]"
  Behebung: [konkrete Maßnahme]

🟡 Risiken (Best Practice / DSK-Empfehlung) — [N]
  ...

🟢 Konform — [N]

⚪ Manuelle Prüfung erforderlich
  - [Pay-or-OK Modell prüfen]
  - [Widerruf nach Newsletter-Abo testen]
  - [Widerruf in App / Mobile testen]

Gesamtbewertung: [konform / teilkonform / nicht konform]
Bußgeldrisiko: [gering / mittel / hoch] — [Begründung]

DSK-Lücke: Die Umsetzungsdetails dieses Befundes basieren auf DSK-Orientierungshilfen,
die nicht im MCP verfügbar sind. Empfehle Prüfung unter datenschutzkonferenz-online.de.

*Dieser Bericht dient der technischen Orientierung und ersetzt keine Rechtsberatung.*
```

---

## Modus B — Code-Audit: Einwilligungs-Implementierung

Prüft den Code auf korrekte Widerruf-Implementierung.

### B1 — Grep-Patterns

```bash
# Consent-Bibliotheken und CMP-Implementierungen
grep -rE "cookieYes|cookiebot|CookiePro|OneTrust|Usercentrics|consentmanager|klaro|cookieconsent|tarteaucitron" \
  --include="*.ts" --include="*.tsx" --include="*.js" --include="*.html" .

# Einwilligungsstatus-Speicherung
grep -rE "localStorage\.setItem.*consent|sessionStorage.*consent|document\.cookie.*consent" \
  --include="*.ts" --include="*.tsx" --include="*.js" .

# Tracking-Initialisierung (wird sie vor Einwilligung gecheckt?)
grep -rE "gtag\(|posthog\.init|analytics\.init|fbq\(|_paq\." \
  --include="*.ts" --include="*.tsx" --include="*.js" .

# Widerruf-Handler vorhanden?
grep -rE "revokeConsent|withdrawConsent|widerruf|resetConsent|consentRevoke|opt.?out" \
  --include="*.ts" --include="*.tsx" --include="*.js" .

# Pre-checked Checkboxen
grep -rE "<input[^>]+type=\"checkbox\"[^>]+checked" \
  --include="*.tsx" --include="*.jsx" --include="*.html" .
```

### B2 — Klassifizieren

Häufige Muster und ihre Bewertung:

- Tracking-Code außerhalb von Consent-Gate → 🔴 Art. 6(1)(a) DSGVO, TTDSG § 25
- Kein `revokeConsent`-Handler in CMP-Integration → 🔴 Art. 7(3) DSGVO
- `checked` Attribut auf optionalen Marketing-Cookies → 🔴 Planet49 EuGH
- CMP ohne Footer-Link zur Einstellungsänderung → 🔴 Art. 7(3) DSGVO
- Consent im localStorage ohne Ablaufdatum → 🟡 — wann fragt man neu?
- Consent-Status wird nicht an alle Tracking-Scripts übergeben → 🔴

### B3 — Ausgabeformat

```
## Widerruf Code-Audit — [Projekt]  |  [Datum]

CMP/Consent-Bibliothek erkannt: [Name / keine]

🔴 [N] | 🟡 [N] | 🟢 [N]

🔴 [Datei:Zeile] [Problem]
  Rechtsgrundlage: [Artikel]
  Fix: [Konkreter Code-Vorschlag]

DSK-Lücke: Details zur CMP-Konfiguration nach DSK-Standard müssen manuell gegen
die DSK-Orientierungshilfe Telemedien geprüft werden (nicht im MCP verfügbar).
```

---

## Modus C — UX-Checkliste

Gibt die vollständige Widerruf-UX-Checkliste für e-commerce aus — für manuelle Prüfung oder Design-Reviews.

```
## Widerruf-UX-Checkliste (Art. 7(3) DSGVO + TTDSG § 25)

### Schritt-Symmetrie
[ ] "Alle ablehnen" mit gleichem Klickaufwand wie "Alle akzeptieren" erreichbar
[ ] Widerruf der gesamten Einwilligung in ≤ 2 Klicks möglich
[ ] Kein Darkpattern: Widerruf nicht in verstecktem Untermenü begraben

### Visuelle Gleichwertigkeit
[ ] "Ablehnen" Button gleiche Größe wie "Akzeptieren"
[ ] Kein deutlicher Farbunterschied (schwaches Grau für Ablehnen = Darkpattern)
[ ] Beides als primärer Button gestaltet

### Nachträglicher Widerruf
[ ] Footer-Link zu Cookie-Einstellungen vorhanden und sichtbar
[ ] Einstellungen ohne Login änderbar
[ ] CMP-Widget zeigt aktuellen Einwilligungsstatus an
[ ] Änderung wird sofort wirksam (Tracking stoppt nach Widerruf)
[ ] Seite bleibt nach Widerruf vollständig zugänglich

### Information
[ ] Datenschutzerklärung beschreibt Widerrufsmöglichkeit (Art. 13(2)(c))
[ ] Datenschutzerklärung nennt, wie Widerruf konkret funktioniert
[ ] Banner informiert beim ersten Aufruf über Widerrufsmöglichkeit
[ ] Benutzer wurde VOR Einwilligung über Widerrufsmöglichkeit informiert (Art. 7(3) S.3)

### Newsletter / Account-Daten
[ ] Abmeldung aus Newsletter so einfach wie Anmeldung (1 Klick in E-Mail)
[ ] Löschanfrage unter Account-Einstellungen direkt zugänglich
[ ] Kein Aufladen / Abmeldeformular mit unnötigen Feldern

### Nach dem Widerruf
[ ] Tracking-Cookies werden nach Widerruf gelöscht oder nicht mehr gesetzt
[ ] Verarbeitungsstopp für zukünftige Daten (vergangene Verarbeitung bleibt rechtmäßig)
[ ] Bestätigung des Widerrufs wird dem Nutzer angezeigt
```

---

## Ausgabestandards

- **Schweregrad:** 🔴 Verstoß (Bußgeldrisiko) / 🟡 Risiko (Best Practice / DSK-Empfehlung) / 🟢 Konform
- **DSK-Lücke immer dokumentieren:** Wenn ein Befund auf DSK-Orientierungshilfen basiert, explizit vermerken, dass diese nicht im MCP verfügbar sind
- **Gesetzeszitate:** Immer Art. 7(3) DSGVO im Wortlaut zitieren, wenn als Grundlage verwendet
- **Haftungsausschluss:** *Dieser Bericht dient der technischen Orientierung und ersetzt keine Rechtsberatung.*

---

## Referenzmaterialien

- **`references/widerruf-checklist.md`** — Vollständige Rechtsprechungsübersicht (Planet49, BGH Cookie-Banner), DSK-Beschlüsse (nicht im MCP), Bußgeldkatalog, Muster-Widerrufstexte für Datenschutzerklärungen
