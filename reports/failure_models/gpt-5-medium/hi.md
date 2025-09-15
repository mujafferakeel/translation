# Failure Model — GPT-5 (medium reasoning) — Hindi

## Summary
GPT-5 (medium reasoning) produces generally faithful Hindi↔English round‑trips but shows systematic drift in specificity, register, and small facts. Most low scores stem from consistent synonym substitution that softens legal/technical force, deadline/time misinterpretations (notably “noon”), small but consequential lexical swaps (e.g., “combustible”→“flammable,” “ox”→“bull,” “lime”→“lemon”), modality weakening (“must”→“should,” exact %→“up to”), and occasional gender/number errors. It also sometimes localizes or transliterates items that should remain in the source script and adds rare meta/translator notes. The most salient weaknesses: modality/metric weakening, time/deadline errors, factual noun swaps, gender/number shifts, and over‑localization of examples/titles.

## Top Failure Modes
- Modality and metric hedging — strong requirements and exact numbers get softened; why: preference for natural‑sounding Hindi with polite/hedged modality and generic marketing/legal phrasing; frequency: common; severity: high
  - Evidence: exact “18% reduction” became “up to 18%”; MUST/SHOULD semantics blurred (“should” replaces “must”); “bag may not inflate” turned into categorical “will not inflate.”

- Time/deadline misinterpretation (“noon”) — “Friday at noon” consistently rendered/understood as “afternoon,” shifting constraints; why: Hindi term “dopahar/dupehar” is ambiguous and model biases toward broader “afternoon”; frequency: common; severity: high
  - Evidence: multiple judges note repeated “noon→afternoon” shifts across dialogue and school policy texts.

- Small factual noun/attribute swaps — near‑synonyms or related terms replace precise referents (tools, animals, materials, weather), altering meaning; why: synonym preference and low penalty for near neighbors in decoding; frequency: common; severity: medium
  - Evidence: “ox→bull,” “sleet→hail,” “firm snow→hard ice,” “trowels→hoes,” “shears→pruners,” “combustible→flammable,” “Blue Line→bus.”

- Gender/number agreement drift — pronouns and countability flip, changing perspective or scope; why: Hindi’s gendered agreement and pronoun drop encourage inference; back‑translation reifies wrong gender/number; frequency: occasional; severity: medium
  - Evidence: “her shadow→his,” “their→his”; “old hands (plural)→old master (singular).”

- Over‑localization and script handling — items intended to remain in English are translated or transliterated to Devanagari; why: model tries to adapt titles/examples to target audience; frequency: occasional; severity: medium
  - Evidence: English example titles output as Hindi titles or in Devanagari; a name shown in Hindi script; technical title list localized.

- Tone/register flattening in legal/poetic texts — evocative/technical diction replaced with blander Hindi synonyms; why: style smoothing during decoding and parallel data skew toward neutral register; frequency: common; severity: medium
  - Evidence: legal force softened (“contend→believe,” “ruling→order”); poetry imagery dulled (“ghost→shadow,” “dusted→smeared”).

- Polarity and factual tweaks within sentences — minor but impactful shifts to antonyms or different loci; why: semantic neighborhood confusion and Hindi paraphrase norms; frequency: occasional; severity: medium to high when safety‑critical
  - Evidence: “move”→“remove” (changing constraint), “midday→afternoon,” “breeze→still air,” “patio→yard.”

- Rare meta/disclaimer intrusions — translator notes or extra commentary appear; why: safety/assistant persona leakage; frequency: rare; severity: medium when contaminating content
  - Evidence: one case added a translator note; dialog line intensified (“Some things aren’t”→“Nothing”).

