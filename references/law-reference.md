# Law Reference — DSGVO / TTDSG / BDSG / TMG

German data protection law quick-reference for the legal-audit skill. Load this when classifying violations or drafting legal text.

---

## Core DSGVO Articles

| Article | Rule | Plain summary |
|---|---|---|
| Art. 5(1)(a) | Lawfulness, fairness, transparency | Every processing must have a legal basis AND be disclosed |
| Art. 5(1)(b) | Purpose limitation | Collect only for specified, explicit purposes — no repurposing |
| Art. 5(1)(c) | Data minimisation | Collect only what is necessary for the stated purpose |
| Art. 5(1)(e) | Storage limitation | Delete when no longer needed; document retention periods |
| Art. 6(1)(a) | Consent | Valid for analytics, marketing — freely given, specific, informed, unambiguous; pre-ticked = invalid |
| Art. 6(1)(b) | Contract | Processing necessary to perform a contract with the data subject |
| Art. 6(1)(c) | Legal obligation | Required by law (e.g. tax retention per AO § 147) |
| Art. 6(1)(f) | Legitimate interest | Must pass 3-step LIA: legitimate interest → necessary → not overridden by subject rights |
| Art. 7 | Consent conditions | Must be withdrawable at any time, as easy to withdraw as to give |
| Art. 12–14 | Transparency | Inform data subjects at point of collection (Art. 13) or within 1 month if not collected directly (Art. 14) |
| Art. 13 | Art. 13 notice | Required at collection: controller identity, DPA contact, purpose, legal basis, retention, recipients, rights, supervisory authority |
| Art. 17 | Right to erasure | Delete on request unless legal retention obligation applies |
| Art. 20 | Data portability | Provide data in machine-readable format on request (applies to consent or contract basis only) |
| Art. 25 | Privacy by design | Default to least data, most privacy — enforceable design obligation |
| Art. 28 | Processor contracts | Written DPA required with every third-party processor; mandatory clauses at Art. 28(3) |
| Art. 30 | Records of processing | VVT required if >250 employees OR processing is non-occasional or involves sensitive/criminal data |
| Art. 32 | Security | Appropriate TOMs — encryption, pseudonymisation, access controls |
| Art. 33 | Breach notification | Notify supervisory authority within 72 hours of becoming aware of a breach |
| Art. 37 | DPO appointment | Required for public bodies, large-scale systematic monitoring, or large-scale sensitive data processing |
| Art. 46(2)(c) | SCCs | Standard Contractual Clauses required for transfers to non-adequate countries (incl. USA post-Schrems II) |
| Art. 83(4) | Fines tier 1 | Up to €10M or 2% global turnover — Art. 28, Art. 30, Art. 37 |
| Art. 83(5) | Fines tier 2 | Up to €20M or 4% global turnover — Art. 5, Art. 6, Art. 7, Art. 13 |

---

## TTDSG (Telekommunikation-Telemedien-Datenschutz-Gesetz)

| Section | Rule | Summary |
|---|---|---|
| § 25(1) | Consent for device access | Any storage in or access to user's device requires consent — covers cookies, localStorage, device fingerprinting |
| § 25(2) | Strictly necessary exemption | Exempts: session management cookies, consent preference storage, load balancing, first-party analytics with no cross-site tracking |
| § 25(3) | Relation to DSGVO | TTDSG § 25 applies first; DSGVO applies to the data collected after access is granted |

**Key implication:** TTDSG § 25(1) is stricter than DSGVO Art. 6 — even a legitimate interest under DSGVO still requires consent for the device access step (unless strictly necessary).

---

## BDSG (Bundesdatenschutzgesetz)

| Section | Rule | Summary |
|---|---|---|
| § 26 | Employee data | Permissible if necessary for employment relationship; consent is weakly valid due to power imbalance |
| § 38 | DPO obligation | Mandatory DPO if 20+ employees regularly process personal data using automated means, OR if processing requires DPIA |
| § 43 | Fines | BDSG-specific violations up to €50,000 |

---

## TMG / DDG (Telemediengesetz / Digitale-Dienste-Gesetz)

