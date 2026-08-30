# <Display Name — e.g. "GI-MAP Review"> 

| | |
|---|---|
| **Author** | <Name, credentials — e.g. Dr. Jane Doe, ND, IFMCP> |
| **Category** | <e.g. Lab review> |
| **Version** | 1.0.0 |
| **Works in** | Meelio · Claude · ChatGPT |

<Two or three sentences: what the skill does, what it takes as input, what it produces.>

> ⚕️ For licensed healthcare professionals only. Decision support — outputs must be reviewed by the treating clinician. See the [library disclaimer](../../../DISCLAIMER.md). Do not paste PHI into non-HIPAA-compliant tools ([details](../../../README.md#️-hipaa--patient-data--read-this-before-pasting-a-lab-report-anywhere)).

## Install

### Meelio (runs against the patient record — HIPAA compliant)

1. **Settings → Skills → New skill**
2. Name: `<Display Name>` · Description: copy the frontmatter `description` from [`SKILL.md`](SKILL.md)
3. Body: paste the full contents of [`meelio.md`](meelio.md)
4. In a patient chat, type `/` and pick the skill.

### Claude

Copy this folder into your skills directory (`~/.claude/skills/<skill-slug>/`), or paste the body of [`SKILL.md`](SKILL.md) into a Project's instructions.

### ChatGPT

Paste the body of [`SKILL.md`](SKILL.md) into a Custom GPT's or Project's instructions.

## Changelog

- **1.0.0** — Initial version.
