# Failure Model — Claude Opus 4.1 (no reasoning) — Chinese

## Summary
Opus 4.1’s zh↔en round‑trip shows strong semantic fidelity but systematic precision drift: it frequently normalizes specific terms into generic synonyms, flips tense/aspect, and destabilizes proper names and roles. The most salient weaknesses are: (1) terminology drift in legal/technical/organizational domains, (2) proper‑name and title corruption, (3) tense/register flattening, (4) number and role mismatches (singular/plural, rank/jurisdiction), and (5) idiom/imagery dilution (especially sound effects and sports/jargon). These are amplified by Chinese’s lack of overt tense/plural morphology and by the model’s paraphrasing bias.

## Top Failure Modes
- Terminology drift in formal domains — domain‑specific terms get “normalized” to near‑synonyms that change meaning or scope; description: “Charter→Constitution,” “Student Council→Student Union,” “minute order→court order summary,” “water‑resistant→waterproof,” “Ward Constable→inspector.” Why: paraphrase bias and preference for high‑frequency Mainland terms over exact source mappings. Frequency: common. Severity: high.
  - Evidence: judges flagged repeated “Charter/Student Council” substitutions; legal phrasing softened in several samples.

- Proper names and titles altered — person/book/cultivar names and document titles are respelled, localized, or swapped; description: Rin Vale→Lin Weir; John Pory→Parry; “Ember Queen” tomato renamed. Why: inconsistent transliteration/back‑transliteration, capitalization loss in zh, and name normalization. Frequency: common. Severity: high.
  - Evidence: “Rin Vale→Lin Weir” noted as major; printer’s name error (Pory/Parry) recurs.

- Tense and register shifts — present→past and formal→neutral changes flatten voice and immediacy; description: narrative present moved to past; archaic/legalistic tone modernized. Why: Chinese weakly marks tense/aspect; model over‑regularizes style on re‑expression. Frequency: common. Severity: medium.
  - Evidence: “present→past alters narrative immediacy”; “archaic legal language modernized (‘season’→‘quarter’).”

- Number and role mismatches — singular↔plural, ranks, official bodies, and parties are flipped; description: Sellers→seller; Lieutenant Commander→Major; Commonwealth→federal government. Why: lack of plural marking in zh, ambiguity in titles, domain bias. Frequency: occasional. Severity: high.
  - Evidence: consistent singularization of “Sellers”; rank change (Lieutenant Commander→Major); jurisdiction shift.

- Idiom, imagery, and jargon flattening — evocative phrases/idioms and onomatopoeia mapped to literal or incorrect terms; domain jargon generalized; description: “Leave it all on the field”→“Give it your all”; “hush‑hush crowd” becomes “booing”; “chossy steps”→“scree sections”; sports “play”→“ball,” “Sunset Lap” renamed. Why: idiom domestication, weak alignment on niche lexicons (sports/mountaineering/poetry). Frequency: common. Severity: medium.
  - Evidence: crowd sound misread as booing; key sports term and closing idiom altered.

- Instructional action drift — imperative steps and technical operations slightly changed, altering actions; description: “Slide forward”→“Lean forward”; “clip the strap”→“fasten the buckles”; “access”→“freight.” Why: synonym preference and low tolerance for terse imperative mapping. Frequency: occasional. Severity: medium.
  - Evidence: safety/gear instructions changed in wording that affects required motion.

- Title/label/level normalization — tiers/levels, product/spec labels, and phone prompts rephrased; description: “Tier 2”→“Level 2”; awkward phone number query; “Item ID”→“Item Number.” Why: label canonicalization and localization tendencies. Frequency: occasional. Severity: low.
  - Evidence: Tier/Level substitution; phone number wording called out as unclear.

- Minor content inventions/omissions — small additions (e.g., “one‑dollar”), or softening/omitting qualifiers; Why: fluency optimization and gap‑filling. Frequency: rare. Severity: medium.
  - Evidence: added price descriptor in a narrative; omitted “fully wheelchair” → “fully accessible.”

