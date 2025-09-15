# Failure Model — Mistral Medium 3.1 — Chinese

## Summary
Overall, the model is fluent but prone to fidelity drift in Chinese, especially with domain terms, numerals/constraints, and proper nouns. The most salient weaknesses: (1) systematic substitution of key terms and named entities (sports, gaming, legal/tech, cooking); (2) number/unit and constraint errors (percentages, word vs character counts, ranks/quantities); (3) instruction-critical mistranslations (music fingering, safety steps); (4) tone/modal shifts that harden recommendations or soften legal nuance; (5) occasional semantic inversions and poetic/formal structure loss (rhyme scheme, tense). Errors often arise from paraphrase bias and over-normalization to common Chinese equivalents.

## Top Failure Modes
- Term drift and named-entity renaming — Swaps or rebrands key terms and proper nouns, altering facts (sports terms, game abilities, product specs, institutions).
  - Why: Preference for familiar Chinese equivalents or literalization; weak copy-through of NE tokens.
  - Frequency: common; Severity: high
  - Evidence: “equalizer” vs “consolation,” and 38%→48% in a match analysis; “Arcane Shield” changed to different effect (“50% more” vs “50% of damage”), location/boss names altered.

- Numbers, constraints, and units distortion — Percentages, counts, species/quantities, and format constraints altered.
  - Why: Paraphrase overprecision; confusion between Chinese character vs word metrics; unit normalization.
  - Frequency: common; Severity: high
  - Evidence: “300/100 words” rendered as “300/100 characters”; “40% off” became “80%”; “metric tons” shrank to “tons.”

- Instructional/technical misrendering — Critical procedural details and jargon drift (music fingering/hand position; safety seats; mechanical terms).
  - Why: Generic synonym substitution; insufficient domain grounding; Chinese ambiguity on instrument/technical lexicon.
  - Frequency: occasional; Severity: high
  - Evidence: Piano “thumb” became “index finger,” B-note description contradicted; “rear‑facing seat” became “rear row”; “buckling”→“bunching,” “lugs”→“latches.”

- Culinary and cultural substitution — Ingredients/dishes swapped or region-invented, changing recipes/content.
  - Why: Overfitting to familiar Chinese culinary terms; paraphrase aesthetic bias.
  - Frequency: occasional; Severity: medium
  - Evidence: “Lemon posset”→“lemon panna cotta”; “tamari glaze”→“yuzu glaze”; “beef shank”→“brisket,” added Sichuan peppercorn.

- Modal/tone and logic shifts — SHOULD→must, and/ or inversions, intensified or flattened tone, policy/requirements altered.
  - Why: Chinese modal ambiguity; preference for authoritative register in zh; paraphrase consolidation.
  - Frequency: occasional; Severity: medium
  - Evidence: Security guidance “SHOULD” strengthened to “must”; logical “and” rendered as “or”; “administration”→“administrative staff,” “Council/Charter”→“Union/Constitution.”

- Semantic inversion/misdirection — Reverses key relations or actions.
  - Why: Idiom/metaphor misread; over-abstract paraphrase.
  - Frequency: rare; Severity: high
  - Evidence: Street-light metaphor flipped (“swap its lights for sun” inverted); football “late consolation” misread as “equalizer.”

- Structure/format loss — Rhyme, tense, list/markup integrity degraded; added formatting.
  - Why: Optimization for fluency; Markdown auto-formatting; tense neutrality in Chinese.
  - Frequency: occasional; Severity: low–medium
  - Evidence: Rhyme scheme changed (ABBABABA→AABBABAB); present→past narrative drift; list reformatted into a sentence; bolding/markdown added.

