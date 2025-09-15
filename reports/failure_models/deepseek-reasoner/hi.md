# Failure Model — DeepSeek V3.1 Reasoner — Hindi

## Summary
DeepSeek V3.1 Reasoner generally preserves core meaning in Hindi but shows recurring drift in precision, register, and terminology. The most salient weaknesses are: (1) lexical substitution of domain terms and proper nouns, (2) systematic tone flattening and idiom literalization (especially in poetry/marketing), (3) normalization or invention of culturally marked items, (4) scope/number/date-time inaccuracies, and (5) formatting/policy artifacts (RFC keywords, meta notes). These errors are often small in isolation but compound to alter factual, technical, or stylistic fidelity.

## Top Failure Modes
- Domain term drift and named-entity corruption — Technical/legal/brand and proper nouns are replaced with near-synonyms or misspelled, changing constraints or facts; causes include over-normalization and weak NE copying under Devanagari. Frequency: common; Severity: high.
  - Evidence: RFC keywords and role terms softened/inverted (“MUST/SHOULD” lightened; “Enforcer”→“Compliant”); brand/name errors (“DaedalUX”→“Deadlux”, “Anker”→“Anchor”), class/UX terms shifted (“Adept”→“Specialist”, “persistent”→“consistent”).

- Poetic/figurative tone flattening and idiom literalization — Vivid images and concise metaphors get paraphrased into generic Hindi or literal renderings, breaking rhyme/imagery. Likely trigger: decoding preference for standard Hindi and avoidance of rare metaphorical collocations. Frequency: common; Severity: medium–high.
  - Evidence: “unfurled”→“open”; “nutshell”→“chest”; rhyme broken; “crisp”→“rough”; marketing punch softened (“spearheading”→“led”).

- Cultural invention and over-Sanskritization — Introduces culturally specific Hindi items absent in source or inflates register with Sanskritized choices; likely due to priors from Hindi literary style and hallucination to fit “poetic” tone. Frequency: occasional; Severity: medium.
  - Evidence: “reed flute”→“narsingha”; added Hindi phrase (“jo chahe de”) and greeting (“Namaste!”); “nettles”→“scorpion herb”.

- Temporal/quantitative and scope errors — Noon/afternoon flips, date changes, plural↔singular scope shifts, mean/median swaps, unit/temperature tweaks; root cause: Hindi temporal lexical ambiguity and weak numeric copying. Frequency: occasional; Severity: medium–high in specs/schedules.
  - Evidence: “Friday noon”→“Friday afternoon”; 14→28 June; 72→70°F; “infected systems” (pl)→“system” (sg); median→mean.

- Safety/standards term misclassification — Hazard/severity categories replaced with near but wrong terms; root cause: lexical closeness and lack of domain calibration. Frequency: occasional; Severity: high (safety/legal).
  - Evidence: “Combustible liquid”→“Flammable liquid”; “containment”→“control”; “relocation”→“transfer”.

- Gender/register and politeness mismatches — Neutral English becomes gendered or overly formal Hindi; cause: Hindi gendered lexicon and politeness defaults. Frequency: occasional; Severity: medium.
  - Evidence: “batters”→“batsman”; colloquial “I’m in” formalized; “Folks”→“Sir”.

- Spurious additions/parentheticals and translator meta — Inserts parentheses, notes, or transliterations, contaminating output; triggered by instruction misread. Frequency: occasional; Severity: low–medium.
  - Evidence: Parenthetical headings, transliteration added, translator notes present.

- Lexical substitutions in concrete items — Tools, fauna, foods, and objects swapped with near neighbors; often from dictionary proximity or low-frequency senses. Frequency: occasional; Severity: medium.
  - Evidence: “vanes”→“veins”; “cava”→“kava”; “trowel/rake/hose”→“spade/broom/pipe”; “snails”→“worms”.

