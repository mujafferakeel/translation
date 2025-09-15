# Failure Model — Mistral Medium 3.1 — Turkish

## Summary
Overall, the model is fluent but prone to fidelity drift: it often substitutes specific terms and names, occasionally flips meanings or modalities, and weakens tone/register in legal, technical, and literary texts. The most salient weaknesses are: (1) hallucinated or swapped domain terms/proper nouns, (2) meaning inversions (negation, time, direction, obligation), (3) loss of personification/gender and role/rank accuracy, (4) numbers/time/units drift, and (5) tone/register flattening, especially for archaic/legal and poetic texts.

## Top Failure Modes
- Hallucinatory substitutions of specific terms and names — Replaces precise items (species, ingredients, tools, place/ability names) with near-neighbors or generic terms; likely due to weak domain term grounding and synonym over-preference in Turkish. Frequency: common; Severity: high.
  - Evidence: puffin→penguin; caddisfly/smelt→sculpin/pearl mussels; smoked paprika/rosemary→bay pepper/fennel; bone awl tip→drill bit; coordinates E→D; storage platters→vinyl.

- Meaning inversions and logical flips — Negation/time/directional reversals and cause/effect switches, often when cues are subtle; triggered by complex sentences or idiomatic/poetic phrasing. Frequency: occasional; Severity: high.
  - Evidence: “spoons ceased clanging”→“did not stop clattering”; onshore→offshore winds; “hard not to feel small”→“hard to feel small”; “forces older than iron or steam” flipped.

- Modality and technical precision drift — Normative keywords (MUST/SHOULD) and legal/technical terms softened, strengthened, or re-scoped; caused by ambiguous Turkish modality mapping and synonym substitution. Frequency: occasional; Severity: high in specs/policy.
  - Evidence: SHOULD→MUST; “tail latencies”→“queue delays”; “venue”→“jurisdiction”; “sanctions”→“litigation costs”; “Non-Goals”→“Out of Scope.”

- Person/reference and role fidelity loss — Gender/personification collapsed, titles/ranks altered, service categories narrowed; triggered by Turkish gender-neutral pronouns and rank/title variability. Frequency: common; Severity: medium-high.
  - Evidence: “she”→“it” for an anthropomorphized animal; Captain/Master→Lieutenant; “service animals”→“guide dogs”; protagonist gender flipped.

- Numbers, times, and units drift — Times/dates shifted; imperial/metric swapped; 12h↔24h format normalized; likely from Turkish locale conventions and normalization heuristics. Frequency: occasional; Severity: medium-high.
  - Evidence: 6:00 PM→20:00; 10:00→10:30; three feet→three meters; visit date moved.

- Tone/register erosion and prosody loss — Archaic/legal diction modernized; rhyme scheme/meter broken; figurative imagery flattened; due to preference for plain Turkish and paraphrase tendencies. Frequency: common; Severity: medium.
  - Evidence: archaic legal style simplified; stated rhyme scheme broken; “restrained” tone rendered “passionate.”

- Additions/omissions and quote alterations — Inserts plausible but unsourced details or drops restrictive clauses/etymology accuracy; especially in narrative and policy contexts. Frequency: occasional; Severity: medium.
  - Evidence: added “drug emojis,” “cheese,” “surveillance”; omitted theft-of-lantern clause; altered direct quotes; “quarantine” etymology misspelled/misrendered.

- Loanword/etymology mishandling — Uses Turkish loanword but misstates foreign origin or alternates between forms inconsistently. Frequency: rare–occasional; Severity: medium.
  - Evidence: “quarantine” rendered “karantina” with incorrect Italian etymology (“karantina giorni”) and inconsistent usage.

