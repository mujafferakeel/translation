# Failure Model — DeepSeek V3.1 Reasoner — Arabic

## Summary
Across low-scoring round‑trip cases, DeepSeek V3.1 Reasoner shows strong structural fidelity but recurrent semantic drift in small yet meaning‑critical items. The most salient weaknesses are: (1) antonym/reversal errors on short actions and labels; (2) proper‑noun and title drift (spelling, role, object class); (3) domain‑term normalization (sci‑fi, legal, technical) that substitutes near‑neighbors; (4) metric/spec inversions and slot swaps; and (5) gender/pronoun forcing from English neutral to Arabic masculine. Occasional artifacts (untranslated words, Chinese/Russian inserts) and duplicated lines appear. Tone often flattens, and specific jargon is generalized.

## Top Failure Modes
- Antonym/gesture reversals — nod↔shake, accept↔reject; flips consent/intent in scenes. Why: short polarity cues/negation and Arabic verb/particle ambiguity under-determine reference; decoder favors frequent collocations. Frequency: common; Severity: high. Evidence: “nods→shakes head” breaks a toppings consent; “flinched→tensed” shifts reaction.

- Proper names and titles drift — spelling changes, honorifics, titles, roles. Why: oscillation between transliteration and normalization, unstable mapping back from Arabic; diacritics absent increases ambiguity. Frequency: common; Severity: high. Evidence: “Feld→Field; Mme→Mlle”; “Elara→Ilara”; “Gentleman→Nobleman”.

- Object/class misidentification — category substituted with a confusable neighbor. Why: semantic nearest‑neighbor bias and lexical gaps; Arabic back/forward passes re-map to higher‑frequency classes. Frequency: occasional; Severity: high. Evidence: “Landscape→Still life”; “wheeled lunar rover→winged asteroid probe”.

- Technical/spec inversions and label swaps — efficiency values swapped, dimensions order flipped, part names misassigned. Why: RTL/LTR mixing for numerals, idāfa/head–modifier order, term polysemy; low tolerance to near‑synonyms in specs. Frequency: occasional; Severity: high. Evidence: “hydraulic vs motor efficiency swapped”; “impeller hub↔shaft; flooded vs submersible suction”.

- Domain term normalization (sci‑fi/engineering/legal) — substitutes specific terms with generic or wrong neighbors. Why: low domain grounding; mapping to more common Arabic paraphrases; glossary not enforced. Frequency: common; Severity: medium–high. Evidence: “Stardate→Astronomical Time; subspace→subatomic space”; “minute order→session minutes”; “checkout→payment”.

- Gender/pronoun forcing — neutral “they” → masculine “he”; added genders. Why: Arabic requires gender; model defaults to masculine when antecedent unclear. Frequency: common; Severity: medium. Evidence: artist misgendered; genders added to unspecified patrons.

- Jargon/style flattening — sports/legal/UX/culinary jargon replaced by generic terms; idiomatic punch lost. Why: preference for standard Arabic registers; avoidance of dialectal/technical variants. Frequency: common; Severity: medium. Evidence: basketball “guard→goalkeeper”; UX “onboarding→joining”; cooking “seared→roasted”.

- Additions, omissions, and duplication — stray insertions, dropped items, repeated lines. Why: decoding instability under long context; attention slips; copy noise. Frequency: occasional; Severity: medium. Evidence: duplicated final paragraph; grills “two→one”; added “god of the sky”.

- Foreign‑script leakage / untranslated tokens — Chinese/Russian/Italian residues. Why: training data contamination; inadequate language gating; beam carries source artifacts. Frequency: occasional; Severity: medium. Evidence: stray “对接”, “早已”, Russian words mid‑paragraph.

- Place/term precision loss — geographic names, conservation areas, rock/site names slightly altered. Why: transliteration ambiguity and lexical proximity. Frequency: occasional; Severity: medium. Evidence: “Copper Gorge→Cooper Gorge”; “Conservancy→Reserve”; “Mukawwa→Makkah”.

