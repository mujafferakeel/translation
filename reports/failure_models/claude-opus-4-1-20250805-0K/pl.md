# Failure Model — Claude Opus 4.1 (no reasoning) — Polish

## Summary
Claude Opus 4.1 generally preserves core meaning in Polish round‑trip translation but shows systematic drift on domain‑specific terminology, occasional meaning inversions in safety/instructional content, and consistent flattening of register and imagery. Polish‑specific polysemy and gender/case pressures lead to role and pronoun errors. Embedded verse and idiomatic dialogue degrade disproportionately. The most salient weaknesses: domain term substitution, polarity flips in procedures, pronoun/gender miscues, poetic/register flattening, and lexeme‑driven false friends (e.g., klucz).

## Top Failure Modes
- Terminology drift in technical domains — Replaces precise terms with near‑synonyms or adjacent categories (materials, soils, archaeology, IT, SaaS, conservation), reducing factual precision.
  - Why: Preference for high-frequency Polish synonyms; limited anchoring to domain glossaries; translationese normalization.
  - Frequency: common; Severity: medium to high (domain dependent)
  - Evidence: sandy loam→sandy clay; stonepaste→stoneware; failover→switchover; “data governance standards”→“data management standards”.

- Polarity and instruction inversions in safety/operations — Reverses or weakens critical qualifiers and disposal guidance; hazard labels softened or swapped (“combustible”↔“flammable”), severity dropped (“serious” omitted).
  - Why: Over-normalization of hazard lexicon; misalignment on regulated phrasing; failure to preserve negative/exception clauses.
  - Frequency: occasional (clustered in one doc, flagged by multiple judges); Severity: high
  - Evidence: small-spill “nonhazardous waste” rendered as “hazardous”; “serious eye irritation” weakened; combustible→flammable.

- Pronoun and gender miscues — Gender-neutral “they/their” and neutral roles shift to masculine forms; sports/roles re-gendered (“batter”→“batsman”).
  - Why: Polish requires gendered agreement; model defaults to masculine when context is ambiguous; domain style guides (neutral) not enforced.
  - Frequency: common; Severity: medium
  - Evidence: neutral student becomes “he/his”; “batter” becomes “batsman”.

- Lexeme polysemy and false-friend substitutions — High-ambiguity Polish nouns/verbs pick the wrong English sense on back-translation (or vice versa), altering concrete details.
  - Why: Polish polysemy + underspecified context; decoding favors most common English mapping.
  - Frequency: occasional; Severity: medium
  - Evidence: klucz “wrench”→“key”; “door”→“doors”; “full fly” (tropik)→“full footprint”; “vane”→“blade”; “lift/priming” variants in pumps.

- Register and tone flattening (legal/procedural/poetic) — Archaic/legal markers (“Whereas”) modernized; professional jargon generalized; vivid verbs dulled; idiomatic dialogue blunted.
  - Why: Style smoothing toward neutral Polish corporate/press style; scarcity of parallel data at target registers.
  - Frequency: common; Severity: medium
  - Evidence: “Whereas”→“Considering that”; “Incident Overview”→“Event”; “unclench”→“open”; “Sit tight”→“Please wait”.

- Domain specificity loss (species, tools, sports) — Specific taxa, gear, and play types generalized or swapped, eroding factual accuracy.
  - Why: Limited specialized lexicon anchoring; confusion between near neighbors in Polish taxonomy/gear; back-translation chooses common hypernyms.
  - Frequency: common; Severity: medium
  - Evidence: dunlins/red knots/godwits→sandpipers/oystercatchers/lapwings; trowel→spade; inbound/leaner misrendered.

- Embedded verse and rhyme/poetics degradation — Meter/rhyme examples garbled, added intrusions, contradictions within lines; imagery substitutions reduce lyric precision.
  - Why: Competing constraints (meaning vs form) without explicit instruction; Polish prosody not preserved as an objective.
  - Frequency: occasional; Severity: medium
  - Evidence: poem lines altered with awkward additions; rhyme note garbled; “rivet→thread,” “gear→wheel,” imagery weakened.

- Small but impactful literal shifts — Singular/plural, directionality, time-of-day, committee/agency names and labels drift.
  - Why: Low-salience tokens normalized; lack of enforced copy fidelity for enumerations/labels.
  - Frequency: occasional; Severity: low to medium
  - Evidence: “door”→“doors”; “after‑school”→“afternoon”; “Transportation Committee”→“Commission”.

