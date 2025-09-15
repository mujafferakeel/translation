# Failure Model — Qwen 3 Max Preview — Japanese

## Summary
Overall, Qwen 3 Max Preview delivers structurally faithful Japanese translations but frequently drifts on precision and voice. The most salient weaknesses are: (1) semantic substitutions for domain terms and named entities, (2) person/role and voice shifts in dialogue due to Japanese pro‑drop and politeness defaults, (3) quantity and packaging misreads (singular/plural, packet vs single unit), (4) stylistic flattening in literary/archaic registers, and (5) occasional omissions of clauses or sentences. These errors are often small in isolation but compound to alter facts and tone.

## Top Failure Modes
- Entity and terminology substitution — Proper names, species, tools, ranks, and domain terms replaced with near neighbors; alters facts and genre/lore.
  - Why: Katakana loanword proximity, weak domain lexicons, and decoding preference for common terms; ambiguity in Japanese common names.
  - Frequency: common; Severity: high
  - Evidence: “crowbar” becomes “pickaxe/fist” in crime scene example; multiple plant/pest swaps (“daffodils→irises,” “flea beetles→leafhoppers”).

- Person/role inversion and voice drift in dialogue — Speaker roles, pronouns, and addressee flip; “you”→“user/third person”; imperative voice introduced.
  - Why: Japanese subject omission and honorific alignment cause the model to guess referents; tendency to formalize to 丁寧語 and instructions.
  - Frequency: common; Severity: high
  - Evidence: “Can I fix this?” rendered as “Can you fix this?”; narrative switches from “we/you” to “I/the user” altering stance.

- Quantity/packaging and number neutrality errors — Plural/singular, collectives, and packaging misinterpreted; units added or altered.
  - Why: Japanese number neutrality, classifier omission, and lexical gaps for packaging lead to underspecified targets; model fills with defaults.
  - Frequency: occasional; Severity: medium
  - Evidence: “a packet of tea bags” becomes “a single teabag”; “Sellers/Buyer(s)” collapsed to singular “Seller/Buyer.”

- Stylistic register flattening (poetry, archaic, nautical/legal flavor) — Imagery and register softened; era-specific/legal/nautical terms generalized.
  - Why: Preference for standard polite Japanese and paraphrase over form‑faithful rendering; sparse training on style‑anchored parallel data.
  - Frequency: common in creative/formal texts; Severity: medium
  - Evidence: Poem’s “unfurled embrace” becomes “embrace slowly undone”; “Magistrate” normalized to “Justice of the Peace,” nautical “starboard/becalmed” lost.

- Omission and minor invention — Clauses or a paragraph dropped; small additions (honorifics, politeness fillers, cultural details) inserted.
  - Why: Length/complexity compression, beam bias toward concise outputs; Japanese formulaic politeness and smoothing during back‑translation.
  - Frequency: occasional; Severity: medium
  - Evidence: Glacier‑retreat paragraph omitted; added “despite your busy schedule” and Fahrenheit where none existed.

- Technical nuance drift — Hazard classes, metrics, traffic ops terms, and RPG mechanics slightly shifted.
  - Why: Term conflation in low‑frequency technical clusters; Japanese synonyms with overlapping scope; model “helpfulness” resolving ambiguity incorrectly.
  - Frequency: occasional; Severity: high when present
  - Evidence: “Combustible liquid”→“Flammable liquid”; “MRR” inflated to “annual MRR”; “turning movements”→“U‑turn.”

## Language‑Specific Factors
- Pro‑drop and pronoun indeterminacy: Japanese omits subjects/objects, triggering person/role guessing on back‑translation, e.g., “we”→“I,” “they said”→“you said.”
- Politeness system defaults: Tendency to 丁寧語 and honorific/impersonal phrasing yields tone shifts (“you”→“user,” imperative voice).
- Number neutrality and classifiers: Missing plural marking and counters cause singular/plural and packaging errors (“packet” vs “one”).
- Katakana and near‑synonym interference: Loanwords and similar phonetics lead to tool/species swaps (“paperback” misread as “paper bag”; “oxbow” distorted).
- Title/name romanization drift: Small kana/romaji variants produce name changes (Eli↔Eri; Rin↔Lin; Harborview↔Harberview).

## Numbers, Units, and Formatting
- Numerals/units: Occasional unit inventions or conversions (added Fahrenheit); metric/measure scope shifts (MRR vs annual; hazard class swap).
- Date/tense: Sporadic present/future tense normalization.
- Lists/headings: Headings rephrased (“Open Questions”→“Unresolved Issues”), bullets preserved but labels changed.
- Singular/plural: Frequent conflation due to Japanese number neutrality (collectives→singular; Sellers→Seller).
- Addresses and proper formatting: Street types and institutional names altered (Avenue→Street; depository names tweaked).

## Meta/Disclaimer Behavior
- Politeness add‑ons and honorifics: Inserts “please check,” “despite your busy schedule,” or honorific titles in formal contexts.
- Safety/policy disclaimers: Rare; no systematic contamination observed. Additions more stylistic than policy‑driven.

## Recommendations
- Prompting
  - Provide a term lock/glossary: “Preserve these terms verbatim: [entity/skill/species/tools].”
  - Enforce voice and perspective: “Maintain 2nd person ‘you’; do not convert to ‘user/third person’. Keep speaker roles and line breaks exactly.”
  - Style constraints: “Match register: casual/plain form for dialogue; retain archaic/legal/nautical terms; avoid added honorifics or politeness fillers.”
  - Ambiguity handling: “If number/packaging unspecified, mirror source scope; do not singularize/pluralize by inference.”
- Decoding
  - Lower temperature/top‑p for determinism on proper nouns; enable constrained decoding for glossary spans and numbers/units.
  - Use copy‑bias for named entities and capitalized tokens.
- Data and fine‑tuning
  - Augment with parallel corpora emphasizing: dialogue with explicit speaker tags, legal/nautical/traffic ops, biology (species/pests), RPG/fictional ability names.
  - Include targeted contrastive pairs for hazardous confusions (combustible vs flammable; crowbar vs pickaxe; packet vs teabag).
  - Train on number/counter disambiguation and packaging expressions (袋, 箱, パック).
- Post‑processing
  - NER and term consistency check against source; flag edits to capitalized terms and domain lexicon entries.
  - Quantity/packaging audit: detect singular/plural drift and “packet/unit” mismatches.
  - Units/hazards/metrics validator: rule‑based checks for class labels, units, and KPIs (MRR, ranks, traffic terms).
  - Dialogue QA: turn‑by‑turn speaker alignment; detect pronoun/role inversion.
  - Style lint: enforce requested register (e.g., no “the user,” no added honorifics); preserve line breaks and poem structure.

## Confidence and Coverage
- Confidence: Moderate. Patterns recur across many judges and samples, with consistent issues in names/terms, voice, and number neutrality.
- Coverage limits: Evidence is round‑trip and multi‑domain; some low‑frequency domains (poetry, RPG, traffic ops) may overrepresent style/term drift. Few instances of hard formatting/table failures; minimal policy disclaimers observed.