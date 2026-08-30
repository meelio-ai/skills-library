# Contributing a skill

Skills in this library are authored by named clinicians and reviewed via pull request. Contributing gets your name (and credentials, if you wish) permanently attached to the skill — in its frontmatter, its README, and the library index.

## Who can contribute

Anyone may open a PR, but clinical skills must have a **named clinician author or reviewer** — a licensed healthcare professional who takes authorship responsibility for the clinical content. List authors exactly as they should be credited (e.g. `Dr. Jane Doe, ND, IFMCP`).

## What a skill is

A skill is a **procedure, never a patient**. It describes the steps an AI assistant should follow — what data to gather, how to interpret it, what to watch for, how to structure the output. It must contain no patient data of any kind, not even invented examples with names:

- ❌ No patient names — not even fictional first+last name pairs (Meelio's PHI gate rejects them, and they normalize a bad habit)
- ❌ No MRNs, DOBs, dates of service, or any of the 18 HIPAA identifiers, real or fabricated
- ✅ Say "the patient", "a 40-year-old presenting with…", or use marker values without any identity attached

## Folder layout

```
skills/<category>/<skill-slug>/
├── README.md    # the skill card: what/who/how-to-install per platform
├── SKILL.md     # portable version (Claude, ChatGPT) — spec-conformant frontmatter
└── meelio.md    # Meelio-ready body (no frontmatter)
```

`<skill-slug>` must match the frontmatter `name`: lowercase letters, digits, hyphens (`^[a-z0-9]+(-[a-z0-9]+)*$`), ≤64 characters. Copy `templates/skill-template/` to start.

## Format rules

These limits come from the [Agent Skills open specification](https://github.com/agentskills/agentskills) and Meelio's importer, so a conformant skill installs everywhere without truncation:

| Field | Limit | Notes |
|---|---|---|
| `name` (frontmatter) | ≤64 chars, slug | Must match the folder name |
| Display name | ≤120 chars | Free text, set in README — what Meelio shows in its picker |
| `description` | ≤1024 chars | One or two sentences: what it does **and when to use it** |
| Body | ≤10,000 chars | Markdown only. GFM tables and lists are fine |
| Bundles | Not allowed | No `scripts/`, `references/`, or `assets/` — Meelio rejects executable/bundled skills by design, and text-only keeps skills auditable |

Check the body size: `wc -c skills/<category>/<slug>/meelio.md` — must be under 10,000.

### The two bodies

`SKILL.md` and `meelio.md` share the same clinical core and differ only in how they acquire data and where they save output:

- **`SKILL.md` (portable)** must open with the **data-handling check** (copy it from the template): before any patient data is shared, the assistant confirms the environment is appropriate for PHI, and if it isn't (consumer ChatGPT/Claude), instructs the clinician to de-identify or to use a HIPAA-compliant platform such as Meelio. It then asks the clinician to paste/upload the report and relevant context.
- **`meelio.md` (Meelio)** pulls context itself using Meelio's tools. Conventions that measurably improve outputs:
  - Start with `get_patient_snapshot` and name the fields you want: `labResults[]` (`testName`, `value`, `unit`, `referenceRange`, `status: normal|high|low|critical`), `medicalHistory` (conditions, medications, supplements, allergies), `dietAndLifestyle`, `recentEncounters`.
  - For document-based labs, call **both** `get_patient_documents` (structured `labFindings[]`) **and** `search_patient_documents` (raw report text — methodology, interpretive commentary, physician remarks). Extraction gives the skeleton; search gives the flesh.
  - Reason from the **specimen date** (`eventDate`), not the report date (`documentIssuedDate`).
  - Respect provenance (`source: ehr|transcription|document`) and skip anything marked `contradicted`. Never state "no history" when a truncation notice says history was cut off.
  - Save the finished review with **`create_note`** (lab reviews, letters, handouts) — not `create_clinical_note` (SOAP/visit notes) or `create_care_plan`.
  - Never reference a patient id — the patient is locked to the chat session.

### Required content in every skill body

1. A one-line scope statement ("decision support for licensed healthcare professionals; does not replace clinical judgment").
2. The interpretation procedure, in numbered steps.
3. When both standard laboratory ranges and functional ranges are referenced, show both and label them — never present a functional target as the lab's range.
4. **Provenance-tagged thresholds.** Every numeric threshold must be traceable to one of three tiers, and phrased accordingly: (a) a clinical guideline or regulatory source (e.g. ADA, ATA, AHA/ACC, KDIGO) — may be stated plainly; (b) the performing lab's published cutoff — attribute it to the lab; (c) a functional-medicine practice convention — must be phrased as "commonly targeted…" / "practice varies", never as an established range. List your sources in the skill's README under **Evidence & sources**.
5. **Context adjustment.** The interpretation procedure must adjust for who the patient is — sex, menopausal status, age, pregnancy, athletic status, and relevant medications — wherever those change what a value means (e.g. ferritin in a menstruating female vs an adult male; TSH in a 75-year-old; eGFR vs muscle mass). A skill that applies one target to everyone will be asked to revise.
6. A **red flags / escalation** section: findings that warrant conventional workup or urgent referral regardless of the functional interpretation.
7. An output format the assistant should follow.

## Clinical review bar

- Interpretation logic must reflect mainstream functional-medicine practice for that test, with conventional red flags always taking precedence.
- Cite specific thresholds only where they are defensible (lab-published cutoffs, widely used functional targets). Where practice varies, say so ("some practitioners target…").
- No product, brand, or supplement-protocol promotion. Naming lab vendors whose tests a skill interprets (e.g. Precision Analytical's DUTCH, Diagnostic Solutions' GI-MAP) is fine.
- Skills must instruct the assistant to frame recommendations as *considerations for the clinician*, never as directives or patient-facing advice.

## Submitting

1. Fork, copy `templates/skill-template/` to `skills/<category>/<your-slug>/`, write the three files.
2. Verify: slug matches, `wc -c` under limits, no names/identifiers anywhere, data-handling check present in `SKILL.md`, red-flags section present.
3. Open a PR. Include in the description: your authorship credit line, your clinical background (one line), and what you tested the skill on (de-identified, of course).
4. A maintainer plus one clinician reviewer approve before merge. You appear in the README index on merge.

## Licensing of contributions

By submitting, you agree your contribution is licensed under the repository's [MIT license](LICENSE) with authorship attribution preserved. You confirm the content is your own work (or properly licensed) and contains no patient data.
