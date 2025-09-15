# Failure Model — Qwen 3 Max Preview — Polish

## Summary
Qwen 3 Max Preview produces fluent Polish but shows recurrent fidelity slips when domain specificity matters. The largest weaknesses are: (1) semantic substitutions of near‑neighbors (ingredients, species, technical terms) that flip facts; (2) degradation of domain terminology and register (legal, patent, archaeology, enterprise) into generic Polish; (3) instability on named entities and category labels (tarot suits, game content, job titles); (4) time/number handling errors (AM/PM, measurements, frequency labels); and (5) subtle tone and idiom flattening. Most problems are not grammar‑level Polish errors but term selection and precision issues that alter meaning.

## Top Failure Modes
- Semantic near-neighbor swaps in concrete nouns and methods — Replaces specific items with related but wrong ones (food, fauna, tools, materials, actions), sometimes producing absurdities.
  - Why: lexical proximity + frequency bias in Polish corpora (e.g., koper vs koper włoski), weak disambiguation for low-frequency domain terms, and over-eager synonymization.
  - Frequency: common; Severity: high
  - Evidence: recipe: “fennel/sea bass/charred” became “dill/cod/carbonated”; field note: “sandy loam” → “clayey sandstone”.

- Domain term precision loss (legal, patent, technical) — Key terms are generalized or mapped to wrong Polish counterparts, altering scope or function.
  - Why: limited exposure to exact equivalents and false friend tendencies (“comprises” vs “consists of”), Polish preference for plainer style unless constrained.
  - Frequency: common; Severity: high
  - Evidence: patent: “cam actuator/comprises” → “wedge drive/consists of”; legal: “minute order” → “trial minutes”; conservation: “flaking/blanching/craquelure” → “cracking/dulling/cracks”.

- Named-entity and label drift — Tarot suits, game quests/abilities, artifact IDs, job titles, and card names change or generalize.
  - Why: entity normalization and low-confidence mapping for niche lexicons; tendency to replace rare with familiar forms.
  - Frequency: occasional; Severity: medium-high
  - Evidence: tarot: “Pentacles” → “Swords”; game: multiple quest/boss/ability names altered; corporate: “CEO” → “Chairman”.

- Measurement, time, and categorical metadata errors — AM/PM handling, numeric labels, and category frequencies are mistranslated or reformatted; sports metrics mislabeled.
  - Why: locale normalization (24h vs 12h), heuristic conversions, and sports jargon ambiguity.
  - Frequency: occasional; Severity: medium
  - Evidence: “6:47 PM” surfaced as “17:47”; side‑effect frequency “Uncommon/Rare” shifted tiers; “vertical jump” mislabeled and “pads” → “blockers”.

- Tone/register drift and idiom literalization — Narrative and professional tone slightly formalized; idioms flattened; puns lost; specialized stylistic cues softened.
  - Why: defaulting to standard written Polish; limited idiom‑to‑idiom mapping; risk‑averse paraphrase.
  - Frequency: common; Severity: medium
  - Evidence: slogan pun (“soles”/“feet”) lost; “bodega” → generic “shop stall”; archaic legal tone modernized; therapy idioms literalized (“blank out” → “forget”).

- Safety/procedure inversion and instruction misinterpretation — Occasional flips in safety steps or process directionality.
  - Why: over‑generalization of synonyms and role inversion when Polish imperative patterns are rephrased.
  - Frequency: rare; Severity: high
  - Evidence: aviation “Brace position” changed from “wrap arms” to “cover head with hands”; hazmat: small‑spill disposal switched nonhazardous↔hazardous.

- Gender/role and agent misassignment — Person or object genders, roles, or agency are swapped.
  - Why: Polish gender marking forces choices under ambiguity; pronoun resolution errors.
  - Frequency: occasional; Severity: medium
  - Evidence: “elderly man” → “woman”; “guard” → “defender”; “It isn’t here” interpreted as “She isn’t here”.

- Residual language and calque artifacts — Untranslated word left in place; false friends that are Polish but shift meaning.
  - Why: mixed-language segments and confidence spikes for common Polish cognates.
  - Frequency: rare; Severity: low-medium
  - Evidence: “significant” surfaced as Polish “istotny” while also reversing advice; one untranslated Polish term remained in a comment.

