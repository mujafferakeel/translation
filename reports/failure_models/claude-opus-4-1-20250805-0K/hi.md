# Failure Model — Claude Opus 4.1 (no reasoning) — Hindi

## Summary
Overall, the model preserves gist and structure but systematically flattens nuance and drifts on specificity in Hindi. The most salient weaknesses are: (1) systematic softening or shifting of modality and tense (must/should; would/will), (2) loss of technical/legal precision via generic synonyms, (3) semantic drift on culturally or lexically ambiguous Hindi terms (time-of-day, culinary/biology/tool names, sports roles), (4) gender, number, and named-entity fidelity slips under Hindi’s agreement and transliteration pressures, and (5) idiom and imagery simplification that dulls voice and metaphor.

## Top Failure Modes
- Modality and conditional drift — English “must/shall/would” becomes softer or more definite Hindi, then back to altered English; why: Hindi has overlapping forms (चाहिए, चाहिए कि, करना होगा, करेगा) and the model defaults to more colloquial/neutral choices; frequency: common; severity: high.
  - Evidence: requirement softened “must”→“should” in specs/policies (e.g., doc_0101, doc_0105); conditional “would” rendered as “will,” changing satirical/hypothetical tone (doc_0040).

- Technical/legal term dilution — Precise terms replaced with broader Hindi equivalents; why: sparse high-quality Hindi domain terminology and synonym conflation in pretraining; frequency: common; severity: high.
  - Evidence: “Residential Burglary”→“Theft,” altering legal category (doc_0108); “admission control”→“access control,” “confidentiality”→“privacy” (doc_0090).

- Time and quantifier ambiguity — Noon/afternoon and “by” vs “up to” shift; why: Hindi time terms (दोपहर/अपराह्न) and quantifier particles (तक/से) are ambiguous; frequency: occasional; severity: high when deadlines/metrics matter.
  - Evidence: “Friday at noon”→“Friday afternoon” repeatedly (doc_0126); “by 30%”→“up to 30%” (doc_0112).

- Named entities and gender agreement errors — Names, genders, and titles drift; why: transliteration ambiguity (ए/ऐ, ब/भ), Hindi grammatical gender pressure, and role lexicon overlap; frequency: occasional; severity: medium.
  - Evidence: Ember→Amber (doc_0109); “her shadow”→“his shadow” (doc_0003); ATTORNEY→Prosecutor shift mismatching “my client” (doc_0031).

- Domain lexicon conflation (ecology, tools, food, sports) — Near-neighbors in Hindi collapse distinct English items; why: single Hindi lemma covers multiple English items (धनिया, नींबू) and under-specified tool names; frequency: occasional; severity: medium.
  - Evidence: riparian→coastal; brackish→saline (doc_0054); trowel→hoe, shears→scissors (doc_0182); cilantro vs coriander confused (doc_0069); limes→lemons (doc_0069).

- Idiom/imagery flattening and metaphor substitution — Vivid phrasing becomes generic; why: preference for safe, literal Hindi and limited exposure to literary Hindi registers; frequency: common; severity: medium.
  - Evidence: levee→dam; kiln of will→furnace of desire; “old hands”→generic phrasing (doc_0008, doc_0099); poetic verbs replaced with simpler ones (doc_0104).

- Role/terminology register shifts to gendered or formalized Hindi — Neutral or inclusive English mapped to gendered/common Hindi; why: sports and media Hindi norms skew masculine and formal; frequency: occasional; severity: low–medium.
  - Evidence: “batter”→“batsman” (doc_0087); “students”→“disciples,” “events”→“programs” shifting institutional tone (doc_0064, doc_0103).

- Specific-location/action misreads within narratives — Small but plot-relevant detail flips; why: resolution loss when Hindi lacks exact lexical anchor (e.g., “windowsill”), leading to plausible alternatives; frequency: occasional; severity: medium.
  - Evidence: poisoning “in the kitchen” instead of correct locus (doc_0082); windowsill→inner wall (doc_0131); “overturn cushions”→“flip mattresses” (doc_0019).

