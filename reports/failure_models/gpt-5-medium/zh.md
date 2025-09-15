# Failure Model — GPT-5 (medium reasoning) — Chinese

## Summary
Overall, gpt-5-medium produces high‑fidelity zh↔en translations, but low‑scoring cases cluster around consistency and formality drift: mixed‑language output (untranslated Chinese in English back‑translations), systematic tone flattening and metaphor loss, domain‑term drift (legal/technical/jargon), pronoun/number mishandling (gender neutrality and plural/singular), and occasional sensory/detail swaps that alter facts. These errors are most visible in structured docs (headers/bullets/specs), literary/poetic passages, and domain‑heavy texts (legal, RFC/standards, culinary, sports).

## Top Failure Modes
- Mixed-language leakage in structured sections — Leaves Chinese text unconverted in headings/bullets; sometimes inserts Chinese where English was expected. Why: segmenter misses non‑sentence spans (headers/TOCs/labels), and the model treats them as metadata to preserve. Frequency: common. Severity: high. Evidence: “header descriptions replaced with Chinese text,” and “untranslated Chinese in a key objective.”
- Tone/imagery flattening and metaphor drift — Poetic or vivid diction becomes literal; key metaphors swapped (ox→cow; hush→boo), and prosody weakened. Why: preference for high‑probability synonyms in zh and loss of register mapping back to en. Triggers: poetry, lyrical prose, marketing taglines. Frequency: common. Severity: medium–high. Evidence: “core metaphor changed ‘ox’→‘cow’,” and “crowd ‘hush’→‘boo’ alters atmosphere.”
- Domain term substitution and precision loss — Replaces exact terms with close but incorrect ones (legal titles, hazard classes, technical nouns), altering facts or scope. Why: vocabulary normalization and low confidence on rare terms; training bias to generic synonyms. Triggers: legal/standards (RFC MUST/SHOULD), patents, conservation reports, sports jargon, culinary items. Frequency: common. Severity: medium–high. Evidence: “Magistrate→Justices of the Peace; RFC MUST/SHOULD lowercased,” “combustible→flammable,” “gesso→gypsum,” “lemon posset→lemon curd,” “redaction‑aware NLP→perceptual masking.”
- Gender neutrality and pronoun artifacts — Neutral “they” becomes gendered “he/she,” or introduces “TA” artifact; shifts person/voice. Why: Chinese gender marking ambiguity and attempt to force clarity; over‑literal back‑mapping. Triggers: narratives, HR/policy, dialog. Frequency: occasional. Severity: medium. Evidence: “gender‑neutral ‘they’→‘he/she’,” and “pronoun artifact ‘TA’ introduced.”
- Number and countability drift (plural/singular, units, casing) — Singular/plural flips (Sellers→seller), title/role scope narrowing/expansion, and casing-sensitive tokens altered (MUST/SHOULD). Rare insertion of “天” in English. Why: Chinese number neutrality; casing not modeled as semantic constraint; heuristic detokenization. Triggers: contracts, specs, timelines. Frequency: occasional. Severity: medium. Evidence: “Sellers→seller,” “30 天 left in English segment,” “tens of thousands→thousands.”
- Sensory/detail swaps that change facts — Small lexical swaps change scene specifics (ticking→dripping; incense→spices; hush→boo), or sports terms generalized. Why: semantic neighborhood confusion plus style smoothing; domain idiom gaps. Triggers: scene descriptions, sports/cricket, birding. Frequency: occasional. Severity: medium. Evidence: “ticking sound→dripping,” “wickets/partnership→dismissals/coordination.”
- Tense/voice and modality shifts — Present↔past flips; passive→active (“we” introduced); obligation markers softened; titles formalized. Why: zh tense underspecification, preference for explicit subjects, modality mismatch. Triggers: reports, instructions, corporate tone. Frequency: occasional. Severity: low–medium. Evidence: “tense shifts in ending,” “passive to active ‘we’,” “MUST/SHOULD lowercased.”

