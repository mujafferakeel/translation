# Failure Model — DeepSeek V3.1 Reasoner — Korean

## Summary
DeepSeek V3.1 Reasoner shows strong fluency in Korean but exhibits recurrent fidelity breaks under entity, terminology, and register pressure. The most salient weaknesses are: (1) factual substitutions of proper nouns, roles/ranks, and domain terms; (2) number/date/window inversions and unsolicited unit conversions; (3) tone and person drift (formality, empathy, and speaker switches) in dialogue and service copy; (4) mixed‑language artifacts (Chinese/foreign insertions) and minor hallucinated glosses; (5) idiom/pun/metaphor flattening, especially in poetry, food/culture, and technical prose.

## Top Failure Modes
- Entity and terminology drift — Proper nouns, titles/ranks, species, and domain terms are altered or generalized; sometimes near‑synonyms replace precise terms.
  - Why: Entity copying is not strictly enforced; Sino‑Korean lexical proximity and subword overlap nudge toward high‑freq variants; the “Reasoner” tendency to paraphrase/normalize.
  - Frequency: common; Severity: high
  - Evidence: “shorebirds” became “seaweed” in a coastal piece; “open‑pollinated” mislabeled “hybrid”; ranks flipped “Detective→Sergeant,” “Lt. Cmdr→Major”; membership “student body→Student Council.”

- Numbers, time windows, and polarity flips — Business hours vs days, “no earlier than 21” vs “no later than 21,” and “couple of weeks→few days”; directive polarity reversals (“tighten→reinforce,” “match your tempo→at their own pace”).
  - Why: Korean numerals/time expressions compress semantics (영업시간/영업일; ~일 이내/이전); the model paraphrases constraints; beam speculation inserts plausible but wrong temporal nouns.
  - Frequency: occasional; Severity: high
  - Evidence: “4–6 business hours→days” in support chat; application timing window inverted; “three‑year→third‑year.”

- Tone/register and person drift — Output becomes more formal/stilted, less empathetic; speaker pronouns swap (“I↔you,” “we↔I”); honorific mismatches; gender misassignment.
  - Why: Korean is pro‑drop and gender‑neutral; the model chooses default polite forms and infers gender; dialogue tagging not preserved increases ambiguity.
  - Frequency: common; Severity: medium–high
  - Evidence: Agent chat softens/over‑formalizes and changes “hours→days”; multiple dialogue lines swap “I/you”; Mrs. Alvarez gender flipped.

- Mixed‑language artifacts and minor hallucinated glosses — Untranslated Chinese segments, added parenthetical explanations, and unit notes; occasional foreign insertions in the BACK translation.
  - Why: Training contamination and bilingual pretraining; “Reasoner” style adds explanatory parentheticals; decoding not constrained to target script.
  - Frequency: occasional; Severity: medium
  - Evidence: Chinese strings (“至关重要,” “或者其他 특성”) appear; added Fahrenheit note; foreign text in agent reply.

- Cultural/idiomatic and figurative loss — Puns, idioms, culinary terms, and poetic imagery flattened or swapped; specific dish/ingredient names normalized to Korean defaults.
  - Why: Preference for high‑probability synonyms; limited parallel idiom coverage; tendency to domesticize food/culture terms (e.g., mapping to gochujang/chunjang).
  - Frequency: common; Severity: medium
  - Evidence: “multiply→double”; jokes altered (“cilantro→horseradish”); beef shank/marrow misrendered; pun/idiom mishandling noted across poetry and essays.

- Directional/spec detail inversions in technical/legal text — Mechanical actions, legal standards, UI terms, and specific options misrendered (e.g., “plausible→compelling,” “subspace→hyperspace,” “Chain of Custody” generalized).
  - Why: Term disambiguation errors under sparse domain context; Korean nominalizations encourage abstraction; model optimizes readability over technical identity.
  - Frequency: occasional; Severity: medium–high
  - Evidence: “avoid buckling→prevent twisting”; “plausible standard→compelling”; “subspace→hyperspace”; “escapement→balance wheel.”

