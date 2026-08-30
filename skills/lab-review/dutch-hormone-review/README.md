# DUTCH Hormone Review

| | |
|---|---|
| **Author** | Meelio Clinical Team *(add yourself — see [CONTRIBUTING](../../../CONTRIBUTING.md))* |
| **Category** | Lab review |
| **Version** | 1.0.1 |
| **Works in** | Meelio · Claude · ChatGPT |

Interpretation of the DUTCH Complete / DUTCH Plus (Precision Analytical dried-urine hormone panel): cortisol production vs clearance vs free-cortisol rhythm (and CAR on the Plus), sex hormones with their phase I/II metabolite pathways (2-OH/4-OH/16-OH estrogens, methylation, 5α/5β androgen preference), progesterone adequacy, and the built-in organic acids. Applies the test's validity caveats (cycle phase, hormonal contraceptives, HRT delivery route, pregnancy) before interpreting anything.

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `DUTCH hormone review` · Description: copy from [`SKILL.md`](SKILL.md) frontmatter
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. Upload the DUTCH PDF to the patient's documents, then type `/` in a patient chat and pick the skill.

### Claude

Copy this folder to `~/.claude/skills/dutch-hormone-review/`, or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions. Use de-identified data unless your organization has a BAA with Anthropic.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions. Use de-identified data unless your organization has a BAA with OpenAI.

## Evidence & sources

Ranges are **Precision Analytical's published, population- and cycle-phase-specific reference sets** — the skill's first job is verifying results were read against the right set. The underlying biochemistry (2-OH/4-OH/16-OH estrogen pathways, COMT methylation, 4-OH quinone DNA reactivity, 5α/5β reduction, cortisol production vs clearance) is established literature; the cortisol awakening response has an independent salivary-research base. Honest limitations stated in the body: independent clinical validation of dried-urine hormone testing is limited and serum remains the endocrinology standard — the skill defers to serum for dosing and fertility decisions, restricts conclusions on hormonal contraceptives, excludes pregnancy (not validated), and forbids framing 4-OH elevation as cancer prediction. Escalation triggers (postmenopausal bleeding, virilization, Cushingoid/adrenal-insufficiency features) follow conventional endocrine practice.

## Changelog

- **1.0.1** — Added reference-population verification to the validity gate and threshold provenance.
- **1.0.0** — Initial version.
