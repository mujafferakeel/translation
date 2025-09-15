# Failure Model — GPT-5 (medium reasoning) — Spanish

## Summary
Overall, the model preserves meaning well but shows consistent “near‑synonym drift” that flattens style and sometimes distorts domain‑specific nuance. The most salient weaknesses are: (1) tone/voice erosion via systematic synonym and register shifts; (2) small but consequential terminology swaps in technical/legal/theatrical/gaming domains; (3) person, gender, and definiteness shifts driven by Spanish grammatical pressures; (4) ambiguous time and weather terms misresolved (evening/afternoon; weather/climate); and (5) mild structural/formatting normalization (headings, quotes, telegraphic style). Most errors are subtle yet accumulate, hurting fidelity in high‑precision or stylistically distinctive texts.

## Top Failure Modes
- Stylistic flattening via synonym drift — Replaces marked, idiomatic, or technical choices with plainer or adjacent synonyms, diluting voice or precision; why: decoding preference for frequent collocations and paraphrase bias; frequency: common; severity: medium-high.
  - Evidence: “backbone/funky/prickle” rendered with literal, safer terms; “erupted” softened to “went off,” poetic diction normalized.
  - Evidence: “Significance”→“Relevance” in research; “originator/standout contributor” paraphrased into weaker, verbose phrasing.

- Domain term substitution (near‑miss terminology) — Key terms replaced with close neighbors that change scope/meaning in specialized domains (legal, theater, gaming, tech, craft); why: insufficient domain anchoring and lack of glossary constraint; frequency: common; severity: high when domain-critical.
  - Evidence: Theater: “upstaging”→“masking”; “cue”→“track,” altering actionable notes.
  - Evidence: Legal: “Carve‑Out”→“Exception,” “relief”→“remedy,” modality swaps (“should”/“must”).
  - Evidence: Gaming: “Perk”→“Advantage,” named abilities altered; Tech: “tail latencies”→“queuing latencies,” “outputs”→“outcomes.”

- Person, gender, and definiteness shifts — Changes in grammatical person (2nd→3rd), gendered assumptions, or article specificity introduce tone and referential errors; why: Spanish enforces gender/number and drops pronouns, nudging the model to over‑specify; frequency: occasional; severity: medium.
  - Evidence: Direct address “your” becomes “their,” losing donor appeal; “you”→“they” reduces immediacy.
  - Evidence: Neutral “child”→“boy,” “kid”→“daughter,” neutral “it”→masculine “he.”

- Time and weather ambiguity misresolution — Ambiguous spans mapped inconsistently (evening↔afternoon; weather↔climate), changing timelines or semantics; why: Spanish category boundaries (tarde/noche) and false‑friends (clima/tiempo); frequency: occasional; severity: medium-high when procedural.
  - Evidence: “Evening” rendered as “afternoon” in status/tide updates; restoration “late evening”→“late afternoon.”
  - Evidence: “Weather” back‑translated as “climate,” changing topic.

- Cultural/lexical localization and untranslated remnants — Over‑localizes culturally bound terms or leaves bits untranslated, hurting fidelity or consistency; why: preference for domestication and copy noise; frequency: occasional; severity: medium.
  - Evidence: “elote” normalized to “corn on the cob”; “mija” softened; Spanish residues in names/quotes; “Ms.”→“Mrs.”

- Idioms, archaisms, and poetic diction mishandled — Archaic/marked expressions literalized or normalized; why: low exposure to rare forms and paraphrase bias; frequency: occasional; severity: medium.
  - Evidence: “arointing geese” paraphrased prosaically; “glean”→“obtain,” rhyme/imagery shifts (“burning bright”→“burning alive”).

- Modality and instruction strength drift — “Should/shall/must” swapped, softening or strengthening requirements; why: English modality mapping ambiguity to Spanish deber/debería/haber de; frequency: occasional; severity: medium.
  - Evidence: “Should consider”→“must consider”; “shall”→“will.”

- Scope/label/heading normalization — Section titles and labels adjusted, or telegraphic notes expanded into sentences; why: normalization bias; frequency: occasional; severity: low-medium.
  - Evidence: “Goals”→“Objectives”; concise notes expanded; search terms altered (“cycle track”→“bike lane”).

