# Failure Model — Gemini 2.5 Pro — Turkish

## Summary
Gemini 2.5 Pro’s Turkish translations are generally faithful on content, but low scores cluster around precision and style fidelity: recurring gender/person shifts driven by Turkish’s gender‑neutral, pro‑drop grammar; systematic formalization/flattening of tone; substitution of domain terms with looser near‑synonyms; and fragile handling of normative/legal modality and tabular/metric formatting. Poetry and highly stylized prose lose meter/rhyme and personification. Acronyms and administrative labels drift on the round trip. The most salient weaknesses: gender/pronoun fidelity, tone preservation, technical/legal modality, poetry form retention, and table/label/number integrity.

## Top Failure Modes
- Gender and person shifts — Description: Neutral Turkish pronouns and frequent subject drop lead to back‑translations that guess or flip gender/person, altering facts or voice. Why: Turkish “o/onu/ona” is ungendered; morphology often hides subjects; possessives allow multiple readings. Frequency: common; Severity: high. Evidence: “Kazi becomes she,” “Suri rendered as he”; “I→she” and “she→it,” changing plot/personification.

- Tone formalization and stylistic flattening — Description: Crisp, idiomatic, or noir/poetic English returns as more formal or generic Turkish, which back‑translates to diluted tone and weakened imagery. Why: Default fluency bias toward formal register; preference for standard Turkish collocations; loss of idiom-to-idiom mapping. Frequency: common; Severity: medium. Evidence: “noir punch lost to formal phrasing”; “evocative terms replaced with generic (weary vs exasperated).”

- Technical/legal term drift and modality errors — Description: Key terms and RFC/standards modals shift (“MUST→should”), and specialized lexicon is replaced (“runway→capital,” “tail latencies→queueing delays,” “Charter→Bylaw”). Why: Over-reliance on close synonyms; lack of domain-locked termbase; Turkish legal/tech equivalents are multiword and easy to soften; capitalization/defined terms not preserved. Frequency: common; Severity: high. Evidence: “MUST becomes should”; “Terms→Conditions,” “Bulk fermentation→First fermentation.”

- Acronyms and named entities mis-handled in round trip — Description: Correct Turkish domain acronyms return as wrong English (e.g., KOAH back as KOAH or altered) and org/program names drift. Why: Model treats localized acronyms as canonical; back‑mapping to original English acronym not enforced. Frequency: occasional; Severity: medium. Evidence: “ADHF→ADKY, COPD→KOAH”; “Housing Trust→Housing Foundation.”

- Poetry and form loss (meter/rhyme/personification) — Description: Rhyme/meter omitted; stanza count misreported; pronoun choices undo personification (“she→it”). Why: Prioritizes propositional content over form; Turkish prosody differs; literal renderings drop constraints. Frequency: occasional; Severity: high (for literary). Evidence: “central rhyme altered; quatrains miscounted”; “she→it removes personification.”

- Concrete factual flips in micro-details — Description: Small but impactful reversals or substitutions (nod↔shake head; larch→black pine; “wrist flicks”→“breaks her wrist”; master bedroom→parents’ bedroom). Why: Ambiguity in Turkish lexical sets; heuristic disambiguation; overconfident paraphrase. Frequency: occasional; Severity: medium. Evidence: “nod vs shake head reversed”; “larch replaced by black pine.”

- Tables, labels, and numerals formatting drift — Description: Fiscal labels mangled (FY24E→MY24T), decimal separator inconsistency, AM/PM turned to 24‑hour. Why: Turkish conventions (24‑hour clock, comma decimals); model normalizes without round‑trip guardrails. Frequency: occasional; Severity: medium. Evidence: “table headers mangled”; “24‑hour clock used instead of AM/PM.”

- Translator notes/insertions — Description: Unneeded parentheticals or glosses and rare soft policyish rephrasings. Why: Helpfulness bias; uncertainty about terms. Frequency: rare; Severity: low. Evidence: “added notes like (kale), (devre ilanı).”

