# Failure Model — DeepSeek V3.1 Reasoner — Swahili

## Summary
Across low‑score cases, the model shows systematic meaning drift rather than surface fluency problems. The most salient weaknesses are: (1) unstable handling of times/dates and schedule semantics (AM/PM flips, hour inversions), (2) lexical substitution of key domain terms (culinary, technical, legal, gaming) with near‑neighbors or wrong hyponyms, (3) named‑entity and kinship/gender confusion, (4) frequent semantic polarity reversals (e.g., improvement↔deterioration; absorb↔take damage), and (5) stylistic/figurative flattening that erodes imagery and precise metaphors. These errors often trigger in contexts where Swahili has ambiguous lexical coverage or different temporal conventions.

## Top Failure Modes
- Time and schedule inversion — AM/PM flips, hour shifts, day/date swaps; why: Swahili time expressions and weak anchoring to 12‑hour clock, plus numeric diffusion; frequency: common; severity: high; evidence: “6:45 a.m.→12:45 p.m.; 5 a.m.→11 p.m.”; “9:00 AM→3:00 AM; museum hours inverted.”
- Domain‑term substitution (culinary and materials) — Key ingredients/terms swapped with wrong items (bay leaf→tamarind; red pepper flakes→black pepper; smoked→fried); why: sparse term coverage and lexical proximity bias in Swahili culinary lexicon; frequency: common; severity: high; evidence: “smoky→fried; bay leaf→tamarind”; “dry shampoo→hair dryer.”
- Technical/jargon drift (legal, finance, product/infra, sports) — Core terms softened/strengthened or replaced (e.g., consistent with→matches; chargebacks→declines; rate limiting→attendance control; half‑spaces→wide); why: limited parallel domain data; preference for high‑frequency near synonyms; frequency: common; severity: high; evidence: “stream graph→flow chart”; “MUST/SHOULD strength changed; lane drop→descending grade.”
- Semantic polarity reversals — Key meanings flipped (improvement↔deterioration; food security↔insecurity; absorb damage↔take damage; nod↔shake); why: antonym confusions and negative prefix handling; frequency: common; severity: high; evidence: “improvement→deterioration”; “child nodding→shaking head; basil→parsley.”
- Named‑entity and proper noun drift — Names modified or normalized (Kazi→Job; Lunaria→Klunaria), locations switched (Avenue↔Street); why: over‑normalization and transliteration pressure; frequency: common; severity: high; evidence: “5th Avenue→5th Street; Avenues→Streets”; “Kazi→Job; event names changed.”
- Kinship/gender/pronoun errors — Nephew↔uncle, gender swaps, person/attribution flips; why: gender‑neutral Swahili pronouns and ambiguous kinship mapping; frequency: occasional; severity: high; evidence: “nephew→uncle; times moved to AM”; “She→He; action attribution misassigned (photo taping).”
- Figurative language collapse and tone flattening — Metaphors re‑literalized or changed (“ox→cow”, “whisper→stump”), humor/poetry diluted; why: metaphor mapping scarcity and preference for literal renderings; frequency: common; severity: medium; evidence: “core metaphor altered (ox→cow)”; “poem imagery: ‘prophetic’→‘kinai’.”
- Additions/omissions and invention — Introduces alien motifs (“magic” for clockwork), adds facts (Fahrenheit), omits constraints (radish); why: over‑generative bias under uncertainty; frequency: occasional; severity: medium; evidence: “introduces ‘magic’ theme”; “ingredient omitted; added ‘for the last time’.”
- Numbers/units drift — Monetary figures and capacity/volume converted or miscopied ($41M↔$38M; gallon↔liter); why: heuristic normalization and auto‑conversion; frequency: occasional; severity: high in factual contexts; evidence: “$41M vs $38M”; “gallon→liter changes meaning.”

## Language‑Specific Factors
- Time system mismatch: Swahili commonly uses contextual dayparts or the “saa” system offset from Western clocks; weak AM/PM anchoring increases flips (asubuhi/jioni/usiku markers inconsistently mapped).
- Gender/kinship ambiguity: Swahili pronouns don’t encode gender, and kinship terms (mjomba/mpwa) depend on perspective; the model often guesses incorrectly, causing uncle↔nephew swaps.
- Lexical gaps and loanwords: Some culinary/technical items lack widely standardized Swahili terms (e.g., bay leaf, red pepper flakes), encouraging approximate or culturally adjacent substitutions.
- Definiteness and role attribution: Swahili’s article‑less noun phrases and frequent subject drop can blur agent/patient roles, leading to action misattribution in narratives.
- Morphology and noun classes: Agreement errors are less flagged than meaning, but class‑based derivations can nudge the model toward semantically nearby but wrong nouns (security↔insecurity).

## Numbers, Units, and Formatting
- Numeral integrity problems: money amounts and counts drift ($38M↔$41M; 312↔512 properties).
- Units auto‑converted or replaced (gallon→liter) without textual warrant.
- Calendar/clock format confusion: AM/PM toggling; morning/evening swapped; weekday/date shifts.
- Tables/lists mostly preserved structurally, but field names are reworded, weakening technical precision (e.g., rate limiting term replaced).

## Meta/Disclaimer Behavior
- Rare but present: occasional policy/disclaimer phrasing and narratorial intrusions in planning/coordination texts (“identifiers” for disclaimers; added summaries). Triggers include task briefs mentioning policies or footnotes; impact is minor compared to semantic errors.

## Recommendations
- Prompting
  - Enforce term and number fidelity: “Preserve all named entities, numbers, times, and units exactly. If unsure, copy source token.”
  - Add domain glossaries inline: “Use this glossary; if a term is unknown, leave it in source language in brackets.”
  - Temporal guardrails: “Do not infer AM/PM. Keep times and dates exactly as written; no conversions.”
  - Polarity check: “Do not invert risk/benefit or positive/negative attributes. If unsure, mark [UNCLEAR].”
- Decoding
  - Lower temperature/top‑p to reduce inventive substitutions.
  - Add copy‑bias or constrained decoding for entities, numbers, and unit tokens.
- Data and fine‑tuning
  - Fine‑tune on high‑quality Swahili parallel corpora in targeted domains (culinary, sports tactics, gaming, legal/finance, infrastructure).
  - Curate Swahili lexicons for ingredients/spices, technical standards (transport, payments, security), and kinship mappings with perspective notes.
  - Augment with time expression mappings (AM/PM, saa system) and contrastive pairs (security vs insecurity, absorb vs take) to penalize polarity flips.
- Post‑processing QA
  - Deterministic validators: regex checks for unchanged numbers, times, dates, units, and named entities.
  - Polarity/antonym scanner for high‑risk terms (improve/worsen, absorb/take, security/insecurity).
  - Kinship/gender consistency rules based on local referents; flag uncle/nephew and pronoun switches.
  - Terminology diff against a provided glossary; highlight OOVs and enforce source retention.
- Workflow
  - For poetic/figurative text, use a two‑pass approach: literal pass with bracketed metaphors, then stylistic smoothing while preserving mapped images.
  - For high‑stakes technical content, require bilingual review or back‑translation diff with automated flags before release.

## Confidence and Coverage
- Confidence: medium‑high for the top failure modes (time inversions, domain‑term substitutions, polarity flips, entity drift), supported by numerous independent judges across varied samples.
- Coverage limits: Evidence skews toward domain‑dense texts (technical, culinary, gaming, finance) and stylized prose/poetry; fewer pure conversational or low‑domain samples. Some observations on Swahili‑specific triggers (time, kinship) inferred from recurring symptoms rather than explicit source segments.