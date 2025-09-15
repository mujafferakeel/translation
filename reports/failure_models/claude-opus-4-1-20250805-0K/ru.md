# Failure Model — Claude Opus 4.1 (no reasoning) — Russian

## Summary
Claude Opus 4.1 produces generally faithful Russian translations but shows recurring fidelity risks when domain precision and modality matter. The most salient weaknesses are: inflation of normative force (SHOULD→MUST), drift on domain‑specific terminology (tech/standards, legal, safety, sports, culinary), instability of proper nouns and in‑world names, idiom/style flattening, and small but consequential factual flips (time, polarity, process). Several issues are amplified by Russian‑specific constraints (modals, gender) and common calques.

## Top Failure Modes
- Normative modality inflation (SHOULD→MUST) — Systematic upgrading of obligation in specs/standards; Russian “должен/обязан” used where “следует/рекомендуется” fits. Why: Russian lacks a single crisp SHOULD; model defaults to stronger modal and back‑translates as MUST. Frequency: common; Severity: high. Evidence: judges note multiple SHOULD→MUST substitutions in an RFC‑style doc; “Abstract” also calqued as “Annotation.”

- Domain terminology drift and false friends — Replacing precise terms with near‑synonyms or Russianized bureaucratic terms, altering nuance or class. Why: preference for more common Russian vocabulary and calques; limited domain grounding. Frequency: common; Severity: medium‑high.
  - Tech/SRE: “failover”→“switchover”, “runbook”→“instruction”; “wipe and re‑image” misrendered as image overwrite prohibition.
  - Legal/standards/safety: “exclusive venue”→“exclusive jurisdiction”; “combustible liquid”→“flammable”; “Datasheet”→“Technical Passport.”
  - Roles/titles: “Incident Commander”→“command post”; “Stage Management”→“Assistant Directors.”
  - Sports/culinary: cricket “runs/stumps/bails”→“points/posts/crossbars”; “cocoa nibs”→“cocoa beans”; “russets”→“red potatoes.”

- Named entity and key term instability — Proper nouns, product/ability/location names get normalized or swapped. Why: over‑normalization and synonymization during generation; lack of entity locking. Frequency: occasional‑common; Severity: medium.
  - Examples: “Clockwork Delta”→“Sentinel Delta”; “Vale”→“Veil”; ability/quest names renamed; person name “Li”→“Lee.”

- Polarity and micro‑fact flips — Small shifts that invert or distort critical details. Why: synonym pressure plus Russian lexical ambiguity. Frequency: occasional; Severity: medium.
  - Examples: “food insecurity”→“food security”; “roasted corn”→“fried”; “afternoon”→“evening”; reversal in “humid trace vs fingerprint”; “per‑tenant”→“per‑user.”

- Tone/idiom flattening and register drift — Evocative, archaic, or professional tones are regularized; puns lost. Why: model favors fluent neutral Russian and safer synonyms; sparse exposure to target register. Frequency: common; Severity: medium.
  - Examples: slogans/puns (“Spring into savings” lost), archaic diction modernized; UX/analytics jargon simplified; poetic diction replaced by generic words.

- Role/gender and category misrendering — Gender‑neutral English shifts to masculine Russian; role labels softened or skewed. Why: Russian lacks widely accepted singular neutral; mapping ambiguity for roles. Frequency: occasional; Severity: medium.
  - Examples: “them/their”→“him/his”; “enforcer”→“executor”; “Justice”→“Judge”; “sentient wards”→“sentient guards.”

## Language‑Specific Factors
- Modality mapping: English RFC‑style MUST/SHOULD/MAY lacks one‑to‑one Russian equivalents; “должен/обязан” vs “следует/рекомендуется/можно.” The model over‑selects “должен,” causing back‑translation to inflate to MUST.
- Gender neutrality: English singular they often becomes masculine (“он/его”). Natural Russian workarounds (pluralization, role nouns, ellipsis) are not consistently used.
- Calques and register conventions: Tendency to use Russian bureaucratic/technical calques (“технический паспорт” for datasheet; “аннотация” for abstract), which back‑translate inaccurately in English QA.
- Culture‑bound lexicon gaps: Low‑frequency domains (cricket, nautical period jargon) lead to genericization or category errors.
- Proper noun handling: Russian often transliterates or leaves names; the model sometimes semantically substitutes instead of preserving surface form.

## Numbers, Units, and Formatting
- Minor numeric/time drift: afternoon/evening swaps; alarm count slight changes; “up to” qualifier dropped. Conversions generally not invented, but qualifiers and schedule granularity can shift.
- Capacity/terms: “capacity”→“number of seats” alters implication; hazard classes (“combustible” vs “flammable”) conflated.
- Tables/markup: No systemic table/markup breakages noted; issues are lexical rather than structural.

## Meta/Disclaimer Behavior
- Minimal safety or policy intrusions. One-off odd lexical substitutions (“compaction”→“compactification”) suggest domain mismatch rather than explicit disclaimers. No repeated meta commentary contamination observed.

## Recommendations
- Prompting
  - Lock normative keywords: “Preserve RFC 2119 keywords (MUST/SHOULD/MAY) verbatim; translate with fixed mappings: MUST=ДОЛЖЕН/ОБЯЗАН, SHOULD=СЛЕДУЕТ/РЕКОМЕНДУЕТСЯ, MAY=МОЖНО. Do not upgrade/downgrade modality.”
  - Protect entities and jargon: “Do not translate names, product/ability/quest titles, acronyms; maintain capitalization. Use provided glossary; if unknown, transliterate, don’t substitute.”
  - Tone control: “Maintain original register (archaic/poetic/technical); avoid paraphrasing and synonym swaps unless required by grammar.”
  - Gender neutrality: “Avoid gendered pronouns; prefer plural reformulations or role nouns; omit pronouns where natural.”
- Decoding
  - Lower temperature/top‑p for translation mode to reduce synonym churn.
  - Use a constrained decoder with a do‑not‑translate list for RFC keywords, units, named entities.
- Data and fine‑tuning
  - Fine‑tune on parallel RU corpora in: RFC/specs with explicit modality annotations; SRE/IR playbooks (failover/runbook/wipe‑reimage), legal contracts, safety MSDS (combustible vs flammable), sports glossaries (cricket), culinary terms.
  - Add Russian style guides for abstract vs annotation, datasheet vs passport, and incident management roles.
  - Include contrastive pairs penalizing SHOULD→MUST inflation and term flips (“food insecurity” vs “security”).
- Post‑processing checks
  - Regex audit for RFC keywords and comparators (“up to,” “at least,” “by default” vs “default”).
  - Named entity consistency check (string match list) pre/post translation.
  - Terminology linter against domain termbase (failover/runbook/IC, hazard classes, cricket terms, recipe ingredients).
  - Time/date token diff and polarity check (negations, insecurity/security).
  - Gendered‑pronoun detector with rewrites to neutral constructions.
  - Back‑translation spot QA for high‑risk docs (specs, legal, IR playbooks, labels).

## Confidence and Coverage
- Confidence: medium. Multiple independent judges converge on the same core issues (modality inflation, terminology drift, name instability, tone flattening) across varied domains.
- Coverage limits: Evidence leans toward general prose, tech/standards, and mixed nonfiction; fewer samples in heavy numeric/tabular content and no systematic formatting failures were reported.