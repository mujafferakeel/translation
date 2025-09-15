# Failure Model — Claude Opus 4.1 (no reasoning) — Arabic

## Summary
Claude Opus 4.1 generally preserves structure and gist in Arabic round‑trip translation but exhibits systematic drift on precision, register, and referential features. The most salient weaknesses are: (1) technical/jargon terms softened to generic Arabic, (2) named entities and proper terms normalized or altered, (3) meaning‑critical flips in small words and set phrases (timing, roles, spatial terms), (4) gender and perspective shifts due to neutral→gendered mapping, and (5) tone/register flattening (legal/archaic/sportscast/marketing). Errors spike with domain specificity (conservation, firefighting, cloud, law, sports, nautical) and with ambiguous pronouns or idioms.

## Top Failure Modes
- Precision drift in domain terminology — Technical or specialized terms rendered with more common Arabic words, losing scope or changing the mechanism.
  - Why: Preference for high‑frequency MSA synonyms; sparse Arabic domain training; avoidance of transliteration.
  - Frequency: Common; Severity: High in technical content, Medium otherwise.
  - Evidence: 
    - Art conservation terms simplified (“flaking/inpainted/reversible” softened to “cracking/repainting/removable”).
    - Firefighting and security terms altered (“mop‑up,” “aggressive attack,” “hashed password”→“encrypted password”; “Forwarders”→“Senders”).

- Proper nouns and canonical terms altered — Fantasy/game/location names, product features, species, and sports jargon changed to near‑synonyms or different entities.
  - Why: Over‑normalization, lack of term memory across steps, low Arabic exposure to niche lexicons.
  - Frequency: Common; Severity: Medium.
  - Evidence:
    - “Catacombs→Crypts,” “Sundering→Shattering,” quest/location names adjusted; “bails” turned into “pieces”; “stag→elk.”

- Meaning‑critical flips in short anchors — Small lexical pivots (prepositions, qualifiers, set phrases) invert constraints or facts (time, count, location).
  - Why: Ambiguity resolution under Arabic rephrasing; loose alignment of fixed expressions; beamless decoding magnifies single best guess.
  - Frequency: Occasional; Severity: High.
  - Evidence:
    - “Apply no earlier than 90 days” became “no later than 90 days.”
    - Real estate: “two‑bedroom loft” became “two‑story apartment”; “deeded parking”→“dedicated.”
    - Safety/compliance: “combustible liquid”→“flammable liquid”; routing “counter” orders→“table” orders; navigation “port side”→“right side.”

- Gender and perspective drift — Neutral English “they/their” becomes masculine; occasional 1st/3rd person shifts.
  - Why: Arabic lacks a singular gender‑neutral pronoun; the model defaults to masculine and sometimes mis‑tracks voice.
  - Frequency: Common; Severity: Medium.
  - Evidence:
    - Multiple cases “their partner”→“his partner”; neutral reviewer becomes “he.”
    - Perspective error: “when she met you”→“when I met you.”

- Tone/register flattening and idiom loss — Colloquial/legal/archaic or stylistic signals normalized to generic MSA, blunting voice and precision.
  - Why: Preference for formal MSA; limited Arabic data for niche registers (sportscasting, archaic legal, ad copy).
  - Frequency: Common; Severity: Medium.
  - Evidence:
    - Archaic legal register modernized; sportscast verbs (“barrels”) literalized; marketing pun “Spring into savings” lost; “pinch of salt”→“fist of salt”; “ox”→“bull.”

- Taxonomy mismatches in related technical nouns — Part vs part, material vs component, or family vs specific member swapped.
  - Why: Term proximity in embeddings; Arabic nominal system encourages generic nouns unless glossary enforced.
  - Frequency: Occasional; Severity: Medium.
  - Evidence:
    - Pump specs: “stainless‑steel hub”→“stainless‑steel shaft”; “tinned copper conductors”→“connectors.”
    - Horticulture: “grafting” replaced with “feeding”; “cloches” generalized to “covers.”

