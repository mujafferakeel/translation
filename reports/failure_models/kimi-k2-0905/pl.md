# Failure Model — Kimi K2-0905 — Polish

## Summary
Overall, the Polish round‑trip output from Kimi K2‑0905 preserves structure and flow but shows recurring meaning drift on domain terms and occasional hard reversals of intent. The most salient weaknesses are: (1) polarity/operation reversals in instructions and legal thresholds; (2) domain terminology drift or substitution (law, engineering, architecture, sports, tools, natural science); (3) specific‑label swaps (species, materials, ingredients, cuts, titles); (4) register/idiom flattening in business/legal/UX prose; and (5) metaphor/imagery distortion in literary text. Polish‑specific pressures (gender/number without articles, idiomatic calques, local term mappings like “mała architektura”) amplify these errors.

## Top Failure Modes
- Polarity or directional reversals — Meaning flips in constraints, operations, or standards; e.g., extend vs set back, prohibition vs allowance, standard of proof stronger/weaker than intended.
  - Why: Ambiguous Polish antonym pairs, negation scope, and preference for fluent paraphrase over exact semantic features.
  - Frequency: common; Severity: high
  - Evidence: “setting back the canopy” becomes “extending the canopy”; “beyond a reasonable doubt” → “beyond all doubt”; “No timers, no transfers—just pour” inverted to “no pouring—just fill.”

- Domain terminology drift (precision → generic synonym) — Technical/legal/UX/architectural terms replaced with looser or adjacent terms, losing accuracy.
  - Why: Sparse domain‑aligned Polish<>English mappings and reliance on frequent general synonyms; Polish polysemy and missing articles push specificity loss on back‑translation.
  - Frequency: common; Severity: high
  - Evidence: “failover” → “switch”; “lead time / customer success / cut corners” simplified to “delivery time / support / not standards”; “parapet” → “sill,” “Street‑Furniture Palette” → “Small‑Architecture Palette.”

- Specific label/category substitutions — Named entities or fine‑grained labels swapped within a taxonomy (species, tools, materials, sports stats).
  - Why: Nearest‑neighbor substitution under lexical uncertainty; limited terminological grounding for niche domains (ornithology, soils, culinary cuts).
  - Frequency: common; Severity: high
  - Evidence: curlew → golden plover; reed bunting → reed warbler; sandy loam → sandy clay; “impact driver/torque drivers” → “impact wrench/torque wrenches”; “runs” → “points.”

- Procedural/rule details corrupted — Critical rules or steps changed, especially orientations, materials, or chemicals.
  - Why: Over‑confident paraphrase; confusion of near‑synonyms with operationally distinct meanings; polarity handling issues.
  - Frequency: occasional; Severity: high
  - Evidence: discard pile face‑up vs face‑down flipped; completed cards face‑up vs face‑down flipped; “bleach” substituted with “hydrogen peroxide.”

- Register and idiom mismatch (formal → generic; idiom literalization) — Corporate/legal/procedural tone softened; idioms normalized or misread.
  - Why: Preference for high‑probability paraphrases; Polish idioms/calques mapped to broad English terms; article loss reduces contractual exactness.
  - Frequency: common; Severity: medium
  - Evidence: “as is” → “as seen”; “Commonwealth” → “Prosecution”; “admission control” → “access control”; “cut corners” rephrased away from the idiom.

- Figurative language distortion — Metaphors re‑anchored into different conceptual fields; imagery swapped (constructive → aggressive).
  - Why: Prioritizing local coherence over extended metaphor consistency; metaphorical polysemy in Polish invites literal or alternate mappings.
  - Frequency: occasional; Severity: medium
  - Evidence: “rivet” → “barb,” “gear” → “tooth”; “paperweight against drafts” → “buzzer.”

- Gender, titles, and participant roles — Neutral or unspecified gender becomes masculine; honorifics or roles misassigned.
  - Why: Polish lacks singular gender‑neutral pronouns; default masculine back‑projection; Ms./Mrs. ambiguity and title normalization.
  - Frequency: occasional; Severity: medium
  - Evidence: gender‑neutral “their” recast as “his”; “Ms.” → “Mrs.”; “Clerk” → “Justice.”

