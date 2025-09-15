# Failure Model — Qwen 3 Max Preview — Spanish

## Summary
Qwen 3 Max Preview shows strong overall fidelity but systematically “smooths” translations into generic Spanish, often sacrificing domain precision, register, and small factual details. The most salient weaknesses are: (1) terminology drift, especially for specialized nouns (data viz, theater, legal, technical, culinary, natural history); (2) register and modality shifts (“should”→“must”), and POV/quotation normalization; (3) improper handling of English singular they and role/titles; (4) subtle fact substitutions for near-neighbors (plant/species, furnishings, device types). Numbers/units are usually preserved, but time-of-day and named entities can drift.

## Top Failure Modes
- Terminology drift to generic synonyms — Consistent replacement of specific terms with broader or adjacent ones, reducing precision. Why: model favors high-frequency Spanish equivalents; weak domain term anchoring. Frequency: common; Severity: high.
  - Evidence: “stream graph” rendered as “flow chart,” changing the chart type; “solar awning” becomes “solar pergola,” altering the invention.
- Domain-confusable near-neighbors — Swaps between close-but-wrong items in specialized domains (theater, architecture, biology, art). Why: lexical neighborhood bias; insufficient domain grounding. Frequency: occasional; Severity: high.
  - Evidence: Theater “bank” unit becomes “bench”; “bay window” becomes “dormer window”; “semaphore” (artwork) becomes “traffic lights.”
- Register and modality hardening — Soft guidance becomes prescriptive and tone/formality drifts. Why: Spanish normative style bias; loss of modality nuance. Frequency: common; Severity: medium.
  - Evidence: “should”→“must” in policy/legal and consent contexts; business/legal tone softened by swapping terms (“checkout”→“payment,” “affordances”→“action indicators”).
- Pronoun and role/titles misalignment — Neutral “they/their” becomes gendered (“her”) or roles misidentified. Why: Spanish lacks widely accepted singular neutral; defaults to binary/gendered; role lexicon ambiguity. Frequency: occasional; Severity: medium.
  - Evidence: Neutral artist “their” changed to “her”; nautical “master” becomes “boatswain.”
- Quote/POV and tense normalization — Direct quotes rendered as reported speech; person and tense shift subtly. Why: stylistic normalization and paraphrase bias. Frequency: occasional; Severity: medium.
  - Evidence: A diary/quote turned from first-person direct to third-person paraphrase; “Level Up.” changed to second-person statement.
- Lexical specificity loss in natural history/culinary — Species and local-color nouns generalized or swapped. Why: low-frequency lexicon; training skew. Frequency: occasional; Severity: medium.
  - Evidence: “cotton-grass”→“milkweed,” “leatherleaf”→“bearberry”; “chowder”→“fish soup.”
- Time and small factual shifts — Time-of-day, minor spatial terms, and small procedural steps drift. Why: preference for common phrases; attention to local context slips. Frequency: occasional; Severity: low–medium.
  - Evidence: “late afternoon”→“late evening”; omitted “spotting” in safety procedure; “blossom end”→“calyx end” for harvest test.

## Language‑Specific Factors
- Singular “they” mapping: Spanish lacks a mainstream singular gender-neutral pronoun; the model often chooses gendered “él/ella,” altering identity or inclusivity.
- Modality/register: English modal nuance (“should,” “may”) tends to become prescriptive or more formal in Spanish, shifting legal/policy meaning.
- Legal term partitions: English “burglary/robbery/theft” distinctions often collapsed to “robo/hurto,” risking legal inaccuracy.
- Specialized jargon availability: Some English domain terms (“stream graph,” specific theatrical “bank,” UI “checkout,” HCI “affordances”) lack entrenched Spanish equivalents; model picks broader terms.
- Idiom/poetic density: Spanish reformulations often flatten idiom and imagery, losing rhythm and metaphorical specificity.

## Numbers, Units, and Formatting
- Generally preserved; few errors in magnitudes or units.
- Conversions/inventions: Rare; more common are temporal shifts (“evening”↔“afternoon”) or list/heading label synonymization.
- Formatting: Occasional rewording of headings (“Goals”→“Objectives”), and game/system UI terms (“Level Up.”→“You leveled up.”). Tables/markup not a prominent failure in the evidence.

## Meta/Disclaimer Behavior
- Minimal to none. A few “disclaimer/meta” tags appear, but no consistent safety notices or authorial intrusions contaminating translations.

## Recommendations
- Prompt patterns
  - Instruct: “Translate with maximal terminological fidelity; do not paraphrase; preserve proper names, headings, quotes, and modality. If a term is uncertain, retain the English term in parentheses.”
  - Provide a termbase/glossary and a do-not-translate list for product names, chart types, theater terms, legal categories, species, and UI strings (e.g., stream graph, checkout, endpoint, capstone, calf binding).
  - Specify pronoun and register policy: “Maintain gender neutrality via periphrasis/passives/second person” or specify a locale-approved form (e.g., neutral constructions or “elle” if acceptable).
  - Add checks: “Flag items with low-confidence domain terms; list any terms left in English.”
- Decoding changes
  - Lower temperature/top-p to reduce paraphrasing; enable length/faithfulness constraints.
  - Use constrained decoding with terminology dictionaries for critical domains.
- Data augmentations
  - Fine-tune with high-quality EN↔ES parallel corpora in targeted domains: data visualization, theater/performing arts, architecture, conservation biology, culinary/regional, HCI/product, e-commerce, legal.
  - Include curated mappings for legal offense taxonomy, theater stagecraft, data viz chart taxonomy, and specialized material culture (bindings, furnishings).
  - Augment with style-preservation sets (quotes, POV, modality, time expressions).
- Fine-tuning targets
  - Penalize synonym swaps on key terms; reward exact term preservation.
  - Train singular-they handling strategies in Spanish via neutral rewrites.
  - Reinforce modality calibration (“should/may/can” mappings); preserve imperative vs advisory tones.
- Post-processing checks
  - Terminology QA: NER and term alignment validator to detect drift (e.g., stream graph vs flow chart).
  - Pronoun/gender audit: Compare entity references; flag unexpected gendering.
  - Register/modality lint: Detect “should”→“must” hardening.
  - Domain-specific validators: species name consistency, architecture/furnishing types, theater lexicon, chart taxonomy, UI labels (“Checkout”).
  - Quote/POV preservation check: Ensure direct quotes remain direct; flag person/tense changes.
  - Optional bilingual review for high-risk domains.

## Confidence and Coverage
- Confidence: medium-high for the identified patterns; many independent judges converge on the same families (terminology drift, register shifts, pronoun gendering, near-neighbor substitutions).
- Coverage limits: Evidence skews toward short to medium texts across varied domains; fewer examples of numeric/unit failures and formatting-heavy content. Observations are specific to Spanish outputs of Qwen 3 Max Preview in round-trip settings and may differ by locale or with glossary-constrained workflows.