# Failure Model — Qwen 3 Max Preview — Chinese

## Summary
Qwen 3 Max Preview generally preserves meaning but shows systematic fidelity drift when translating to/from Chinese in areas that require rigid terminology, names, and quantification. The most salient weaknesses: (1) characters vs. words confusion in counts/constraints; (2) over‑normalized or altered proper nouns, titles, and technical terms; (3) tense/modality weakening (past/present shifts; MUST/SHOULD strength); (4) poetry/quotations paraphrased instead of preserved, losing register/idioms; (5) number/species/detail inflation/deflation and singular/plural toggles. These issues appear even when overall tone and structure remain close.

## Top Failure Modes
- Words vs. characters conflation — Treats “words” as “characters,” corrupting constraints and facts; why: Chinese uses characters as units; model defaults to 字 over 词 in count contexts and back‑transfers this; frequency: common; severity: high; evidence: judges flag repeated “words→characters” swaps and that this alters a core constraint in an article about word counts.

- Proper‑noun and title normalization — Systematically changes names, places, games/skill names, and institutional titles; why: tendency to translate or standardize to common Chinese forms, then back‑translate to different English (e.g., 学生会→Student Union; 地名音译/意译 drift); frequency: common; severity: high; evidence: “Student Council→Student Union,” “Kangerlussuaq→Conglomerate Suak,” “Aster‑3→Stellar Rock‑3,” character names (Jake→Jack; Kaelen→Karen), game terms and ranks altered.

- Tense, aspect, and register drift — Present↔past shifts and formality elevation; why: Chinese lacks tense morphology; defaults to neutral aspect and formal written register, then back‑maps inconsistently; frequency: common; severity: medium; evidence: multiple notes of present→past narrative; “more formal/verbose tone,” instructional voice changed from imperative to descriptive.

- Normative and legal term weakening — RFC‑style MUST/SHOULD and legal venues/dockets become softened or narrowed; why: Chinese modals (应/必须/宜) and legal terminology mappings (“hearing” vs “trial,” “venue” vs “jurisdiction”) are ambiguous; frequency: occasional; severity: high; evidence: “SHOULD→must/should inconsistency,” “hearing/minute order→trial/trial minute,” “venue→jurisdiction,” omission of “single” arbitrator.

- Quotations, poetry, and set phrases paraphrased — Embedded poems/quotes and archaic lines are rephrased, losing rhyme/register or period flavor; why: preference for fluency and explicitation over micro‑form fidelity; frequency: occasional; severity: medium; evidence: poem lines reworded; historical quotes modernized (“detach households→quarantine households”); puns like “bright idea” lost.

- Technical/detail substitutions and hypernyms — Specific culinary, biological, and hardware terms replaced by broader or adjacent terms; why: lexical gap handling and frequency bias to common synonyms; frequency: occasional; severity: medium; evidence: “pâté→meat ragù,” “menthol→mint,” “tetryon→quadron,” “pogo‑pin→spring pin,” bird species and counts altered.

- Quantities, plurality, and scope shifts — Thousand↔ten‑thousand, singular↔plural, and scope terms (season vs quarter) drift; why: 万/千 scaling habits, lack of plural marking, and multiple senses of 季度/季度/季; frequency: occasional; severity: medium; evidence: “thousands→tens of thousands,” “Sellers (plural)→Seller (singular),” “same season→same quarter.”

## Language‑Specific Factors
- Character vs. word unit: Chinese often counts by characters; “word limit” contexts invite 字 misinterpretation and back‑translation to “characters.”
- Tense/aspect neutrality: No inflection leads to underspecified aspect; re‑Englishing selects a convenient tense (often past), shifting narrative immediacy.
- Number and plurality: No obligatory plural marking encourages singular/plural drift; classifiers not carried back precisely.
- Proper‑noun handling: Competing transliteration (音译) vs. translation (意译) norms cause names/titles to be normalized or localized, then mismatched on return.
- Register normalization: Written Chinese tends toward concise formal style; colloquial/archaic/poetic source registers become formalized or literalized.
- Legal/technical modality: Mapping 应/应当/必须/可 to RFC and common‑law terms is delicate; the model inconsistently calibrates strength.
- Idioms and wordplay: Puns and meter are often sacrificed for propositional accuracy, especially when idioms lack neat Chinese equivalents.

