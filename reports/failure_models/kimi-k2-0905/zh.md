# Failure Model — Kimi K2-0905 — Chinese

## Summary
Kimi K2-0905 tends to produce smooth, natural Chinese but at the cost of exact fidelity when translating to/from English. The most salient weaknesses are: (1) systematic tense/person and voice shifts; (2) drift on proper nouns and institutional/technical terms; (3) occasional polarity/direction reversals that flip meaning; (4) number/unit inventions or conversions; and (5) steady paraphrasing that erodes specificity (legal/technical/poetic diction). These issues are amplified by Chinese-specific ambiguities (tense, number, role marking) and a domestication bias (normalizing names, terms).

## Top Failure Modes
- Tense/person and narrative voice shifts — Rewrites past into present (and vice versa) and flips 3rd to 1st person, altering stance and immediacy; likely triggered by Chinese’s weak tense marking and the model’s preference for flowing narrative.
  - Why: Chinese lacks inflectional tense; the model normalizes aspect; decoding favors fluency.
  - Frequency: common; Severity: high
  - Evidence: “entire narrative shifted to present” (past→present); “from ‘he’ to ‘I’” with factual errors.

- Proper noun drift and domestication — Changes names of people, places, quests/abilities, titles; sometimes transliterates or substitutes with common Chinese terms, then back-translation returns a different English name.
  - Why: Sparse domain coverage; preference for familiar Chinese forms; inadequate copy-through constraints for NER.
  - Frequency: common; Severity: high
  - Evidence: Multiple changed dungeon/location/quest names; “Rin Vale→Lin Weir,” “Lina→Lena,” route place names altered.

- Terminology substitution and specificity loss — Replaces precise domain terms with near-synonyms or generic labels, especially in culinary, legal, technical, and institutional contexts.
  - Why: Vocabulary normalization; alignment/data gaps for low-frequency terms; paraphrase bias.
  - Frequency: common; Severity: medium–high
  - Evidence: “lemon posset→panna cotta,” “tamari‑glaze→soy sauce,” “Magistrate→Constable,” “minute order→hearing summary,” “EPDM→EDM,” “food webs→food chain,” “Student Council→Student Union.”

- Directionality/antonym flips and role inversions — Reverses core directional or causal relations (onshore↔offshore flow; pressing trigger roles; occupancy↔vacancy), corrupting logic while leaving surrounding details intact.
  - Why: Lexical antonym proximity; Chinese lexical ambiguity; insufficient cross-sentence constraint tracking.
  - Frequency: occasional; Severity: high
  - Evidence: “onshore flow repeatedly inverted to offshore”; “pressing trigger reversed (CB pass/keeper)”; “Retail occupancy→vacancy.”

- Numbers, units, and probability normalization — Adds or converts units unasked; misreads frequency/ratios; shifts quantifiers; changes modality intensity (will→may).
  - Why: Hallucinated helpfulness; attempt to “complete” specs; ambiguity in Chinese percentage/of constructions.
  - Frequency: occasional; Severity: medium
  - Evidence: Added “Celsius”; “Uncommon (1–1000)→‘1 in every 1,000’”; “30% more→30% of”; “will receive→may receive.”

- Register and tone drift (formal/informal, normative markers) — Softens or hardens style, switches dialect/register, or loses normative emphasis markers.
  - Why: Fluency optimization; lack of guidance on preserving register cues like capitalization.
  - Frequency: occasional; Severity: medium
  - Evidence: RFC “MUST/SHOULD” not capitalized; friendly “when you can”→“ASAP”; British register shift; “asks→demands.”

- Small omissions/additions that compound — Drops small but meaningful tokens (e.g., [sic], “neon,” “at home”) or inserts embellishments and specifics not in source.
  - Why: Concision bias; poetic smoothing; completion bias.
  - Frequency: occasional; Severity: low–medium
  - Evidence: “[sic]” omitted; added “vegetable gardens”; “duct tape” lost; minor adjective insertions.

## Language‑Specific Factors
- Tense/aspect underspecification — Chinese lacks inflectional tense; aspect markers are optional, leading to English back-translations that default to present or inconsistently mix tenses.
- Number and definiteness — No plural morphology; classifiers optional; back-translation oscillates between singular/plural (Seller(s), boat(s), topman/men).
- Role/voice ambiguity — Topic-prominent structure and flexible word order encourage passive/active flips; subject omission can shift agency (e.g., who doubles rations).
- Modality polysemy — 会/能/要 mapped to may/can/will inconsistently; leads to will→may softening/hardening.
- Proper name handling — Chinese often normalizes or translates titles/organizations; without a glossary, model “localizes” (Council→Union; Riverside/Riverfront) and transliterates inconsistently.

## Numbers, Units, and Formatting
- Unit invention/conversion: adds units (Celsius) or converts frequencies without instruction.
- Ratio/percentage misread: “30% more” vs “30% of”; rarity scales normalized to per‑N phrasing.
- Capitalization-sensitive semantics: normative RFC keywords (MUST/SHOULD) lose capitalization; weakens obligation tone.
- Technical codes/specs drift: EPDM→EDM; material names and acronyms simplified.
- Time/format tweaks: occasional time format shifts and “start early” strengthened to “must start at dawn.”

## Meta/Disclaimer Behavior
- Rare authorial intrusions or policy-ish rewordings that change certainty or framing, often in governance/tech policy contexts.
  - Examples: “may not capture→cannot capture,” “takedown→removed,” slight certainty inflation or euphemism.

## Recommendations
- Prompting
  - State hard constraints: “Do not add units, convert numbers, or change names/titles. Copy all proper nouns exactly. Preserve tense and person.” 
  - Provide a glossary/Do‑Not‑Translate list for names, organizations, quests/abilities, legal/technical terms; include examples.
  - Require capitalization preservation for normative terms (e.g., “keep MUST/SHOULD capitalization”).
  - Ask for a two-pass output: translation + a short self-check list (tense/person, names, units, polarity terms like onshore/offshore).
- Decoding
  - Use lower temperature/top‑p for technical/legal texts to reduce paraphrase drift.
  - Enable constrained decoding for named entities and glossary terms (copy-mode or span constraints).
- Data and fine‑tuning
  - Augment with parallel zh‑en corpora emphasizing: meteorology (onshore/offshore), sports tactics roles, culinary niche terms (posset, tamari), legal terms (minute order), materials (EPDM), RFC normative language.
  - Include counterexamples exhibiting tense/person preservation and polarity minimal pairs; add penalties for antonym/direction flips.
  - Train on name-copy tasks and transliteration vs. translation disambiguation with gold constraints.
- Post‑processing and QA
  - NER alignment: diff names/titles/terminology against source; flag any change.
  - Polarity/direction checker: regex for pairs (onshore/offshore, occupancy/vacancy, deflect/redirect, pass/press roles) and warn on mismatches.
  - Numbers/units validator: detect added units, converted ratios, %/of/than patterns; block unrequested conversions.
  - Modality/tense consistency check: compare will/shall/must vs may/should; forbid unprompted weakening/strengthening.
  - Capitalization enforcement for normative keywords and acronyms.
  - Domain-specific linting: culinary dish dictionary; material/acronym whitelist; legal/institutional titles.

## Confidence and Coverage
- Confidence: medium-high. Patterns recur across diverse judges and documents, with multiple independent confirmations (e.g., tense shifts, proper-noun drift, onshore/offshore).
- Coverage limits: Evidence is skewed toward narrative, legal/technical brief passages, and some domain niches (culinary, gaming, weather). Very formal tabular/markup scenarios are less represented; safety/meta intrusions are rare.