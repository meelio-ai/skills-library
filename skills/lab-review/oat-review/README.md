# Organic Acids Test (OAT) Review

| | |
|---|---|
| **Author** | Meelio Clinical Team *(add yourself — see [CONTRIBUTING](../../../CONTRIBUTING.md))* |
| **Category** | Lab review |
| **Version** | 1.0.1 |
| **Works in** | Meelio · Claude · ChatGPT |

Interpretation of a urinary organic acids test (Mosaic/Great Plains-style OAT) by functional block: yeast/fungal and bacterial overgrowth markers (arabinose, HPHPA, 4-cresol), oxalates, mitochondrial/Krebs intermediates, neurotransmitter metabolites (HVA, VMA, 5-HIAA, quinolinic), methylation and B-vitamin markers, fatty-acid oxidation, and glutathione status — with the diet/supplement/collection caveats that make single OAT markers unreliable on their own.

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `OAT review` · Description: copy from [`SKILL.md`](SKILL.md) frontmatter
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. Upload the OAT PDF to the patient's documents, then type `/` in a patient chat and pick the skill.

### Claude

Copy this folder to `~/.claude/skills/oat-review/`, or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions. Use de-identified data unless your organization has a BAA with Anthropic.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions. Use de-identified data unless your organization has a BAA with OpenAI.

## Evidence & sources

Ranges are the **performing lab's age-bracketed, creatinine-corrected reference sets** (Mosaic Diagnostics, formerly Great Plains) — always attributed to the lab. Evidence tiers are stated honestly in the body: nutrient-functional markers (methylmalonic acid for B12, FIGLU for folate, xanthurenic for B6) and the inborn-error red flags (glyceric/glycolic for primary hyperoxaluria, orotic acid for urea-cycle stress) rest on established biochemical genetics and are the strongest part of the test; dysbiosis markers (arabinose, HPHPA, 4-cresol) rest largely on small, lab-affiliated studies, and the dopamine-β-hydroxylase inhibition story is mechanistic with limited clinical validation — which is why the skill treats the OAT as a screening/pattern tool, requires convergence across blocks, and recommends confirmation before anything would change management.

## Changelog

- **1.0.1** — Added age-bracket/reference verification, pregnancy and athlete/ketogenic context, and evidence tiering.
- **1.0.0** — Initial version.
