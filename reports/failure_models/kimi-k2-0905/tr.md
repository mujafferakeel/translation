# Failure Model — Kimi K2-0905 — Turkish

## Summary
Kimi K2-0905 produces fluent Turkish but shows systematic fidelity drift when source texts are figurative, domain‑dense, or structurally constrained. The most salient weaknesses are: (1) negation and polarity flips; (2) semantically adjacent but incorrect lexical substitutions that distort facts and technical terms; (3) role/gender/number shifts exacerbated by Turkish pronoun and agreement properties; (4) invention/omission of concrete details; and (5) degradation of poetry/idioms and analogy direction. These errors recur across literature, UI/legal/technical, and numbers/units contexts and often co‑occur, compounding meaning loss.

## Top Failure Modes
- Negation/polarity inversions — Drops or flips “not/never,” inverts oaths and questions; why: Turkish negation morphology (-ma/-me, değil, yok) and question particles can be mishandled under compression; frequency: common; severity: high.
  - Evidence: “not to show us” rendered as affirmative choice; Emerson quote loses negation; oath line reversed.

- Semantically adjacent substitutions (near‑synonym drift) — Replaces key nouns/terms with close neighbors that change facts (peach→apricot, tug→dock crane, grief→law); why: decoding favors high‑probability Turkish collocates; weak domain grounding; frequency: common; severity: high.
  - Evidence: “tugboat” becomes “dock crane,” “peaches”→“apricots,” “climate grief”→“climate law.”

- Role/agent, person, and gender shifts — Subject/object swaps, pronoun gender assignment, and number/person changes; why: Turkish is pro‑drop with gender‑neutral “o,” flexible word order; back‑translation then reassigns roles; frequency: common; severity: medium–high.
  - Evidence: protagonist gender flipped; who taped the photo swapped; bosun/master roles swapped; nodding vs shaking head action reversed.

- Technical term drift and register flattening — Standards/legal/UX terms replaced by looser wording (Handling→Transport, Student Council→Assembly, Checkout→Payment), weakening precision; why: limited parallel term memory and preference for common Turkish equivalents; frequency: common; severity: medium–high.
  - Evidence: safety doc “combustible”→“flammable,” formal office titles remapped; UI label misrendered.

- Numbers, units, and discrete parameters altered — DC 12→17, 4‑4‑2→4‑4‑4, 30‑sol→30‑day, N→S latitude; why: numeric tokens normalized or “corrected,” and domain tokens (DC, sol) unfamiliar; frequency: occasional; severity: high in procedural content.
  - Evidence: D&D check DC wrong; soccer formation changed; Martian “sol” converted to “day.”

- Additions/omissions of concrete details — Inserting plausible but absent items (e.g., “Black Sea”) or omitting critical labels/temporal adverbs; why: hallucination under uncertainty; compression; frequency: occasional; severity: medium–high.
  - Evidence: invented location; omitted button label; “tomorrow” dropped.

- Figurative/idiom/analogy distortion — Idioms and metaphors literalized or inverted; specialized imagery mangled (tassels→pendulums; “grand scheme”→“vast reservoir”); why: literal translation bias and weak idiom grounding; frequency: occasional; severity: medium.
  - Evidence: inverted simile; nonsensical object substitutions (“man in robes”→“shovel” statue).

- Poetic form and tone loss — Rhyme, meter, and image networks flattened; why: target fluency over formal constraints; frequency: occasional; severity: medium.
  - Evidence: poem rhyme scheme not replicated; key images (“hum”→“roar”) shifted, changing tone.

## Language‑Specific Factors
- Gender‑neutral pronouns and pro‑drop: Turkish “o” (he/she/it) and omitted subjects encourage back‑translation gender/agent misassignments, especially in dialogue and narrative.
- Flexible SOV word order: Without explicit case cues in compressed clauses, agent/patient roles can flip on back‑translation.
- Negation morphology and clitics: -ma/-me, değil, yok, and question particle “mi” are easy to drop or mis-scope, causing polarity inversions.
- Compounding and institutional names: Choosing between Konsey/Meclis, Kurul/Heyet, vs. generic “kurul” leads to systematic term drift in governance/academic/legal contexts.
- Idioms and metaphor density: Turkish idiomatic equivalents are sometimes replaced by literal renderings, or English figuratives are over-literalized into Turkish, later reading as nonsense.
- Plurality/countability: Turkish optional plural marking and collective nouns induce number mismatches (singular/plural tokens in games, ingredients, pieces).

## Numbers, Units, and Formatting
- Numeral mutation: DC thresholds, knot speeds, formations, and latitudes altered; some singular→plural flips.
- Unit normalization: “sol”→“day,” fathoms and knots misread; time formats toggled between 24h and 12h.
- Structured tokens: RPG stats, aviation gates, and placeholders paraphrased or “corrected.”
- Table/label fidelity: UI buttons/field names occasionally omitted or generalized.

Conversions/inventions: Occur when unfamiliar domain tokens appear (RPG, astronomy, maritime); the model “normalizes” to common Turkish phrasing or domestic standards.

## Meta/Disclaimer Behavior
- Occasional policy/style normalization: Advisory→Warning or Ministry substitution for Department; journalistic/legal register softened or stiffened.
- No frequent overt safety disclaimers, but register shifts can implicitly reframe authority or obligation (SHOULD→MUST).

Triggers: Government/legal/standards text where modality and institutional titles are salient.

## Recommendations
- Prompting
  - Provide a term glossary and do-not-change list for critical nouns, titles, and UI labels; e.g., “Keep: DC 12, 4‑4‑2, sol, Student Council, Checkout.”
  - Add constraints: “Preserve ALL negations and numbers; do not add locations; do not modernize imagery; retain gender neutrality unless explicitly specified.”
  - Request a two-pass output: Turkish translation + a keyed checklist echoing negations, numbers, named entities, and roles.
  - For poetry: instruct to preserve imagery and rhyme/metre where present; otherwise mark “free translation.”

- Decoding
  - Lower temperature/top‑p for technical/legal/UI content to reduce speculative substitutions.
  - Use constrained decoding or lexically guided decoding for glossary items and numeric tokens.

- Data and Fine‑tuning
  - Curate Turkish parallel corpora with negation contrastive pairs and evaluation that penalizes polarity flips.
  - Domain packs: maritime/aviation, RPG/game mechanics, culinary, ornithology, safety/standards, UI/UX lexicons with Turkish canonical terms.
  - Idiom/analogy training sets mapping figuratives with validated Turkish equivalents.
  - Add gender/agent coreference sets reflecting Turkish pro‑drop and back‑translation alignment.

- Post‑processing
  - Automatic validators for:
    - Negation preservation (diff “not/never/no” vs değil/yok/-ma).
    - Number/unit consistency (regex guardrails; whitelist “sol,” formations, DC values).
    - Named-entity/title consistency (lookup “Konsey vs Meclis,” “Checkout vs Payment”).
    - Role/agent checks using dependency cues; flag when subjects/objects swap between source and back-translation.
  - Glossary enforcement with fallback: if OOV, keep source term in parentheses.
  - For poetry, optional human pass for rhyme/imagery.

## Confidence and Coverage
- Confidence: medium–high. Evidence is dense and consistent across judges and domains, with repeated patterns (negation flips, near‑synonym drift, role/gender swaps, numbers/units).
- Coverage limits: Round‑trip judging may exaggerate Turkish‑specific ambiguities (gender, pro‑drop). Domain mix is broad but skewed toward narrative, legal/UX, and hobbyist technical (RPG, sports), so recommendations emphasize these areas.