## Language‑Specific Factors
- Script and copying: English proper nouns/brands within Devanagari often get phonetic normalization or respelling (e.g., Anchor for Anker); transliteration vs. retention decisions are inconsistent.
- Register inflation: Tendency toward Sanskritized or formal Hindi in place of concise colloquial phrasing, shifting tone (marketing, dialogues, poems).
- Gender and neutrality: Hindi’s grammatical gender nudges neutral roles to gendered forms (“batsman,” masculine defaults); pronoun choices can shift referents.
- Modality and legal emphasis: English MUST/SHOULD lacks direct capitalized equivalents in Hindi; without explicit mapping, norms get softened.
- Temporal lexemes: “dopahar” (afternoon/noon) ambiguity drives noon↔afternoon flips; date formats lack strong anchoring without numeric copying constraints.
- Idiom and metaphor: English idioms are rendered literally or with generic synonyms, losing compact imagery; rhyme/meter rarely preserved unless prompted.

## Numbers, Units, and Formatting
- Numerals/dates: sporadic digit errors and swaps (72→70; 14→28) and “s7” typos; singular/plural scope shifts in instructions.
- Standards keywords: RFC/BSD-like MUST/SHOULD/MAY softened to “should/can” or inverted; defined-term capitalization lost (“Terms”) reducing legal force.
- Tables/headings: Added parenthetical headings or redundant parentheses; capitalization/style for defined entities dropped.
- Units/metrics: Mean/median confusion; minor color/state adjectives (“ice”→“snow,” “gray”→“brown”) drift.

## Meta/Disclaimer Behavior
- Occasional translator notes or added phrases (Hindi glosses, greetings like “Namaste!”) appear when prompts are open-ended or include examples; also added transliterations in parentheses. These intrusions tend to occur in formal letters, announcements, or mixed-language inputs.

## Recommendations
- Prompting
  - Use explicit constraints: “Translate verbatim; do not add notes, greetings, or transliterations. Preserve names, numbers, dates, and RFC keywords exactly.”
  - Provide a glossary block with mappings: MUST→“अनिवार्य (MUST)”, SHOULD→“उचित/अनुशंसित (SHOULD)”, Enforcer→“प्रवर्तक (Enforcer)”, and instruct to keep English in parentheses for first mention.
  - Add name/number locks: “All proper nouns and numerals must be copied unchanged; if unsure, retain in Latin script.”
  - Style guardrails: “Maintain original register; avoid Sanskritized substitutions unless present in source; keep colloquial tone where present; preserve metaphors.”
  - Temporal precision: “Render ‘noon’ as ‘दोपहर 12 बजे’, not ‘दोपहर’.”

- Decoding/Control
  - Lower temperature and enable constrained decoding with lexically constrained tokens for: RFC modals, branded terms, dates, numbers.
  - Turn on repetition penalty only after confirming no parenthetical/meta additions; prohibit bracketed content unless source has it.

- Data/Fine-tuning
  - Augment with parallel EN–HI corpora in: RFC/specs, legal contracts, safety sheets (with explicit MUST/SHOULD mappings), technical manuals; include contrastive pairs (combustible vs flammable).
  - Curate poetry/marketing parallel sets emphasizing idiom preservation and metaphor fidelity; include rhyme-preservation examples.
  - NE-copy curriculum: mixed-script training where brands and product names are retained in Latin within Devanagari text.
  - Temporal/quantitative augmentation: schedules, logs, and date-time datasets with evaluation on noon/afternoon, mean/median.

- Post-processing QA
  - Named-entity/number checker: diff source vs translation for names, dates, temperatures, measurements; flag edits.
  - Modality linter for RFC/legal: verify capitalized keywords and defined terms; enforce glossary.
  - Scope/number consistency: plural/singular alignment for critical nouns (“systems” vs “system”).
  - Domain term glossary check: sustainability, microfiction, containment, relocation, etc.
  - Optional stylistic validator: detect added greetings/parentheses; block output if found.

## Confidence and Coverage
Moderate–high confidence. Evidence spans ~100+ low-to-mid scores across diverse judges and genres (poetry, specs, legal, UX, marketing, narrative). Strong convergence on term drift, tone flattening, and numeric/time errors; fewer but clear cases of cultural invention and meta insertions. Coverage may underrepresent code, tables, and highly technical math/units; severity in safety/legal domains inferred from fewer but impactful examples.