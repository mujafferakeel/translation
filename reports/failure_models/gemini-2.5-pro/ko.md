# Failure Model — Gemini 2.5 Pro — Korean

## Summary
Gemini 2.5 Pro’s Korean round‑trip translations are usually semantically close but show recurrent fidelity drift: it paraphrases stylistically (especially poetry and rhetoric), softens register, and alters domain terms and fine‑grained facts. The most salient weaknesses are: systematic tone flattening; precision loss in technical/legal terminology; unwanted additions such as translator notes/parentheticals; micro‑fact distortions (proper names, taxonomy, dates/centuries); and Korean‑specific ambiguity handling (pronouns, kinship, color terms) that triggers meaning shifts.

## Top Failure Modes
- Tone flattening and stylistic paraphrase — Description: Evocative, idiomatic, or period/genre tones are rendered into more generic, formal, or verbose Korean, then back into plain English, losing rhythm and force. Why: Preference for safe, neutral Korean paraphrase and register normalization during generation; decoding favors fluency over faithfulness. Frequency: common; Severity: medium-high. Evidence: judges note “poetic nuance flattened” and “archaic legal tone modernized”; “dialogue subtext softened; parallelism lost.”
- Terminology drift in domain texts — Description: Specific terms in legal/technical/culinary/game systems are replaced by near-synonyms that change scope or meaning. Why: Korean lexical gaps or multiple competing terms (loanwords vs Sino‑Korean) lead to normalization; lack of enforced glossaries. Frequency: common; Severity: high when in legal/tech. Evidence: “minute order → hearing summary order,” “Bulk fermentation → Primary fermentation,” “jurisdiction → venue swap,” “combustible → flammable.”
- Micro‑fact and entity distortions — Description: Proper names, species, ranks, architectural terms, numerals/centuries, and labels drift subtly. Why: Korean transcription/orthographic normalization and semantic smoothing; ambiguous mapping (e.g., taxa with common-name overlap). Frequency: occasional; Severity: medium. Evidence: “thirteen centuries → 13th‑century,” “red knot → dunlin,” “bay window → sash window,” “S. Raines → Reins,” “Lina → Rina.”
- Role/membership and scope misinterpretation — Description: Logical relations and membership statements flip or narrow. Why: Head-final structures and topic-prominent Korean encourage compact paraphrase; the model compresses scope markers. Frequency: occasional; Severity: high when institutional meaning changes. Evidence: “All students are members of the student council → all students are the student body”; “orders → order” and plural→singular collapses.
- Unwanted additions: translator notes, politeness particles, hedges — Description: Inserts parenthetical Korean terms, explanations, or politeness (“please,” “try to”), and meta glosses. Why: Safety/teaching priors and bilingual-explanation patterns in training; Korean politeness defaults. Frequency: occasional; Severity: medium. Evidence: “Korean terms in parentheses,” “added explanation ‘rime (frost)’,” “please” added in instructions.
- Pronoun, person, and gender shifts — Description: Neutral or quoted person shifts (“we→you,” “they→he”), and gender neutral→male defaults. Why: Korean pro-drop and lack of gendered pronouns; model resolves ambiguity incorrectly. Frequency: occasional; Severity: medium. Evidence: “they/their → he/his,” “we → you,” quote person switch in peer quote.
- Lexico‑semantic drift in imagery and idioms — Description: Colors, animals, and metaphors change (“ox→cow,” “blue→green”), idioms literalized. Why: Korean lexical polysemy (푸르다 blue/green), generic hypernyms (소), idiom avoidance. Frequency: occasional; Severity: medium. Evidence: “ox → cow,” “blue walls → green wall,” idioms rendered as literal phrases.
- Game/system and UI term inconsistency — Description: Systematic renaming of classes/skills/perks and UI phrases; plural/singular inconsistencies. Why: Lacking domain lexicon and copy constraints; paraphrase bias. Frequency: occasional; Severity: medium. Evidence: “skill/perk names changed,” “regional summaries → singular,” “armor reduction → defense reduction.”

