# Thyroid Panel Review

| | |
|---|---|
| **Author** | Meelio Clinical Team *(add yourself — see [CONTRIBUTING](../../../CONTRIBUTING.md))* |
| **Category** | Lab review |
| **Version** | 1.0.1 |
| **Works in** | Meelio · Claude · ChatGPT |

Full thyroid assessment with functional ranges alongside the lab's intervals: TSH, free T4, free T3, reverse T3, and antibodies read as patterns (primary vs central vs conversion problems vs non-thyroidal illness vs Hashimoto's), with nutrient-cofactor context (ferritin, selenium, zinc, iodine caution) and the interference checks that most often produce misleading thyroid results (biotin, levothyroxine timing, acute illness, pregnancy).

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `Thyroid panel review` · Description: copy from [`SKILL.md`](SKILL.md) frontmatter
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. In a patient chat, type `/` and pick the skill.

### Claude

Copy this folder to `~/.claude/skills/thyroid-panel-review/`, or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions. Use de-identified data unless your organization has a BAA with Anthropic.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions. Use de-identified data unless your organization has a BAA with OpenAI.

## Evidence & sources

The pattern taxonomy (primary/subclinical/central hypothyroidism, non-thyroidal illness, thyroiditis phases, Graves) is standard endocrinology per **ATA/AACE guidance**, as are the escalation thresholds (TSH >10, pregnancy management, subclinical hyperthyroidism follow-up for AF/bone risk) and the age-related TSH rise. Biotin interference follows the **FDA safety communication** on biotin and immunoassays; the 6–8-week re-equilibration window is standard pharmacology. Functional elements are labeled as practice conventions: the TSH ~1.0–2.5 target, FT3-upper-half preference, and especially rT3/FT3:rT3 ratios — conventional endocrinology considers rT3 rarely clinically useful, and the skill presents it only as a "conversion lens." Selenium's modest TPOAb-reduction evidence and the iodine caution in TPOAb-positive patients reflect published trial data and mainstream caution respectively.

## Changelog

- **1.0.1** — Added patient-context adjustments (pediatric, sex, postpartum window, menopause overlap) and threshold provenance.
- **1.0.0** — Initial version.
