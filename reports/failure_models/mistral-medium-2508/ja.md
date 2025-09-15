# Failure Model — Mistral Medium 3.1 — Japanese

## Summary
Overall, Mistral Medium 3.1 produces fluent Japanese but shows recurrent fidelity drift when translating domain‑specific nouns, proper names, and precise instructions. The most salient weaknesses are: (1) systematic substitution of technical or specific terms (species, materials, legal terms, ingredients); (2) instability in proper names and named entities; (3) numeric/time and unit drift, including unsolicited conversions; (4) tone/register flattening in literary and legal prose; and (5) occasional agency/pronoun errors that flip who did what in dialogue. These errors are often small in isolation but compound to alter facts and reader intent.

## Top Failure Modes
- Term substitution in specialized domains — Replaces precise items with near‑neighbors or generic terms (fauna/flora, materials, legal/culinary/technical vocabulary); why: limited domain grounding + preference for high‑probability synonyms in JP where loanwords and common names are ambiguous; frequency: common; severity: high.
  - Evidence: sea trout→sea bass; curlew/snipe swaps; calf→vellum/sheep; “row‑crop agriculture”→“ridge‑and‑furrow”; legal “venue”→“jurisdiction.”

- Proper name drift and normalization — Alters surnames, spellings, or placeholders; why: phonetic/orthographic normalization in JP (katakana/romanization), plus weak name anchoring; frequency: common; severity: high.
  - Evidence: Ruiz→Lewis; Whitaker‑Ruiz→Whitaker‑Lewis; Rin→Lyn, Kazi→Kaji; Wight→Wheat; Beecher→Beacher; Suri→Suli.

- Ingredient and species confusions in food/nature text — Swaps close culinary items or pests; why: JP common names collapse distinctions (e.g., daikon vs radish) and the model “corrects” to culturally frequent items; frequency: common; severity: high.
  - Evidence: lamb shoulder→pork shoulder; radish↔daikon; cabbage root fly→cabbage moth; vine weevil→click beetle.

- Time/number/unit divergence — Changes exact times/quantities and adds conversions; why: formatting normalization and heuristic “helpfulness” (metricization), plus digit copying errors; frequency: occasional; severity: medium–high.
  - Evidence: 7:45→11:45; 47→48 homes; added metric units and AM/PM where none existed.

- Tone/register flattening and metaphor loss — Literary/poetic/legal nuance becomes generic; why: JP stylistic simplification and synonym preference; frequency: common; severity: medium.
  - Evidence: “hum”→“growl”; “thrilled”→“deeply moved”; legal “Charter”→“constitution,” “voice”→“forum”; poetic meter misdescribed.

- Dialogue/agency misassignment — Subject/tense shifts flip agency or intent; why: JP subject omission and tense smoothing produce wrong back‑mapping; frequency: occasional; severity: high in narrative.
  - Evidence: “It’s not listening”→“I wasn’t listening”; “sat on my knees”→“sat on someone’s lap”; STOP button action reassigned.

- Instructional accuracy errors — Procedural/technical directions altered; why: over‑paraphrase and term confusion under ambiguity; frequency: occasional; severity: high for task texts.
  - Evidence: “three‑down‑capable”→“situational third‑down back”; Hot Cross Buns note positions mislocated; patent “collapsible”→“foldable” and claim structure reformatted.

- Structural/formatting meddling — Reformatting into lists, bolding, added glosses/headers; why: helpfulness bias + template priors; frequency: occasional; severity: low–medium.
  - Evidence: Patent claims reformatted as lists; added bold and Japanese term glosses; added explanatory parentheticals.

## Language‑Specific Factors
- Subject omission and neutral tense in Japanese encourage back‑translation errors in agency and temporality (e.g., listener vs speaker responsibility, past vs progressive).
- Lack of plural marking and frequent genericization push singular↔plural and species generalization (waders, godwits→dowitchers; pests).
- Katakana/phonetic confusions and homophones make proper nouns brittle (Wight/Wheat), and L/R alternations surface in names (Li→Ri).
- Common‑name mapping in JP for species/ingredients encourages culturally dominant defaults (daikon for radish; sea bass vs trout).
- Legal and technical term pairs with near equivalents but different scope (venue/jurisdiction; bulk vs primary fermentation) are easily conflated due to overlapping JP renderings.
- Literary register: tendency toward 丁寧体/neutral prose flattens metaphor and meter; English meter analysis terms are weakly grounded in JP data.

## Numbers, Units, and Formatting
- Unsolicited metric conversions and AM/PM annotations appear in otherwise literal translations.
- Occasional digit drift or substitution in times and counts (7:45→11:45; 47→48).
- Reformatting structured prose into lists or adding headings/quotes; minor capitalization/tense normalization (titles, cues).
- Rare unit invention or role mislabeling (e.g., three‑down capability reframed).

## Meta/Disclaimer Behavior
- Occasional translator notes, glosses, or added parentheticals (e.g., adding Japanese names, explanatory terms) that were not in source.
- No strong safety/policy disclaimers observed; intrusions are primarily “helpful” additions and clarifications.

## Recommendations
- Prompting
  - Use a strict, testable instruction block: “Translate exactly. Do not add, omit, explain, or convert units. Preserve names, numbers, times, and formatting. Keep register and metaphor. Keep legal/technical terms as given unless a standard JP term exists; otherwise transliterate and footnote only if explicitly asked.”
  - Provide a locked glossary for: proper names, species/ingredients, legal terms, bindings/materials, game terms. Example: GLOSSARY: Ruiz, Whitaker‑Ruiz, ox, sea trout, calf binding, venue, bulk fermentation.
  - Add constraints: “Do not pluralize/singularize unless explicit; do not normalize street types; do not translate surname spellings.”
- Decoding
  - Lower temperature and reduce paraphrase (e.g., nucleus p ≤ 0.8; temperature 0.2–0.4).
  - Use constrained decoding for protected spans: wrap names/numbers/units in markers and force copy (e.g., <LOCK>Ruiz</LOCK>, 7:45).
- Data and fine‑tuning
  - Fine‑tune on parallel JP–EN corpora emphasizing: legal contracts (venue vs jurisdiction), bookbinding/bibliography, field guides (birds/fish/invertebrates), culinary recipes/ingredients, technical instructions and music pedagogy.
  - Add negative examples penalizing unsolicited unit conversions and structural reformatting.
  - Curate lexicons for species/materials/culinary pests with JP–EN disambiguation; include contrastive pairs (sea trout vs sea bass; godwit vs dowitcher; calf vs vellum).
- Post‑processing QA
  - Automated diff checks for named entities, numbers, times, units, addresses; flag any change.
  - Domain validators: species/materials dictionary lookup; legal term map consistency (venue≠jurisdiction).
  - Style checker to detect register shifts (commands vs recommendations) and added gloss markers or bolding.
  - Round‑trip unit test set: verify no unit conversions, no street type renaming, no surname changes.

## Confidence and Coverage
Confidence: medium–high. The failure modes are supported by many low‑scoring judgments across diverse domains (literary, legal, technical, culinary), with repeated patterns (term substitution, name drift, numeric/time changes). Coverage limits: evidence is round‑trip and multi‑judge; some issues may reflect back‑translation artifacts. Safety disclaimers were rare; meta additions were mild and “helpful,” so that mode may be underrepresented.