## Language‑Specific Factors
- Polysemy traps: klucz = key/wrench; bateria = battery; tropik (tent fly) vs podłoga/footprint; żywica/żywiczne and conservation terms; pył/ił/glina in soil taxonomy. These encourage wrong English senses on back-translation.
- Gender agreement pressure: Polish compels gendered forms; without context, model defaults to masculine, breaking neutrality of “they.”
- Case and aspect: Agent/patient roles can blur in condensed Polish, nudging role swaps (e.g., “lifts” vs “impulse”); imperfective/perfective choices soften or strengthen modality.
- Register markers: Archaisms like “Whereas” map to several Polish options (“Jako że,” “Mając na uwadze”), often modernized in re‑Englishing.
- Sports/jargon localization: Basketball and cricket terms have narrower Polish usage; model substitutes generic terms (“on offense”, “partnership”→“teamwork”), then back-translation loses specificity.

## Numbers, Units, and Formatting
- Technical label drift: pump head/priming→lift height/flooding; base slots→holes; high‑efficiency→high‑performance. Conversions are not invented; units generally preserved.
- Metadata normalization: “Accession Number”→“Inventory number”; “[n/a]”→“[n.d.]”; signage/labels softened (“EMERGENCY”→“ALARM”).
- Tables/markup not highlighted as a major source of error; numeric values and measures were largely stable. Frequency: rare to occasional; Severity: low to medium when technical.

## Meta/Disclaimer Behavior
- Minimal explicit safety/policy intrusions. Occasional translator artifacts in commentary/notes (e.g., rhyme note garble) and slight role/title renaming (“forensics lead” vs “manager”).
- Triggers: embedded analytical commentary around quoted verse or examples; incident report headings. Frequency: rare; Severity: low.

## Recommendations
- Prompting
  - Enforce terminological fidelity: “Translate into Polish and back without changing domain terms. Preserve hazard labels and disposal categories verbatim. If ambiguous (e.g., klucz), prefer the technical sense from context.”
  - Style constraints: “Preserve legal register (Whereas…), do not modernize tone. Keep gender-neutral references. Maintain singular/plural and capitalization in labels.”
  - Verse handling: “For poems, prioritize semantic fidelity; do not impose rhyme. Mark quoted lines with quotes and keep line breaks.”
  - Safety-critical flag: “If text contains hazards/instructions, avoid synonym substitution and do not invert polarity (‘no’, ‘non‑’).”
- Decoding
  - Use lower temperature/top‑p for technical/safety/legal text to reduce synonym drift; allow slightly higher creativity only for literary segments when asked.
  - Apply constrained decoding with copy-bias for labeled fields and enumerations (HEAD, PRIMING, ACC #).
- Data and fine‑tuning
  - Augment with Polish–English parallel corpora in:
    - GHS/SDS, conservation reports, archaeology/soil taxonomy (USDA/WRB), pumps/HVAC manuals, SaaS/DevOps jargon.
    - Sports glossaries (basketball shot types, cricket roles), outdoor gear (tents: tropik/footprint).
  - Lexeme disambiguation sets for high-risk Polish polysemes (klucz, tropik, muszka/midge vs mosquito, łopata/kielnia).
  - Inclusive language finetune: reinforce neutral rendering patterns across Polish ↔ English.
- Post‑processing checks
  - Rule-based validators:
    - Hazard polarity and disposal type consistency (“nonhazardous” must not flip).
    - Terminology lint for known pairs (stonepaste≠stoneware; failover≠switchover).
    - Gender neutrality scanner comparing source vs back-translation pronouns.
    - Singular/plural and numeric field diff on labels and headings.
  - Domain glossary locking: inline term dictionaries for each document; hard constraints on critical tokens.
  - Literary QA: highlight metaphor-heavy lines and embedded verse for human review.

## Confidence and Coverage
- Confidence: medium-high. Patterns are reinforced across many judges and samples, with multiple independent flags for the same failure types (terminology drift, safety polarity, gender shifts).
- Coverage limits: Evidence is skewed toward technical, legal/procedural, and literary excerpts; fewer numeric/unit-heavy tables and no non-Latin scripts. Some severe issues cluster in a single SDS document, so high-severity frequency is estimated as occasional overall.