## Language‑Specific Factors
- Number and plurality neutrality — Chinese often omits plural marking, leading to singular/plural mistakes on back‑translation (e.g., Sellers→seller).
- Gender marking — Chinese third‑person pronouns map ambiguously to English; attempts to disambiguate cause gendering or “TA” artifacts; loss of English neutral “they.”
- Modality and legal register — English RFC/legal modality (MUST/SHOULD) and archaic/legal titles (Magistrate) have no direct Chinese casing/register equivalent; capitalization can be lost.
- Idioms, poetic compression, and wordclass fluidity — Chinese favors concise literal renderings; metaphoric density and sound symbolism (hush vs boo) are often flattened; slogans/puns (“Spring into savings”) are recast as literal messages.
- Domain lexicon with near neighbors — Culinary (posset vs curd; hummus vs purée), conservation/art terms, sports jargon (cricket, climbing) are prone to “close but wrong” equivalents due to sparse exposure in zh corpora.

## Numbers, Units, and Formatting
- Mixed script residues — Occasional Chinese units/characters remain in English output (e.g., “30 天”).
- Casing and token preservation — Spec terms and modal verbs lose uppercase (MUST/SHOULD), altering normative force.
- Role/quantity normalization — Plural→singular and scope changes in role names; bullet headers translated or rewritten in Chinese during back‑translation.
- Units themselves are usually preserved; conversions/inventions are rare, but noun scope (e.g., “acquisitions”→“acquisition campaign”) can drift.

## Meta/Disclaimer Behavior
- Rare stylistic “intensification” and label shifts framed as tasting notes or evaluative paraphrase; no safety/policy disclaimers observed. Trigger: subjective domains (flavor notes). Evidence: “vocabulary intensified; ‘preserved citrus’→‘candied citrus’.”

## Recommendations
- Prompting
  - Use a strict style system prompt: “Translate, do not paraphrase. Preserve terminology, capitalization (MUST/SHOULD), numerals, and formatting. Translate ALL text, including headers, bullets, and inline labels. Do not leave any Chinese characters in English output. Preserve gender neutrality (‘they’) and plurality. Keep metaphors and idioms.”
  - Provide a glossary/term bank per document (e.g., Student Council, Magistrate, gesso, posset, jackline, wickets/partnership, cash runway) and instruct: “Treat glossary as fixed; do not substitute synonyms.”
  - For literary/marketing: “Maintain imagery and metaphors; avoid explaining or downgrading figurative language. Prefer vivid over generic synonyms.”
  - For pronouns: “If gender unknown, use gender‑neutral English ‘they’; in Chinese, prefer neutral noun phrases (该人/对方/当事人) rather than TA.”
- Decoding
  - Lower temperature/top‑p for high‑precision domains (legal, RFC, patents) to reduce synonym drift.
  - Enable constrained decoding for casing‑critical tokens (MUST/SHOULD/SHALL) and glossary entries.
- Data/Training
  - Augment parallel corpora with: RFC/spec texts (modal capitalization), contracts/deeds with plural parties, niche culinary items (posset), conservation/art restoration terminology, cricket/sports jargon, and marketing copy with idioms/puns and their faithful zh mappings.
  - Add contrastive pairs penalizing ox/cow, hush/boo, combustible/flammable, gesso/gypsum, posset/curd, wickets/dismissals substitutions.
  - Include gender‑neutral narrative datasets mapping to Chinese neutral forms and back to English “they.”
- Post‑processing QA
  - Automated checks: detect residual Han characters in English output; verify glossary adherence; casing validator for RFC modals; singular/plural consistency checks against entity lists (Sellers/Buyers).
  - Domain lint rules: hazard‑class lexicon (combustible vs flammable), legal title whitelist, sports term maps.
  - Diff‑based style guard: flag high metaphor density loss or sensory word class swaps (sound→liquid).
- Workflow
  - For structured docs, segment and translate headers/labels separately with “translate all metadata” flag.
  - Human-in-the-loop spot checks on first pass for terminology sections and headers before full batch.

## Confidence and Coverage
Confidence: medium. Evidence is dense across many judges and domains, with multiple corroborations for mixed‑language leakage, tone flattening, and term drift. Less coverage on units/date conversions (rare), and meta/disclaimer behavior (rare). Findings are strongest for structured documents, literary passages, and domain‑specific terminology.