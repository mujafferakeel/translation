# Failure Model — Mistral Medium 3.1 — Russian

## Summary
Overall, the model is fluent but often over-normalizes and “smooths” Russian output, which back-translates into stronger modality, generalized terminology, and softened or shifted tone. The most salient weaknesses are: (1) systematic strengthening of normative modality (SHOULD→MUST), (2) replacement of precise legal/technical terms with broader or wrong near-synonyms, (3) instability in named entities and domain-specific lexicon, (4) poetic/literary image loss and tone flattening, and (5) occasional unit/format conversions and stylistic additions that alter fidelity.

## Top Failure Modes
- Modal strengthening (SHOULD→MUST; MAY→CAN/SHOULD)
  — Description: RFC-style and policy texts come back with stronger obligations than the source intended.
  — Why: Russian lacks a one-to-one modal system; the model favors “должен/обязан” over “следует/рекомендуется,” likely due to frequency bias and general-register training.
  — Frequency: common
  — Severity: high
  — Evidence: judges note repeated “SHOULD→MUST” shifts; “understates optionality” and “compliance semantics strengthened.”

- Terminology drift in legal/technical registers
  — Description: Specific terms are generalized or swapped with close-but-wrong neighbors: burglary↔robbery, failover→switchover, gesso putty→glazing ground, living hinge→flexible hinge, “minute order”→“transcript.”
  — Why: Preference for higher-frequency Russian equivalents; sparse parallel data for niche terms; lack of stable term memory across a document.
  — Frequency: common
  — Severity: high
  — Evidence: “minute order labeled transcript throughout”; “failover consistently rendered as switchover.”

- Named entity and role/title instability
  — Description: Proper names, initials, product and place names, ranks/titles drift (Diane↔Diana; Singh initial changed; board→committee; first mate/name changes).
  — Why: Aggressive normalization to familiar forms; Russian inflection triggers reanalysis; model smooths to common titles.
  — Frequency: common
  — Severity: medium
  — Evidence: “first letter of Singh changed”; “Mr. Whitaker→Mr. Wakefield; starboard→mizzen.”

- Domain idioms and factual inversions
  — Description: Game rules and tactical details flipped; cooking, horticulture, nautical, and safety instructions altered (“counts as two actions” vs “both draws”; “open‑pollinated”→“cross‑pollinated”; “stackable”→“foldable”).
  — Why: Low-confidence domain mapping and overreliance on general synonyms; insufficient disambiguation of polysemes; failure to preserve contrastive logic.
  — Frequency: occasional
  — Severity: high
  — Evidence: “Wild Jet draw mechanic inverted”; “pollination type and ripeness test swapped.”

- Poetic/literary tone flattening and imagery drift
  — Description: Metaphors diluted, rhyme/rhythm broken, key images replaced with safer or literal wordings; occasional additions.
  — Why: Training favors literal adequacy and fluency; Russian poetic diction/rhyme require targeted modeling not present; decoder favors safe synonyms.
  — Frequency: occasional
  — Severity: medium
  — Evidence: “rhyme scheme broken; images altered”; “sentinels of chance→guards of random malfunctions.”

- Units, formatting, and stylistic interference
  — Description: Implicit conversion of units (in→cm; ft→m), time formats; added bolding/emphasis; K–8→1–8; Drive→Street.
  — Why: Localization prior and stylistic polishing; lack of hard constraint to “preserve form and units.”
  — Frequency: occasional
  — Severity: medium
  — Evidence: “6 inches→15 cm with label change”; “added bolding; document style altered.”

