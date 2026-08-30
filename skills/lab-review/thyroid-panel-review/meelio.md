# Thyroid Panel Review

Decision support for licensed healthcare professionals. Analysis only — the practitioner reviews, verifies, and decides.

## Gather context (before interpreting anything)

1. Call `get_patient_snapshot`. Use `labResults[]` (testName, value, unit, referenceRange, status) for thyroid markers — TSH, free T4, free T3, reverse T3, TPOAb, TgAb — plus ferritin, vitamin D, CRP if present; `medicalHistory` for conditions (autoimmune disease, thyroid history, pregnancy), medications (levothyroxine/liothyronine with dose, lithium, amiodarone, steroids, OCPs/estrogen) and supplements (**biotin**, iodine, selenium, iron); `recentEncounters` for symptoms.
2. Call `get_patient_documents` for structured `labFindings[]` from thyroid-containing reports, **and** `search_patient_documents` for the raw text — draw time relative to medication dose, lab methodology, and commentary that extraction misses.
3. Date by **specimen date** (`eventDate`) and build a trend for each marker across all prior results. Note explicitly which of FT4/FT3/rT3/antibodies have never been measured.
4. Skip anything marked `contradicted`. If history was truncated, write "history shown may be incomplete" — never "no history".

## Interpretation procedure

1. **Interference gate first.** Biotin in the supplement list distorts many thyroid immunoassays (typically mimicking hyperthyroidism — suppressed TSH, elevated FT4/FT3); if biotin appears in `supplements` and wasn't held 48–72 h before the draw, flag the results as potentially unreliable. Levothyroxine taken just before the draw inflates FT4. Acute illness or caloric restriction shifts the whole panel (non-thyroidal illness). Estrogen (OCP/HRT) raises binding proteins — free fractions matter more. Pregnancy requires trimester-specific ranges — say so and defer.
2. **Dual-range assessment.** Honor the recorded `status` for the lab interval, then add the functional lens, labeled as a practice convention: TSH functional target often ~1.0–2.5 µIU/mL (tolerate higher in older adults — TSH rises physiologically with age); FT4 mid-range; FT3 upper half; rT3 <~15 ng/dL with FT3:rT3 (ng/dL basis) >~0.2 as a conversion lens.
   Adjust for who the patient is (use `demographics` and `medicalHistory`): children need pediatric intervals (do not apply adult targets); autoimmune thyroid disease is markedly more common in women — keep a lower threshold for suggesting antibodies; within ~12 months postpartum (check encounters), think thyroiditis phases before Graves or permanent hypothyroidism; in the menopause transition, symptom overlap (fatigue, sleep, mood, temperature) makes lab–symptom discordance common — name it rather than force a thyroid explanation.
3. **Pattern recognition — name the pattern, not just the flags:**
   - *Primary hypothyroidism:* high TSH + low FT4. *Subclinical:* high TSH + normal FT4 — trend and antibodies decide significance.
   - *Hashimoto's:* positive TPOAb (±TgAb) — antibodies can precede TSH change by years; titers fluctuate, so trend over months beats single values.
   - *Central hypothyroidism:* low/inappropriately normal TSH + low FT4 — pituitary/hypothalamic evaluation, conventional referral.
   - *Conversion impairment:* normal TSH/FT4, low FT3 ± elevated rT3 — look for drivers in the chart before naming it: inflammation (CRP), caloric restriction, high stress, low ferritin/selenium/zinc.
   - *Non-thyroidal illness:* low FT3, high rT3, variable TSH during/after documented illness — recommend re-testing after recovery rather than interpreting.
   - *Hyperthyroidism:* suppressed TSH + high FT4/FT3 — conventional territory (see red flags). *Subclinical:* suppressed TSH + normal FT4/FT3 — still needs follow-up (AF/bone risk).
   - *Thyroiditis phases:* transient hyper → hypo, often postpartum or post-viral — encounter history and trend distinguish from Graves.
   - *On levothyroxine:* normal TSH with persistently low FT3 + documented residual symptoms is the classic poor-converter-on-T4-monotherapy picture — a consideration, not a directive.
4. **Cofactor and context review.** Ferritin (functional target often ~50+ ng/mL for conversion/hair/energy complaints), selenium (modest TPOAb-reduction evidence), zinc, vitamin D (autoimmunity context), iron/calcium/coffee spacing from thyroid medication. **Iodine caution:** flag any high-dose iodine supplement in a TPOAb-positive patient — it can trigger or worsen autoimmune thyroiditis.
5. **Trend and monitoring logic.** TSH re-equilibrates ~6–8 weeks after a dose change — flag any result drawn sooner as non-steady-state (check medication start/change dates in the chart). A TSH drifting upward with positive antibodies is a different conversation from a stable one.
6. **Correlate with symptoms in the record.** State concordance or discordance explicitly ("biochemically euthyroid on the lab interval; documented fatigue persists — differential includes iron deficiency, sleep, mood, perimenopause"). Note provenance for transcription-sourced symptoms.

## Red flags — escalate regardless of functional interpretation

Not exhaustive. Suppressed TSH with elevated FT4/FT3 (overt hyperthyroidism) — conventional evaluation, urgent with tachycardia/arrhythmia; thyroid storm features are an emergency; TSH >10 µIU/mL, or symptomatic hypothyroidism in pregnancy or when planning pregnancy — conventional management; suspected myxedema (profound hypothyroidism with altered mentation/hypothermia) — emergency; low TSH + low FT4 (central pattern) — pituitary workup; any nodule, goiter, dysphagia, hoarseness, or neck mass in the chart — imaging/ENT-endocrine pathway regardless of labs. Put these first in the output.

## Output format

1. **Snapshot** — one paragraph: the pattern and the single most important finding.
2. **Reliability notes** — interference and timing issues, stated first.
3. **Findings table** — Marker | Value | Lab range | Functional lens | Flag | Trend (specimen dates).
4. **Pattern** — which named pattern fits and why; what's missing to confirm it.
5. **Cofactors and context** — nutrient/medication interactions found in the chart.
6. **Red flags** — or an explicit "none identified against the checked list".
7. **Considerations for the practitioner** — missing tests worth adding (antibodies, FT3/rT3, ferritin), re-test timing, referral thresholds. Options, never directives.
8. **Patient-friendly summary** — only if asked.

If the practitioner wants it saved, save with `create_note` (a lab review is not a clinical note or care plan).

End with: "Decision support only — for review by the treating clinician."
