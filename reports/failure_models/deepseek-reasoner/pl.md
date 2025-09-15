# Failure Model — DeepSeek V3.1 Reasoner — Polish

## Summary
Overall, DeepSeek V3.1 Reasoner produces fluent Polish but shows systematic drift away from source‑exactness when mapping specialized terms, idioms, and registers. The most salient weaknesses are: (1) domain‑critical term swaps in safety, legal, technical, and niche jargon; (2) tone/register flattening with impersonalization and generic synonyms; (3) idiom/proverb replacement and metaphor reshaping; (4) proper‑noun and class/name changes in fictional/game contexts; (5) occasional numbers/units/date and metadata perturbations. These issues are amplified by Polish‑specific polysemy (e.g., klucz = key/wrench), domestication tendencies (jurisdiction, time formats), and gender/pronominal pressures.

## Top Failure Modes
- Hazard and policy term inversions — description: flips or blurs regulated/safety categories (combustible vs flammable; nonhazardous vs hazardous), or narrows/broadens policy scope (citywide congestion pricing → city center; equity → equality). Why: Polish lexical crowding for safety terms (palny/łatwopalny/niepalny), negation handling, and preference for “closest common” terms; insufficient domain anchoring. Frequency: occasional. Severity: high.
  - Evidence: safety sheet reverses disposal class; congestion pricing scope narrowed and “means‑tested rebates” muddled.

- Domain jargon to generic synonym — description: consistent replacement of precise terms with broader or nearby synonyms across legal, technical, culinary, nautical, archaeology, finance, and CBT/therapy language. Why: term disambiguation bias toward frequency; limited parallel exposure for exact Polish→English back mapping; decoding favors fluency over terminological fidelity. Frequency: common. Severity: medium.
  - Evidence: “severability/opt‑out/carve‑out” softened (enforceability/waiver); “standby replica/replication lag” → generic “backup/delay”; “blanching” → “matting”; “binnacle lamp” → “quarter gallery lamp”.

- Polysemy traps and false friends (Polish↔English) — description: key items misrendered due to ambiguous Polish lemmas or near‑neighbors: klucz (wrench) → key; mulcz → bark; galette/disk → cake; tassels → pompoms; trowel → watering can; saladette → cocktail. Why: Polish lexemes with multiple everyday senses; training data bias toward consumer wording (e.g., bark mulch in PL retail); lack of in‑domain constraints. Frequency: common. Severity: medium.
  - Evidence: repeated wrench→key; mulch rendered as bark across retail flyer; gardening tool “trowel” turned into watering can twice.

- Idiom/proverb and figurative reshaping — description: replaces idioms with unrelated proverbs; adjusts metaphors/rhyme or poetic images; changes set phrases in slogans/taglines. Why: domestication strategy and paraphrase pressure; poetic register sparsity; rhyme/imagery preservation not optimized. Frequency: occasional. Severity: medium.
  - Evidence: closing proverb swapped for different one; poem’s “embrace” → “sprawl,” rhyme and analysis drift.

- Tone/register drift to impersonal and flatter style — description: shifts second‑person to impersonal “one,” softens or formalizes voice, or intensifies rhetoric; commands become passive. Why: Polish normative formal style; decoder penalizes colloquial or imperative density; preference for register smoothing. Frequency: common. Severity: low–medium.
  - Evidence: instructionals converted to impersonal voice; CBT, legal and corporate texts lose “of‑art” terms in favor of generic diction.

- Named entities, classes, and proper nouns altered — description: fantasy/game classes, abilities, locations and nautical ranks/names changed or normalized; plural/singular contract entities drift. Why: hallucination toward familiar templates; weak name‑copy bias without constraints; morphological agreement leads to normalization. Frequency: occasional. Severity: medium.
  - Evidence: Adept→Apprentice; Wight Lord→Lichlord; “Buyer(s)” number inconsistently toggled; “Master, Mr. Whitaker” → “Captain Whitaker”.

- Temporal/locale and metadata normalization — description: 12h/24h toggles, header dates miscopied, addresses localized (Voivodeship), placeholders left in Polish, or English expected fields renamed. Why: locale adaptation heuristic; format inference; insufficient preservation constraints. Frequency: occasional. Severity: low–medium.
  - Evidence: date 11→10 Sept; “State ZIP” → “Voivodeship Postal Code”; untranslated Polish placeholders in otherwise English return.

- Numbers/units/labels perturbation — description: label/category swaps (sigils → call numbers), frequency misstatement (“1 in 1000”), added parenthetical unit notes, inventory vs accession numbers, traffic engineering term mix‑ups (“left‑turn storage” → “overlap”). Why: label lexicalization bias; measurement zeal to “clarify”; low exposure to niche taxonomies. Frequency: occasional. Severity: medium.
  - Evidence: fantasy “sigils” normalized to cataloging “call numbers”; left‑turn storage misrendered twice; redundant unit glosses added.

