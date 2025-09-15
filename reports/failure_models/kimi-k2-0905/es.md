# Failure Model — Kimi K2-0905 — Spanish

## Summary
Overall, Kimi K2-0905 produces fluent Spanish but frequently drifts from source semantics through near‑synonym substitution, softening/hardening of modality, and unnecessary localization. The most salient weaknesses are: (1) mutation of fixed terms and proper names (gaming, technical, legal, safety), (2) pronoun/gender handling errors and actor/agency swaps, (3) time/number distortions and unit localizations, (4) tone/style flattening in literary passages and odd handling of onomatopoeia/rhyme, and (5) false‑friend choices in everyday domains (culinary, business).

## Top Failure Modes
- Terminology drift and false friends (technical/business/culinary) — Replaces precise terms with near synonyms or false friends, reducing specificity or altering meaning; likely due to preference for “more natural” Spanish and insufficient domain anchoring.
  - Why: Lack of domain‑locked lexicons; semantic similarity over precision in decoding.
  - Frequency: common; Severity: high
  - Evidence: “Endpoint” rendered as “punto final”; “competence” became “competición”; “earnings” to “revenue”; “sauce projects” to “salsa projects.”

- Proper noun and system term mutation (games, plans, product names) — Changes the names of abilities/quests/locations and plan names, preserving gist but breaking factual fidelity.
  - Why: Aggressive paraphrase plus lack of copy‑through bias for capitalized tokens.
  - Frequency: common; Severity: high
  - Evidence: Multiple RPG terms renamed (“Wight Lord”→“Lord of the Undead”, ability/quest/location names altered); plan title changed and verbs like “lift”→“push.”

- Pronoun/gender shifts and actor misattribution — Swaps neutral to masculine, feminine to neuter, or assigns actions to the wrong character.
  - Why: Spanish gender constraints + heuristic disambiguation; weak coreference tracking during paraphrase.
  - Frequency: common; Severity: high
  - Evidence: Neutral “they” became “he”; “she” turned into “it” in a poem; the taping action moved from one character to another.

- Time, numbers, and formation distortions — Alters times, schedules, or numeric formations; occasionally localizes units unasked.
  - Why: Over-normalization and hallucinated “clarity” edits; number token confusion.
  - Frequency: occasional; Severity: high
  - Evidence: Evening→afternoon in several notices; “4‑4‑2” soccer formation became “4‑4‑4”; miles turned to kilometers.

- Tone/modality hardening or softening — “Should” becomes “must,” imperatives softened, noir/evocative style flattened; inclusive/neutral phrasing shifted to more prescriptive or generic registers.
  - Why: Preference for standard formal Spanish; loss of stylistic features in beam search.
  - Frequency: common; Severity: medium
  - Evidence: Success metric changed from “should increase” to “must increase”; imperative coaching softened to declarative; poetic diction replaced by generic synonyms.

- Domain terminology mis-mapping (sports, outdoors, safety) — Tactical/sports and outdoors/climbing jargon altered or generalized; safety instructions nudged to different concepts.
  - Why: Sparse domain data or interference from similar terms.
  - Frequency: occasional; Severity: medium
  - Evidence: “4‑4‑2 block”→“4‑4‑4”; climbing “crest/ridge” conflated with “summit”; “line of sight” rephrased as “eye contact.”

- Localization overreach in UI/policy/legal — Translates labels/policy language into different intents; adds cultural localization not requested.
  - Why: Goal of “helpful” localization outweighs fidelity constraints.
  - Frequency: occasional; Severity: medium
  - Evidence: “Back to sign in” became “Log in again”; policy narrowed from “any sexual content” to “unwanted sexual content.”

- Literary devices and sound symbolism loss — Rhyme/metrical choices reworked, onomatopoeia invented or odd; imagery shifted.
  - Why: Attempts to preserve poetic effect via free adaptation; lack of constraint to literal fidelity in round‑trip tasks.
  - Frequency: occasional; Severity: medium
  - Evidence: Rhyme rephrased; “pío‑pío” added in sports play‑by‑play; “wreath” repeatedly rendered as “crown,” changing symbolic load.

