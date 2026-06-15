# Document Templates — DSGVO Drafting Reference

Reference for legal-audit Document Draft mode (Mode D). Load this when drafting privacy sections, Impressum blocks, or consent notices.

---

## Art. 13 Required Disclosure Elements

Every privacy policy section covering a specific processing activity MUST include all of these (Art. 13 DSGVO). Check each before outputting any draft:

- [ ] **Controller identity** — full name + address
- [ ] **DPO contact** (if DPO appointed) — name or title + email
- [ ] **Purpose of processing** — specific, not generic ("send booking confirmation" not "improve services")
- [ ] **Legal basis** — cite the specific Art. 6(1) subparagraph AND explain why it applies
- [ ] **Legitimate interests** (if Art. 6(1)(f)) — briefly describe the LIA conclusion
- [ ] **Recipients** — who receives the data (name the processor, not just "third parties")
- [ ] **Third-country transfers** — if applicable, state the safeguard (SCC, adequacy, etc.)
- [ ] **Retention period** — specific duration, not "as long as necessary"
- [ ] **Data subject rights** — Art. 15 (access), 16 (rectification), 17 (erasure), 18 (restriction), 20 (portability), 21 (objection)
- [ ] **Right to withdraw consent** (if consent basis) — must be withdrawable at any time
- [ ] **Right to lodge complaint** — name the supervisory authority (BlnBDI for Berlin: mailbox@datenschutz-berlin.de)
- [ ] **Whether provision is statutory or contractual** — and consequences of not providing data

---

## Processor Section Template (DE + EN)

Use this for `draft privacy-section <processor>`. Fill in `[PROCESSOR]`, `[SERVICE]`, `[DATA]`, `[PURPOSE]`, `[BASIS]`, `[RETENTION]`, `[DPA_URL]`.

```markdown
### [PROCESSOR] <!-- Art. 13(1)(e) — Empfänger -->

**DE:**
Wir nutzen [SERVICE] von [PROCESSOR]. <!-- Art. 13(1)(a) — Verantwortliche -->
Dabei werden folgende Daten verarbeitet: [DATA]. <!-- Art. 13(1)(c) — Zweck, Art. 13(1)(d) — Rechtsgrundlage -->
Zweck der Verarbeitung: [PURPOSE].
Rechtsgrundlage: Art. 6 Abs. 1 [BUCHSTABE] DSGVO ([BASIS]).
[Wenn Einwilligung:] Die Einwilligung kann jederzeit mit Wirkung für die Zukunft widerrufen werden. <!-- Art. 13(2)(c) -->
[Wenn LI:] Unser berechtigtes Interesse besteht in [INTEREST].
Speicherdauer: [RETENTION]. <!-- Art. 13(2)(a) -->
[PROCESSOR] ist Auftragsverarbeiter gemäß Art. 28 DSGVO. Datenschutzvertrag: [DPA_URL]. <!-- Art. 13(1)(e) -->
[Wenn Drittland:] Die Datenübertragung erfolgt auf Grundlage von Standardvertragsklauseln gemäß Art. 46 Abs. 2 lit. c DSGVO. <!-- Art. 13(1)(f) -->

**EN:**
We use [SERVICE] by [PROCESSOR].
Data processed: [DATA].
Purpose: [PURPOSE].
Legal basis: Art. 6(1)[letter] GDPR ([BASIS]).
[If consent:] Consent may be withdrawn at any time with effect for the future.
[If LI:] Our legitimate interest consists of [INTEREST].
Retention: [RETENTION].
[PROCESSOR] acts as a data processor under Art. 28 GDPR. Data processing agreement: [DPA_URL].
[If transfer:] Data is transferred on the basis of Standard Contractual Clauses (Art. 46(2)(c) GDPR).
```

---

## Impressum Checklist (TMG § 5 / DDG § 5)

Every commercial website must display an Impressum reachable within one click from every page.

Required fields:

```markdown
# Impressum

<!-- TMG § 5 Abs. 1 Nr. 1 — Anbieter -->
[Vorname Nachname] / [Firmenname + Rechtsform]
[Straße Hausnummer]
[PLZ Ort]
Deutschland

<!-- TMG § 5 Abs. 1 Nr. 2 — Kontakt -->
E-Mail: [email]
Telefon: [phone] <!-- or Fax — one of these is required -->

<!-- TMG § 5 Abs. 1 Nr. 3 — Registereintrag (if GmbH/AG/etc.) -->
Registergericht: [Amtsgericht Stadt]
Registernummer: [HRB/HRA XXXXX]

<!-- TMG § 5 Abs. 1 Nr. 4 — USt-IdNr (if applicable) -->
Umsatzsteuer-Identifikationsnummer gemäß § 27a UStG: DE[9 digits]

<!-- TMG § 5 Abs. 1 Nr. 6 — regulated profession (if applicable) -->
Zuständige Aufsichtsbehörde: [name + address]

<!-- § 55 Abs. 2 RStV — responsible for content (if journalistic/editorial) -->
Verantwortlich für den Inhalt nach § 55 Abs. 2 RStV:
[Name, Adresse]

<!-- Zweckentfremdung (for STR apartments in Berlin) -->
Registriernummer gemäß § 5 Abs. 3 ZwVbG BE: [XX/X/AZ/XXXXXX-XX]
```

