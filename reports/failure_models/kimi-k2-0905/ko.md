# Failure Model — Kimi K2-0905 — Korean

## Summary
Overall, Kimi K2-0905 produces fluent Korean but exhibits systematic fidelity drift when translating back and forth, especially with proper nouns, domain‑specific terminology, modality in legal/technical text, and fine‑grained imagery. The most salient weaknesses are: 1) unstable handling of names and fixed terms (people, places, awards, product/menu items), 2) semantic substitutions in technical/scientific/culinary vocabulary, 3) modality and normative-strength shifts (should/may ↔ must), 4) poetic/literary metaphor flattening or inversion, and 5) occasional unit/number and list/plurality errors. These often trigger in sports play‑by‑play, legal/policy, culinary, natural history/ecology, and literary prose.

## Top Failure Modes
- Proper-noun and key-term drift — Names, ranks, brands, awards, places, and fixed jargon frequently change spelling or identity; sometimes whole terms are replaced with near‑neighbors.
  - Why: Korean romanization ambiguity (R/L, vowel length), model’s paraphrasing bias, weak copy fidelity for OOV entities.
  - Frequency: common; Severity: high
  - Evidence: “Razor→Laser; Maya→Maia” in sports recap; “Thornbury→Somerby; Whitaker→Whittaker” in sea story; awards/initiative names altered.

- Domain term substitution (culinary, species, engineering) — Specific items are replaced with adjacent concepts: ingredients, cuts, cooking methods; species/habitats; mechanical terms.
  - Why: Korean polysemy and overlapping lay/pro terms; insufficient domain lexicons; preference for more common synonyms.
  - Frequency: common; Severity: high
  - Evidence: “star anise→fennel; beef shank→knee; pan‑fried→un‑fried” in a food review; “dunlins→godwits,” “marine worms→insects” in birding; “hydraulic→fluid,” “pump unit→pump body.”

- Modality and legal nuance shifts — SHOULD/MAY become must; venue↔jurisdiction; commitments softened/strengthened; key qualifiers omitted.
  - Why: Korean modality mapping is subtle (권고 vs 의무); legal register simplification; omission of conditional/qualifying clauses.
  - Frequency: common; Severity: high
  - Evidence: “should→must,” omission of “licensed to practice law,” “exclusive venue→exclusive jurisdiction”; normative shifts in policy doc.

- Logical/state contradictions in procedural/sports text — Event outcomes contradict scores or sequence; tactical instructions inverted.
  - Why: Over‑summarization; loss of aspect markers; rephrasing that drops negation or outcome.
  - Frequency: occasional; Severity: high
  - Evidence: Free throw “miss” but score increases; press cue “show then jump” changed to unrelated “close angle.”

- Metaphor/imagery inversion and tone flattening (literary) — Key metaphors altered or literalized; archaic or poetic tone simplified; sensory terms swapped.
  - Why: Preference for standard Korean diction; difficulty preserving unusual collocations; back‑translation normalizes figurative language.
  - Frequency: common; Severity: medium
  - Evidence: “ox→cow,” “knotted breath→hawk,” “hum→crackle,” “dam the warming storm→warm the storm.”

- Numbers/units and list/plural stability errors — Celsius/Fahrenheit misread; plural components made singular; counts and enumerations drift.
  - Why: Unit assumptions; Korean optional plurality; summarization of lists.
  - Frequency: occasional; Severity: medium
  - Evidence: Celsius used where Fahrenheit implied; “batteries/wheels” switched to singular; step counts slightly altered.

- Register and genre mismatches — Official terms replaced (“Conservancy→Association”), genre labels generalized (“microfiction→short story”); imperative softened.
  - Why: Over‑normalization to frequent news/neutral style; politeness/register mapping issues.
  - Frequency: occasional; Severity: medium
  - Evidence: Imperatives turned declarative in workout guide; “Chain of Custody” altered; “Coordinator→Moderator.”

- Small but critical omissions/additions — Drop conditional phrases or add extraneous details that change policy/scope.
  - Why: Sentence compression; connective/hedge loss; hallucinated helpfulness.
  - Frequency: occasional; Severity: medium
  - Evidence: Omission “rather than physical presence”; added “This is an announcement”; “may be served→will be served”; added “teriyaki.”

