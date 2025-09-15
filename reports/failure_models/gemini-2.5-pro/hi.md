# Failure Model — Gemini 2.5 Pro — Hindi

## Summary
Overall, Gemini 2.5 Pro’s Hindi translations keep core meaning but systematically flatten nuance and precision. The most salient weaknesses are: recurrent temporal/quantity drift (e.g., noon→afternoon), loss of domain-specific terminology, named-entity and label corruption, stylistic/idiomatic literalization (especially poetry/voice), and low‑level content inventions like parenthetical explanations. Errors are usually small but frequent and cumulative, leading to tone shifts and factual nudges that matter in technical, legal, or safety contexts.

## Top Failure Modes
- Temporal and quantitative drift — Time and magnitude terms shift toward looser Hindi paraphrases (e.g., “noon” rendered as “afternoon”; quantifiers softened/hardened). Why: Hindi ambiguity around dopahar/madhyahna and preference for generic quantifiers; decoder bias toward naturalness. Frequency: common; Severity: medium/high when deadlines or specs matter. Evidence: “Friday at noon” became “Friday afternoon” repeatedly; “steady breeze” context also saw “midday” drift to “afternoon,” and “several”→“many.”
- Terminology dilution (technical/legal/business/safety) — Precise terms mapped to generic synonyms, changing register and sometimes meaning (hazard classes, legal burdens, security terms, finance). Why: Sparse parallel data for high-register Hindi; model defaults to colloquial or Indian‑English approximations. Frequency: common; Severity: high in regulated contexts. Evidence: “Combustible liquid”→“Flammable liquid”; legal “suspicion”→“doubt,” “compelling”→“irrefutable”; security “exploited”→“used”; business “pivot/lead time”→generic phrasing; finance “forward EPS/trade‑down” lost.
- Named-entity and label corruption — Proper names, model/product names, and variety names altered or respelled; category labels softened. Why: Transliteration normalizes to common Indian spellings; beam search favors familiar variants; article/case loss in Hindi. Frequency: occasional; Severity: medium. Evidence: Tomato “Ember”→“Amber”; rover “Aster‑3”→“Ester‑3”; personal names “Sarah”→“Sara”; wine profile “layers”→“a light layer.”
- Figurative/stylistic flattening and idiom literalization — Metaphors, onomatopoeia, and poetic diction replaced with literal or safer verbs; register shifts from evocative to prosaic. Why: Preference for clarity; idiom transfer gaps; Hindi poetic compactness expanded into explicatives. Frequency: common; Severity: medium (high for literary content). Evidence: “ghost”→“reflection”; “future’s cheek”→“course of the future”; “cradling/bleeding” toned down; rhyme/onomatopoeia altered (“tan‑tan”, “Shhh”).
- Category/attribute substitutions (near‑neighbor mix‑ups) — Semantically close but wrong items swapped (species, tools, ingredients, materials), or attributes drift (color/intensity). Why: Embedding proximity + sparse domain lexicon; Hindi general words encourage substitution. Frequency: occasional; Severity: medium. Evidence: “geese”→“ducks”; “trowel”→“hoe”; “thyme”→“ajwain”; “smirk”→“smile,” “glare”→“stare”; “pale”→“yellow.”
- Physical/environmental condition flips — Key physical descriptors inverted or blunted, affecting factual plausibility. Why: Hindi lexical gaps for fine granularity (breeze vs still air) and collocation bias. Frequency: occasional; Severity: high in instruction/safety. Evidence: “steady breeze”→“still air” for kite; “icy” vs “frigid”; “high spring tides”→“high tide.”
- Number, plurality, and gender drift — Singular/plural, grammatical gender, and animacy/personification shift; pronoun changes. Why: Hindi unmarked plurals, optional articles, and gender agreement complexity; “she” for ships vs neutral in Hindi. Frequency: occasional; Severity: low/medium. Evidence: ship “she”→“it”; plural “neon signs”→singular; “her shadow”→“his”; plural seams→“cracks.”
- Additions/parenthetical “translator notes” — Unrequested explanatory glosses or clarifications introduced. Why: Helpful‑explainer prior; uncertainty triggers explanatory behavior. Frequency: occasional; Severity: low/medium. Evidence: Added lunar terms (Shukla/Krishna Paksha); parentheticals like “shelled creatures” or explaining “craquelure.”

