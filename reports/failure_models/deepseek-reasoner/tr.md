# Failure Model — DeepSeek V3.1 Reasoner — Turkish

## Summary
Overall, the model is fluent but prone to meaning‑changing substitutions and over‑localization when translating to/from Turkish. The most salient weaknesses are: (1) aggressive (and often wrong) unit/number “conversions” and locale swaps (911→112, ft²→m²); (2) corruption of domain terminology and acronyms (MRR/ARR/CAC/SQL, legal/security modals) and systematic synonym swaps (“checkout”→“payment”); (3) direction/action reversals and factual flips (nod↔shake, onshore↔offshore, house‑left↔stage‑left); (4) proper‑noun and title normalization (street/festival names, ranks) and partial translation of names/titles; (5) occasional omissions/additions or untranslated fragments (slogans/captions), plus tone flattening.

## Top Failure Modes
- Over‑localization of units, numbers, and locale‑specific entities — The model “helpfully” converts or substitutes units and emergency numbers, often incorrectly, and sometimes invents details.
  - Why: Heuristic/localization bias; training on localized corpora; lack of hard constraints to preserve source units/numbers.
  - Frequency: common; Severity: high
  - Evidence: “sq ft became sq m and ‘waterfront’ was added”; “911 changed to 112; hours became days; sols→days.”

- Domain acronym/term drift (business/tech/legal/security) — Key acronyms and terms are replaced by lookalikes or looser synonyms, altering metrics/requirements.
  - Why: Token‑level confusion with similar strings; preference for Turkish generic equivalents; lack of protected‑term handling.
  - Frequency: common; Severity: high
  - Evidence: “MRR→MGR, ARR↔ACV, SQL→MQL, CAC→CPM; VP Sales→CRO”; “MUST weakened to SHOULD; normalization misinterpreted.”

- Direction/action and antonymic reversals — Opposite or conflicting actions/directions/engineering states are produced.
  - Why: Ambiguity in Turkish aspect/negation; interference from idiomatic paraphrasing; weak attention to polarity/directionality tokens.
  - Frequency: occasional; Severity: high
  - Evidence: “nodded became shook her head”; “house left became stage left”; “onshore winds→offshore”; “deburr→raise a burr.”

- Proper‑noun and title normalization/translation — Named entities get localized or paraphrased; titles/ranks/classes altered.
  - Why: Over‑translation of names; absence of NER lock; exposure to localized mappings.
  - Frequency: occasional; Severity: medium–high
  - Evidence: “Elm St.→Çınar; Riverfront→Riverside”; “Magistrate→Mayor; Captain for Lt. Commander”; “Adept→Apprentice.”

- Additions, omissions, and untranslated fragments — Inserts genre‑specific guesses, drops details, or leaves source phrases unrendered.
  - Why: Hallucination under uncertainty; compression for fluency; failure to translate embedded text/signage.
  - Frequency: occasional; Severity: medium
  - Evidence: “added ‘iambic pentameter’”; “omitted ‘nephews’/‘publicly traded’”; “Turkish slogans/caption left untranslated.”

- Terminology precision loss and technical substitutions — Specific technical nouns/verbs swapped for near‑synonyms that change meaning.
  - Why: Preference for more common Turkish lexemes; sparse parallel exposure for niche terms; decoder pushing fluency over precision.
  - Frequency: common; Severity: medium–high
  - Evidence: “cable‑stayed→suspension; runbook→workbook; container‑free→shell‑less”; “slush fund mistranslated; port↔starboard.”

- Modality/register and tone shifts — Requirements weakened and style flattened; legal/security statements softened.
  - Why: Turkish modality mapping (zorunda/gerekir/şart) ambiguity; politeness bias.
  - Frequency: occasional; Severity: medium
  - Evidence: “MUST→SHOULD; safety advice reversed (‘don’t fixate’→‘do not take your eye off’).”

