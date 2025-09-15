# Failure Model — Grok 4 — Chinese

## Summary
Grok 4’s zh↔en round‑trip translations are generally faithful but degrade under pressure on tense/register, technical specificity, and consistency. The most salient weaknesses: systematic tense drift (especially past↔present), flattening of tone and idiom, inconsistent handling of domain terms/proper nouns, occasional mixed‑language output (Chinese fragments left in English passages), and formatting/capitalization loss in formal registers (legal/RFC/headers). Errors concentrate where Chinese under-specifies tense/case and where domain lexicons (ornithology, nautical, legal/military, sci‑fi) require exact mappings.

## Top Failure Modes
- Tense and voice drift — Description: Past↔present shifts and conditional→declarative flips that alter narrative stance or temporal framing. Why: Chinese aspect/tense underspecification plus decoder preference for present‑tense fluency; weak constraint to preserve verb morphology on back‑translation. Frequency: common; Severity: high. Evidence: “eulogy becomes tribute to a living person due to tense”; “systematic past→present in initial condition report.”

- Tone flattening and idiom dilution — Description: Poetic/evocative diction softened to literal synonyms; idioms paraphrased generically; onomatopoeia misread (“hush”→“boo”). Why: Preference for semantic adequacy over stylistic fidelity; limited mapping from English idiophones/idioms to compact Chinese forms and back. Frequency: common; Severity: medium–high. Evidence: “final sports idiom rendered as ‘go all out’”; “poetic imagery ‘dusted pink’→‘stained full of pink’.”

- Technical term and taxonomy errors — Description: Domain terms replaced or distorted (species, nautical gear, sci‑fi particles, legal/military ranks). Why: Incomplete bilingual lexicons; Chinese term conflation and genericization; hallucinated nearest‑neighbor mapping. Frequency: common; Severity: high. Evidence: “curlew vs. snipe; binoculars→telescope”; “tetryon→quadruple ion; Commander→Colonel”; “becalmed→anchored.”

- Proper nouns and named entities drift — Description: Street, place, event, and personal names altered, pluralized, or adorned (e.g., adding “Tree”). Why: Over‑eager semantic translation vs. transliteration; ambiguity in capitalization and compounding; training bias to naturalize names. Frequency: occasional; Severity: medium–high. Evidence: “‘Maple Avenue’→‘Maple Tree Avenue’”; “Jake→Jack; ‘Archives Night’ variants.”

- Mixed‑language and untranslated fragments — Description: Chinese headers or sentences left inside English back outputs; isolated non‑target words (e.g., Russian). Why: Header/metadata blocks bypassed or mis‑segmented; copy‑through on low‑confidence spans; insufficient hard constraint to translate all text. Frequency: occasional; Severity: high. Evidence: “entire ‘Clarity’ paragraph left in Chinese”; “English headers replaced with Chinese headers.”

- Register and role misclassification — Description: Formal→informal or imperative rewrites; politeness particles added (“please”), soft vs. strict modality shift (request→require). Why: Chinese politeness norms and modal ambiguity; model normalizes toward courteous instructions. Frequency: occasional; Severity: medium. Evidence: “‘asked to’→‘required to’”; “added ‘please’; conditional ‘would’→‘will’.”

- Directionality and spatial/temporal detail errors — Description: Onshore winds inverted; “evening”→“afternoon”; “secondary school”→“middle school.” Why: Lexical near‑neighbors with overlapping translations; lack of context anchoring for locale‑specific categories. Frequency: occasional; Severity: medium. Evidence: “‘onshore’ interpreted as ‘land-to-sea’”; “time of day and school level shifts.”

- Formatting and capitalization loss in formal text — Description: Capitalized defined terms (Terms, Dispute) and RFC modal verbs (MUST/SHOULD) decapitalized; date/number formats normalized; category labels changed. Why: Chinese script lacks case; normalization during generation; detokenization preferences. Frequency: occasional; Severity: medium. Evidence: “loses capitalization for defined legal terms and RFC keywords”; “date formats awkward/altered.”

