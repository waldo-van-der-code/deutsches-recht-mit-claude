# deutsches-recht-mit-claude

**Deutsches Rechts-Compliance direkt in Claude Code** — drei Skills für DSGVO/GDPR, BFSG Barrierefreiheit und Einwilligungs-Widerruf. Grounded in echten deutschen Gesetzestexten via [rechtsinformationen.bund.de](https://www.rechtsinformationen.bund.de/) MCP.

**German legal compliance built into Claude Code** — three skills covering DSGVO/GDPR, BFSG accessibility, and consent withdrawal. Grounded in live German federal law via the [rechtsinformationen.bund.de](https://www.rechtsinformationen.bund.de/) MCP.

> ⚠️ **Keine Rechtsberatung / Not legal advice.** Diese Skills helfen Entwicklern, rechtlich relevante Muster zu finden und zu dokumentieren. Outputs sollten von einem qualifizierten Datenschutzbeauftragten oder Rechtsanwalt geprüft werden. / These skills help developers identify and document legally relevant patterns. Always have outputs reviewed by a qualified Datenschutzbeauftragter or attorney.

---

## Drei Skills — ein Repo

| Skill | Befehl | Was er macht |
|---|---|---|
| **gdpr-dsgvo** | `/legal-audit` | DSGVO/GDPR Compliance: Code-Audit, URL-Scan, Datenfluss-Mapping, Dokumentenentwürfe |
| **bfsg** | `/bfsg-check` | Barrierefreiheit: prüft BFSG + WCAG 2.1 AA Konformität für E-Commerce-Sites |
| **widerruf** | `/widerruf-check` | Einwilligungs-Widerruf: prüft ob Widerruf so einfach ist wie Einwilligung (Art. 7(3) DSGVO) |

Alle drei greifen auf das `rechtsinformationen-bund-de` MCP für live Gesetzestexte zurück, wenn verfügbar.

---

## Schnellstart

### 1. (Empfohlen) Live-Gesetzes-MCP installieren

```bash
git clone --depth 1 https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp.git \
  ~/.claude/mcp-servers/rechtsinformationen-bund-de
cd ~/.claude/mcp-servers/rechtsinformationen-bund-de
npm install && npm run build
```

In `~/.mcp.json` eintragen:

```json
{
  "mcpServers": {
    "rechtsinformationen-bund-de": {
      "type": "stdio",
      "command": "node",
      "args": ["~/.claude/mcp-servers/rechtsinformationen-bund-de/dist/index.js"]
    }
  }
}
```

Claude Code neu starten, danach ist der MCP aktiv.

### 2. Skills installieren

```bash
# Repo klonen
git clone https://github.com/waldo-van-der-code/deutsches-recht-mit-claude.git /tmp/de-recht-skills

# Alle drei Skills installieren
for skill in gdpr-dsgvo bfsg widerruf; do
  cp -r /tmp/de-recht-skills/skills/$skill ~/.claude/skills/$skill
done
```

Oder nur einzeln:

```bash
# Nur DSGVO/GDPR
cp -r /tmp/de-recht-skills/skills/gdpr-dsgvo ~/.claude/skills/gdpr-dsgvo

# Nur BFSG Barrierefreiheit
cp -r /tmp/de-recht-skills/skills/bfsg ~/.claude/skills/bfsg

# Nur Widerruf/Consent Withdrawal
cp -r /tmp/de-recht-skills/skills/widerruf ~/.claude/skills/widerruf
```

---

## Skills im Detail

### `/legal-audit` — DSGVO / GDPR

```bash
/legal-audit audit ./mein-nextjs-projekt   # Quellcode-Scan
/legal-audit scan https://example.de       # Live-URL-Scan
/legal-audit flow ./src                    # Datenfluss-Mapping → Art. 30 VVT
/legal-audit draft privacy-section "Resend (transaktionale E-Mails)"
/legal-audit draft impressum
/legal-audit draft cookie-banner
```

**Abgedeckte Gesetze:** DSGVO Art. 5, 6, 7, 13, 25, 28, 30, 32, 33, 46, 83 · TTDSG § 25 · BDSG § 26, 38 · TMG/DDG § 5

**Aktiviert sich automatisch** bei: „Ist unsere Datenschutzerklärung korrekt?", „Brauchen wir einen AV-Vertrag für Stripe?", „Datenschutz", „DSGVO Check", „Impressum"

---

### `/bfsg-check` — Barrierefreiheit (BFSG + WCAG 2.1 AA)

```bash
/bfsg-check scan https://example.de       # Live-Scan auf BFSG-Konformität
/bfsg-check audit ./src                   # Code-Audit für Barrierefreiheits-Anti-Pattern
/bfsg-check erklaerung                    # Barrierefreiheitserklärung (§ 12 BFSG) generieren
```

Das **Barrierefreiheitsstärkungsgesetz (BFSG)** gilt seit 28. Juni 2025 für B2C-Anbieter digitaler Dienste. Der Skill prüft WCAG 2.1 AA Erfolgskriterien, E-Commerce-spezifische Anforderungen und die Pflicht zur Barrierefreiheitserklärung. Bußgeld: bis zu 100.000 € pro Verstoß.

**Aktiviert sich automatisch** bei: „BFSG", „Barrierefreiheit", „WCAG", „barrierefrei", „Screenreader", „Kontrast prüfen"

---

### `/widerruf-check` — Einwilligungs-Widerruf (Art. 7(3) DSGVO)

```bash
/widerruf-check scan https://example.de   # Cookie-Banner + Widerruf-UX prüfen
/widerruf-check audit ./src               # Einwilligungs-Implementierung prüfen
/widerruf-check checklist                 # Vollständige Widerruf-UX-Checkliste ausgeben
```

Kernfrage: Ist der Widerruf **so einfach wie die Einwilligung** (Art. 7(3) DSGVO)? Der Skill prüft Klick-Symmetrie, visuelle Gleichwertigkeit von Ablehnen- und Akzeptieren-Buttons, und ob nachträglicher Widerruf immer möglich ist.

**Aktiviert sich automatisch** bei: „Widerruf", „Einwilligung widerrufen", „Widerruf-Button", „Art. 7(3)", „Cookie Ablehnen"

---

## Das Alleinstellungsmerkmal: echte Gesetzestexte

Mit dem `rechtsinformationen-bund-de` MCP rufen alle drei Skills **live den aktuellen Gesetzestext** vom offiziellen deutschen Bundesrechtsportal ab, bevor sie einen Artikel zitieren. Ohne MCP funktionieren die Skills trotzdem — greifen auf Trainingswissen zurück und kennzeichnen dies deutlich.

---

## Projektstruktur

```
deutsches-recht-mit-claude/
├── README.md
├── LICENSE
└── skills/
    ├── gdpr-dsgvo/
    │   ├── SKILL.md                    /legal-audit — DSGVO/GDPR Compliance
    │   └── references/
    │       ├── law-reference.md        DSGVO/TTDSG/BDSG Artikel, Rechtsprechung, Behörden
    │       ├── audit-patterns.md       Grep-Muster, Domain-Klassifikation, Banner-Checkliste
    │       ├── data-flow.md            Art. 30 VVT Template, AV-Vertrags-URLs
    │       └── document-templates.md  Art. 13 Checkliste, Impressum, Vorlagen
    ├── bfsg/
    │   ├── SKILL.md                    /bfsg-check — Barrierefreiheitsprüfung
    │   └── references/
    │       └── bfsg-requirements.md   WCAG 2.1 AA Kriterienkatalog, BFSG-Paragraphen
    └── widerruf/
        ├── SKILL.md                    /widerruf-check — Widerruf-UX-Prüfung
        └── references/
            └── widerruf-checklist.md  Rechtsprechung, DSK-Beschlüsse, Muster-Texte
```

---

## Wir suchen Experten-Feedback 🙏

Diese Skills wurden von einem Entwickler gebaut, nicht von Juristen. Issues und PRs sind herzlich willkommen — besonders von:

- [ ] **Datenschutzbeauftragte / Datenschutzanwälte** — stimmen die Artikel-Zitate? Gibt es Lücken im Verstoß-zu-Artikel-Mapping?
- [ ] **BFSG / Barrierefreiheits-Experten** — korrekte EN 301 549 Interpretation? Fehlende E-Commerce-Anforderungen?
- [ ] **CMP-Erfahrene** — welche Konfigurationen sind in der Praxis konform? Erfahrungen mit Behörden?
- [ ] **Österreich (DSG) / Schweiz (nDSG)** — Unterschiede zur DSGVO brauchen korrekte Abdeckung
- [ ] **DSFA-Screening** — Art. 35 DSGVO und Muss-Listen der Datenschutzbehörden

Rechtsprofis besonders willkommen — bitte Hintergrund kurz nennen, damit Beiträge entsprechend eingeordnet werden können.

---

## Inspiration

- [zubair-trabzada/ai-legal-claude](https://github.com/zubair-trabzada/ai-legal-claude) — URL-Scan-Pattern, paralleler Subagent-Ansatz
- [Sushegaad/Claude-Skills-Governance-Risk-and-Compliance](https://github.com/Sushegaad/Claude-Skills-Governance-Risk-and-Compliance) — Code-Audit-Pattern, Schweregrad-Abstufung, Dokumentenentwürfe
- [wolfgangihloff/rechtsinformationen-bund-de-mcp](https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp) — Live-Gesetzestext-Lookup

## Lizenz

MIT — siehe [LICENSE](LICENSE).