---

## Cookie Banner Text Template

For `draft cookie-banner`. Must comply with TTDSG § 25(1) and Planet49/BGH requirements.

```markdown
**DE:**

# Datenschutzeinstellungen

Diese Website verwendet Cookies und ähnliche Technologien. Notwendige Cookies sind für den Betrieb der Website erforderlich (TTDSG § 25 Abs. 2). Optionale Cookies nutzen wir nur mit Ihrer Einwilligung (TTDSG § 25 Abs. 1, Art. 6 Abs. 1 lit. a DSGVO).

**Notwendig** (immer aktiv)
Speicherung Ihrer Cookie-Einstellungen, Sitzungsverwaltung.

**Analyse** (optional)
[Service name] für Websitestatistiken — Ihre IP-Adresse wird dabei verarbeitet.

[Alle akzeptieren] [Nur notwendige] [Einstellungen]

Weitere Informationen in unserer [Datenschutzerklärung] und unserem [Impressum].

---

**EN:**

# Privacy Settings

This website uses cookies and similar technologies. Necessary cookies are required to operate the website (TTDSG § 25(2)). Optional cookies are only used with your consent (TTDSG § 25(1), Art. 6(1)(a) GDPR).

**Necessary** (always active)
Storing your cookie preferences, session management.

**Analytics** (optional)
[Service name] for website statistics — your IP address is processed.

[Accept all] [Necessary only] [Settings]

See our [Privacy Policy] and [Imprint] for more information.
```

**Design requirements (do not omit):**
- "Accept all" and "Necessary only" must have identical visual weight
- The X / close button must = "Necessary only" (not = accept)
- "Settings" must allow per-service toggles
- Footer must link back to this banner to allow withdrawal

---

## Art. 13 Notice for Form (inline)

For `draft art13-notice`. Displayed directly at a form that collects PII.

```markdown
**DE:**
Ihre Daten ([DATA_FIELDS]) werden verarbeitet, um [PURPOSE] (Art. 6 Abs. 1 [BUCHSTABE] DSGVO).
[Wenn Einwilligung:] Diese Einwilligung können Sie jederzeit widerrufen.
Weitere Informationen: [Datenschutzerklärung](link).

**EN:**
Your data ([DATA_FIELDS]) is processed to [PURPOSE] (Art. 6(1)[letter] GDPR).
[If consent:] You may withdraw this consent at any time.
More information: [Privacy Policy](link).
```

---

## Data Subject Rights Section Template

Required in every Datenschutzerklärung.

```markdown
## Ihre Rechte / Your Rights

**DE:**
Sie haben das Recht auf:
- **Auskunft** (Art. 15 DSGVO): Kopie Ihrer gespeicherten Daten
- **Berichtigung** (Art. 16 DSGVO): Korrektur unrichtiger Daten
- **Löschung** (Art. 17 DSGVO): Löschung Ihrer Daten, soweit keine gesetzliche Aufbewahrungspflicht besteht
- **Einschränkung** (Art. 18 DSGVO): Einschränkung der Verarbeitung
- **Datenübertragbarkeit** (Art. 20 DSGVO): Übertragung Ihrer Daten in maschinenlesbarem Format (nur bei Einwilligung oder Vertrag)
- **Widerspruch** (Art. 21 DSGVO): Widerspruch gegen Verarbeitung auf Basis berechtigter Interessen
- **Widerruf** (Art. 7 Abs. 3 DSGVO): Widerruf einer erteilten Einwilligung jederzeit mit Wirkung für die Zukunft

Zur Ausübung Ihrer Rechte wenden Sie sich an: [EMAIL]

**Beschwerderecht:** Sie haben das Recht, sich bei der zuständigen Datenschutzaufsichtsbehörde zu beschweren.
Zuständige Behörde: Berliner Beauftragte für Datenschutz und Informationsfreiheit (BlnBDI)
Alt-Moabit 59–61, 10555 Berlin — mailbox@datenschutz-berlin.de

---

**EN:**
You have the right to:
- **Access** (Art. 15 GDPR): receive a copy of your stored data
- **Rectification** (Art. 16 GDPR): correction of inaccurate data
- **Erasure** (Art. 17 GDPR): deletion of your data, subject to legal retention obligations
- **Restriction** (Art. 18 GDPR): restriction of processing
- **Data portability** (Art. 20 GDPR): transfer of your data in machine-readable format (consent or contract basis only)
- **Objection** (Art. 21 GDPR): objection to processing based on legitimate interest
- **Withdrawal** (Art. 7(3) GDPR): withdrawal of consent at any time with future effect

To exercise your rights, contact: [EMAIL]

**Right to complain:** You have the right to lodge a complaint with the competent supervisory authority.
Competent authority: Berliner Beauftragte für Datenschutz und Informationsfreiheit (BlnBDI)
Alt-Moabit 59–61, 10555 Berlin — mailbox@datenschutz-berlin.de
```