DDG replaced TMG in 2024 but § 5 requirements are substantively identical.

**Required Impressum fields (TMG § 5 / DDG § 5):**
- Full name or company name + legal form
- Street address (no PO box)
- Contact: email address AND phone or fax number
- VAT ID (if applicable)
- Supervisory authority (if regulated profession)
- Responsible for content per § 55 RStV (if journalistic/editorial content)

---

## Violation-to-Article Mapping

| Scenario | Article | Severity | Fix |
|---|---|---|---|
| Analytics script fires before consent granted | TTDSG § 25(1), Art. 6(1)(a) | 🔴 | Gate behind consent check |
| Third-party script loaded, not disclosed in policy | Art. 13, Art. 5(1)(a) | 🔴 | Add processor section to privacy page |
| Email processor undisclosed (nodemailer, Resend, etc.) | Art. 13, Art. 28 | 🔴 | Add to privacy page + sign DPA |
| No Impressum page | TMG/DDG § 5 | 🔴 | Create /impressum page |
| US processor without SCC mention in policy | Art. 46(2)(c) | 🟡 | Document SCCs in privacy page |
| Google Fonts from CDN, no consent or self-hosting | LG München I 2022, Art. 6(1)(f) | 🟡 | Self-host or document LIA |
| No retention period documented | Art. 5(1)(e) | 🟡 | Add retention periods to VVT and privacy page |
| localStorage for consent preference only | TTDSG § 25(2) | 🟢 | Strictly necessary — no action needed |
| Analytics only after explicit opt-in | Art. 6(1)(a), TTDSG § 25(1) | ✅ | Compliant — document it |

---

## Key German Case Law

| Case | Court | Year | Ruling |
|---|---|---|---|
| Google Fonts (3 O 17493/20) | LG München I | 2022 | Loading Google Fonts from Google CDN = IP transfer to Google = DSGVO violation if no consent. Self-host or block with consent. |
| Planet49 (C-673/17) | CJEU | 2019 | Pre-ticked checkboxes for cookies are not valid consent. Consent must be active. |
| Schrems II (C-311/18) | CJEU | 2020 | EU-US Privacy Shield invalid. Use SCCs + supplementary measures for US transfers. |
| Cookie Banner (I ZR 186/17) | BGH | 2020 | German implementation of Planet49 — active consent required for all non-essential cookies. |

---

## Supervisory Authorities by Bundesland

| Bundesland | Authority | Contact |
|---|---|---|
| Berlin | BlnBDI | mailbox@datenschutz-berlin.de |
| Bavaria | BayLDA | poststelle@lda.bayern.de |
| Hamburg | HmbBfDI | mailbox@datenschutz.hamburg.de |
| Baden-Württemberg | LfDI BW | poststelle@lfdi.bwl.de |
| North Rhine-Westphalia | LDI NRW | poststelle@ldi.nrw.de |
| Hesse | HBDI | poststelle@datenschutz.hessen.de |
| Lower Saxony | LfD Niedersachsen | poststelle@lfd.niedersachsen.de |
| Saxony | SDSuB | saechsdsb@slt.sachsen.de |
| Federal (cross-Länder) | BfDI | poststelle@bfdi.bund.de |
| Austria | DSB | dsb@dsb.gv.at |
| Switzerland | EDÖB/PFPDT | info@edoeb.admin.ch |

---

## Legitimate Interest Assessment (LIA) — 3-step test

Required before relying on Art. 6(1)(f):

1. **Legitimate interest:** Is there a genuine, lawful business reason? (e.g. fraud prevention, security logging, infrastructure analytics)
2. **Necessity:** Is processing necessary for that interest, or could a less invasive method work?
3. **Balancing:** Would the data subject reasonably expect this processing? Does it override their rights?

**Example — Vercel Analytics passes LIA:** (1) understanding traffic patterns is a legitimate business interest, (2) server-side infrastructure logs are the least invasive method — no cookies, no JS, (3) reasonable expectation for any site operator.

**Example — Google Fonts CDN fails LIA** (post-LG München I): self-hosting is equally effective and involves no third-party data transfer — step 2 fails because a less invasive method exists.
