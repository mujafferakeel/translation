# Failure Model — DeepSeek V3.1 Reasoner — Chinese

## Summary
DeepSeek V3.1 Reasoner produces fluent Chinese, but round‑trip fidelity drops due to systematic drift in proper nouns, domain terms, and register. The model often normalizes or embellishes content, adds units/explanations, and shifts tense/voice. Most damaging are consistent name/title substitutions, technical/jargon mismatches, and fact changes in numbers/offers. Style is frequently formalized, and Chinese‑specific artifacts (literal back‑translations, pronoun and tense choices) amplify deviations on the return pass.

## Top Failure Modes
- Proper noun drift and renaming — Changes or “sinicizes” names of people, places, works, and in‑world terms; sometimes standard terms are replaced by near neighbors.
  - Why: Weak copy bias for NEs; transliteration/romanization uncertainty; training data favors normalized Chinese renderings; back‑translation guesses canonical English.
  - Frequency: common; Severity: high
  - Evidence: “Kaelen→Kailun,” multiple quest/location names altered; gallery/venue names swapped (“Duret→Durand‑Ruel”).

- Technical term substitution and calque errors — Specific domain terms replaced with generic or wrong neighbors (materials, engineering, law, sci‑fi).
  - Why: Embedding proximity and low‑frequency term handling; Chinese term chosen correctly but back‑translates literally (e.g., “女儿墙”→“daughter wall”).
  - Frequency: common; Severity: high
  - Evidence: “stonepaste→stoneware”; “tetryon→quadriton”; “parapet” rendered as “女儿墙,” then back as “daughter wall.”

- Additive rationalization: units, time zones, and explanations — Inserts Celsius/metric conversions, Beijing Time, or clarifying phrases not in source.
  - Why: Helpful completion prior; Chinese conventions encourage unit localization; round‑trip then treats additions as source facts.
  - Frequency: occasional to common; Severity: medium
  - Evidence: Weather and sports pieces gained Celsius conversions and time zone notes; “up to ten degrees” became “a full ten °C.”

- Register/ modality shift (formalization or softening/strengthening) — Conversational or legal tone moves to bureaucratic/formal Chinese; modality changes (“should”→“must” or vice versa).
  - Why: Default zh writing norm skews formal; hedging markers not mapped 1:1; politeness insertion (“请”).
  - Frequency: common; Severity: medium–high (style‑critical)
  - Evidence: Legal/tech texts: “licensed” softened to “in good standing”; suggestions turned into commands; curator’s note/marketing copy flattened.

- Quantitative and categorical fact errors — Numeric offers, counts, categories and hazard classes drift.
  - Why: Numeric hallucination/rounding; synonym mapping errors; domain knowledge interference.
  - Frequency: occasional; Severity: high
  - Evidence: “40% off→60% off”; “one arm→two arms” on figurehead; “combustible→flammable” hazard class.

- Domain‑specific substitution in food/culinary — Ingredients/dishes swapped with nearby concepts; methods mislabeled.
  - Why: High synonymy in food lexicon; tendency to localize to familiar terms; “purée→hummus,” “tamari‑ginger glaze→teriyaki.”
  - Frequency: occasional; Severity: high
  - Evidence: “posset→cheesecake”; “parsley→cilantro”; “macerated→candied.”

- Tense/POV/pronoun drift — Present→past; “I”↔“you”; singular they collapses to gendered “he” in back‑translation.
  - Why: Chinese lacks tense morphology and neutral spoken‑written mapping; “ta/他们” ambiguity resolved arbitrarily on return.
  - Frequency: occasional; Severity: medium
  - Evidence: Narratives systematically moved to past; neutral “their” returned as “his.”

- Quote and idiom paraphrase — Direct quotes and idiomatic phrases recast, losing rhetorical force or archaic flavor.
  - Why: Style optimization; idiom mapping via paraphrase rather than lexically faithful choices.
  - Frequency: occasional; Severity: medium
  - Evidence: Historical quotes paraphrased; noir lines rewritten; nautical/journal idioms replaced with generic phrasing.

