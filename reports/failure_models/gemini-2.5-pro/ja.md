# Failure Model — Gemini 2.5 Pro — Japanese

## Summary
Across low-scoring round‑trip cases, Gemini 2.5 Pro systematically drifts register and lexical specificity when translating to/from Japanese. The most salient weaknesses are: persistent over-formalization/verbosity, instability of proper nouns and institutional terms, metaphor/idiom literalization, modality/negation shifts, and small-but-meaningful technical/jargon substitutions. These often arise in dialogue, poetic or highly stylized prose, legal/administrative text, and domain‑jargon passages.

## Top Failure Modes
- Register drift to polite/formal and wordy prose — Rewrites concise or colloquial voice as 丁寧/説明的 Japanese, then back to English as more formal and verbose; natural subtext is flattened.
  - Why: Japanese politeness norms and “safe” explanatory defaults; decoding prefers explicitness over ellipsis.
  - Frequency: common; Severity: medium-high.
  - Evidence: “warm, direct voice” became “formal, wordy” with CTA altered; repeated “please” added to imperatives.

- Proper-noun and term instability (names, titles, orgs) — Changes entity forms, titles, or specific terms: “Charter→constitution,” “Student Council→Student Government Association,” fantasy/place names altered; spellings drift (Rin/Lin).
  - Why: Lack of hard constraints and preference for common Japanese equivalents; katakana/kanji back-and-forth noise.
  - Frequency: common; Severity: high when identity/continuity matters.
  - Evidence: Multiple notes of Council/Association swaps; names like Murkwater→Marwater, Xylos→Zyros.

- Metaphor/idiom flattening and imagery loss — Figurative language paraphrased into literal/prosaic phrasing; key metaphors swapped (trail→footprint; ox→bull), affecting tone and meaning.
  - Why: Bias toward explicitation in JA; difficulty aligning culturally bound metaphors; limited figurative parallel data.
  - Frequency: common; Severity: medium.
  - Evidence: Poetic “tide of graphs” softened; “carcasses of books→remains,” “refined and revealed→polished then appears.”

- Modality, polarity, and nuance shifts — “Should→must,” possibility→certainty, negation flips in dialogue; double negatives mishandled; eligibility/affected scope errors.
  - Why: Japanese modality/tense underspecification; negative scope and ellipsis in JA lead to ambiguous mappings.
  - Frequency: occasional; Severity: high.
  - Evidence: “Not alone→Alone”; “consistent with→not inconsistent with”; “eligible→affected.”

- Domain‑jargon substitution drift — Precise terms replaced with near-synonyms, altering technical meaning (legal, culinary, games, aviation).
  - Why: Preference for generic high-probability synonyms; insufficient domain lexicon anchoring.
  - Frequency: occasional; Severity: medium.
  - Evidence: “Wild Jet counts as both draws” rule misrendered; “seared→sautéed,” “mechanical inspection→aircraft inspection.”

- Number, countability, and plurality mismatches — Singular/plural, collective vs countable items drift; specific counts or items altered.
  - Why: Japanese number/plural marking is optional; English requires explicit number.
  - Frequency: occasional; Severity: medium.
  - Evidence: “one sock→a pair”; Sellers/Buyers plural→singular; “cold spot→cold spots.”

- Person/voice and perspective shifts — First/third person substitutions, indirect reporting; corporate/first-person voice injected into neutral announcements.
  - Why: Subject omission in JA; pragmatic inference replaces original narrator stance.
  - Frequency: occasional; Severity: medium.
  - Evidence: “you→I/we” in sea journal; formal announcement turned first-person corporate.

- Small factual substitutions in culturally specific domains — Sports scoring, fauna, press types, etc., mapped to near but wrong categories.
  - Why: Overgeneralization of terms; knowledge priors override source specificity.
  - Frequency: occasional; Severity: low-medium.
  - Evidence: “runs→points” (cricket); “wasps→bees”; “regional→local newspapers.”

