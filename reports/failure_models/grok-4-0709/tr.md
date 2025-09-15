# Failure Model — Grok 4 — Turkish

## Summary
Grok 4’s Turkish round‑trip translations are generally faithful but exhibit recurrent fidelity leaks: register flattening, precision drift in domain terms, gender/personification losses, and sporadic meaning inversions involving negation, modality, or ratios. Weaknesses concentrate around Turkish-specific ambiguities (genderless pronouns, idioms, gesture verbs) and normalization habits (time/percent formats). The most costly issues: (1) register/terminology softening in technical/legal text, (2) species/term substitutions and domain lexicon swaps, (3) gender/personification and role errors, (4) numeric/modality inversions, and (5) occasional foreign‑token leakage or structural glitches.

## Top Failure Modes
- Register flattening and jargon dilution — Technical/legal/poetic registers are softened (specific terms replaced by generic synonyms); why: preference for high‑probability paraphrases and Turkish’s tolerance for abstract synonyms; frequency: common; severity: medium-high.
  - Evidence: “tail latencies”→“queue delays” in an RFC; legal “minute order”→“minute entry.”
  - Evidence: “cloud‑native”→“cloud‑based”; museum/printing “impression”→“print.”

- Domain term/label substitutions (species, nautical, ag/med) — Specific nouns swapped within same semantic field; why: sparse domain mappings in TR data and synonym over‑generalization; frequency: common; severity: medium.
  - Evidence: bird species swapped (curlew↔lapwing; puffin→seagull); “damping‑off” misread as “powdery mildew.”
  - Evidence: “bosun”→“sheet chief”; “chronograph”→“chronometer.”

- Gender/personification loss and role drift — Character gender flips; personified “she” for objects defaulted to “it”; job titles/ranks altered; why: Turkish lacks gender marking and permits pro‑drop; back‑translation re‑imposes defaults; frequency: common; severity: medium.
  - Evidence: protagonist’s gender “he↔she” toggled; lighthouse “she”→“it.”
  - Evidence: rank/title shifts (“Lieutenant Commander”→“Lieutenant Colonel”; “Magistrate”→“Justice of the Peace”).

- Modality and polarity errors (negation, MUST/SHOULD, idioms) — Inversions of requirement strength or yes/no; idioms negated or literalized; why: modality keywords and idioms map imperfectly to Turkish; frequency: occasional; severity: high.
  - Evidence: RFC “SHOULD”→“must”; “at my wits’ end” became a negated odd idiom.
  - Evidence: child’s nods rendered as head‑shakes; “not pecking” inserted against context.

- Numeric and ratio misunderstandings; format normalization — Ratios misinterpreted; hazard classes conflated; percent/time formats normalized; why: TR number conventions (% symbol placement, 24‑hour time), term ambiguity (yanıcı); frequency: occasional; severity: high.
  - Evidence: “three to one” rendered as “by a third”; “combustible”→“flammable.”
  - Evidence: “%90” and “≥%55” symbol/placement errors; 12‑hour to 24‑hour time shift.

- Causality/timeline reversals and swap errors in parallel contrasts — Reversed tradeoffs, causes, or sequences; why: complex clauses + SOV reordering; frequency: occasional; severity: high.
  - Evidence: eutrophication cause reversed; “traded equity for altitude” inverted.

- Structural/list integrity breaks — Component lists and claims occasionally mangled or with omissions; why: sentence re‑packing and list detection errors; frequency: occasional; severity: medium.
  - Evidence: patent claim 1 structure “nonsensical phrase”; parts list dropped “brackets.”

- Foreign‑token leakage and named‑entity drift — Chinese token appears; Turkish month name left; brand/taglines altered; why: training contamination and aggressive paraphrase; frequency: rare; severity: low-medium.
  - Evidence: stray “偶尔”; “Eylül” kept in English context; “Consciously Crafted”→“Mindfully Produced.”

