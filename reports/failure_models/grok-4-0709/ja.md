# Failure Model — Grok 4 — Japanese

## Summary
Grok 4 generally preserves meaning in JA↔EN round‑trip, but exhibits recurring issues: (1) register drift toward polite or generic Japanese that flattens style and weakens modality; (2) instability on domain terms and named entities (meteorology, law, biology, bibliographic), sometimes reversing nuance; (3) number/units formatting artifacts (digit‑by‑digit readings, Japanese numeral carry‑over); (4) occasional critical semantic flips in procedural instructions; and (5) rare but severe failures to fully back‑translate (segments left in Japanese).

## Top Failure Modes
- Untranslated segments in back‑translation — Some BACK outputs contain intact Japanese spans instead of English; why: weak enforcement of target language in multi‑segment tasks and early‑stop/format confusion; frequency: rare; severity: high
  - Evidence: “BACK includes large sections in Japanese”; “large sections…not back‑translated into English.”

- Register/tone drift to polite/generic Japanese — Consistent softening or formalization: MUST/SHOULD annotated or moved, “Please” added, colloquial/journalistic/poetic edges flattened; why: Japanese politeness defaults and loss of modality cues across JA morphology; frequency: common; severity: medium
  - Evidence: “added ‘Please’”; “MUST/SHOULD moved to parentheses, altering technical style.”

- Domain‑term substitution and nuance inversions — Key terms swapped with near‑neighbors (explanatory↔descriptive, burglary↔robbery, onshore flow→land winds, combustible→flammable), species/tool mismaps (curlew→dunlin; sea trout→sea bass; mallet→hammer); why: lexical ambiguity, dictionary bias, and insufficient domain grounding; frequency: occasional; severity: high
  - Evidence: “explanatory→descriptive changed the critique”; “onshore flow→land winds”; “combustible→flammable.”

- Named entity and proper noun instability — Character and place names drift (Rin Vale→Lin Veil; Eli→Eri), capitalization misread as common nouns (Lichens as a name); why: katakana/romanization noise and casing loss across scripts; frequency: occasional; severity: medium
  - Evidence: “Name alteration (Rin Vale→Lin Veil)”; “lichens becomes a name.”

- Numbers and measurement artifacts — Odd numeral rendering (“one seven five zero”; “four zero paces”), Japanese numeral echoes, side‑effect frequency thresholds and hazard categories mislabelled; why: script/format normalization errors and over‑literal back‑reading of kanji digits; frequency: occasional; severity: medium
  - Evidence: “Japanese numeral artifact”; “‘one seven five zero’ digit reading.”

- Person/number/tense mismatches — Singular/plural, we→they, tense flips; weak retention of markers absent in Japanese; why: zero‑morphology for number/tense in JP surface form; frequency: common; severity: low–medium
  - Evidence: “we→they,” “pluralized/singularized items,” “tense shifts.”

- Critical instruction flips in procedures — Safety‑relevant inversions (face‑down→face‑up), tool substitution (rubber mallet→hammer); why: rephrasing underdetermination and synonym overreach in imperative lists; frequency: rare; severity: high
  - Evidence: “Step 7 reverses face‑down to face‑up”; “hammer vs rubber mallet.”

- Idiom/imagery degradation in literary/poetic text — Ox→bull, corners→horns, meter and wordplay lost; why: preference for generic synonyms and difficulty preserving metaphor under JA rephrasing; frequency: common; severity: medium
  - Evidence: “ox→bull; ‘corners’→‘horns’”; “wordplay (‘settle’) lost; colloquial tone flattened.”

## Language‑Specific Factors
- Politeness and modality: Japanese defaults to polite/indirect forms; MUST/SHOULD/imperatives often softened or relocated, changing normative force.
- Number and plurality: Lack of obligatory number marking yields frequent singular/plural drift on back‑translation; collective nouns, counts, and list quantities are fragile.
- Script and casing: Loss of capitalization and katakana transliteration cause proper noun confusion and common‑noun reinterpretation (e.g., Lichens vs lichens).
- Lexical conflation: Homologous pairs prone to swap due to JA lexical overlap (burglary/robbery; explanatory/descriptive; combustible/flammable).
- Meteorological terms: “陸風/海風” vs “onshore/offshore flow” mapping is error‑prone; wind directionality often inverted or generalized.

## Numbers, Units, and Formatting
- Digit‑by‑digit readings and kanji carry‑over reappear in BACK (“one seven five zero,” “two zero shillings”).
- Frequency/threshold categories and hazard classes relabeled, subtly changing regulated meanings.
- Minor unit omissions (“metric” dropped) and pluralization of counts.
- Occasional table/heading reshaping and parenthetical insertions around modality keywords.

## Meta/Disclaimer Behavior
- Added parenthetical notes or glosses for standards/terms (e.g., MUST/SHOULD annotations; STRIDE expansions).
- Politeness additives (“please consider,” “I request confirmation”) in otherwise neutral text.
- These intrusions surface in technical, policy, or instruction‑heavy content where the model “helps” by clarifying.

## Recommendations
- Prompting
  - Enforce target language and fidelity: “Back‑translate to English only. No Japanese text, no annotations, no added explanations. Preserve numbers, names, and modality exactly.”
  - Register locks: “Preserve modality tokens (MUST/SHOULD), formality, and tone; do not add ‘please’ unless present.”
  - Numeral handling: “Copy numerals and units verbatim; do not spell out digits unless spelled out in the source.”
  - Glossaries: Provide per‑doc term maps for high‑risk domains (meteorology, legal, medical, conservation species, bibliographic).
  - Name guards: “Preserve proper nouns and character names exactly as given.”
- Decoding
  - Lower temperature/top‑p for procedural and legal texts to reduce synonym drift and inventions.
  - Enable constrained decoding with phrase lists for MUST/SHOULD, hazard classes, species, tool names.
- Data and fine‑tuning
  - Augment with JA↔EN parallel corpora emphasizing modality, technical standards, meteorology, legal distinctions, species common/latin names, bibliographic terms.
  - Include style‑preservation objectives (penalize unnecessary politeness and paraphrase in round‑trip).
  - Numeral/units training with diverse historical and archaic forms to prevent digit‑by‑digit artifacts.
- Post‑processing QA
  - Automated checks: language‑ID for BACK; numeric/units diff; detection of modality tokens; named‑entity consistency; hazard/frequency label mapping.
  - Heuristics for wind/flow directionality, legal charge types, procedural polarity (face up/down), and tool types.
  - Flag parenthetical insertions not present in source.
  - For literature: dual‑pass with “literal pass” + “style pass” and a final constraint to keep metaphors/wordplay alignments.

## Confidence and Coverage
- Confidence: medium‑high for identified patterns; many judges and diverse docs corroborate key modes.
- Coverage limits: Evidence skewed toward mid–high scores with a few low‑score outliers; severe failures (untranslated segments, instruction inversions) are rare but impactful; domain breadth wide but deepest signals in technical, literary, and procedural texts.