- Untranslated or extraneous Chinese artifacts — Residual Chinese tokens or editorial notes appear in output; placeholders persist.
  - Why: Mixed‑language training; intermediate notes leak; failure to clean copy.
  - Frequency: occasional; Severity: medium
  - Evidence: Inserted “弦纹,” “推介,” Chinese bracketed notes, and localized placeholders.

## Language‑Specific Factors
- Name handling and transliteration: Chinese often prefers normalized transliterations or semantic names; back‑translation then guesses a plausible English original, causing NE drift.
- Tense/aspect neutrality: Lack of inflection causes default past‑tense back‑translations and loss of narrative immediacy.
- Pronoun ambiguity: Gender‑neutral “ta/其” returns as “he,” and singular they is often gendered or pluralized.
- Register defaults: Written Chinese trends formal; politeness markers and bureaucratic phrasing surface even when not present.
- Term granularity: Many technical terms have multiple Chinese renderings; “parapet=女儿墙” is correct in zh but literal back‑translation (“daughter wall”) is wrong.
- Local conventions: Automatic addition of units (°C, metric) and time zones due to localization norms.

## Numbers, Units, and Formatting
- Units: Adds metric/°C or time zones not in source; sometimes amplifies bounds (“up to 10”→“full 10°C”).
- Counts and percentages: Occasional invention or change (discounts, player stats), and range qualifiers (“at least”) dropped.
- Tables/markup/URLs: Minor format normalization (URL styling, bracket/quote removal) and heading/title capitalization changes.
- Sports/stat jargon: Category swaps (“scrimmage yards→all‑purpose yards”), shot types misread.

## Meta/Disclaimer Behavior
- Politeness and editorial insertions: Adds “请,” explanatory parentheticals, or Chinese notes.
- Safety/legal hedging: Strengthens/weakens modality in legal/HR texts (“preferred” vs “nice‑to‑have,” “must” vs “should”).
- Placeholder leakage: Non‑English placeholders persist in back‑translation.

## Recommendations
- Prompting
  - Enforce copy fidelity: “Do not add explanations, units, or time zones. Preserve all names/titles/URLs exactly as in source. Do not translate or normalize proper nouns.”
  - Lock register/tense: “Match the original tone and tense; do not formalize or add politeness markers.”
  - Glossary injection: Provide inline term locks for domain documents (e.g., parapet=女儿墙, and back‑translation must return ‘parapet,’ not a literal gloss).
  - Quotes mode: “Render direct quotes verbatim; avoid paraphrasing idioms or historical citations.”
- Decoding
  - Lower temperature/top‑p; increase repetition penalty on named entities; use constrained decoding with copy‑span bias for capitalized tokens and numbers.
  - Enable NE and number copy mechanisms (span pointer) in inference if available.
- Data and fine‑tuning
  - Augment with high‑fidelity zh‑en corpora emphasizing NE preservation and domain glossaries (law, materials, sports, sci‑fi, culinary).
  - Contrastive fine‑tunes penalizing additions (units/time zones) and paraphrase of quotes; include pairs with “女儿墙↔parapet” to avoid literal back‑translations.
  - Style sets where colloquial/register must be preserved; modality calibration sets (“should/must/may”).
- Post‑processing
  - NE consistency checker: Diff source vs translation/back‑translation for names, titles, locations, branded terms.
  - Numeric validator: Detect changes in digits, ranges, percentages, currencies, and hazard classes; flag unit insertions or conversion text.
  - Quote integrity: Ensure quoted spans remain quotes, unchanged punctuation/brackets where required.
  - Pronoun/tense audit: Flag gendered pronouns introduced in back‑translation and global tense shifts.
  - Culinary/jargon lexicons: Lint food and domain terms against whitelist to catch “hummus”/“posset”/sports jargon swaps.
  - Artifact scrubber: Detect stray Chinese tokens and placeholder leakage before finalization.

## Confidence and Coverage
- Confidence: Medium‑high. Patterns recur across many judges and domains with consistent symptoms (NE drift, technical term swaps, unit additions, formalization).
- Coverage limits: Evidence is round‑trip and multi‑judge; some issues may originate from the reverse direction. Topic mix is broad but skewed to news/tech/lit; fewer examples in medical/regulatory.