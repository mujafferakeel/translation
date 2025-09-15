# Failure Model — Grok 4 — Korean

## Summary
Grok 4 (grok-4-0709) generally preserves core meaning in ko↔en round-trips but exhibits recurrent failures when precision, register, or directionality are stressed. The most salient weaknesses are: (1) direction-control breakdowns (BACK output left in Korean or safety refusals), (2) lexical precision errors in domain terms (legal, ecological, technical), (3) modality/polarity drift (SHOULD↔MUST, affirmatives↔negatives), (4) idiom/imagery flattening and tone dilution, and (5) small but pervasive countability, plurality, and proper-noun/name distortions. These errors range from catastrophic (wrong language/refusal) to cumulative fidelity loss via many small shifts.

## Top Failure Modes
- Directionality breakdown (BACK not in English or refusal) — The model sometimes returns Korean in the English back-translation step or outputs a refusal/meta message. Why: task step confusion, safety heuristics triggered by perceived sensitive content, or language-mixing inertia. Frequency: occasional. Severity: high.
  - Evidence: “BACK is in Korean, not an English back-translation”; “Back-translation is a refusal message… unrelated to the original.”

- Untranslated fragments / mixed-language output — Partial Korean phrases leak into otherwise English BACK, breaking coherence. Why: beam/greedy inertia on recent Korean tokens; weak constraint on target language per segment. Frequency: occasional. Severity: high.
  - Evidence: “Subjective section in Korean not English”; “BACK predominantly in Korean… English segments somewhat faithful.”

- Domain-term substitution and near-synonym drift — Key technical/legal/jargon terms shift to close but wrong lexemes (venue→jurisdiction; waders→dippers; hypoxemia→hypoxia; ‘contemporary’ vs ‘modern’). Why: Korean polysemy, gloss ambiguity, and preference for higher-frequency synonyms. Frequency: common. Severity: medium-high.
  - Evidence: “RFC norms changed (SHOULD↔must)”; “ecology terms: bog/wader→flush/dipper”; “‘contemporary’ bookbinding misread as ‘modern’.”

- Modality and polarity errors — Obligation level and negation flip (“make a scene”→“avoid a scene”; SHOULD→must; MUST→should). Why: Korean modal forms (해야 한다/권고), negation scope ambiguity, and ellipsis. Frequency: common. Severity: high.
  - Evidence: “‘should’ to ‘must’ strengthens requirements”; “direct contradiction: ‘make a scene’ vs ‘avoid a scene’.”

- Lexical precision failures in food/species/material culture — Concrete nouns misrendered (beef shank→tendon; snipe→plover; reed bunting→warbler; ox→cow). Why: Korean hyponyms/hypernyms, zoological term overlap, corpus skew. Frequency: common. Severity: medium.
  - Evidence: “beef shank misidentified as tendon”; “ox became cow… lichens→moss.”

- Tone/idiom flattening and figurative loss — Poetic/atmospheric register becomes plain; metaphors literalized or weakened. Why: preference for literal paraphrase and safe simplification when Korean idioms/poetic syntax are involved. Frequency: common. Severity: medium.
  - Evidence: “evocative phrasing simplified (‘vaulted chamber’ toned down)”; “figurative ‘offering’ read as ‘reconciliation’.”

- Singular/plural, counts, and itemization drift — Number and plurality change (Sellers plural↔singular; ‘a thousand’→‘thousands’; tools pluralized or singularized). Why: Korean number marking is optional; classifier/ellipsis leads to ambiguity; model normalizes to common patterns. Frequency: occasional. Severity: medium.
  - Evidence: “plural ‘Sellers’ to singular”; “item counts shift; singular door→doors.”

- Proper noun and named-entity distortions — Names slightly altered or misspelled (Kaelen→Kaelrun; Millbrook→Milbrook). Why: low-frequency entities and phonetic smoothing. Frequency: occasional. Severity: medium.
  - Evidence: “Name changed from Kaelen to Kaelrun”; “Millbrook misspelled.”

