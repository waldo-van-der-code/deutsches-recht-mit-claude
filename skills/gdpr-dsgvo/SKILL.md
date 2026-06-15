---
name: legal-audit
description: This skill should be used when the user asks to "check privacy compliance", "audit GDPR", "is this legal", "do we need a DPA", "check our privacy policy", "DSGVO check", "is our site compliant", "update the privacy policy", "cookie consent legal", "Datenschutz", "Impressum", "TTDSG", "data protection", "review our terms", "legal compliance", "privacy audit", or asks any question about GDPR, DSGVO, TTDSG, BDSG, data processor agreements, cookie law, or German data protection law.
version: 1.0.0
---

# /legal-audit — German DSGVO Legal Compliance

German-jurisdiction legal compliance for websites and apps. Covers DSGVO, TTDSG, BDSG, TMG/DDG, and relevant German case law. Grounds all findings in actual statute via the `rechtsinformationen-bund-de` MCP when available (see README for setup).

**Default jurisdiction:** Germany. Supervisory authority defaults to the authority of your Bundesland — see `references/law-reference.md` for a complete list. Also covers Austria (DSG) and Switzerland (nDSG) where noted.

---

## Step 0 — Route

Parse the invocation args or user intent:

| Pattern | Mode |
|---|---|
| `audit <path>` or a file/dir path given | → **Code Audit** |
| `scan <url>` or arg starts with `http` | → **URL Scan** |
| `flow <path>` | → **Data Flow Review** |
| `draft <type>` | → **Document Draft** |
| No args or natural-language legal question | → **Full Suite** (Audit + Scan + Flow → Draft offer) |

Check MCP availability: attempt `semantische_rechtssuche` with query `DSGVO Artikel 6`. If it responds, use it for every law lookup. If unavailable, fall back to built-in knowledge — note which source was used in each finding.

Display:
> **legal-audit** | Mode: [X] | Target: [Y] | Law source: [MCP / built-in knowledge]

---

## Mode A — Code Audit

Scan the codebase for data collection points and classify each finding by severity.

### A1 — Discover

Run these grep patterns (load `references/audit-patterns.md` for the full list):

```bash
# Third-party fetches
grep -r "fetch(\|axios\.\|import.*resend\|import.*nodemailer\|gtag(\|posthog\|supabase\|stripe\|loadStripe" --include="*.ts" --include="*.tsx" --include="*.js" .

# Storage writes
grep -r "document\.cookie\|localStorage\.setItem\|sessionStorage\.setItem\|setCookie\|set-cookie" --include="*.ts" --include="*.tsx" .

# PII field names
grep -rEi "email|phone|iban|firstName|lastName|birthDate|passport|ipAddress" --include="*.ts" --include="*.tsx" .

# Consent gating
grep -r "consent\|cookie_consent\|analytics_storage\|posthog\.init\|opt_out" --include="*.ts" --include="*.tsx" .
```

### A2 — Classify

For each finding, assign severity (🔴 Hoch / 🟡 Mittel / 🟢 Niedrig) and cite the relevant article.

Load `references/law-reference.md` for the full article-to-violation mapping table. Key rules:

- Third-party script fires before consent check → 🔴 Hoch — TTDSG § 25(1), DSGVO Art. 6(1)(a)
- PII collected in form with no Art. 13 disclosure → 🔴 Hoch — DSGVO Art. 13
- Email send handler, processor not in privacy policy → 🔴 Hoch — DSGVO Art. 13, Art. 28
- US processor used, no SCC documented → 🟡 Mittel — DSGVO Art. 46(2)(c)
- Google Fonts loaded from CDN → 🟡 Mittel — LG München I 2022
- localStorage for consent preference only → 🟢 Niedrig — TTDSG § 25(2) exemption
- Analytics gated behind consent → 🟢 Niedrig — compliant

Also check:
- **Impressum:** TMG § 5 / DDG § 5 — name, address, contact, VAT ID required on every commercial site
- **DPO threshold:** BDSG § 38 — mandatory if 20+ employees regularly process personal data

### A3 — Law lookup

For each 🔴 finding, use `semantische_rechtssuche` to pull the exact statutory language. Quote the article verbatim in the finding output.

### A4 — Output format

```
## Code Audit — [project]  |  [date]  |  Law: [MCP/built-in]

🔴 Hoch — [N]
  [file:line] [issue] — [Article] DSGVO / § [X] TTDSG
  Statutory text: "[quoted text from MCP]"
  Fix: [one-line concrete action]

🟡 Mittel — [N]  ...

🟢 Niedrig / Compliant — [N]  ...

Impressum: [found / missing / partial — list gaps]
DPO threshold: [below / above / unclear]

Summary: 🔴 [N] | 🟡 [N] | 🟢 [N] | Remediation effort: [low/medium/high]
```

---

## Mode B — URL Scan

Audit the live site as a user agent sees it.

Use `WebFetch` on the target URL. Extract and analyze:

