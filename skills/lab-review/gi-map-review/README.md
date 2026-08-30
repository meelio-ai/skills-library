# GI-MAP Review

| | |
|---|---|
| **Author** | Meelio Clinical Team *(add yourself — see [CONTRIBUTING](../../../CONTRIBUTING.md))* |
| **Category** | Lab review |
| **Version** | 1.0.1 |
| **Works in** | Meelio · Claude · ChatGPT |

Structured interpretation of the GI-MAP (Diagnostic Solutions Laboratory, qPCR stool analysis) in a fixed clinical order: pathogens → H. pylori and virulence factors → commensal/keystone flora → opportunists and overgrowth → fungi and parasites → intestinal health markers (elastase, sIgA, β-glucuronidase, calprotectin, zonulin). Reads cross-marker patterns, correlates with the patient's symptoms and history, and puts conventional red flags (calprotectin, occult blood, true pathogens) first.

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `GI-MAP review` · Description: copy from [`SKILL.md`](SKILL.md) frontmatter
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. Upload the GI-MAP PDF to the patient's documents, then type `/` in a patient chat and pick the skill.

### Claude

Copy this folder to `~/.claude/skills/gi-map-review/`, or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions. Use de-identified data unless your organization has a BAA with Anthropic.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions. Use de-identified data unless your organization has a BAA with OpenAI.

## Evidence & sources

Quantitative thresholds (e.g. H. pylori ~1e3, calprotectin 173 µg/g) are **Diagnostic Solutions Laboratory's published cutoffs** — attributed as such, not presented as guideline values; conventional gastroenterology treats fecal calprotectin ~50–250 µg/g as a gray zone with retest/colonoscopy pathways (ACG/ECCO-consistent), and the skill escalates on both bases. Pathogen and occult-blood handling follows conventional practice. Honest limitations stated in the body: qPCR commensal "reference ranges" are not clinically validated; zonulin assays have published validity concerns (labeled "supportive, never diagnostic"); stool candida has poor sensitivity; Blastocystis/Dientamoeba pathogenicity is genuinely debated. Elastase-1 (<200 µg/g pancreatic insufficiency) is a validated conventional assay. The β-glucuronidase–estrogen recirculation link is mechanistically grounded but clinically thin — framed as a cross-reference consideration, not a finding.

## Changelog

- **1.0.1** — Added patient-context adjustment step (age brackets, menopausal status/HRT for β-glucuronidase, immunocompromise) and threshold provenance.
- **1.0.0** — Initial version.
