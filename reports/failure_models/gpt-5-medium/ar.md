# Failure Model — GPT-5 (medium reasoning) — Arabic

## Summary
Overall, gpt-5-medium produces structurally faithful Arabic translations but systematically softens specificity and tone. The most salient weaknesses are: (1) genericizing precise nouns and domain terms (species, instruments, technical jargon), (2) pronoun, gender, and subject mismatches caused by Arabic agreement pressures, (3) modality/tense weakening (“must”→“should,” aspect drift), (4) idiom and figurative language flattening (poetry, puns, metaphors), and (5) small factual substitutions (names, counts, titles) that slip past fluency. These appear most under literary, scientific/technical, and bureaucratic/legal registers.

## Top Failure Modes
- Specificity erosion in content words — Replaces precise lexemes with broader synonyms (e.g., species, tools, roles), diluting fact and tone; likely driven by coverage gaps and decoder preference for high-probability synonyms. Frequency: common; Severity: high.
  - Evidence: bird species swapped (reed bunting→sedge warbler); “Magistrate/Constable” → “judge/officer”; soil textures (sandy loam→sandy silt).

- Pronoun/gender/subject misassignment — Arabic’s obligatory gender/number marking pushes the model to pick a gender or wrong subject; neutral or inanimate “it/they” becomes “he,” and speakers flip. Frequency: occasional; Severity: high.
  - Evidence: “their”→“his”; Librarian “it”→“he”; “I smeared”→“She smeared.”

- Modality and tense drift — Strong requirements softened, and aspect mismatches appear with Arabic imperfect/perfect choices; time expressions normalized incorrectly. Frequency: occasional; Severity: medium.
  - Evidence: “MUST”→“should”; “Hour nine”→“nine o’clock”; flashback tense inconsistencies.

- Idiom, metaphor, and register flattening — Poetic/figurative choices normalized; puns/rhyme lost; specialized register (naturalist, legal, theatrical) made generic. Frequency: common; Severity: medium.
  - Evidence: “arointing geese”→plain “clamorous goose”; ox→bull with altered imagery; legal “deem/special deference”→“think/special treatment”; “blocking/upstaging”→general verbs.

- Named entity and title perturbations — Small but important substitutions in names/titles reduce factual fidelity. Frequency: occasional; Severity: medium.
  - Evidence: middle name Elaine→Ellen; “Grounded” pillar→“Realistic”; “bodega/charrette” generalized.

- Numerals/quantifiers and countable nouns drift — Misreads counts or aspectual adverbs; singular/plural switches under Arabic broken plurals/collectives. Frequency: occasional; Severity: medium.
  - Evidence: “twelve once”→“twelve times”; plural→singular in doorway line; “players’ mounts”→“boats.”

- Quote stance and voice changes — Direct quotes turned into reported speech; speaker voice formalized; sign-offs/tags altered. Frequency: rare; Severity: low.
  - Evidence: “I trusted”→“she trusted”; “P.S.”→“Note:”; added “cross-team.”

## Language‑Specific Factors
- Gender and agreement pressure: Arabic enforces gender/number on verbs, adjectives, and pronouns; English neutral “they” forces a gendered choice, and inanimate/epicene references often become masculine. This triggers subject/pro-noun flips and over-specification.
- Definiteness and number: The definite article and broken plurals/duals promote singular↔plural drift and loss of exact count nuances.
- Aspect/tense mapping: Arabic perfect/imperfect vs. English tense/aspect causes imperfective/perfective mismatches and flashback drift; time-of-day and ordinal/hour idioms get normalized.
- Register pull toward MSA formality: The model defaults to formal constructions, leading to obsequious or stilted tone in casual voice.
- Lexical gaps and loanwords: Specialized domains (CS, theater, archaeology, natural history) lack widely standardized Arabic terms; the model paraphrases (e.g., idempotency, tail latency, blocking) or picks near-neighbors.

## Numbers, Units, and Formatting
- Modality softening and requirement verbs drift: “must”→“should,” “shall be”→“is.”
- Quantifier and count errors: “once”→“times,” singular/plural toggles, and time normalization (“Hour nine”→clock time).
- Titles/labels and UI strings: Button text and headings occasionally altered (“Checkout”→“Pay”).
- Units/tables/markup: No systemic unit conversion errors or markup breakage observed; issues are lexical rather than formatting.

## Meta/Disclaimer Behavior
- Rare stylistic meta shifts: “P.S.”→“Note:,” occasional added qualifier (“cross‑team”) and heightened formality in official letters (“is honored to submit”). No safety/policy disclaimers contaminating content were observed. Triggers: email/letter formats, corporate comms.

## Recommendations
- Prompting
  - Enforce fidelity constraints: “Translate with maximal lexical fidelity; do not paraphrase. Preserve names, species, technical terms, modality (must/shall), numbers, and speaker attributions. Do not infer gender; retain neutral forms. Keep quotes verbatim.”
  - Provide glossaries/term locks via placeholders or tags (e.g., <term lock>idempotency key</term lock>).
  - Specify register: “Maintain original register (naturalist/legal/poetic). Avoid formalization.”
  - For poetry: “Preserve metaphors and key images; do not replace with generic synonyms; ignore rhyme if needed but keep imagery.”
- Decoding
  - Lower temperature/top‑p to reduce synonym drift; enable constrained decoding for named entities and locked terms.
  - Add penalties for altering modality tokens (must/shall), numerals, and quoted spans.
- Data and Fine‑tuning
  - Augment with high-quality EN↔AR parallel corpora in undercovered domains: ecology/taxonomy, archaeology soil classifications, CS/infra (idempotency, tail latency), theater jargon.
  - Curate style pairs for legal, naturalist, and technical registers to reduce formalization pull.
  - Gender-neutral training: include datasets emphasizing preservation of neutral pronouns and avoidance of gender inference.
  - Poetic/literary fine-tuning to retain metaphors and precise imagery.
- Post‑processing and QA
  - Automatic checks: diff modality verbs; verify numbers/dates/times; named-entity consistency (names, titles, product/pillar names); detect singular/plural mismatches.
  - Domain term validators: glossary hit-rate checks; flag out-of-gloss substitutions (soil textures, species, gameplay terms).
  - Quote/speaker auditor: ensure direct quotes remain direct; speaker tags unchanged.
  - Pronoun audit: flag gender insertions where source is neutral; suggest neutral Arabic rewrites (plural generic, passive, or noun repetition).

## Confidence and Coverage
Confidence: medium-high. Evidence spans many judges and genres with consistent patterns (genericization, pronoun/gender drift, modality/tense shifts, metaphor flattening). Coverage limits: few explicit cases on numeric units or markup; safety/meta intrusions are rare; conclusions emphasize observed domains (literary, legal/bureaucratic, technical) rather than UI/tables.