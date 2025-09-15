# Failure Model — GPT-5 (medium reasoning) — Polish

## Summary
Overall, gpt-5-medium produces fluent Polish with high factual fidelity, but exhibits systematic paraphrasing that erodes register, technical specificity, and subtle tone. The most salient weaknesses are: (1) over-normalization of terms (technical, legal, scientific) to more common Polish synonyms; (2) pronoun and referent drift (especially loss of gender neutrality and shifts in agency/personhood); (3) precision loss on domain nomenclature (species, materials, hazards, UI strings); and (4) tone flattening in literary/poetic passages. Numbers/formatting are generally stable, while meta/disclaimer intrusions are rare.

## Top Failure Modes
- Terminology dilution in technical/legal domains — Replaces precise domain terms with broader or adjacent synonyms, altering semantics (planned vs unplanned; access vs identity; hazard class). Why: preference for frequent Polish collocations and inadequate domain disambiguation. Frequency: common. Severity: high.
  - Evidence: “failover”→“switchover” across a procedure; “authentication” rendered as “authorization”; “combustible liquid” softened to “flammable.”
- Gender and referent drift — Neutral “they” becomes masculine “he,” and human roles become impersonal “project/it,” shifting inclusivity and agency. Why: Polish lacks singular gender‑neutral pronouns; the model defaults to masculine or abstracts the agent. Frequency: common. Severity: medium.
  - Evidence: repeated “they→he” substitutions; artist reframed from “singer‑producer” to an impersonal “project,” quote voice altered.
- Domain nomenclature substitution (taxonomy, materials, components) — Specific labels replaced with nearby but incorrect items (species, soil types, fasteners), preserving gist but losing precision. Why: lexical proximity and frequency bias; limited anchored term memory. Frequency: occasional. Severity: medium–high (depends on domain).
  - Evidence: “red knots”→“red‑necked stints” (+added species); soil classes (“silty clay”→“clayey clay”); “cam bolt”→“cam screw.”
- Register and style flattening — Formal/clinical/legal/reporting voices softened; incident and UI/report headings paraphrased; literary tone made literal. Why: decoding prefers natural, general Polish; insufficient register control. Frequency: common. Severity: medium.
  - Evidence: “cache”→“buffer”; incident report “Lessons Learned”→“Conclusions”; legal diction systematically generalized; poetic images like “hush”→“semidarkness,” “hum”→“rumble.”
- Lexical sense misselection for polysemy/idioms — Picks wrong sense under contextual pressure, altering tone/meaning. Why: English→Polish sense mapping errors and idiom literalization. Frequency: occasional. Severity: medium.
  - Evidence: “button” (ending beat)→“punchline”; “scavenger”→“looter”; “horsefly”→“bittern”; “line of sight”→“eye contact.”
- Proper-noun/UI label drift and structural edits — Product strings, pillar names, button texts, and singular/plural counts changed, reducing fidelity. Why: preference for “natural” phrasing over string preservation; agreement/number smoothing in Polish. Frequency: occasional. Severity: medium.
  - Evidence: “Purposeful”→“Thoughtful”; button labels (“Complete Purchase” altered); singular “dungeon” made plural.

## Language‑Specific Factors
- Gender neutrality gap: Polish lacks a natural singular gender‑neutral pronoun; the model defaults to masculine (on) or impersonal forms, causing inclusivity shifts and agency loss.
- Case and agreement pressures: Polish inflection encourages explicit subjects and number agreement; this can push singular→plural smoothing and pronoun concretization.
- Register stratification: Polish has clear boundaries between colloquial, professional, and legal registers; overuse of everyday synonyms makes technical/legal text sound less precise.
- Polysemy and idioms: English polysemous terms (“button,” “scavenger,” “cache”) require domain‑aware picks; literal or frequency‑biased choices skew meaning.
- Terminology gaps and borrowing: Where Polish has narrow technical calques, the model sometimes substitutes broader domestic terms (“failover”→“przełączenie” instead of “awaryjne przełączenie”).

## Numbers, Units, and Formatting
- Generally stable with few quantitative errors. The main issues are categorical/label precision rather than numeric conversion.
- Specific term labels within structured content occasionally drift: hardware parts (“cam bolt”→“cam screw”), button text strings, hazard classes. Conversions/inventions: rare. Tables/markup: not reported as problematic.

## Meta/Disclaimer Behavior
- Rare instances of meta notes; largely clean. A minor “note/memo” phrasing shift and one DM‑style aside adjustment observed. Triggers: instructional or game-master contexts. Severity: low.

## Recommendations
- Prompt patterns
  - Enforce register and terminology: “Translate into Polish preserving domain terminology exactly; do not paraphrase technical terms; maintain original register (legal/clinical/UI).”
  - Gender handling: “Preserve gender neutrality; avoid assigning gender; prefer impersonal or role‑repetition forms (e.g., ‘osoba/autor/uczestnik’) instead of ‘on/ona’ unless specified.”
  - String fidelity: “Treat product names, UI labels, species, and hazard classes as immutable strings; do not localize unless a standard Polish term exists.”
  - Quote and count fidelity: “Preserve quotes verbatim; keep singular/plural and list membership unchanged.”
- Decoding changes
  - Lower temperature and reduce paraphrase propensity for translation tasks; increase repetition penalty only slightly to avoid over‑synonymy while not harming fluency.
  - Use constrained decoding with a glossary lock for sensitive terms (authn/authz, failover, hazard classes, soil taxonomy, species lists, game/UI strings).
- Data and fine‑tuning
  - Augment with high‑quality EN‑PL parallel corpora in targeted domains (SRE/infra, security, HAZMAT, archaeology/soil science, ornithology, legal/procedural, gaming UI).
  - Include Polish style guides for legal/clinical/incident reports to reinforce register.
  - Curate a gender‑neutral Polish dataset showcasing impersonal constructions and role‑based reference to model neutral voice strategies.
  - Build domain termbanks (auth/authz, cache vs buffer, failover vs switchover, combustible vs flammable; species and soil glossaries) and fine‑tune with them.
- Post‑processing checks
  - Terminology QA: automatic diff against a locked glossary; flag substitutions (e.g., auth/authz, failover/awaryjne przełączenie, cache/pamięć podręczna).
  - Nomenclature validators: species and soil names checked against gazetteers; hazard labels and material classes validated.
  - UI/string protection: detect and preserve button and feature names; enforce exact match unless a known localized term exists.
  - Gender neutrality scan: flag masculine pronouns where source is neutral; suggest impersonal rewrites.
  - Quote integrity: compare quoted spans for divergence; enforce exactness.
- Operational workflow
  - Two‑pass approach: raw literal pass with glossary constraints → stylistic polish pass limited to non‑glossary spans.
  - Provide per‑project glossaries and register notes to the model at inference time.

## Confidence and Coverage
Confidence: medium. The evidence is dense around synonym drift, gender/pronoun shifts, and domain terminology errors across multiple judges and samples, with fewer reports on numbers/formatting and meta behaviors. Topic mix spans literary, legal, technical, and UI/game texts, supporting generalization to those domains, but may underrepresent highly numeric or tabular content.