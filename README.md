# claude-dsgvo

**A Claude Code skill for German DSGVO / GDPR compliance** — code audits, live URL scans, data flow mapping, and policy drafting, grounded in actual German federal law via the [rechtsinformationen.bund.de](https://www.rechtsinformationen.bund.de/) MCP.

> ⚠️ **Not legal advice.** This skill helps developers identify and document DSGVO-relevant patterns. Always have outputs reviewed by a qualified Datenschutzbeauftragter or attorney before relying on them.

---

## What it does

| Command | What happens |
|---|---|
| `/legal-audit audit <path>` | Scans your codebase — finds third-party scripts, PII collection, consent gaps, missing Impressum. Severity: 🔴 Hoch / 🟡 Mittel / 🟢 Niedrig |
| `/legal-audit scan <url>` | Fetches the live site — detects undisclosed third-party scripts, audits consent banner correctness, checks required pages |
| `/legal-audit flow <path>` | Maps all PII flows → generates an Art. 30 VVT (Verzeichnis von Verarbeitungstätigkeiten) and DPA checklist |
| `/legal-audit draft <type>` | Generates bilingual (DE + EN) document sections: `privacy-section`, `privacy-page`, `impressum`, `cookie-banner`, `vvt`, `art13-notice` |
| `/legal-audit` | Full suite: audit + scan + flow, then offers to draft fixes |

**Auto-triggers** on natural-language legal questions — say "is our privacy policy compliant?" or "do we need a DPA for Stripe?" and the skill activates automatically.

### Law coverage

- **DSGVO** — Art. 5, 6, 7, 13, 25, 28, 30, 32, 33, 46, 83
- **TTDSG** — § 25(1) device access consent, § 25(2) strictly necessary exemption
- **BDSG** — § 26 employee data, § 38 DPO threshold
- **TMG / DDG** — § 5 Impressum requirements
- **Key case law** — LG München I 2022 (Google Fonts), Planet49 (CJEU 2019), Schrems II (CJEU 2020), BGH Cookie Banner 2020

### Live law lookups (optional but recommended)

When the `rechtsinformationen-bund-de` MCP is configured, the skill pulls **live statutory text** from the official German federal law portal before citing any article. Without it, the skill falls back to built-in training knowledge and flags it.

---

## Installation

### 1. Install the skill

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/waldo-van-der-code/claude-dsgvo.git ~/.claude/skills/legal-audit
```

The skill is immediately available as `/legal-audit` in any Claude Code session.

### 2. (Optional) Install the live law MCP

Enables real-time lookup of German federal law text. Without this, the skill still works but cites from training knowledge.

```bash
# Clone and build the rechtsinformationen.bund.de MCP server
git clone --depth 1 https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp.git \
  ~/.claude/mcp-servers/rechtsinformationen-bund-de
cd ~/.claude/mcp-servers/rechtsinformationen-bund-de
npm install && npm run build
```

Add to your `~/.mcp.json`:

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

Restart Claude Code / Claude Desktop to activate.

---

## Usage examples

```
# Audit a Next.js project
/legal-audit audit ./my-nextjs-app

# Scan a live site
/legal-audit scan https://example.de

# Generate a privacy policy section for a specific processor
/legal-audit draft privacy-section "Resend (transactional email)"

# Generate a complete Impressum block
/legal-audit draft impressum

# Natural language — skill auto-triggers
"Is our cookie banner legally valid under TTDSG?"
"Do we need to sign a DPA with Vercel?"
"Our site loads Google Fonts from the CDN — is that a problem?"
```

---

## Project structure

```
claude-dsgvo/
├── SKILL.md                        Core skill — routing logic, modes A–E
└── references/
    ├── law-reference.md            DSGVO/TTDSG/BDSG article table, case law, supervisory authorities
    ├── audit-patterns.md           Grep patterns, domain classification, consent banner checklist
    ├── data-flow.md                Art. 30 VVT template, processor DPA URLs, transfer matrix
    └── document-templates.md       Art. 13 checklist, Impressum, cookie banner, rights section templates
```

The skill uses **progressive disclosure** — `SKILL.md` stays lean (~1,700 words) and reference files load only when the relevant mode runs.

---

## Inspiration

Built by studying and combining the best ideas from:

- [zubair-trabzada/ai-legal-claude](https://github.com/zubair-trabzada/ai-legal-claude) — URL scan pattern, parallel subagent approach
- [Sushegaad/Claude-Skills-Governance-Risk-and-Compliance](https://github.com/Sushegaad/Claude-Skills-Governance-Risk-and-Compliance) — code audit pattern, severity grading, document drafting structure
- [wolfgangihloff/rechtsinformationen-bund-de-mcp](https://github.com/wolfgangihloff/rechtsinformationen-bund-de-mcp) — live German federal law lookup (the key differentiator)

---

## Seeking expert input 🙏

This skill was built by a developer, not a lawyer. It needs review and improvement from people who know German data protection law deeply.

**Specifically looking for:**

- [ ] **Datenschutzbeauftragte / privacy lawyers** — are the article citations correct? Any gaps in the violation-to-article mapping?
- [ ] **German DPA practitioners** — what patterns trigger supervisory authority attention that aren't in the audit checklist?
- [ ] **Austrian DSG coverage** — the skill covers DE and mentions AT, but the Austrian specifics (DSG 2018, differences from DSGVO) need proper coverage
- [ ] **Swiss nDSG coverage** — the revised Swiss Data Protection Act (in force since Sept 2023) differs meaningfully from DSGVO
- [ ] **DPIA screening** — Art. 35 DSGVO and BlnBDI's published list of processing requiring DPIA — should be a Mode F
- [ ] **Accuracy review of case law citations** — especially LG München I 2022 (Google Fonts ruling), has anything changed?

Open an issue or PR. All contributions welcome.

---

## Contributing

Issues and PRs welcome. When contributing:

- Cite the specific DSGVO article, TTDSG section, or court case for any legal claim
- Note if you are a legal professional (helps weight the input appropriately)
- If correcting something, link to the authoritative source (gesetze-im-internet.de, EUR-Lex, or a DPA decision)

---

## License

MIT — see [LICENSE](LICENSE).

---

## Disclaimer

*Dieser Skill dient der technischen Orientierung und ersetzt keine Rechtsberatung. Outputs should be reviewed by a qualified Datenschutzbeauftragter or attorney before relying on them for compliance decisions.*