## Language‑Specific Factors
- Time terms: dopahar/dopahar ke baad vs madhyahna cause noon→afternoon drift; evening/dusk ambiguity (sandhya/shaam) nudges timing.
- Articles and plurality: Hindi lacks articles and often unmarked plurals, leading to singular/plural ambiguity and added/omitted definiteness.
- Gender and agreement: Morphological gender inflection can flip pronouns or force defaults; English personification (“ship = she”) rarely preserved.
- Register and code‑mix: High‑register legal/technical Hindi is niche; model gravitates to colloquial Hindi or English calques, losing domain tone.
- Idioms and onomatopoeia: English idioms/onomatopoeia lack 1:1 Hindi equivalents; the model often literalizes or substitutes with common Indian sound words, altering style.
- Transliteration variability: Proper names and product terms get normalized to common Indian spellings (Sara for Sarah; Li→Lee), risking factual shifts.

## Numbers, Units, and Formatting
- Time/date modality: Conditional→future (“would/should”→“will/must”) stiffens plans; deadline precision softened (“noon”→“afternoon”).
- Quantifiers/scales: “Up to 40%” softened; “several”→“many”; intensity adjectives downgraded/upgraded.
- Category labels: Hazard classes misrendered (combustible vs flammable).
- Lists/tables: Occasional renaming of headings (Itinerary→Travel Plan); pluralization inconsistencies.
- Regional variants: Indian English bleed-through (torch for flashlight) acceptable in some contexts but can alter tone/register.

## Meta/Disclaimer Behavior
- Occasional parenthetical explanations or glosses when encountering rare terms or culture‑specific items (“craquelure,” lunar notes; fauna glosses).
- Softening of claims (“likely”→“possibly”) appears as risk‑averse paraphrasing, not formal safety disclaimers.
- Triggers: Low familiarity, technical art/history terms, culture‑specific festivals or astronomy; promotional/safety copy with precise claims.

## Recommendations
- Prompting
  - Lock critical entities and terms: “Preserve all named entities, product names, and times exactly; do not transliterate or respell. Keep deadlines and numbers unchanged.”
  - Style guardrails: “Maintain original register (legal/technical/poetic). Do not simplify metaphors or add explanations.”
  - Time disambiguation: “Translate ‘noon’ as ‘madhyahna (12 baje dopahar)’; ‘dusk’ as ‘sandhya (dhalti shaam)’; retain exact times.”
  - Term glossaries: Provide per‑document glossaries (hazard classes, finance, legal burdens) and instruct “use exactly as given.”
- Decoding
  - Lower temperature/top‑p for high‑precision content; increase length penalty to discourage verbose explicatives; penalize parentheses and bracket insertions.
  - Constrained decoding with lexicon locks for hazard labels, legal terms, and named entities.
- Data
  - Augment parallel Hindi corpora for: EHS/hazard standards; legal burdens of proof; finance (equity research jargon); security/privacy; maritime/meteorological lexicon; culinary/tools taxonomy.
  - Curate idiom/metaphor pairs and onomatopoeia mappings Hindi↔English with style tags.
  - Include time-expression contrastive pairs (noon vs afternoon; dusk vs evening) and evaluation sets for deadline preservation.
- Fine‑tuning targets
  - Objectives that penalize temporal and numeric drift; entity consistency losses; register shifts on tagged corpora (legal/technical/poetic).
  - Multi‑task with NER and term normalization heads to improve named-entity fidelity.
- Post‑processing
  - Automated QA: Detect and flag changes in times/dates/numbers, hazard classes, units, and named entities via alignment checks.
  - Lexicon checks: Verify presence/exactness of glossary terms; diff for “noon/afternoon,” “combustible/flammable,” etc.
  - Back‑translation spot checks for literary pieces focusing on metaphor/onomatopoeia retention.
  - Human-in-the-loop for high‑risk domains (legal, safety, finance) with checklists.

## Confidence and Coverage
- Confidence: Moderate. The evidence is dense and consistent across many judges and domains, with repeated patterns (time drift, term dilution, entity misspelling, stylistic flattening).
- Coverage limits: Most low‑scores cite nuanced shifts rather than catastrophic errors; fewer pure numeric/unit cases. Literary and technical/business texts dominate; limited evidence for code, tables, or highly formulaic data sheets.