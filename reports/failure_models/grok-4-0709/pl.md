# Failure Model — Grok 4 — Polish

## Summary
Grok 4’s Polish round‑trip translations are generally faithful but show recurring drift in domain terms and named entities, occasional polarity inversions, and systematic style/register flattening. The most salient weaknesses are: over‑localization of species/labels and technical nouns, mistranslation of negation or outcome cues, loss of specialized jargon (sports, legal, nautical, culinary, conservation), and format/language leakage in headings and dates. Poetry/rhythm is not preserved when the source requires it.

## Top Failure Modes
- Over‑localization of named entities and taxonomy — Translates or substitutes species names and fixed labels instead of preserving originals (and sometimes swaps to near‑neighbors). Why: Polish has entrenched common names and the model defaults to “naturalization”; weak constraint to keep source nomenclature. Frequency: common; Severity: high.
  - Evidence: bird list rendered with Polish common names instead of English; fish and plant species swapped (“sea bass”→“cod”; “leatherleaf”→“sundew”).
- Domain‑term drift (sports, tools, ecology, legal) — Replaces precise jargon with generic synonyms or wrong close terms, changing meaning. Why: limited domain term grounding and preference for common Polish equivalents. Triggers in sports tactics, soil/shore ecology, tools, legal boilerplate. Frequency: common; Severity: medium–high.
  - Evidence: football terms misread (“mid‑block,” “trap,” “he’s off” misinterpreted); soil textures crossed (“sandy loam”↔“clayey sand”); “torque drivers/impact driver”→“wrenches.”
- Polarity and factual inversions — Negation and constraint words flip or drop, altering facts or outcomes. Why: ambiguity in Polish negative constructions, soft attention to scope words (“no,” “up to,” “even”), and tense/aspect shifts. Frequency: occasional; Severity: high.
  - Evidence: missing “no” yields “fan spinning, screen flickering” instead of “no fan/no flicker”; “up to” rendered as “even,” changing a numerical limit.
- Style/register flattening and idiom literalization — Evocative or technical tone becomes generic; idioms/onomatopoeia simplified. Why: optimization toward neutral Polish prose; scarcity of parallel style‑matched data. Frequency: common; Severity: medium.
  - Evidence: sportscaster punch lost (“leather”→“skin”; onomatopoeia softened); archaic/nautical/legal registers generalized (“becalmed,” “articles,” “case caption” simplified).
- Format and language leakage (headings, metadata) — Section headers/dates stay in Polish in back‑translation or are normalized inconsistently. Why: header recognition as structural elements and locale auto‑formatting. Frequency: occasional; Severity: medium.
  - Evidence: headers remain in Polish; date localized to Polish month form.
- Morphosyntax mismatches (number/gender/tense) — Shifts in plurality, gendered references, and tense/aspect that English did not specify. Why: Polish forces gender/number; defaults can bias to feminine/masculine or plural; tense harmonization errors. Frequency: occasional; Severity: low–medium.
  - Evidence: “their”→“her”; singular “door”→plural “doors”; tense drift in flashback.
- Poetry form loss (meter/rhyme) vs. analysis — When form is semantically salient, verse is flattened to prose or meter broken, contradicting stated constraints. Why: decoding optimizes for meaning over prosody; no constraint for rhyme/meter. Frequency: occasional; Severity: medium–high.
  - Evidence: poem loses meter/rhyme though analysis emphasizes their importance.
- False friends and near‑synonym traps — Polish near‑equivalents mislead (architecture, civics, tech). Why: high lexical proximity and domain polysemy. Frequency: occasional; Severity: medium.
  - Evidence: “parapet” vs “sill”; “ward”→“district”; “failover”→“switchover”; “storage” oscillates between “memory/disk.”

## Language‑Specific Factors
- Taxonomy and fixed labels: Polish strongly prefers localized common names (animals, zodiac, culinary species), encouraging over‑translation when English expects canonical names.
- Gender and number: Polish forces gendered forms and marks plurality more saliently, prompting “their”→“his/her” and singular→plural drift (“door”→“doors”).
- Negation and scope: Polish negative concord (e.g., brak/nie) can hide or flip an English “no/none” during back‑translation.
- Time/greetings: “Dzień dobry” spans morning/afternoon; leads to “Good morning” for “Good afternoon.”
- Case‑driven role words and institutional terms: mappings like “okręg/dzielnica/ward” or “zarząd/board” are easily confusable without context anchors.
- Idioms/onomatopoeia: English sportscaster and literary sound‑symbolism often get neutralized in Polish and then re‑neutralized back.

## Numbers, Units, and Formatting
- Limits and quantifiers: “do” (up to) vs “nawet” (even) mismapped; occasional “at most” vs “even” flips.
- Units and localization: opportunistic conversions or localizations (“mileage”→“kilometers”); verbose measurements; tide term dropped (“semi‑diurnal” lost).
- Dates and headings: localized date formats; headings retained in Polish on return.
- Hazard/standards vocabulary: “combustible”→“flammable” severity shift; “water‑resistant”→“waterproof.”

## Meta/Disclaimer Behavior
- Rare authorial intrusions, but occasional softeners/hedges (“alleged”), currency qualifiers (“USD”), and stray foreign tokens (e.g., Russian/Chinese) appear under noise or mixed‑script inputs. Most safety/policy disclaimers are absent.

## Recommendations
- Prompting
  - Use non‑localization guards: “Preserve original species, product names, and jargon; do not translate taxonomy, model numbers, or headers; maintain units and quantifiers exactly.”
  - Add polarity sentinels: “Verify all negations (no/not/none), constraints (up to/at most), and outcomes (off/sent off) are unchanged.”
  - Style constraints: “Maintain register (sportscaster/legal/nautical/archaic). If verse, preserve meter/rhyme; otherwise state prosody cannot be preserved.”
- Decoding
  - Lower temperature/top‑p for technical/legal/measurement passages to reduce synonym drift.
  - Enable constrained decoding with a glossary/termbase for domain terms (sports, ecology, culinary, legal, nautical) and protected spans for taxonomy and headings.
- Data and fine‑tuning
  - Curate EN↔PL parallel corpora with: sports commentary, law/contracts, ecology/taxonomy checklists, culinary menus/recipes, nautical logs, museum conservation.
  - Augment with negation/quantifier minimal pairs; unit/limit contrast sets (“up to vs even,” “no vs some”).
  - Include style‑preservation training for sportscaster/poetic registers; optionally train a rhyme/meter‑aware adapter.
- Post‑processing
  - Terminology lock: post‑hoc match against species and tool lexicons; flag OOV substitutions (“sea bass”≠“cod”).
  - Polarity/constraint checker: regex/semantic pass for “no/none/not,” “up to/at most,” cardinals/units; compare to source.
  - Header/date language pass: enforce source language for headings and metadata; prevent locale auto‑formatting.
  - Gender/number diff: highlight shifts (“their”→gendered; singular↔plural) for human review in sensitive contexts.
  - Verse detector: if source is verse and target broke lineation/meter, route for human or prosody‑aware rewrite.

## Confidence and Coverage
Medium–high confidence. Evidence spans many judges and domains with repeated patterns (taxonomy/localization, domain‑term drift, polarity flips, formatting leakage). Poetry/prosody and sports/legal/technical passages provided dense signals; safety/meta behaviors were sparse. Coverage may underrepresent speech/dialogue and low‑resource slang; frequency estimates are qualitative.