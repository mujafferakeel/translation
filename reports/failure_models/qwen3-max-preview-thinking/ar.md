# Failure Model — Qwen 3 Max Preview — Arabic

## Summary
Across low‑scoring round‑trip translations, Qwen 3 Max Preview shows strong fluency but recurrent fidelity lapses: mid‑document corruption and truncation; systematic drift on proper nouns and domain terms; precision loss in technical, scientific, and gaming registers; unintended gender and register shifts; and occasional errors with units/dates/mechanics. The most salient weaknesses are: 1) content omission/garbling mid‑output; 2) substitution of specific terms with generic Arabic paraphrases; 3) factual swaps in named entities and taxonomies; 4) pronoun/gender normalization to masculine; 5) sporadic number/unit and mechanics misinterpretation.

## Top Failure Modes
- Truncation with garbling in the middle — description: Output abruptly breaks with foreign characters or repeated fragments, dropping later sections. Why: decoding/length handling or corrupted byte/character handling under long contexts; beam instability around boundaries. Frequency: common. Severity: high.
  - Evidence: “Body scan” translation inserts non‑Arabic glyphs mid‑section and omits everything after knees; later instructions missing.
  - Evidence: Early sections faithful, then abrupt cutoff and repetition; ending of guided meditation omitted.

- Proper noun and term substitution — description: Consistent changes to names, titles, and specialized terminology (apps, places, game items, art titles) producing factual inaccuracy. Why: uncertainty about transliteration vs translation; over‑regularization to common Arabic terms; weak entity anchoring. Frequency: common. Severity: high.
  - Evidence: “Mounts” becomes “boats”; multiple boss/ability/location names altered in a game guide.
  - Evidence: “Pagination” rendered as “punctuation”; “Ember” variety changed to “Empire.”

- Domain precision loss (technical/scientific) — description: Meteorology, engineering, legal, UX, nautical, and sports terms softened to generic Arabic, shifting meaning. Why: limited domain term coverage in Arabic training data; preference for high‑probability synonyms; lack of term memory. Frequency: common. Severity: high.
  - Evidence: Forecast errors: temps shifted by ~10°F; “drizzle” → “fog”; “weak trough” → “weak low.”
  - Evidence: “Through ball” translated as “cross”; pump “hub” vs “shaft”; “brushless” → “cordless.”

- Taxonomy and object misidentification — description: Species, food, artifacts, and habitat terms swapped for near‑neighbors, distorting referents. Why: low lexical grounding for rare nouns; Arabic lexical gaps; reliance on hypernyms. Frequency: common. Severity: medium.
  - Evidence: “Kingfisher” → “heron”; “curlew” → “crane”; “mudflat” → “marsh.”
  - Evidence: Recipe terms: “paprika” → “red pepper”; “sea bass” → “wild fish.”

- Register and tone drift — description: Formal/archaic/legal registers flattened; modality strengthened (should → must); poetic/evocative diction simplified; meter misclassified. Why: decoder preference for common modern Arabic registers; sparse exposure to archaic/legal prose and prosody alignment. Frequency: occasional. Severity: medium.
  - Evidence: 18th‑century legal text made modern: “City” → “town,” “Watch” → “watchmen.”
  - Evidence: Poem analysis flips “iambic” to “trochaic,” altering meter claim.

- Pronoun and gender shifts — description: Gender‑neutral English “they/their” re‑rendered as masculine; some roles/genders shifted in narratives and notices. Why: Arabic lacks a standard singular gender‑neutral; model defaults to masc.; alignment pressure to resolve ambiguity. Frequency: occasional. Severity: medium.
  - Evidence: Neutral subject becomes male throughout press and narrative passages.
  - Evidence: Salutation and referents switch to masculine forms; neutral customer becomes male.

- Mechanics and rule misinterpretations — description: Game stats and checks swapped; nautical/sailing hardware mistranslated; table‑service vs counter‑service inverted. Why: overlapping near‑synonyms; domain jargon ambiguity; insufficient grounding. Frequency: occasional. Severity: medium.
  - Evidence: RPG checks: Intelligence swapped to Wisdom; “pools” becomes “freezes.”
  - Evidence: “Jackline” rendered as generic “safety line”; “bilge” as “water tank.”