## Language‑Specific Factors
- Gender marking and lack of neutral singular: Drives “they→he” defaults; verbs/adjectives reflect masculine agreement unless constrained.
- Definiteness and idafa (construct state): Can push genericization (“the” applied too broadly) or alter possession scope without explicit anchors.
- Register choice (MSA vs colloquial): Model gravitates to formal MSA, flattening idioms, sportscasting, and archaic legal diction.
- Polysemy and near‑synonyms in Arabic technical borrowings: Multiple viable renderings (hashed vs encrypted; forwarder vs sender) lead to inconsistent picks without a glossary.
- Number and aspect marking: Median/average and tense timelines drift under Arabic aspectual rephrasing.
- Low‑density lexicons (cricket, nautical, conservation): Limited Arabic exposure yields paraphrase or wrong near‑class terms (“sheet/bilge/jackline” simplified).

## Numbers, Units, and Formatting
- Quantifier/metric drift: “Median”→“average,” “population‑level”→“community‑level.”
- Temporal anchors: “tomorrow afternoon”→“tomorrow noon”; “hour nine” normalized to clock time.
- Spatial/side terms: “port”→“right.”
- Material/soil classes: “sandy loam”→“sandy silt.”
- Policy timing: “no earlier than 90 days” flipped to “no later than 90 days.”
- Behavior: Occasional conversions/inventions; measurements mostly preserved when numerals are explicit; categorical labels more error‑prone than raw numbers.
- Tables/markup: No systemic table/markup corruption observed; terminology columns drift via synonymization.

## Meta/Disclaimer Behavior
- Little to no policy disclaimers intruding into translations.
- Contamination is semantic, not meta: e.g., security terms altered (“hashed”→“encrypted”) without safety disclaimers.

## Recommendations
- Prompt patterns
  - Provide a term‑locked glossary and require “use these exact Arabic terms; do not substitute,” including:
    - Domain jargon (security, cloud, conservation, firefighting, sports, nautical).
    - Proper names and quest/feature titles.
  - Add guardrails: “Preserve timing phrases, quantities, sides (port/starboard), and legal terms verbatim; do not invert or normalize.”
  - Gender control: “Avoid assigning gender if not specified; repeat the noun or use plural‑generic constructions to maintain neutrality.”
  - Register control: “Maintain the source register (archaic/legal/sportscast/marketing); avoid formalizing or simplifying idioms.”
  - Back‑check instruction: “Before finalizing, re‑compare critical anchors (time, polarity, side, count, hazard class) and list any unchanged.”

- Decoding
  - Lower temperature and increase n-best sampling for reranking with terminology constraints.
  - Apply constrained decoding or lexically constrained beam for glossary and named entities.

- Data and fine‑tuning
  - Curate Arabic parallel corpora for:
    - Security/cloud/SRE, conservation science, horticulture, firefighting, maritime, cricket/sports commentary, archaic/legal Arabic.
  - Augment with contrastive pairs targeting:
    - Median vs average; combustible vs flammable; deeded vs dedicated; population‑ vs community‑level; port vs right.
  - Gender‑neutral rendering datasets in Arabic (templates for neutrality strategies).
  - Lexicon alignment for soil taxonomy, pump components, and culinary/horticulture terms.

- Post‑processing QA
  - Terminology enforcement: Automatic term scanner comparing output against a source‑aligned glossary; flag deviations.
  - Anchor/constraint checker: Regex or rule‑based checks for:
    - Temporal phrases (no earlier/later than X), sides (port/starboard), hazard classes, units, medians vs averages.
  - Named‑entity consistency: Fuzzy match back‑translation of NEs against source; flag edits.
  - Gender audit: Detect unsignaled gendering of humans; suggest neutral rewrites.
  - Style check: Register classifier to detect unwanted formalization where source is colloquial/archaic/marketing.

## Confidence and Coverage
- Confidence: Medium‑high. Dozens of low‑score rationales show consistent patterns (terminology drift, NE changes, gender shifts, anchor flips), repeated across multiple judges and domains.
- Coverage limits: Evidence is round‑trip and heterogeneous in genre; limited direct visibility into pure Arabic→English errors outside summarized notes; fewer instances of tables/markup and strict numeric conversion, and minimal meta/safety intrusions.