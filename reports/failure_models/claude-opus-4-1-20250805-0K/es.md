# Failure Model — Claude Opus 4.1 (no reasoning) — Spanish

## Summary
Overall, the Spanish round‑trip shows strong meaning retention but systematic precision drift: the model frequently “smooths” specific, domain‑bound English terms into more generic Spanish phrasing and then back into near‑synonyms that alter register, tone, or facts. The most salient weaknesses are: (1) persistent synonym flattening that erodes technical, legal, and stylistic nuance; (2) improper handling of fixed names and UI strings; (3) modal strength, tense, and gender neutrality shifts driven by Spanish morphology; (4) small but consequential factual tweaks (time of day, locations, parts, roles); and (5) idiom/poetry meter mismatches and archaic/legal register modernization.

## Top Failure Modes
- Terminology flattening in domain texts — Replaces precise terms with generic synonyms, reducing register and technical accuracy; likely due to preference for idiomatic Spanish and insufficient domain constraints. Frequency: common; Severity: medium-high.
  - Evidence: “hashed” became “encrypted”; “stemming”→“lemmatization”; “heavy timber”→“dense forest”; “lugs”→“tabs”.
  - Evidence: Business/tech terms generalized: “inventory write-down”→“inventory reduction”; “patrons”→“users”.

- Unstable proper nouns and in‑world names — Fixed game/lore names, abilities, and places get altered or normalized, then back‑translate differently; likely lack of glossary/Do‑Not‑Translate handling. Frequency: common; Severity: high when factual.
  - Evidence: Boss/ability/location names consistently renamed (“Wight Lord”→“Specter Lord”; “Sundering/Rending Strike”; “Dragonmaw/Swamps”).
  - Evidence: Species/unique entities drift: “Glassback Marsh Stag”→“Crystal‑Backed Swamp Deer.”

- Small factual shifts (time, objects, roles) — Micro‑edits to details (late evening→late afternoon; bay window omitted; “burglary”→“robbery”; “waders”→“fisherman’s boots”) likely triggered by lexical ambiguity or Spanish lexical gaps. Frequency: common; Severity: medium.
  - Evidence: Multiple cases of evening/afternoon swap and omission of “bay” in “bay window.”
  - Evidence: Legal nuance lost: “burglary” rendered as “robbery”; “Ward Constable”→“Bailiff.”

- Modal/tense strength and UI label changes — “Must/should” and imperative/UI button text drift; Spanish modal/imperative choices back‑translate with different force; lack of string‑literal treatment. Frequency: occasional; Severity: medium-high in specs/UX.
  - Evidence: “should”↔“must” swap flagged; “Checkout” replaced with “Pay” and “GO” softened to “PROCEED.”
  - Evidence: “Back to sign in”→“Log back in” changes function.

- Gender and inclusivity shifts — Neutral references become gendered in Spanish and re‑emerge incorrectly; pronoun/agent roles drift. Frequency: occasional; Severity: medium in inclusive or legal contexts.
  - Evidence: “their” became “his”; quote “They”→“He.”
  - Evidence: Neutral “batter” turned into gendered “batsman.”

- Idioms, archaic, poetic register loss — Idioms calqued or simplified; rhyme/meter not preserved; archaic/legal openers modernized. Frequency: occasional; Severity: medium for style fidelity.
  - Evidence: “arointing geese” mishandled; “Whereas”→“Considering that.”
  - Evidence: Recurrent note: evocative lexicon flattened (“thrill”→“emotion,” “rushed in”→“returned”).

- Category/part misidentification — Specific part/endpoint/discipline swapped by near neighbor; likely due to term proximity and Spanish polysemy. Frequency: occasional; Severity: medium.
  - Evidence: “blossom end” vs “calyx end”; “warded metal”→“guarded metal.”
  - Evidence: “crash” interpreted as “freeze”; “stream graph”→“flow graph.”