## Language‑Specific Factors
- Polysemy and coverage gaps:
  - Time-of-day: दोपहर/अपराह्न spans noon→afternoon; triggers noon→afternoon drift.
  - Quantifiers: “तक,” “तकरीबन,” “के द्वारा” blur “by” vs “up to.”
  - Culinary/botanical: नींबू (lemon/lime), धनिया (coriander seed/cilantro leaves), leading to swaps.
  - Tools: खुरपी/कुदाल and lack of precise terms for trowel/shears.
  - Ecology: तटबंध/बंध (levee/dam), तटीय/तटीय नदी तट vs riparian/coastal conflation.
- Gender and agreement pressure: Hindi enforces grammatical gender and agreement; ambiguous English subjects or possessives often get forced gender (“her”→“his”) and then back-translated incorrectly.
- Register norms: Hindi journalism/officialese pushes formal synonyms and domesticated phrasing; sports lexicon historically gendered (“बल्लेबाज” read as male).
- Transliteration ambiguity: Vowel length and consonant pairs (ए/ऐ; ब/भ; व/ब; एम्बर/अम्बर) yield Name drift (Ember→Amber).
- Word order and light verbs: Complex English phrasal verbs/idioms map to light-verb constructions that invite genericization and lose imagery.

## Numbers, Units, and Formatting
- Time: noon vs afternoon shifts via दोपहर/अपराह्न; weekday+time constraints distorted in back-translation.
- Percentages/targets: “by X%”→“up to X%” due to “तक” defaults.
- Lists and modality in bullet points: Imperatives softened; parallel verb forms normalized, weakening stylistic consistency (doc_0118).
- No systematic unit conversion errors observed; tables/markup generally preserved.

## Meta/Disclaimer Behavior
- Rare, low impact. Occasional register shift toward “field notes” or added meta tone in formal reports (doc_0084); no persistent safety disclaimers contaminating translations.

## Recommendations
- Prompting
  - Pin modality: “Preserve deontic/epistemic force exactly; do not soften ‘must/shall’; keep ‘would’ as conditional.”
  - Glossary injection: Provide domain term pairs (burglary=गृहभेदन, levee=तटबंध, riparian=नदी तटीय, trowel=खुरपी/त्रिशूलाकार खुरपी, cilantro leaves=हरा धनिया, coriander seeds=धनिया दाना).
  - Entity locks: “Do not alter names, titles, or genders; transliterate names consistently; keep spellings (Ember, Greyhaven) unchanged.”
  - Time/number guardrails: “Render times literally: noon=दोपहर 12 बजे; do not substitute ‘afternoon’.”
  - Style constraint: “Maintain imagery and idioms; prefer vivid, specific Hindi over generic synonyms; avoid formalization unless source is legal/archaic.”

- Decoding
  - Lower temperature/top-p to reduce synonym drift.
  - Enable constrained decoding on named entities, dates, numerals, and modals (must/shall/would).

- Data and fine‑tuning
  - Augment with parallel corpora in Hindi for:
    - Legal/policy texts (burglary vs theft, admissibility, confidentiality).
    - Networking/security and software docs (admission control vs access control).
    - Ecology/biology/cuisine/tools glossaries.
    - Sports with gender-neutral terminology in Hindi.
  - Targeted contrastive training on modality/time/quantifier minimal pairs (must vs should; noon vs afternoon; by vs up to).
  - Literary Hindi datasets to improve idiom and metaphor retention.

- Post‑processing QA
  - Automated checks:
    - NER consistency (names, places, titles).
    - Modality diff (must/shall/should; would vs will).
    - Time/number diff (noon vs afternoon; percentages; dates).
    - Domain-term spotter against glossary (burglary, riparian, trowel, cilantro).
  - Human-in-the-loop for high‑risk domains (legal, technical specs, incident reports).
  - Back‑translation diffing focused on: location/action nouns (windowsill, patio), puzzle mechanics/instructions.

## Confidence and Coverage
Confidence: medium-high. Evidence spans many judges and topics with consistent patterns (modality drift, domain synonym dilution, time/quantifier ambiguity, entity/gender slips, idiom flattening). Coverage limits: Hindi-specific annotations are inferred from back-translation rationales; some phenomena (tables/markup) had sparse negative evidence; meta/disclaimer issues appear rare.