- Minor factual/time/context shifts — Times of day, small scene or object attributes altered; measurement/context minimized.
  - Why: Preference for more common collocations in Polish; absence of articles yields scope/number shifts.
  - Frequency: occasional; Severity: low
  - Evidence: afternoon → morning; lunch hour → morning rush; “self‑closing door” → plural “doors.”

## Language‑Specific Factors
- Article‑less Polish → articleful English: Leads to specificity and definiteness errors (a/the), causing “Seller” role scope shifts and “as is” misrendered as “as seen.”
- Gender and agreement: Polish pushes masculine defaults; neutral “they” often returns as “he,” and titles (Pani/Pan) map imperfectly to Ms./Mrs./Mr.
- Institutional/urbanism calques: “Mała architektura” is Polish standard for street furniture; model maps it back as “Small‑Architecture,” not “Street‑Furniture,” yielding register drift.
- Legal/technical term pairs with subtle distinctions: combustible vs flammable (palny/łatwopalny), admission vs access control, “as is” clauses — Polish lexical overlap fuels precision loss.
- Sports lexicon gaps: Cricket/baseball use “runs,” but Polish generalizes to “punkty,” promoting “points” on return.
- Rich inflection and free word order: Increases ambiguity in scope/negation, contributing to polarity reversals in instructions.

## Numbers, Units, and Formatting
- Numbers/units: Few numeric conversion mistakes; occasional format shifts (“No. 10/8” vs “ten/eight”) acceptable.
- Dates/times: Time‑of‑day swaps appear (“afternoon” → “morning”).
- Tables/markup: Not implicated.
- Orientation/state descriptors: Critical face‑up/face‑down and on/off style states occasionally flipped in rules/instructions, causing high‑impact errors.

## Meta/Disclaimer Behavior
- No safety notes, policy disclaimers, or authorial intrusions observed contaminating translations, even for legal/medical/procedural content. The model remains in translation mode.

## Recommendations
- Prompting
  - Use strict constraints: “Translate exactly; do not paraphrase; preserve polarity (negations, increases/decreases), legal thresholds, and domain terms. If uncertain, retain the source term in brackets.”
  - Provide mini‑glossaries per domain (e.g., law, architecture, soils, tools, sports) and require term locking: “Do not replace glossary terms.”
  - Add polarity sentinels: “Confirm that ‘set back’ ≠ ‘extend’, ‘face‑up’ ≠ ‘face‑down’, ‘reasonable doubt’ ≠ ‘all doubt’.”
  - Enforce register: “Maintain formal corporate/legal register; prefer ‘as is,’ ‘admission control,’ ‘failover,’ ‘Street‑Furniture’ exactly.”
- Decoding
  - Lower temperature/top‑p to reduce synonym drift; enable constrained decoding on glossary terms and critical polarity tokens (no/not, up/down, increase/decrease).
- Data
  - Fine‑tune on parallel Polish–English corpora for: planning/architecture guidelines, legal jury instructions, UX/business documentation, DevOps/infra (failover, tail latency), sports rulebooks (cricket/baseball), ornithology/botany, soils/materials, tools/hardware.
  - Contrastive training for antonym/polarity pairs (set back/extend; face‑up/face‑down; combustible/flammable; reasonable/all doubt).
  - Augment with Polish institutional terminology mappings (e.g., “samorząd uczniowski” → “Student Council,” not “Self‑Government”).
- Post‑processing QA
  - Terminology validator: flag out‑of‑glossary substitutions for key terms (legal, technical, product, species).
  - Polarity checker: detect antonym flips and orientation/state inversions.
  - Gender/role audit: preserve gender neutrality unless specified; keep titles (Ms./Mr./Clerk) as in source when possible.
  - Named‑entity/type checker: species, materials, cuts, tools validated against controlled vocabularies.
  - Procedural consistency checks: steps/chemicals/materials unchanged; highlight any substitutions (bleach vs hydrogen peroxide).
  - Style filter: enforce domain register (legal/corporate) and idiom preservation (“as is,” “cut corners”).

## Confidence and Coverage
- Confidence: Moderate. Dozens of low‑score rationales across varied domains show consistent patterns (terminology drift, polarity reversals, register flattening, label swaps).
- Coverage limits: Evidence skews toward technical/legal/procedural and literary samples; fewer pure numeric/unit cases and no meta policy intrusions observed. Multiple judges provide diversity, but some items are near‑misses rather than catastrophic errors.