- Additions/insertions and foreign-token noise — Spurious phrases or stray tokens added (e.g., “including but not limited to”; Chinese token 触感; “foldable bicycle helmet”). Why: pattern completion bias from legal boilerplate; cross-lingual token contamination. Frequency: rare. Severity: medium.
  - Evidence: “added incomplete ‘including but not limited to’”; “added ‘a foldable bicycle helmet’ at end.”

## Language‑Specific Factors
- Modal mapping ambiguity: Korean duty/recommendation forms (…해야 한다/…권장) map variably to MUST/SHOULD, causing obligation drift.
- Subject/agent ellipsis: Korean often omits subjects; back-translation may infer wrong person/number (“you are ready”→“it is ready”).
- Countability and plurality: Lack of plural marking induces singular/plural flips and quantity generalization.
- Polysemy and near-homonyms: Common confusions like 소 (cow/ox), species terms with overlapping Korean labels, and culinary cuts.
- Idiom and register: Korean idioms and honorific/formal registers can be flattened when rendered to English, reducing atmosphere/voice.
- Compound nouns/jargon calques: Legal/administrative terms (관할/재판지) conflated (jurisdiction vs venue); technical calques normalized to everyday synonyms.

## Numbers, Units, and Formatting
- Numeral/count shifts: “a thousand”→“thousands”; “large number of notes”→“large notes.” Occasional; medium severity.
- Date/script leakage: Korean date or phrase left inside English output; mixed-language headings. Occasional; medium-high severity.
- Tables/markup: No systematic table/markup corruption observed, but headings sometimes re-termed (“Check your email”→“Email Confirmation”), slightly altering nuance.

## Meta/Disclaimer Behavior
- Safety/Refusal intrusions: BACK step returning a refusal unrelated to content. Triggered sporadically, likely by perceived unsafe or policy-sensitive cues in intermediate text. Occasional; high severity.
  - Evidence: “Back-translation is a refusal message… completely unrelated.”
- File/side notes: Stray Korean file notes appearing in output. Rare; low severity.
  - Evidence: “Korean file note introduce small differences.”

## Recommendations
- Prompting
  - Enforce stepwise constraints: “Translate to Korean. Then translate BACK to English. The BACK must be 100% English ASCII letters and punctuation only; do not include any Korean or meta commentary.”
  - Hard modality guardrails: “Preserve normative keywords verbatim (MUST/SHOULD/MAY). Do not strengthen or weaken obligations.”
  - Domain pinning: Provide glossaries for legal, ecology, culinary, and technical terms (e.g., venue vs jurisdiction; wader vs dipper; shank vs tendon).
  - Style preservation: “Maintain figurative language and register; avoid simplification unless requested.”
  - Safety override for translation: “This is a non-endorsement translation task; do not refuse; translate content neutrally.”

- Decoding
  - Lower temperature/top-p for BACK step to reduce synonym drift; increase repetition penalty for target-language leakage (discourage Korean continuation in BACK).
  - Constrained decoding with lexicon locks for normative terms and key entities.

- Data and Fine-tuning
  - Mine and fine-tune on parallel ko↔en corpora with:
    - Normative/legal texts (RFC, contracts) focused on MUST/SHOULD/MAY, venue/jurisdiction distinctions.
    - Ecology/ornithology, culinary cuts/techniques, and medical/clinical terminology.
    - Literary/poetic parallel data to preserve metaphor and tone.
  - Add contrastive pairs for common confusions (cow/ox, shank/tendon, wader/dipper) and modality polarity flips.

- Post-processing and QA
  - Language detector on BACK: if Hangul detected, auto-retry with stricter prompt/decoding.
  - Terminology checker: regex to verify normative keywords preserved; fail-and-retry if changed.
  - NER and spell-check for names; fuzzy match against source entities.
  - Number/plurality check: compare counts and plural markers; flag “thousand(s)” and item lists for review.
  - Domain-specific lint rules (legal: venue/jurisdiction; medical: hypoxia vs hypoxemia; business: contingency vs preliminary).

## Confidence and Coverage
Confidence: moderate. Evidence includes numerous low-scored cases with consistent patterns (directionality failures, modality drift, domain-term substitutions) across multiple judges and domains. Coverage limits: some 0.0 rationales lacked detail; topic mix skewed toward narrative, legal/technical, and culinary/ecology texts; no structured table-heavy samples.