## Language‑Specific Factors
- Lack of tense/aspect morphology in Chinese encourages English tense drift on back‑translation (present→past; certainty modals shift).
- No plural marking and weak determiners drive singular/plural party flips (e.g., Sellers↔seller) and article/quantifier softening.
- Proper noun detection is harder due to casing absence and flexible word segmentation; names/titles get normalized or transliterated inconsistently.
- Idioms/onomatopoeia/sports jargon often lack 1:1 mappings; the model prefers literal paraphrases (“hush”→“booing”) or domesticated expressions, reducing fidelity.
- Register mapping: Chinese formal/legal style often re‑expressed in modern plain Chinese, then back into modern English, eroding archaic/legalistic tone.

## Numbers, Units, and Formatting
- Singular↔plural role labels: consistent drift for contractual parties and job titles. Conversions/inventions are rare but present (adding “Fahrenheit” parenthetical once).
- Labels and levels normalized: “Tier”→“Level,” “primary/master,” “Ward Constable→inspector.”
- Phone/metadata prompts occasionally become awkward or less specific; tables/structures generally preserved, but headings can be paraphrased.
- Units mostly stable; the larger issue is label/category integrity rather than numeric values.

## Meta/Disclaimer Behavior
- Minimal meta/disclaimer intrusion. One-off register shifts or audience label changes (“cyclists”→“riders”) occur, but safety/policy disclaimers did not contaminate output. Triggered rarely in public‑notice or advisory text.

## Recommendations
- Prompting
  - Use a locked‑term glossary block: “Preserve exactly: Student Council, Charter, Minute Order, Tier 2, Commonwealth, Sunset Lap, [names/titles]. Do not paraphrase these.”
  - Explicit constraints: “Keep tense as in source; preserve register (archaic/legalistic/poetic). Keep singular/plural and party labels exactly.”
  - Annotated markup: wrap named entities, titles, and controlled terms in tags (<NE>, <TERM>) to signal non‑alteration across directions.
  - Add a “do‑not‑domesticate idioms” note; for idioms/jargon, prefer literal plus parenthetical gloss in zh, then remove gloss on back‑translation if required.

- Decoding
  - Lower temperature/top‑p to reduce synonym churn in back‑translation.
  - Apply constrained decoding with a terminology list and regex locks for ranks, parties, levels, and phone/email formats.

- Data and fine‑tuning
  - Augment with high‑precision parallel corpora for: legal orders, contracts (party plurality), academic bibliographies, sports play‑by‑play, mountaineering and AV/industrial manuals.
  - Fine‑tune on name/title transliteration consistency sets (EN↔ZH↔EN), including rare surnames and cultivar/brand names.
  - Include curated idiom/onomatopoeia mappings (crowd sounds, impact SFX, sports idioms) with back‑translation preservation targets.

- Post‑processing checks
  - Term consistency checker: diff against a whitelist for exact matches (Charter vs Constitution; Student Council vs Union; Minute Order).
  - NE audit: Levenshtein/name dictionary to flag Rin/Rin→Lin/Weir; book/printer/publisher names; cultivar/variety names.
  - Morphology audit: enforce party plurality and role labels (Sellers/Buyers; ranks/titles) and flag singularization.
  - Tense/register audit: detect present→past drift and style changes (archaic markers lost).
  - Action integrity lint: compare imperatives with verb class constraints (slide vs lean; clip vs fasten) and flag motion/tool changes.

## Confidence and Coverage
- Confidence: medium‑high. Dozens of low‑score rationales converge on the same families (terminology drift, names, tense/register, number/role, idiom/jargon).
- Coverage limits: Evidence spans multiple domains and judges but is skewed toward English‑origin texts into zh and back. Few extreme numeric/unit cases and minimal meta‑policy triggers; table/markup fidelity not deeply probed.