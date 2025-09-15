# Failure Model — GPT-5 (medium reasoning) — Turkish

## Summary
Overall, gpt-5-medium produces faithful round‑trip translations into/from Turkish, but it systematically softens or strengthens modality, dilutes domain‑specific terminology, and flattens tone—especially in poetic or highly technical/legal text. Turkish‑specific ambiguities (gender, plural/possessive, evidentiality) often surface as shifts on back‑translation. Less frequent but high‑impact issues include factual substitutions (carts→cars; larch→pines), unit/number glitches, and occasional meta insertions.

## Top Failure Modes
- Modality drift (SHOULD↔MUST; MUST↔SHOULD) — Strengthens or softens normative force in specs, policies, or guidance; often maps English modals to Turkish forms (e.g., “-meli/-malı,” “gerekir,” “zorundadır”) inconsistently, then back‑transfers incorrectly.
  - Why: Turkish lacks a one‑to‑one mapping for RFC‑style modals; model normalizes to common register or over-regularizes.
  - Frequency: common; Severity: high.
  - Evidence: “SHOULD→MUST” noted repeatedly in RFC sample; “must→should” softening in a guidance doc.

- Terminology precision loss (technical/legal/jargon) — Replaces precise terms with generic or adjacent synonyms; mixes near‑domain terms (e.g., cricket, hiking, legal).
  - Why: Insufficient term anchoring; bias toward high‑frequency Turkish paraphrases; missing domain glossaries.
  - Frequency: common; Severity: medium–high.
  - Evidence: “scrambles” rendered as “climbing pitches”; “oral argument” → “oral statements”; cricket terms (“pitch/stumps/bails”) → generic “strip/posts/bars”.

- Factual lexical substitutions — Concrete nouns misread or modernized (anachronisms) and key items swapped.
  - Why: Semantic neighborhood bias; preference for contemporary defaults; limited long‑range disambiguation.
  - Frequency: occasional; Severity: high.
  - Evidence: “carts” → “cars” in a historical setting; “larch” → “black pines”; “cork” → “mushrooms”.

- Numbers/units corruption and code‑switch artifacts — Units mistranslated or localized; numerals or ranges strengthened; leftover foreign tokens.
  - Why: Localization priors (imperial↔metric), tokenization over foreign unit strings, pattern completion on percentages.
  - Frequency: occasional; Severity: medium–high.
  - Evidence: “fifty feet” → “elli feet”; “ten pounds” → “ten libre”; “up to 90%” → “90%”.

- Tone/style flattening and register drift — Evocative/poetic diction becomes generic; professional registers shift (business/legal/scientific).
  - Why: Decoding preference for frequent paraphrases; Turkish SOV restructuring with style smoothing; safety toward neutral tone.
  - Frequency: common; Severity: medium.
  - Evidence: “geysered” → “shot”; “The energy was palpable” → “almost palpable”; slogans/wordplay dropped (“Spring into savings” lost).

- Gender/pronoun and role shifts — Neutralization or reassignment of gender/number; titles normalized.
  - Why: Turkish uses gender‑neutral “o”; possessive/plural morphology ambiguous; back‑translation guesses.
  - Frequency: occasional; Severity: medium.
  - Evidence: “her” → “they”; “Daddy/Papa” → “Father”; “their son” → “his sons”.

- Tense/aspect/evidentiality and certainty shifts — Present↔past flips; probability/precision strengthened.
  - Why: Turkish -DI vs -mIş vs -yor mapping; English evidentiality implicit; model regularizes.
  - Frequency: occasional; Severity: medium.
  - Evidence: Past progressive → simple past; “reduce by up to 90%” → “reduce by 90%”.

- Meta/disclaimer/translator notes intrusions — Adds explanations or glosses in text flow.
  - Why: Overhelpful translation behavior; training on annotated corpora.
  - Frequency: rare; Severity: low–medium.
  - Evidence: Added parenthetical “(kırağı)” and translator note; “prepare the footnote” misread plus note.

- Placeholder/localization leakage — Non‑English terms retained or Turkish terms left inside placeholders/examples.
  - Why: Template boundary detection weak; treats placeholders as translatable.
  - Frequency: rare; Severity: medium.
  - Evidence: “Non‑English terms in placeholders”; Turkish terms left in examples.

