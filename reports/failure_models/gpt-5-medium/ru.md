# Failure Model — GPT-5 (medium reasoning) — Russian

## Summary
GPT-5 (medium reasoning) reliably preserves core meaning in Russian round‑trip translations but repeatedly weakens register and precision. The most salient weaknesses are: (1) lexical dilution of specialized terms and tones (tech, legal, safety, marketing), (2) over‑generalization or misclassification of domain terminology (biology, tools, hazards), (3) modality/register drift (shall/must softened; archaic/legal style flattened), (4) figurative/idiomatic misreads and poetic flattening, and (5) occasional small omissions/additions and detail drift (body parts, locations, captions). Language‑specific friction points include Russian grammatical gender, modality choices, and ambiguous domain term mappings (e.g., failover/switchover).

## Top Failure Modes
- Lexical dilution of specialized terms and tone — Replaces precise terms with generic synonyms, softening domain nuance (tech/legal/marketing). Why: Russian lexical preference for broader synonyms; model’s safety/fluency bias favors common words over jargon. Frequency: common; Severity: medium.
  - Evidence: “failover”→“switchover,” “runbook”→“guideline”; “Duty Cycle”→“Operating mode”; “confidentiality”→“privacy.”
- Terminology over‑generalization/misclassification — Substitutes with near‑neighbor but wrong category/species/tool/hazard, preserving gist but losing specificity. Why: distributional proximity in training data and ambiguous Russian term inventories. Frequency: common; Severity: high when safety/biology/tools matter.
  - Evidence: “combustible”→“flammable” (hazard class shift); “red knot”→“red‑necked stint,” “torque drivers”→“impact drivers.”
- Modality and register drift (legal/safety/formal) — Softens or alters force of requirements, and flattens archaic/legal diction. Why: Russian has multiple ways to express deontics; defaulting to “нейтрально/естественно” weakens force. Frequency: common; Severity: medium to high in compliance contexts.
  - Evidence: “must”→“should”; “shall”→present; “Whereas” paraphrased modernly; “Advisory”→“Warning” (tone shift).
- Figurative/idiomatic misreads; poetic flattening — Misinterprets subtle cues or simplifies imagery/metaphor, shifting narrative voice. Why: idiom equivalence and metaphor density; Russian paraphrase bias toward clarity. Frequency: common; Severity: medium.
  - Evidence: “still” (fountain)→“has frozen”; “ox”→“bull”; “replay loop”→“scrolling cycle”; child’s “why”→“where”.
- Small omissions/additions — Drops or inserts brief details that change nuance or instructions. Why: compression/expansion during paraphrase; list normalization. Frequency: occasional; Severity: medium when procedural.
  - Evidence: Missing dialogue about dropping phone; omits “education” in final paragraph; adds “adjustable” (wrench), “light snacks.”
- Gender and pronoun drift — Unwanted gendering or back‑translation of grammatical gender as biological gender. Why: Russian grammatical gender forces choices; back‑translation reifies “она/он” into “she/he.” Frequency: occasional; Severity: low to medium (tone/inclusivity).
  - Evidence: Neutral “their”→“his”; inanimate referred to as “she.”
- Spatial/physical detail drift — Incorrect body part, orientation, or location; caption modality shifts. Why: near‑synonym substitution and Russian collocation preferences. Frequency: occasional; Severity: medium.
  - Evidence: “neck”→“head”; “wrap arms”→“wrap hands”; front quarter panel→rear side panel; “sail Windy Bay”→“walk along.”
- Countability, numerals, and minor unit shifts — Singular/plural flips, unit name changes, isolated typos. Why: Russian number agreement and non‑mapped unit norms. Frequency: rare; Severity: low to medium.
  - Evidence: centimeter vs inch; plural “alarms”→singular; “24 ч” typo.

## Language‑Specific Factors
- Modality mapping: English must/shall vs Russian должен/надо/следует; model often picks “следует/стоит,” softening obligations.
- Legal/formal register: Archaic openers (“Whereas”) lack natural Russian equivalents; the model paraphrases into modern prose, losing style.
- Grammatical gender: Russian forces gender on nouns; back‑translation may improperly gender neutral English or map inanimate “она/он” to “she/he.”
- Domain ambiguity in Russian lexicon: Multiple common Russian renderings collapse distinctions (e.g., failover/switchover both “переключение”; “shelter”/“evacuation point”).
- Idioms and metaphor density: Russian preferrs explicit paraphrase; imagery is normalized (poetry/marketing loses vividness).
- Terminology calques: Hazards (“flammable/combustible”), biology (species), and tools lack one‑to‑one mapping; model overuses colloquial or umbrella terms.

## Numbers, Units, and Formatting
- Units: Occasional unintended conversions or mismatches (cm↔inch).
- Countability/number: Singular/plural flips (archives/alarm), article omissions (expected in Russian) that reappear in back‑translation as meaning shifts.
- Headings/labels: Titles normalized (“Non‑Goals”→“Out of scope”), advisory level labels drift (“Advisory”→“Warning”).
- Tables/markup: No systemic failures observed; minor list style changes occur.

## Meta/Disclaimer Behavior
- Minimal explicit policy disclaimers. Subtle meta drift shows up as tone/label changes in safety messaging (“Advisory” vs “Warning”), likely a register choice rather than a safety interjection.
- Triggered by: safety/alert content and institutional forms where the model prefers conventional Russian phrasing.

## Recommendations
- Prompting
  - Enforce term fidelity: “Preserve domain terms verbatim unless a standard Russian term exists; maintain a glossary; do not generalize.”
  - Modality lock: “Do not soften obligations; map must/shall to должен/обязан; retain advisory level labels exactly.”
  - Style constraints: “Preserve archaic/legal openings and poetic imagery even if unusual; avoid simplification.”
  - Gender guidance: “Keep source gender neutrality where unspecified; avoid assigning gender to inanimates in back‑translation.”
  - Numerics: “Do not convert units; keep numerals and plurals as in source; preserve capitalization in titles.”
- Decoding
  - Lower temperature/top‑p for legal/technical/safety passages to reduce paraphrase drift.
  - Use constrained decoding with a terminology list (failover, duty cycle, credential stuffing, Emergency Shelter).
- Data augmentation
  - Fine‑tune on EN↔RU parallel corpora for: legal (archaic/shall), safety advisories, SRE/infra docs (failover/runbook), biology/taxonomy, tools/engineering, and marketing/poetry with preserved figurative language.
  - Inject counter‑examples emphasizing combustible vs flammable, species names, and shelter taxonomy with authoritative mappings.
- Fine‑tuning targets
  - Penalize synonym substitution for whitelisted terms; reward register preservation (shall/Whereas).
  - Add gender‑neutrality constraints and tests; keep inclusive pronouns when source is neutral.
- Post‑processing checks
  - Terminology QA: automatic diff against glossary; flag “failover→switchover,” “Emergency Shelter→Evacuation Point,” hazard classes, legal deontics.
  - Modality check: detect must/shall weakened to should/можно/следует.
  - Unit/number validator: flag unit changes; singular/plural mismatches on key nouns.
  - Entity/biology linker: map species/tools to Wikidata; block near‑neighbor swaps.
  - Risk domain linting: highlight changes to warnings/advisories and safety instructions; require human review.

## Confidence and Coverage
Confidence: moderate. Evidence is dense across many domains with consistent patterns of term dilution, register drift, and occasional detail loss; judges are diverse. Limitations: Most samples are mid‑ to high‑scoring, so catastrophic errors may be underrepresented; limited visibility into highly technical numeric/table formatting and low‑resource idioms.