## Language‑Specific Factors
- Pronouns and pro-drop: Turkish has a single third‑person pronoun and routinely omits subjects; gender must be inferred from context. This triggers gender swaps and person shifts on back‑translation.
- Possessive/case ambiguity: Agglutinative morphology can hide agent/patient roles and ownership, contributing to “wife’s money vs his money” type flips.
- Register defaults: Written Turkish defaults to formal register; idioms and slang often get normalized, softening tone.
- Compound nouns and specialization: Many English technical terms map to Turkish multiword expressions; the model often picks a near‑synonym that back‑translates loosely.
- Prosody mismatch: Preserving English rhyme/meter in Turkish is nontrivial; without explicit instruction, form is sacrificed for literal sense.
- Localized acronyms: Turkish uses localized domain acronyms (e.g., KOAH for COPD). Without constraints, back‑translations keep local forms or mis‑expand them.

## Numbers, Units, and Formatting
- Fiscal/label corruption: FY/quarter labels altered; table headers relettered. Occasional numeral/decimal separator shifts.
- Time formats: AM/PM often normalized to 24‑hour in Turkish, then not restored.
- Units mostly retained; isolated “unit conversion” mentions, but not systemic.
- Severity: medium when specs/reports rely on strict labels.

## Meta/Disclaimer Behavior
- Rare additions of parenthetical translator notes or mild explanatory insertions that break clean translation. Triggered by uncertainty over domain terms or food/place names.
- No strong safety/policy disclaimers observed contaminating outputs.

## Recommendations
- Prompting
  - Specify role and constraints: “Translate to Turkish preserving gender, person, defined terms, acronyms, capitalization, and RFC modals (MUST/SHOULD). Do not add notes.”
  - Provide a mini‑glossary/termbase per task: runway, moonshot, tail latencies, bulk fermentation, Terms vs Conditions, Charter, master bedroom, etc.
  - For literary texts: choose mode explicitly (“preserve meter/rhyme and personification” or “semantic literal, no form”); instruct to keep pronouns/personification intact.
  - For tables/specs: “Preserve original labels, time formats, and numeric separators verbatim.”
- Decoding
  - Lower temperature/top‑p and enable “strict translation” style for legal/technical.
  - Use constrained decoding or placeholders for defined terms/acronyms and restore after.
- Data/Training
  - Augment with high‑quality EN–TR parallel corpora in legal/spec/clinical and style‑rich literary pairs emphasizing tone and modality fidelity.
  - Include term-locked fine‑tuning using glossaries (RFC2119, finance labels FY/Q, clinical acronyms COPD/ADHF).
  - Add contrastive pairs penalizing gender/person flips from pro‑drop contexts.
- Post‑processing
  - Rule‑based checks:
    - Modals: detect and flag MUST/SHOULD/MAY inversions.
    - Acronyms/labels: whitelist mapping (COPD↔KOAH, then back to COPD when target is English; FY/Q labels).
    - Time/number normalization: restore AM/PM if source used it; enforce decimal/thousands format of source.
  - Pronoun/gender QA: highlight named entities and pronouns; require human confirmation or run coreference consistency checks.
  - Terminology QA: automatic diff against provided termbase; flag substitutions (“Charter,” “Bulk fermentation,” “tail latency,” “runway”).
  - Literary QA: stanza/line counts, rhyme markers, personification pronouns preserved.

## Confidence and Coverage
- Confidence: medium‑high. Patterns recur across many judges and domains with consistent symptoms (gender/person shifts, tone flattening, modality/label drift).
- Coverage limits: Evidence skews toward mixed domains (literary, legal/spec, clinical, marketing) and round‑trip evaluations; acronym “errors” sometimes reflect correct Turkish localization penalized by back‑translation. No long-form scientific/math-heavy samples observed.