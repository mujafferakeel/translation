# Failure Model — DeepSeek V3.1 Reasoner — Russian

## Summary
DeepSeek V3.1 Reasoner generally preserves meaning, but recurrent RU↔EN round‑trip failures cluster around precision drift and style flattening. The most salient weaknesses are: (1) domain‑term substitution that alters facts (technical, legal, aviation, games, sports), (2) proper‑name and toponym instability (spelling, type, or role changes), (3) tone/register erosion (legalistic/archaic/poetic voices made generic), (4) occasional critical polarity flips in rules or instructions, and (5) contamination with untranslated or foreign tokens. Poetry and specialized jargon are especially fragile.

## Top Failure Modes
- Terminology drift in technical/legal/game domains — Description: swaps precise terms for near‑synonyms or broader labels, degrading accuracy; sometimes flips a mechanism. Why: preference for fluent paraphrase over term fidelity; weak domain anchors for RU ↔ EN cognates. Frequency: common; Severity: high.
  - Evidence: “market cards face up” became “face down” (rules inverted); “foot valve”→“check valve,” “unenforceable”→generic; “environmental system”→“life support.”
- Proper‑name and label instability — Description: changes spellings/titles/types of people/places/items; RPG names and sports roles drift. Why: over‑normalizing to common forms; transliteration vs translation confusion. Frequency: common; Severity: medium.
  - Evidence: “Diane”→“Diana”, “Elm Street”→“Elm Avenue”; game names/abilities: “Murkwater Fens”→“Swampy Marshes,” “Sigil Weave” renamed.
- Tone/register flattening (legal, archaic, marketing, poetic) — Description: formal/legalistic/archaic/poetic styles are neutralized; idioms replaced with plain prose; modality softened/hardened. Why: decoder favors clarity; limited style control across RU inflection and aspect. Frequency: common; Severity: medium.
  - Evidence: archaic legal tone modernized (“Ward,” “ratepayers” simplified); “must”→“should” in specs; marketing idioms/puns lost (“Spring into savings”).
- Critical factual inversions or misreads of actions — Description: localized flips that change requirements, safety, or plot facts. Why: lexical ambiguity + RU aspect/negation; sports/aviation/game jargon under-specified in training. Frequency: occasional; Severity: high.
  - Evidence: “revive expired certificate” read as “revoke old one”; “over‑wing exits” rendered as “flaps”; “ground duels”→“aerial duels”; key character interaction reversed.
- Untranslated/mixed‑script artifacts — Description: stray Russian words or other-language fragments embedded; Cyrillic attributes in otherwise translated contexts. Why: copy-through on OOV or markup; decoder confusion with metadata. Frequency: occasional; Severity: medium.
  - Evidence: residual “пишете”; random Spanish in sidebar; fantasy item stats left in Cyrillic.
- Gender/role and tense shifts — Description: neutral “they” mapped to masculine; tense/aspect altered, changing nuance and sometimes agency. Why: Russian lacks singular gender-neutral pronoun; aspectual mapping variability. Frequency: occasional; Severity: low–medium.
  - Evidence: student “they/them”→“he/his”; past habitual vs completed action drift; “still”→“frozen.”
- Poetry form/imagery loss — Description: meter/rhyme ignored; metaphors replaced or added; subsequent analysis becomes wrong. Why: low constraint support for verse metrics; RU↔EN poetic equivalence hard. Frequency: occasional; Severity: medium–high in poetic tasks.
  - Evidence: rhyme/meter lost; added imagery like “tower’s skeleton,” altered central metaphors (“ox”→“bull”).
- Omission/addition of minor details — Description: drop of setup paragraphs or qualifiers; added parentheticals/meta lines. Why: compression bias; desire to “help” with explanations. Frequency: occasional; Severity: low–medium.
  - Evidence: intro paragraph missing in comic synopsis; added “Main idea:” line; extra intensifier “to the death.”