## Language‑Specific Factors
- Pronoun drop and honorifics: Korean omits subjects and encodes politeness via endings; back‑translations pick arbitrary person/gender or add politeness (“please,” hedges), shifting agency/tone.
- Kinship specificity: Korean often requires older/younger distinctions; the model over‑inserts “older/younger sister,” causing invented details.
- Ambiguous color/lexeme mapping: 푸르다 spans blue/green; 소 covers cattle/ox/cow; these yield imagery drift.
- Sino‑Korean vs native vs loanwords: Competing registers (e.g., 헌장/헌법, 관할/관할권 vs 재판지/법정지) trigger term swaps that alter legal precision.
- Head‑final syntax and topic prominence: Scope and membership clauses can compress or flip if particles (은/는, 의, 의존명사) are paraphrased, leading to misread institutional statements.
- Idioms and onomatopoeia: Korean tends to normalize or re-create sound symbolism; poetry and quotes lose cadence and parallelism.

## Numbers, Units, and Formatting
- Century/era confusion: “thirteen centuries” misread as “13th‑century,” indicating numeral + classifier handling issues under paraphrase.
- Hazard classes and technical qualifiers: “combustible” vs “flammable” swapped; singular/plural toggles for counts (“awards,” “dashboards”) appear.
- Name/label normalization: Titles, ranks, and product names altered or misspelled; some English left untranslated in placeholders or added parentheticals.
- Conversions/inventions: No unit conversion errors noted, but explanatory additions in parentheses occur; tables/markup not flagged.

## Meta/Disclaimer Behavior
- Translator notes and inline glosses: Adds parenthetical Korean/English terms or brief definitions, likely triggered by perceived specialized terms or poetic language.
- Hedging and prescriptive softening: Inserts “try to,” “must have,” politeness forms, especially in instructions or recommendations.
- Safety/policy intrusions: Not observed as policy warnings; intrusions are explanatory rather than safety‑driven.

## Recommendations
- Prompting
  - Use strict constraints: “Translate with maximal fidelity. Do not add explanations, notes, or parentheticals. Preserve register, punctuation, quotes, and [sic]. Keep proper nouns, numbers, ranks, and species unchanged unless obviously wrong.”
  - Provide glossary and register directives: “Use legal term: minute order; keep ‘Bulk fermentation’; keep cricket ‘runs/wickets’; retain gender-neutral pronouns.”
  - Add persona: “You are a court/tech/poetry translator; preserve archaic tone” and include 2–3 parallel style anchors.
  - Add ambiguity instructions: “If pronoun/kinship is ambiguous, keep it ambiguous; avoid inferring older/younger relations.”
- Decoding
  - Lower temperature/top‑p for determinism in term copying; enable constrained decoding with a glossary for domain terms and named entities.
  - Use copy‑bias for numerals, proper names, and bracketed content; penalize paraphrase in high‑precision spans (regex‑anchored).
- Data and fine‑tuning
  - Fine‑tune on ko↔en legal/technical parallel corpora with curated glossaries (venue vs jurisdiction, combustible vs flammable, cricket, baking, game systems).
  - Add contrastive pairs for Korean ambiguity pitfalls (푸르다, 소, pro‑drop pronouns, kinship) with evaluation probes.
  - Include poetry/idiom corpora emphasizing rhythm/parallelism preservation; penalize literalization in training objectives.
- Post‑processing
  - Automated checks: NER/name consistency, numeral/century classifier alignment, species/taxonomy from controlled vocab, rank/title dictionaries.
  - Style/regression checks: Classifier for added politeness markers, parentheticals, hedges; reject if present.
  - Term-lint: Diff against provided glossary; flag scope flips (plural→singular), membership statements, and hazard class keywords.

## Confidence and Coverage
- Confidence: Moderate. Dozens of low‑score rationales from multiple judges converge on consistent patterns (tone flattening, term drift, micro‑fact errors, additions).
- Coverage limits: Evidence spans varied domains; fewer explicit cases on units/tables and policy disclaimers. Findings are strongest for legal/technical, poetry/literary, and institutional prose in ko↔en round trips.