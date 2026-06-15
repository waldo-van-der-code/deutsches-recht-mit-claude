# Data Flow Reference — VVT Template & Processor DPAs

Reference for the legal-audit skill Data Flow Review mode (Mode C). Load this when building the Art. 30 VVT or checking processor DPA status.

---

## Art. 30 VVT Template

Fill one row per processing activity. Art. 30 VVT is required if >250 employees OR processing is non-occasional or involves special category data (Art. 9). For small operators, it's best practice regardless.

```markdown
# Verzeichnis von Verarbeitungstätigkeiten (Art. 30 DSGVO)
Controller: [Name, address]
DPO: [Name / "Not required — below BDSG § 38 threshold"]
Last updated: [date]

| # | Verarbeitungstätigkeit | Zweck | Betroffenengruppe | PII-Kategorien | Rechtsgrundlage | Empfänger / Auftragsverarbeiter | Drittlandtransfer | Löschfrist |
|---|---|---|---|---|---|---|---|---|
| 1 | Kontaktformular | Beantwortung von Anfragen | Interessenten | Name, E-Mail, Nachrichteninhalt | Art. 6(1)(b) DSGVO | E-Mail-Dienstleister (Auftragsverarbeiter) | Ggf. — SCC | 2 Jahre |
| 2 | Website-Analytics | Betrieb und Optimierung der Website | Websitebesucher | IP (anonymisiert), User Agent, Seitenaufrufe | Art. 6(1)(a) DSGVO (Einwilligung) | Google LLC — GA4 (Auftragsverarbeiter) | Ja — SCC | 14 Monate |
| 3 | Nutzeranmeldung / Auth | Zugangsverwaltung | Registrierte Nutzer | E-Mail, Passwort-Hash, Sitzungsdaten | Art. 6(1)(b) DSGVO | Auth-Dienstleister (Auftragsverarbeiter) | Ggf. — SCC | Bis Kontolöschung |
| 4 | Fehlerprotokollierung | Systembetrieb und Sicherheit | Websitebesucher | Fehlermeldungen, ggf. Stack Traces | Art. 6(1)(f) DSGVO (berechtigtes Interesse) | Monitoring-Dienstleister (Auftragsverarbeiter) | Ggf. — SCC | 90 Tage |
| 5 | Server-seitige Infrastruktur-Analytics | Betrieb der Website | Websitebesucher | URL, Referrer, keine Cookies | Art. 6(1)(f) DSGVO (berechtigtes Interesse) | Hosting-Anbieter (Auftragsverarbeiter) | Ggf. — SCC | 30 Tage |
```

---

## Known Processor DPA URLs

Check and confirm each DPA is in place. Verify links still resolve — these are correct as of June 2026.