## Language‑Specific Factors
- Pro‑drop and ambiguous subjects/objects: Korean allows omitted subjects and topic markers (은/는, 이/가) leading to person/agent swapping in back‑translation and within dialogues.
- Gender neutrality: Korean lacks gendered pronouns; the model over‑infers and sometimes flips genders of referents on re‑rendering.
- Time/count expressions: Similar forms for hours/days and constraints (~이전/이내/까지) trigger window inversions; “couple” vs “few” often neutralized as “몇.”
- Sino‑Korean homophones/near‑synonyms: Facilitates rank/title and institution drift (Board/Committee; Council/student body), and technical term approximation.
- Script mixing and code‑switching: Frequent Korean use of English loanwords; the model oscillates between transliteration and substitution, sometimes leaving non‑Korean scripts untouched.
- Cultural domestication: Tendency to normalize foreign culinary and fauna terms to more familiar Korean equivalents (e.g., gochujang, sandpipers), altering specificity.

## Numbers, Units, and Formatting
- Time windows and business metrics: Hours↔days swaps; “no earlier than / no later than” inversions; “couple of weeks→few days.”
- Units: Unrequested conversions and annotations (e.g., Fahrenheit note); misstating entities like “Commonwealth→United States.”
- Counts and plurality: Singular/plural toggles (“kids→kid,” tools plural→singular).
- Technical measurements: Ingredient quantities and cup allocations garbled; “three sprigs→three kinds.”
- Tables/markup: Occasional foreign characters or mixed scripts in notes.
- Severity: Numbers/time window errors are high‑impact despite occasional frequency.

## Meta/Disclaimer Behavior
- Added explanations or safety‑style parentheticals (units, definitions), and rare translation notes creep into outputs, especially in technical or service contexts.
- Trigger conditions: Ambiguous units, temperatures, or domain jargon; model leans into “Reasoner” explanatory voice.
- Severity: Low–medium but undesirable for strict translation fidelity.

## Recommendations
- Prompting
  - Use a strict translation scaffold: “Translate into Korean. Preserve all proper nouns, numbers, units, ranks, and UI text verbatim. Do not add explanations, notes, or unit conversions. Maintain the original speaker labels and person. If a term is unknown, keep it in English with parentheses.”
  - For dialogues: Provide role tags and require line‑by‑line mapping with a speaker glossary; enforce politeness/register: “match tone; keep empathy markers.”
  - For legal/technical: Include a provided glossary (ranks, standards, mechanisms); instruct “no synonym substitution for glossary terms.”
  - For time/quantity: Add a checklist in the instruction: “Reconfirm all numeric ranges, time windows, and polarity (before/after, no earlier/later).”
- Decoding
  - Lower temperature/top‑p for translation tasks to reduce paraphrastic drift.
  - Constrain generation to target script except for quoted proper nouns/terms; enable lexically constrained decoding for glossary and entities.
- Data and fine‑tuning
  - Fine‑tune on ko↔en parallel corpora emphasizing entity/number preservation, domain legal/technical datasets, and dialogue corpora with explicit speaker/tenor control.
  - Augment with curated ko translations of bird species, culinary items, and scientific/engineering lexicons; include pun/idiom parallel pairs with literal vs adaptive strategies labeled.
  - Add contrastive pairs for hours↔days, window polarity, and excused/unexcused to penalize subtle semantic flips.
- Post‑processing
  - Automated QA: Named‑entity and number alignment checks; regex to compare digits, date tokens, and rank/title lists.
  - Polarity/unit validator: Detect and flag “hours/days,” “earlier/later,” and unit conversions; block outputs with added parentheses unless bracketed quotes existed.
  - Bilingual diff view for reviewer: surface deviations on glossary items and UI strings.
  - Script sanitation: Filter non‑Korean/CJK insertions unless present in source.

## Confidence and Coverage
- Confidence: Moderate–high. Dozens of low‑score cases from multiple judges show consistent patterns (entity/term drift, time/number inversions, tone/person shifts, mixed‑language artifacts, figurative loss).
- Coverage limits: Evidence leans toward English↔Korean general domain with some legal/tech, culinary, poetry, and customer service; fewer examples for scientific equations or long tables. Mixed judges reduce bias, but not exhaustive across all genres.