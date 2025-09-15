# Failure Model — Gemini 2.5 Pro — Swahili

## Summary
Overall, Gemini 2.5 Pro’s Swahili translation keeps broad meaning but shows recurrent drift on facts, register, and imagery under pressure. The most salient weaknesses: time handling (AM/PM and Swahili clock), precision loss in technical/legal terminology, consistent tone flattening in poetry and stylized prose, entity/term substitutions (proper names, species, culinary items), and gender/pronoun instability. Safety and instructional texts suffer from altered imperatives that change meaning.

## Top Failure Modes
- Time conversion and Swahili clock confusion — Changes absolute times and schedules; AM/PM flips and 5 p.m. becomes 11 p.m. Root cause: interference between English clock and Swahili “saa” system (+/−6‑hour offset), plus normalization to local conventions then back-translation literalizes numerals. Frequency: common; Severity: high.
  - Evidence: “6:45 a.m. → 12:45 a.m.” warps incident chronology; “Thursday 5 p.m. → 11 PM” alters a key deadline.

- Register and term precision loss (legal/technical/business) — Replaces domain terms with general synonyms, shifting nuance or facts (e.g., drivetrain→operating system; jury→court council; failover→transfer; hashed→encrypted). Why: limited Swahili parallel domain data; preference for high‑frequency paraphrases; uncertainty with loanwords. Frequency: common; Severity: high.
  - Evidence: “drivetrain” mistranslated; “minute order/sanctions” generalized to “summary/fine.”

- Stylized language flattening (poetry, metaphor, idiom) — Evocative imagery and rhyme become literal paraphrase; metaphors swapped or softened, invented agents appear (“Sakitu” for frost). Why: low exposure to Swahili poetic equivalents; decoding favors literal clarity; hallucination when encountering rare imagery. Frequency: common; Severity: medium–high.
  - Evidence: rhyme lost and whimsical tone flattened; “frost traces” becomes “Sakitu draws,” neon→colorful.

- Entity and specific-item drift — Proper names, place types, initiative names, species/culinary items, and materials changed or generalized (Park→Garden; Cool→Quiet; curlew→snipe; tomato paste→canned tomato; copper→brass). Why: lexical gaps and synonym overuse; back-translation picks different near‑synonyms; sparse term grounding. Frequency: common; Severity: medium.
  - Evidence: “Park” repeatedly rendered “Garden”; “curlew→snipe,” “tomato paste→canned tomato.”

- Imperative and safety semantics shifts — Verbs in advice/instruction contexts altered, changing safety meaning (move steadily→move quietly; rope handy→essential). Why: aspect/modality mismaps; tone smoothing; Swahili imperative nuance mapping. Frequency: occasional; Severity: high.
  - Evidence: Rockfall advice verb changed; equipment necessity escalated.

- Pronoun/gender instability — Gender-neutral references turn male; specific gender flipped; dialogue roles altered. Why: Swahili “yeye” (he/she) ambiguity; agreement patterns force choices; back-translation resolves ambiguity incorrectly. Frequency: occasional; Severity: medium.
  - Evidence: child’s gender swapped; “their”→“his.”

- Numbers/quantities and units drift — Counts and measurement terms changed (blocks→streets; summers→springs; bulk fermentation→initial fermentation). Why: term gaps; morphological preference for common words; metonymy misread. Frequency: occasional; Severity: medium.
  - Evidence: “12 city blocks” rendered “12 streets”; fermentation stages renamed.

- Meta/disclaimer intrusions and additions — Translator notes or extra instructions inserted (“(sakitu)”; “in capital letters”), minor filler phrases. Why: safety/pedagogy priors; attempt to clarify unknown terms. Frequency: rare; Severity: low–medium.
  - Evidence: added translator note; added capitalization instruction.