## Language‑Specific Factors
- Politeness/register system: The model overuses 丁寧語/敬語, adding “please,” softeners, and indirectness; back-translations amplify formality.
- Zero pronouns and ellipsis: JA omits subjects/modals; the model fills gaps inconsistently, causing person, modality, and negation scope errors.
- Singular/plural neutrality: Leads to count mismatches (“one sock→pair,” collective/plural alternations).
- Loanwords and institutional terms: Confusion between 憲章/会則/規約; 生徒会 vs 学生自治会; Ward/Parish mapping; katakana transliteration variance (Rin/Lin).
- Idioms and figurative compression: Tendency to explicate or literalize Japanese metaphors into plain prose in English.
- Script/orthography variance: Proper-name spellings drift across katakana/romanization; poetic lineation and rhythm often flattened.

## Numbers, Units, and Formatting
- Numbers/units: Mostly preserved; occasional rule/count errors and pluralization issues. Rare unit/date conversions observed.
  - Evidence: Card-game rule counting misrendered; “last five passwords→last 5 times.”
- Formatting/markup: Adds parentheticals/explanations; slight header/URL formatting changes; parentheses added in lists.
  - Evidence: “Chain of Custody” reformatted; sponsor URL slightly altered.

## Meta/Disclaimer Behavior
- Occasional explanatory notes or parentheticals inserted in legal/technical text (“etc.,” brief glosses), but no strong safety disclaimers.
  - Triggers: Legal explanations, guild/term definitions, historical notes.
  - Evidence: Added explanatory parentheticals for legal terms; “I think,” “etc.” inserted in guidance emails.

## Recommendations
- Prompting
  - Style lock: “Preserve original register, brevity, and metaphors. Do not add politeness markers or explanations. Mirror sentence boundaries and line breaks.”
  - Entity protection: “Copy proper nouns, titles, product/quest names verbatim unless a known Japanese official translation exists.”
  - Modality/negation guard: “Preserve modality strength (should/may/must) and negation exactly.”
  - Domain termbase: Provide mini-glossaries (legal, culinary, sports, aviation) and require strict usage.
  - Ambiguity retention: “If subject/modality is ambiguous in Japanese, keep it ambiguous; do not resolve with I/you/we.”

- Decoding
  - Lower temperature and increase repetition/constraint penalties for entities and terms.
  - Constrained decoding with protected spans for names and key terms; use placeholders for entities during JA pass, then restore.
  - Force line-break preservation for poetry/script; disable sentence fusion.

- Data and fine‑tuning
  - Curate EN↔JA parallel corpora emphasizing:
    - Dialogue with subtext, colloquial voices; poetry/literary style retention.
    - Legal/administrative templates with modality/term stability.
    - Domain glossaries (sports scoring, cooking techniques, TCG rules).
    - Institutional terminology mapping (Charter/憲章 vs 会則; Council vs Association).
  - Contrastive training for register fidelity and metaphor preservation.
  - Hard-negative pairs for polarity and eligibility/affected distinctions.

- Post‑processing and QA
  - Automated checks: Proper‑noun diffs, modality/negation diff, count/singularity checks, glossary conformance.
  - Lint for added politeness (“please,” ください/〜いたします) when source lacks it.
  - Back‑translate spot checks focused on dialogue lines, rules, and counts.
  - Terminology review step with a locked glossary and unknown-term flagging.

## Confidence and Coverage
- Confidence: Medium. Dozens of low‑scored rationales show consistent patterns (register drift, entity/term instability, metaphor loss, modality errors). Numbers/formatting and meta insertions appear less frequent.
- Coverage limits: Evidence spans multiple genres with varied judges; fewer explicit numeric/unit failures and limited table/markup cases. Findings are strongest for dialogue, poetic/literary text, legal/administrative prose, and jargon-heavy passages.