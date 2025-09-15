# Failure Model — DeepSeek V3.1 Reasoner — Spanish

## Summary
DeepSeek V3.1 Reasoner shows strong meaning preservation but systematic drift in precision and voice when handling Spanish. The most salient weaknesses are: (1) semantic precision loss in domain terms (legal, safety, technical), often due to Spanish–English false friends; (2) gender/pronoun instability, especially neutral “they” and role/possessor flips; (3) tone flattening and register shifts that overformalize colloquial/poetic text; (4) stray language artifacts (untranslated Spanish phrases, mixed formats); and (5) sporadic factual inversions on small but critical details (negations, timelines, named items).

## Top Failure Modes
- Term-of-art erosion (legal/safety/technical) — Spanish terms back-translated with near-synonyms that change meaning (“robo” for burglary; “inflamable” vs. “combustible”; “pan traps” → “pitfall traps”; “brushless” → “wireless”). Why: Spanish polysemy and false friends; preference for higher-frequency synonyms; weak domain glossaries. Frequency: common. Severity: high. Evidence: doc_0108 burglary→robbery; doc_0165 combustible→flammable; doc_0020 pan traps→pitfall traps.

- Pronoun and gender drift — Neutral “they/their” made gendered; his↔her swaps; role possessors misassigned. Why: Spanish gendered morphology and “su” ambiguity; decoder resolves to a specific gender; co-reference not strictly enforced. Frequency: common. Severity: high. Evidence: doc_0189 they→she; doc_0026 his→her implant; doc_0003 “her shadow”→“his”.

- Tone/voice flattening — Hardboiled, poetic, or colloquial registers rendered more clinical/formal; figurative verbs and onomatopoeia normalized; headings/taglines simplified. Why: safety/clarity bias toward literalness; Spanish paraphrasing to frequent synonyms; loss of style constraints in round-trip. Frequency: common. Severity: medium. Evidence: doc_0009 leaner→fadeaway with onomatopoeia diluted; doc_0098 “unclench”→“open”; doc_0102 archaic legal “Whereas” modernized.

- Critical detail flips and micro-facts — Negation and timeline reversals; instruction buttons/labels changed; hazard classes and species swapped. Why: local rephrasing, attention slip on modifiers, normalization to defaults. Frequency: occasional. Severity: high. Evidence: doc_0169 “no fan spin”→“fan does spin”; doc_0124 late evening→late afternoon; doc_0173 “Checkout”→“Pay”.

- Named entity and proper-noun mutation — Game abilities/quests, species, product teams, button text, project names altered to generic or different names. Why: overtranslation of capitalized tokens; lack of copy constraints; domain OOD. Frequency: occasional. Severity: medium. Evidence: doc_0192 multiple ability/quest names changed; doc_0149 dunlins/red knots→sandpipers/knots.

- Residual Spanish and insertion/omission artifacts — Anchor phrases or short segments left in Spanish; stray “por ciento”; mixed Spanish units/dates; occasional added Spanish phrase. Why: language-ID slip at boundaries; inconsistent DNT handling; format copying. Frequency: occasional. Severity: medium. Evidence: doc_0063 anchor phrase left in Spanish; doc_0108 “horas”, “pulgadas”; doc_0146 “por ciento”.

- Units, dates, and rarity scales normalization — Feet→meters conversions; US date/time normalization; rarity labels changed (“Uncommon (1–1000)”→“1 in 1000”). Why: automatic normalization heuristics; lack of “preserve-as-is” constraint. Frequency: occasional. Severity: medium. Evidence: doc_0089 feet→meters; doc_0044 rarity phrasing change.

