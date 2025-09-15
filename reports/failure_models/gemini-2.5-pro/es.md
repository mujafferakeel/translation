# Failure Model — Gemini 2.5 Pro — Spanish

## Summary
Overall, Gemini 2.5 Pro delivers high fidelity for Spanish but shows systematic weaknesses when precision and domain‑specific terminology matter. The most salient issues are: (1) polarity/direction reversals and timeline shifts in quantitative or operational content; (2) domain term flattening or substitution (business/finance, legal, technical, data viz, tools); (3) register drift toward more formal/verbose phrasing, reducing tone fidelity; (4) gender and neutrality handling (defaulting to masculine or adding gender); and (5) occasional mishandling of fixed UI strings/labels and proper nouns.

## Top Failure Modes
- Polarity and timeline reversals — Reverses metric directions (NPS up vs down; time-to-sign shorter vs longer) and shifts timeframes (late evening → late afternoon); why: over‑normalization to Spanish idiom and paraphrasing of comparatives; frequency: common; severity: high; evidence: “NPS direction flipped; time‑to‑signature 54↔42”; “power restoration late evening → late afternoon.”
- Domain terminological substitution (business/finance) — Replaces precise terms with near neighbors (“new bookings” → “new hiring”; “beneficial ownership” → “real ownership”; “Outperform” → “Superior Performance”); why: lexical proximity bias and weak domain glossary anchoring; frequency: common; severity: high; evidence: “new bookings interpreted as hiring”; “industry rating ‘Outperform’ generalized.”
- Legal category confusions — Maps distinct legal categories to looser Spanish terms (“Residential Burglary” → “Robbery in domicile”; “minute order” → generic “succinct record”); why: cross‑jurisdiction false friends and missing legal register; frequency: occasional; severity: high; evidence: “burglary vs robbery distinction lost”; “minute order replaced by generic phrase.”
- Technical specificity loss (software, data viz, hardware) — Substitutes key technical items (“stream graph” → “flow chart”; “tail latencies” → “queue latencies”; “living hinge” → “flexible hinge”; tools “torque drivers” → “impact wrenches”); why: preference for higher‑frequency Spanish terms; frequency: common; severity: medium–high; evidence: “stream graph misidentified as flow chart”; “tool names swapped.”
- Gender neutrality drift — Neutral “they/their” rendered as masculine or gender added where none existed; why: Spanish gendered morphology and model prior favoring default masculine; frequency: occasional; severity: medium; evidence: “they → he/his”; “child gender added.”
- UI strings/labels and taxonomy drift — Fixed labels changed (“Checkout” → “Pay”; STRIDE categories expanded with descriptors; menu tags “GF” → “SG”); why: eagerness to clarify or localize; frequency: occasional; severity: medium; evidence: “button ‘Checkout’ replaced by ‘Pay’”; “‘GF’ tag altered to ‘SG’.”
- Proper nouns and set phrases altered — Partial translation of names/placeholders and titles (“Condado de San Francisco” inside a placeholder; book title localized); why: over‑translation of named entities; frequency: occasional; severity: medium; evidence: “County rendered as ‘Condado’ in placeholder”; “book title translated.”
- Idiom, onomatopoeia, and stylistic flattening — Vivid or playful language literalized or formalized (“seal it” → “sentence it”; SWISH→other SFX; “warming storm” nuance lost); why: preference for formal Spanish and literal mappings; frequency: common; severity: low–medium; evidence: “sports SFX altered reducing climax”; “poetic tone flattened.”

## Language‑Specific Factors
- Gendered morphology: Spanish lacks a widely accepted singular neutral pronoun; the model defaults to masculine or infers gender, shifting inclusivity.
- Title and role conventions: “CEO” often rendered as “Director General,” which can be acceptable but shifts register and global flavor.
- Legal lexicon mapping: English distinctions (burglary/robbery; “minute order”) require jurisdiction‑specific Spanish; generic “robo,” “orden” blur meaning.
- Compound technical terms: Calques like “stream graph” lack a high‑frequency Spanish equivalent (“streamgraph”/“gráfico de corrientes”); the model gravitates to familiar but incorrect neighbors like “diagrama de flujo.”
- Infrastructure/transport terms: “Cycle track” vs “ciclovía” (event vs facility) is a Spanish false friend pitfall.
- Idioms/onomatopoeia: English sports/news SFX and idioms often need preservation or culturally neutral handling; literal replacements feel unnatural in Spanish.

## Numbers, Units, and Formatting
- Direction and magnitude inversions: 42↔54; up vs down; late evening ↔ late afternoon; frequency: common; severity: high.
- Period labels and categories: Q3/Q4 confused with T3/T4; STRIDE labels expanded; GF tag altered; frequency: occasional; severity: medium.
- Ratings/labels: “Outperform” normalized to “Superior Performance”; severity: medium.
- Minor entity/unit slips: “bank” (seating) → “bench”; “semaphore” → “traffic lights” in technical context; severity: low–medium.
- Proper noun casing/localization inside placeholders (“Condado…”): severity: low–medium.
No consistent evidence of unit conversions or invented numbers—errors are directional/labeling rather than arithmetic.

## Meta/Disclaimer Behavior
- Little to no safety/policy intrusions observed. One procedural ambiguity in a substitution note was noted but not a safety disclaimer. Contamination by authorial intrusions appears rare.

## Recommendations
- Prompting
  - Enforce a locked glossary: Provide bilingual term lists for domain terms (“bookings,” “tail latency,” “streamgraph,” legal categories, ratings like “Outperform,” STRIDE labels, UI texts like “Checkout”).
  - Add faithful‑translation constraints: “Do not change polarity, direction, quantities, dates, tags, or button text. Preserve capitalization and punctuation of labels verbatim.”
  - Gender guidance: “Maintain gender neutrality; avoid inferring gender unless explicit.” Or specify inclusive strategies (pluralization or periphrasis).
  - Register control: “Match tone and register; avoid formalization unless present.”
- Decoding
  - Lower temperature/top‑p to reduce creative paraphrase and label drift.
  - Use constrained decoding or lexically constrained decoding for protected terms (UI strings, tags, metrics, legal categories).
- Data and fine‑tuning
  - Curate EN↔ES parallel corpora in finance/SaaS ops, legal (U.S./UK to neutral ES), software/SRE, data visualization, product/UI strings, and safety/incident reporting.
  - Augment with hard negatives where near‑synonyms are incorrect (“burglary vs robbery,” “bookings vs hiring,” “streamgraph vs flowchart”).
  - Include gender‑neutral English inputs with annotated Spanish neutral renderings; reinforce strategies for neutrality without masculine default.
- Post‑processing QA
  - Numeric and polarity checks: Validate all numbers unchanged; flag direction words (increase/decrease, shorter/longer) against source.
  - Label/terminology diff: Exact‑match checks for UI strings, tags (GF), ratings, STRIDE, quarter labels (Qn vs Tn).
  - Named entity/placeholder guard: Do‑not‑translate spans for names/placeholders.
  - Domain lint rules: Legal (burglary≠robbery), finance (bookings≠hiring), viz (streamgraph≠flow chart), tools.
  - Style lint: Detect over‑verbosity; flag shift from imperative/concise instructions to verbose phrasing.

## Confidence and Coverage
Confidence: medium. Evidence spans many judges and domains with repeated patterns (metric polarity reversals; domain term substitutions; gender drift; label changes). Coverage skews toward business, legal, technical, and editorial prose; fewer examples on scientific units or tabular formatting.