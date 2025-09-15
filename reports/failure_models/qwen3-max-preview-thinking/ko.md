# Failure Model — Qwen 3 Max Preview — Korean

## Summary
Overall, Qwen 3 Max Preview produces fluent Korean but shows brittle fidelity under entity/term pressure and when style/register is extreme. The most salient weaknesses are: (1) corruption of proper nouns and technical terms, (2) unit/number drift and unprompted conversions, (3) omissions and occasional polarity flips of key clauses, (4) register/tone normalization (formalizing colloquial or modernizing archaic), and (5) sporadic hallucinated details. One catastrophic case returned an API-like error message instead of a translation.

## Top Failure Modes
- Entity/Term drift (proper nouns, species, roles, game/legal/tech terms) — Names, titles, and precise domain terms are frequently substituted with near-neighbors or generic synonyms, altering facts.
  - Why: Korean transliteration ambiguity, weak entity grounding, bias toward high-frequency synonyms, and back-translation ambiguity (plurality/capitalization lost).
  - Frequency: common; Severity: high
  - Evidence: “astrolabe” → “sextant”; “Ruiz” → “Lewis,” street type altered; species swaps like red knot→redshank; legal “venue”→“jurisdiction.”

- Units and numerals tampering — Adds Celsius conversions, flips Fahrenheit/Celsius, and alters durations/counts.
  - Why: Over-helpful normalization, unit-conversion priors in training data, number copying errors during generation.
  - Frequency: occasional–common; Severity: high when technical/safety-critical
  - Evidence: “40–70°F” rendered with Celsius or changed to °C; “three weeks” → “three months.”

- Critical meaning inversion (polarity/logic) — Reverses mechanism bias, flips ease/possibility, changes hazard class.
  - Why: Long-distance negation handling in Korean (topic/subject drop), antonymal synonym choice, and misreading passive/causative forms.
  - Frequency: occasional; Severity: high
  - Evidence: lock “biased to locked” → “biased to unlocked”; “combustible” → “flammable”; “easy to forget” → “hard to forget.”

- Content omission — Drops paragraphs or key details (ingredients, ages, attributions) while keeping structure smooth.
  - Why: Length control bias, compression of repeated/parenthetical info, difficulty with list-like detail density.
  - Frequency: occasional; Severity: medium–high
  - Evidence: glacier paragraph missing; omitted age/relatives; missing “bok choy.”

- Hallucinated additions and embellishments — Inserts plausible but absent details (diet, tools, “and more”), or over-clarifies procedures.
  - Why: Preference for “complete” explanations; Korean style pressures to explicate; decoder exploring high-probability continuations.
  - Frequency: occasional; Severity: medium
  - Evidence: adds “birds” to diet; adds rinsing step to recipe; adds extra tools (“shovel”) and phrases (“and more”).

- Register/voice flattening — Systematically shifts should↔must/may, formalizes casual speech, modernizes archaic legal/religious prose, and softens vivid imagery.
  - Why: Korean politeness/modality mapping ambiguity; lack of genre-conditioned decoding; training bias toward polite/neutral business Korean.
  - Frequency: common; Severity: medium
  - Evidence: “should” → “must”; archaic proclamation modernized; “offering” → “sacrifice” shift and broader formalization.

- Idiom and figurative language literalization — Misreads idioms/metaphors and poetic devices; substitutes conceptual near-terms.
  - Why: Cross-lingual idiom gaps, meter/rhyme untranslatability, preference for literal paraphrase.
  - Frequency: occasional; Severity: medium
  - Evidence: “leave it all on the field” rendered as “leave everything behind”; “parallax” → “time zones”; “meter” confused with “rhyme.”

- Catastrophic non-translation output — Returns an error/status message rather than a translation.
  - Why: Serving/tooling pathway leak or safety/meta guard triggering.
  - Frequency: rare; Severity: critical
  - Evidence: Entire output is an API-like error, unrelated to source.

