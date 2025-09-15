# Failure Model — Grok 4 — Arabic

## Summary
Grok 4’s Arabic round‑trip translations are generally intelligible but show systematic brittleness in semantic polarity, technical precision, and register control. The most salient weaknesses are: (1) antonym/direction reversals on action words and instructions; (2) drift from domain‑specific terms to generic synonyms; (3) gender hallucinations and role reversals caused by Arabic morphosyntax; (4) tone flattening toward formal MSA and cultural domestication; and (5) proper‑noun and terminology instability (names, product labels, stage terms). These issues appear across creative, technical, and procedural texts, with high severity when they flip intent or instructions.

## Top Failure Modes
- Antonymic and directional reversals — Description: Critical flips of meaning (affirmation/negation, up/down, positive/negative) in actions and instructions. Why: Arabic preposition mapping (e.g., “up/down”), negation scope, and ambiguous verb forms; weak anchoring to directionality terms. Frequency: common. Severity: high. Evidence: “nod” rendered as “shake head” repeatedly; stage directions “downstage” flipped to “upstage”; “false negatives” turned into “false positives.”

- Technical term dilution and misclassification — Description: Specific domain terms replaced with near‑neighbors that alter constraints or safety. Why: Insufficient domain grounding; Arabic term equivalence often conflates near‑synonyms; preference for common words over technical jargon. Frequency: common. Severity: high in specs/safety, medium elsewhere. Evidence: “drop‑in alleles” became “allele dropouts”; “gale/storm threats” swapped to “thunderstorm/squall”; “strain relief” to “pressure relief”; “buckling” to “twisting.”

- Gender hallucination and role/agent flips — Description: Added or changed gendered pronouns/titles; subject/object or agent/patient reversals. Why: Arabic requires gender agreement; absent gender cues lead to guesswork; VSO/SVO shifts obscure agents. Frequency: common. Severity: medium to high when it alters who did what. Evidence: “her” became “his”; neutral “their” forced to “his”; “Ms.” to “Mrs.”; “When pressed” (by reporter) turned into “When pressing” (by subject).

- Proper noun, label, and taxonomy drift — Description: Names, places, product types, and biological/engineering labels morph into near variants or misspellings. Why: No capitalization cues in Arabic; transliteration variance; preference for familiar lexemes. Frequency: common. Severity: medium. Evidence: “Rin Vale”→“Ren Val,” “Aether”→“Ether,” multiple person/place name variants; “Marsh Stag”→“gazelle”; “cable‑stayed”→“suspension.”

- Modality/strength and prescriptive force erosion — Description: MUST/SHOULD/REQUIRED softened or strengthened; constraints altered. Why: Modal verbs in Arabic map to overlapping forms; register smoothing. Frequency: occasional. Severity: high in policy/specs. Evidence: “MUST”→“should”; “requested”→“required”; “as instructed”→“as recommended.”

- Tone flattening and cultural domestication — Description: Evocative/idiomatic or archaic voices become generic formal MSA; occasional cultural substitution. Why: Bias toward formal register; limited idiom mapping; avoidance of marked cultural terms. Frequency: common. Severity: medium (creative/legal nuance loss). Evidence: “funky”→“fragrant”; archaic preamble replaced with modern invocation; “steeple”→“minaret.”

- Timeframe and scope shifts — Description: Temporal spans and counts drift (weekend→week; hop count→steps). Why: Preference for descriptive paraphrase; numeral/context loss. Frequency: occasional. Severity: medium. Evidence: “weekend sale” turned into “week”; “hop count” paraphrased as “number of steps.”

- Small lexical shifts causing factual errors — Description: Single-word changes invert or relocate facts. Why: Over‑synonymization; disambiguation errors. Frequency: occasional. Severity: high when safety/location/instruction changes. Evidence: “creek bed”→“bay area”; “lectern”→“platform”; “unmarked”→“unremarkable.”

