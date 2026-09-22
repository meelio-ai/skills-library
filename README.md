# Functional Medicine Clinical Skills Library

**Repeatable AI workflows for functional & integrative medicine — written by clinicians, for clinicians.**

A *skill* is a clinical procedure written down once — "how I review a GI-MAP", "how I interpret a DUTCH panel against the chart" — that an AI assistant can then follow every time, consistently. This library collects openly licensed, clinician-authored skills that work in [Meelio](https://meelio.ai), Claude, and ChatGPT.

> ⚕️ **For licensed healthcare professionals only.** These skills are clinical decision-support procedures. They do not diagnose, treat, or replace clinical judgment. Read the [full disclaimer](DISCLAIMER.md) before use.

---

## Skills

| Skill | Category | Author | Description |
|---|---|---|---|
| [Comprehensive Lab Review](skills/lab-review/comprehensive-lab-review/) | Lab review | Meelio Clinical Team | Review new lab results against the full patient record — trends, functional ranges, medication context, and a prioritized action list |
| [GI-MAP Review](skills/lab-review/gi-map-review/) | Lab review | Meelio Clinical Team | Structured interpretation of the GI-MAP (qPCR stool analysis): pathogens, dysbiosis patterns, intestinal health markers, and red flags |
| [DUTCH Hormone Review](skills/lab-review/dutch-hormone-review/) | Lab review | Meelio Clinical Team | DUTCH Complete/Plus interpretation: sex hormone metabolites, cortisol pattern, methylation, and phase-appropriate context |
| [Thyroid Panel Review](skills/lab-review/thyroid-panel-review/) | Lab review | Meelio Clinical Team | Full thyroid assessment with functional ranges: conversion, autoimmunity, nutrient cofactors, and interference checks |
| [Organic Acids (OAT) Review](skills/lab-review/oat-review/) | Lab review | Meelio Clinical Team | OAT interpretation: yeast/bacterial overgrowth markers, mitochondrial function, neurotransmitter metabolites, and nutrient status |

Want to contribute a skill and be credited as its author? See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## How each skill is packaged

Every skill folder contains:

| File | For | Format |
|---|---|---|
| `README.md` | Humans | The skill card: what it does, who wrote it, how to install it on each platform |
| `SKILL.md` | Claude / ChatGPT | Portable version with YAML frontmatter. Asks the clinician to supply the report and includes a **data-handling check** before any patient data is shared |
| `meelio.md` | Meelio | Meelio-ready body (no frontmatter — Meelio doesn't parse it). Written against Meelio's patient-record tools so the assistant pulls labs, history, and medications itself |

The two bodies share the same clinical core. The Meelio version is richer because Meelio's assistant has structured access to the chart; the portable version works anywhere but depends on what you paste in.

## Using a skill

### In Meelio (recommended for patient data)

Meelio is HIPAA compliant, signs BAAs, and runs skills natively against the patient record — the assistant retrieves labs, history, and medications itself, so you never paste PHI anywhere.

1. Open **Settings → Skills → New skill**.
2. Copy the skill's `name` and `description` from its README, and paste the full contents of `meelio.md` into the body.
3. In any patient chat, type `/` and pick the skill (or use the Skills picker).

Notes for Meelio: skills describe **steps, never patients** — Meelio checks every skill for patient information and flags anything that contains it. Bodies are capped at 10,000 characters; every skill in this library fits. A deeplink install flow ("Open in Meelio") is planned; each skill's metadata already carries what it needs.

### In Claude

- **Claude Code / Claude Desktop skills:** drop the skill folder into your skills directory (e.g. `~/.claude/skills/gi-map-review/SKILL.md`), then invoke it by name.
- **Claude.ai Projects:** paste `SKILL.md` (minus the frontmatter) into the project's instructions.

### In ChatGPT

- **Custom GPT:** paste `SKILL.md` (minus the frontmatter) into the GPT's instructions.
- **Projects / one-off:** paste the body into a project's custom instructions, or at the top of a conversation before providing the (de-identified) report.

---

## ⚠️ HIPAA & patient data — read this before pasting a lab report anywhere

Where you run a skill determines what data you may lawfully use with it:

| Platform | HIPAA status | What you may use |
|---|---|---|
| **Meelio** | HIPAA compliant; BAA available (privacy@meelio.ai). Encrypted, audit-logged, EHR-grade architecture | Real patient data — the skill runs against the chart |
| **Claude** (Free/Pro/Team) | **No BAA — not HIPAA compliant** | De-identified data only. Never paste PHI |
| **Claude Enterprise / API** | BAA possible via Anthropic sales; requires your org to have one in place | PHI only if your organization has an executed BAA and your policies allow it |
| **ChatGPT** (Free/Plus/Team) | **No BAA — not HIPAA compliant** | De-identified data only. Never paste PHI |
| **ChatGPT Enterprise / OpenAI API** | BAA possible via OpenAI sales; requires your org to have one in place | PHI only if your organization has an executed BAA and your policies allow it |

De-identification means removing **all 18 HIPAA identifiers** (name, DOB, dates of service, MRN, location below state level, etc.) — not just the name. If you are unsure whether your setup is compliant, use de-identified data or use a HIPAA-compliant platform. Every portable skill in this library begins with this check.

## Disclaimers

In short — the [full disclaimer](DISCLAIMER.md) governs:

- **Professional use only.** These skills are for licensed healthcare professionals acting within their scope of practice.
- **Decision support, not decisions.** Outputs are supportive analysis to be reviewed, verified, and overridden by the treating clinician. Nothing here is medical advice, diagnosis, or treatment.
- **Functional ranges are opinions — with provenance.** Skills always show the performing lab's reference interval alongside any functional target, and every threshold is tiered by source: clinical guideline, lab-published cutoff, or functional-medicine practice convention. Each skill's README lists its sources under **Evidence & sources**. Functional targets are not universally accepted; apply your own clinical standards.
- **You are responsible for compliance** with HIPAA, GDPR, state law, and your professional obligations in whatever tool you run these skills.

## Contributing

Skills are contributed by named clinician authors via pull request. The [contributing guide](CONTRIBUTING.md) covers the format, the authoring rules (procedures, never patients), and how attribution works.

## License

[MIT](LICENSE). Attribution to authors is preserved in each skill's README and frontmatter.
