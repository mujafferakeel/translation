# Failure Model — Grok 4 — Swahili

## Summary
Grok 4 shows strong fluency but recurrent meaning drift when translating to/from Swahili, especially on ambiguous items where Swahili underspecifies gender, articles, technical registers, and some lexicon. The most salient weaknesses are: (1) polarity and role inversions (must/should, nod/shake, client/consumer) and other fact flips; (2) systematic term dilution in legal/technical domains; (3) lexical confusion for near‑neighbors and culture‑specific items (code/tax, chickpea/lentil, leather/skin); (4) mishandling of time/units and counts; and (5) incomplete back‑translation with Swahili fragments or invented terms.

## Top Failure Modes
- Polarity, role, and action inversions — Negations, obligations, and who‑does‑what often flip. Why: Swahili lacks morphological cues for modality and roles; model overgeneralizes during paraphrase. Frequency: common; Severity: high.
  - Evidence: “must” softened to “should” in legal instructions; child’s affirmative nod rendered as refusal; protocol blamed “clients” instead of “consumers.”

- Terminology dilution in legal/technical registers — Precise terms mapped to generic or adjacent words. Why: domain gaps and preference for common synonyms in Swahili back‑translations. Frequency: common; Severity: high.
  - Evidence: “deference”→“wisdom,” “classifier”→“guide,” “Small Claims carve‑out”→“waiver,” “median”→“average,” “data export”→“data migration.”

- Ambiguous Swahili → wrong English specificity (gender, agency, definiteness) — Gender and participant roles drift. Why: Swahili pronoun “yeye” and lack of articles; model guesses in back‑translation. Frequency: common; Severity: high.
  - Evidence: repeated she↔he swaps for protagonists; vendor age reversed; “hosted events”→“attended.”

- Near‑synonym and look‑alike lexical swaps — Semantically close items routinely confused. Why: overlapping Swahili glosses and sparse domain lexicon. Frequency: common; Severity: medium.
  - Evidence: “code”→“tax”; “canopy”→“roof,” “cornice”→“corner”; “leather”→“skin”; “ox”→“cow”; “beef shank”→“tendon.”

- Culinary and material term mis-mapping — Ingredient and tool names frequently substituted. Why: low-frequency loanwords in Swahili and polysemy in food/tool vocab. Frequency: common; Severity: medium.
  - Evidence: chickpea↔lentil; basil/fennel↔dill; trowel→hoe; torque drivers→controllers.

- Time and unit misinterpretation — Hours, counts, and measures drift. Why: Swahili time expressions and aspect; number/specifier ambiguity. Frequency: occasional; Severity: high when safety/accuracy matters.
  - Evidence: “until ten o’clock” → “until four o’clock”; marine “chop” vs “swells”; week→weeks; “two‑measure”→“two‑beat.”

- Safety/tone softening and register flattening — Formal/archaic or expert tone reduced to simpler wording. Why: preference for high‑probability paraphrases; Swahili lacks articles and often uses general nouns. Frequency: common; Severity: medium.
  - Evidence: legal and archaeological jargon simplified; poetry and satire lose texture; “binding”→“mandatory.”

- Partial or contaminated back‑translations — Segments remain in Swahili or invented names appear. Why: quotation/sign detection errors; copying due to perceived foreign text; hallucinated calques. Frequency: occasional; Severity: medium.
  - Evidence: major sections or slogans left in Swahili; “Librarian” became invented “Maktabari.”

- Directional/temporal preposition flips — “By” vs “until,” before/after, up/downhill. Why: preposition mapping ambiguity; discourse smoothing. Frequency: occasional; Severity: medium.
  - Evidence: tide “by”→“until”; “barrels downhill”→“passes under the hill.”

## Language‑Specific Factors
- Pronouns and gender: Swahili “yeye” (he/she) and lack of gendered nouns lead to frequent gender/pronoun guessing errors on back‑translation.
- Articles/definiteness: No articles in Swahili causes shifts in specificity and scope (singular/plural, generic/specific).
- Polysemy and near neighbors:
  - kodi (tax/rent) vs “code”; ngozi (skin/leather); ngazi (stairs/flight).
  - Time: confusion between “saa…” conventions and colloquial phrasing; “saa nne usiku/saa kumi” patterns are easy to misread in English constraints.
- Domain borrowings: Low‑resource borrowings for culinary/herb names (basil/fennel/dill) and technical jargon cause substitutions.
- Idioms and tone: Poetic compression and metaphor in Swahili prompt literal or flattened English; idioms like “kupumua” (breathe/sigh) oscillate in tone.

## Numbers, Units, and Formatting
- Time errors and conversions: “ten o’clock”→“four o’clock”; East African time phrasing misread.
- Statistics/metrics drift: “median”→“average”; “plurality”→“number”; person‑throughput generalized.
- Counts/aspect: week→weeks; exhibition singular/plural mismatches.
- Domain measures: marine “chop” vs “swells”; music “measure” vs “beat.”
- Tables/labels: Slogans and labels sometimes left untranslated or re‑localized.

## Meta/Disclaimer Behavior
- Occasional contamination of register: headings and policy‑like rephrasings (“questions/trainings,” “caution”→“warning”).
- Triggered by: instructional or standards texts; evaluation headings. No major safety disclaimers, but sporadic editorialization softens modality.

## Recommendations
- Prompting
  - Enforce role/modality tracking: “Preserve polarity (must/may/should), subject/object roles, and gestures (nod vs shake).”
  - Gender/placeholders: “Mark ambiguous gender as [yeye] and do not infer; preserve original gendered cues.”
  - Time/units gate: “Copy all numerals/time exactly; do not convert time; flag if unsure.”
  - Terminology locks: Provide a glossary (e.g., code, classifier, carve‑out, canopy, chickpea, fennel) with “use exactly as given.”
  - Quotations/signs: “Translate all signage/slogans in brackets; do not leave Swahili text in the back‑translation.”
- Decoding
  - Lower temperature/top‑p for legal/technical passages to reduce synonym drift.
  - Enable constrained decoding with lexicon constraints for critical terms.
- Data and fine‑tuning
  - Augment Swahili–English parallel data in legal, technical, culinary, and sports domains with bidirectional mappings.
  - Curate hard sets for: polarity/negation, subject–object swaps, gesture verbs, time expressions (saa nne/kumi), and ambiguous pronouns.
  - Add lexicons for herbs/ingredients, architecture, statistics, and protocols; include minimal‑pair contrasts (median vs average; export vs migration).
- Post‑processing
  - Automated checks: polarity (must/should), negation, numeric/time diffs, and party roles (client/consumer).
  - Gender/role QA: compare entities across source/back for pronoun and age/role consistency.
  - Terminology diff: enforce glossary exact matches; flag near‑synonym substitutions.
  - Residual‑language scan: detect Swahili tokens in English output (including signage/labels).
  - Domain validators: simple rule checks for marine/weather terms, musical notation, and legal headings.

## Confidence and Coverage
- Confidence: medium‑high. Dozens of low‑score cases across diverse judges consistently show the same families (polarity/role flips, term dilution, time/units, gender ambiguity, domain lexicon gaps).
- Coverage limits: Evidence skews toward narrative, legal/technical, culinary, and guidelines; fewer scientific/mathematical samples. Some zero‑score items lack rationales, so exact triggers there are inferred from similar cases.