## Language‑Specific Factors
- R/L and vowel ambiguity in transliteration leads to entity drift on back‑translation (e.g., Razor/Laser; Maya/Maia).
- Korean lacks obligatory plural marking and articles, contributing to plural↔singular and definiteness ambiguity.
- Modality mapping is delicate: “-해야 한다/필수” (must) vs “-하는 것이 좋다/권장” (should), “-할 수 있다/허용” (may). The model often upgrades/downgrades strength.
- Pronouns are often omitted in Korean; back‑translation may reinsert gendered pronouns (“they→he”) or flip perspective.
- Sino‑Korean near‑synonyms encourage substitution (“관할” jurisdiction vs “재판지/재판 관할” venue; “검증/검정/검수” validation/verification).
- Culinary and species lexemes have close neighbors: “회향” (fennel/anise confusion), “사태”(shank) vs “무릎”(knee), common bird names with overlapping folk labels.
- Literary idioms/onomatopoeia: strong English sound words (“CRACK”) rendered as Korean mimetics then back‑ascribed imprecisely; archaic/legal prose often simplified.

## Numbers, Units, and Formatting
- Units: Occasional Celsius/Fahrenheit substitution; no explicit conversion requested but model normalizes to Celsius.
- Numbers/lists: Plurals collapse to singular; enumerations compressed or expanded; minor count drift.
- Titles/metadata: Misread headings (“PROPERTY Disclosure→Pepper Disclosure” typo), capitalization confusions.
- Tables/specs: Slight wording shifts (“hydraulic→fluid”) and hyphenation/casing changes; generally preserves structure but loses precision.

## Meta/Disclaimer Behavior
- Mild, context‑triggered additions in announcements/policies: e.g., prepend “This is an announcement,” add freebies (“wheelchairs for free”), or invented pairings (“teriyaki” in wine notes).
- Triggers: Instructional, customer‑facing, or hospitality copy where the model “helpfully” elaborates.

## Recommendations
- Prompting
  - Enforce entity/number lock: “Preserve all proper nouns, numbers, units, dates, and list counts exactly. Do not rename or translate proper names; copy spellings.”
  - Modality guardrails: “Map MUST/SHOULD/MAY precisely; do not strengthen or weaken obligations. Preserve ‘venue’ vs ‘jurisdiction’ exactly.”
  - Style constraints: “Maintain register and genre markers (archaic/legal/poetic). Preserve metaphors and imagery literally unless nonsensical.”
  - Domain hints: Provide mini‑glossaries for culinary cuts, spices, species, and technical terms; ask for “[TERMINOLOGY: …] use exactly.”
- Decoding
  - Lower temperature/top‑p for translation tasks to reduce paraphrasing.
  - Use constrained decoding/copy‑bias for detected entities, numbers, and units.
- Data and fine‑tuning
  - Augment with Ko‑En parallel corpora featuring: legal contracts with modality annotations; sports play‑by‑play; culinary menus/recipes; ecology/taxonomy; engineering specs.
  - Curate contrastive pairs for modality (must/should/may), venue/jurisdiction, and near‑synonym traps (fennel vs star anise; shank vs knee).
  - Add transliteration consistency training and entity preservation objectives.
- Post‑processing
  - Named‑entity and terminology checker against source; flag changes in spellings, titles, awards, place names.
  - Unit and numeral validator; forbid unit conversion unless explicitly requested.
  - Modality linter to detect upgrades/downgrades; legal term map (venue/jurisdiction; administered/judgment entered).
  - Glossary enforcement for domain terms; species/cut/spice lexicon cross‑check.
  - Gender/pronoun policy: prefer neutral forms or copy source ambiguity; avoid gender insertion.

## Confidence and Coverage
- Confidence: Moderate‑high. Dozens of low‑score rationales converge on the same families (entity drift, domain substitutions, modality shifts, imagery flattening, units).
- Coverage limits: Evidence spans multiple judges and domains but is biased toward English↔Korean round‑trips with literary, legal, culinary, technical, and sports content; fewer examples of highly structured tables or code.