## Language‑Specific Factors
- Gender-neutral pronouns (o) and lack of grammatical gender: encourages neutralization of “she/he” and personification loss; back-translations may default to “it.”
- Modality mapping ambiguity: English MUST/SHOULD/MAY map variably to “zorunda/gerekir/olabilir,” inviting strength shifts in legal/technical text.
- Agglutination and compounding: long noun phrases can obscure heads/modifiers, prompting re-attachment (ranks, titles, technical compounds).
- Register calibration: Turkish favors concise, modern phrasing; maintaining archaic/legal or poetic register requires deliberate lexical choices; rhyme/meter are difficult to preserve due to syllable timing and suffix load.
- Numerals/time conventions: Turkish commonly uses 24-hour time and meters; the model normalizes formats (6 PM→20:00; feet→meters).
- Proper nouns and capitalization: tendency to adapt or genericize names (Spine→Ridge) when Turkish equivalents seem plausible.

## Numbers, Units, and Formatting
- Time/date shifts: 6 PM→20:00; 10:00→10:30; date moved by a day. Some AM/PM vs 24h normalization.
- Unit conversions/inventions: feet→meters; “another twenty meters” dropped or altered.
- Normative formatting: uppercase keywords (MUST/SHOULD) softened or strengthened; headers/meta inserted.
- Tables/markup: occasional added asterisks, meta headers; minor list/format tweaks.
- Conversions are not always justified; sometimes invented or normalized without instruction.

## Meta/Disclaimer Behavior
- Occasional authorial intrusions: added explanatory phrases (e.g., festival notes, IRB explanation), translator note/headers/asterisks.
- Triggered by: policy/legal/academic contexts where the model “helpfully” clarifies; creative texts that invite embellishment.

## Recommendations
- Prompt patterns
  - Use a guardrail preface: “Translate exactly into Turkish. Do not add, omit, or explain. Preserve all names, ranks, quotes, numbers, times, units, capitalization, and normative keywords (MUST/SHOULD/MAY) verbatim.”
  - Add constraints: “Copy numerals, times, and units as-is. Keep proper nouns unchanged. Preserve rhyme scheme if stated; otherwise retain imagery and figurative meaning.”
  - Personification: “If the source uses gendered pronouns for animals/objects, explicitly preserve personification (e.g., repeat the name or use contextual noun phrases that imply gender).”
  - Technical/legal: “Maintain exact modality (MUST/SHOULD). If no precise Turkish term, keep the English term in parentheses on first mention.”
- Decoding changes
  - Lower temperature (0.0–0.2) and conservative sampling to reduce substitutions/additions.
  - Enable constrained decoding with a glossary for: normative keywords, domain terms (aviation, ecology, architecture, specs), names, coordinates.
- Data and fine‑tuning
  - Augment with parallel Turkish corpora in legal/specification domains emphasizing MUST/SHOULD mapping and tail latencies, etc.
  - Curate literary parallel data preserving personification, archaic register, and rhyme/meter strategies in Turkish.
  - Add targeted datasets for time/unit fidelity and for directionality (onshore/offshore), polarity (“hard not to”), and idioms.
  - Build domain glossaries (species, culinary, tools, gaming, architecture) and train with glossary enforcement.
- Post‑processing checks
  - Automated audits:
    - Numerals/time/units diff: flag any changes; AM/PM vs 24h mismatches.
    - Modality checker: ensure MUST/SHOULD/MAY unchanged.
    - NER consistency: names, ranks, titles, coordinates, ability/item names.
    - Polarity/direction test: detect negation and antonym flips (ceased vs continued; onshore vs offshore).
    - Quote fidelity: exact-match direct quotations.
    - Rhyme/structure: if poem/constraints declared, verify scheme length and rhyme markers.
  - Human-in-the-loop review for high-risk genres (legal/technical specs; poetry).
  - First-mention dual-form policy: loanword in Turkish + source term in parentheses to stabilize terminology (“karantina (quarantine)”).

## Confidence and Coverage
Confidence: medium-high. The sample includes many low to mid scores across diverse domains (legal, technical, literary, policy, cooking, ecology, gaming), with multiple independent judges showing consistent patterns (term swaps, modality/time drift, personification loss, inversions). Limitations: back-translation judging may penalize inherent Turkish features (gender neutrality, 24h time) and some genre-specific expectations (poetic meter). Evidence density is strong for substitutions and modality/time issues; rarer behaviors (etymology notes, meta additions) have fewer instances.