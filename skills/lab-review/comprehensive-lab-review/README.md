# Comprehensive Lab Review

| | |
|---|---|
| **Author** | Meelio Clinical Team *(add yourself — see [CONTRIBUTING](../../../CONTRIBUTING.md))* |
| **Category** | Lab review |
| **Version** | 1.0.1 |
| **Works in** | Meelio · Claude · ChatGPT |

Reviews a new set of lab results against the patient's full record: trends against prior results, standard **and** functional ranges side by side, medication/supplement interference checks, systems-based pattern analysis, and a prioritized summary with red flags first. The default skill to run when any new bloodwork arrives.

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `Comprehensive lab review` · Description: copy from [`SKILL.md`](SKILL.md) frontmatter
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. In a patient chat, type `/` and pick the skill. Meelio pulls the labs, history, and medications itself — nothing to paste.

### Claude

Copy this folder to `~/.claude/skills/comprehensive-lab-review/`, or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions. Use de-identified data unless your organization has a BAA with Anthropic.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions. Use de-identified data unless your organization has a BAA with OpenAI.

## Evidence & sources

Conventional flagging always uses the performing lab's reference interval. Guideline-tier anchors used: ADA *Standards of Care* (glycemic thresholds), AHA/ACC and EAS lipid guidance (ApoB/non-HDL-C preference, Lp(a) once-in-a-lifetime measurement, hsCRP risk tiers), KDIGO (CKD grading), Endocrine Society (vitamin D sufficiency ≥30 ng/mL). Functional targets (glucose 75–90, insulin <7, ferritin ~50–150, homocysteine <9, ALT/GGT optima, vitamin D 50–80) are practice conventions as taught in IFM-style training and codified in blood-chemistry analysis references (e.g. Weatherby & Ferguson; Optimal DX) — labeled as such in the body, never presented as validated cutoffs. Interference facts (biotin — FDA safety communication; metformin–B12; PPI–B12/Mg) are established pharmacology.

## Changelog

- **1.0.1** — Added patient-context adjustment step (sex, menopausal status, age, pregnancy, athletes) and provenance labeling of thresholds.
- **1.0.0** — Initial version.