## Language‑Specific Factors
- Gender marking and neutral “they”: Spanish lacks a direct singular neutral; choices (él/ella, they‑style forms) often back‑translate as gendered English. This triggers role/gender flips and loss of inclusivity.
- Modal force and obligation: Spanish modal/impersonal constructions (deber/debería/hay que) can re‑enter English with stronger/weaker obligation (“must”/“should”) differences.
- Lexical granularity: Spanish often encodes fewer fine‑grained distinctions for legal/technical pairs (e.g., robo/hurto vs burglary/robbery), leading to back‑translation collapse.
- Register mapping: Archaic/legal openers (“Whereas”) and period‑specific nautical/sport jargon incline the model toward contemporary, natural Spanish, then back to modernized English.
- Idioms and poetry: Maintaining meter/rhyme across Spanish↔English often yields paraphrase and loss of meter; idioms get literalized or replaced with approximate locutions.
- Cultural substitutions: The model occasionally “domesticates” with Hispanic cultural terms (e.g., “salsa,” “raspado”), then back‑translates to non‑original items.

## Numbers, Units, and Formatting
- Numerals/units largely preserved; overt numeric inventions are rare. Occasional register/term swaps alter precision (“puncture‑resistant”→“puncture‑proof”; overclaim).
- Dates/times: several time‑of‑day shifts (evening↔afternoon) suggest lexical ambiguity or normalization in Spanish that reenters English misaligned.
- UI/tables/headers: Headings and button labels often paraphrased (“Abstract”→“Summary”; “Checkout”→“Pay”); capitalization and literal string fidelity not enforced.

## Meta/Disclaimer Behavior
- Safety or policy insertions are rare to absent. “Disclaimer_meta” tag occurrences in evidence correspond to terminology drift rather than added safety text. Contamination via authorial intrusions not observed as a pattern.

## Recommendations
- Prompting
  - Enforce glossary and DNT: “Use a bilingual glossary; do not translate or alter proper nouns, game terms, UI strings, or code identifiers. Preserve capitalization and punctuation exactly for bracketed strings [like this].”
  - Literalness toggles: “Prioritize terminological precision over fluency; avoid synonym substitution, preserve modals (must/should), and keep time expressions verbatim.”
  - Register constraints: “Maintain legal/archaic register; do not modernize (‘Whereas’, ‘hereby’). Preserve poetic imagery even if meter is imperfect.”
  - Gender neutrality: “Preserve gender neutrality; avoid inferring gender; when Spanish requires gender, prefer neutral paraphrases and annotate if necessary.”
- Decoding
  - Lower temperature/top‑p for UI/technical/legal segments to reduce paraphrasing. Consider segment‑wise decoding: strict mode for marked spans, normal for narrative.
- Data
  - Augment parallel corpora with: UI strings, game/fictional lore glossaries, legal/archaic forms, nautical/cricket/sports terminology, and security/ML technical lexicons (stemming vs lemmatization, hashing vs encryption).
  - Include Spanish neutral/inclusive language patterns and back‑translation training that penalizes gender and modal drift.
- Fine‑tuning targets
  - Contrastive pairs penalizing: must/should swaps, burglary/robbery confusion, time‑of‑day shifts, blossom/calyx ends, crash/freeze, warded/guarded.
  - Name fidelity objective: penalties for altering proper nouns and in‑world terms.
- Post‑processing
  - String checks: Regex lock for bracketed/quoted UI labels and proper nouns; diff source vs translation for exact matches.
  - Modal/gender lint: Heuristics to flag must/should changes; detect gendered pronouns introduced where source was neutral.
  - Terminology QA: Domain dictionaries (security, NLP, woodworking, nautical, sports) to flag near‑synonym substitutions.
  - Detail diffs: Rules to flag time expressions, locations, and part names for manual review.

## Confidence and Coverage
Moderate confidence. Evidence spans many judges and domains with consistent patterns (terminology flattening, name instability, modal/gender shifts, small factual edits). Less coverage on strict numerical/unit errors or formatting tables; meta/disclaimer behavior appears negligible. Domain mix is broad but game/UI/legal/tech cases dominate the error signals.