## Language‑Specific Factors
- Gender and “su” ambiguity — Spanish forces gendered or ambiguous possessives; back-translation can hallucinate or flip gender/owner.
- False friends and near-cognates — cámara/chamber vs. camera; robo/robbery vs. burglary; combustible/inflamable; sauce/salsa; cordless/brushless vs. inalámbrico. These drive domain errors.
- Register mapping — Spanish often favors formal register; colloquial English (slang, onomatopoeia) tends to be neutralized when passing through Spanish.
- Legal/technical nomenclature — Spanish legal distinctions (allanamiento, hurto, robo) and safety labels require precise mappings; the model defaults to high-frequency terms.
- Species and technical jargon — Spanish common names vary regionally; back to English drifts to generic categories (e.g., shorebirds) or close-but-wrong hardware terms.

## Numbers, Units, and Formatting
- Unwanted unit conversion (imperial↔metric) and date/time localization; mixed-language units/dates in back-translation.
- Scale/rarity phrases normalized or reinterpreted.
- Percent formatting leakage (“por ciento” carried through).
- Table/label/button strings paraphrased instead of copied.
- Inventions rare, but occur in specialized gaming/mechanics distances and labels.

## Meta/Disclaimer Behavior
- Minimal safety disclaimers. However, mild authorial intrusions appear as explanatory notes or translated headings that add commentary (e.g., UX heading with an explanation).
- Triggered when headings look like guidance/summary or when the source contains clarifying parentheticals.

## Recommendations
- Prompt patterns
  - Enforce copy constraints: “Do not alter named entities, UI strings, abilities, species, or button text. Preserve units, dates, and capitalization exactly.”
  - Domain glossaries per batch: provide small bilingual tables for legal (burglary/allanamiento), safety (combustible/inflammable), tools (brushless/sin escobillas), methods (pan traps/trampas de bandeja).
  - Style locks: “Preserve figurative language, slang, onomatopoeia, and register. Avoid formalization unless present.”
  - Negation/quantifier guard: “Pay special attention to negations, timelines, and quantities; do not invert.”

- Decoding changes
  - Lower temperature/top-p to reduce synonym drift on constrained segments.
  - Use constrained decoding for glossary/DNT spans (copy mode for UI strings, product names, ability names).
  - Segment-level tagging: wrap critical spans with markers to bias faithful copying.

- Data augmentation
  - Parallel corpora emphasizing: legal charge taxonomy (robo/hurto/allanamiento), safety labels (NFPA/OSHA), ecology species names, gaming/MMO glossaries, tools/hardware (cordless vs brushless).
  - Style-preservation fine-tuning with bilingual literary and colloquial datasets to resist formalization.
  - Gender/neutrality sets: English they/their ↔ Spanish strategies and back-translation without gender hallucination; possessive disambiguation cases.

- Fine-tuning targets
  - Error-aware contrastive pairs for top confusions (burglary/robbery; combustible/flammable; chamber/camera; sauce/salsa).
  - Register-sensitive loss: penalize replacements of figurative/onomatopoetic tokens with plain synonyms.
  - Structured string protection: UI/button labels, headings, measurements as labeled entities.

- Post-processing checks
  - Term QA: automatic diff against provided glossary; block outputs with mismatches.
  - Negation and polarity checker for critical statements (“no/not/without” preserved).
  - Units/dates validator: forbid unit conversions; regex to flag metricization and “por ciento” leakage.
  - Named-entity lock: detect mutations in capitalized tokens and known lists (quests, abilities, product teams).
  - Gender/pronoun audit: flag shifts from neutral to gendered or his↔her swaps; require confirmation or neutral rephrase.
  - Domain lint for legal/safety: map outputs to allowed label sets; reject out-of-set synonyms.

## Confidence and Coverage
Confidence: medium-high. Evidence spans ~100+ low-to-mid scoring cases across diverse domains (legal, safety, technical, literary, gaming), with multiple independent judges agreeing on recurring patterns (term-of-art drift, pronoun/gender instability, tone flattening, format/unit artifacts). Coverage limits: fewer examples of tables/markup-heavy inputs and limited highly technical STEM equations; minimal evidence of strong safety disclaimers.