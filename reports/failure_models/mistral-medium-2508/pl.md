# Failure Model — Mistral Medium 3.1 — Polish

## Summary
Overall, the model delivers fluent Polish but shows a recurring tendency to domesticate and paraphrase, which causes factual drift. The most salient weaknesses are: (1) substitution of semantically adjacent but incorrect nouns/terms (species, tools, materials, roles), (2) polarity/direction flips in technical and safety content, (3) unit/time “normalization” and formatting changes, (4) tonal flattening toward formal/neutral Polish and proverb/idiom replacement, and (5) occasional meta insertions and translator notes. Poems and domain‑dense texts (culinary, outdoors/safety, material culture, sports play‑by‑play) are disproportionately affected.

## Top Failure Modes
- Lexical near‑synonym and entity substitution — Replaces precise terms with adjacent but wrong items (birds, fish, tools, materials, roles, places). Symptoms: curlew↔snipe, sea bass↔cod, earthenware↔stonepaste, micro‑spikes↔snowshoes, trowel↔planter; job roles and committee names swapped. Why: preference for high‑frequency Polish equivalents, weak disambiguation on low‑resource domain lexicons, and domestication bias. Frequency: common. Severity: high.
  - Evidence: “sea bass → cod; macerated → pickled” in menu; “micro‑spikes → snowshoes; Sentinel Dome misnamed” in hiking guide.

- Polarity/direction/category flips — Safety/technical flips that invert meaning (combustible↔flammable; nonhazardous↔hazardous; onshore↔offshore; light goes off = activates ↔ turns off). Why: polysemy of English phrasals and category terms; Polish lexical gaps; insufficient grounding. Frequency: occasional. Severity: high.
  - Evidence: “nonhazardous spill → hazardous” and “combustible → flammable”; “onshore winds → offshore.”

- Poetic imagery and proper‑name misreadings — Key images or capitalized common nouns misinterpreted; rhyme scheme altered; added translator asides. Why: poetic latitude + ambiguity resolution toward creative paraphrase; sparse parallel poetry data; capitalization ambiguity (“Frost” season vs name). Frequency: occasional. Severity: medium to high.
  - Evidence: “Frost → squadron,” “dusted pink → pink with pollen,” plus added translator note; “breathless → soulless,” “rime → shard,” rhyme broken.

- Tone domestication and idiom replacement — Shifts to formal/neutral Polish; replaces culture‑specific sayings with different Polish proverbs; marketing/legal diction softened/hardened. Why: decoding preference for standard register; alignment training penalizing slang; search for “equivalence” over fidelity. Frequency: common. Severity: medium.
  - Evidence: gardening folklore swapped for a different saying; “shall” legal force flattened; sportscaster voice dulled.

- Unit/time/format normalization — Converts imperial→metric, adds Fahrenheit, switches 12h→24h time, alters tables/labels. Why: localization bias in training data and heuristic “helpfulness.” Frequency: common. Severity: medium.
  - Evidence: feet→meters and added “(Fahrenheit)”; 24‑hour time substitution in event notice.

- Role/agency confusion in action sequences — Misassigns who acts on whom in sports or narrative (passer vs receiver, defender label, who insisted/served). Why: Polish free word order and dropped pronouns increase ambiguity; loose rephrasing breaks coreference. Frequency: occasional. Severity: medium.
  - Evidence: inbound recipient vs thrower swapped; “Thomas insisted on serving” became “Margaret insisted on opening.”

- Technical‑term generalization — Replaces domain jargon with generic terms (IT, legal, theater, geology). Why: term coverage gaps; preference for common synonyms. Frequency: common. Severity: medium.
  - Evidence: “likelihood ratio → probability ratio”; “failover/standby → switch/backup”; “minute order → minutes of hearing.”

- Proper‑noun and title drift — Names of committees, bosses, skills, routes, books altered; capitalization and morphology nudges. Why: hallucinated normalization and inflectional adaptation. Frequency: occasional. Severity: medium.
  - Evidence: board/committee names changed; “Travels” retitled “Embassy”; game abilities/items renamed.

- Spurious meta/disclaimer insertions — Adds “Translation:” headings or translator notes. Why: mixture-of-format training examples; over‑explanation bias. Frequency: occasional. Severity: low.
  - Evidence: leading “Translation:” tag; translator aside about “shard” choice.