## Language‑Specific Factors
- Noon vs afternoon: “noon” lacks a crisp, consistently used Hindi equivalent; “dopahar/dupehar” often spans noon→afternoon, triggering systematic shifts in deadlines and time references.
- Gender agreement and pronoun drop: Hindi often omits pronouns and enforces gender on verbs/adjectives; the model infers gender (his/her) and can flip perspective during back‑translation.
- Number and countability: Singular/plural alternations in Hindi can be neutralized in surface form; re‑Englishing then invents number (“old hands”→singular).
- Modality mapping: English RFC/legal modals (MUST/SHOULD/MAY) lack widely standardized Hindi equivalents; typical renderings (“अनिवार्य,” “अत्यावश्यक,” “चाहिए,” “सकता है”) are variably strong, leading to hedging.
- Lexical specificity gaps: Hindi general terms for tools/materials (e.g., “कैंची,” “औज़ार,” “लोहे की कतरनी”) encourage generalization; literary metaphors tend to be normalized to common nouns (“भूत”→“परछाई”).

## Numbers, Units, and Formatting
- Exact→range hedging: exact percentages or counts become “up to” or approximate ranges (“by 30%→up to 30%”; “18%→up to 18%”).
- Temporal precision loss: “Friday at noon”→“Friday afternoon”; “midday”→“afternoon.”
- Hazard/engineering term drift: “combustible” vs “flammable”; “damping→dimming.”
- Transport/infra labeling: specific line vs mode confusion (“Metro Blue Line→bus”).
- Script choices for named items: English titles/names transliterated into Devanagari or translated rather than preserved verbatim.
- Inventions are rare; most issues are precision/label drift, not fabricated numbers.

## Meta/Disclaimer Behavior
- Occasional translator note or explanatory aside appears when the model “helps” or clarifies; tends to trigger in literary/dialogue passages or when the model senses ambiguity.
  - Evidence: added translator note in a dialogue sample; intensified line “Some things aren’t”→“Nothing” alongside meta tone.

## Recommendations
- Prompting
  - Enforce fidelity constraints: “Translate exactly. Do not add notes. Preserve all numbers, units, dates, times, names, and example titles verbatim; do not localize or transliterate unless explicitly requested.”
  - Modal glossary injection: Provide a mapping table for MUST/SHOULD/MAY and require consistent use; e.g., “MUST=अनिवार्य/अवश्य,” “SHOULD=उचित/चाहिए,” “MAY=हो सकता है/वैकल्पिक.”
  - Time disambiguation: Explicitly specify noon mapping: “noon=दोपहर 12 बजे (12:00).”
  - Entity/script policy: “Keep English titles/names in Latin script unless otherwise stated.”
  - Style lock: “Maintain the same tone and register (legalistic/technical/poetic). Prefer literal equivalents over interpretive synonyms.”
- Decoding
  - Lower temperature or use constrained decoding for legal/technical domains to reduce synonym drift.
  - Use lexically constrained decoding or vocabulary biasing for domain terms (RFC modals, hazards, tools).
- Data and fine‑tuning
  - Fine‑tune on Hindi↔English parallel corpora with strong supervision for modality, metrics, and safety instructions.
  - Curate pairs emphasizing time expressions (noon vs afternoon), hazard labels (combustible vs flammable), and precise tool/material vocab.
  - Include literary parallel data preserving imagery to reduce poetic flattening.
  - Add do‑not‑localize training examples for titles, product names, standards keywords.
- Post‑processing and QA
  - Automated checks: diff numbers/percentages, dates/times, polarity (negations), and modality keywords between source and back‑translation.
  - Terminology QA: enforce glossary for legal/RFC terms, hazards, tools.
  - Script policy validator: flag non‑Latin where Latin expected; flag title/example localization.
  - Gender/number audit: heuristic check for pronoun/gender shifts when source constrains them.
  - Time normalization: normalize “noon” to “12:00” and back‑check.

## Confidence and Coverage
- Confidence: medium‑high. Multiple judges across many samples highlight consistent patterns (modality weakening, noon misread, factual noun swaps, gender/number drift, over‑localization).
- Coverage limits: Evidence skews toward small‑to‑moderate deviations rather than catastrophic failures; fewer samples on tables/units or domain‑specific numerics, and rare meta/disclaimer cases.