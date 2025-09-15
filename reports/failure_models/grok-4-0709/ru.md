# Failure Model — Grok 4 — Russian

## Summary
Grok 4 (ru) typically preserves core meaning but shows consistent erosion of nuance and register, with occasional high‑impact failures. The most salient weaknesses are: (1) modality drift (MUST/SHOULD swaps) in specs and legal prose, (2) replacement of domain‑specific terminology with generic synonyms across many fields, (3) lexical false friends and polysemy errors that flip meanings, (4) tone flattening and stylistic domestication (poetic/archaic/legal/jargon), and (5) sporadic catastrophic meta/refusal outputs. There are also smaller but recurring issues with species/proper names, singular/plural shifts, and minor metadata/heading changes.

## Top Failure Modes
- Catastrophic meta/refusal output — The back‑translation is an outright refusal/meta message instead of a translation; meaning is entirely lost. Why: safety/policy trigger or prompt confusion about task stage. Frequency: rare. Severity: high. Evidence: “Back-translation is a refusal, unrelated to the incident report”; “a refusal to perform the task.”
- Modality inflation/deflation — SHOULD↔MUST swaps (and SHALL/SHOULD softening) change normative force in specs/legal/instructions. Why: ambiguous Russian mappings (должен vs следует), model preference for stronger/clearer modals. Frequency: common. Severity: medium–high. Evidence: “SHOULD to MUST alters requirements”; “must → should softens requirements.”
- Domain terminology dilution — Specific terms replaced with generic or adjacent terms, reducing precision (legal, technical, police, conservation, UX, SRE, nautical, culinary). Why: limited term grounding; preference for higher‑frequency synonyms; Russian→English re‑render loses specificity. Frequency: common. Severity: medium. Evidence: “‘craquelure’ → ‘cracks’, ‘failover/standby’ → ‘switch/backup’”; “‘checkout’ → ‘order placement’.”
- False friends and semantic flips — Polysemy/confusable pairs yield incorrect sense: salvage/salvation, treatment/script, well‑staffed/well‑stocked, burglary/robbery, cold brew/cold coffee, combustible/flammable. Why: Russian lexical ambiguity and high‑freq collocations; inadequate sense disambiguation. Frequency: occasional. Severity: medium–high. Evidence: “‘well‑staffed’ → ‘well‑stocked’”; “‘burglary’ → ‘robbery’.”
- Tone/register domestication — Archaic/legal/folkloric/poetic/jargon tones become neutral, formal, or literal; idioms flattened or inverted. Why: optimization for clarity; style transfer loss; idiom literalization. Frequency: common. Severity: medium. Evidence: “archaic legal phrasing simplified”; “folkloric ‘ox’ → ‘bull’, idioms like ‘at your fingertips’ reversed/flattened.”
- Named entities and specialized labels — Species, ship prefixes, honorifics, and job‑/role‑titles mistranslated or over‑translated; sometimes kept in Russian where English is required, or titles swapped (Ms.→Miss; HMS→‘royal ship’). Why: term normalization and cross‑lingual entity handling gaps. Frequency: occasional. Severity: medium. Evidence: “bird names output in Russian instead of English”; “‘HMS’ → ‘royal ship’.”
- Minor structural/metadata drift — Headings and labels altered; singular↔plural, gender defaults, and small omissions (“up to”) shift nuance. Why: over‑editing for fluency; Russian morphology mismatch; heuristic summarization of headers. Frequency: occasional. Severity: low–medium. Evidence: “‘Lessons Learned’ → ‘Conclusions’”; omission of “up to” before temperature; neutral ‘their’ → ‘his’.

## Language‑Specific Factors
- Modality mapping ambiguity: Russian должен/нужно/следует map imperfectly to MUST/SHOULD/SHALL, leading to inflated/softened requirements in back‑translation.
- Polysemy and false friends: Russian words covering multiple English senses (e.g., спасение/утилизация, обработка/сценарий) nudge the model toward wrong English senses on return.
- Register compression: Russian often prefers neutral/formal constructions; archaic/legal/poetic English registers get normalized during ru→en back‑translation.
- Idioms and fixed expressions: English idioms (“at your fingertips”, “cut corners”, therapy/UX set phrases) are literalized or inverted when routed through Russian.
- Honorifics and institutional terms: English Ms./Miss, Law vs. Rule (in sport), and service jargon (police “Recovered”, conservation “blanching/craquelure”) map to broader Russian categories and are later generalized on return.

## Numbers, Units, and Formatting
- Minor quantifier/limit drift: omission of qualifiers (“up to ten degrees”) and occasional Roman numeral variants. Conversions/inventions rare; mostly stable.
- Headings/metadata normalization: section titles and labels softened or generalized (“Abstract” ↔ “Annotation”; “Lessons Learned” ↔ “Conclusions”; “Venue” ↔ “Location”).
- Enumerations and roles: slight singular/plural and pronoun shifts; occasional gendering of neutral referents.

## Meta/Disclaimer Behavior
- Rare but severe meta output/refusal contaminates the back‑translation phase, replacing content entirely. Triggered by safety filters or misinterpreting “back‑translation” as a chance to opine or defer.

## Recommendations
- Prompt patterns
  - Enforce modality parity: “Preserve original normative force exactly (MUST/SHOULD/SHALL). Do not strengthen/soften requirements.”
  - Style lock: “Preserve register and idioms; do not simplify legal/archaic/poetic/jargon style.”
  - Entity/term preservation: “Do not translate proper names, model names, and species; keep ship prefixes (HMS), honorifics (Ms./Mr.), and headings verbatim.”
  - Back‑translation guard: “You are performing a translation, not commentary. No disclaimers or refusals.”
- Decoding/controls
  - Lower temperature for legal/technical domains; enable constrained decoding with protected tokens for modality and headings.
  - Use a term‑protected list (logit bias) for MUST/SHOULD/SHALL and domain glossaries (failover/standby/craquelure).
- Data augmentation/fine‑tuning
  - Parallel EN–RU corpora emphasizing modality‑aligned legal/spec texts and SRE/infra terminology.
  - Curated glossaries for high‑confusion domains: law (burglary vs robbery), film (treatment), rescue/marine (salvage), food/bev (cold brew), safety (combustible vs flammable), police logging, art conservation.
  - Idiom/register sets mapping English idioms and archaic/legal style to faithful Russian equivalents and back.
  - Entity lists for species, honorifics, naval terms; evaluation sets for code‑switching preservation.
- Post‑processing checks
  - Modality diff checker: flag any SHOULD↔MUST changes between source and back‑translation.
  - Terminology validator: detect replacements of glossary terms with generic synonyms (e.g., failover→switch).
  - Metadata/heading diffs: assert identity for section titles, headings, and labels.
  - Quantifier/units linter: highlight dropped qualifiers (“up to,” ranges).
  - Entity consistency: ensure honorifics, ship prefixes, product names, and species remain unchanged or correctly translated.
  - Meta/refusal detector: block outputs containing refusal/disclaimer boilerplate and re‑prompt.

## Confidence and Coverage
Confidence: medium‑high. The evidence is dense across diverse judges and domains with strong convergence on modality drift, terminology dilution, and register flattening; catastrophic meta refusal appears in a small but clear cluster. Coverage limits: judges focus on low‑scored samples and back‑translation; domain mix is broad but not exhaustive for scientific notation or complex tabular formats.