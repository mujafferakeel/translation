# Failure Model — Kimi K2-0905 — Russian

## Summary
Overall, Kimi K2-0905 produces fluent Russian but shows systematic meaning drift in domain-specific terminology, normative modality, and proper nouns. The model often “hardens” soft requirements (SHOULD→MUST), substitutes precise terms with generic or near-synonymous Russian words (especially legal, technical, and sports theatre jargon), and alters named entities or specialty nouns. Occasional polarity flips (negations, thresholds, food insecurity/security) and number/date slips occur. In literary content, meter/imagery are misanalyzed or flattened. Gender defaults to masculine in neutral contexts. Most errors reflect Russian-specific modality/gender pressures plus insufficient domain glossaries.

## Top Failure Modes
- Normative hardening (SHOULD→MUST) — description: Soft recommendations become obligations, altering compliance and policy strength; why: Russian modality mapping (“следует/рекомендуется” vs “должен/обязан”) and a preference for assertive style; frequency: common; severity: high; evidence: “SHOULD repeatedly rendered as MUST in spec”; “suggested action became mandatory.”
- Domain-term drift/substitution — description: Precise terms replaced by near-misses, changing instructions or technical meaning (law, patents, theatre, DevOps, horticulture, sports); why: inadequate term grounding and Russian synonym pressure; frequency: common; severity: medium-high; evidence: “exclusive venue→exclusive jurisdiction”; “prior art→prototype; examiners→experts”; “failover/control planes→switch/control panels”; “through ball→lofted pass”; “blossom-end rot→brown spot end.”
- Proper noun and label corruption — description: Names and canonical terms altered or normalized; why: weak entity anchoring, transliteration/orthographic uncertainty; frequency: common; severity: medium-high; evidence: “Wight Lord→Vampire Lord; ability/quest names changed”; “Vale→Veil; Aether→Ether”; “Calendra/Kalendra misspelling.”
- Polarity/threshold inversion — description: Negation/comparator flips or antonyms that reverse conditions; why: misreading comparative cues, Russian negation scope; frequency: occasional; severity: high; evidence: “block if <30 days to expiry→older than 30 days”; “food insecurity→food security.”
- Number/date/category slips — description: Ordinal/quantity errors, singular↔plural, hazard-class drift; why: numeral casing and category mapping errors; frequency: occasional; severity: medium; evidence: “seventeenth→seventh”; “pocket watch→pocket watches”; “combustible→flammable.”
- Gender/pronoun overcommitment — description: Neutral references become masculine in Russian; why: Russian requires gendered forms; frequency: occasional; severity: medium; evidence: “they/their artist→his/he”; neutral roles gendered.
- Literary prosody and imagery distortion — description: Metrical terms reversed, imagery misread, or poetic diction flattened; why: weak metrical knowledge and literalism; frequency: occasional; severity: medium; evidence: “trochee vs iambic reversed; rhyme examples off”; “poem images diluted (‘sentinels of chance’ mistranslated).”
- Concrete noun/term misselection within domains — description: Specific items substituted (eye vs nasal drops; species; culinary cuts/techniques); why: close lexical neighbors and low frequency items; frequency: occasional; severity: medium; evidence: “eye drops→nasal drops”; “stone loach→lamprey”; “charred→grilled; lamb shoulder omitted.”

## Language‑Specific Factors
- Modality mapping: English SHOULD/RECOMMENDED vs Russian следует/рекомендуется vs должен/обязан. Model gravitates to stronger “должен,” causing requirement hardening.
- Gender and number: Russian enforces gendered adjectives/participles; neutral “they” often forced to masculine, and neutral roles become gendered.
- Legal/procedural lexicon: “Venue” vs “jurisdiction” (подсудность/место рассмотрения vs юрисдикция); “prior art” (уровень техники) vs “prototype”; small shifts change legal meaning.
- Terminology without direct calques: Theatre “bank/rake,” sports “overlap/trap,” DevOps “failover/control plane,” horticulture “blossom-end rot” (вершинная гниль плодов) — the model defaults to generic Russian.
- Poetic meter/terms: “Iamb/trochee,” rhyme pattern names, and idioms are fragile; Russian metrical metalanguage is specialized and often mishandled.

## Numbers, Units, and Formatting
- Ordinal/date errors and singular/plural drift: 17th→7th; singular “pocket watch”→plural.
- Hazard/category misclassification: “Combustible liquid”→“Flammable liquid.”
- Comparator/threshold inversion: “<30 days” became “older than 30 days.”
- Heading and metadata shifts: Normative keyword casing and section headers occasionally reworded (“Minor Comments”→“Additional comments”).
- No systematic unit conversion problems, but occasional category renaming (engine→pump unit).

## Meta/Disclaimer Behavior
- Rare authorial intrusions or additions: Occasional minor additions (“Ba-ang” sound), softened or amplified tone; no persistent safety disclaimers. Triggered by narrative/creative inputs or when trying to “polish” style.

## Recommendations
- Prompting
  - Lock modality: “Translate normative keywords verbatim: MUST=должен, SHOULD=следует/рекомендуется, MAY=можно. Do not strengthen or weaken modality.”
  - Terminology guardrails: Provide per-document glossary (e.g., prior art=уровень техники; venue=место рассмотрения; failover=аварийное переключение; blossom-end rot=вершинная гниль плодов; overlap=забегание/рывок вразрез в зависимости от контекста).
  - Entity fidelity: “Preserve proper nouns exactly; if uncertain, keep original Latin spelling in parentheses.”
  - Negation/comparison check: “Preserve signs and thresholds (<, >, before/after).”
  - Gender neutrality: “Avoid assigning gender unless explicit; use role nouns or plural constructions to stay neutral.”
- Decoding
  - Lower temperature/top-p to reduce synonym drift.
  - Bias/penalty toward copying uppercase normative tokens (MUST/SHOULD) and numerals.
- Data
  - Fine-tune on EN↔RU parallel corpora featuring standards (RFCs, legal contracts), technical docs (cloud/DevOps), sports/theatre manuals, horticulture guides, and poetry with annotated meter.
  - Terminology dictionaries for law (venue/jurisdiction), IP (prior art), stagecraft, football tactics, horticulture pathology.
  - Add contrastive pairs for SHOULD vs MUST; negation and comparator minimal pairs; date/ordinal disambiguation.
- Fine-tuning targets
  - Loss-weight boosts on named entities, numerals, comparatives, and normative tokens.
  - Evaluation tasks on polarity/threshold preservation and modality classification.
- Post‑processing
  - Diff checks for normative keywords; flag any SHOULD/RECOMMENDED strengthened to MUST.
  - Numeral/date validator (ordinals, < vs >, counts, singular/plural) with rule-based alerts.
  - Named-entity consistency checker against source.
  - Domain glossary validator to detect forbidden substitutions.
  - Style filter for gendered forms when source is neutral.

## Confidence and Coverage
Moderate confidence. Evidence spans many domains and judges, with consistent signals on modality hardening, term drift, named-entity changes, and occasional polarity/number errors. Literary meter and imagery issues appear but with fewer samples. Limited direct visibility into raw Russian outputs constrains fine-grained morphology analysis; recommendations emphasize high-frequency, high-severity patterns observed across the dataset.