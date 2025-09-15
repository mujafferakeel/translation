# Failure Model — Gemini 2.5 Pro — Polish

## Summary
Gemini 2.5 Pro’s Polish round‑trip translations are generally faithful in content but show systematic drift in register and terminology. The most salient weaknesses are: (1) consistent shift to a more formal, verbose, and impersonal tone (often changing 2nd→3rd person and softening imperatives), (2) dilution or mis-selection of domain jargon across legal, technical, sports, and corporate contexts, (3) loss of gender neutrality via masculine defaults and other inclusivity slips, (4) precision errors in safety/technical terms and specialized lexicons, and (5) stylistic flattening of poetic/idiomatic language. Secondary issues include mild normalization of names/titles, number/date formatting shifts, and occasional small additions.

## Top Failure Modes
- Register drift to formal/impersonal Polish — Rewrites concise, imperative, or conversational English into formal, verbose, or passive Polish; often switches 2nd→3rd person and adds politeness markers. Why: bias toward formal Polish register and helpfulness; training data skew; decoding preferring safe high‑probability forms. Frequency: common; Severity: medium.
  - Evidence: “you” instructions rendered as “the user” or advisory “powinno się”; added “proszę,” softening urgency.

- Jargon dilution and wrong term mapping — Replaces precise domain terms with generic synonyms or near‑neighbors; sometimes picks the wrong sense. Why: insufficient domain grounding; preference for high‑freq synonyms; lack of enforced glossaries. Frequency: common; Severity: high in domain texts.
  - Evidence: “minute order”→“abridged minutes”; “Evidence Log”→“Register”; “Ranger District”→“forest district”; “runway (financial)”→operational “czas działania”; sports roles “guard/wing”→“defender/forward”.

- Gender neutrality loss and pronoun shifts — Neutral “they/their” becomes masculine (“he/him”), changing inclusivity and sometimes meaning. Why: Polish lacks singular they; model defaults to masculine rather than impersonal or periphrastic solutions. Frequency: common; Severity: medium.
  - Evidence: Reviews and bios shift to “on/jego”; HR/performance texts: neutral subject becomes male.

- Safety/technical precision errors — Subtle but critical substitutions in risk or instruction contexts. Why: sense disambiguation errors; over‑generalization. Frequency: occasional; Severity: high.
  - Evidence: “combustible liquid”→“flammable liquid”; “four‑way stops”→“equivalent intersections”; “water caches”→“springs”; “fountain’s still”→“frozen”.

- Stylistic and idiomatic flattening — Poetic imagery, rhythm, puns, and idioms become literal or prosaic; cross‑domain voice (sports announcer, LitRPG UI, corporate voice) loses color. Why: literal decoding preference; weak style transfer; conservative paraphrasing. Frequency: common; Severity: medium.
  - Evidence: Announcer jargon and onomatopoeia toned down; species/imagery substitutions (“horsefly”→“bittern”); “button” (screenwriting)→“punchline”.

- Proper names, titles, and category normalization — Changes headings, titles, character names, or product pillars; localizes when it shouldn’t. Why: normalization/localization heuristic; lack of name locking. Frequency: occasional; Severity: medium.
  - Evidence: “Grounded” pillar→“Authentic”; “Circular”→“Closed loop”; “Ms.”→“Mrs.”; Shakespeare roles renamed; misspelling “Dżahangir”.

- Numbers, dates, labels, and categorical values drift — Minor but real shifts in numeric formats and categorical tags. Why: Polish convention adaptation; uncertainty with specialized labels. Frequency: occasional; Severity: low–medium.
  - Evidence: “Uncommon 1–1000”→“1/1000”; 12h→24h time; “next purchase”→“future purchases”.

- Small additions/authorial intrusions — Adds “please,” clarifications, extra examples, or softeners not in source. Why: helpfulness bias; politeness norms in Polish; over‑explaining. Frequency: occasional; Severity: low–medium.
  - Evidence: Added “proszę”; extra rhyme examples; inserted “cash” in burn multiple.

