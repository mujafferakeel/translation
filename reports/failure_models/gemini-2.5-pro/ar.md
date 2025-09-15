# Failure Model — Gemini 2.5 Pro — Arabic

## Summary
Gemini 2.5 Pro’s Arabic translations preserve gist and structure but frequently dilute specificity and voice. The most salient weaknesses are: (1) systematic replacement of domain‑specific terminology with generic Arabic equivalents; (2) alteration of proper names, product names, and species/titles; (3) tone/register drift toward verbose or flattened phrasing, with idioms literalized; (4) instability around gender and inclusivity (they→he), and (5) occasional misreads of culturally/locally marked concepts (sports, legal, UI). Numbers are usually stable but time-of-day and label/button text can shift.

## Top Failure Modes
- Terminology generalization and mis-mapping — Replaces precise domain terms with generic ones (sports, tech, arts, finance), losing technical meaning.
  - Why: Sparse Arabic domain exposure; preference for high-frequency synonyms; low confidence in niche lexicons.
  - Frequency: Common; Severity: High
  - Evidence: “bowler→thrower; innings→turn; draw vs tie confusion” (cricket); “control planes→dashboards, standby replica→replica” (cloud infra).

- Proper nouns and named entities altered — Product, company, species, ranks, or place names are respelled, localized, or swapped with near-synonyms.
  - Why: Lack of capitalization cues in Arabic; aggressive normalization; training bias to translate rather than transliterate; noisy entity priors.
  - Frequency: Common; Severity: High
  - Evidence: “Citrus‑Based Cleaner→Acidic cleaner”; “Arcline→Arklayn”; “Catacombs→Crypts,” “Wight Lord→Ghost Lord.”

- Tone/register drift and verbosity — Formalizes or flattens energetic/poetic/professional voice; turns imperatives to gerunds; adds connective fillers (“And…”).
  - Why: Arabic stylistic priors favor explicitation; decoder favors fluency over style preservation; idiom avoidance.
  - Frequency: Common; Severity: Medium
  - Evidence: “punchy, idiomatic voice→literal, verbose”; legal and business texts “more conversational” with added words.

- Idiom and metaphor literalization — Figurative or culture-specific phrases rendered literally or with explanatory paraphrase, losing imagery.
  - Why: Low idiom alignment; risk-averse decoding; missing parallel idioms in training data.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “moonshot/runway metaphors missed”; “toast popped→toast jumps.”

- Gender and inclusivity shifts — Gender‑neutral “they” mapped to masculine; roles misgendered or gender added where absent.
  - Why: Arabic requires gender agreement; defaults to masculine without explicit guidance; corpus bias.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “they→he” in artist/biography content; added genders in instructions.

- Semantic “near miss” substitutions — Small but impactful swaps that alter facts or constraints (legal/tech/sports).
  - Why: Over-reliance on semantic similarity; insufficient term locking.
  - Frequency: Occasional; Severity: High
  - Evidence: “autofill→auto‑formatting”; “hashed password→encrypted”; “edge of the six→six-player group” (loses six‑yard box).

- Time, titles, and label drift — Greetings, times, law titles, and UI/button text wobble between close variants.
  - Why: Cultural normalization; lack of strict label preservation; ambiguity in source mapping.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “Good afternoon→Good evening; ‘tomorrow afternoon’→‘by noon tomorrow’”; “Checkout→Finalize Order.”

- Category/lexeme confusion in specialized nature/arts domains — Species, anatomy, and material culture terms swapped with nearby categories.
  - Why: Sparse Arabic taxonomic vocab; mapping via hypernyms.
  - Frequency: Occasional; Severity: Medium
  - Evidence: “Stag→Elk; fisher→hunter; dunlins/red knots→sandpipers/redshanks”; “females have antlers vs horns” inversion.

## Language‑Specific Factors
- Gender and agreement pressure: Arabic forces gendered pronouns and agreement; without explicit cues, model defaults to masculine or inserts gender.
- Capitalization absence: Arabic script lacks capitalization, increasing confusion between proper nouns and common nouns, prompting translation or Arabization (e.g., brand/product names).
- Register conventions: Arabic often prefers explicitation and nominal style; model leans into verbosity, softening concise English legal/technical style.
- Terminology availability: Limited standardized Arabic for certain domains (cricket, baking, restoration, DevOps) leads to generic substitutions or calques.
- Idiom mismatch: English idioms/metaphors without direct Arabic equivalents trigger literal renderings or toned-down paraphrases.
- Numbered sports/locale terms: Phrases like “edge of the six” rely on culture-specific shorthand unfamiliar in Arabic news style.

## Numbers, Units, and Formatting
- Generally stable numerics and metrics; however:
  - Time/greetings drift: afternoon/evening and noon/afternoon swaps.
  - UI/button labels and rating terms reworded rather than copied (“Outperform” rendered clunkily; “Checkout→Finalize Order”).
  - Security/CS terms altered: “hashed→encrypted,” “canonical→base,” risking technical correctness.
  - Sports shorthand misread: “edge of the six” decoded as participant count.
- No systematic unit conversion or invention observed; occasional tense/conditional shifts.

## Meta/Disclaimer Behavior
- Minimal policy/safety intrusions. The main meta behavior is stylistic inflation: added parentheticals, expansions of acronyms, and explanatory insertions in lists or guidance.
- Triggers: instructional/FAQ/legal sections where the model “clarifies” with parentheses or added conjunctions.

## Recommendations
- Prompting
  - Enforce term locking: “Do not change proper nouns, product names, UI labels, and button text; copy as is.” Provide a term list/glossary block.
  - Register guardrails: “Preserve tone and concision; no added explanations; no synonyms for technical terms.”
  - Gender policy: “Preserve gender‑neutral references; avoid inferring gender; use plural or periphrasis to maintain neutrality in Arabic.”
  - Idiom handling: “If an idiom has an accepted Arabic equivalent, use it; otherwise translate literally and keep metaphor.”
  - Sports/locale hinting: Include domain tag (e.g., “CRICKET TEXT”) to select the correct lexicon.

- Decoding
  - Lower temperature/top‑p to reduce creative synonymy.
  - Penalize paraphrase: enable constrained decoding for glossary items and UI strings.
  - Use copy-bias for capitalized spans and alphanumerics (NER‑aware decoding).

- Data and Fine‑tuning
  - Curate parallel Arabic corpora for underrepresented domains: cricket/soccer sportswriting, cloud/DevOps, baking/restoration, finance/equity research, theater/film terms.
  - Build domain glossaries (EN–AR) with preferred transliterations and standardized terms; fine‑tune with contrastive pairs penalizing generic substitutes.
  - Augment with gender‑neutral English → Arabic strategies (pluralization, role nouns avoiding gender).
  - Include style-aligned corpora to preserve poetic/marketing/legal register.

- Post‑processing
  - Named‑entity and label diff: detect and flag edits to proper nouns, product names, and UI strings.
  - Terminology QA: lint for known term pairs (hashed, autofill, control plane, bulk fermentation, six‑yard box).
  - Gender audit: detect inconsistent pronoun shifts; surface warnings.
  - Time/date checker: extract/align greetings, times, and schedules; flag noon/afternoon swaps.
  - Style checks: measure added tokens vs source to prevent verbosity in legal/tech outputs.

## Confidence and Coverage
- Confidence: Moderate. Evidence spans many samples and judges, with consistent patterns around terminology, names, tone, and gender. 
- Coverage limits: Fewer instances of numeric/unit distortion and no strong evidence of safety disclaimers; sports errors are cricket‑specific; some domains (medical, Qur’anic/classical) are underrepresented in the set.