## Language‑Specific Factors
- Gender and pronouns: Russian typically forces gendered past forms; round‑trip tends to default to masculine, erasing English gender‑neutral usage.
- Aspect/tense mapping: imperfective/perfective choices trigger “still” vs “frozen,” “chase” vs “catch,” or conditional→future shifts.
- Register and legalese: Russian bureaucratic/legal registers differ; calquing loses specificity of English legal terms (“minute order,” “unenforceable,” “carve‑out”).
- Terminology homonymy: Russian generic terms invite back‑translation drift (“канал”→control plane/panel; “система окружающей среды” misread as life support).
- Poetic constraints: Russian translation often prioritizes semantic content over meter/rhyme; back‑translation exposes imagery substitutions.
- Transliteration vs translation: street types, surnames, and fantasy terms oscillate between translation and transliteration, altering identity and safety cues (“crest”→“summit”).

## Numbers, Units, and Formatting
- Time formats: 12‑hour swapped to 24‑hour and vice versa without instruction; minor but consistent. Frequency: occasional; Severity: low.
  - Evidence: “1 p.m.”→“13:00.”
- Terminology tied to units/specs: valve types, cable gauge/cross‑section, materials (“stonepaste”→“stoneware”) lose precision. Frequency: occasional; Severity: medium.
- Lists/metadata: occasional heading insertions, altered labels, or missing field details; RPG stat labels in Cyrillic. Frequency: occasional; Severity: low–medium.
- Species/items and colors: specific names simplified or swapped (“dunlins/red knots”→generic sandpipers; “Electric blue” cut). Frequency: occasional; Severity: low–medium.

## Meta/Disclaimer Behavior
- Translator intrusion: sporadic additions like “Main idea,” parenthetical clarifications, or rating explanations. Triggered by expository or instructional tone that invites summarization. Frequency: rare; Severity: low.
- No safety/policy disclaimers observed contaminating outputs in this set.

## Recommendations
- Prompting
  - Enforce term and name fidelity: “Preserve all proper names, titles, addresses, game terms, and product names exactly; do not normalize or localize.” Provide a glossary block if available.
  - Style locks: “Maintain original register (legal/archaic/marketing/poetic). Do not simplify or explain. No additions or omissions.”
  - Formatting guardrails: “Keep numbers, units, and time formats as in source; do not convert 12/24‑hour time.”
  - Ambiguity notes: “If a term is ambiguous in Russian, include a bracketed alt: [alt: …] rather than choosing a different term.”
  - Poetry mode: “Prioritize preserving imagery and key metaphors; reflect rhyme/meter only if feasible—otherwise note [free verse].”
- Decoding
  - Lower temperature/top‑p for translation tasks to reduce paraphrase drift; use constrained decoding with forbidden paraphrase lists for critical terms (“exit,” “valve,” “face up”).
  - Bias toward copy for capitalized tokens and camel‑case terms (NER‑aware decoding).
- Data and fine‑tuning
  - Add RU↔EN parallel corpora with strict domain glossaries: aviation safety, sports play‑by‑play, TTRPG terms, legal filings, patents, conservation biology, culinary menus, and material culture.
  - Train on gender‑neutrality preservation patterns and legal modality mapping (“must/shall/should”).
  - Include verse corpora with aligned meter/rhyme annotations to reduce poetic imagery drift.
- Post‑processing
  - Named‑entity and terminology diff: compare source vs translation for edits to capitalized tokens, addresses, model/item names; flag changes for review.
  - Polarity/opposite checker: patterns like up/down, open/close, revive/revoke, over‑wing exit/flap; verify against domain lexicons.
  - Register checker: detect shifts from must→should, conditional→future; flag in legal/spec contexts.
  - Time/units validator: ensure time formats and material/valve types preserved; flag conversions.
  - Artifact scrubber: detect non‑target language tokens and mixed scripts; enforce single‑script output.

## Confidence and Coverage
Moderate confidence. Evidence spans many domains and judges with dense signals on terminology drift, names, tone, and a handful of critical inversions. Less coverage on tables/markup and numeric conversions beyond time format; few safety/meta intrusions observed.