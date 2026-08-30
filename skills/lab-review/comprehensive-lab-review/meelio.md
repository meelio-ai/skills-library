# Comprehensive Lab Review

Decision support for licensed healthcare professionals. Analysis only — the practitioner reviews, verifies, and decides.

## Gather context (before interpreting anything)

1. Call `get_patient_snapshot`. Use: `labResults[]` (testName, value, unit, referenceRange, status), `medicalHistory` (conditions, medications, supplements, allergies), `dietAndLifestyle`, `vitalSigns`, `recentEncounters`.
2. Call `get_patient_documents` for structured `labFindings[]` from lab reports, **and** `search_patient_documents` for the raw report text — collection conditions, methodology, and interpretive commentary that extraction misses.
3. Date every result by its **specimen date** (`eventDate`), not the report date. Identify the newest result set — that is what this review covers; everything older is trend material.
4. Skip anything marked `contradicted`. If a truncation notice says history was cut off, write "history shown may be incomplete" — never "no history".
5. If no recent labs exist, say so and stop — do not review stale results as if new.

## Interpretation procedure

1. **Inventory.** List every new result with its specimen date; identify the panels present and what's missing that the clinical question would normally require.
2. **Verify before interpreting.** Use values exactly as recorded; if a value is implausible or units are ambiguous, flag it as a data-quality query rather than interpreting it.
3. **Trend.** For each marker with priors, state direction and rate. Only call a change meaningful if it plausibly exceeds combined biological + analytical variation (TSH 2.1→2.4 is noise; ferritin 90→30 is signal).
4. **Dual-range flagging.** Honor the recorded `status` (normal/high/low/critical) for the lab's interval, then add a functional lens, clearly labeled. Commonly used functional targets (practice conventions, not validated cutoffs): fasting glucose 75–90 mg/dL; HbA1c <5.4% (ADA prediabetes threshold is 5.7%); fasting insulin <7 µIU/mL (HOMA-IR <1.5); TSH ~1.0–2.5 µIU/mL; ferritin ~50–150 ng/mL; 25-OH vitamin D 50–80 ng/mL (guideline sufficiency is ≥30); hsCRP <1.0 mg/L (AHA low-risk tier); homocysteine <9 µmol/L; ALT <~26 U/L (M) / <~22 U/L (F); GGT <~25 U/L; triglyceride:HDL <2 (ideally <1.5); B12 >500 pg/mL with normal MMA. Where a guideline threshold and a functional target differ, state both. If the practice has its own reference ranges encoded, prefer those and say so.
5. **Adjust for who the patient is** — use `demographics` (age, sex, dateOfBirth) and `medicalHistory` before flagging anything, and say in the output which adjustments applied:
   - *Sex and menstrual status:* ferritin and hemoglobin run lower in menstruating females; the same low-normal ferritin in an adult male or post-menopausal female is more suspicious and, with anemia, triggers GI evaluation. Lipids typically shift upward across the menopause transition — interpret a first post-menopausal panel against that, not as sudden pathology.
   - *Age:* TSH rises physiologically with age (tolerate higher targets in older adults); eGFR declines with age — grade CKD by guideline, but don't over-call mild age-consistent decline; be more conservative with borderline findings in elderly polypharmacy.
   - *Pregnancy:* trimester-specific ranges apply to thyroid and many analytes; plasma-volume expansion dilutes hemoglobin and ferritin — defer to obstetric reference sets and say so.
   - *Athletes / recent hard training (check `dietAndLifestyle`):* transient ALT/AST/CK elevations, higher plasma volume (pseudo-anemia) — recommend re-draw after 48–72 h rest before interpreting muscle/liver markers.
   - *Body composition:* creatinine/eGFR against muscle mass (suggest cystatin C when discordant); very low muscle mass masks renal decline.
6. **Group by system; read patterns, not single values.**
   - *Glycemic/metabolic:* glucose + insulin + HbA1c together (normal glucose with rising insulin = early insulin resistance; HbA1c unreliable with anemia/hemoglobinopathy — cross-check fasting glucose).
   - *Lipids:* prefer ApoB (or non-HDL-C) over LDL-C; TG:HDL as insulin-resistance proxy; note if Lp(a) has never been measured.
   - *Thyroid:* TSH alone is insufficient — note if FT4/FT3/antibodies are missing.
   - *Inflammation:* hsCRP, ESR, ferritin (acute-phase — high ferritin with high CRP is not an iron result), fibrinogen.
   - *Hematology:* CBC as patterns — microcytosis + low ferritin (iron deficiency) vs macrocytosis (B12/folate, alcohol, hypothyroid); RDW rises early; interpret with iron studies.
   - *Liver/kidney:* isolated mild ALT rise in metabolic context suggests hepatic fat; GGT as oxidative-stress/alcohol marker; creatinine/eGFR against muscle mass; suggest cystatin C when discordant.
   - *Micronutrients:* B12 with MMA/homocysteine rather than serum B12 alone; RBC magnesium over serum; zinc:copper balance.
7. **Interference check against the actual med/supplement list.** Biotin distorts many immunoassays (TSH, T4 — hold 48–72 h); metformin lowers B12; PPIs lower B12/magnesium; OCPs/HRT raise SHBG and binding proteins; levothyroxine timing affects FT4; recent illness elevates ferritin/CRP; intense exercise elevates ALT/AST/CK; non-fasting draws invalidate glucose/insulin/TG.
8. **Correlate with the chart.** Tie each significant finding to documented conditions, symptoms, and encounters. Explicitly note discordance, and note provenance when a correlation rests on transcription- or document-sourced data.
9. **Prioritize:** (a) urgent/safety, (b) primary drivers, (c) borderline/watch, (d) testing gaps worth considering.

## Red flags — escalate regardless of functional interpretation

Not exhaustive. Any result with recorded status `critical`; hemoglobin <10 g/dL or rapid unexplained drop; new iron-deficiency anemia in an adult male or post-menopausal female (GI evaluation); platelets <100 or neutropenia; potassium/sodium/calcium outside safe bounds; eGFR newly <45 or rapidly declining; transaminases >3× ULN or bilirubin rise with enzyme elevation; suppressed TSH with elevated FT4/FT3; glucose >250 mg/dL or HbA1c ≥9%; unexplained ferritin >1000; monoclonal protein or unexplained cytopenias. Put these first in the output and recommend conventional workup or urgent referral for the practitioner's consideration.

## Output format

1. **Snapshot** — one short paragraph: overall picture and the single most important finding.
2. **Findings table** — Marker | Value | Lab range | Functional lens | Flag | Trend (with specimen dates).
3. **Pattern analysis by system** — prose, patterns over single values.
4. **Correlations with history** — what fits, what doesn't, sourced from the chart.
5. **Red flags** — or an explicit "none identified against the checked list".
6. **Considerations for the practitioner** — possible further testing, monitoring intervals, referral considerations. Options, never directives.
7. **Patient-friendly summary** — only if asked.

If the practitioner wants it saved, save with `create_note` (a lab review is not a clinical note or care plan).

End with: "Decision support only — for review by the treating clinician."
