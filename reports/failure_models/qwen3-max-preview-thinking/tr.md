# Failure Model — Qwen 3 Max Preview — Turkish

## Summary
Overall, Qwen 3 Max Preview produces fluent Turkish but shows recurring fidelity problems when round-tripped: semantic inversions, role/gender drift, and precision loss in domain terms and proper nouns. The model over-normalizes (time/units/register), substitutes near-synonyms that change facts, and occasionally hallucinates or drops small but meaning-critical details. Most severe issues concentrate in imagery-heavy prose and rule-like texts where a single noun, title, or exception clause anchors the meaning.

## Top Failure Modes
- Hallucinated agent or meaning inversion in key clauses — Description: Introduces a new subject/agent or flips a condition, rewriting the core image or rule. Why: Ambiguous Turkish mappings (implicit subjects, metaphor personification), plus over-aggressive paraphrasing on back-translation. Frequency: occasional; Severity: high. Evidence: “Frost traces …” becomes a person watching; moonlight exception inverted in a municipal rule.

- Gender/role drift (he↔she; titles/roles swapped) — Description: Character gender flips; official roles or relationships shift (Magistrate→Mayor; grandfather→father). Why: Turkish lacks gendered third-person pronouns and often omits subjects; weak entity state-tracking over multiple sentences. Frequency: common; Severity: high. Evidence: Protagonist gender repeatedly flipped; “Magistrate” surfaced as “Mayor” with month omitted.

- Proper noun and culturally bound phrase errors — Description: Misreads personified metaphors and place types; calques or genericization of named entities (Avenue↔Street; Statue of Liberty). Why: Literalization of metaphors/personifications in Turkish (“great copper lady”→“…Hanım”); cadde/sokak confusion; inadequate named-entity grounding. Frequency: common; Severity: high. Evidence: “Great copper lady” rendered as a named “Mrs. Bakır”; “Avenue” repeatedly changed to “Street.”

- Domain terminology dilution/substitution — Description: Replaces precise domain terms with near-neighbors, altering meaning (legal, technical, gaming, sports, culinary, ornithology). Why: Preference for frequent synonyms over domain-specific lexemes; gaps in Turkish term lexicons; back-translation picks different English heads. Frequency: common; Severity: medium–high. Evidence: “failover/replica”→“switchover/copy”; “Level Up”→“Skip Level”; football “three‑down back”→“three quarters”; species and cooking methods swapped (charred↔roasted).

- Object identity swaps on concrete nouns — Description: Tools/props misidentified (wrench→key; crowbar→saw), evidence labels changed. Why: Visual-semantic clustering and synonym bias; Turkish hyponyms/hypernyms conflated; sparse context anchoring. Frequency: occasional; Severity: high. Evidence: Key prop changed from wrench to key; forensic “crowbar” mistranslated as “saw.”

- Numbers/units and format normalization — Description: Units converted or misread; counts misinterpreted; time format changed 12h→24h; addresses altered. Why: Normalization heuristic in Turkish output; polysemy in numeric phrases; address term mapping. Frequency: occasional; Severity: medium. Evidence: “3 feet”→“3 meters”; “24 named cities”→“city named 24”; “Suite 400”→“400th Floor”; consistent 12h→24h conversions.

- Omission or weakening of limiting language — Description: Drops “only,” “up to,” months, or disclaimers; weakens risk (“fire”→“overheating”). Why: Compression during generation; Turkish often omits determiners/quantifiers; post-edit smoothing. Frequency: occasional; Severity: medium. Evidence: “up to” omitted on range; month “May” omitted; hazard “fire” softened.

- Antonym or polarity drift on key adjectives/virtues — Description: Swaps positive/negative attributes (kindness↔coarseness), flips sentiment. Why: Lexical proximity in embeddings; ambiguous Turkish roots; metaphorical context missed. Frequency: rare; Severity: high. Evidence: “kindness” became “coarseness,” altering a core character judgment.

## Language‑Specific Factors
- Gender-neutral pronouns and pro-drop: Turkish “o” and frequent subject omission lead to fragile gender/role recovery on back-translation, triggering he/she flips and role reassignment across sentences.
- Cadde vs. sokak and official titles: Turkish street/avenue distinctions and public-office lexicon (yargıç/hâkim, kaymakam, belediye başkanı, zabıta) invite mis-mapping to English titles.
- Personification and idioms: Turkish may literalize or honorific-ize metaphors (“great copper lady”→“…Hanım”), which returns as a spurious proper name.
- Agglutinative morphology: Case/possessive markers compact nuance that is easy to drop on RT (“only,” temporal/aspect cues), changing scope and conditions.
- Domain lexis scarcity: Ornithology, nautical ranks, American football schemes, and gaming UI terms have less standardized Turkish equivalents, increasing synonym drift on return.

## Numbers, Units, and Formatting
- Unit conversions/inventions: Feet→meters and added distances; “set of tokens”→singular; numeric labels reinterpreted (“24 named cities” misread).
- Time normalization: Systematic 12-hour→24-hour conversion; date/month omissions.
- Address/metadata errors: “Suite”→“Floor”; organization names adjusted; RFC-style MUST/SHOULD capitalization loosened, altering normative force.
- Tables/lists: Generally retained, but range qualifiers (“up to”) and counts sometimes drop, overstating specs.

## Meta/Disclaimer Behavior
- Minimal explicit safety/policy intrusions observed. One cluster shows omission of critics’ notes and label regularization rather than added disclaimers. Model rarely inserts meta text; primary contamination is factual drift, not policy verbiage.

## Recommendations
- Prompting
  - Lock terminology: “Use the provided glossary; preserve exact terms; do not substitute synonyms. Keep units, numbers, and capitalization as-is. Keep time format as source.”
  - Entity preservation: “Do not translate or rename proper nouns, titles, or button/skill names unless a standard Turkish equivalent exists. If unsure, keep the source in parentheses.”
  - Scope and polarity guardrails: “Do not add or remove words like only, up to, unless, except. Preserve exceptions and conditions verbatim.”
  - Gender hints: Where relevant, annotate entities with gender tags in source or prompt (“[FEMALE] Suri”) to stabilize pronouns on RT.
- Decoding
  - Use low temperature/top-p for high-fidelity modes; disable style optimization. Prefer constrained decoding with dictionary locks for critical terms.
- Data and fine-tuning
  - Augment with Turkish–English parallel corpora in targeted domains: legal/RFC (normative keywords), DevOps (failover/replica), sports playbooks, culinary/ornithology/forensics.
  - Train with termbase-aware objectives (glossary copy constraint) and gender/role coreference supervision over multi-sentence spans.
  - Include culturally bound metaphor/personification examples to avoid calquing into spurious proper names.
- Post-processing
  - Term/NER consistency checks: Align named entities, titles, and product/skill names to a whitelist; flag deviations.
  - Numeric and unit validator: Regex-based parity for all numerals, ranges, and units; block unintended conversions; enforce time-format parity.
  - Polarity/scope lint: Detect dropped/added limiters (only, up to, at most, unless) and exception inversions.
  - Tool/prop ontology checks: Validate critical nouns against context (e.g., tool inventories, evidence items).
  - Optional RT QA: Automatic back-translation diff with heuristic alerts for role/gender changes, antonyms, and condition flips.

## Confidence and Coverage
Confidence: medium-high. Evidence spans many judges and domains with consistent patterns (gender/role drift, term dilution, units/time normalization, proper-noun and tool identity errors). Coverage limits: Source texts are varied but skew to prose, technical, and rules; limited exposure to highly colloquial chat or code-mixed inputs. Meta/disclaimer behavior appears rare in this set and may be under-sampled.