## Language‑Specific Factors
- Rich inflection and gender: Polish gendered forms encourage unintended gendering (council member→councilwoman) and folklore gender flips (doe→male). Case inflections can obscure agent/patient roles in free word order, contributing to role swaps in play‑by‑plays.
- Lexical gaps and near‑synonyms: Multiple Polish options for English terms with subtle distinctions (combustible/flammable; blanching/blooming; trowel types) lead to genericization or wrong branch selection.
- False friends/orthography: fiolka (vial) vs fiołek (violet); bank (brzeg skarpy) vs bank (financial). Missing diacritics or frequency bias can push to the wrong lemma.
- Idiom domestication: Tendency to replace source sayings with culturally common Polish proverbs rather than literal equivalents, especially in gardening/folklore and promotional copy.
- Poetic meter/rhyme: Polish morphology lengthens words; the model compensates by paraphrasing imagery, harming fidelity and rhyme schemes.

## Numbers, Units, and Formatting
- Unit conversions: Feet→meters, inches→centimeters, Fahrenheit added; sometimes alters perceived magnitude or style. Conversions are not requested and occasionally inconsistent across a text.
- Time format: 12h→24h switches; date/time normalization to Polish conventions.
- Instrument/measure mislabels: “goes off” semantics flipped; “bank” and “storage/memory” swapped; occasional table/label renaming.
- No systematic numeral hallucination observed, but local additions like “100 kg” occur sporadically.

## Meta/Disclaimer Behavior
- Adds “Translation:” header or translator notes, especially in poems or when uncertain about a term choice.
- No safety-policy boilerplate, but occasional explanatory asides contaminate output when the source is figurative or ambiguous.

## Recommendations
- Prompting
  - Enforce fidelity: “Translate into Polish. Preserve all technical terms, units, names, and register. Do not add notes, headings, or explanations. Keep units and time formats exactly as in the source.”
  - Add guardrails per genre: for poetry “Preserve imagery and key nouns; do not domesticate proverbs; do not add rhymes if absent.” For safety/technical “Do not convert units; copy hazard classes verbatim.”
  - Use inline glossaries/Do‑Not‑Translate lists for domain terms, species, gear, materials, legal/IT jargon, and proper nouns.

  Example constraints:
  - Do‑Not‑Translate: product/route/skill names, committee titles.
  - Copy‑exact tokens: units (ft, in, °F), hazard terms (Combustible, Nonhazardous), wind directions (onshore/offshore).

- Decoding
  - Lower temperature and reduce top‑p for technical/safety/legal to curb paraphrasing.
  - Use constrained decoding for named entities and units (regex copy constraints).
  - Apply domain‑aware biasing (vocabulary boosting) for critical lexicons (ornithology, culinary species, materials, theater/forensics).

- Data and fine‑tuning
  - Fine‑tune on high‑fidelity EN–PL parallel corpora in target domains: EHS/SDS sheets, outdoor safety guides, museum/object cataloging, forensic SOPs, sports recaps.
  - Add contrastive pairs emphasizing minimal pairs (combustible vs flammable; onshore vs offshore) and phrasal verbs (goes off = activates).
  - Curate poetry parallel data with annotations linking key image-bearing nouns; penalize content-substituting paraphrases.

- Post‑processing QA
  - Automated checks:
    - Polarity/direction diff: detect antonym flips for hazard classes, directions, light on/off semantics.
    - Unit/style checker: flag unintended unit/time conversions; enforce original style.
    - Named‑entity matcher: Levenshtein/NER to ensure proper nouns are copied (within Polish inflection rules).
    - Domain lexicon validator: species/tools/materials against whitelist.
  - Human-in-the-loop spot checks for poems and safety-critical texts.

- Workflow
  - Two‑pass translation: literal pass with locked terms, followed by stylistic smoothing pass with a diff guard to prevent content changes.
  - Provide the model with a short source glossary per document (extracted terms) to anchor choices.

## Confidence and Coverage
- Confidence: medium. Error patterns are consistent across many judges and genres, with multiple independent reports reinforcing key modes (term substitution, polarity flips, units, tone). 
- Coverage limits: Evidence skews toward specific domains (poetry, culinary, outdoors/safety, legal/IT); fewer examples for conversational or highly technical STEM formulas. Some attributions (language‑specific roots) are inferred from recurring Polish misselections rather than directly evidenced in every case.