# Failure Model — Grok 4 — Hindi

## Summary
Grok 4’s Hindi round‑trip translations are broadly faithful but repeatedly drift on polarity/direction, pronouns/gender, and domain‑specific precision. Failures cluster around (1) gender and perspective shifts due to Hindi’s neutral pronouns and free word order, (2) polarity reversals and path/preposition errors, (3) truncation/garbling at the end of texts, (4) systematic softening of modality and loss of technical/legal registers, and (5) near‑synonym swaps that break factual or terminological specificity. Poetic/idiomatic language is often flattened. Numbers and names are occasionally changed. The most consequential errors change meaning in satire, narrative relations, legal/technical constraints, and safety/time values.

## Top Failure Modes
- Polarity and direction reversals — The model flips “away from”/“towards,” “stay”/“stop,” “through”/“from,” inverting key claims or subtext; why: Hindi prepositions/postpositions and ellipsis encourage reinterpretation; frequency: common; severity: high.
  - Evidence: “journey away from productivity” became “towards productivity”; “You never stay” became “You never stop.”

- Gender, role, and perspective drift — Third‑person gender flips (her→his), role swaps (“her shoulder”→“his”), first/third‑person shifts inside quotes; why: Hindi’s gender‑neutral “vah,” dropped subjects, and coreference ambiguity; frequency: common; severity: high.
  - Evidence: Dialogue/stage directions swap who touches whose shoulder; a diary “I” rendered as “she,” and a “business partner” misassigned.

- Truncation and end‑sentence garbling — Final paragraphs/lines omitted or corrupted; sporadic foreign fragments; why: output length/stop‑sequence issues, detokenization glitches, or late decoding instability; frequency: occasional; severity: high.
  - Evidence: Closing paragraph missing with corrupted compensation line; dangling definition (“this commitment to …”); stray Chinese “告诉我”.

- Register and modality weakening in technical/legal texts — “MUST”→“should,” “taking(s)”→“seizure,” “contained”→“controlled”; why: preference for generic synonyms and lack of domain‑anchored constraints in Hindi; frequency: common; severity: high.
  - Evidence: RFC‑style MUST consistently softened; constitutional “takings” doctrine normalized to “seizure.”

- Terminology drift within semantic field — Near‑synonym but wrong specifics (“combustible”→“flammable,” “tallow”→“oil,” “hull”→“helm,” “knees”→“elbows,” tools swapped); why: lexical proximity bias and low penalty for fine‑grained distinctions; frequency: common; severity: medium.
  - Evidence: Safety label class changed; nautical hull misread as helm; anatomy “knees” replaced by “elbows.”

- Name and label instability — Proper names, model/variety names, and spellings altered; why: normalization/domestication bias and orthographic uncertainty; frequency: occasional; severity: medium.
  - Evidence: Elaine→Ellen; “Ember Queen”→“Amber Queen”; captain’s name altered.

- Poetic/idiomatic flattening and imagery shifts — Figurative language simplified, metaphors swapped; why: preference for literal clarity and avoidance of marked Hindi idioms; frequency: common; severity: medium.
  - Evidence: “kiln of will”→“furnace of desire”; “levee against harm”→“dam against loss”; “substantial deposit”→“compact.”

- Numerals, counts, and time errors — Minute counts and rarity rates changed; why: numeral normalization and inattentive transfer; frequency: occasional; severity: medium.
  - Evidence: “45 minutes”→“40 minutes”; “Uncommon (1–1000)” rephrased as “1 out of 1000.”

## Language‑Specific Factors
- Third‑person pronouns: Hindi “वह/वे” do not encode gender; back‑translation often guesses, causing her↔his flips and partner/actor role confusion.
- Free word order and dropped subjects/objects: Facilitates mis‑attachment of agents/patients in dialogue tags and stage directions.
- Postpositions/preposition mapping: “से/तक/की ओर/के पार” mappings trigger away/toward and through/from reversals.
- Light verb constructions: Nuance like “try to steal” (चोरी करने की कोशिश) collapses to “steal” (चोरी करना).
- Modality and obligation: Hindi forms for must/should (“अवश्य/ज़रूरी/चाहिए/करना होगा”) back‑translate inconsistently, weakening RFC/legal force.
- Idioms and register: Literary/archaic English terms (“wherein,” “affixed,” “metonymy”) lack stable high‑register Hindi equivalents; outputs normalize tone.

## Numbers, Units, and Formatting
- Numeral drift: Occasional 45↔40 minutes, and frequency scales reinterpreted; no unit conversion errors noted.
- Capitalized normative keywords: RFC MUST/SHOULD not preserved as marked tokens in Hindi; back‑translation often softens.
- Lists/bullets: No systemic table/markup breakage reported, but some list items and final lines get truncated.

## Meta/Disclaimer Behavior
- Minimal safety/policy intrusions. Rare tags noted, but evidence shows content‑level shifts (e.g., caution→warning) rather than explicit disclaimers. Triggered mostly in sports/legal domains as register substitutions, not policy text.

## Recommendations
- Prompting
  - Enforce polarity and modality: “Preserve direction (away/toward, through/from) and obligation (MUST/SHOULD). Do not soften modals.”
  - Role/gender anchoring: “Maintain original genders, speaker roles, and pronouns. If ambiguous in Hindi, insert gender‑neutral markers and align to source during back‑translation.”
  - Name/number lock: “Do not alter names, spellings, dates, or numbers.” Optionally echo a ‘Names and Numbers’ checklist after translation.
  - Register pinning: “Keep legal/technical register; prefer established domain terms; do not paraphrase specialized terms.”
- Decoding
  - Increase max output tokens and add end‑of‑document sentinel to reduce truncation; ban unknown‑language tokens; enable repetition penalty near document end to deter garbling.
  - Use constrained decoding for MUST/SHOULD and key terms via lexically constrained beams.
- Data and fine‑tuning
  - Augment with parallel hi–en corpora emphasizing: RFC/legal texts (MUST/SHOULD), safety labels (combustible vs flammable), nautical/anatomical/tool lexicons, satire/irony with directional cues.
  - Include contrastive pairs for polarity/preposition minimal pairs and for gendered role attribution in dialogue.
  - Train with style‑preserving literary pairs to reduce poetic flattening; add glossed idioms.
- Post‑processing and QA
  - Automatic diff checks on: polarity tokens (away/toward; through/from), modals (MUST/SHOULD), numerals, named entities, hazard classes.
  - Coreference pass: verify gender/role consistency across sentences; flag her↔his flips.
  - Terminology whitelist: enforce domain glossaries (“takings,” “contained,” “tallow,” hull/knee) with exception logs.
  - End‑of‑text integrity check: assert final paragraph presence; reject outputs containing foreign script fragments.
  - Style flags: detect excessive synonymy drift in legal/technical registers and require review.

## Confidence and Coverage
Moderate confidence: Dozens of low‑score rationales show consistent patterns across narrative, legal/technical, and poetic texts from multiple judges. Evidence is dense for polarity reversals, gender/role drift, modality softening, truncation, and near‑synonym errors; less density for formatting/markup and explicit meta disclaimers.