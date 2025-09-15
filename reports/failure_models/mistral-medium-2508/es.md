# Failure Model — Mistral Medium 3.1 — Spanish

## Summary
Overall, Mistral Medium 3.1 produces fluent Spanish but systematically drifts from source fidelity when terminology is domain‑specific, when proper nouns should be preserved, and when style or narrative stance matters. The most salient weaknesses: (1) improper handling of proper nouns and in‑universe/game terms, (2) meaning‑changing lexical substitutions for technical jargon across many domains, (3) unit/date normalization (feet→meters; “by”→“on”), (4) person/voice/tone shifts, and (5) occasional additions of formatting or translator notes. A few high‑impact misreadings (e.g., “villanelle”→“villancico,” action attribution swaps) also appear.

## Top Failure Modes
- Proper‑noun and universe term drift — Alters names of locations/skills/items/quests; replaces brand/feature names with Spanish equivalents; sometimes “localizes” what should be copied.
  - Why: Over‑generalization and lack of term preservation bias; Spanish training data may reward translating names; limited in‑domain lexicons (games, fantasy).
  - Frequency: Common; Severity: High
  - Evidence: “Dungeon/abilities/quest/creature names changed”; “Wight Lord→Specter Lord; Quiet→Silent Step; added ‘Level Up’ voice.”

- Domain jargon flattened or swapped — Substitutes precise technical terms with near‑synonyms that change meaning (tools, soil science, conservation, legal, firefighting, software).
  - Why: Preference for high‑probability synonyms over lower‑frequency domain terms; false friends/cognates; insufficient domain grounding.
  - Frequency: Common; Severity: High
  - Evidence: “sandy loam→sandy silt; clay loam→clayey silt”; “impact driver→impact drill; torque drivers→torque screwdrivers”; “tail latencies→queue latencies; multi‑tenant→multi‑user”; “inpainting→reintegration; satin→matte.”

- Meaning‑changing lexical mistranslations of concrete nouns/actions — Specific objects/actions are misread, altering factual content.
  - Why: Ambiguity resolution favors frequent senses; low attention to disambiguating context.
  - Frequency: Occasional; Severity: High
  - Evidence: “trowels→transplants (repeated)”; “parapet→water table”; “binnacle lamp→cabin lamp”; “bank→bench” (staging); “burglary→robbery.”

- Unit, measure, and date normalization — Converts imperial to metric or alters statistical terms; misreads temporal qualifiers.
  - Why: Learned normalization heuristics; lack of “preserve units” constraint.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “three feet→three meters; 50 ft→15 m”; “mileage→kilometers”; “median travel time→mean”; “‘by August 5’→‘on August 5.’”

- Voice, person, and tone drift — Shifts second‑person to third‑person; formalizes or flattens vivid diction; loses puns/alliteration; adds narration.
  - Why: Decoding prefers common register; Spanish paraphrastic tendencies; temperature leads to stylistic substitution.
  - Frequency: Common; Severity: Medium
  - Evidence: “you→their in donor section”; “poetic/evocative words replaced with generic (‘hissed’→‘honked’); taglines lose wordplay.”

- Action/agent attribution errors — Assigns actions to wrong character; flips states (slept vs. awake).
  - Why: Coreference confusion; long‑distance attention errors.
  - Frequency: Occasional; Severity: High
  - Evidence: “Photo taped by wrong character”; “guard who dozed off rendered as stayed up late.”

- Terminology inconsistency within document — Mixed choices across a text (“quarantine” vs “cuarentena”; “Ms.”→“Mrs.”; “quarters”→“coins”).
  - Why: Lack of document‑level term memory; no enforced glossary consistency.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “Multiple uses of ‘cuarentena’ for ‘quarantine’”; “Ms. consistently changed to Mrs.; quarters→coins.”

- Poetry/genre term misclassification — Key literary term misread with a false friend; device labels swapped.
  - Why: False friend (“villanelle”↔“villancico”); device taxonomy confusion.
  - Frequency: Occasional; Severity: High
  - Evidence: “‘villanelle’ rendered as ‘villancico’ throughout”; “enjambments vs. end rhymes mislabeled.”