1. All `<script src="...">` and `<link href="...">` tags — classify first-party vs. third-party domains
2. Cookie consent banner presence — does it block scripts before acceptance?
3. `<form>` elements with PII inputs
4. Presence and reachability of `/impressum`, `/privacy` (or `/datenschutz`), `/robots.txt`
5. Privacy page content — does it name every third-party domain found in step 1?

Load `references/audit-patterns.md` for the third-party domain classification table and consent-banner correctness checklist.

**Gap check:** detected third-party domains vs. domains disclosed in the privacy page. Any detected-but-undisclosed domain is 🔴 Hoch — DSGVO Art. 13.

**Output format:**
```
## URL Scan — [url]  |  [datetime]

Third-party scripts: [table: domain | service | data sent | legal basis | DPA required]
Consent banner: [present/absent | scripts blocked before consent: yes/no | reject-all prominent: yes/no]
Required pages: Impressum [✓/✗] | Privacy [✓/✗] | robots.txt [✓/✗]
Gap — detected but not disclosed: [list — 🔴 for each]
Verdict: [compliant / partially compliant / non-compliant] — [1-paragraph assessment]
```

---

## Mode C — Data Flow Review

Map all PII flows through the system. Produces the Art. 30 VVT (Verzeichnis von Verarbeitungstätigkeiten).

### C1 — Discover flows

Scan for:
- **Ingress:** form handlers, booking creation, auth flows, webhooks
- **Processing:** DB writes, email sends, third-party API forwarding, logging with PII, analytics events with PII params
- **Egress:** external API calls with PII in body, outbound emails, blob/CDN storage

### C2 — Build VVT table

Load `references/data-flow.md` for the full VVT template, DPA status checklist, and cross-border transfer matrix.

For each processing activity, document:
- Processing activity name
- PII categories involved
- Legal basis (Art. 6 DSGVO — consent / contract / legitimate interest)
- Processor name + DPA URL
- Transfer outside EU/EEA? → SCC or adequacy decision
- Retention period

### C3 — DPA checklist

Cross-reference each detected processor against the known DPA URLs in `references/data-flow.md`. Flag processors with no signed DPA as 🔴 Hoch.

---

## Mode D — Document Draft

Generate ready-to-paste German-law DSGVO document sections.

Load `references/document-templates.md` for templates and required disclosure elements per document type.

Supported types:

| Type | Generates |
|---|---|
| `privacy-section <processor>` | Single Art. 13 processor disclosure block (DE + EN) |
| `privacy-page` | Full Datenschutzerklärung skeleton |
| `impressum` | Impressum block per TMG § 5 / DDG § 5 |
| `cookie-banner` | Consent banner text (DE + EN), TTDSG § 25 compliant |
| `vvt` | Blank Art. 30 VVT template |
| `art13-notice` | Art. 13 notice for a specific form or data collection point |
| `dpa-checklist` | Processor DPA to-do list with links |

For each draft, pull required disclosure elements via `semantische_rechtssuche` (query: `Datenschutzerklärung Pflichtangaben Artikel 13`). Verify every required element is present before outputting.

Every draft must include:
1. German version (primary)
2. English version (secondary, marked as translation)
3. Inline comments citing the DSGVO article that requires each element
4. Disclaimer: *Dieser Text wurde mit KI-Unterstützung erstellt und ersetzt keine Rechtsberatung.*

---

## Mode E — Full Suite

Run all four modes sequentially:
1. Code Audit (Mode A)
2. URL Scan (Mode B)
3. Data Flow Review (Mode C)
4. Consolidated Gap Report — all 🔴 findings ranked by remediation effort (quick wins first)
5. Offer: "Draft the privacy policy sections that fix the 🔴 gaps?"

---

## Output Standards

- **Severity:** 🔴 Hoch (likely DPA interest, fix immediately) / 🟡 Mittel (fix before next release) / 🟢 Niedrig or ✅ Compliant
- **Citations:** every finding cites a DSGVO article, TTDSG section, or BDSG section — no unanchored claims
- **Law source note:** if MCP used, state which queries ran; if fallback, note *(Gesetzestext aus Trainingsdaten — verifizieren auf gesetze-im-internet.de)*
- **Disclaimer:** append to every output — *Dieser Bericht dient der technischen Orientierung und ersetzt keine Rechtsberatung.*

---

## Additional Resources

Load these reference files when needed — do not load all at once:

- **`references/law-reference.md`** — DSGVO/TTDSG/BDSG article table, violation-to-article mapping, key German case law (LG München I 2022, Planet49, Schrems II), supervisory authorities per Bundesland
- **`references/audit-patterns.md`** — grep patterns for code audit, third-party domain classification table, consent banner correctness checklist, framework-specific patterns (Next.js, Astro, React)
- **`references/data-flow.md`** — Art. 30 VVT template, known processor DPA URLs, cross-border transfer matrix
- **`references/document-templates.md`** — required disclosure elements per document type, Art. 13 checklist, Impressum checklist, cookie banner template, data subject rights section