## Language‑Specific Factors
- Word vs character metrics: Chinese often reports character counts; the model repeatedly converted “words” constraints to “characters,” altering task specs.
- Proper noun handling: Tendency to literal-translate or normalize institutional, place, and cultivar names (e.g., “Riverside Drive”→“Avenue”; cultivar “Ember Queen” literalized), reflecting Chinese translation conventions but harming fidelity when copy-through is required.
- Aspect/tense ambiguity: Chinese lacks overt tense, enabling unintended back-translation shifts (present→past) that change immediacy.
- Modal verbs and politeness: Chinese modal particles bias toward stronger obligation (“应/必须”), causing SHOULD→must escalations.
- Sports and game jargon: Chinese has fixed set terms; the model substitutes near-neighbors (“equalizer”/“consolation”; “second ball” vs “far post”), likely due to sparse domain anchoring.
- Idiom/poetic form: Rhyme and meter often sacrificed for semantic fluency; metaphors sometimes normalized or inverted in zh rendering.

## Numbers, Units, and Formatting
- Numbers: Frequent percentage and quantity changes (e.g., 38%→48%; 40%→80%).
- Units: Word→character, tons→metric tons ambiguity; rank/quantity shifts.
- Tables/markup: Occasional added markdown/bolding; list-to-sentence collapse; headings altered.
- Conversions/inventions: Adds “ever” to promo claims; reformats timelines; changes “sols” to “Lunar Days.”
- Risk profile: Common and high-impact when constraints or offers/prices are involved.

## Meta/Disclaimer Behavior
- Limited direct safety disclaimers observed. However:
  - Formatting intrusions: Added bolding/markdown or heading changes in technical/announcement contexts.
  - Register inflation: Formalization that reads like policy authorial voice, especially in security/legal texts.

## Recommendations
- Prompting
  - Use constraint locks: “Preserve all numbers, units, names, and ranks exactly; do not convert words↔characters.” Include a checklist: {NUMBERS}, {NAMES}, {UNITS}, {MODALS}, {TENSE}.
  - Provide glossaries/term locks with XML tags (e.g., <term lock>Arcane Shield</term>) and “Do not translate/alter locked terms.”
  - Ask for two-step output: (1) translation, (2) self‑audit diff listing all numbers, names, modals, and dish/ingredient terms with “changed/unchanged” flags.
  - Specify modal parity: “Translate SHOULD as 应当/宜, not 必须; preserve logical operators (and/or).”
  - For constraints: “Keep ‘words’ as words (not characters). If uncertain, copy ‘word(s)’ literally.”

- Decoding
  - Lower temperature/top‑p for high‑precision domains (legal, safety, specs, stats).
  - Enable constrained decoding on named entities and numerals (copy spans).

- Data
  - Augment zh parallel corpora with:
    - Domain glossaries: football analytics, gaming mechanics, child safety, lutherie/music pedagogy, mechanical engineering.
    - Counterfactual training focusing on “word vs character” distinctions and numeric fidelity stress tests.
    - Style-preservation sets (poetry rhyme/meter, archaic legal register).

- Fine‑tuning targets
  - A “Do‑Not‑Paraphrase” adapter for locked spans (NEs, numbers, units, ranks).
  - Modal calibration on security/legal standards (RFC SHOULD/MUST).
  - Recipe and craft instructions with ingredient/tool precision evaluation.

- Post‑processing
  - Automated checks:
    - Regex parity for all numerals/percentages and currency/discounts.
    - Named-entity copy check (string match against source list).
    - Unit/constraint validator (words vs characters; tons vs metric tons).
    - Modal diff (SHOULD/shall/must equivalents).
  - Poetry/structure: Rhyme/meter schematic validator when applicable; stanza/line count checks.
  - Human-in-the-loop review for high‑risk categories (safety, legal, offers).

## Confidence and Coverage
- Confidence: Medium. Multiple judges and domains consistently cite the same families (term drift, numerals/constraints, proper nouns, modals). 
- Coverage limits: Evidence is aggregated across varied sources and tasks; not all are Chinese-origin prompts, and some are back-translation judgments. Domain density highest in sports, cooking, gaming, safety/legal; fewer pure scientific/medical samples.