## Language‑Specific Factors
- Gender inflection required: Arabic forces gender on verbs/adjectives; English neutral subjects map to masculine by default, leading to misgendering or invented genders.
- Diacritic omission: Unvowelled Arabic amplifies ambiguity on short verbs and minimal pairs (e.g., nod vs shake), raising reversal risk on re-translation.
- Head–modifier order (iḍāfa): Misattachment can swap which noun is the head (e.g., “motor efficiency” vs “hydraulic efficiency”).
- RTL text with LTR numerals: Mixed directionality increases chances of dimension order flips and slot/holes confusion.
- Transliteration instability: Proper nouns and specialized terms drift across passes; the definite article “الـ” can be misinterpreted as part of names on back‑translation.
- Register and diglossia: Preference for Modern Standard Arabic smooths slang/onomatopoeia and domain‑specific jargon, flattening tone.

## Numbers, Units, and Formatting
- Swapped or misattributed metrics: hydraulic/motor efficiency, dimensions order, hub/shaft labels, “Hour nine→Nine o’clock.”
- Team/club names and acronyms altered; minor numeral style changes; occasional unit inventions.
- Tables/lists: slot vs hole, array vs cluster misreadings suggest structure confusion.
- Conversions are rare; inventions occur occasionally when a term seems incomplete (“widespread pitting” added to surface description).

## Meta/Disclaimer Behavior
- Occasional authorial intrusions: “translator’s note” remnants and evaluative adjectives (“amusing,” “infamous”).
- Foreign language leakage (Chinese/Russian/Italian) appears sporadically, often around named entities or technical terms.
- Safety/policy disclaimers were not a systematic issue in this set.

## Recommendations
- Prompting
  - Enforce invariants: “Do not add, omit, or explain. Preserve named entities, numbers, and titles exactly. If uncertain, transliterate and mark with [·] rather than substitute.”
  - Provide a mini‑glossary per domain: e.g., sci‑fi (Stardate, subspace), legal (minute order, sanctions roles), technical (impeller hub/shaft), sports (guard, bank shot).
  - Gender control: “Preserve original pronoun/gender; if neutral in source, keep neutral wording in Arabic (avoid gendered additions).”
  - Numerics/layout guardrail: “Keep numbers/units and their order unchanged; do not reorder dimensions.”

- Decoding
  - Lower temperature and reduce sampling for high‑precision segments (specs, names, quotes); use constrained decoding with a protected lexicon for entities/terms.
  - Segment-by-semantics decoding: translate specs/tables in structured mode (key:value lines) to minimize order/inversion errors.

- Data and Fine‑tuning
  - Fine‑tune on parallel EN↔AR corpora emphasizing:
    - Minimal‑pair reversals (nod/shake; accept/reject).
    - Gender‑neutral renderings in Arabic without forced gender.
    - Domain glossaries (sci‑fi, legal, engineering, sports, culinary).
    - RTL/LTR numerals with dimension orders and technical schematics.
  - Augment with round‑trip hard negatives (rover/probe; landscape/still life) and train a contrastive penalty for semantic category swaps.

- Post‑processing
  - Named‑entity and term audit: extract entities/terms from source; diff against output; flag edits for review.
  - Numbers/units checker: verify identical counts, orders, and unit strings; flag swaps or inserted units.
  - Polarity sentinel: run a lightweight NLI/entailment check on critical verbs/gestures; flag antonym risk.
  - Foreign‑token filter: detect non‑Arabic/English script leakage; strip/flag for manual correction.
  - Duplicate/omission scan: sentence alignment check to catch repeats or drops.

## Confidence and Coverage
- Confidence: medium‑high. Dozens of low‑score cases from diverse judges align on the same error families (reversals, name drift, term swaps, metric inversions, gender forcing), with multiple independent corroborations.
- Coverage limits: Source domains skew to narrative, sci‑fi, legal, and technical descriptions; fewer examples of chatty colloquial Arabic or code‑mixed dialects. Some findings (foreign‑script leakage) are occasional and may reflect dataset artifacts rather than core model behavior.