## Language‑Specific Factors
- Modality mapping: Russian lacks precise SHALL/SHOULD/MAY distinctions; “должен/обязан” vs “следует/рекомендуется/может” is subtle and context-bound. The model overuses “должен,” causing SHOULD→MUST on back-translation.
- Legal taxonomy mismatch: English legal terms (burglary vs robbery; minute order) have non-trivial Russian mappings; the model often picks a familiar but wrong equivalent (“протокол/транскрипт”).
- Inflection and agreement: Names/titles inflect (cases, gender, animacy), increasing risk of reanalysis and drift when mapping back (ranks, ship parts, roles).
- Idioms and set phrases: Russian idiom choices regularly literalize or soften English metaphors, especially in poetry and stylized prose; preservable contrasts and wordplay are lost.
- Domain lexicon sparsity: Nautical, archival, bibliographic, horticulture, and culinary micro-terms in Russian are low-frequency; the model defaults to generic terms.

## Numbers, Units, and Formatting
- Units: Converts imperial↔metric without instruction, sometimes with label shifts (in→cm, ft→m) and occasional meaning tweaks (combustible→flammable).
- Ranges/grades: K–8 rendered as 1–8 (kindergarten dropped).
- Addresses/titles: Street types changed (Drive→Street), “Board”→“Council/Committee.”
- Visual/style: Occasional added bolding/emphasis; capitalization and time format normalization.
- Conversions/inventions: Mostly conversions rather than inventions; but some re-labeling (e.g., “Fulfillment”→“Shipping”).

## Meta/Disclaimer Behavior
- Minimal safety or policy insertions. One-off mild additions noted (e.g., stylistic emphasis), but no systemic disclaimers contaminating output. Trigger appears negligible; not a core issue here.

## Recommendations
- Prompt patterns
  - Enforce modal glossary: “Map MUST=должен/обязан; SHOULD=следует/рекомендуется; MAY=может; SHALL in legal=подлежит/должен (juridical). Do not strengthen/soften modality. Preserve exact modality.”
  - No localization: “Do not convert units, dates, or titles. Preserve numerals and labels as in source.”
  - Entities lock: “Preserve all proper names, initials, product names, and role titles verbatim; do not normalize or translate named entities.”
  - Domain guardrails: “If a domain term is ambiguous, prefer the established technical translation; otherwise keep the English term in parentheses on first use.”
  - Style constraint: “Do not add styling (bold/italics), headings, or tone shifts. Maintain register (legal/technical/poetic).”
  - For poetry: request two-pass output: Version A literal (image/rhyme flagged), Version B poetic (meter/rhyme); choose A for round-trip fidelity.

- Decoding changes
  - Lower temperature/top-p and length normalization for legal/technical to reduce synonym drift.
  - Enable constrained decoding or copy-bias for named entities and units.
  - Use glossary/termbase constraints (forced vocabulary) for key domains (RFC modal verbs, legal taxonomy, DB ops, nautical parts).

- Data and fine-tuning
  - Fine-tune on Russian legal-tech parallel corpora emphasizing modality (RFC 2119/8174-compliant texts), court clerical artifacts (“minute order” vs “transcript”), and DB operations docs (“failover” vs “switchover”).
  - Augment with curated domain lexicons (nautical Russian, bibliographic terms, horticulture/cookery).
  - Add contrastive pairs penalizing modality strengthening and unit conversion.
  - Include poetry/prose parallel with alignment targets for imagery and rhyme preservation.

- Post‑processing checks
  - Modality linter: diff SHOULD/MUST/MAY/SHALL across source and back-translation.
  - NE/terminology diff: regex exact-match for names, titles, product/feature strings; flag changes.
  - Unit/format sentinel: detect and block unit conversions; enforce original units and labels.
  - Domain validators: rule checks for grade ranges (K–8), legal charges (burglary vs robbery), DB ops (“failover” not “switchover”).
  - Style QA: detect added markdown or emphasis; strip if present.

## Confidence and Coverage
Confidence: moderate. Evidence is dense and consistent across multiple judges and domains, with repeated signals for modality strengthening, term drift, and entity instability. Coverage skews toward legal/technical, instructional, and literary texts; fewer examples of highly colloquial dialogue and specialized scientific subdomains.