## Language‑Specific Factors
- Genderless pronouns and pro‑drop: Turkish lacks grammatical gender; implicit subjects encourage gender defaulting or flips on back‑translation; personification often lost.
- Flexible word order (SOV) and clausal embedding: increases risk of scope, causality, and contrast inversions when re‑linearizing.
- Idioms and gesture verbs: “başını sallamak” can be ambiguous; idioms like “wits’ end” lack direct calques and get negated or literalized.
- Derivational compounding and domain calques: terms like “yanıcı” cover combustible/flammable; “ölçek/gauge/kalınlık” confusions; legal/nautical calques easily drift.
- Style/register: Turkish encourages synonymy and nominalizations, leading to tone flattening in poetry and precision loss in legal/technical prose.
- Format conventions: percent sign precedes number in TR (%90), 24‑hour time common; automatic normalization shifts the source style.

## Numbers, Units, and Formatting
- Percent symbol placement and symbol errors: % position flipped or malformed (e.g., “≥%55”); occasional “%90.”
- Ratios and counts: “three to one” misread as “by a third”; anecdotal number changed.
- Time formats: 12‑hour changed to 24‑hour; Z‑time phrasing altered (“become west by 09Z”→“west through 09Z”).
- Hazard/standards classes: combustible vs flammable conflated; RFC keyword capitalization and MUST/SHOULD modality shifted.
- Headings/labels: section titles softened (“Significance”→“Importance”); “checkout”→“payment.”

## Meta/Disclaimer Behavior
- No systematic safety disclaimers or policy intrusions observed. Rare foreign‑token leakage into output in otherwise English context (e.g., stray Chinese word; Turkish month name) when translating mixed content.

## Recommendations
- Prompting
  - Instruct: “Preserve modality (MUST/SHOULD), numbers, units, dates, and time formats exactly as in source. Do not normalize.” 
  - Provide a locked glossary per domain (e.g., RFC, legal, medical, nautical, ornithology). Add: “Use these terms verbatim.”
  - Add constraints: “Maintain character genders and personifications; if ambiguous in Turkish, retain original gender in back‑translation.”
  - For idioms/gestures: “Translate idioms idiomatically; avoid negation unless present. Map nod/shake explicitly.”
  - Force structure: “Keep list counts and item order; do not merge or drop items.”
- Decoding
  - Lower temperature and increase length penalties modestly for technical/legal inputs to reduce paraphrase and register drift.
  - Enable constrained decoding for terminology (lexically constrained beam with glossary).
- Data augmentation
  - Expand EN↔TR parallel corpora for: RFC/spec language (MUST/SHOULD), hazard classes, legal procedures, nautical/ornithology/agriculture lexicons, medical/psych terms.
  - Curate contrastive pairs for negation, ratios, and tradeoff clauses (equity/altitude, cause/effect).
  - Include style-preservation pairs (poetry/literary) to penalize tone flattening.
- Fine‑tuning targets
  - Objective for modality fidelity and numeric invariance (copy‑exact loss on numbers/units/timestamps).
  - Disambiguation heads for gender/personification and gesture polarity; tie to evaluation on nod vs shake and personified inanimates.
  - Term consistency head across document windows (e.g., “chronograph” vs “chronometer”).
- Post‑processing checks
  - Numeric/units diff checker (ratios, %, time, ranks); flag any change.
  - Modality linter for RFC/legal keywords and hazard classes.
  - NER/terminology consistency validator with domain glossaries (species, nautical ranks, legal titles).
  - Structure validator for lists/claims (item counts; missing tokens).
  - Foreign‑token detector to block stray non‑TR/EN tokens.
  - Optional style checker to preserve capitalization and taglines/branding.

## Confidence and Coverage
- Confidence: medium-high. Dozens of low‑scored rationales across varied domains consistently show the outlined patterns.
- Coverage limits: Evidence skews toward technical/legal, nature/nautical, and literary texts; fewer data points on code, tables, or highly formulaic financial statements. Judges are diverse models; one item had missing rationale.