# Failure Model — Kimi K2-0905 — Arabic

## Summary
Overall, Kimi K2-0905 preserves gist and structure but shows frequent lexical drift and precision loss when translating to/from Arabic, especially with domain terms, proper nouns, and figurative language. The most salient weaknesses are: (1) antonym/role reversals and polarity flips (e.g., absorb↔reflect, nod↔shake); (2) technical term swaps and genericization (photodiodes→LEDs; glaze→glass); (3) proper name and place errors (misspellings, substitutions); (4) tone/register normalization with occasional culturally‑colored additions (religious phrasing in obituaries); (5) number/formatting inaccuracies (IDs, delimiters, MUST/SHOULD). Many errors reflect Arabic‑specific pressures: gender defaulting, collective/plural ambiguity, idiom mapping (“front row”→“first grade”), and discipline‑specific lexicon gaps.

## Top Failure Modes
- Antonym/polarity flips in actions and effects — Description: Core meaning reversals for gestures, mechanics, and logical relations (e.g., nod↔shake, absorb↔reflect, gives relief↔prevents comfort). Why: Arabic verbs for gestures (أومأ/هز رأسه) and effects can be loosely mapped; model over‑relies on nearest frequent counterpart without domain check. Frequency: common; Severity: high. Evidence: “absorbs became reflects” in game abilities; “nods becomes shakes head” in dialogue.

- Technical term substitution and genericization — Description: Replacing precise terms with near neighbors or more common items (photodiodes→LEDs; tread→spoke; everted→inverted rim). Why: Sparse Arabic parallel data for niche domains (hardware, ceramics, rover ops); lexical gaps trigger semantic nearest‑neighbor. Frequency: common; Severity: high. Evidence: “photodiodes become LEDs” in heart‑rate sensor; “everted” read as “inverted,” “glaze” rendered as “glass.”

- Proper nouns and domain labels drift — Description: Names, locations, products and program titles mistransliterated or swapped; guild/class/zone names normalized. Why: Inconsistent transliteration conventions; tendency to domesticate names; low exposure to specific named entities. Frequency: common; Severity: medium–high. Evidence: “Oak Creek Canyon → Oak Creek Valley,” “Duret → Durand,” “Wight Lord → Lord White.”

- Figurative language and metaphor degradation — Description: Key metaphors misread, literalized, or replaced with unrelated imagery (front‑row seat→first grade; rivet in steel→nail in sheet; thirsty star). Why: Idiom interference and calques; Arabic ambiguity without diacritics; preference for higher‑frequency collocations. Frequency: common; Severity: medium–high. Evidence: “front‑row seat” reinterpreted as schooling level; poem imagery “forging”→“folded”; “radiating star”→“thirsty star.”

- Lexical confusions causing semantic swaps — Description: Concrete nouns and species/objects replaced by wrong but phonologically/semantically adjacent items (pavement→bullet; veil→veal; sea bass→barramundi; ox→bull). Why: Lexeme proximity and absence of diacritics; weak domain disambiguation; over‑aggressive synonymization. Frequency: common; Severity: medium. Evidence: “pavement hums” became “bullet whispers”; “veil” became “veal.”

- Legal/standards modality and term precision loss — Description: SHOULD/MUST casing lost; semicolon/comma delimiters altered; key legal terms generalized or swapped (reasonable doubt, minute order). Why: Arabic modality mapping (يجب/ينبغي) flattening; punctuation conventions differ; limited legal parallel data. Frequency: occasional; Severity: high in domain. Evidence: header delimiter changed to comma; “bounded vs defined” swap; “minute order”→“short order.”

- Gendering and number shifts — Description: Neutral “they” rendered as masculine; collective/plural nouns altered (pigeons→dove; benches→players). Why: Arabic forces gender/number choices; حمام (pigeons) as collective; defaults to masculine. Frequency: occasional; Severity: medium. Evidence: neutral pronouns → “he/his”; “pigeons” singularized.

- Cultural/tonal additions or normalization — Description: Injected religious phrases in obituaries; formality shifts; softened/hardened tone (should→must). Why: Cultural priors for Arabic obits; style normalization in MSA; policy‑like tone preferences. Frequency: occasional; Severity: medium. Evidence: added religious blessing in a memorial; “should→must” in guidance.

- Numbers, stats, and IDs errors — Description: Digits transposed; stats altered; time/units normalized imprecisely (sols→days). Why: Attention slip; unfamiliar units; Arabic/English numeral handling. Frequency: occasional; Severity: medium–high where critical. Evidence: claim number 983461→983961; touches 741→761.

