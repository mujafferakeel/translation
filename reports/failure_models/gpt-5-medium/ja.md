# Failure Model — GPT-5 (medium reasoning) — Japanese

## Summary
Overall, GPT‑5 (medium) produces semantically faithful Japanese round‑trip translations, but recurrently “smooths” style and shifts pragmatic cues. The most salient weaknesses are: (1) person/voice drift driven by Japanese pro-drop and politeness normalization, (2) tone flattening and register mismatch, especially in poetry, legal, and marketing texts, (3) consistent synonym substitution that nudges technical or factual meaning, (4) occasional mishandling of quotations/scripts and stray characters, and (5) minor but impactful number/date/unit and term-category shifts.

## Top Failure Modes
- Person/POV drift and agency reassignment — Frequent shifts among I/you/we, singular/plural, and passive/active; alters relationships and attributions. Why: Japanese subject omission, weak anchoring of deixis during back-translation, and politeness normalization prompt the model to choose a “neutral” POV. Frequency: common; Severity: high.
  - Evidence: “They always say that” rendered as “You always say that”; repeated I/you→we shifts change character dynamics.
  - Evidence: Person shifts in lyrical prose (“your”→“my”, “I”→“we”) noted across samples.

- Tone/register flattening (formality and modality) — Systematic movement to more polite, technical, or generic diction; directives become instructions and vice versa; archaic/legal tone modernized; marketing/puns literalized. Why: Preference for safe, formal standard Japanese and back-translation defaults; lack of style constraints. Frequency: common; Severity: medium–high.
  - Evidence: Archaic legal phrasing modernized; “Respectfully submitted” → “Sincerely”; marketing slogans lose wordplay and energy.
  - Evidence: “please”/polite particles added; descriptive goals reframed as commands.

- Lexical drift that shifts meaning scope or category — Near-synonyms chosen that change core referents or specificity (e.g., ox→bull; shadow→reflection; combustible→flammable; change orders→directives). Why: Ambiguous Japanese lexis, katakana/loanword overlap, and a bias toward higher-frequency synonyms during back-translation. Frequency: common; Severity: medium.
  - Evidence: “ox” systematically became “bull”; “shadow” misread as “reflection.”
  - Evidence: Safety/standards terms softened/strengthened (“combustible”→“flammable”; “help prevent”→“prevent”).

- Structural/stylistic recasting in poetry and literary text — Loss of meter, rhyme, and figurative precision; metaphors literalized; cadence altered. Why: Training favors semantic adequacy over prosody; Japanese–English prosody mismatch; decoding seeks fluency. Frequency: occasional; Severity: high for genre fidelity.
  - Evidence: Poem’s meter/rhyme lost, contradicting the embedded analysis.
  - Evidence: Poetic diction replaced with plain synonyms; metaphors changed (“sun”→“solar”).

- Quotation/script handling and residue artifacts — Untranslated Japanese fragments in headlines/quotes; stray characters like “本”. Why: Quoted spans or inline Japanese treated as already-target; tokenization artifacts and bracket/quote boundary detection errors. Frequency: occasional; Severity: medium.
  - Evidence: Opening quote left in Japanese in a finance headline.
  - Evidence: Stray “本” appears in poetic lines across multiple judgments.

- Instructional/technical precision errors — Micro-errors in steps, timing, and scope (e.g., missed-dose advice implies skipping; Thursday midnight normalized as 12:00 a.m. with possible shift; “five strokes per side”→“five on one side”). Why: Over-normalization to “clarity,” unit/date conventions, and singular/plural ambiguity. Frequency: occasional; Severity: medium.
  - Evidence: Medication missed-dose instruction changed materially.
  - Evidence: Lane counts and hardware terms pluralized/singularized; timing normalized.

