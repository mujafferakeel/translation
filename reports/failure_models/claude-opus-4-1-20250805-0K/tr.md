# Failure Model — Claude Opus 4.1 (no reasoning) — Turkish

## Summary
Overall, Opus 4.1 handles Turkish round‑trip meaning reasonably well but shows recurring precision loss under domain pressure. The most salient weaknesses are: polarity/degree inversions in dense changelogs/specs, systematic gender/pronoun drift due to Turkish’s gender‑neutral grammar, flattening of tone/register (especially archaic, nautical, legal/RFC), degradation of domain terminology (tech/legal/culinary/ornithology), and occasional conversions or weakening of formal markers (units, RFC keyword capitalization, modal force). Proper nouns/titles and safety‑critical terms sometimes shift to near‑synonyms that change facts.

## Top Failure Modes
- Polarity and degree inversions in scoped statements — Model flips “now X more” to “no longer X more” or softens must/urgency; why: Turkish negation and aspect remap ambiguities (“artık/eskisi gibi değil,” -mez) and modality mapping; frequency: occasional; severity: high
  - Evidence: Mage buff “now absorbs 50% more” became “no longer absorbs 50% more”; evacuation “must evacuate” softened to “should.”
- Gender/pronoun and role swaps — Characters’ gender or ownership flips; why: Turkish lacks gendered pronouns and possessive ambiguity, back-translation guesses; frequency: common; severity: medium-high
  - Evidence: Protagonist consistently female instead of male; line about implant ownership switched; dialogue “I was seven” became “she was seven.”
- Tone/register flattening and temporal/modal drift — Archaic/literary or professional tone becomes modern/plain; “would”→“will,” MUST/SHOULD case lost; why: preference for generic Turkish lexemes and neutral modality (-malı/-meli often softened), capitalization not preserved in TR; frequency: common; severity: medium
  - Evidence: 19th‑century voice leveled; RFC keywords rendered lowercase and MUST→should; nautical diction simplified.
- Domain terminology substitution (precision loss) — Technical/culinary/science terms mapped to close but wrong terms; why: sparse Turkish-domain alignments and multiword compound segmentation; frequency: common; severity: medium
  - Evidence: “idempotency token”→“uniqueness token”; “combustible”→“flammable”; “diffracting”→“refracting”; “charred”→“smoked”; bird species swapped (curlew→plover; dunlins/godwits→redshanks/stilts).
- Proper nouns, titles, and named entities drift — Names and formal titles altered; why: preference for common Turkish equivalents and ambiguity in historical titles; frequency: occasional; severity: medium
  - Evidence: “Mukawwa Stone”→“Cardboard Stone”; “Riverfront Arts Festival”→“Riverside Art Festival”; “Master”→“First Mate”; “Magistrate/Constable”→generic modern titles.
- Safety/requirements softening — Force of commands and hazards weakened; why: modality and technical register mapping issues; frequency: occasional; severity: high (in specs/safety)
  - Evidence: “MUST/SHOULD” weakened; “Combustible”→“Flammable”; “crest”→“summit” (safety nuance).
- Small factual detail substitutions — Minor but meaning‑relevant substitutions in objects, materials, or locations; why: synonym overfit and polysemy; frequency: occasional; severity: low‑medium
  - Evidence: sleet→downpour; roasted corn→fried corn; shaved ice→ice cream; “bank” unit→“bench”; canopy→eave; port↔starboard.
- Instructional polarity and step errors — Imperatives altered or mis‑targeted; why: Turkish imperative politeness choices and homonymy; frequency: rare; severity: medium
  - Evidence: “Retype both fields” became “Rewrite both fields”; “Lay out/Cool” confusion in assembly steps.