## Language‑Specific Factors
- Zero grammatical gender and pronoun economy — Turkish lacks gendered pronouns; back‑translation guesses can flip genders or attributions (“Suri” male; possession of documents misassigned).
- Case and possessive markers — Ambiguity with -ın/-in and pronoun drops can swap who‑owns‑what (evidence owner reversed).
- Directionality lexicon — “Sahne solu/evin solu” and nautical/technical direction terms (iskele/sancak; kıyıya/onshore) are easy to invert if not constrained.
- Agglutinative morphology and compounding — Domain terms are re‑morphed into generic paraphrases; compounds get semantic drift (“climate array”→“alignment”).
- Idiom and meter — English idioms/rhyme often rendered literally or flattened in Turkish; poetry/rhyme structures lost.

## Numbers, Units, and Formatting
- Units converted or invented without instruction (ft→m, ft²→m²), emergency numbers localized (911→112), time spans changed (business hours→days), planetary “sols”→“days”.
- Date/time formats auto‑switched (p.m.→24h); postal/ID fields localized (SSN→TR ID).
- Occasional rounding or range tightening (“up to three meters”→“three meters”).
- Inventions: added “waterfront,” added conversions in parentheses.
- Conversions/inventions: yes, frequent; table/markup mostly intact but occasionally rephrased headings.

## Meta/Disclaimer Behavior
- Rare overt policy disclaimers, but occasional authorial additions in notes/substitutions (e.g., cooking substitution guidance altered, vegetarian caveat dropped).
- Sometimes leaves signage/slogans unrendered or partially Turkish/English mixed in back output when embedded text present.

## Recommendations
- Prompting
  - Explicit constraints: “Do not localize or convert units, numbers, phone/emergency numbers, IDs, or proper nouns. Preserve acronyms and class/ability names verbatim. Do not add details.”
  - Add a protected‑token list inline: [911, SSN, MRR, ARR, CAC, SQL, VP Sales, ‘onshore’, ‘stage left’, named entities].
  - Require a self‑check: “Before finalizing, list any units/numbers/names changed; confirm none were altered.”
  - For signage/graphics: “Translate all visible text literally; do not leave source phrases untranslated.”
  - For modality: “Preserve MUST/SHOULD/MAY exactly; use ‘zorunlu/melidir’ mapping without weakening.”

- Decoding
  - Lower temperature/top‑p for technical/legal/metric content (e.g., T≤0.3); increase repetition penalty slightly to stabilize acronyms.
  - Use constrained decoding for protected tokens and numerals (copy‑mechanism or regex‑guarded spans).

- Data and Fine‑tuning
  - Augment with Turkish–English parallel corpora rich in: SaaS metrics/acronyms, legal/security specs (RFC modality), naval/nautical and stage direction glossaries, engineering terms (deburr, lugs/pins).
  - Include negative examples penalizing 911→112, ft²→m² conversions, and SSN→TR ID swaps.
  - Build a terminology memory/domain lexicon for recurring verticals (sales metrics, gaming skill names, trail/proper names).

- Post‑processing
  - Regex validators: detect unit/numeral mismatches; flag added conversions; verify emergency numbers unchanged.
  - Acronym whitelist check: ensure MRR/ARR/CAC/SQL, class/skill names, and UI labels (Checkout) are preserved.
  - NER alignment: compare source vs translation named entities; flag localization or Turkishization (Elm→Çınar).
  - Polarity/direction checklist: detect antonym candidates (nod/shake, onshore/offshore, left/right, raise/deburr).
  - Optional bilingual reviewer heuristics: highlight modality weakening (MUST/SHOULD), legal term drift (venue/jurisdiction).

## Confidence and Coverage
- Confidence: moderate–high. Patterns recur across many judges and domains (real estate, legal/security, business metrics, nautical, stage directions).
- Coverage limits: Evidence skews toward short/medium texts with varied domains; fewer long structured tables. Some issues (meta/disclaimers) appear rarely; poetry/rhyme findings are present but not dominant.