## Language‑Specific Factors
- Lexical traps: koper (dill) vs koper włoski (fennel); gliny/piaski/iły vs geological “sandstone/loam” mapping; culinary and species names require precise Polish binomials or well-established common names.
- Case and agreement drive disambiguation: missing subjects force gender/number choices, increasing gender and role errors.
- Legal/patent register: Polish equivalents (“obejmuje” for “comprises”, “klin/krzywka” precision) are sensitive; overuse of “składa się z” narrows scope wrongly.
- Idioms and fixed expressions: Polish tends to standardize metaphors; preserving marked register (archaic legal, technical patina) requires explicit constraint.
- Borrowed domain labels often kept in English in Polish practice (game items, tarot suits); replacing with Polish near-synonyms changes canonical meaning.

## Numbers, Units, and Formatting
- Time formats: 12h → 24h conversion inconsistently applied; AM/PM dropped or misinterpreted.
- Sports/tech metrics: mislabeled jump types; report headings normalized (“Lessons Learned” → “Conclusions”).
- Category and frequency tiers: adverse event frequencies shifted one tier; safety labels generalized.
- Tables/metadata: section headers and proper nouns lightly normalized; no systemic table corruption observed.
- Conversions/inventions: occasional reformulation rather than unit conversion; no systematic invention of numbers, but sporadic time and label changes occur.

## Meta/Disclaimer Behavior
- Minimal policy/safety intrusions observed. Contamination manifests more as interpretive additions (e.g., invented phrase in a title) than explicit disclaimers. Triggered by creative or marketing copy and instructions where the model “helps” by rephrasing.

## Recommendations
- Prompt patterns
  - “Translate exactly; preserve all proper nouns, product/quest names, tarot suits, and headings verbatim unless a standard Polish canonical form exists. Do not substitute ingredients/species/materials. Preserve measurements, times (keep AM/PM), and category labels.”
  - Provide a term lock glossary per job: fennel=koper włoski; loam=madziarka/gleba gliniasto‑piaszczysta (domain-specified); “comprises”=“obejmuje”; “minute order”=[leave English or “zarządzenie ‘minute order’”].
  - Add “No paraphrase. No modernization of tone/register. Preserve idioms/puns; if untranslatable, add bracketed note.”
- Decoding/controls
  - Lower temperature/top‑p for translation tasks; enable constrained decoding with lexicon locks for entities and critical terms.
  - Use span-level “do-not-translate” tags around names, abilities, IDs, SKU/tarot labels.
- Data/augmentation
  - Fine‑tune on Polish domain corpora: culinary taxonomy, field ecology, archaeology, legal/patent (PL-EN aligned), aviation safety manuals, pharma labeling frequencies.
  - Hard negative pairs for common confusions: dill↔fennel; cod↔sea bass; loam↔sandstone; “comprises”↔“consists”; “combustible”↔“flammable”; tarot suits mapping.
  - Include AM/PM retention cases and sports reporting jargon aligned to Polish.
- Post‑processing checks
  - Named-entity consistency check against source; flag edits to capitalized tokens and known domain lexicons (tarot, game DBs, artifact IDs).
  - Numeric/time diff checker: detect AM/PM loss or value changes; flag frequency tier terms.
  - Domain term QA: glossary conformance linter; highlight banned substitutions (list of near-neighbor traps).
  - Safety/procedure validator: rule‑based scan for polarity flips (e.g., “flammable/combustible”, “wrap arms/cover head”).
- Workflow
  - For high‑risk domains, run dual-pass: literal pass → stylistic smoothing pass with locked terms; require reviewer checklist for entities, numbers, and domain jargon.

## Confidence and Coverage
- Confidence: moderate. Evidence is dense across many domains with repeated patterns (ingredient/species swaps, technical term drift, time/measurement errors, entity drift).
- Coverage limits: Back-translation judgments may amplify perceived errors; limited exposure to some domains (finance, medicine) in this set; judges vary in strictness. Nonetheless, error families recur across multiple judges and samples, suggesting stable failure modes.