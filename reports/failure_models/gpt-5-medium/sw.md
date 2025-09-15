# Failure Model — GPT-5 (medium reasoning) — Swahili

## Summary
Overall, the model delivers fluent Swahili but shows systematic fidelity drift when mapping back to English, particularly on attributes and specialized terms. The most salient weaknesses are: (1) time-of-day conversion and AM/PM handling, (2) gesture polarity flips (nod ↔ head‑shake), (3) gender/pronoun preservation failures caused by Swahili’s ungendered pronominal system, (4) technical/jargon dilution (e.g., product, legal, and engineering terms), and (5) occasional content-altering lexical swaps (e.g., ox/bull, sapphires/rubies). These issues are often high-severity because they alter facts or character attributes despite otherwise faithful prose.

## Top Failure Modes
- Time and schedule conversion errors — Swahili time-of-day words and AM/PM mappings are mishandled; times are shifted by hours or misclassified by period; why: confusion between 12/24‑hour formats and Swahili “asubuhi/jioni/usiku” labels, plus occasional inadvertent “Swahili time” (East African count from 6) interference; frequency: common; severity: high; evidence: “6:30 PM → 12:30 evening”; “10 AM–12 PM → 4–6 AM.”
- Gesture polarity inversion (nod vs head shake) — Affirmatives become negatives and vice versa; why: Swahili often uses “kutikisa kichwa” with context to indicate agreement or refusal, and the model back-resolves incorrectly; frequency: common; severity: high; evidence: “nod becomes head shake” in multiple stories; “child’s mayo/cheese nods inverted.”
- Gender/pronoun drift — Female characters become male or neutral on back-translation; why: Swahili third-person pronouns are ungendered (yeye/wao), so gender must be tracked via discourse; the model defaults or oscillates; frequency: common; severity: high; evidence: “Rin Vale misgendered ‘he/him’”; “her knife → his”; “Suri she → they.”
- Technical/jargon dilution and wrong term swaps — Precise domain terms are replaced with near-synonyms that change meaning or scope; why: limited domain term coverage in Swahili corpora and preference for high-frequency synonyms; frequency: common; severity: medium-high; evidence: “data export → data migration”; “Q3 earnings → revenue”; “artificial → artistic”; “docking → anchoring”; “runbook → guide.”
- Content-altering lexical substitutions — Salient nouns/adjectives replaced with neighbors that shift facts or symbolism; why: semantic neighborhood bias and poetic paraphrasing tendencies; frequency: occasional; severity: high; evidence: “ox → bull”; “sapphires → rubies”; “geese → ducks.”
- Idiom and register flattening — Idioms rendered literally or with softer tone; specialized registers (legal/technical/poetic) become generic; why: idiom gaps and style normalization during generation; frequency: common; severity: medium; evidence: “‘cut corners’ softened to procedural phrasing”; “marketing punch lines reworded, taglines altered.”
- Foreign token leakage/untranslated term — Rare instances of non-Swahili terms persisting; why: mixed-language source segments or copying; frequency: rare; severity: medium; evidence: “Chinese ‘提供’ left untranslated.”

## Language‑Specific Factors
- Ungendered pronouns and noun classes: Swahili lacks grammatical gender in third person (yeye/wao) and uses agreement classes, so gendered English references require explicit preservation. This drives recurrent misgendering on back-translation.
- Gesture semantics: “Kutikisa kichwa” (shake) can encode agreement or refusal by context; “kukubali kwa kutikisa kichwa” (nod) vs “kukataa kwa kutikisa kichwa” (shake) are easily conflated, causing affirmative/negative flips.
- Time expressions: Mapping English AM/PM to “asubuhi/mchana/jioni/usiku,” plus potential interference from colloquial “Swahili time” (saa moja ≈ 7 AM), increases risk of hour and period errors.
- Lexical gaps in specialized domains: Some technical, legal, and hobbyist registers have sparse, competing Swahili equivalents, encouraging genericization (e.g., runbook, craquelure, chossy).
- Poetic economy vs Swahili verbosity: Efforts to maintain fluency can expand concise imagery or shift figurative choices, altering symbolism (e.g., ox↔bull).

## Numbers, Units, and Formatting
- Times/dates: Frequent period/hours shifts; occasional timeframe drift (“this week” → “next week”). Conversions more error‑prone when prose uses period words (evening/noon) instead of digits.
- Quant/financial terms: Scope shifts (“earnings” ↔ “revenue”), severity: medium.
- Technical specs/tables: Term specificity softened (“nominal” → “typical,” “solids” → “particles”); otherwise formatting largely preserved.
- Rare inventions/insertions: Occasional added qualifiers in captions or taglines; severity low-medium.

## Meta/Disclaimer Behavior
- Minimal safety/policy intrusions overall. When present, they manifest as register shifts (e.g., “pending audit” → “pending review”) or mild explanatory additions. Triggered by formal/legal or procedural texts; severity low.

## Recommendations
- Prompting
  - Instruct strict preservation of times, numerals, names, genders, and polarity-sensitive actions. Example: “Do not change times/dates. Preserve character gender and gestures exactly (nod vs head shake).”
  - Ask for an ambiguity note pass: “If Swahili pronouns are gender-ambiguous, annotate gender from context and keep English gender on back-translation.”
  - Provide mini-glossaries for domain terms (e.g., export vs migration; legal carve‑out; mountaineering classes) and forbid substitution.
  - Require a post-translation checklist: times, gender, gestures, key nouns (animals/gems/tools), and idioms.
- Decoding
  - Use lower temperature/top‑p for high‑precision tasks; enable constrained decoding on glossary terms and time strings.
  - Preserve source numerals and AM/PM verbatim unless explicit conversion is requested.
- Data augmentation
  - Curate parallel Swahili–English corpora emphasizing: (a) AM/PM and time-of-day mapping; (b) gesture verbs and polarity contexts; (c) gendered character narratives; (d) domain‑specific lexicons (legal, software, maritime, climbing, oenology, archaeology).
  - Add contrastive pairs penalizing: nod↔shake, earnings↔revenue, export↔migration, ox↔bull, sapphire↔ruby.
- Fine-tuning targets
  - Attribute consistency objectives (gender, species, materials, gemstones).
  - Terminology adherence losses with hard constraints for tagged terms.
  - Style-preservation for idioms/poetry: retain figurative anchors unless instructed to localize.
- Post‑processing checks
  - Rule-based validators: detect flips (affirmative/negative gestures), time shifts (regex compare original vs output), and unit/financial term swaps.
  - Entity/attribute consistency checker over core slots: character gender, titles/roles, object types (knife owner, animal species, gem type).
  - Terminology linter driven by document-specific glossary; flag near‑synonym drift.
  - Optional human-in-the-loop for high‑risk categories (schedules, legal, technical release notes).

## Confidence and Coverage
Confidence: medium-high. The evidence set is dense and consistent across multiple judges with repeated patterns (time errors, nod/shake flips, gender drift, terminology dilution). Coverage limitations: fewer pure Swahili morphology errors reported, and limited visibility into long-form tables/markup; safety/meta issues appear rare in this sample.