# Failure Model — Grok 4 — Spanish

## Summary
Grok 4 shows strong baseline fidelity but recurring Spanish-specific leakage and precision issues in round‑trip (EN↔ES↔EN) workflows. The most salient weaknesses: language leakage/untranslated Spanish segments (especially headers, signs, titles, and quoted items), technical/terminology drift (IT, legal, scientific), systematic tone/register flattening, pronoun/gender and person shifts, and small yet consequential factual/temporal errors. Formatting artifacts (ES dates, connectors) also recur.

## Top Failure Modes
- Language leakage and partial untranslated Spanish — Back-translation leaves chunks in Spanish (sections, first paragraphs, riddles, signs, headers), breaking language consistency and omitting required English output; often triggered by quoted or typographically distinct spans.
  - Why: Insufficient span-level language control; heuristics treat headers/titles/quoted text as “keep as is.”
  - Frequency: common; Severity: high
  - Evidence: “articles switched to Spanish after first paragraph”; “riddle and section headers left in Spanish.”

- Terminology drift and precision loss — Domain terms mapped to near-synonyms or wrong categories (IT, legal, technical, ecology), altering meaning.
  - Why: Preference for high-frequency synonyms; inadequate domain grounding or glossary use; ES–EN polysemy traps.
  - Frequency: common; Severity: medium–high
  - Evidence: “onshore → from inland”; “desktops → desks”; “loam → marl”; “tamper-evident → tamper-proof.”

- Tone/register flattening and stylistic dilution — Literary/marketing/legal voice softened; archaic or clipped register modernized; imagery weakened.
  - Why: Decoding favors generic fluency over stylistic constraints; paraphrasing bias in back‑translation.
  - Frequency: common; Severity: medium
  - Evidence: “evocative vocabulary replaced by generic synonyms”; “historical ‘Whereas’ modernized to ‘Considering that’.”

- Pronoun, gender, and person shifts — Neutral they/their becomes he/his; second person to third; role nouns gendered or altered.
  - Why: Spanish gendered morphology pressures and ambiguity resolution; alignment to typical distributions.
  - Frequency: occasional; Severity: medium
  - Evidence: “their → his throughout artist description”; “your → their in donor paragraph.”

- Proper names, culturally marked terms, and signage mishandling — Titles, event names, and signs either left in Spanish or unnecessarily Hispanized in English.
  - Why: False “do-not-translate” inference for capitalized strings/cultural items; over-assimilation into Spanish.
  - Frequency: occasional–common; Severity: medium
  - Evidence: “Vuelta al Atardecer not re-Englishized”; “sauce/cheese → salsa/queso”; “English signs rendered in Spanish.”

- Small but consequential factual/temporal shifts — Time-of-day, counts, and directionality altered though structure remains.
  - Why: Preference for common collocations; smoothing during paraphrase.
  - Frequency: occasional; Severity: medium
  - Evidence: “Wednesday evening → Wednesday afternoon”; “late evening → end of the afternoon.”

## Language‑Specific Factors
- Gendered inflection pressure: Spanish forces gender resolution, leading to he/his insertions on re-Englishizing neutral “they/their.”
- Person deixis: Spanish often defaults to third person or impersonal forms, which then persist back into English (“your” → “their”).
- Cognates and false friends: “escritorio/desktops,” “marga/loam,” “conductor/driver,” “batter/batsman” mismappings.
- Named constructs and idioms: Cultural terms and proper events can be over‑preserved in Spanish or over‑localized (e.g., translating generic “sauce” to “salsa”).
- Register normalization: Spanish normative prose encourages smoothing of clipped/poetic English, then not fully restored on the return pass.

## Numbers, Units, and Formatting
- Dates and locale artifacts: Spanish date strings bleed into English (“5 de August de 2023”), and section labels like “Asunto,” list connectors “y.”
- Labels/titles: Headers and example titles persist in Spanish on back‑translation.
- Conversions/inventions: Limited numeric fabrication observed; more often slight timeline shifts rather than unit changes.
- Tables/markup: No systemic table/HTML issues reported; the problem is chiefly header/label language leakage.

## Meta/Disclaimer Behavior
- Occasional intrusions like Spanish interjections (“¡Salud!”) or a stray courtesy line (“thank you”), typically around quoted or list contexts.
- Safety/policy disclaimers not observed as a pattern; intrusions are stylistic rather than policy-driven.
- Triggers: Quoted aphorisms, toasts, or list headings where the model infers preserved-language spans.

## Recommendations
- Prompting
  - Enforce strict language guards: “Translate all content back to English, including headers, signs, titles, riddles, quoted text, and images/alt text. Do not retain any Spanish unless it is an untranslatable proper name; otherwise provide the English equivalent.”
  - Add span policy: “For every capitalized or quoted span, translate it; if you decide not to translate, output [DNT] with reason.”
  - Specify register: “Preserve original person (2nd vs 3rd), pronouns (they/them), tense, and legal/technical terms exactly. Avoid modernization.”
  - Glossary and DNT lists: Provide domain glossaries (IT: desktop, fan‑out; Geo: onshore; Archaeology: loam, sherd) and proper-noun DNTs with English exonyms where applicable.
- Decoding
  - Lower temperature and reduce paraphrase for back‑translation; enable constrained decoding with terminology constraints.
  - Use repetition penalties sparingly; prioritize exact reinstatement of style tokens (pronouns/person).
- Data and fine‑tuning
  - Fine-tune on ES↔EN round‑trip corpora with penalties for language leakage in headings, signage, and quoted content.
  - Augment with gender‑neutral English → Spanish → English examples to preserve singular they; include legal/IT/science termbanks.
  - Include culturally marked items with required translation back to English (signs, titles), plus time-of-day disambiguation pairs.
- Post‑processing
  - Language-ID sweep to flag residual Spanish tokens/headers; auto-fix or send to fallback pass.
  - Pronoun/person checker to compare source and back‑translation for they/them and 2nd/3rd person consistency.
  - Term consistency checker with glossaries for high-risk domains; flag false friends (escritorio/desktop).
  - Date/timeline validator to detect ES date formats and time-of-day shifts.
  - Named-entity pass: NER + translation policy to ensure titles/signs are translated or annotated with [orig: ES, EN: …].

## Confidence and Coverage
- Confidence: medium-high for identified modes; patterns recur across many judges and documents with converging critiques.
- Coverage limits: Evidence skews toward mixed-domain prose and technical copy; fewer quantitative/table-heavy cases and limited safety/policy samples.