## Language‑Specific Factors
- Gender neutrality (o) and possessive/plural ambiguity: Encourages neutralization (“she/he”→“they”), or back‑translation misguesses number/gender (“their son”↔“his sons”).
- Modality mapping: English MUST/SHOULD vs Turkish “zorundadır/gerekir/-meli/-malı” is many‑to‑many; the model oscillates, causing strengthened/softened norms on return.
- Evidentiality and aspect: Turkish -DI vs -mIş vs -yor can imply certainty/source; back‑translations often shift certainty or tense.
- Agglutinative compounding and derivation: Technical compounds may be paraphrased into multiword generics, losing precision (“living hinge” → “flexible hinge”).
- Idioms/wordplay: Turkish often requires creative equivalence; model opts for safe literalism, losing puns/slogans.
- Word order (SOV) and definiteness: Reordering tends to degrade cadence and specificity in poetry and legal text.

## Numbers, Units, and Formatting
- Units: Occasional incorrect unit carryover or code‑switch (‘libre’), inch↔millimeter substitutions, imperial kept but corrupted (“elli feet”).
- Percentages/ranges: “up to X%” strengthened to “X%”; advisory precision replaced with certainty.
- Headings/labels: Titles normalized (“Medium” → “Intermediate”; “Non Goals” → “Out of Scope”).
- Placeholders: Language inside placeholders/examples sometimes translated or localized when it shouldn’t be.
- Dates/times: Minor normalizations (“by midday” → “by noon”).
- Conversions/inventions: No systematic conversions, but sporadic inventions/localizations appear; severity rises in technical/spec text.

## Meta/Disclaimer Behavior
- Occasional translator notes or parenthetical glosses inserted into the body, especially for rare words or poetic terms.
- Triggered by low‑frequency lexical items (e.g., “rime”) or when the model “helps” with clarity (“prepare the footnote” misinterpreted).

## Recommendations
- Prompting
  - For specs/legal/technical: “Preserve modality exactly (MUST/SHOULD/MAY). Do not soften/strengthen. Keep units, numerals, headings, and placeholders unchanged. Use provided glossary verbatim.”
  - For literary: “Preserve imagery, metaphors, and register; avoid simplification and rewording; maintain cadence.” Add: “Prefer literal renderings unless idiom demands.”
  - Gender fidelity: “Maintain explicit gender/number if present; do not neutralize or infer.” If needed: “Mark unknown gender as [unspecified].”
  - Placeholders: “Do not translate content inside {...}/<tags>/[PLACEHOLDER].”
- Decoding
  - Lower temperature/top‑p for technical/legal to reduce paraphrase drift.
  - Enable constrained decoding with lexicon locks for MUST/SHOULD, units, and key terms.
  - Penalize repetition‑avoidance that pushes synonym substitution in domain text.
- Data/Glossaries
  - Fine‑tune on EN↔TR parallel corpora for RFC/legal, safety sheets, cricket/nautical/hiking, and engineering manuals; include contrastive examples for modality and percentages (“up to” vs fixed).
  - Build TBX/CSV termbases for domains (e.g., “scramble,” “living hinge,” cricket parts) and enforce during decoding.
  - Add augmentation with unit/number stress tests (feet/meters, pounds/lira, ranges).
- Post‑processing QA
  - Modality checker: diff MUST/SHOULD/MAY across source and back‑translation.
  - Number/unit validator: detect altered numerals, ranges, and unexpected unit tokens; flag code‑switch artifacts.
  - Terminology QA: glossary compliance check; flag genericizations.
  - Gender/role consistency: track named entities/pronouns; warn on shifts.
  - Placeholder integrity: ensure placeholders unchanged.
  - Style guardrails: optional “poetry/literary meter preserved?” heuristic (cadence similarity) to catch flattening.
- Workflow
  - Route high‑risk genres (legal/spec/technical) through stricter settings and QA; allow slightly freer decoding for creative texts with a glossary‑backed style brief.

## Confidence and Coverage
- Confidence: medium‑high. The issues recur across diverse judges and samples, with consistent patterns (modality drift, term dilution, tone flattening, units/numbers).
- Coverage limits: Evidence skews toward English↔Turkish general, literary, technical/legal, and niche domains (sports, hiking, nautical); fewer data points for tables/markup‑heavy docs and code.