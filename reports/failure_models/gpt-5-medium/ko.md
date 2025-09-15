# Failure Model — GPT-5 (medium reasoning) — Korean

## Summary
Overall, GPT-5 (medium reasoning) produces meaning-faithful ko↔en translations but exhibits systematic drift in person, gender, plurality, tone/register, and domain terminology. The most salient weaknesses are: (1) person/gender resolution errors triggered by Korean zero pronouns and implicit subjects; (2) consistent tone and modality softening (must→should, formalization) and literary flattening; (3) term precision loss in technical/legal/jargon-heavy content; (4) small factual substitutions for specific nouns; and (5) occasional omissions/additions that compress or extrapolate details.

## Top Failure Modes
- Person and gender shifts in dialogue/narration — Korean often omits subjects and gender; the model reintroduces them inconsistently in back-translation, flipping I↔we/you and misgendering roles. Why: zero pronouns, lack of gendered pronouns in ko, and heuristic re-anchoring during back-translation. Frequency: common; Severity: high.
  - Evidence: “we→I” and misgendered mayor; multiple lines swap “They said→You said,” “Should I→Should we,” altering who speaks/acts.

- Modality and register softening — Imperatives/obligations are weakened (must→should; shall→is), and tone shifts toward neutral/formal prose; poetic voice flattens. Why: ko polite forms map to a range of English modalities; model biases toward safe/formal registers. Frequency: common; Severity: medium.
  - Evidence: Evacuation order “must evacuate→should”; poem “laughed→smiled,” imperatives rendered as requests.

- Terminology drift in domain-specific content — Substitution of near-synonyms reduces precision (admission control→authorization controls; combustible→flammable; Perk→Trait; heavy timber/handline lost; “salvage courier”→“relief-goods hauler”). Why: insufficient term locking, ko compounding ambiguity, preference for more common terms. Frequency: common; Severity: medium–high (when safety/technical).
  - Evidence: “admission control” repeatedly generalized; hazard class changed “combustible→flammable”.

- Lexical substitutions for specific referents — Concrete fauna/objects and craft terms swap to neighbors (snipe→woodcock; midges→cranefly; tallow→resin; clay loam→sandy loam), and metaphors are reinterpreted. Why: rare-word mapping ambiguity, dictionary polysemy, poetic imagery normalization. Frequency: occasional; Severity: medium.
  - Evidence: bird/insect swaps; candle material misrendered; soil type error.

- Number, plurality, and scope shifts — Singular/plural toggles; role plurality narrowed; counts reframed; category scope narrowed. Why: Korean lacks plural marking by default; classifier/counter omissions; tendency to normalize. Frequency: occasional; Severity: medium.
  - Evidence: “Sellers/Buyers”→“Seller/Buyer”; “47 others”→“47 houses”; summaries→summary; generators plural→singular.

- Omission or addition of scene/details — Dropping short images or adding small clarifiers that bend nuance. Why: compression under ambiguity; coherence-driven elaboration. Frequency: occasional; Severity: medium.
  - Evidence: omission of “man/camel/oranges” scene; added “sancha”; added “do not stop” instruction.

- Voice/genre drift and idiom domestication — Literary diction and idioms rendered literally or paraphrased; game/marketing/legal genres lose native jargon/puns. Why: default paraphrase tendency, safety/formality bias, limited genre-tuned data. Frequency: common; Severity: medium.
  - Evidence: “Spring into savings” lost; “bodega→corner store”; “inspire→lead,” pun lost.

## Language‑Specific Factors
- Zero pronouns and topic-drop: Korean omits subjects/objects; back-translation guesses speaker/agent, causing I/we/you flips and misattributions in dialogue.
- Gender neutrality: Korean lacks grammatical gender; titles (시장, etc.) map ambiguously; model misgenders recurring figures.
- Plural marking optionality: -들 often omitted; leads to singularization of legal parties and components.
- Honorifics/register: Politeness endings push the model to neutral/formal English, weakening imperatives and directness.
- Lexical compounding and polysemy: Noun compounds (e.g., 접근/승인/인증/허가) and near neighbors in technical domains cause term drift.
- Idiom/poetic compression: Korean idiomatic or metaphorical lines are normalized into plain prose in English, losing imagery and cadence.
- Word-order re-interpretation: Flexible SOV ordering and modifier stacking lead to attachment errors and small factual swaps in dense noun phrases.

## Numbers, Units, and Formatting
- Plurality and counts: frequent singular↔plural shifts; occasional scope narrowing of lists/sets.
- Hazard/standards terminology: classification drift (combustible vs flammable) without unit changes; risk severity altered.
- Tables/structured content: minor title/section softening; no consistent table/markup breakage reported.
- Dates/units: no systemic unit conversion errors observed; number inventions rare but scope/category reframings appear.

## Meta/Disclaimer Behavior
- Minimal policy disclaimers; rare meta-notes in operational docs slightly misframed scope (“failed primary” vs “failed-over primary”). Triggered by status/incident templates containing stock disclaimers; model paraphrases headers/notes.

## Recommendations
- Prompting
  - Enforce role/voice: “Preserve original person (I/you/we), speaker attributions, and gender; do not infer.” Require bracketed speaker tags for dialogue.
  - Modality lock: “Preserve modality and force words (must/shall/should) verbatim.”
  - Terminology guardrails: Provide glossary tables (ko↔en) for domains; instruct “Do not substitute listed terms.”
  - Style preservation: “Match register and genre; avoid paraphrase; keep idioms/metaphors literal unless nonsensical.”
  - Literal-first mode: “Translate with minimal rephrasing; no additions; mark any unavoidable paraphrase with [P].”
- Decoding
  - Lower temperature/top-p; increase repetition penalty for key terms; enable constrained decoding on glossary tokens and modal verbs.
  - Use lexically constrained decoding for named entities, numbers, titles, and hazard classes.
- Data and fine-tuning
  - Fine-tune on ko↔en dialogue with zero-pronoun resolution aligned to speaker tags; include scripts with dense pronoun drops.
  - Curate domain corpora with term-locked pairs (SRE, networking, NFPA/OSHA, legal contracts, firefighting, hiking/outdoors, games).
  - Add literary parallel data emphasizing imagery retention and cadence; augment with idiom-preserving examples.
  - Counterfactual training on minimal pairs for modality (must/should), hazard classes, and gendered titles.
- Post-processing
  - Consistency checker: Diff original vs back-translation for person/gender, numbers, plurals, named entities, and modals; flag deviations.
  - Terminology validator: Regex/ontology checks for glossary compliance; reject near-synonym substitutions.
  - Hazard/standards linter: Map to controlled vocab (e.g., “combustible” vs “flammable”) and block substitutions.
  - Dialogue attribution audit: Ensure line-by-line speaker preservation; detect shifts in second/first person.
  - Style meter: Penalize length inflation >10% and register drift in sensitive genres (poetry, legal, marketing).

## Confidence and Coverage
Confidence: medium-high. Evidence is dense and multi-judge with recurring patterns across literary, technical, legal, and operational texts. Limitations: few extreme low scores and limited exposure to numeric-heavy/tabular inputs; safety/meta issues appear rare. Potential bias toward English→Korean→English round-trip artifacts may overstate person/gender errors compared to single-pass translation.