# Failure Model — Qwen 3 Max Preview — Hindi

## Summary
Overall, the Hindi round‑trip behavior shows fluent output but recurrent meaning drift on domain-critical terms, proper nouns, and quantifiers. The most salient weaknesses are: consistent substitution of near‑neighbor concepts (e.g., arbitration→mediation; defense→prosecution), technical/legal modality flips (MUST/SHOULD; “not exceeding”→fixed term), instability on low-frequency culinary/technical lexemes (parsley/basil/tamari; tallow), and brittle handling of proper names, dates, and AM/PM. Many errors reflect Hindi-specific lexical gaps or common-word bias (e.g., umbrella for awning; tulsi for basil), then amplify during back‑translation.

## Top Failure Modes
- Concept conflation in legal/technical domains — Swaps a term with a related but non-equivalent concept (e.g., arbitration→mediation; defense→prosecution), changing procedure or burden of proof; why: preference for more common or culturally salient Hindi terms and semantic smoothing; frequency: common; severity: high. Evidence: “arbitration consistently changed to mediation”; “‘defense’ swapped to ‘prosecution’ in reasonable doubt section.”

- Modality and bound misrendering — Spec requirement and penalty bounds altered (MUST/SHOULD inversion; “not exceeding ten days”→“for ten days”); why: Hindi modality (“चाहिए,” “जरूरी,” “अनिवार्य”) and “तक/से अधिक नहीं” are ambiguous, and back‑translation collapses nuance; frequency: occasional to common; severity: high. Evidence: “MUST→should and SHOULD→must”; “not exceeding ten days became fixed ten.”

- Rare/low-frequency noun substitution (culinary, materials, equipment) — Specific items replaced with more familiar Hindi terms (parsley→celery/ajwain; tamari→tamarind; tallow candle→oil candle; awning→umbrella), shifting facts; why: limited Hindi lexicon coverage for niche items and cultural defaults; frequency: common; severity: medium-high. Evidence: “sourdough→sardine; tamari→tamarind”; “tallow candle changed to candle of oil”; “‘awning’ consistently replaced by ‘umbrella.’”

- Proper names and exact tokens drift — Names, brands, and titles mutated (Duret→Durand; Aster→Ester; Caffiend→CaféAnd), and some dates/numbers altered; why: script transliteration ambiguity and normalization to familiar forms; frequency: common; severity: medium. Evidence: “Elaine→Ellen; May 3→May 5”; “Aster‑3 to Ester‑3; ValueSpree to ValueSpry.”

- Temporal and quantitative detail errors — AM/PM swaps, “up to/by” confusion, ranges converted to point values; why: Hindi time expressions (सुबह/शाम) and “तक/द्वारा” mapping are brittle; frequency: occasional; severity: high when present. Evidence: “4:00 AM became 4:00 p.m.”; “‘by 30%’ became ‘up to 30%.’”

- Precision loss in technical terminology — Legal/art conservation/aviation terms replaced by generic synonyms (motions→petition; flaking→cracking; offshore/onshore flipped once); why: synonym preference and insufficient domain grounding; frequency: common; severity: medium. Evidence: “‘Provenance’→‘Authenticity’”; “‘flaking/blanching’→‘cracking/spotting.’”

- Stylistic smoothing that alters semantics — Tone-preserving paraphrases change key polarity or imagery in poetry/prose (“kindness will not”→“no mercy”; “neon buds”→“neem buds”); why: aggressive fluency and idiom normalization; frequency: occasional; severity: medium. Evidence: “‘kindness’ to ‘no mercy’”; “‘neon’ became ‘neem.’”

## Language‑Specific Factors
- Lexical availability and cultural defaults:
  - Botanical/culinary items: basil often mapped to “तुलसी,” parsley conflated with “धनिया/अजवाइन,” tamari→इमली; these are natural Hindi defaults but not faithful to source.
  - Built environment: awning (“शामियाना/कैनोपी”) defaulting to umbrella (“छाता”) due to frequency.
