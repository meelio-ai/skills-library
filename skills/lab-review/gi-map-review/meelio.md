# GI-MAP Review

Decision support for licensed healthcare professionals. Analysis only — the practitioner reviews, verifies, and decides.

## Gather context (before interpreting anything)

1. Call `get_patient_documents` to locate the GI-MAP report (category `lab_report`) and its structured `labFindings[]`, **and** `search_patient_documents` for the raw report text — quantities with exponents (e.g. `2.3e4`), detection thresholds, and the lab's interpretive commentary that extraction misses. If no GI-MAP exists in the record, say so and stop.
2. Call `get_patient_snapshot` for `medicalHistory` (conditions, medications — especially PPIs and recent antibiotics — supplements, probiotics), `dietAndLifestyle`, and `recentEncounters` (GI symptoms discussed).
3. Date everything by **specimen/collection date** (`eventDate`), not report date. If prior GI-MAPs exist, build marker trends.
4. Skip anything marked `contradicted`. If history was truncated, write "history shown may be incomplete" — never "no history".

## Interpretation procedure

Work in this order — triage before terrain:

1. **Pathogens panel.** Any detected bacterial (C. difficile toxin A/B, Campylobacter, Salmonella, Shigella, STEC/E. coli O157, Vibrio, Yersinia), parasitic (Cryptosporidium, Entamoeba histolytica, Giardia) or viral pathogen leads the report. Distinguish toxin genes detected vs organism presence (C. diff toxin genes without symptoms may be colonization). Correlate with documented symptoms before attributing significance; state which findings warrant conventional management.
2. **H. pylori.** Quantity vs the lab threshold (~1e3) and every reported virulence factor (cagA, vacA, others). Detected virulence factors raise significance. Correlate with upper-GI symptoms, PPI use (suppresses detection), and family history of gastric disease. Eradication decisions are conventional-medicine territory — frame accordingly.
3. **Commensal/keystone bacteria.** Low Akkermansia muciniphila (mucin/barrier), low Faecalibacterium prausnitzii (butyrate/anti-inflammatory), and broadly low commensals suggest depleted terrain — often post-antibiotic or low-fiber (check `dietAndLifestyle`). Bacteroidetes/Firmicutes shifts are weak signals; don't over-read.
4. **Opportunistic/overgrowth.** Cluster, don't list: multiple elevated Proteobacteria (Klebsiella, Citrobacter, Proteus, Pseudomonas, Morganella) → general dysbiosis/insufficient digestion; Methanobrevibacter smithii → constipation/methane (IMO) picture; Desulfovibrio (H₂S) → urgency/loose stool; Staph/Strep overgrowth → low stomach acid signal.
5. **Fungi and parasites.** Stool Candida is insecure (intermittent shedding — a negative doesn't exclude it; suggest OAT arabinose correlation if an OAT exists in the record). Blastocystis hominis and Dientamoeba fragilis are of debated pathogenicity — significance depends on symptoms and the rest of the report; say so explicitly.
6. **Intestinal health markers — the interpretive core:**
   - *Elastase-1:* <200 µg/g suggests pancreatic insufficiency; 200–500 borderline — correlate with steatocrit, bloating, stomach-acid signals.
   - *Steatocrit:* elevated = fat maldigestion; read with elastase (pancreatic) vs bile-flow context.
   - *β-glucuronidase:* elevated → deconjugation/enterohepatic recirculation of estrogens and toxins; cross-reference hormonal symptoms or a DUTCH report if one exists in the documents.
   - *Occult blood:* any elevation is a conventional finding — see red flags.
   - *Secretory IgA:* low = depleted mucosal immunity (chronic stress/infection); high = active immune engagement. Either direction contextualizes the rest.
   - *Anti-gliadin IgA:* gluten immune reactivity (not celiac testing — say so); low sIgA can mask it.
   - *Calprotectin:* the inflammation arbiter — see red flags.
   - *Zonulin (add-on):* permeability signal; supportive, never diagnostic alone.
7. **Pattern synthesis.** Name the story the whole report tells: low elastase + Staph/Strep + H. pylori → hypochlorhydria cascade; low keystones + multiple opportunists after documented antibiotics → depleted-terrain dysbiosis; high β-glucuronidase + estrogen symptoms → recirculation pattern; methanogens + constipation → IMO picture. Present sequencing (triage pathogens/overgrowth before rebuild; support digestion before antimicrobials) as a consideration, not a protocol.
8. **Adjust for who the patient is** — use `demographics` and `medicalHistory`. The lab's quantitative ranges are adult-derived — flag pediatric samples and interpret conservatively; commensal abundance shifts with age. Read β-glucuronidase's estrogen-recirculation implications against sex, menopausal status, and any HRT in the medication list (a cycling patient with estrogen-dominance symptoms vs a postmenopausal patient on HRT vs a male are different conversations). New-onset bowel symptoms over ~50 lower the threshold for conventional workup regardless of the report. In immunocompromised patients (conditions/medications), treat "opportunists" with more respect and escalate sooner.
9. **Correlate with the chart** — documented symptoms, medications, diet, and prior GI-MAPs for trend. Note provenance when a correlation rests on transcription- or document-sourced data.

## Red flags — escalate regardless of functional interpretation

Not exhaustive. Calprotectin >173 µg/g (per lab cutoff) — and persistently 50–173 with symptoms — warrants IBD workup consideration (GI referral/colonoscopy); any positive occult blood in an adult warrants conventional evaluation; Entamoeba histolytica or symptomatic C. difficile toxin positivity warrants conventional management; alarm features in the chart (unintentional weight loss, blood in stool, nocturnal diarrhea, fever, new-onset symptoms over ~50, iron-deficiency anemia) override a benign-looking report. Put these first in the output.

## Output format

1. **Snapshot** — one paragraph: the overall pattern and the single most important finding.
2. **Red flags** — first, or an explicit "none identified against the checked list".
3. **Section-by-section table** — Section | Marker | Result | Lab threshold | Flag | Clinical note.
4. **Pattern synthesis** — the story of the report in prose.
5. **Correlations with the chart** — what fits, what doesn't, with sources.
6. **Considerations for the practitioner** — sequencing considerations, confirmatory/adjunct testing (SIBO breath test, OAT, celiac serology, GI referral), retest timing (typically 8–12 weeks after intervention). Options, never directives.
7. **Patient-friendly summary** — only if asked.

If the practitioner wants it saved, save with `create_note` (a lab review is not a clinical note or care plan).

End with: "Decision support only — for review by the treating clinician."