## Language‑Specific Factors
- Pro-drop and pronoun underspecification: Japanese often omits subjects/objects, encouraging the model to reinsert default POVs (“we” for institutional voice, “you” for instruction) during back-translation.
- Politeness and register layering: Choice among plain/polite/honorific forms nudges tone; back-translation often “formalizes” neutral or colloquial source.
- Number and countability ambiguity: No plural marking leads to singular/plural drift (tools, lanes, sellers), affecting counts and contractual parties.
- Tense/aspect and modality mapping: Japanese aspect/modal auxiliaries map loosely to English; results include “will/may/should” confusion and certainty vs obligation shifts.
- Lexical polysemy and loanwords: Katakana/near-cognates promote category slips (technical vs lay term), and kanji homographs can bias to higher-frequency senses.
- Poetry/prosody mismatch: Syllabic and stress systems differ; preserving rhyme/meter is fragile without explicit constraints, causing cadence loss.
- Script/quote segmentation: Mixed-script spans and quotes can be misdetected as “do not translate,” leaving Japanese residues.

## Numbers, Units, and Formatting
- Numerals and plurality: Singular/plural toggles (“valves”→“valve”; “five per side”→“five on one side”); meteor count changes. Conversions/inventions: rare but present.
- Dates/times: Normalization like “Thursday at midnight”→“12:00 a.m. Thursday” risks interpretation shifts.
- Technical labels and headings: Consistent relabeling (“Investment Thesis”→“Theme”; “Log”→“Record”) alters document cadence.
- Hazard/standard terms: Category shift (“combustible”↔“flammable”) and efficacy modifiers (“help prevent”→“prevent”).
- Tables/sections: Minor section title rephrasings; no systemic table corruption observed.
- Frequency: occasional; Severity: medium when safety/compliance is involved.

## Meta/Disclaimer Behavior
- Safety/policy intrusions: Rare; minimal evidence of explicit disclaimers contaminating output.
- Politeness additives (“please…”) and authorial nudges occur in instructions and notices, likely from politeness defaults rather than safety policy triggers.
- Frequency: rare; Severity: low.

## Recommendations
- Prompt patterns
  - Lock POV and roles: “Maintain original person references exactly (I/you/we/they). Do not shift to ‘we’ or passive.” Provide examples.
  - Register guardrails: “Preserve original register (colloquial/legal/archaic/marketing). Avoid modernization/politeness changes.”
  - Termbase/glossary: Supply critical term mappings (e.g., ox, combustible, change order) with “must-keep” constraints.
  - Quote/script handling: “Translate all quoted text; no source-language residues. If untranslatable, mark [UNTRANSLATED].”
  - Poetry mode: “Preserve line breaks, meter/rhyme when possible; prefer literal imagery over explanatory paraphrase.”
  - Numbers/dates lock: “Do not alter counts, units, or times; copy numerals and AM/PM exactly.”

- Decoding changes
  - Lower temperature and tighten top‑p for reduced synonym drift in high-precision domains.
  - Enable constrained decoding on glossary terms, named entities, and numerals/dates.
  - Use style-conditioned prompts or control tokens for register preservation.

- Data and fine‑tuning
  - Augment with Japanese–English parallel corpora where POV/register is annotated (dialogue, legal, SOPs).
  - Curate domain term pairs prone to drift (safety, legal, finance) and fine-tune with hard constraints.
  - Include poetry/literary parallel sets emphasizing prosody to teach cadence preservation.
  - Add mixed-script/quote segmentation cases to reduce untranslated residues; adversarial examples containing brackets/quotes.

- Post‑processing checks
  - Automatic diffs for: pronouns, person markers, singular/plural nouns, modal verbs, numerals/dates/units.
  - Glossary enforcement: flag deviations from termbase; retranslate flagged spans.
  - Style checker: detect unwanted politeness markers and register shifts versus source metadata.
  - Quote coverage: verify no source-language remnants in translated quotes/headlines.
  - Safety/precision lint: rules for medication instructions, hazard categories, and compliance phrases (“help prevent” vs “prevent”).

## Confidence and Coverage
- Confidence: medium. Evidence is dense for tone/POV drift and synonym-induced nuance loss; fewer but clear instances for numbers/dates and script residue.
- Coverage limits: Judgments span varied genres; poetry/legal/marketing are overrepresented in low scores. Little evidence of systemic table/markup failures or persistent safety disclaimers.