- Legal modality in Hindi: MUST/SHOULD distinctions blur across चाहिए/अनिवार्य/उचित; “not exceeding” (से अधिक नहीं) vs “for” (के लिए) easily collapses back to a fixed duration.
- Role labels: बचाव पक्ष vs अभियोजन पक्ष inverted by proximity; both are short, common nouns in Hindi.
- Time expressions: 4 AM/PM requires explicit सुबह/शाम; omission invites reversal on back‑translation.
- Abstract philosophy/criticism: “बुद्धि/ज्ञान/प्रज्ञा” and “स्पष्टता/जीवंतता” can be near-synonyms in Hindi; back‑translation picks the wrong English counterpart.
- Transliteration instability: Proper nouns move Hindi→Latin with vowel/consonant drift (Duret/Durand; Ember/Amber).

## Numbers, Units, and Formatting
- Quantifier drift: “by 30%”→“up to 30%”; “up to” limits turned into fixed values; “not exceeding”→exact term.
- Time/date errors: 4 AM↔4 PM; specific dates altered by one or two days.
- Hazard classes and terms: “combustible”↔“flammable.”
- Range/optionality markers: “at least every four hours”→“every four hours.”
- No systemic table/markup issues observed, but headings/titles occasionally mislabeled.

## Meta/Disclaimer Behavior
- Minimal policy/safety intrusions. One “notes” section contained factual substitutions (apricot→peach; pâté→foie gras) but not explicit disclaimers. Intrusions do not appear to be a primary failure source.

## Recommendations
- Prompting
  - Provide a term glossary and do-not-substitute list: e.g., arbitration=पंचाट (not मध्यस्थता); defense=बचाव पक्ष; motions=मोषन्स; tamari=टमरी (leave transliterated).
  - Instruct: “Preserve proper nouns, dates, numbers, AM/PM exactly; if unsure, transliterate and add [sic].”
  - For legal/spec text: “Maintain modality exactly (MUST/SHOULD/MAY); retain ‘not exceeding’ and range limits verbatim.”
  - Force explicit time markers: “Write times with सुबह/शाम and AM/PM both.”
- Decoding
  - Lower temperature and penalize paraphrasing for legal/technical domains.
  - Use constrained decoding with lexicon locks for critical terms and named entities.
- Data and fine‑tuning
  - Fine‑tune on Hindi–English parallel corpora with:
    - Legal contracts, RFCs, and court instructions emphasizing arbitration/mediation and burden‑of‑proof contrasts.
    - Culinary/botanical glossaries mapping parsley/basil/tamari/tallow; materials conservation terminology; aviation/weather terms.
  - Add contrastive pairs for MUST/SHOULD; “not exceeding X days” vs “for X days”; awning vs umbrella; offshore vs onshore; combustion classes.
  - Expand transliteration training with ISO 15919 and supervised back‑transliteration for names/brands.
- Post‑processing checks
  - Automated diff for:
    - Named entities, dates, numerals, AM/PM.
    - Modals (MUST/SHOULD/MAY), bounded phrases (“not exceeding,” “at least,” “up to”).
    - Domain-specific pairs: arbitration/mediation; defense/prosecution; combustible/flammable.
  - Term QA against provided glossary; flag substitutions for reviewer approval.
  - Time normalization rule: if AM/PM lost or altered, fail the check.
  - Culinary/botanical validator to flag culturally biased swaps (tulsi/basil; coriander/parsley).
- Workflow
  - Two-pass: constrained translation → back‑translation self-check → repair pass guided by flagged diffs.
  - Offer “literal mode” for legal/spec text and “creative mode” for poetry; default to literal when domain uncertain.

## Confidence and Coverage
Confidence: medium-high. The error families recur across many judges and samples, with strong convergence on legal term swaps, modality errors, culinary/technical substitutions, and name/time drift. Coverage is broad across genres (legal, specs, culinary, literary, technical), but some domains (numerical units, tables/markup) had limited evidence; severity estimates are qualitative.