## Language‑Specific Factors
- Gender and neutrality: Arabic pushes gendered pronouns; neutral English “they” often becomes masculine, shifting inclusivity.
- Collective nouns and number: Animal collectives (حمام) and mass/plural distinctions cause singular/plural drift (pigeons→dove).
- Gesture semantics: “هز الرأس” commonly implies “no,” while “أومأ برأسه” is “nod”; loose mapping leads to nod/shake reversals.
- Idioms and calques: “Front row” misread as “first grade” (الصف الأول); metaphors like “soft landing” recast as aviation clichés; ceramic “glaze” confused with “glass” (زجاج/طلاء زجاجي).
- Script/diacritics ambiguity: Undiacritized Arabic amplifies homograph risk and near‑neighbor substitutions, encouraging “veil/veal,” “pavement/bullet”‑type drift on back‑translation.
- Register normalization: Tendency to MSA formalization shifts tone (marketing/poetry flattened; legal archaic tone modernized).

## Numbers, Units, and Formatting
- Numerals: Digit transposition and minor stat changes (claim IDs, touch counts). Occasional; medium severity.
- Units/time: “sols” normalized to “days,” PM/afternoon drift. Occasional; medium.
- Delimiters/markup: Semicolon→comma in spec headers; loss of standardized MUST/SHOULD casing. Occasional; high in specs.
- Taxonomy/jargon tables: Domain headers/roles normalized (guilds/cohorts; runway/launch). Occasional; medium.

## Meta/Disclaimer Behavior
- Cultural additions: Religious condolence phrases injected in memorial/obituary contexts. Occasional; medium.
- Register enforcement: Guidance shifts “should→must,” and safety‑like firmness creeps into neutral docs. Occasional; low–medium.
- No notable safety disclaimers beyond tone shifts observed.

## Recommendations
Prompting
- Constrained translation prompt:
  - “Translate into Arabic; preserve proper nouns, numbers, punctuation, and formatting exactly. Do not add cultural formulas. Keep modality (MUST/SHOULD) as in source. Maintain technical terms; if unsure, transliterate and add the English in parentheses.”
  - For dialogue/stage/game text: “Maintain polarity and effects verbatim (absorb≠reflect). Avoid rephrasing gestures; map ‘nod’=‘أومأ برأسه (نعم)’, ‘shake head’=‘هز رأسه (لا)’.”
- Provide a term/glossary block per task (photodiode=ثنائي ضوئي, glaze=طلاء زجاجي, runway=مهلة تمويل, stream graph=مخطط تيارات, ride‑hail=خدمة طلب سيارة عبر التطبيق).
- Request gender neutrality: “Avoid gendering unless specified; prefer plural neutral forms or second person.”

Decoding
- Lower temperature and reduce paraphrase bias for high‑precision domains (legal/spec/tech).
- Apply constrained decoding for numbers/punctuation (copy‑span constraints on IDs, headers, delimiters, MUST/SHOULD tokens).
- Use lexically guided decoding with domain term dictionaries in‑context.

Data and Fine‑tuning
- Augment Arabic parallel corpora for:
  - Technical hardware, legal procedure, standards (RFC‑style), ceramics/art history, gaming UI/mechanics.
- Contrastive fine‑tuning on antonym pairs and gesture disambiguation (absorb vs reflect; nod vs shake) with Arabic mappings.
- Entity‑centric training: transliteration consistency and named‑entity preservation, including geographic toponyms and proper names.
- Style control tuning to prevent cultural formula insertion in obits/news unless present in source.

Post‑processing
- Automatic checks:
  - Entity and term consistency (dictionary/regex for known names, classes, zones).
  - Number/ID validator (Levenshtein distance flag on numerals; checksum when available).
  - Modality/delimiter auditor (MUST/SHOULD; semicolon/comma in headers).
  - Antonym/polarity scanner for high‑risk pairs (absorb/reflect, nod/shake, bounded/defined).
  - Domain lexeme lints (glaze vs glass; photodiode vs LED; everted vs inverted).
- Human‑in‑the‑loop QA triggers when flags raised or domain=legal/technical/gaming/medical.

## Confidence and Coverage
Confidence: medium–high. The failure modes are supported by diverse judges and many samples, with repeated patterns across literary, technical, legal, and gaming texts. Coverage limits: Evidence is round‑trip based and may conflate forward/back errors; some attributions (e.g., diacritics) are inferred from typical Arabic behavior rather than explicitly logged. Domain distribution skews toward narrative, tech, and policy; fewer data points for scientific/medical reports.