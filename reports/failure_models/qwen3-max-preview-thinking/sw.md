# Failure Model — Qwen 3 Max Preview — Swahili

## Summary
Overall pattern: frequent meaning drift and substitution rather than surface fluency issues. The model often preserves structure but swaps core entities, roles, or mechanics; corrupts numbers/units; and occasionally collapses into repetitive gibberish. Most salient weaknesses: (1) catastrophic lexical substitutions/inversions of facts (fog→wind, lentils→kidney beans, noodles→skewers), (2) domain‑term confusion in legal/technical/spec/spec‑like text (modalities, actor roles, delimiters), (3) unstable numerals/units/times, and (4) repetition/glitch and truncation failures. Swahili‑specific pressures (gender‑neutral pronominal system, noun‑class polysemy, limited domain lexicons) amplify role/gender swaps, sports/legal polysemy, and culinary/poetic misreadings.

## Top Failure Modes
- Catastrophic semantic substitution/inversion — swaps a key concept with a near‑neighbor or opposite, altering meaning entirely; often in narrative or descriptive passages; why: reliance on distributional similarity + ambiguous Swahili lexemes; frequency: common; severity: high
  - Evidence: weather advisory flips “fog” to “wind” and sets wrong day/time; recipe changes “red lentils” to “red kidney beans.”

- Domain‑term misassignment (legal/technical/spec) — confuses actors, modalities, and terms (SHOULD→MUST, client vs consumer, delimiter types); why: overlapping Swahili mappings and calques; frequency: common; severity: high
  - Evidence: API spec: delimiter and actor roles wrong, requirement levels upgraded; legal clause flips “indirect damages excluded” and force‑majeure scope.

- Numbers/units/time corruption — feet↔meters, AM/PM swaps, price/duration errors, population digits drift; why: normalization/inference rather than faithful copy, decimal conventions; frequency: common; severity: high
  - Evidence: feet→meters in folklore piece; 80k citizens→30k; 2:45 p.m. rendered as a.m.; Fri–Sun sale becomes Fri–Sat.

- Role/agent confusion (people/jobs/sports) — “defender”→“lawyer,” therapist→lawyer, host vs visitor, gender/person swaps; why: Swahili polysemy (mtetezi/wakili), gender‑neutral pronouns (yeye) leading to back‑translation instability; frequency: common; severity: medium–high
  - Evidence: play‑by‑play calls a defender a lawyer and misstates actions; counseling startup flips therapist/lawyer and inverts “social anxiety.”

- Imagery/idiom literalization and poetic drift — key metaphors/images replaced or flattened (“glass city”→“frost city,” pigeons→flies), tone softened; why: low idiomatic parallel data and noun‑class agreement biasing to concrete senses; frequency: common; severity: medium–high
  - Evidence: poem swaps “glass/wakes” to “frost/sinks”; “panels”→“tools,” “whisper”→“gusting wind.”

- Omission/addition and repetition glitch — mid‑document collapse into repetitive fragments, section omissions; why: decoding instability and long‑context fatigue; frequency: occasional; severity: high
  - Evidence: public letter and game patch notes end in repeated phrases and drop final sections (compensation, known issues).

- Safety/operational instruction corruption — colors/objects/actions altered in evac/safety steps; why: aggressive semantic smoothing + term ambiguity; frequency: occasional; severity: high
  - Evidence: life‑vest red tabs→black; remove “flammables” instead of “sharps”; hiking “helmet”→“hat,” “microspikes”→“snowshoes.”

- Metadata/format drift and structure tampering — headers/titles/roles renamed, wrong delimiters, list/table fields altered; why: stylistic normalization; frequency: occasional; severity: medium
  - Evidence: “Proclamation”→“Notice,” “Magistrate”→“Mayor/Alderman,” wrong field separator in config.

- Hard failure: unrelated error message — outputs a system error string instead of translation; why: tool‑mode confusion; frequency: rare; severity: critical
  - Evidence: back‑translation returns an error message unrelated to source content.

