# Failure Model — Kimi K2-0905 — Hindi

## Summary
Overall the model preserves gist but frequently drifts on lexical precision, register, and polarity in Hindi. The most salient weaknesses are: (1) meaning inversions on small but pivotal tokens (yes/no gestures, antonyms, strategic priorities), (2) systematic softening/flattening of tone and style (poetry, archaic/legal/professional registers), (3) lexical substitutions that distort technical or concrete referents (species, materials, book formats, terrain), (4) gender/pronoun and proper-noun instability, and (5) minor but recurrent number/plurality and unit/detail shifts.

## Top Failure Modes
- Polarity and micro‑semantic flips — Description: Reverses key binary meanings (affirmative vs negative gestures; A-over-B priorities → B-based strategy; “sheep”→“wolves,” “impervious”→“reflective”). Why: Over-reliance on loose paraphrase and semantic clustering; Hindi ambiguity around gesture verbs and negation particles; inattentive mapping of “over/versus” constructions. Frequency: common; Severity: high.
  - Evidence: Child’s nod “yes” rendered as head‑shake “no,” changing food preference; “profitability over volume” inverted to “volume‑based profitability.”

- Register flattening and idiom literalization — Description: Archaic, poetic, or technical voices are modernized or simplified; metaphors get generic synonyms, weakening imagery and argument force. Why: Preference toward neutral Hindi, limited exposure to high-register or poetic Hindi equivalents, decoding set to paraphrase. Frequency: common; Severity: medium–high.
  - Evidence: “ordained and decreed” softened to “ordered and proclaimed”; “kiln of will”→“forge of want,” “breathless”→“lifeless.”

- Terminology drift in technical/legal/artistic domains — Description: Specific terms replaced by near neighbors or wrong category (“octavo”→“folio,” “turbid”→“dusty,” “User Stories” weakened; “Venue”→“Location”). Why: Sparse domain lexicons in training; mapping to more frequent Hindi generalisms; false friends. Frequency: common; Severity: medium–high.
  - Evidence: Book format misread (octavo/folio); environmental terms (“impervious surface”→“reflective surface”), legal clause “arbitrated as written” misconstrued.

- Concrete noun/attribute substitutions — Description: Wrong species/materials/colors or object roles (“ox”→“bull,” “larch”→“cedar,” “sugar packets”→“cubes,” “horn”→“antler”). Why: Distributional proximity + Hindi lexical gaps or polysemy; tendency to normalize to common items. Frequency: common; Severity: medium.
  - Evidence: Nature passages swap species/landforms; culinary/tools lists alter item specifics (hose→pipe, shears→scissors).

- Gender, pronoun, and named-entity instability — Description: Gender swaps, loss of neutrality, and name spellings drift (Ember→Amber, Eli→Ellie), kinship/gendered address misrendered. Why: Hindi requires gender agreement; default masculine bias; lack of constraints to preserve proper nouns; “they” mapping pressure. Frequency: common; Severity: medium.
  - Evidence: “mija”→“mijo” style gender flip; nonbinary “they” rendered masculine; multiple proper-name variants.

- Quantity, frequency, and plurality mismatches — Description: Plural↔singular toggles, frequency ranges reduced or altered; minor number/category slips. Why: Hindi allows number elision; decoder preference for concise paraphrase; unit/range formatting not constrained. Frequency: occasional; Severity: medium.
  - Evidence: Tomatoes→tomato; “Uncommon” frequency misread (1 in 1000 vs 1–100 in 1000).

- Instructional/ procedural precision loss — Description: Steps, positions, or qualifiers altered/omitted (“without hurry” dropped; brace position changed; “tabs” singularized). Why: Compression bias and synonym substitution in action verbs; difficulty with stacked modifiers. Frequency: occasional; Severity: medium.
  - Evidence: Yoga/first‑aid/UX instructions lose specific placements or qualifiers.

## Language‑Specific Factors
- Gendered agreement pressure: Hindi verbs/adjectives mark gender and number; neutral “they/them” and ambiguous roles push the model to pick masculine or flip gender.
- Gesture semantics: Hindi phrases like “sir hilaana” can imply nodding yes or general head movement; without explicit “haan/naa,” polarity can invert.
- Register mapping: High-archaic legal or poetic English often mapped to either Sanskritized Hindi (too stiff) or casual Hindustani (too soft); model tends to the latter, flattening tone.
- Idiom and compound metaphor: Dense English imagery often gets decomposed to literal or simpler Hindi, losing lyricism and double meanings.
- Postpositions and A-over-B constructs: English “X over Y” (preference/priority) can be mistransferred to “Y‑based X” in Hindi if not carefully structured (e.g., “X ko Y par prathamikta” vs “Y‑aadharit X”).

## Numbers, Units, and Formatting
- Frequency/ratio ranges simplified or altered (1 in 1000 vs range 1–100/1000).
- Plural/singular drift in lists and inventories (tools, ingredients).
- Named quantities/details normalized (gusts→speed; “Basalt canyons”→proper name “Basalt Canyon”).
- No systemic date/decimal issues observed; occasional capitalization and title-case looseness in legal/section headers.

## Meta/Disclaimer Behavior
- Rare translator intrusions or notes: occasional parenthetical insertions like “(core)” in poetry; no consistent safety/policy disclaimers contaminating outputs.
- Tends to rephrase quotes rather than mark as verbatim, changing tone in reported speech.

## Recommendations
- Prompting
  - Enforce constraints: “Translate, do not paraphrase. Preserve polarity, entities, numbers, units, and register. Keep metaphors intact unless ungrammatical.”
  - Add checklists: “Before finalizing, verify: gestures (yes/no), ‘X over Y’ priorities, legal terms, species/objects, plural vs singular, and numerical ranges.”
  - Specify gender handling: “Preserve original gender/neutrality; if unspecified, prefer neutral phrasing (e.g., vyakti/bachcha; plural-neutral constructions).”
  - Quote fidelity: “Render quoted speech verbatim in Hindi quotation marks; do not modernize archaic/legal phrasing.”

- Decoding
  - Lower temperature/top‑p to reduce paraphrase drift on high‑register and technical texts.
  - Use constrained decoding for named entities and numbers (copy bias).

- Data/Training
  - Fine‑tune with parallel EN–HI corpora emphasizing: legal contracts, technical/scientific prose, lyrical poetry/literary Hindi, and domain glossaries (environmental science, book history, climbing/outdoors, culinary tools).
  - Curate contrastive pairs focusing on: nod vs shake, over/versus constructs, species/terrain lexicons, book formats (folio/octavo), and legal boilerplate.
  - Augment with gender‑neutral rendering patterns and evaluations for neutrality preservation.

- Post‑processing QA
  - Automated diffs for named entities, numerals, units, and polarity cues (negation, antonyms, “no/not/without,” yes/no gestures).
  - Terminology validator with domain glossaries; flag out‑of‑vocabulary substitutions.
  - Style/register checker to detect excessive simplification of legal/archaic tone.
  - Optional round‑trip or back‑check for high‑stakes content to catch inversions.

## Confidence and Coverage
- Confidence: medium-high for the identified failure families; many independent judges cite similar issues (polarity flips, register flattening, terminology drift, gender/name instability).
- Coverage limits: Evidence spans mixed domains with fewer hard numeric/date cases and limited code/markup samples; Hindi‑specific analysis inferred from recurring patterns rather than direct side‑by‑side source/target visibility.