# Failure Model — Qwen 3 Max Preview — Russian

## Summary
Qwen 3 Max Preview generally preserves meaning in Russian round‑trip translations but shows consistent drift on precision-heavy content. The most salient weaknesses are: (1) polarity/role reversals in rules and legal texts; (2) systematic substitution of domain terms and proper names with near‑synonyms or adjacent concepts; (3) tone/formality inflation and modality hardening; (4) fragile handling of specialized nomenclature (species, sports, UX/legal/art restoration); and (5) occasional unit/temporal slips. Poetry and idiomatic prose often lose imagery and form.

## Top Failure Modes
- Polarity/role reversals in mechanics and legal stance — Critical meaning flips (face up/down; dissent/concurrence) that break rules or misstate decisions; likely triggered by ambiguous Russian lexical choices and over‑confident paraphrasing during back‑translation. Frequency: occasional. Severity: high.
  - Evidence: “market cards rendered as face down instead of face up”; “dissent mislabeled as concurrence.”

- Domain term drift and taxonomy mislabeling — Replacing precise terms with close but wrong ones (sports set pieces; legal headers; technical jargon; biological species), sometimes swapping to a sibling concept. Likely due to inadequate domain priors and preference for high-probability synonyms in Russian that back‑translate loosely. Frequency: common. Severity: high.
  - Evidence: “through ball → one‑touch pass”; “minute order → protocol ruling”; “dunlins/red knots → avocets/sandpipers.”

- Proper noun and label instability — Spelling variants and renamings (names, variety titles, brands, route names), plus narrowing/broadening of labels. Caused by transliteration uncertainty and normalization to common Russian forms. Frequency: common. Severity: medium.
  - Evidence: “Ember → Amber; open‑pollinated → self‑pollinating”; “Li → Lee; Ms. → Mrs.”; “Silent Concord → Silent Accord.”

- Tone inflation and modality hardening — Conversational or suggestive source becomes formal/mandatory (“should” → “must”), euphonious idioms become neutral; Russian’s register and aspect choice push toward formal bureaucratic style. Frequency: common. Severity: medium.
  - Evidence: “should enroll → must register”; repeated soft→strict shifts (“should”→“must”, “respectfully submits”→“officially challenges”).

- Imagery/poetic/formal structure loss — Rhyme scheme, metaphors, or technical metaphors simplified or contradicted by analysis; personification often literalized. Likely due to prioritizing semantic clarity over form. Frequency: occasional (common within poetry). Severity: medium.
  - Evidence: “poem loses rhyme scheme; analysis becomes nonsensical”; “iron lips → iron edges.”

- Partial omissions/additions of qualifiers and details — Dropping venue clauses, “up to,” colors, or adding small descriptors; often from compression and register smoothing. Frequency: occasional. Severity: medium.
  - Evidence: “key sentence on venue omitted”; “lost ‘up to’ (overstates degree).”

- Units, time, and measurement slips — Misrendered units or specialized measures; timeline shifts. Frequency: rare–occasional. Severity: medium.
  - Evidence: “half‑knot → half‑stuck”; “30‑sol → 30‑day”; “tomorrow afternoon → by noon.”

## Language‑Specific Factors
- Modality and aspect pressure: Russian lacks a clean “should” that’s as soft as English; choices like “следует/нужно/должен” lead to hardening (“should → must”). Imperfective/perfective selection can imply obligation or finality (“stand firm” → “stand to the death”).
- Lexical gaps and defaults: Legal and bureaucratic Anglicisms (“minute order,” “onboarding,” “backoff”) push the model to more generic Russian terms (“постановление протокола,” “регистрация”), which then back‑translate imprecisely.
- Animacy/gender interference: Nouns like “книга” (feminine) encourage gendered pronouns; back‑translation can yield “she” for inanimate referents.
- Terminology traditions: Russian sports/tech/culinary registers have distinct term sets; calques create near‑misses (“сквозной пас” vs “пас в одно касание”), and biology common names often map many‑to‑one, inducing species swaps.
- Transliteration variance: Proper names (Li/Lee; Sara/Sarah) and toponyms often normalize to frequent Russian patterns; case endings can spawn misspellings when reversed.

## Numbers, Units, and Formatting
- Units/measures: Occasional misconversion or corruption of specialized units (“half‑knot,” sol → day).
- Time and schedules: Mild drift in exact times (“afternoon → noon”), 12/24‑h formatting shifts.
- Cataloging/metadata labels: Museum/library fields generalized (“Accession Number → Inventory number”; “Depository → Repository”).
- Headings/sections: Technical headers normalized (“Non‑Goals → Out of Scope”) reducing spec nuance.
- Quantifier qualifiers: “Up to,” ranges, or per‑tenant/per‑client distinctions sometimes collapsed.

No systematic invention of numbers observed, but occasional normalization/substitution can alter constraints.

## Meta/Disclaimer Behavior
- Little to no safety or policy intrusions detected. The main “meta” issue is explanatory paraphrase creeping into headers or labels, not policy disclaimers.

## Recommendations
Prompting
- Lock terminology and polarity:
  - “Translate exactly; do not paraphrase. Preserve orientation/state words (face up/down, on/off, dissent/concurrence).”
  - Provide a glossary for domain terms (sports, legal, UX, art conservation, biology) and require exact reuse.
  - Add constraints: “Do not strengthen modality; prefer ‘следует/рекомендуется’ over ‘обязан/должен’ unless explicitly present.”
- Proper nouns handling:
  - “Preserve names, titles, route/species/item names as in source; do not transliterate unless list provided.”
  - Use tags: {NE_START}{NE_END} around critical names to discourage alteration.
- Style retention:
  - “Maintain register and figurative language; preserve rhyme/parallelism where present; avoid explicative rewrites.”

Decoding
- Increase length and decrease paraphrase pressure on critical texts:
  - Slightly higher temperature with nucleus sampling can reduce deterministic but wrong synonym locking; pair with constrained decoding for glossary terms.
  - Use lexically constrained decoding for must‑keep tokens (terms, polarity cues, units).

Data and fine‑tuning
- Add Russian↔English parallel data with:
  - High‑precision legal/sports/UX/art/culinary corpora focusing on term disambiguation.
  - Contrastive pairs for polarity minimal pairs (face up/down; dissent/concurrence; combustible/flammable).
  - Modality calibration sets (“should vs must”) and aspect selection tasks.
  - Proper‑noun preservation training (name variants; transliteration vs identity).
  - Poetry/formal‑structure parallel sets emphasizing meter/rhyme retention.

Post‑processing QA
- Automatic checks:
  - Polarity and orientation lexicon diff: flags when any of {face up/down, on/off, allow/forbid, dissent/concurrence} flip.
  - Named‑entity and term consistency diff: Levenshtein + glossary validation; block edits unless whitelisted.
  - Units/time validator: detect unit changes (knot/sol/AM‑PM vs 24h) and timeline shifts.
  - Quantifier/qualifier detector: ensure “up to,” “no earlier than,” and per‑scope adjectives match.
- Human spot‑checks:
  - Domain SMEs on low‑confidence segments (species lists, set pieces in sports, restoration materials).
  - Poetic/figurative passages get style pass with form checklist.

## Confidence and Coverage
Confidence: moderate. Evidence spans many judges and domains with repeated patterns (term drift, polarity flips, modality hardening, name instability). Poetry issues and species/technical nomenclature errors recur but are less frequent overall than synonym drift and tone shifts. Limited direct evidence on numbers/dates, but enough instances to flag as occasional. No notable safety/meta contamination observed.