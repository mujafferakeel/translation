# Failure Model — Gemini 2.5 Pro — Chinese

## Summary
Gemini 2.5 Pro’s zh translation shows high semantic coverage but recurrent fidelity loss from paraphrase bias, register softening, and unstable handling of proper nouns and technical terms. The most salient weaknesses are: (1) proper-noun and domain-term drift, (2) tone/register flattening with added politeness and verbosity, (3) idiom/metaphor normalization that changes meaning, (4) tense/number instability due to Chinese morphosyntax, and (5) numeric/scale and category shifts in technical/legal contexts.

## Top Failure Modes
- Proper noun and terminology drift — Names, titles, organizations, fantasy/scifi/game mechanics, and legal/technical labels are replaced or normalized; transliterations vary. Why: tendency to choose “more natural” Chinese renderings, ambiguity in transliteration, weak term locking. Frequency: common; Severity: high. Evidence: “Rin Vale→Lin Weir; Aether→Ether; Kazi→Kaz”; “Council→Association; Charter→Constitution; Item ID→Evidence Number”; “publisher/title mismatched.”

- Register softening and verbosity — Imperatives become polite requests; concise/poetic or official style is flattened; extra hedges and synonyms inflate text. Why: RLHF politeness bias, paraphrase preference, Chinese formal style default. Frequency: common; Severity: medium-high. Evidence: “adds ‘please’ throughout instructions/advisories”; “poetic tone flattened; terse style became wordier.”

- Idiom/metaphor misalignment — English idioms calqued into common Chinese idioms and back lose original meaning; metaphors made literal or hedged. Why: idiom-for-idiom substitution without reversible mapping. Frequency: common; Severity: medium. Evidence: “‘the die is cast’→‘wood is already a boat’”; “‘a ghost’→‘as if ghosts’.”

- Tense/aspect and number instability — Present becomes past; singular↔plural shifts; role/collective nouns drift. Why: Chinese lacks tense/number inflection, model overcommits on back-translation. Frequency: common; Severity: medium. Evidence: “consistent shift to past tense”; “colonies→colony; repeat offenders→second offender; bails singular↔plural.”

- Technical precision loss (domain semantics) — Hazard classes, materials, UX terms, astronomical terms drift to near-synonyms with different denotation. Why: insufficient domain grounding; synonym preference over term fidelity. Frequency: occasional; Severity: high. Evidence: “combustible→flammable”; “stonepaste→stoneware”; “affordances→feature visibility”; “sol→lunar day.”

- Sports/nautical/horticulture jargon erosion — Specific field terms replaced with generic wording, altering facts or imagery. Why: sparse exposure to niche glossaries; normalization. Frequency: occasional; Severity: medium. Evidence: “six‑yard box misread as six players”; “slush fund/becalmed lost”; “vanes→blades; determinate → limited growth.”

- Numeric/scale and unit drift — Magnitudes, specialized units, and counts change; headings/IDs re-termed. Why: paraphrase and unit generalization; number copying not enforced. Frequency: occasional; Severity: medium-high. Evidence: “thousand→ten million”; “bank (formation unit)→earthen mound”; “valve plural→singular.”

- Meta/disclaimer intrusions — Translator notes and parentheticals appear in-body. Why: safety/instruction-following habits leaking into translation. Frequency: occasional; Severity: medium. Evidence: “parenthetical translator notes throughout”; “translator note inserted.”

## Language‑Specific Factors
- Lack of inflection (tense/aspect/number) encourages the model to re-decide tense/number on back-translation, causing systematic shifts.
- Proper-noun handling in Chinese: transliteration vs translation vs normalization (e.g., institutions) leads to variable forms and back-translation mismatches.
- Idiom mapping: replacing English idioms with established Chinese idioms preserves readability but breaks reversibility.
- Register norms: Chinese defaults to courteous/formal phrasing; the model often adds 请 and softeners that alter imperative/official tone.
- Classifiers and role nouns: shifts between collective vs singular readings (guilds, watch/watchman, committee/board) arise from zero-marked plurality.
- Poetic compression vs explanatory prose: the model expands condensed imagery into descriptive paraphrase, losing cadence and metaphor density.

## Numbers, Units, and Formatting
- Numerals/scale: occasional magnitude inflation or deflation; singular/plural counts diverge.
- Units/terms: domain-specific units and identifiers generalized or renamed (e.g., sol→“lunar day,” bank→“earthen mound,” Item ID→Evidence Number).
- Headings/IDs: section titles and committee names re-termed, weakening legal/official precision.
- Conversions/inventions: rare explicit conversions, but re-termed labels suggest invented normalization rather than strict copying.

## Meta/Disclaimer Behavior
- Translator notes/parentheticals inserted in informational copy and essays; appears when content is figurative or culture-laden or when terms feel obscure.
- Effect: contaminates fidelity and style; breaks clean translation requirement even when meaning is intact.

## Recommendations
- Prompting
  - Enforce preservation: “Translate with minimal paraphrase. Preserve all proper nouns, numbers, titles, headings, class/skill names, and IDs exactly. Do not add notes, bracketed glosses, or politeness like ‘请.’ Keep tense and imperative mood.”
  - Idioms: “Prefer literal renderings of idioms and metaphors; avoid substituting with different idioms.”
  - Domain hints: provide a short inline glossary for key terms (legal, UX, sports, fantasy) and specify “use glossary verbatim; no synonyms.”
- Decoding
  - Lower temperature/top‑p for reduced paraphrase; increase repetition penalty only modestly to avoid synonym churn.
  - Apply constrained decoding with lexically constrained tokens for names, numbers, units, and glossary terms.
- Data and fine‑tuning
  - Train on high-quality EN↔ZH parallel corpora with entity and term locking; include back‑translation targets that penalize semantic class drift (hazard categories, materials, legal terms).
  - Augment with domain-specific glossaries (legal, UX, sports, gaming, nautical, horticulture, astronomy).
  - Add transliteration consistency data: name preservation pairs and rules (retain Latin spellings or provide stable Pinyin with capitalization).
  - Include idiom preservation tasks where literal calques are preferred for reversibility.
- Post‑processing
  - Named-entity and numeral checker: enforce exact match with source (Levenshtein/regex for Latin names, numbers, IDs, dates).
  - Terminology QA: validate mapped terms against a project glossary; flag near-synonyms and category changes (e.g., flammable vs combustible).
  - Register checker: detect and strip polite particles (“请/请您”), hedges, and injected notes.
  - Tense/number guardrails: for back-translation workflows, annotate source tense/number and verify alignment automatically.
  - Diff-based reviewer: highlight changed proper nouns, titles, and magnitudes for human approval.

## Confidence and Coverage
Confidence: moderate-high. Evidence spans many judges and domains with consistent patterns (proper nouns/terms, register, idioms, tense/number). Coverage limits: the dataset is skewed to low-scoring round-trip cases and may overrepresent literary/jargon-heavy texts; limited evidence on tables/markup and on long-form numerical data transformations.