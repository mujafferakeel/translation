# Failure Model — Kimi K2-0905 — Japanese

## Summary
Overall, Kimi K2-0905 produces fluent Japanese but shows recurrent fidelity failures when precision matters. The most salient weaknesses are: (1) substitution of semantically adjacent terms (species, ingredients, roles, technical nouns), (2) numeric/unit errors (money, temperature, timing), (3) polarity and instruction reversals (negation, safety directives), (4) perspective and modality drift (2nd→1st person; SHOULD↔MUST), and (5) proper-noun instability (names, places, titles). These issues are amplified by Japanese-specific ambiguities (subjects, plurality, register) and the model’s tendency to paraphrase.

## Top Failure Modes
- Entity/term substitution (species, food, technical nouns) — Replaces specific items with near-neighbors or broader categories, changing facts or domain meaning. Why: weak grounding for domain lexicons; Japanese common-name variability; paraphrase bias. Frequency: common; Severity: high.
  - Evidence: “black bear” rendered as “grizzly,” altering hazard; “reed bunting/horsefly/snipe” swapped to other birds/insects; “lemon chickpea purée” → “lemon chili purée,” “tamari” → “tamarind.”

- Numbers and units corruption — Misreads or converts values incorrectly (currencies, temperatures, tide timings, counts). Why: automatic normalization/conversion; Japanese unit phrasing ambiguity; copying vs converting inconsistency. Frequency: common; Severity: high.
  - Evidence: $78M became $312M; 40–70°F interpreted as “4–70°C”; low tide timing shifted; added/changed units.

- Polarity and instruction inversions — Negation, safety guidance, or constraints are flipped. Why: Japanese negation scope and litotes; paraphrase simplification; imperative vs advisory mapping. Frequency: occasional; Severity: high.
  - Evidence: “never make eye contact” became “lock eyes”; “descend 30 m below the crest” turned into action “from the summit”; “uncontrolled ricochet” became “controlled.”

- Perspective, modality, and tense drift — Shifts person (you→I/we), weakens/strengthens modality (SHOULD↔MUST), and toggles tense. Why: Japanese zero pronouns; modal verbs mapped to Japanese evidentials; narrative style smoothing. Frequency: occasional; Severity: medium.
  - Evidence: Second person narrative rendered as first person; RFC-style “SHOULD” strengthened to “MUST”; past→present tense shifts reducing suspense.

- Proper nouns, roles, and titles instability — Names, places, titles altered or normalized. Why: katakana variants, title mapping (Commissioner/Chair), kana/romanization noise; over-normalization. Frequency: occasional; Severity: medium.
  - Evidence: Mayor’s name misspelled; “Commissioner” → “Chair”; festival and surnames altered (Ruiz→Lewis; Jensen→Jenson).

- Register flattening and stylistic paraphrase — Poetic/archaic/legal or technical register simplified; idioms neutralized. Why: decoder preference for common phrasing; Japanese stylistic norms; alignment for readability. Frequency: common; Severity: medium.
  - Evidence: Archaic legal tone simplified; “invaluable” softened to “valuable”; precise conservation terms replaced by general ones.

## Language‑Specific Factors
- Zero pronouns and plurality: Japanese omits subjects and lacks strict plural marking, encouraging POV shifts and singular↔plural errors (e.g., “we slept” → “you slept”; “rings” → “ring”).
- Negation scope and polarity: Constructs like “決して〜しない/〜しないように” are mis-scoped, leading to inversions (“never do X” → imperative to do X).
- Modality mapping: English RFC/legal modals (SHOULD/MUST/MAY) map imperfectly to Japanese (“べき/しなければならない/してもよい”), often strengthening or weakening force.
- Title and role mapping: English role granularity vs Japanese titles (〜長, 委員/局長) causes “Commissioner” ↔ “Chair/Director” drift.
- Lexical ambiguity in common names: Japanese vernacular names for fauna/flora/food overlap or differ by region; model normalizes to frequent alternatives (e.g., seabirds, shorebirds, herbs).
- Loanwords and domain terms: Katakana choices (ホームセンター vs hardware store) and technical terms (ゲージ vs 直径) encourage generalization (“gauge” → “diameter”).
- Register and discourse: Tendency to smooth idiomatic imagery and archaic/legal diction into standard polite style, flattening tone.

## Numbers, Units, and Formatting
- Currency magnitudes: Zero miscounting/rewriting large numbers (M/B) and inserting alternative figures ($78M→$312M).
- Temperature conversion: Auto-conversion without context (40–70°F→4–70°C) or unit confusion (°F/°C misread).
- Times/tides/durations: Shifts in timing and sequence (low vs high tide; afternoon vs evening).
- Lists/specs: Plural/singular toggles in bullet points; “not included” → “sold separately”; added units not in source.
- Conversions/inventions: Occasional invented units or added specifics to “clarify.”

## Meta/Disclaimer Behavior
- Limited explicit safety/policy disclaimers. Minor authorial intrusions include explanatory additions (“as we now tell it”), quote paraphrases, or softening/strengthening advisories. Triggered by narrative or safety content; generally rare.

## Recommendations
- Prompting
  - Enforce fidelity: “Translate literally, do not paraphrase. Preserve numbers, units, titles, names, capitalization, and person/tense/modality. Do not convert units or currencies. Preserve negations exactly.”
  - Term locks: Provide a glossary for domain terms (fauna/flora, culinary items, legal/RFC modals) and require exact matches. Example: SHOULD=「〜すべき」, MUST=「〜しなければならない」.
  - Safety flag: “If uncertain about species/measurements/tides, retain the source term in quotes and add [?] rather than substituting.”
  - Perspective guard: “Maintain original person (I/you/we), voice, and tense throughout.”
- Decoding
  - Reduce temperature/creativity (lower sampling) for technical/legal/safety texts.
  - Bias towards copy for digits, units, named entities (constrained decoding or protected spans).
- Data and fine‑tuning
  - Augment JP parallel corpora with high‑precision domains: field guides, menus, conservation papers, legal/RFC texts. Emphasize mappings for species names and culinary terms.
  - Curate contrastive pairs targeting negation scope, modality strength, and POV preservation.
  - Add unit/currency non‑conversion training examples and tests.
- Post‑processing checks
  - Numeric/unit validator: Diff all numbers/units; block F↔C conversions unless explicitly requested.
  - Negation/polarity checker: Heuristics to catch inversions (“never,” “do not,” safety imperatives).
  - NER consistency: Ensure names, titles, and places are unchanged; cross‑check against source.
  - Domain lexicon checker: Species and ingredient dictionaries to flag out‑of‑set substitutions.
  - Modality auditor: Enforce SHOULD/MUST/MAY mappings in technical/legal outputs.
  - POV/tense monitor: Compare pronouns and tense markers against source; alert on shifts.

## Confidence and Coverage
Confidence: moderate. The sample set spans diverse domains with multiple independent judges. Evidence is dense for species/ingredients substitutions, numeric/unit errors, and polarity/POV shifts, including several high‑impact safety and finance cases. Less evidence for meta/disclaimers. Findings reflect observed failures in Japanese round‑trip translations specifically for Kimi K2‑0905 across narrative, technical, legal, and advisory genres.