## Language‑Specific Factors
- Gender neutrality and person reference: Swahili pronoun “yeye” is gender‑neutral; back‑translation to English frequently assigns the wrong gender or swaps roles.
- Polysemy/false friends in domains:
  - Defender: “mtetezi” overlaps with “wakili” (lawyer/advocate), causing sports→legal confusions.
  - Culinary staples: “dengu” (lentils) vs “maharagwe” (beans); “tambi” (noodles) vs skewers; sparse domain lexicon yields dish swaps.
- Noun class agreement and derivation: derivational prefixes encourage concrete senses over specialized metaphors, pushing poetic/technical imagery toward literal everyday nouns.
- Articles/aspect: No articles and different TAM system encourage invented specificity or time shifts when back‑translating; contributes to AM/PM and day shifts in narratives/advisories.
- Loanwords/orthography: Mixed acceptance of English loanwords (meters/feet, technical nouns) yields inconsistent unit handling and invented conversions.

## Numbers, Units, and Formatting
- Units: frequent feet↔meters “conversion” without instruction; invented SI substitutions; severity high when safety/measure‑critical.
- Numerals/dates: day/name shifts, AM/PM inversions, durations (Fri–Sun→Fri–Sat), and population/price digit drift (80k→30k).
- Modality/markup: MUST/SHOULD flips; delimiter tokens changed; titles/headers normalized; lists truncated.
- Inventions: adds durations, prices, or removes constraints; replaces exact colors and gear names.

## Meta/Disclaimer Behavior
- Rare emission of an unrelated error message instead of translation (one sample); appears when the system misdetects an error state or instruction boundary. Limited evidence of conventional “AI disclaimer” insertions otherwise.

## Recommendations
- Prompting
  - Use translation‑only system prompts with explicit constraints: “Translate faithfully. Do not add, omit, or convert units/dates. Preserve numbers, roles, and named entities verbatim.”
  - Provide a mini‑glossary per domain: defender=mchezaji wa ulinzi (sports), therapist=mtaalamu wa tiba, lentils=dengu, noodles=tambi, delimiter=kitenganishi, SHOULD=INAPASWA, MUST=LAZIMA, etc.
  - Wrap protected spans with tags and instruct to copy verbatim: <ENT>, <NUM>, <UNIT>, <ROLE>, <HEADER>.
  - Ask for a two‑pass output: translation then a short self‑check list (numbers, units, roles, time, modality) without changing text.

- Decoding
  - Lower temperature/top‑p for factual/safety/technical text to reduce substitutions; enable length penalty to discourage truncation and repetition; consider constrained decoding for tagged spans.
  - Use shorter chunking with overlap for long documents; stitch post‑hoc to avoid mid‑doc decay.

- Data and fine‑tuning
  - Curate Swahili‑English parallel corpora in:
    - Legal/technical/spec and config texts (modal verbs, delimiters, actor roles).
    - Safety/aviation/weather advisories (units, times, hazards).
    - Culinary and sports domains (stable term mappings: dengu/tambi/positions).
  - Augment with contrastive pairs for common confusions (fog vs wind; lentils vs kidney beans; defender vs lawyer; feet vs meters; AM vs PM).
  - Include poetic/prose corpora with aligned metaphors to reduce literalization.

- Post‑processing checks
  - Deterministic validators:
    - Number/unit parity: ensure all numerals and unit tokens match source; flag any unit conversion.
    - Time/date/day consistency: strict copy unless explicitly instructed; AM/PM guard.
    - Role/actor map consistency: compare against source term list; flag “lawyer/advocate” when “defender” appears in sports context.
    - Modality diff: detect SHOULD/MUST/May flips.
  - Terminology QA: apply glossary substitution pass for high‑risk terms.
  - Repetition/degeneration detector: if n‑gram repetition > threshold or abrupt truncation, auto‑retry with lower temperature or shorter chunk.

## Confidence and Coverage
Confidence: medium‑high for the identified top modes (broad, repeated across many judges and samples). Coverage limits: evidence is skewed toward difficult genres (poetry, specs, safety, weather) and low‑score cases; less visibility into high‑fidelity translations or casual dialogue. Some language‑specific attributions (e.g., exact lexical roots) are inferred from typical Swahili usage rather than the raw samples.