## Language‑Specific Factors
- Swahili clock offset: Everyday Swahili time counts from roughly 6 a.m. (saa 1) to 7 a.m. (saa 2), etc. The model appears to localize to “saa” internally, then back‑translate numerals literally, causing 5 p.m.↔11 mappings and similar flips.
- Gender neutrality: “Yeye” is gender-ambiguous; back-translation often forces a gender, producing swaps where the source was explicit or neutral.
- Loanword/term handling: Many technical/legal terms rely on standardized borrowings (e.g., ushahidi “evidence,” hati ya mashtaka “indictment,” faili la kumbukumbu “minute order”). The model often picks colloquial paraphrases instead of established terms.
- Idiom/poetry: Swahili poetic devices (parallelism, alliteration, ideophone usage) differ; naive literal mapping erases rhyme/measure; rare nature imagery triggers substitutions.
- Noun class agreement can push generalization: precise species/materials drift to broader classes as agreement-driven paraphrases are chosen.

## Numbers, Units, and Formatting
- Times: Frequent AM/PM shifts and hour offsets; occasional 24h vs 12h inconsistency.
- Dates/seasons: Season substitutions (autumn↔winter); daypart dilution (late evening→evening).
- Quantities/units: Domain units and counts simplified (blocks→streets; waders→waterfowl; alluvial terraces→ridges).
- Names/labels/UI text: Button labels and initiative names altered (Checkout→Pay; Cool Streets→Quiet Streets).
- Conversions/inventions: Occasional invention or escalation of requirement (handy→essential) and added timing specificity.

## Meta/Disclaimer Behavior
- Occasional translator notes or bracketed glosses appear, and stray instructional asides get inserted. Triggered by uncertain terminology or perceived need to clarify unfamiliar imagery/loanwords. Rare but contaminating in literary or formal texts.

## Recommendations
- Prompting
  - Pin times, numbers, currencies, and names: “Preserve all times/numbers/names exactly. Do not convert between time systems; do not change AM/PM.”
  - Register lock: “Maintain the original register and domain terminology. Prefer established Swahili technical/legal terms. If uncertain, keep the source term in parentheses.”
  - Style fidelity: “Preserve metaphors, rhyme, and figurative language; avoid paraphrasing unless required by grammar.”
  - Gender/neutrality: “Preserve explicit gender; where ambiguous, keep it ambiguous in Swahili and avoid forcing gender in back-translation.”
- Decoding
  - Lower temperature/top‑p for legal/technical texts to reduce paraphrase; enable constrained decoding with a terminology list for critical domains.
  - Use placeholder tagging for time/number entities during translation to prevent conversion, then restore.
- Data and Fine‑tuning
  - Augment with Swahili parallel corpora in: legal (court documents), engineering/IT (security, infrastructure), scientific (ornithology, materials), culinary.
  - Build a Swahili termbase/glossaries for: clocks/time expressions, UI labels, safety verbs, species, ingredients. Fine‑tune with contrastive examples penalizing synonym drift and time conversions.
  - Include Swahili poetry/literary corpora and aligned figurative-language datasets to improve stylistic preservation.
- Post‑processing QA
  - Automated checks for: time consistency (regex detect AM/PM shifts; flag 5↔11, 6:45↔12:45 patterns), proper noun stability (NER diff), domain term mapping (terminology matcher), imperative/safety verb changes, and gender pronoun consistency across a document.
  - Domain-specific lint rules: e.g., “failover” must not become “transfer”; “hashed” vs “encrypted.”
  - Human-in-the-loop review for high-risk genres: safety instructions, legal notices, UI strings, poetry.
  
## Confidence and Coverage
Confidence: medium-high. The error families are repeatedly evidenced across many judges and samples, with dense signals on time errors, register loss, and entity drift. Coverage limits: we infer from back-translation judgments across varied domains; some issues may originate in either direction of the round trip. Topic mix is broad but skewed toward literary, legal/technical, and guidance texts; numerical/table-heavy content is underrepresented.