- Additions of meta/formatting/translator notes — Inserts parenthetical Spanish terms, bracketed notes, bold/italics not in source.
  - Why: Instruction‑following bleed from general chat; over‑eager “helpfulness.”
  - Frequency: Occasional; Severity: Medium
  - Evidence: “Added parenthetical Spanish translations/notes”; “italics/bold added to key terms.”

- Fine‑grained taxonomy substitutions (species, gear, artifacts) — Swaps bird/fish species; hiking infrastructure; book parts.
  - Why: Low confidence mapping among near‑neighbors; limited domain lexicons.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “curlew↔whimbrel; red knot↔whimbrel; stone loach→bullhead”; “pastedown→flyleaf”; “bear boxes↔canisters.”

## Language‑Specific Factors
- Over‑translation of names and set terms: Spanish often naturalizes names/skills (e.g., “Quiet”→“Paso Silencioso”), but back‑translation requires exact preservation; the model frequently translates what should be copied.
- False friends and near cognates: villanela/villancico; “parapeto” is valid, but confusion with unrelated terms suggests sense drift; “autolisis/autolyse” toggling; legal and stage terms (“Dirección de Escena” vs. Stage Management).
- Register smoothing: Spanish output tends toward neutral/formal register, reducing slang/idiom vividness and puns; losses in alliteration/rhyme when translating slogans/poetry, then flattened back.
- Person and determiners: Spanish resolution of implicit subjects encourages person shifts when back‑translating (“tu/usted/uno” into third person), impacting direct address.

## Numbers, Units, and Formatting
- Units: Converts imperial→metric and mileage→kilometers absent instruction; occasional feet→meters errors (“3 ft→3 m”). Statistical term drift (median↔mean).
- Dates and temporal qualifiers: “by” dates rendered as “on”; time‑of‑day flips (afternoon↔evening).
- Formatting/markup: Adds italics/bold; inserts brackets/parentheses with clarifications; occasional code/term formatting bleed.

## Meta/Disclaimer Behavior
- Adds translator notes and Spanish glosses in parentheses or brackets; sporadic typographic emphasis. No safety/policy disclaimers were noted, but authorial intrusions (explanations, term notes) appear when texts are expository or instructional.

## Recommendations
- Prompting
  - Enforce preservation constraints: “Do not translate or change proper nouns, skill/item names, labels, or brand/product names. Do not convert units or dates. Do not add formatting or translator notes. Maintain person and tense.”
  - Provide a termbase/glossary inline (names, skills, species, technical terms) and instruct: “If not in glossary, copy as is.”
  - Style lock: “Match register, tone, and rhetorical devices; preserve puns/wordplay literally unless a Spanish equivalent exists—if none, mark [pun].”
- Decoding
  - Lower temperature/top‑p to reduce synonym churn; enable constrained decoding for protected terms with regex/NER spans.
  - Use span‑copy bias for capitalized sequences and measurements.
- Data/Training
  - Fine‑tune with high‑quality parallel corpora in: gaming/fantasy localization (with name preservation), soil/archaeology, conservation art, legal filings, hardware/DIY, wildfire/firefighting, hiking/outdoors, software/SRE.
  - Augment with unit/date non‑conversion examples and literary forms (villanelle vs. villancico) to de‑bias false friends.
- Post‑processing
  - Term consistency pass: glossary enforcement + QA to flag out‑of‑glossary substitutions and intra‑doc inconsistencies (“quarantine/cuaren…” “Ms./Mrs.”).
  - Unit/date checker: detect and flag converted or altered measurements/dates/statistics.
  - NER‑based name lock: ensure named entities/skills/items unchanged; flag deviations.
  - Domain linter packs: soil taxonomy, tool names, stage directions, bird/fish species.
  - Perspective/style checker: detect person shifts (2nd↔3rd), imperative/voice consistency; poetry/figure labels validator (enjambment, end rhyme).
- Workflow
  - Back‑translation spot checks for high‑risk domains; human review focus on proper nouns, measures, and specialized taxonomy.

## Confidence and Coverage
Moderate confidence. Evidence spans many judges and domains with repeated patterns (proper nouns drift, jargon substitution, unit/date changes, tone/person shifts, meta additions). Some phenomena (gender/number agreement, punctuation) were not flagged and may be underrepresented. The dataset emphasizes technical, literary, and game/fantasy content; patterns may differ in other genres.