## Language‑Specific Factors
- No singular “they” in Polish leads to default masculine forms. The model seldom chooses impersonal/passive (“należy,” “użytkownik,” “osoba”) or periphrastic avoidance, causing inclusivity loss.
- Strong norm toward formal register in public/instructional Polish nudges outputs away from colloquial, imperative, or voicey English (podcast/script/game UI loses immediacy).
- Term granularity mismatches: English has fine‑grained roles (sports positions, legal motions, tech pillars) with no single canonical Polish equivalent; the model often picks broader, safer hypernyms.
- Polysemy traps amplified in Polish: “runway,” “cache,” “button,” hazard classes (combustible vs flammable) — Polish lexical choices demand explicit disambiguation, which the model doesn’t force.
- Inflection and case may encourage nominalizations/passives, reinforcing impersonal tone and lengthening sentences.

## Numbers, Units, and Formatting
- Numerals: occasional normalization or reinterpretation (range “1–1000” shown as fraction “1/1000”).
- Time/date: shift to Polish 24‑hour and date formats; usually harmless but can break fidelity requirements.
- Categories/ratings: “Exceeds”→“Above,” “Outperform”→“Above Market,” altering calibrated labels.
- Lists/headings: mild renaming (“Non‑Goals”→“Out of scope”), capitalization shifts.
- Units/technical labels: rare but notable category swaps (“recoveries”→“tackles” in sports stats).

## Meta/Disclaimer Behavior
- No safety disclaimers observed, but polite additions and honorifics appear in customer/therapist/notice contexts (“Pan Michael,” added “proszę”), shifting tone toward formal politeness.

## Recommendations
- Prompting
  - Explicit register lock: “Preserve second person and imperative mood; do not add ‘proszę’; match original formality and concision.”
  - Gender neutrality: “Preserve gender neutrality; prefer impersonal forms (należy, prosimy) or repeat noun (‘osoba’) instead of masculine pronouns.”
  - Terminology guardrails: Provide a glossary and “Do not localize or generalize these terms; copy exact casing for headings/labels.”
  - Disambiguation hints: Add parenthetical sense notes for polysemous items (runway=financial, cache=water stash).
  - Style fidelity: “Preserve voice/jargon/onomatopoeia; avoid paraphrase unless needed for idiomatic Polish.”
- Decoding
  - Lower temperature/top‑p to reduce inventive additions; enable constrained decoding with glossaries and protected spans (NER, headings, labels).
  - Penalize lengthening to curb verbosity; enable casing/number preservation checks.
- Data
  - Fine‑tune or adapter‑train on Polish parallel corpora for: legal dockets, safety manuals, UI/game text, sports commentary, and corporate/policy docs with enforced glossaries.
  - Add contrastive pairs targeting frequent confusions (combustible vs flammable; runway finance vs airfield; cache vs spring; minute order; Evidence Log).
  - Curate inclusivity training for Polish strategies avoiding gendered defaults.
- Post‑processing QA
  - Terminology validator: glossary diff with hard fail on deviations.
  - Pronoun audit: flag masculine pronouns when source is neutral; suggest impersonal rewrites.
  - Numbers/dates checker: regex to preserve ranges, units, timestamps; block unauthorized format changes.
  - Name/title lock: NER‑based copy‑through for product pillars, character names, headings.
  - Style lint: detect added politeness markers (“proszę,” honorifics) and 2nd→3rd person shifts.

## Confidence and Coverage
- Confidence: medium‑high. Dozens of low‑scoring cases across diverse judges and domains show consistent patterns (register drift, jargon dilution, gender/pronoun shifts, safety precision slips).
- Coverage limits: Evidence skews toward mid‑to‑high fidelity samples with nuanced issues rather than catastrophic failures; fewer examples of tables/markup or heavy numeric content.