| Processor | Service | DPA URL | SCC for DE? | Notes |
|---|---|---|---|---|
| Google LLC | GA4, GTM, Gmail, Workspace | business.safety.google/adsprocessorterms/ | Yes (or EU-US DPF) | Auto-accepted when using service; save a copy |
| Stripe Payments Europe Ltd | Payments | stripe.com/legal/dpa | No (IE entity) | EU-based entity — no SCC needed |
| Stripe Inc. (US entity) | Payments | stripe.com/legal/dpa | Yes — SCCs included | Check which entity your account is under |
| Vercel Inc. | Hosting, Analytics | vercel.com/legal/dpa | Yes | Must actively download and sign |
| Supabase Inc. | Database | supabase.com/legal/dpa | Yes (or EU region) | Available in Supabase dashboard → Settings → Legal |
| Resend Inc. | Transactional email | resend.com/legal/dpa | Yes | Download from resend.com/legal/dpa |
| PostHog Inc. (EU cloud) | Session analytics | posthog.com/dpa | No (EU cloud) | EU servers; DPA in PostHog account settings |
| PostHog Inc. (US cloud) | Session analytics | posthog.com/dpa | Yes | Prefer EU cloud to avoid transfer |
| Anthropic PBC | Claude API | anthropic.com/legal/dpa | Yes | Must request separately — not auto-signed |
| SendGrid / Twilio | Transactional email | twilio.com/en-us/legal/privacy | Yes | Check if DPA included or must be requested |
| Sentry Inc. | Error tracking | sentry.io/legal/dpa/ | Yes | Available in Sentry account settings |
| Cloudflare Inc. | CDN, DNS | cloudflare.com/gdpr/subprocessors/ | Yes | DPA in Cloudflare dashboard |
| AWS (Amazon) | Cloud infrastructure | aws.amazon.com/compliance/gdpr-center/ | Yes | DPA auto-accepted via service terms |
| Hetzner Online GmbH | Cloud hosting | hetzner.com/legal/privacy-policy | No (DE entity) | German company — no SCC needed |
| Netlify Inc. | Hosting | netlify.com/gdpr-ccpa/ | Yes | DPA available on request |
| GitHub Inc. | Code hosting | docs.github.com/en/site-policy/privacy-policies/github-data-protection-agreement | Yes | For source code — check if any user PII in repos |
| Mailchimp / Intuit | Email marketing | mailchimp.com/legal/data-processing-addendum/ | Yes | DPA in account settings |

---

## Cross-Border Transfer Matrix

For each US (or non-EU/EEA) processor:

| Processor | Country | Safeguard | SCC Module | Adequacy / DPF? |
|---|---|---|---|---|
| Google LLC | USA | SCCs (EU Commission 2021) | Module 2 (C→P) | Partial — EU-US DPF covers some Google services |
| Vercel Inc. | USA | SCCs | Module 2 | No |
| Resend Inc. | USA | SCCs | Module 2 | No |
| Anthropic PBC | USA | SCCs | Module 2 | No |
| PostHog Inc. EU | Ireland / EU | None needed | — | EU-based |
| Supabase Inc. | USA (EU region avail.) | SCCs or EU region | Module 2 | No |
| Sentry Inc. | USA | SCCs | Module 2 | No |
| Cloudflare Inc. | USA | SCCs | Module 2 | Partial DPF |

**EU-US Data Privacy Framework (DPF):** Adopted July 2023. Check certification at dataprivacyframework.gov/s/participant-search. SCCs remain best practice as belt-and-suspenders even for DPF-certified processors.

---

## Data Flow Discovery Checklist

Use this checklist to ensure no data flow is missed:

### Ingress (data entering the system)
- [ ] Contact forms — what fields? Where do submissions go?
- [ ] Registration/auth forms — email, password, OAuth tokens
- [ ] Payment/checkout forms — card data, billing address
- [ ] File uploads — any PII in filenames or content?
- [ ] Webhook receivers — what data arrives from third parties?
- [ ] API endpoints — any public endpoints accepting PII?

### Processing (what happens to the data)
- [ ] Database writes — which fields are stored? In what tables?
- [ ] Email sends — to whom? What data in the email body?
- [ ] Third-party API forwarding — is PII passed in request bodies?
- [ ] Logging — does `console.log` or error logging capture PII?
- [ ] Analytics events — do tracking calls include PII params (email, name, transaction amounts)?
- [ ] Server-side rendering — is PII interpolated into HTML sent to client?

### Egress (data leaving the system)
- [ ] Outbound emails — who receives them? What do they contain?
- [ ] Webhook dispatches — what data is sent?
- [ ] CDN/Blob storage — is any PII stored in publicly accessible storage?
- [ ] PDF/export generation — what data ends up in downloads?
- [ ] Notification services (Telegram, Slack) — any PII in alert messages?

### Retention
- [ ] Is there a deletion mechanism for user data?
- [ ] Is data deleted after the stated retention period?
- [ ] Are backups included in the retention policy?
- [ ] Does the code implement any TTL or scheduled deletion?