## Language‑Specific Factors
- Transliteration ambiguity: Proper nouns in Hangul often map to multiple English spellings on back-translation (e.g., “Rin/Lin,” “Xylos/Silos”), encouraging name drift.
- Capitalization loss: Korean script lacks capitalization; proper/common noun distinctions may be lost and reinterpreted incorrectly when back-translating.
- Plurality and determiners: Korean often omits plural markers and articles; back-translation can flip singular/plural or specificity (e.g., “buildings” omitted, singularized).
- Modality/politeness: Korean modal auxiliaries and speech levels push “should” toward “must” or soften “will” to “may,” shifting compliance tone.
- Voice and causatives: Passive/causative constructions and subject/topic drop can invert agency or bias (e.g., mechanism “biased to locked”).
- Lexical gaps: Domain species/nautical/legal items lack common Korean equivalents; model picks near-domain synonyms (“bosun”→“captain,” bird species swaps).
- Poetic prosody: Korean prosody diverges; meter/rhyme mapping fails, leading to semantic substitutions and loss of scansion.

## Numbers, Units, and Formatting
- Units: Adds Celsius alongside Fahrenheit; converts F→C; swaps units without source cues.
- Durations/dates: Occasional span drift (“weeks”↔“months”).
- Counts/measurements: Ingredient quantities and cup sizes inverted; singular/plural mismatches.
- Conversions/inventions: Yes—temperature conversions and inserted notations appear unprompted.
- Tables/markup: No systematic table issues observed; minor field/label rewording in UI/legal forms.

## Meta/Disclaimer Behavior
- One instance of emitting an error/status message instead of a translation (catastrophic contamination).
- Policy/safety disclaimers not broadly observed otherwise.
- Trigger: Likely API/pathway glitch or internal error state surfaced to output.

## Recommendations
- Prompting
  - Freeze entities/units: “Translate verbatim. Do not convert units or add equivalents. Keep all numbers, dates, and temperatures exactly as written.”
  - Entity preservation: “Preserve all proper nouns and product/skill/species names in source form; if transliteration is required, also include the original in parentheses.”
  - Modality/voice guard: “Preserve modality (should/must/may), tense, and voice; do not strengthen or weaken requirements.”
  - Idiom handling: “If an idiom or poetic term is uncertain, retain source term in parentheses rather than substituting.”
  - Anti-omission: “Do not omit any sentences, clauses, or list items; preserve paragraph count and line breaks.”

- Decoding
  - Use deterministic decoding (temperature 0 or very low) to reduce synonym drift and inventions.
  - Constrain numerals/units with regex-guided decoding or post-hoc copy constraints.
  - Apply a glossary/term bank (NER-extracted) with high logit bias to enforce exact matches.

- Data
  - Fine-tune with parallel ko↔en corpora emphasizing:
    - Technical/legal/game/nautical lexicons and species lists.
    - Unit preservation test suites (no-conversion tasks).
    - Modality/negation/polarity minimal pairs (locked vs unlocked; should vs must).
    - Transliteration with back-mapping annotations (maintain source form in parentheses).
    - Poetry/idiom datasets with fidelity metrics, not just fluency.

- Post-processing
  - Verifier pass: Small QA model or rules to diff source vs translation for:
    - Unit/number mismatches; duration spans; polarity cues (not/un-).
    - Named-entity consistency (Levenshtein or dictionary match).
    - Modality shifts (should/must/may) and hazard class terms.
  - Species/term lookups against controlled vocabularies (birds, nautical roles, game terms).
  - Flag additions like “and more,” extra tools/ingredients, or added Celsius conversions.
  - Fallback on error text detection: if output matches error pattern, automatically reissue request.

## Confidence and Coverage
Confidence: medium-high for the identified top modes (entity/term drift, units, modality/polarity, omissions, tone normalization) due to multiple independent judges citing similar issues across genres. Coverage limits: Evidence is back-translation based and spans mixed domains; a single catastrophic non-translation was observed, so frequency may be undercounted. Poetry/legal/technical/game texts are well represented; tables/markdown and RTL/script mixing are scarcely represented, so formatting/script issues beyond units/numbers may be underexplored.