## Numbers, Units, and Formatting
- Numerals/units: “Words↔characters” conversion; thousand↔ten‑thousand scaling; season↔quarter confusion; occasional strengthening/loosening of counts (“thousands→tens of thousands”).
- Lists/headings/labels: Title‑case and UX copy rephrased (“Progress you can trust”→descriptive heading), impacting product voice.
- Technical book/edition metadata: Binding/endpaper annotations mislocated or renamed; annotator counts changed.
- Species/terms: Bird species swapped; dish names normalized (“lemon posset→lemon curd”); game skill names generalized.
- Conversions/inventions: Occasional invented specifics (e.g., “humanoid” added) and plural↔singular tech items (RTEGs), but no pervasive unit conversion beyond counts.

## Meta/Disclaimer Behavior
- Policy/authorial intrusions are rare. The main “meta” drift is normative language re‑statement (e.g., MUST/SHOULD strength changes) rather than safety disclaimers. These trigger in standards, legal, and compliance texts where modal calibration is required.

## Recommendations
- Prompt patterns
  - Enforce units and modality: “Preserve ‘word’ vs. ‘character’ exactly as in source. Do not convert counts. Preserve RFC 2119 keywords verbatim (MUST/SHOULD/MAY).”
  - Names/terms lock: “Do not translate or alter proper nouns, ranks, skill names, dish names, or product terms; copy them exactly unless an official Chinese exonym exists (provide both: 原文(中文) on first mention).”
  - Register fidelity: “Preserve tense/aspect and narrative voice. If tense is ambiguous in Chinese, mirror the source tense in English.”
  - Quotes/poetry mode: “Render quotations and poems verbatim with minimal paraphrase; preserve rhyme/meter markers if present; flag untranslatables.”
  - Diff checklist instruction: “Before finalizing, compare: names, numbers, units, modality, legal terms (‘hearing,’ ‘venue’), and pluralization.”

- Decoding changes
  - Lower temperature/top‑p for legal/technical passages to reduce synonym drift.
  - Enable constrained decoding/dictionaries for protected tokens (RFC keywords, names, SKUs, species).
  - Use span‑copy bias for quoted segments and inline code/standards keywords.

- Data augmentations
  - Curate zh↔en parallel corpora with explicit “word vs character” contexts and editorial style guides.
  - Add legal/RFC alignment sets mapping 应/应当/必须/宜/可↔MUST/SHOULD/MAY/WARN, with contrastive negatives for modality errors.
  - Include proper‑noun stability sets (games, fantasy names, toponyms, scientific species, culinary items) with transliteration vs. translation decisions labeled.
  - Poetry/archaic register pairs emphasizing non‑paraphrase fidelity.

- Fine‑tuning targets
  - Term stability head: penalize synonym substitutions for whitelisted domains (legal, standards, part names).
  - Numeracy and count‑unit head: classify and preserve count units; explicitly detect and block 词↔字 conversion unless instructed.
  - Modality calibrator: supervised signals for MUST/SHOULD strength preservation.

- Post‑processing checks
  - Automated diff on: proper nouns, numbers, units, dates, modality keywords, singular/plural.
  - Heuristics: flag if “word(s)” co‑occurs with 字/字符; flag 万/千 scaling; flag “trial” vs “hearing” and “venue” vs “jurisdiction.”
  - Terminology QA: domain glossaries (legal, culinary, gaming) with soft constraints and reviewer prompts.
  - Quote integrity: compare quoted spans for paraphrase distance; require human approval if distance > threshold.

## Confidence and Coverage
Confidence: medium‑high. Evidence spans many judges and genres (legal/technical, narrative, UI copy, poetry), with consistent patterns in names/terms, modality, and counts. Coverage limits: most samples score mid‑to‑high, so failure modes are often subtle rather than catastrophic; safety/meta behaviors were infrequent; domain breadth good but not exhaustive (limited scientific units and markup/table cases).