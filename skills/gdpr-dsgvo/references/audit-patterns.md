# Audit Patterns — Code Scan & URL Analysis

Reference patterns for the legal-audit skill. Load this during Code Audit (Mode A) and URL Scan (Mode B).

---

## Code Grep Patterns

### Third-party service detection

```bash
# Analytics
grep -r "gtag\|ga4\|G-[A-Z0-9]\{6,\}\|GoogleTagManager\|GTM-\|plausible\|matomo\|fathom\|posthog" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.astro" .

# Email / notification services
grep -r "nodemailer\|createTransport\|sendMail\|import.*resend\|new Resend\|sendgrid\|mailchimp\|brevo\|sendEmail\|Resend(" --include="*.ts" --include="*.tsx" --include="*.js" .

# Payment processors
grep -r "stripe\|loadStripe\|paypal\|klarna\|mollie\|checkout\.com" --include="*.ts" --include="*.tsx" --include="*.js" .

# Booking/CRM systems
grep -r "smoobu\|airtable\|hubspot\|salesforce\|pipedrive\|calendly" --include="*.ts" --include="*.tsx" --include="*.js" .

# Error tracking / monitoring
grep -r "sentry\|Sentry\|datadog\|newrelic\|bugsnag\|rollbar\|logrocket" --include="*.ts" --include="*.tsx" --include="*.js" .

# Database clients
grep -r "import.*supabase\|createClient\|prisma\|drizzle\|mongoose\|firebase" --include="*.ts" --include="*.tsx" --include="*.js" .

# AI / LLM APIs
grep -r "anthropic\|openai\|OpenAI\|Anthropic\|gemini\|groq\|mistral" --include="*.ts" --include="*.tsx" --include="*.js" .

# CDN / media
grep -r "fonts\.googleapis\|fonts\.gstatic\|cloudinary\|imgix\|unpkg\.com\|cdnjs\|jsdelivr" --include="*.ts" --include="*.tsx" --include="*.html" --include="*.astro" .

# External API calls with potential PII
grep -r "fetch(\|axios\." --include="*.ts" --include="*.tsx" --include="*.js" . | grep -v "localhost\|127\.0\.0\|node_modules"
```

### Storage / device access detection

```bash
# Cookie operations
grep -r "document\.cookie\|setCookie\|parseCookies\|Cookies\.set\|nookies\.set\|set-cookie" --include="*.ts" --include="*.tsx" --include="*.js" .

# localStorage / sessionStorage
grep -r "localStorage\.\|sessionStorage\." --include="*.ts" --include="*.tsx" --include="*.js" .

# Identify what's stored
grep -r "localStorage\.setItem\|sessionStorage\.setItem" --include="*.ts" --include="*.tsx" --include="*.js" . -A2
```

### PII field detection

```bash
# Common PII field names in forms and API payloads
grep -rEi "(email|phone|telefon|handy|iban|sepa|firstName|lastName|vorname|nachname|fullName|birthDate|geburtsdatum|address|adresse|passport|personalId|nationalId|ipAddress|userAgent|deviceId)" --include="*.ts" --include="*.tsx" --include="*.js" .

# Form inputs
grep -r 'type="email"\|type="tel"\|name="phone"\|name="email"\|name="iban"' --include="*.tsx" --include="*.astro" --include="*.html" .
```

### Consent gating detection

```bash
# Find consent checks before analytics/tracking
grep -r "consent\|cookie_consent\|localStorage.*consent\|analytics_storage\|ad_storage" --include="*.ts" --include="*.tsx" .

# Find posthog init (should be inside consent gate)
grep -r "posthog\.init\|posthog\.opt_out\|opt_out_capturing" --include="*.ts" --include="*.tsx" .

# Find gtag consent commands
grep -r "gtag.*consent" --include="*.ts" --include="*.tsx" --include="*.js" .

# Google Consent Mode v2 defaults
grep -r "analytics_storage.*denied\|ad_storage.*denied\|default.*denied" --include="*.ts" --include="*.tsx" --include="*.js" .
```

---

## Third-Party Domain Classification

When found in URL scan or code, classify these domains:

| Domain pattern | Service | Data sent | Legal basis | DPA required |
|---|---|---|---|---|
| `google-analytics.com`, `googletagmanager.com` | GA4 / GTM | IP, pageview, user agent, events | Art. 6(1)(a) consent | Yes — Google LLC (US) |
| `fonts.googleapis.com`, `fonts.gstatic.com` | Google Fonts | IP address | Art. 6(1)(f) LIA (fails post-LG München I) or self-host | No (no personal data stored by Google per their terms, but IP still transferred) |
| `js.stripe.com`, `*.stripe.com` | Stripe | Browser fingerprint, device data | Art. 6(1)(b) contract | Yes — Stripe Payments Europe Ltd (IE) |
| `eu.posthog.com`, `app.posthog.com` | PostHog | Session data, events | Art. 6(1)(a) consent | Yes — PostHog Inc. (EU cloud) |
| `vercel-analytics.com`, `vitals.vercel-insights.com` | Vercel Analytics | Server-side only — page URL, referrer, no JS | Art. 6(1)(f) LIA | Covered by Vercel DPA |
| `*.supabase.co`, `*.supabase.com` | Supabase | DB content (whatever app stores) | Depends on what's stored | Yes — Supabase Inc. |
| `api.resend.com` | Resend | Email metadata + body | Art. 6(1)(b) contract | Yes — Resend Inc. (US) |
| `api.smoobu.com` | Smoobu | Booking data, guest PII | Art. 6(1)(b) contract | Yes — Smoobu GmbH (DE) |
| `api.anthropic.com` | Anthropic Claude API | Message content | Art. 6(1)(f) or (b) | Yes — Anthropic PBC (US) |
| `api.sendgrid.com` | SendGrid (Twilio) | Email metadata + body | Art. 6(1)(b) contract | Yes — Twilio Inc. (US) |
| `sentry.io`, `*.sentry.io` | Sentry | Error logs, potentially PII in stack traces | Art. 6(1)(f) LIA | Yes — Sentry, Inc. (US) |
| `cdn.jsdelivr.net`, `unpkg.com`, `cdnjs.cloudflare.com` | CDN | IP address | Art. 6(1)(f) LIA | No (transient) |
| `api.telegram.org` | Telegram Bot | Message content | Art. 6(1)(f) LIA | Check Telegram DPA |
| `eu.i.posthog.com` | PostHog EU cloud | Session data | Art. 6(1)(a) consent | Yes — EU jurisdiction |

---

## Consent Banner Correctness Checklist

When auditing a live site's cookie banner, check all of the following:

**Blocking check:**
- [ ] Third-party analytics scripts are NOT present in DOM before user interacts with banner
- [ ] `<script>` tags for GA4/GTM/PostHog are loaded conditionally (not in `<head>` unconditionally)
- [ ] No network requests to analytics domains appear in browser DevTools before consent

**Banner design (CJEU Planet49 + German BGH 2020):**
- [ ] "Accept" and "Reject" options are equally prominent (same font size, same visual weight)
- [ ] No pre-ticked checkboxes
- [ ] Closing the banner WITHOUT clicking = reject (not = accept)
- [ ] Purpose descriptions are specific, not generic ("analytics" not "improve your experience")
- [ ] Individual service toggle available (not just "all or nothing")

**Consent memory:**
- [ ] Consent preference stored in localStorage (TTDSG § 25(2) strictly necessary exemption applies)
- [ ] On return visit, banner does NOT re-show if user already consented
- [ ] On return visit, analytics scripts fire only if consent was previously granted

**Withdrawal:**
- [ ] A way to withdraw consent exists (footer link, settings button, or re-opening the banner)
- [ ] Withdrawal is as easy as giving consent (Art. 7(3))

---

## Astro-specific Patterns

For Astro sites:

```bash
# Script tags in Astro
grep -r "define:vars\|is:inline\|src=\|client:load\|client:idle" --include="*.astro" src/

# External scripts in layouts
grep -r "script\|link.*href" src/layouts/ --include="*.astro"

# Environment variables used in frontend (public)
grep -r "import\.meta\.env\.PUBLIC_" --include="*.astro" --include="*.ts" src/
```

## Next.js-specific Patterns

For Next.js sites:

```bash
# Script component usage
grep -r "from 'next/script'\|<Script" --include="*.tsx" --include="*.ts" app/ components/

# API route handlers that receive PII
grep -r "req\.body\|request\.json\|formData\|params\." app/api/ --include="*.ts" -l

# Server actions
grep -r "\"use server\"\|'use server'" --include="*.ts" --include="*.tsx" app/ -l

# Middleware (often handles auth/session — check what data it touches)
cat middleware.ts 2>/dev/null || cat middleware.js 2>/dev/null
```
