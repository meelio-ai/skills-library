# Organic Acids Test (OAT) Review

Decision support for licensed healthcare professionals. Analysis only — the practitioner reviews, verifies, and decides.

## Gather context (before interpreting anything)

1. Call `get_patient_documents` to locate the OAT report (category `lab_report`) and its structured `labFindings[]`, **and** `search_patient_documents` for the raw report text — OAT ranges are age-adjusted and creatinine-corrected, and the lab's own commentary matters; extraction alone loses it. If no OAT exists in the record, say so and stop.
2. Call `get_patient_snapshot` for `demographics` (age — ranges are age-specific), `medicalHistory` (conditions, medications — SSRIs, recent antibiotics/antifungals; supplements — vitamin C, probiotics, MCT, 5-HTP/tryptophan), `dietAndLifestyle` (fruit intake, fasting patterns), and `recentEncounters` for the clinical question (fatigue, mood/behavior, suspected candida, chronic pain).
3. Date by **specimen/collection date** (`eventDate`). If a GI-MAP or prior OAT exists in the documents, plan to cross-reference (see synthesis).
4. Skip anything marked `contradicted`. If history was truncated, write "history shown may be incomplete" — never "no history".

## Interpretation procedure

Interpret by block, then synthesize. Single OAT markers are weak evidence; convergent patterns across blocks are the signal.

1. **Collection validity and the right reference bracket.** Non-first-morning collection, high fruit intake in the prior 48 h (falsely raises arabinose and furans), vitamin C supplementation (oxalate/ascorbate pathways), and recent antibiotics reshape the profile — state applicable caveats from the chart before interpreting. Confirm the age bracket the ranges were applied against (`demographics.age`; OAT ranges are age-specific and creatinine-corrected); much of the marker literature is pediatric, so extrapolation to adults carries extra uncertainty — say so when it applies. In pregnancy and in athletes/ketogenic or fasted states (`dietAndLifestyle`), energy-metabolite and ketone blocks shift physiologically — interpret against that context rather than as pathology.
2. **Yeast/fungal markers.** Arabinose is the workhorse candida marker (with citramalic, tartaric, and furan compounds). Elevations suggest fungal overgrowth **after** diet is excluded; correlate with documented signs and any GI-MAP in the record (stool candida is insecure — OAT and GI-MAP together beat either alone). Tricarballylic relates to fusarium/mold exposure context.
3. **Bacterial and Clostridia markers.** General bacterial: elevated hippuric, 2-hydroxyphenylacetic, 4-hydroxybenzoic; DHPPA-type markers reflect beneficial flora. **Clostridia block is priority:** HPHPA and 4-cresol elevations inhibit dopamine-β-hydroxylase — check the neurotransmitter block for high HVA with low-normal VMA (high HVA:VMA). Marked elevations with behavioral/neurological symptoms deserve stool confirmation.
4. **Oxalates.** High oxalic with high glyceric or glycolic suggests genetic hyperoxaluria (conventional referral — see red flags). High oxalic alone: dietary load (spinach, almonds), fungal contribution (cross-check block 2), fat malabsorption, low B6, or high-dose vitamin C. Correlate with any kidney-stone history in the chart.
5. **Mitochondrial/Krebs block.** Elevated citric, aconitic, 2-oxoglutaric, succinic, fumaric, malic = impaired energy metabolism read as a pattern (multiple intermediates up = functional bottleneck; which ones hint at cofactor needs — B-vitamins, magnesium, CoQ10, carnitine). 3-methylglutaric/3-hydroxy-3-methylglutaric relate to HMG/CoQ10 pathway stress. Correlate with documented fatigue.
6. **Neurotransmitter metabolites.** HVA (dopamine turnover), VMA (norepinephrine/epinephrine), HVA:VMA ratio (elevated → DBH inhibition — Clostridia, copper status, vitamin C); 5-HIAA (serotonin turnover — SSRIs and 5-HTP/tryptophan in the med list shift it; note interference); quinolinic and kynurenic (inflammation-shifted tryptophan metabolism — high quinolinic:5-HIAA suggests neuroinflammatory pressure).
7. **B-vitamin, methylation, and detox markers.** Methylmalonic (functional B12 — cross-check serum B12 in `labResults`), FIGLU (folate), xanthurenic/kynurenic (B6), 3-hydroxyisovaleric (biotin), pantothenic (B5), glutaric (B2/carnitine); pyroglutamic as glutathione-depletion signal; orotic (ammonia/urea-cycle stress — marked elevation is a conventional finding); 8-OHdG where reported (oxidative DNA stress).
8. **Fatty-acid oxidation block.** Adipic, suberic, sebacic, ethylmalonic, methylsuccinic — carnitine/riboflavin-dependent β-oxidation inefficiency vs fasting/MCT intake; distinguish using `dietAndLifestyle` and the supplement list.
9. **Synthesis.** Name convergent stories tied to the chart: candida block + oxalates + documented GI symptoms; Clostridia markers + high HVA:VMA + behavioral symptoms; Krebs + FA-oxidation elevations + fatigue (mitochondrial support picture); quinolinic shift + pyroglutamate + chronic inflammation. Cross-reference the GI-MAP where present. State confidence honestly — the OAT is a screening/pattern tool; recommend confirmation where a finding would change management.

## Red flags — escalate regardless of functional interpretation

Not exhaustive. High oxalic **with** elevated glyceric or glycolic (possible primary hyperoxaluria — nephrology/genetics referral); markedly elevated orotic acid (urea-cycle evaluation, especially with protein intolerance or neurological symptoms); patterns suggesting an inborn error of metabolism in a child (multiple extreme elevations across Krebs/FA-oxidation blocks — metabolic specialist referral); severe unexplained neurological or behavioral deterioration warrants conventional workup independent of any OAT finding. Put these first in the output.

## Output format

1. **Snapshot** — one paragraph: dominant pattern and the single most important finding.
2. **Validity notes** — diet/supplement/collection caveats from the chart, stated first.
3. **Findings by block** — table: Block | Marker | Value | Age-adjusted range | Flag | Note.
4. **Pattern synthesis** — convergent stories across blocks and against the GI-MAP where present, with confidence stated.
5. **Red flags** — or an explicit "none identified against the checked list".
6. **Considerations for the practitioner** — confirmatory testing (stool testing, serum B12/MMA, RBC nutrients), cofactor-support directions as options, retest timing (typically 3–6 months). Options, never directives.
7. **Patient-friendly summary** — only if asked.

If the practitioner wants it saved, save with `create_note` (a lab review is not a clinical note or care plan).

End with: "Decision support only — for review by the treating clinician."
