# Failure Model — Gemini 2.5 Pro — Russian

## Summary
The model’s Russian round‑trip translations are generally faithful but show systematic drift on domain terms and proper nouns, frequent register flattening toward formal/verbose Russian, and instability with gender and UI/label text. The most salient weaknesses are: (1) replacement of terms‑of‑art with generic synonyms, (2) corruption of names/labels/abbreviations, (3) tone/register shifts (colloquial/poetic/legalistic → neutral/formal), (4) gender and inclusivity loss, and (5) occasional unit/format conversions and small factual swaps.

## Top Failure Modes
- Term-of-art to generic synonym drift — Domain jargon (legal, technical, gaming, arts) is replaced with near‑synonyms that lose precision (e.g., “Chain of Custody,” “runbook,” “rolled rim,” “living hinge,” game stats/skills).
  - Why: Preference for common Russian collocations; limited domain-specific lexicon; over-regularization to “safe” wording.
  - Frequency: Common
  - Severity: High in technical/legal/gaming content
  - Evidence: “HP/MP, perks, attributes” rendered as different letters/words; “Stage Management” → “Assistant Director”; “runbook” → “instruction.”

- Proper noun and label instability — Person/variety/species/product names and UI strings are altered (spelling, gender, species, titles), and abbreviations are expanded or swapped.
  - Why: Hallucinated normalization to Russian norms; casing/inflection pressures; low penalty for surface form changes.
  - Frequency: Common
  - Severity: High when names/labels are keys
  - Evidence: “Lysander/Demetrius” → feminine forms; “Ember” → “Amber”; button “Checkout” → “Place Order.”

- Tone/register flattening and idiom loss — Colloquial, journalistic, poetic, or archaic/legalistic styles are softened into formal, wordier Russian; puns/wordplay are dropped.
  - Why: Training bias toward expository Russian; lengthening and nominalization tendencies; difficulty preserving idioms/puns cross‑lingually.
  - Frequency: Common
  - Severity: Medium (High in marketing/poetry/legal)
  - Evidence: Archaic/official “Whereas,” screenwriting/legal jargon, and puns (“Spring into savings”) become generic/formal phrasing.

- Gender and inclusivity errors — Gender‑neutral “they” becomes masculine; inanimate/gender‑neutral references get gendered; statue/object gendered; partner pronouns masculinized.
  - Why: Russian grammatical gender forces choices; defaulting to masculine; lack of targeted guidance for neutral renderings.
  - Frequency: Occasional (repeats across samples)
  - Severity: Medium (High for DEI-sensitive content)
  - Evidence: “they” → “he” across music/HR content; partner changed to “his”; neutral statue described with gender.

- Small factual swaps within a domain — Specific items within a taxonomy or part name change to a nearby category (species, materials, soil textures, auto body parts, sports terms).
  - Why: Semantic neighborhood confusion; insufficient anchoring to glossaries; retrieval of more common near‑neighbors.
  - Frequency: Occasional
  - Severity: Medium–High when details are critical
  - Evidence: “red knot” → different sandpiper; “earthenware with quartz temper” → “faience”; “quarter panel” → “rear right fender”; cricket “runs” → “points.”

- UI/metrics/units formatting drift — Units localized or converted; time formats changed; button/heading text paraphrased; store names/feet lost.
  - Why: Over‑eager localization; stylistic normalization of headings; cultural substitution.
  - Frequency: Occasional
  - Severity: Medium
  - Evidence: “km” → “км”; 24‑hour clock substitution; “three feet/CVS” dropped; headings/section titles reworded.

## Language‑Specific Factors
- Gendered grammar pressure: Russian requires gender in many contexts, nudging the model to choose masculine defaults and to gender inanimate referents; maintaining neutrality requires deliberate strategies (plural/periphrasis).
- Calques and Russian media register: Russian often prefers formal nouns over English verbs/idioms, pushing “town hall” to “общий сбор,” “workshop” to “мастер‑класс,” “CEO” to “гендиректор,” “downtown” to “центр города.”
- Abbreviation ambiguity: English gaming and technical acronyms (HP/MP, DEX/WIS) are unstable in Russian; the model invents Cyrillic letter substitutes or re-expands with different semantics.
- Terminology false friends: “Workshop”→“мастер‑класс,” “resilient”→“sustainable,” “rolled rim”→“beaded,” “microfiction”→“микропроза,” “capital improvement”→“инвестиция” reflect common Russian near‑synonyms that miss the term‑of‑art.
- Word order and style: Russian allows freer order and often nominalizes verbs; the model leans into this, increasing verbosity and softening punchy English prose.

## Numbers, Units, and Formatting
- Units/time localization: Switching km→км, 12→24‑hour time, losing imperial references (feet), and soft renaming of retail references (CVS).
- UI strings and headings: Button text paraphrased to culturally typical labels (“Checkout” → “Place Order/Оформить”), headings normalized.
- Tables/labels: Section titles and metric names rephrased; acronym expansions added as parentheticals.
- Conversions/inventions: Rare hard conversions, but occasional invention of added terms (“proton” in instrument name).

## Meta/Disclaimer Behavior
- Minimal safety/meta intrusion overall. Occasionally adds explanatory parentheticals or usage glosses in definitions/notes, likely from helpfulness bias rather than safety triggers.

## Recommendations
- Prompting
  - Enforce invariants: “Do not change proper nouns, product/variety names, UI strings, abbreviations, or numbers; copy labels verbatim.” Provide a [LOCKED TERMS] list.
  - Register control: “Preserve original tone/register (colloquial/poetic/legal/archaic). Avoid added verbosity or nominalizations.”
  - Gender handling: “Maintain gender neutrality; prefer plural or role nouns. Do not assume gender.”
  - Units/UI: “Do not localize units/time or retail names. Do not paraphrase buttons/headings.”
  - Domain hints: “Use domain‑specific jargon as in source (gaming stats, legal terms, materials, sports).”
- Decoding
  - Lower temperature/top‑p to reduce synonym drift; enable constrained decoding for [LOCKED TERMS], names, and UI keys.
  - Use glossary‑constrained beam search where available (lexicon of legal/game/technical terms).
- Data
  - Augment with high‑quality EN↔RU parallel corpora for: game UI/mechanics, legal contracts, manufacturing specs, sports journalism, bird taxonomy, ceramics/materials, auto repair.
  - Add counter‑examples emphasizing gender‑neutral renderings and preservation of button labels/abbreviations.
- Fine‑tuning targets
  - Penalize synonym substitutions for terms‑of‑art and UI labels; reward retention of names/abbreviations and exact numerals/units.
  - Style adapters for legalese/archaic, journalistic brevity, and poetic concision in Russian.
- Post‑processing
  - Automated checks: Named entity and label alignment; acronym consistency; numbers/units/time diffs; UI string equality.
  - Domain validators: Bird/species/materials lookup; auto body part glossary; sports term mappings; gaming stat/perk lists.
  - Back‑translation diffing to flag tone/register drift and label changes for human review.

## Confidence and Coverage
- Confidence: Medium‑high. Patterns recur across many judges and domains with consistent symptoms (term drift, names, register, gender, units).
- Coverage limits: Evidence skews toward short/medium texts with diverse domains; fewer examples of code/tables. Safety/meta issues appear rare; conclusions there are low‑confidence.