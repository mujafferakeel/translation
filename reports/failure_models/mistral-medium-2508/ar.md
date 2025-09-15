# Failure Model — Mistral Medium 3.1 — Arabic

## Summary
Overall: fluent but unstable on factual fidelity. The model systematically drifts on proper nouns and domain terms, frequently substitutes near-synonyms that alter technical meaning, and sometimes flips polarity/details (directions, roles, numeric magnitudes). Tone/register often gets “normalized,” and occasional meta/formatting intrusions appear. Most salient weaknesses: proper‑noun corruption/invention, jargon/taxonomy drift, numbers/time errors, action/polarity reversals, and register flattening.

## Top Failure Modes
- Proper‑noun drift and invention — Changes, misspellings, or fabricated names for people, places, abilities, and products; sometimes Arabicizes or “improves” spellings.
  - Why: Arabic lacks capitalization cues; diacritics omitted; transliteration uncertainty; decoding favors plausible Arabic forms over exact copying.
  - Frequency: common; Severity: high
  - Evidence: “Xylos→Zelos; multiple location/quest names altered”; “Beecher→Butcher; Mukawwa→‘Stone of the Iron’.”

- Domain term/jargon substitution (sports, legal, technical, gaming, archaeology, ecology) — Replaces precise terms with close but wrong categories.
  - Why: Sparse domain alignments and paraphrase bias in training; Arabic technical lexicons are inconsistent; model prefers generic, familiar terms.
  - Frequency: common; Severity: high
  - Evidence: “Guard→Goalie; cleats for sneakers; ‘shotgun’→‘pistol’”; “minute order→concise minute; ‘as‑is’→current condition; ‘eutrophication’ reinterpreted.”

- Numeric/time/measurement distortions — AM/PM ranges, percentages, depth/time cues altered or “rounded.”
  - Why: Weak copying constraints across RTL+Latin; normalization bias; Arabic‑Indic vs Latin numeral noise; tokenization around symbols.
  - Frequency: occasional; Severity: high
  - Evidence: “50% more vs 50% of damage”; “9PM–3AM→9PM–8AM”; “ankle‑deep→knee‑deep.”

- Action/polarity and directional reversals — Antonyms or reversed stage/field directions; head gestures, outcomes, or game states flipped.
  - Why: Ambiguity from undiacritized Arabic; metaphorical mappings over-literalized; low attention to small function words.
  - Frequency: occasional; Severity: high
  - Evidence: “nodding→shaking; speaker swap”; “downstage↔upstage”; “late consolation→late equalizer.”

- Taxonomy/material mislabeling (species, materials, food) — Substitutes near species/materials or wrong culinary items.
  - Why: Sparse Arabic taxonomic terms; high synonym pressure; hallucinated familiarity.
  - Frequency: common; Severity: medium
  - Evidence: “curlew→woodpeckers; snipe→bittern”; “fritware↔earthenware”; “sea bass→sea bream; charred octopus garbled.”

- Register and idiom normalization — Archaic/legal tone modernized; professional jargon flattened; idioms literalized or recast.
  - Why: Preference for generic MSA; limited exposure to specialized Arabic registers; safety/style tuning toward neutrality.
  - Frequency: common; Severity: medium
  - Evidence: “‘salvage’→‘salvation’ (register loss)”; “opt‑out→escape hatch; legal ‘venue’→jurisdiction.”

- Additions of meta/formatting and minor inventions — Injected headers (“Translation:”), notes, fixed placeholders (20XX→2020), extra clauses.
  - Why: Over‑helpfulness pattern from instruction tuning; formatting priors; eagerness to “polish.”
  - Frequency: occasional; Severity: medium
  - Evidence: “Added ‘Translation:’/‘Note:’ lines”; “20XX auto‑resolved to 2020.”

## Language‑Specific Factors
- No capitalization in Arabic encourages proper‑noun/common‑noun confusion; model then normalizes or renames entities.
- Undiacritized script yields homograph ambiguity, triggering wrong sense choices (e.g., inhale vs hold breath) and role/action flips.
- Transliteration uncertainty: multiple valid renderings of foreign names; model occasionally translates rather than copies or picks a familiar Arabic variant.
- Morphology and agreement: gender/number shifts can flatten specificity (dual/plural) and alter roles; determiners (al‑) encourage genericization.
- Borrowed lexicons are unstable across domains (sports formations, legal procedures, TTRPG mechanics), prompting generic Arabic substitutes.
- RTL with embedded Latin tokens/numerals can destabilize copying of times, percentages, and SKUs.

## Numbers, Units, and Formatting
- Percentages/times: off‑by‑sense (“more” vs “of”), wrong AM/PM endpoints, and unit “normalization.”
- Placeholders and dates: model “fixes” placeholders (20XX→specific year).
- Measurements: depth/phase misrenderings; occasional color/material substitutions interpreted as categories.
- Tables/headers: adds “Translation:” or bold headers; softens legal boilerplate formatting.
- Conversions/inventions: No systematic unit conversion, but occasional invention/expansion of ranges or descriptors.

## Meta/Disclaimer Behavior
- Occasional translator notes or “correction” comments inserted into output, especially when the source looks formal or when ambiguity is high.
- Triggered by document‑like inputs (memos, legal, email) or when the model detects inconsistencies/placeholders it “helpfully” resolves.

## Recommendations
- Prompting
  - Instruct: “Preserve all names, terms, numbers, dates, and units exactly as in source. Do not add notes, headers, or explanations. If uncertain, copy the term.”
  - Provide a short glossary per document (proper nouns, formations, legal terms) and require “copy‑through” for those spans.
  - Add constraints: “If a percentage/time range appears, copy numerals and AM/PM unchanged.”
- Decoding
  - Use low temperature (≤0.2) and disable sampling for high‑precision segments.
  - Constrained decoding with protected spans for entities/numbers; bias toward exact copy of Latin tokens and percentages.
- Data
  - Fine‑tune with Arabic parallel corpora emphasizing entity/number preservation and domain‑specific glossaries (sports, legal, ecology, gaming).
  - Augment with transliteration exemplars and negative examples penalizing noun/number drift and polarity flips.
  - Include undiacritized Arabic training with disambiguation supervision to reduce homograph errors.
- Post‑processing
  - Named‑entity and numeral diff checks against source; block if changes detected unless whitelisted.
  - Heuristics: gesture/direction/polarity checklist (nod/shake; upstage/downstage; equalizer/consolation; more/of).
  - Domain lint rules: formations (shotgun/pistol), legal boilerplate (minute order, as‑is, venue), mechanics (saving throws vs skill checks).
  - Glossary compliance validator; flag out‑of‑glossary substitutions.
- Workflow
  - For high‑stakes content, two‑pass translation: literal pass with entity/number lock; then stylistic smoothing pass constrained not to touch locked spans.
  - Human spot‑checks focused on entities, numbers, and polarity lines.

## Confidence and Coverage
- Confidence: medium. Evidence spans many judges and domains with consistent patterns of name drift, jargon substitution, and numeric/polarity errors. However, all observations come from round‑trip evaluation summaries, not direct source→Arabic audits, so some shifts may be amplified by back‑translation. Topic mix is broad; Arabic‑specific inferences are consistent with known script and transliteration challenges.