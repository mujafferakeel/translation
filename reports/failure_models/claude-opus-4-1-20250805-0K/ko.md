# Failure Model — Claude Opus 4.1 (no reasoning) — Korean

## Summary
Claude Opus 4.1 typically preserves core meaning in ko↔en round‑trip, but it systematically flattens register, drifts on proper nouns/institutional terms, and weakens technical/legal precision. Korean‑specific ambiguities (zero pronouns, weak plural marking, flexible word order, honorific/register choices) frequently trigger person/number flips and tone shifts. Occasional but high‑impact errors occur with times (AM/PM), hazard/technical labels, and instruction sequencing.

## Top Failure Modes
- Proper noun and label drift (entities, titles, varieties) — Swaps or morphs names/titles and specific labels into near-synonyms or misspellings; undermines identity/precision. Why: transliteration variability, preference for common Sino‑Korean terms, and over-normalization. Frequency: common; Severity: medium–high.
  - Evidence: “Student Council→Student Association,” and multiple fantasy/science names (“Rin Vale/Lin Veil; Aether/Ether; Kazi/Kaji”); cultivar “Ember Queen→Amber Queen.”

- Register flattening and tone formalization — Literary/poetic or persuasive voice becomes generic/informal, losing imagery or rhetorical force. Why: Korean tends toward concise neutral diction in MT; model favors safe synonyms and sentence simplification. Frequency: common; Severity: medium.
  - Evidence: Evocative terms simplified (“chartreuse→green,” “shatteringly crisp→crispy”); ad copy slogans/idioms muted (“Spring into savings→Enjoy spring savings”).

- Person and agency shifts (they/you/we; active/passive) — Dialogue attributions and perspective change, altering dynamics. Why: Korean pro‑drop and ambiguous subject marking; model resolves ellipsis incorrectly on back‑translation. Frequency: occasional; Severity: medium.
  - Evidence: “They said→You said” in dialogue; “we→I” and passive voice substitutions in narratives/announcements.

- Number (singular/plural) and scope drift — Plural parties or items become singular (or vice versa), changing legal or procedural scope. Why: Korean lacks obligatory plural marking; back‑translation over‑specifies. Frequency: common; Severity: medium.
  - Evidence: “Sellers/Buyer(s)→seller/buyer,” “bunches→bunch,” multiple opportunities/memberships multiplied/split.

- Technical/terminology precision loss — Near‑synonym swaps alter domain meaning (safety, conservation, archaeology, culinary). Why: lexeme ambiguity and preference for higher‑frequency synonyms in Korean; limited domain glossary anchoring. Frequency: occasional; Severity: high when safety/compliance critical.
  - Evidence: “combustible liquid→flammable liquid,” “flaking→blooming,” “rolled rim→everted rim,” “tamper‑evident→tamper‑proof,” “rear‑facing seat→rear seats.”

- Temporal and unit misinterpretation — AM/PM flips, access windows, or quantities shift; occasional instruction reordering. Why: Korean time expressions (오전/오후/12시) are error‑prone in translation; free paraphrase of procedural steps. Frequency: occasional; Severity: high for schedules/instructions.
  - Evidence: “10:00 AM–12:00 PM→10:00–12:00 AM”; “daylight hours vehicle access→weekly access”; cooking step order changed.

- Semantic narrowing/generalization — Specific categories broaden or narrow improperly. Why: tendency to normalize to common categories; Korean hypernyms preferred. Frequency: occasional; Severity: medium.
  - Evidence: “service animals→service dogs,” “quarters→coins,” “municipal maps→poetry maps.”

- Idiom and figurative language literalization — Idioms paraphrased into plain assertions; imagery altered. Why: idiom mismatch and over-literal back‑translation. Frequency: occasional; Severity: low–medium.
  - Evidence: “cut corners→do not compromise quality,” “snow listened→snow lifted,” “murmuration→dance.”

- Minor additions/omissions and label changes — Inserts common UX/legal boilerplate or trims qualifiers, subtly shifting tone. Why: model “helpfulness” priors and Korean concision norms. Frequency: occasional; Severity: low–medium.
  - Evidence: Added “check spam folder,” appended courtesy “Thank you”; dropped “confines of,” “hint of.”