- Numbers, units, and temporal scale errors — description: Unit category swaps and scale mistakes; date/time greetings; planetary sols misread. Why: heuristic normalization; rarity of domain units in Arabic corpora. Frequency: occasional. Severity: medium.
  - Evidence: Mars “sols” become “lunar days,” changing mission timescale.
  - Evidence: “Afternoon” rendered as “evening”; temperatures off by a band.

## Language‑Specific Factors
- Gender neutrality gap: English singular they pushes the model to default to masculine Arabic; neutrality strategies (passive voice/plural) are unevenly applied, causing unintended gendering.
- Terminology scarcity in Arabic: Low‑frequency items (heath, mudflat, lacewing, “through ball,” “jackline,” “minute order”) lack widely accepted Arabic equivalents; model substitutes generic or near‑neighbor terms, reducing fidelity.
- Transliteration vs translation ambiguity: Proper nouns and branded names often over‑translated or respelled (e.g., Arcline/Arkline), reflecting inconsistent transliteration policies.
- Register mapping: Arabic formal registers can default to Modern Standard simplifications, losing archaic/legal specificity and rhetorical modality nuances.
- Prosody and poetic meter: English meter labels and scansion cues are misidentified when back‑rendered to Arabic and back, likely due to weak prosodic modeling across scripts.

## Numbers, Units, and Formatting
- Unit/category swaps: planetary time (sols vs lunar days), temperature bands, battery type (pouch vs coin‑cell).
- Temporal expressions: greetings (“afternoon” vs “evening”), hours vs clock labels.
- Mechanics attributes: D&D‑style stat checks (Intelligence vs Wisdom) swapped.
- Conversions/inventions: Occasional invention or normalization of quantities; not systemic but impactful when present.
- Tables/markup: No strong evidence of structural table breakage; the key formatting failure is mid‑output corruption and truncation in long texts.

## Meta/Disclaimer Behavior
- Minimal authorial intrusions or safety disclaimers observed. “Disclaimer_meta” tags corresponded to terminology errors rather than visible policy statements. Contamination is not a primary failure mode.

## Recommendations
- Prompt patterns
  - Enforce no‑omission fidelity: “Translate all content fully. Do not summarize, omit, or add. Preserve order and line breaks.”
  - Entity protection: “Keep all proper names, game stats, and item names unchanged in Latin script inside ‹…›; transliterate only common nouns.”
  - Domain guardrails: Provide a per‑document glossary/termbase and require exact reuse: “Use these Arabic terms verbatim; if uncertain, copy the source term.”
  - Gender handling: “Maintain gender neutrality; prefer passive voice or plural where needed; avoid introducing gendered pronouns.”
  - Numbers/units lock: “Do not convert units, dates, or mechanics; reproduce numeric ranges exactly.”
  - Verification step: Chain‑of‑thought suppressed externally but ask for a second pass: “Now re‑read and list any changed names/units/roles; correct them.”

- Decoding/serving
  - Increase max tokens and enable no‑truncation; add stopping criteria on source‑aligned paragraph counts.
  - Use constrained decoding or lexically constrained beam search for provided glossaries and entities.
  - Apply repetition/garble detection: abort and retry on non‑Arabic glyphs mid‑sequence; enable byte‑fallback guard.

- Data and fine‑tuning
  - Curate Arabic parallel corpora for under‑served domains: meteorology, pumps/engineering, nautical, sports tactics, RPG manuals, legal/archaic.
  - Build an Arabic termbase with high‑precision mappings and transliteration policies; fine‑tune with copy‑span supervision for entities/units.
  - Augment with gender‑neutral rendering patterns in Arabic and evaluation that penalizes unwanted gender assignment.

- Post‑processing QA
  - Entity diffing: align source and translation to detect changes in proper nouns, SKUs, and acronyms.
  - Unit/number checker: regex-based validation for temperatures, ranges, units, planetary time terms.
  - Mechanics validator: pattern checks for game stats/attributes and role titles.
  - Style/register checker: flag modality shifts (should/must) and legal term drift (e.g., “minute order”).
  - Long‑form integrity check: paragraph count and keyword coverage to catch omissions.

## Confidence and Coverage
- Confidence: medium. Evidence spans many judges and topics, with dense signals on omissions, term drift, and technical precision; fewer cases on formatting and meta behavior.
- Coverage limits: Judgments emphasize EN→AR→EN round‑trip; specific Arabic dialectal issues and table/markup fidelity are underrepresented; poetry/prosody evidence is present but limited.