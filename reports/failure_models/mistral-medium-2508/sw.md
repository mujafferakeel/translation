# Failure Model — Mistral Medium 3.1 — Swahili

## Summary
Across domains, the model often preserves overall structure but frequently substitutes core content with semantically adjacent or culturally common terms, introduces inventions, and flips key facts. The most salient weaknesses are: (1) systemic lexical substitution in domain‑specific nouns (food, tools, botany, tech) leading to entity drift; (2) number/time/unit corruption (months→years, oz→lb, dates/times); (3) figurative and poetic imagery being literalized or surrealized; (4) technical terminology collapse (patent, legal, forensic) into everyday Swahili or wrong English loans; (5) sporadic meta/disclaimer insertions. Many errors reflect Swahili‑specific pitfalls (polysemy, noun‑class pressure, limited domain lexicon), amplified by aggressive paraphrasing.

## Top Failure Modes
- Entity drift via “nearest neighbor” nouns — swaps core nouns with plausible but wrong neighbors; cascades through the text
  - Why: Limited/unstable Swahili domain lexicon; reliance on high‑frequency collocations; mapping to more common loanwords.
  - Frequency: common; Severity: high
  - Evidence: “red lentils”→“kidney beans” repeated; “Ember Queen tomato”→pepper with invented “fiery kick.”

- Figurative imagery distortion — metaphors replaced by literal or bizarre imagery; tone flips
  - Why: Figurative language resolved to concrete high‑probability phrases; poetic registers underrepresented in Swahili training data.
  - Frequency: common; Severity: high
  - Evidence: “pigeons by the fountain”→“souls by a dried well” with “grammar of rainy season”; “unfurled embrace”→“clash of struggle.”

- Numbers, dates, and times corruption — unit conversions invented; AM/PM and duration flips
  - Why: Weak grounding in imperial units and AM/PM in Swahili; tendency to normalize to familiar patterns.
  - Frequency: common; Severity: high
  - Evidence: “18 months”→“18 years/8 years”; “4 oz tallow candle”→“4 lb oil wick”; 3 p.m.↔3 a.m., Friday→Tuesday.

- Technical term collapse (patent/legal/forensic/finance) — maps to wrong domains or colloquialisms
  - Why: Ambiguous English stems (“cam”, “drop‑in”), scarce Swahili technical lexicon, overuse of generic loans.
  - Frequency: common; Severity: high
  - Evidence: “cam actuator”→“camera component”; “operating cash flow”→“legal tender profits”; “reagent blanks, stochastic effects” garbled.

- Role/action inversions in procedural or play‑by‑play text — actors, directions, or outcomes flipped
  - Why: Free word order tendencies in Swahili; pronoun/gender neutrality; pressure to smooth narratives.
  - Frequency: occasional; Severity: high
  - Evidence: Basketball play: tied score and foul/free‑throw sequence wrong; stage “downstage”→“upstage.”

- Omission/addition with meta intrusions — headers, salutations, editorializing appear; key lines dropped
  - Why: Training on mixed web text; decoder filling with templatey framing; length control misfires.
  - Frequency: occasional; Severity: medium
  - Evidence: Added “Swahili Explanation/Mpendwa”; missing intros; editorial “overcharged” added to a lease dispute.

- Directional and spatial flips — left/right, up/down, floor/ceiling inversions
  - Why: Spatial term ambiguity; back‑translation error sensitivity; smoothing to prototypical scenes.
  - Frequency: occasional; Severity: medium
  - Evidence: Left turn→right; ceiling trap→floor; “landing”→“ceiling.”

## Language‑Specific Factors
- Polysemy and loanword overload: English “cam” mapped to “kamera”; “ink”→“wine” in poetic registers; “fog”→“rain” likely from higher frequency mvua collocations.
- Noun class and gender: Swahili’s limited grammatical gender and flexible agent marking contribute to gender swaps (Ms.→Mr.) and role inversions.
- Unit/time conventions: Swahili discourse favors 24‑hour times and metric; AM/PM and imperial ounces/pints are error‑prone (oz↔lb, p.m.↔a.m.).
- Figurative density: Swahili poetic diction differs; the model defaults to everyday imagery or moralizing phrases, yielding surreal insertions (“grammar of rainy season”).
- Domain lexicon gaps: Botany/culinary terms (lentils varieties, heirloom tomato cultivars) and sports/forensic/patent jargon lack robust, standardized Swahili equivalents, prompting “closest fit” substitutions (tomato↔pepper; holding mid↔center‑back).

## Numbers, Units, and Formatting
- Recurrent duration and date drift: months→years; weekdays shifted; ranges extended.
- Unit misrendering: imperial to exaggerated metric/weight (oz→lb); tax schedules (quarterly→annual).
- Count and tally errors: scores, guard counts, grill counts, public comments flipped.
- Formatting intrusions: added headings (“Swahili version”), salutations, or role labels; occasional table/section renaming.

## Meta/Disclaimer Behavior
- Occasional injected meta notes or framing (“Swahili Explanation,” translator headers), sometimes with editorial judgments.
- Triggers: emails/announcements, instructional or official tones, and when the source contains headings—model mirrors/expands formatting.

## Recommendations
- Prompting
  - Use a fidelity guardrail: “Translate into Swahili. Do not add or omit. Preserve all numbers, dates, units, names, ingredients, and technical terms exactly. If unsure of a term, keep it in English in quotes and add a brief Swahili gloss in parentheses.”
  - Add domain hints: “Domain: patent law/forensic genetics/culinary” and a term list to preserve.
  - For poetry: “Maintain imagery literally; avoid paraphrase and moralizing; no additions.”
  - Enforce constraints: “Return only the translation, no headings, salutations, or notes.”
- Decoding
  - Lower temperature/top‑p to reduce creative paraphrase; enable repetition penalty only for meta tokens (e.g., “Mpendwa,” “Maelezo”).
  - Length penalty near source length to discourage omissions/additions.
- Data and fine‑tuning
  - Curate parallel Swahili corpora for: patents, legal contracts, finance reports, forensic testimony, sports commentary, horticulture/culinary, and stage directions.
  - Build a Swahili termbank for high‑risk domains (e.g., “cam actuator,” “reagent blank,” “drop‑in allele,” “red lentils,” heirloom cultivar names, football roles).
  - Augment with unit/number contrastive pairs (AM/PM, oz vs lb, months vs years) and left/right spatial disambiguation sets.
  - Include modern Swahili poetic translations aligned to literal imagery retention.
- Post‑processing
  - Automated checks: regex to compare numbers, dates, units, named entities, and cardinal directions against source; flag mismatches.
  - Domain lexicon locking: dictionary‑based protection for critical nouns (e.g., “haragwe nyekundu” vs “dengu”; enforce “dengu/masoor” for lentils).
  - Back‑translation QA loop focused on red‑flag domains; diff‑based alarms for entity or unit drift.
  - Style lint: blocklist common meta insertions; enforce no extraneous headings/sign‑offs.

## Confidence and Coverage
- Confidence: medium‑high. Patterns recur across many low‑score cases and include multiple samples explicitly from Mistral Medium 3.1 in Swahili.
- Coverage limits: Evidence spans diverse genres but is skewed toward error cases; relative frequencies are qualitative. Some issues (sports subroles, archaeology color codes) may reflect sparse domain training rather than systematic language weaknesses.