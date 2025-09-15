# Failure Model — Claude Opus 4.1 (no reasoning) — Japanese

## Summary
Opus 4.1 generally preserves meaning but consistently drifts on speaker/pronoun reference, proper nouns/titles, and domain‑specific terminology. In Japanese, ambiguity from ellipsis, politeness levels, and weak number marking amplifies shifts in person, plurality, and tone. The model also flattens stylistic nuance (poetry, literary cadence) and occasionally alters tense/modality or taxonomy (legal, clinical, technical), leading to subtle yet consequential fidelity loss.

## Top Failure Modes
- Speaker and pronoun drift in dialogue/quotes — Description: First/second/third person flips and speaker misattribution, especially in quoted lines; subtle “we/I/you/they” swaps change relationships and agency. Why: Japanese subject ellipsis, optional pronouns, and free reference resolution; model normalizes for fluency. Frequency: common; Severity: high. Evidence: “They said→You said,” “You’ve fixed worse→I’ve fixed worse” (multiple judges); “we” in a quote rendered as “I.”
- Proper noun and role/title corruption — Description: Character surnames, place names, and job titles altered or normalized (e.g., Ruiz→Lewis; Lieutenant Commander→Major; Port Alden→Port Olden). Why: Katakana/romanization variants and Overton‑window normalization; weak copying under translation pressure. Frequency: common; Severity: high. Evidence: recurring Ruiz→Lewis; rank/system name shifts (Kalendra→Calendra).
- Technical/terminological precision loss — Description: Domain terms softened or substituted (“wall‑clock time”→“real time,” “open‑pollinated”→“heirloom,” “burglary”→“robbery,” “runs”→“points”). Why: Preference for frequent synonyms over precise terms; Japanese term ambiguity and back‑mapping. Frequency: common; Severity: high. Evidence: computing term downgraded; horticulture/legal taxonomy swaps.
- Tone/register normalization and politeness insertion — Description: Archaic, formal, or terse styles become generic/polite; adds “Please,” softens direct imperatives, shifts “we”→impersonal. Why: JA stylistic conventions bias toward 丁寧語; the model optimizes for readability. Frequency: common; Severity: medium. Evidence: instruction‑like tone made polite; report voice loses first‑person “we.”
- Singular/plural and quantifier drift — Description: Singular↔plural flips, count scope changes, and list multiplicity shifts. Why: Japanese weak plural marking and classifier use; implicit numerosity lost on back‑mapping. Frequency: common; Severity: medium. Evidence: participant→participants; cold spot→cold spots; headlamp singular→plural.
- Stylistic/imagery flattening (poetry/literary prose) — Description: Sound symbolism, cadence, and vivid metaphors simplified; onomatopoeia strength reduced; cadence broken by sentence splitting. Why: Model favors semantically safe synonyms and shorter clauses; JA onomatopoeia/idioms are hard to preserve. Frequency: occasional; Severity: medium. Evidence: “rolled eyes”→“widened eyes”; “hum”→“groans”; rhyme/scheme broken.
- Tense/modality and aspect shifts — Description: Past/conditional→present; “should”→“must”; possibility vs commitment shifts. Why: JA aspect/tense mapping ambiguity; modality particles under‑specified. Frequency: occasional; Severity: medium. Evidence: “was funded”→“is fully funded”; “should”→“must.”
- Domain label/frequency misclassification — Description: Side‑effect frequency categories and authorities/fund names changed; sports and clinical classification errors. Why: Label hierarchies memorized inconsistently; Japanese category words map to overlapping English bins. Frequency: occasional; Severity: medium. Evidence: uncommon→rare→very rare chain; “utility fund”→“Public Works Fund”; “wasps”→“bees.”

## Language‑Specific Factors
- Pronoun and subject ellipsis: Japanese often omits subjects, encouraging the model to “resolve” to I/you/we incorrectly on back‑translation, causing speaker swaps.
- Politeness/register: Choice among だ/です/敬語 pushes the model toward politeness or impersonal corporate tone, diluting original voice.
- Number and plurality: Lack of obligatory plural marking and classifier dependency yields frequent singular/plural drift.
- Tense/aspect and modality: Flexible tense usage and particle nuance (〜べき, 〜ねばならない) trigger “should/must” and past/present shifts.
- Loanwords and transliteration: Katakana proper nouns and technical terms (ranks, nautical parts) get normalized or misread on the way back.
- Idioms, onomatopoeia, poetic devices: Giseigo/gitaigo and fixed idioms are paraphrased into literal phrases, weakening imagery and cadence.
- Role titles and legal terms: Japanese near‑synonyms (e.g., 県/州/局/課) prompt incorrect English re‑expansion (master/navigator; article/law; burglary/robbery).

## Numbers, Units, and Formatting
- Quantitative detail loss: Specific windows (“within one hour”) dropped or softened.
- Category/scale drift: Side‑effect frequency labels and severity scales remapped.
- List/titles/capitalization: Proper capitalization and precise list labels altered; committee/office names normalized.
- Tables/metadata: Singular/plural of datasets/modules/dashboards shifted; minor ordering changes.
- Conversions/inventions: Occasional renaming of funds/agencies and job titles; no systematic unit conversion errors observed.

## Meta/Disclaimer Behavior
- Minimal safety disclaimers, but occasional authorial intrusions: added “please,” “emergency,” or second‑person framing that wasn’t in source.
- Triggered by instructions, public notices, or when the source is terse/impersonal, prompting the model to add courtesy or specificity.

## Recommendations
- Prompting
  - Explicit constraints: “Preserve pronouns, person, tense, modality, numerals, names, titles, and quotes verbatim; do not add politeness or rephrase.”
  - Speaker map: Provide dialogue with speaker tags and require a quote alignment table.
  - Protected spans: Wrap names, titles, measurements, and domain terms in tags to copy through unchanged.
  - Style lock: “Use plain, neutral register (常体), no added 敬語; preserve sentence boundaries and cadence.”
- Decoding
  - Lower temperature and reduce paraphrase bias; enable constrained decoding on tagged spans.
  - Use beam search with coverage penalty to discourage dropping timeframes and counts.
- Data/finetuning
  - Augment JA–EN corpora with quote‑heavy dialogues, legal/clinical/nautical glossaries, sports scoring, and computing terminology (wall‑clock time).
  - Include minimal‑pair training for we/I/you and should/must distinctions; pluralization stress tests.
  - Add transliteration robustness data (proper names, ranks, ship parts) and poetic/onomatopoeic parallel corpora.
- Post‑processing
  - Automated QA checks: diffs for named entities, numerals, dates, ranks/titles, modality (“should/must”), and person (“I/you/we/they”).
  - Terminology/glossary enforcement for domain texts; dictionary lookup for legal/medical/sports terms.
  - Heuristics: flag changes in frequency labels, list counts, and time expressions; regex for speaker/pronoun flips in quotes.
  - Back‑translation spot checks focused on quotes, names, and metrics.

## Confidence and Coverage
- Confidence: Medium‑high. Patterns are consistent across many judges and samples; multiple independent mentions of the same failure types (names, pronouns, tone, terms).
- Coverage limits: Evidence skews toward general prose, technical summaries, and some literary passages; fewer hard STEM unit conversions and table‑heavy docs, so number/unit issues may be underrepresented.