## Language‑Specific Factors
- Gender agreement pressure: Arabic forces gender on verbs/adjectives and occupations, prompting added or incorrect gender where English is neutral.
- Lack of capitalization: Proper names and organizations are hard to track in Arabic script, inviting misspellings and genericization.
- Word order and agent marking: Frequent VSO ordering and dropped copula can blur subjects/objects, leading to agent/patient flips.
- Prepositions/directionality: “Up/down,” “in/on/at,” and theatrical blocking terms have non‑obvious Arabic mappings, triggering “downstage/upstage” reversals.
- Register and diglossia: The model gravitates to formal MSA, ironing out idiom, dialectal color, and archaic tones.
- Polysemy and borrowed technical lexicon: Several Arabic equivalents cover larger semantic fields, encouraging “near but wrong” choices (e.g., relief/pressure; storm classes).
- Diacritic absence: Without tashkīl, ambiguity increases for homographs, impacting precision in poetry and technical prose.

## Numbers, Units, and Formatting
- Temporal granularity drift: Weekend/week and count simplifications (hop count→steps). Occasional, medium severity.
- Table/heading artifacts: Non‑English or untranslated Arabic headers leaking into back‑translation; label swaps (Price Target/Target Price). Occasional, low severity.
- Date/connector prepositions: Minor preposition/date slips. Rare, low severity.
- Units and class terms: Weather/aviation layers (“marine layer” vs mid‑level) and bridge types misrendered. Occasional, medium severity.
- No systematic numeral invention observed, but sporadic substitution (e.g., quota→$1.1M equity) indicates semantic, not numeric, confusion; occasional, high severity when it changes meaning.

## Meta/Disclaimer Behavior
- Disclaimer inversion: “verifiable nature” reframed as “verifiable events,” flipping intended caveat. Occasional, medium severity.
- Accessibility/notes smoothing: Minor rewording adds advisory tone. Occasional, low severity.
- Spurious headers/boilerplate: Added Arabic or non‑English section titles in back‑translations. Occasional, low severity.
Triggers: policy/legal sections, editorial framing, or documents with explicit qualifiers/disclaimers.

## Recommendations
- Prompting
  - Enforce a bilingual glossary and do‑not‑change list for critical terms: {drop‑in alleles, downstage/upstage, gale/storm, false positives/negatives, strain relief, checkout}.
  - Add polarity and modality guards: “Preserve logical polarity (affirm/negate, up/down, false pos/neg) and RFC keywords (MUST/SHOULD) verbatim.”
  - Gender neutrality instruction: “Do not infer gender; keep neutral forms; avoid adding gendered titles/pronouns unless explicit.”
  - Register control: “Match tone/register (archaic/poetic/colloquial). Do not modernize or domesticate cultural/religious terms.”
  - Location/role clarity: “Preserve agents and recipients; do not change subject/object roles.”

- Decoding
  - Lower temperature/top‑p for technical/legal domains to reduce synonym drift.
  - Constrained decoding with lexically anchored terms for stage directions, safety classes, and engineering vocabulary.
  - Name preservation mode: copy‑through or transliteration lock for proper nouns.

- Data and fine‑tuning
  - Curate Arabic<->English parallel corpora with:
    - Minimal pairs for antonym/directional contrasts (nod/shake, up/down, positive/negative).
    - Stagecraft, aviation/marine weather, forensics/genetics, standards/RFCs.
    - Gender‑neutral, agent‑explicit sentences to reduce hallucinated gender.
    - Proper noun transliteration pairs and style guides.
  - Include diglossia/register examples (archaic legal, poetic, journalistic) to teach register preservation.

- Post‑processing checks
  - Antonym/polarity validator: scan for known contrast pairs and flag flips (nod↔shake; downstage↔upstage; false pos↔false neg; MUST↔should).
  - Entity and label consistency: verify names, product labels, and taxonomy terms against source; fuzzy‑match alerts for off‑by‑one character changes.
  - Modality/policy linter: detect MUST/SHOULD/REQUIRED drift; constraint strength diffs.
  - Direction/instruction map: deterministic mapping table for stage directions and spatial prepositions.
  - Timeframe/number diff: detect shifts in periods (weekend/week), counts, units, and class levels (bridge types, weather layers).
  - Optional dual‑pass: produce a literal pass + a style‑smoothed pass; prefer literal for back‑translation tasks.

## Confidence and Coverage
Confidence: medium‑high. The evidence spans many judges and genres with recurring patterns (polarity reversals, term dilution, gender drift, register flattening). Coverage limits: we infer from lowest‑scored rationales; some domains (e.g., medicine, finance tables) are less represented, and numbers/units errors appear sporadically rather than systematically.