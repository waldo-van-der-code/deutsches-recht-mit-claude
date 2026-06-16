# deutsches-recht-mit-claude

**3 Claude Code Skills für Website-Betreiber in Deutschland** — DSGVO, BFSG Barrierefreiheit und Einwilligungs-Widerruf. Auf Basis echter Gesetzestexte via [rechtsinformationen.bund.de](https://www.rechtsinformationen.bund.de/) MCP.

*3 Claude Code skills for German website operators — DSGVO/GDPR, BFSG accessibility, and consent withdrawal, grounded in live statutory text.*

> ⚠️ **Keine Rechtsberatung.** Diese Skills helfen dabei, rechtlich relevante Muster zu finden und zu dokumentieren. Outputs sollten von einem qualifizierten Datenschutzbeauftragten oder Rechtsanwalt geprüft werden.

---

## Wofür ist das?

Andere Legal-Tools für Claude Code sind für Kanzleien gebaut oder decken das gesamte deutsche Recht ab. Dieser Skill-Set ist für Website-Betreiber: Booking-Sites, E-Commerce, SaaS, Freelancer-Websites. Genau die 3 Bereiche, die für den laufenden Betrieb einer deutschen Website relevant sind.

Was diese Skills von anderen unterscheidet: sie nutzen den [rechtsinformationen.bund.de MCP-Server](https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp) von wolfgangihloff und holen live den aktuellen Gesetzestext vom offiziellen deutschen Bundesrechtsportal, bevor sie einen Artikel zitieren. Keine Wissensdatenbank, die manuell aktualisiert werden muss. Kein halluzinierter Artikeltext.

---

## 3 Skills

| Skill | Befehl | Was er tut |
|---|---|---|
| **gdpr-dsgvo** | `/legal-audit` | DSGVO-Audit: Code, live URL, Datenfluss, Dokumentenentwürfe |
| **bfsg** | `/bfsg-check` | Barrierefreiheit: BFSG + WCAG 2.1 AA, gilt seit 28. Juni 2025 |
| **widerruf** | `/widerruf-check` | Widerruf-UX: prüft ob Ablehnen genauso einfach ist wie Akzeptieren |

---

## Schnellstart

### 1. MCP installieren (empfohlen)

Der MCP von [wolfgangihloff](https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp) liefert live Gesetzestexte. Ohne ihn funktionieren die Skills trotzdem — sie greifen dann auf Trainingsdaten zurück und kennzeichnen das deutlich.

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

Claude Code neu starten.

### 2. Skills installieren

```bash
git clone https://github.com/waldo-van-der-code/deutsches-recht-mit-claude.git ~/.claude/skills/deutsches-recht-mit-claude
```

Alle 3 Skills sind danach direkt verfügbar.

---

## Skills im Detail

### `/legal-audit` — DSGVO

```bash
/legal-audit audit ./mein-nextjs-projekt   # Quellcode-Scan
/legal-audit scan https://example.de       # Live-URL-Scan
/legal-audit flow ./src                    # Datenfluss-Mapping → Art. 30 VVT
/legal-audit draft privacy-section "Resend (transaktionale E-Mails)"
/legal-audit draft impressum
/legal-audit draft cookie-banner
```

Findet alle Datenverarbeiter, prüft Datenschutzerklärung, Rechtsgrundlagen, AVVs, SCCs für US-Dienste. Befunde kommen als 🔴 / 🟡 / 🟢 mit exaktem Gesetzestext.

Schreibt auch: Datenschutzerklärung-Abschnitte (DE + EN), Impressum, Cookie-Banner, Art. 30 VVT.

**Abgedeckte Gesetze:** DSGVO Art. 5, 6, 7, 13, 25, 28, 30, 32, 33, 46, 83 · TTDSG § 25 · BDSG § 26, 38 · DDG § 5

**Aktiviert sich automatisch** bei: „Ist unsere Datenschutzerklärung korrekt?", „Brauchen wir einen AV-Vertrag für Stripe?", „DSGVO Check", „Impressum"

---

### `/bfsg-check` — Barrierefreiheit (BFSG + WCAG 2.1 AA)

```bash
/bfsg-check scan https://example.de   # Live-Scan
/bfsg-check audit ./src               # Code-Audit
/bfsg-check erklaerung                # Barrierefreiheitserklärung (§ 12 BFSG) generieren
```

Das BFSG gilt seit 28. Juni 2025 für B2C-Digitalangebote. Bußgeld bis 100.000 € pro Verstoß. Kleinstunternehmen unter 10 Mitarbeitern und 2 Mio. € Umsatz sind meist ausgenommen — der Skill prüft das zuerst.

Prüft WCAG 2.1 AA, E-Commerce-spezifische Checkout-Anforderungen, und ob Barrierefreiheitserklärung (§ 12) und Beschwerdeverfahren (§ 17) vorhanden sind.

**Aktiviert sich automatisch** bei: „BFSG", „Barrierefreiheit", „WCAG", „barrierefrei", „Screenreader"

---

### `/widerruf-check` — Einwilligungs-Widerruf (Art. 7(3) DSGVO)

```bash
/widerruf-check scan https://example.de   # Cookie-Banner + Widerruf-UX
/widerruf-check audit ./src               # Einwilligungs-Implementierung
/widerruf-check checklist                 # Vollständige UX-Checkliste
```

Art. 7(3) DSGVO: Widerruf muss so einfach sein wie die Einwilligung. In der Praxis scheitern die meisten Implementierungen — 1 Klick zum Akzeptieren, 3 Menüs zum Ablehnen.

Prüft Klick-Symmetrie, visuelle Gleichwertigkeit der Buttons, Login-freier Widerruf, und ob die Datenschutzerklärung erklärt wie man widerruft.

**Aktiviert sich automatisch** bei: „Widerruf", „Einwilligung widerrufen", „Art. 7(3)", „Cookie Ablehnen"

---

## Projektstruktur

```
deutsches-recht-mit-claude/
├── README.md
├── LICENSE
└── skills/
    ├── gdpr-dsgvo/
    │   ├── SKILL.md
    │   └── references/
    │       ├── law-reference.md        DSGVO/TTDSG/BDSG Artikel, Rechtsprechung, Behörden
    │       ├── audit-patterns.md       Grep-Muster, Domain-Klassifikation, Banner-Checkliste
    │       ├── data-flow.md            Art. 30 VVT Template, AV-Vertrags-URLs
    │       └── document-templates.md  Art. 13 Checkliste, Impressum, Vorlagen
    ├── bfsg/
    │   ├── SKILL.md
    │   └── references/
    │       └── bfsg-requirements.md   WCAG 2.1 AA Kriterienkatalog, BFSG-Paragraphen
    └── widerruf/
        ├── SKILL.md
        └── references/
            └── widerruf-checklist.md  Rechtsprechung, DSK-Beschlüsse, Muster-Texte
```

---

## Experten-Feedback willkommen

Von einem Entwickler gebaut, nicht von Juristen. Issues und PRs willkommen — besonders von:

- [ ] **Datenschutzbeauftragte / Datenschutzanwälte** — stimmen die Artikel-Zitate? Lücken im Verstoß-zu-Artikel-Mapping?
- [ ] **BFSG / Barrierefreiheits-Experten** — korrekte EN 301 549 Interpretation? Fehlende E-Commerce-Anforderungen?
- [ ] **CMP-Erfahrene** — welche Konfigurationen sind in der Praxis konform?
- [ ] **Österreich (DSG) / Schweiz (nDSG)** — Unterschiede zur DSGVO
- [ ] **DSFA-Screening** — Art. 35 DSGVO und Muss-Listen der Datenschutzbehörden

---

## Danke an

- [wolfgangihloff/rechtsinformationen-bund-de-mcp](https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp) — Live-Gesetzestext-Lookup
- [zubair-trabzada/ai-legal-claude](https://github.com/zubair-trabzada/ai-legal-claude) — URL-Scan-Pattern
- [Sushegaad/Claude-Skills-Governance-Risk-and-Compliance](https://github.com/Sushegaad/Claude-Skills-Governance-Risk-and-Compliance) — Code-Audit-Pattern, Schweregrad-Abstufung

## Lizenz

MIT — siehe [LICENSE](LICENSE).