## Language‑Specific Factors
- Pronoun drop and gender inflection: Spanish forces gender/number agreement and often omits subject pronouns, leading to over‑specified genders (kid→daughter) or person shifts (your→their).
- Tarde/noche boundary: “Evening” vs “afternoon/night” is fuzzy; the model often chooses tarde, causing timeline drift.
- clima vs tiempo false friends: “Weather” can be misread as “climate,” especially in abstract or poetic contexts.
- Register choice (tú/usted/ustedes): Pressure to fix a formality level sometimes pushes 2nd→3rd person to avoid commitment, altering tone.
- Idiomatic density and archaism: Spanish tends to normalize archaic/idiomatic English (“arointing,” “nigh”), reducing markedness.
- Definite articles and nominal specificity: The tendency to add articles or specify nouns can over‑commit meaning (patio→backyard; entry→main entrance).

## Numbers, Units, and Formatting
- Numbers/units: Few numeric/unit errors reported; primary issues are label/category shifts rather than figures.
- Dates/times: Temporal label confusion (evening/afternoon) more than numeric dates.
- Headings/markup: Occasional renaming of headings (“Goals”→“Objectives”), and telegraphic/outline styles expanded into full sentences.
- Tables/metadata: No systematic table corruption observed.
- Conversions/inventions: No unit invention; occasional reclassification (“drive”→“unit”; “patio”→“backyard”) reduces precision.

## Meta/Disclaimer Behavior
- Minimal policy disclaimers; main meta issue is stylistic normalization of terse notes into standard prose and sporadic authorial intrusions (Spanish interjections).
- Triggers: Bullet‑point/telegraphic inputs, quotes, or when direct address might be sensitive (shifts to neutral/indirect forms).

## Recommendations
- Prompt patterns
  - Enforce fidelity: “Translate literally unless a direct equivalent is unidiomatic. Preserve person (2nd vs 3rd), modality (must/should/shall), tense, time expressions, headings, quotes, and named terms. Do not localize culture‑specific terms; keep them as source.” 
  - Provide termbase/glossary blocks per domain (theater: upstage, cue; legal: carve‑out, relief; gaming: perk/sigil names; tech: tail latency).
  - Specify register: “Maintain second-person direct address (tú/usted as specified: …). Avoid inferring gender when unspecified.”
  - Disambiguation hints: “Map ‘evening’→‘noche’ unless context specifies tarde. Map ‘weather’→‘tiempo’ (not ‘clima’) unless discussing long‑term climate.”
- Decoding changes
  - Lower temperature / increase guidance to reduce paraphrase drift.
  - Use constrained decoding/lexically constrained translation for glossary terms, headings, and named entities.
- Data augmentations
  - Parallel corpora emphasizing modality fidelity, person preservation, and time/meteorology distinctions.
  - Domain‑specific bitext for legal/theatrical/gaming/tech with strict term alignment.
  - Style‑sensitive literary pairs featuring archaisms and idioms to reduce normalization.
- Fine‑tuning targets
  - Penalize synonym substitution on protected spans (terms, cues, metrics, signage).
  - Train on prompts with explicit person/register constraints and evaluate with contrastive pairs (you vs they; should vs must).
- Post‑processing checks
  - Automated diff checks for: person (2nd→3rd), modality lexemes, time‑of‑day tokens, weather/climate terms, section headings, quoted text.
  - Terminology QA: glossary match rate; flag near‑synonyms for reviewer approval.
  - Named entities and measurements consistency check; cultural term preservation (elote, mija) policy enforcement.

## Confidence and Coverage
- Confidence: Moderate. The dataset shows many medium‑score cases with consistent synonym/tone drift and repeated domain term swaps; fewer hard errors or numeric issues.
- Coverage limits: Evidence skews toward literary, legal, tech, and instruction texts; fewer highly technical scientific/math tables. Multiple judges agree on patterns, but regional Spanish variation and genre‑specific edge cases may reveal additional issues not captured here.