- Gender/pronoun shifts — description: neutral/unspecified entities rendered as masculine; inclusivity lost. Why: Polish grammatical gender pressure and back‑mapping defaults; lack of neutral strategies. Frequency: occasional. Severity: low–medium.
  - Evidence: neutral “their” → “his”; press quote slightly altered along with pronouns.

## Language‑Specific Factors
- Polysemy and homonymy: klucz (key/wrench), zamek (lock/castle/zip), sklejka/klej, piła/saw; frequent cause of tool/part mistranslations.
- Gendered grammar: Polish pushes explicit gender; back‑translation often reverts to masculine where source is neutral.
- Register norms: Polish favors impersonal/passive in formal texts, then back‑translates as “one should,” softening direct imperatives.
- Locale domestication: tendency to localize jurisdictions (FOIA→Voivodeship Act), addresses, and time formats (24h), then not restore original scope/format.
- Fixed phrases/idioms: pressure to swap to common Polish equivalents, then back‑translate to a different English proverb or weakened metaphor.
- Terminology scarcity: niche domains (archaeology, nautical, CBT therapy, fantasy RPG) have low‑frequency Polish alignments; the model generalizes to common neighbors.

## Numbers, Units, and Formatting
- Label/category drift: “sigils” misread as “call numbers”; “Accession Number” → “Inventory number.”
- Frequency/ratio distortions: a stated rate changed to “1 in 1000.”
- Time/date normalization: switches 12h↔24h; header date altered; “by” vs “until” in provenance ranges.
- Traffic/eng terms: “left‑turn storage” → “left‑turn overlap” (phasing vs geometry); “lane”→“route.”
- Unit meddling: added parenthetical clarifications and redundant repeats; slight mis‑conversions not observed, but invention/over‑explanation appears.
- Placeholders/markup: Polish placeholders persisted in English back‑translation; occasional added headings (“We Recommend”).

## Meta/Disclaimer Behavior
- Occasional authorial intrusions: added parenthetical unit notes or translator‑style glosses; added rating preface (“We Recommend”); sporadic translator notes like “(draw).”
- Triggers: product/offer lists, analyst notes, sports scoring, and research abstracts where the model “clarifies” or editorializes.

## Recommendations
- Prompting
  - Enforce terminology: “Use the provided glossary exactly; do not substitute synonyms. Do not localize jurisdictions, dates, or time formats. Preserve named entities, classes, skill names, and labels verbatim.”
  - Anti‑domestication: “Do not replace idioms/proverbs or slogans—translate literally unless a standard published equivalent exists; if unsure, retain original in quotes.”
  - Safety/regulated: “Do not alter hazard classes or disposal categories; copy terms like ‘combustible,’ ‘flammable,’ ‘nonhazardous’ exactly.”
  - Register control: “Preserve person (you vs one), voice (active/imperative), and tone. Avoid impersonal constructions unless present in the source.”
  - Gender: “Maintain neutral pronouns; if Polish forces gender, annotate with neutral markers and back‑translate to neutral.”
- Decoding
  - Lower temperature and stronger length penalty on constrained segments; apply constrained decoding for glossary/NER spans.
  - Use a “copy‑bias” for capitalized tokens, all‑caps labels, class names, and numbers/units.
- Data/Fine‑tuning
  - Add high‑precision parallel corpora for: safety SDS, legal ToS (opt‑out, severability), archaeology/nautical catalogs, traffic engineering, CBT therapy, culinary techniques, fantasy/RPG manuals.
  - Hard negative training on Polish polysemy sets (klucz, zamek, tama/wał, pompony/chwosty, mulcz/kora), and on idiom/proverb preservation.
  - Include locale‑reversal tasks to discourage jurisdiction/time/address domestication.
- Post‑processing
  - Terminology validator: regex/term lists to flag swapped terms (hazard classes, legal clauses, database labels, traffic phasing).
  - NER consistency check: forbid edits to proper nouns, class/ability names, SKUs, accessions.
  - Numbers/dates audit: ensure exact match of numerals, rates, times, ranges, and header dates; block 12h/24h flips unless specified.
  - Style lints: detect you→one shifts; passive inflation; proverb/idiom substitutions.
  - Gender neutrality pass: restore neutral pronouns in English back‑translation.

## Confidence and Coverage
- Confidence: medium. The failure modes recur across many low‑scored cases with agreement among multiple judges; a few severe safety/policy inversions appear clearly.
- Coverage limits: Evidence skews toward literary, legal, technical, and product texts; fewer scientific unit conversions observed. Some judgments come from different evaluators with varying strictness; no access to full source/target pairs—analysis relies on paraphrased rationales.