# Failure Model — DeepSeek V3.1 Reasoner — Japanese

## Summary
DeepSeek V3.1 Reasoner shows strong fluency but recurrent fidelity drift when translating to/from Japanese, especially around proper nouns, domain terms, voice/politeness, and idioms. The most salient weaknesses are: (1) frequent proper-name and label mutations; (2) domain-specific terminology swaps (culinary, species, gaming, sports, traffic/engineering, legal); (3) person/pronoun and tone/politeness shifts; (4) idiom/metaphor flattening and dialogue misreads; and (5) light but recurring additions/omissions. These tend to be triggered by Japanese ambiguity (subjects omitted, plural-neutral), katakana/near-synonym priors, and a bias toward normalization/formality.

## Top Failure Modes
- Proper names and titles drift — Changes or misspellings of names, places, ranks, and product/series titles; often “normalized” to a more common form.
  - Why: Copy bias toward familiar strings; katakana/romanization ambiguity; weak name copying constraints in JP contexts.
  - Frequency: common; Severity: high
  - Evidence: “Whitaker-Ruiz→Whittaker Lewis”; gallery “Duret→Durand”; brand “Anker→Anchor”; police ranks swapped (Detective↔Sergeant/Captain).

- Domain term substitution (food, species, technical) — Specific items replaced with nearby but incorrect terms.
  - Why: Katakana/near-synonym priors; lack of domain lexicons; pressure to naturalize JP terms.
  - Frequency: common; Severity: high
  - Evidence: “chickpea↔lentil,” “tamari→teriyaki”; birds swapped (curlew/snipe/woodcock, dunlins→plovers); “beef shank→tendon,” “peppercorn→sesame”; “protected left-turn phase→lane”; “oven spring→oven lift.”

- Person/voice and politeness drift — Shifts in grammatical person (I↔we, you↔I), and tone lifting to formal/polite or adding honorifics.
  - Why: JP subject omission and politeness system; model’s default to 丁寧体; heuristic rephrasing on back-translation.
  - Frequency: common; Severity: medium
  - Evidence: “I→we” in policy/narrative pieces; adding “-san” to names; “Check your email→Verify Email” plus added “Please.”

- Idiom/metaphor and dialogue misinterpretation — Literalization or semantic flips in idioms, metaphors, and sports slang; pronoun roles reversed in dialogue.
  - Why: Preference for safe literal renderings; difficulty with JP discourse deixis; loss of subtext in back-translation.
  - Frequency: occasional; Severity: high (when story/relationship changes)
  - Evidence: “celebration of life→funeral”; football jargon (“runs behind his pads,” “clean hits”) misrendered; dialogue person reversal changing dynamics.

- Game/fiction and spec term renaming — Class/skill/zone names altered; severity/obligation modals shifted (must↔should).
  - Why: Over-generalization to generic terms; style normalization; low-cost synonym swaps in JP→EN pass.
  - Frequency: occasional; Severity: medium
  - Evidence: ability names changed; “burglary→robbery”; “must→should” and vice versa in requirements.

- Minor additions/omissions and register smoothing — Dropped qualifiers (“hint of,” safety specifics), added glosses/parentheticals; slight time/date conversions.
  - Why: Concision/expansion normalization; JP ellipsis; formatter heuristics.
  - Frequency: occasional; Severity: low–medium
  - Evidence: omitted “shaved/toasted”; added parenthetical notes; “4:00→16:00.”

## Language‑Specific Factors
- Subject/pronoun omission and topic–comment structure encourage person drift (I↔we/you) and dialogue role confusion.
- Politeness and honorifics push toward 丁寧体 and additions like “-san,” softening tone or changing register.
- Number/plural neutrality leads to singular/plural mismatches and countable noun drifts.
- Katakana and near-synonyms bias substitutions (tamari/teriyaki, tendon/shank), and false friends in technical jargon.
- Script and naming: romanization/kana choices increase name/rank/title mutation; English capitalization cues lost in JP.
- Idioms/onomatopoeia: JP-specific sound symbolism and idioms get flattened or replaced; English idioms literalized back.

## Numbers, Units, and Formatting
- Mild but recurring issues:
  - Time formats converted (4:00→16:00); week/timeframe shifts (“next week” vs “this week”).
  - Singular/plural inventory changes; loot/item counts fluctuate slightly.
  - Headings/labels normalized (“Price Target↔Target Price”).
  - Rare unit/term downgrades (duty/load; dry weight/weight).
- Conversions/inventions: Generally no unit invention, but occasional title/label reinterpretation that implies changed scope.

## Meta/Disclaimer Behavior
- Occasional translator notes or glosses injected, and politeness padding (“please,” definitions for waxing/waning).
- Triggered by instructional, UI, or public-facing tones where the model aims for helpfulness or clarity.

## Recommendations
- Prompting
  - Use hard constraints: “Preserve all proper names, ranks, product titles, and numbers verbatim; no additions or explanations; maintain original person (I/you/we) and tone/register.”
  - Provide a term lock/glossary block (e.g., Ingredients, species, game skills, traffic terms) and instruct “Do not substitute.”
  - Specify style: “Use plain form if source is informal; do not add honorifics; avoid paraphrase.”
  - For dialogue: “Track speaker roles; keep pronouns and deixis aligned with source; no normalization.”
- Decoding/constraints
  - Apply copy-bias or constrained decoding for capitalized tokens, named entities, and numeric strings.
  - Penalize novel token introduction; lower temperature; enable length/verbosity penalty to curb expansions.
  - Use lexicon constraints for high-risk domains (culinary, ornithology, sports, gaming, traffic/engineering, legal).
- Data and fine‑tuning
  - Augment with parallel corpora emphasizing: JP–EN dialogue with omitted subjects; domain glossaries; idiom/football/gaming jargons; menus/species lists; traffic/urban-planning terms.
  - Contrastive pairs: penalize “tamari→teriyaki,” “curlew↔snipe,” rank/title swaps, and “must↔should” flips.
  - Politeness-control fine-tune to faithfully mirror source register without honorific injection.
- Post‑processing QA
  - Named-entity and terminology alignment check against source; flag edits to ranks, titles, SKUs, dish/ingredient names, species, class/skill names.
  - Person/voice and politeness lint: detect “I↔we/you” shifts and added honorifics.
  - Idiom/jargon detector: flag literalizations of known idioms/jargon lists.
  - Numbers/time diff: highlight unit/format/timeframe changes and pluralization drift.
  - Style diff for headings/labels to prevent normalization of UI/legal headers.

## Confidence and Coverage
- Confidence: medium-high. Patterns are consistent across many judges and domains with repeated name/term and tone/person drifts.
- Coverage limits: Evidence is from low-scoring round-trips across mixed genres; fewer hard numerical/unit failures; less visibility into very technical STEM/math units. Judges are diverse, but some domains (e.g., medicine, finance tables) are underrepresented.