## Language‑Specific Factors
- Gender neutrality: Turkish “o/ona/onun” yields gender ambiguity; back‑translation often assigns a gender inconsistently, leading to swaps.
- Modality and evidentiality: Mapping MUST/SHOULD/WOULD to -malı/-meli/-dır or present/future in Turkish often softens or shifts force and temporality; back‑translation loses RFC capitalization semantics.
- Agglutination and compound nouns: Multiword technical terms (e.g., idempotency token) can become descriptive paraphrases in Turkish, which then back‑translate to broader synonyms.
- Word order and focus: Turkish SOV and postpositions encourage recasting; polarity particles like “artık” can be interpreted as cessation rather than increase.
- Domain lexicon gaps: Historical/legal titles (Magistrate, Constable, Lamplighter), nautical ranks, and rare species names lack stable Turkish mappings, inviting generic or wrong substitutions.

## Numbers, Units, and Formatting
- Units converted or localized: yards→meters; severity: low‑medium when specs rely on originals.
- RFC keyword casing lost: MUST/SHOULD/MAY lowercased; severity: high in specs.
- Time formats drift: 12h→24h; minor.
- Directional/technical sides flipped: port/starboard; severity: medium in technical narratives.
- Terminology capitalization/brands/titles normalized: pillar names (“Grounded”→“Authentic”); minor but noticeable.

## Meta/Disclaimer Behavior
- Minimal policy intrusions. The notable “meta” issue is normative capitalization weakening in standards‑like texts; otherwise, explicit safety disclaimers or authorial notes are rare and not contaminating content.

## Recommendations
- Prompting
  - Enforce polarity and modality: “Preserve all polarity (no/not, now/no longer), degrees (+/−, %, comparatives), and modality (MUST/SHOULD/MAY) exactly. Do not soften requirements.”
  - Lock units and casing: “Do not convert units or date/time formats. Preserve all capitalization of standards keywords.”
  - Entity/term preservation: “Preserve proper nouns, titles, ranks, and product/ability names verbatim unless a well‑established Turkish exonym exists.”
  - Gender disambiguation: “If source gender is explicit, preserve it; otherwise keep neutral and avoid assigning gender in back‑translation.”
  - Domain glossaries inline: Provide a mini‑glossary before translating (e.g., idempotency token=idempotency token; Master=Gemi Kaptanı).
- Decoding
  - Lower temperature/top‑p for specs, changelogs, and legal texts to reduce synonym drift.
  - Use constrained decoding/lexical masking for RFC keywords, units, and critical tokens (“no longer/now,” numeric percentages, port/starboard).
- Data and fine‑tuning
  - Augment TR↔EN parallel corpora with: RFCs/standards preserving keyword casing; game patch notes/changelogs; nautical/legal/historical titles; culinary techniques; ornithology lexicons.
  - Create contrastive pairs for polarity and degree (“now +50%” vs “no longer +50%”), modality strength, and hazard taxonomy (combustible vs flammable).
  - Add termbanks for species, ranks, engineering optics (diffract vs refract), and GIS/topographic safety terms (crest vs summit).
- Post‑processing QA
  - Automated checks:
    - Polarity: flag toggles of “now/no longer,” negation particles, comparative/percent phrases.
    - Modality: ensure RFC keywords preserved verbatim and uppercase.
    - Units/directions: detect unit conversions; verify nautical directions and sides.
    - Named entities: diff proper nouns/titles against source.
  - Human spot‑checks for domains with high risk: specs, safety, legal, patch notes, nautical.
  - Back‑translation A/B: compare neutral vs literal settings; if divergence in polarity/modality, escalate.

## Confidence and Coverage
Confidence: medium-high. Evidence spans many judges and domains with repeated patterns (gender drift, polarity flips, tone flattening, terminology precision loss, RFC/units issues). Coverage limits: We infer Turkish‑specific causes from back‑translation errors; not all original TR outputs are visible, so some phenomena (e.g., specific suffix misuse) are deduced rather than directly observed. Domain mix is broad but skews to literary, technical specs, and changelogs; fewer examples for medical or financial compliance texts.