- Minor omissions/additions that tilt nuance — Drops small qualifiers or injects metaphors while preserving structure.
  - Why: Summarizing tendency; stylistic smoothing.
  - Frequency: occasional; Severity: low
  - Evidence: Omitted qualifiers like “long”; added metaphors (“meteor”) not in source.

## Language‑Specific Factors
- Gender and inclusivity: Spanish forces gender where English can stay neutral. The model often defaults to masculine or neuter, or assigns gender ad hoc. Better strategies (e.g., repeating nouns: “la persona/estudiante,” pluralization, second‑person) are inconsistently applied.
- Modality/register: English “should/may” vs Spanish “debería/debe/puede” often shift to stronger forms; maintaining soft advice in Spanish requires deliberate choices the model misses.
- Lexical false friends: “salsa/sauce,” “competencia/competición” vs “competence,” “combustible/inflamable” risk domain errors; Spanish culinary/technical lexemes have overlapping forms the model confuses.
- Sports/outdoors terminology: Iberian/LatAm variants and anglicisms complicate soccer and climbing jargon; fixed formations and ridge/crest/summit distinctions get blurred.
- Poetic/onomatopoeic rendering: Spanish phonotactics tempt creative substitutions that break round‑trip fidelity.

## Numbers, Units, and Formatting
- Numbers/formations: Occasional invention or alteration (e.g., “4‑4‑2”→“4‑4‑4”).
- Time/date shifts: Evening/afternoon frequently swapped; schedule windows diluted.
- Units: Unrequested conversions (miles→kilometers).
- UI labels/headings: Button text and headers paraphrased, changing intent.
- Conversions/inventions: Mostly substitutions, not systematic numeric inventions, but impactful when they occur.

## Meta/Disclaimer Behavior
- Minimal explicit safety/policy insertions overall. However, policy and safety notes can be semantically reshaped (e.g., inclusive bans narrowed; safety “spotting” omitted).
- Triggered by policy‑like or instructional content where the model “clarifies” rather than mirrors.

## Recommendations
- Prompt patterns
  - Enforce term locking: “Do not alter proper nouns, item/ability names, formations, units, or UI labels; copy them verbatim.”
  - Glossary injection: Provide a per‑document glossary (RPG terms, climbing jargon, UI strings) with “must remain exactly as listed.”
  - Fidelity guardrails: “No additions, no omissions, no localization or unit changes; preserve times and numbers exactly.”
  - Inclusivity guidance: Specify pronoun strategy for singular they in Spanish (repeat role noun, avoid gendering unless explicit).
  - Modality preservation: “Preserve modality and register (should/may vs must).”
- Decoding changes
  - Lower temperature/top‑p to reduce paraphrase; consider constrained decoding for capitalized tokens and numerals.
  - Bias copy-through on entities/numerals via lexically constrained decoding.
- Data augmentation
  - Fine‑tune on parallel corpora with: gaming/system UIs, sports tactics, climbing/outdoors, safety/policy/legal, and business/product docs emphasizing modality and deadlines.
  - Hard negatives: pairs penalizing “salsa/sauce,” “competence/competition,” “combustible/flammable” swaps; soccer formations; evening/afternoon contrasts.
  - Inclusive Spanish patterns for neutral referents across registers.
- Fine‑tuning targets
  - Named entity and terminology preservation objective (contrastive loss when names drift).
  - Modality fidelity objective (penalize should→must shifts).
  - Numeric/time consistency objective.
- Post‑processing checks
  - Automatic diff on: proper nouns, numerals, times/dates, units, formations.
  - Terminology QA: glossary enforcement; domain spell‑checks (sports/climbing/cooking/safety).
  - Gender/pronoun audit: flag gendered forms when source is neutral; suggest noun‑based neutral rewrites.
  - UI/policy string validator: ensure label text and policy scope unchanged.

## Confidence and Coverage
- Confidence: medium‑high. Dozens of low‑score rationales consistently point to term drift, pronoun/agency shifts, and time/number errors, with multiple Kimi‑specific instances.
- Coverage limits: Evidence spans varied domains but is skewed toward short passages and mixed‑model samples; poetry/literary and tech/business are well represented, while highly specialized legal/medical corpora are lighter.