## Language‑Specific Factors
- Zero pronouns and topic drop: Korean often omits subjects/objects; back‑translation guesses roles, causing “They→You/We/I” and active↔passive shifts in dialogue/narrative.
- Weak plural marking: Nouns without 들 lead to singular/plural flips in legal/contractual or multi‑party contexts (“Sellers”→“seller”).
- Modality/register mapping: English must/shall vs. Korean 하여야 한다/해야 한다/권장한다 map inconsistently, softening requirements (“must→should”).
- Proper noun handling: Variable transliteration and Sino‑Korean calques push “Council↔Association,” name spellings (Rin/Lin), and species/product names.
- Time expressions: 오전/오후 with 12시 and “정오/자정” ambiguity triggers AM/PM errors; “주간” can be misread as “weekly” vs. “daytime.”
- Technical lexicon overlap: Near‑synonymous Sino‑Korean terms blur distinctions (연소성/가연성; 박락/백화; 전용/방지), altering domain meaning.
- Idioms/onomatopoeia: English sound symbolism and set phrases lack direct Korean equivalents; often paraphrased, diluting voice.

## Numbers, Units, and Formatting
- Time/date: AM/PM flips; “daylight hours→weekly” misread; tense/timeline formatting turned into telegraphic logs.
- Counts and plurals: Item/person counts generalized; singular/plural toggles.
- Units/quantities: Occasional conversion mistakes or attribute swaps (amount→temperature).
- Headings/labels/tables: Section titles normalized (“Minor Comments→Detailed Comments”); institutional headings renamed; menu tags preserved but nuanced adjectives simplified.

Conversions/inventions: Rare explicit unit invention; more common are scope/label changes and step order reordering.

## Meta/Disclaimer Behavior
- Occasional helpful boilerplate additions (e.g., “check spam folder,” “emergency”) and courtesy closings. No strong safety/policy intrusions observed. Triggers: customer‑service or UX contexts where Korean conventions favor helpful additions. Frequency: rare; Severity: low.

## Recommendations
- Prompting
  - Enforce fidelity constraints: “Translate conservatively. Do not add or omit. Preserve proper nouns and official titles verbatim. Maintain modality (‘must/shall’) strength and time expressions (AM/PM).”
  - Provide glossaries/style guides per domain: legal (shall/must→…하여야 한다), conservation (박락=flaking), safety (combustible vs flammable distinctions), culinary terms.
  - Ask for ambiguity preservation: “If number/person is unclear, preserve ambiguity; do not infer singular/plural or you/we.”
  - Instruct transliteration policy: “Keep Latin script for names/brands; do not rename councils/committees.”
- Decoding
  - Lower temperature/top‑p to reduce synonym drift and stylistic paraphrase.
  - Use constrained decoding with phrase preservation lists (proper nouns, technical terms, hazard classes, pillar names).
- Data and fine‑tuning
  - Augment with bidirectional ko↔en parallel corpora emphasizing:
    - Legal/technical modality mapping, AM/PM disambiguation, hazard and archaeology terminology.
    - Dialogues with pronoun‑drop to maintain speaker attributions.
    - Style‑rich literary and ad copy to retain imagery and idioms.
  - Build domain glossaries and enforce via lexically constrained training or adapters.
- Post‑processing QA
  - Automated checks:
    - Named‑entity consistency (exact string match pre/post).
    - Modality and negation diff (“must/shall/may,” “prohibited/not for use”).
    - Time validator (AM/PM, ranges, “daylight” vs “weekly”).
    - Count/party consistency (singular/plural roles in contracts).
    - Hazard/tech term whitelist (combustible vs flammable; tamper‑evident vs tamper‑proof).
  - Dual translation/back‑translation round with diffs highlighting tone/term shifts; human in the loop for high‑risk docs.
  - Instruction/order checkers for recipes/procedures to catch step reordering.

## Confidence and Coverage
Confidence: medium‑high. Evidence spans many samples and judges, with consistent patterns in tone flattening, proper noun drift, modality weakening, time misreads, and singular/plural flips. Coverage limits: few purely numeric/unit conversion cases; minimal safety/policy intrusion evidence; domain skew toward literary, policy, UX, and technical prose, less on scientific formulas or tabular data.