## Language‑Specific Factors
- Tense/aspect underspecification: Chinese lacks inflectional tense, leading to present‑leaning rewrites on back‑translation and conditional→declarative shifts.
- Capitalization absence: Case distinctions (proper nouns, legal defined terms, RFC keywords) are not encoded in zh, and Grok 4 often fails to reinstate them in en.
- Polysemy and taxonomic granularity: Chinese often uses broader categories (birds, tools, ranks), encouraging generic substitutions or near‑misses when mapping back.
- Idiom and onomatopoeia mapping: English idioms and crowd sounds have no one‑to‑one zh equivalents; model paraphrases, losing register or intent (“hush” vs “boo”).
- Politely marked directives: Chinese style favors polite particles/softeners; the model sometimes inserts “please” or upgrades requests to requirements in English.
- Transliteration vs translation: Model inconsistently chooses semantic translation over name preservation, leading to added descriptors (“Tree”) or normalized event names.

## Numbers, Units, and Formatting
- Date and list normalization: Dates reformatted awkwardly; ranges or rarity labels reinterpreted (“Uncommon (1–1000)”→“1 in 1000”). Occasional; medium.
- Capitalization-sensitive tokens: MUST/SHOULD, defined legal terms, product features and headers lose capitalization or switch to Chinese headers. Occasional; medium–high in legal/technical contexts.
- Arrays/counts and plurality: Singular/plural flips (“fingerprint”→“fingerprints”), item counts tweaked; mid‑block vs. in two blocks. Occasional; medium.
- Units/terminology drift: Culinary/technical items normalized (“pesto”→“basil sauce,” “stonepaste”→“stoneware”). Occasional; low–medium.

## Meta/Disclaimer Behavior
- Translator notes/parentheticals: Occasional insertion of notes or artifacts as if explanatory aside. Rare; low–medium. Evidence: “added parenthetical artifacts as translator notes.”
- Politeness or stance additions: “Please,” softened/heightened modality without prompt. Occasional; low–medium.
- Safety/policy disclaimers: Not observed contaminating content in this set. Rare; low.

## Recommendations
- Prompting
  - Enforce invariants explicitly: “Preserve tense, modality, capitalization (MUST/SHOULD, defined terms), headers, and named entities exactly. Do not add politeness or translator notes.”
  - Provide register cues: “Maintain 19th‑century nautical diction” or “retain legal capitalization and section headings.”
  - Include a mini‑glossary per document for domain terms/species/ranks; instruct “do not substitute or generalize terms.”
  - Add coverage check: “Report any span you cannot translate; do not leave source language unaltered.”

- Decoding
  - Lower temperature with constrained decoding for formal/technical text; enable case‑preserving constraints for RFC/legal tokens and headers.
  - Use constrained named‑entity preservation: copy‑mode for proper nouns/IDs; prevent semantic adornments (“Tree,” pluralization).
  - Enable sentence‑aligned round‑trip checks: penalize outputs that mix zh in en or vice versa.

- Data
  - Augment with high‑fidelity zh↔en parallel corpora in:
    - Ornithology, nautical, legal/military ranks, sci‑fi terminology, sports commentary/onomatopoeia.
  - Include style‑preservation pairs (poetic, archaic, legalese) emphasizing tense/register and capitalization re‑projection from zh→en.
  - Curate lexicons: species lists, military/naval glossaries, RFC keyword/casing rules, place‑name gazetteers.

- Fine‑tuning targets
  - Loss terms for invariant preservation (case, numerals, named entities, headers).
  - Contrastive training on tense/register consistency: penalize past↔present or conditional↔declarative flips.
  - Domain adapters for legal/technical writing to resist synonym substitution.

- Post‑processing
  - QA rules:
    - Detect and restore capitalization for RFC/defined terms; verify headers’ language.
    - Regex checks for untranslated non‑ASCII/Chinese characters in English outputs.
    - Named entity diffs against source; block unintended additions/deletions/pluralization.
    - Terminology QA using domain glossaries (species, ranks, nautical parts).
    - Tense/modality lint: identify shifts in finite verbs and auxiliaries.
  - Human‑in‑the‑loop for high‑risk domains (legal, scientific, safety‑critical).

## Confidence and Coverage
Moderate confidence. Evidence spans many documents and multiple judges with consistent flags for tense/register drift, domain term errors, capitalization/formatting loss, mixed‑language fragments, and idiom flattening. Less density on numbers/units and meta/disclaimers, which appear but are not pervasive. Topic mix is broad (poetry, legal, technical, nautical, wildlife, sports), supporting generalization across genres; however, conclusions are limited by absence of raw source/target pairs and reliance on back‑translation rationales.