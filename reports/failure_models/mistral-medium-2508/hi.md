# Failure Model — Mistral Medium 3.1 — Hindi

## Summary
Mistral Medium 3.1 generally preserves structure but shows recurrent fidelity breaks in Hindi round‑trip: polarity flips on key semantics, drift of technical/legal terms, culturally “domesticated” imagery in literary passages, and instability on proper nouns and numbers. The most damaging weaknesses are meaning reversals of operational terms (e.g., face up/down; nod/shake; mobilization/demobilization), softening of RFC/legal modality (MUST→should), and domain term substitutions (volatile→volcanic; combustible→flammable; person-throughput→vehicle capacity). Poetry/literary translations often inject Indic defaults (minarets, ghee, car horns) or swap seasonal images (frost→dew). Occasional artifacts include untranslated foreign words and meta headers.

## Top Failure Modes
- Polarity and orientation reversals — Antonyms or directional opposites are flipped (yes↔no, up↔down, increase↔reduce), breaking core logic; likely caused by ambiguous Hindi paraphrases and model preference for fluent rather than constrained mappings. Frequency: common; Severity: high.
  - Evidence: “face up” cards rendered “face down,” breaking game rules; child’s nod yes rendered as head‑shake no; “reduce venous return” rendered as “improve.”
- Domain term drift and false-friend substitutions — Precise technical/legal terms map to near neighbors or false friends, changing specifications. Frequency: common; Severity: high.
  - Evidence: “mobilization costs” became “demobilization”; “volatile compounds” became “volcanic”; “combustible liquid” → “flammable liquid”; “admission control” → “access control.”
- Modality and requirement softening — Normative force reduced in standards/official text (MUST/SHOULD/MAY). Frequency: occasional; Severity: medium-high.
  - Evidence: consistent MUST→should; “consider establishing” hardened to “establish,” or the reverse in other samples (should→must), altering obligations.
- Proper noun/name and unit/detail instability — Names, labels, and small numerics drift; domain labels get localized incorrectly. Frequency: occasional; Severity: medium.
  - Evidence: “Ember”→“Amber,” “Lin”→“Linn,” temperature 72→70; “person-throughput” → “private-vehicle capacity”; “curbside” → “sidewalk.”
- Literary domestication and imagery substitution — Cultural defaults and generic synonyms replace precise images, eroding tone and meaning. Frequency: common in literary; Severity: medium.
  - Evidence: “towers/dome” → “minarets/domes”; “frost/iron” → “dew/rusty metal”; “charcoal” → “coal”; “ghost on the wall” → “shadow”; added “ghee,” “car horns.”
- Omission/addition and artifact leakage — Minor omissions of key clauses; occasional added translator notes; stray foreign words. Frequency: occasional; Severity: medium.
  - Evidence: missing introductory paragraph; untranslated Russian/Hindi/German word within line; “Translation:” header added.

## Language‑Specific Factors
- Hindi polarity and aspect mapping:
  - “Face up/face down” can be paraphrased variably (“खुला/उलटा,” “ऊपर की ओर/नीचे की ओर”); loose paraphrase leads to inversion.
  - Reduce vs improve often expressed with light verbs; “कम करना/घटाना” vs “बेहतर करना” can flip if context isn’t anchored.
- Normative modality:
  - English MUST/SHOULD/MAY lack direct everyday Hindi equivalents; “अवश्य/चाहिए/हो सकता है” are often softened in back‑translation if not anchored to RFC sense.
- Technical lexicon false friends:
  - Volatile (“उड़नशील”) confused with volcanic (“ज्वालामुखीय”); combustible (“दहनशील/ज्वलनशील”) vs generic flammable; admission vs access control; curbside (कर्ब/सड़क किनारा) over‑generalized to फुटपाथ (sidewalk).
- Gender and pronoun ambiguity:
  - Hindi third-person pronouns are gender‑neutral in form; back‑translation can flip “his/her” (e.g., “her shadow” → “his shadow”).
- Cultural domestication tendency:
  - Literary passages shift to familiar Hindi imagery (minarets, ghee, car horns), and seasonal/agrarian terms (frost→dew) get normalized, altering source scenes.

## Numbers, Units, and Formatting
- Numbers and small facts: occasional drift or rounding in temperatures, times, or counts (e.g., 72→70; “hundreds”→“a hundred”).
- Directions/orientation misrendered (eastbound → coming from the east), affecting navigation/accident reports.
- Tables/headers/metadata: occasional injected headers (“Translation”), bold text; rare untranslated snippets (Russian/German).
- Units themselves are mostly preserved; no systemic unit conversion errors observed.

## Meta/Disclaimer Behavior
- Occasional translator notes or meta headings inserted in output even when not requested; occurs more with expository or formal requests where the model “frames” the translation.
  - Evidence: “Translation (English)” added; mild formatting insertions in guidance text.

## Recommendations
- Prompting
  - Use a constrained brief: “Translate literally with no additions/omissions. Preserve polarity, orientation, numbers, names, and domain terms. Do not localize imagery or modernize.”
  - Add guardrails: “Do not change MUST/SHOULD/MAY; keep them as MUST/SHOULD/MAY equivalents: MUST=अवश्य/अत्यावश्यक, SHOULD=उचित/चाहिए, MAY=संभव/हो सकता है.”
  - Provide mini‑glossaries per domain: volatile=उड़नशील; combustible=दहनशील/ज्वलनशील; curbside=कर्ब/सड़क किनारा; person-throughput=व्यक्ति-आवागमन क्षमता; face up=खुली तरफ ऊपर; face down=उलटी तरफ ऊपर.
  - Ask for a post‑pass checklist: “List numbers, directions, names, and modals you preserved.”
- Decoding
  - Lower temperature/top‑p to reduce paraphrasing; enable a constrained decoding mode with a protected term list for domain keywords and named entities.
- Data and fine‑tuning
  - Fine‑tune on Hindi↔English parallel corpora emphasizing:
    - Contrastive pairs for polarity (reduce/improve; up/down; yes/no; eastbound/westbound).
    - RFC/legal modality mappings with tests for exact preservation.
    - Domain lexicons (medical, transport, beekeeping, urban planning) with glossary supervision.
    - Literary datasets that penalize cultural domestication and reward image fidelity.
  - Add round‑trip consistency training and adversarial tests for antonym traps and number perturbations.
- Post‑processing
  - Automated validators:
    - Numeric/direction diff checker; flag any changes.
    - Modal verb auditor (MUST/SHOULD/MAY map).
    - Named‑entity consistency (copy‑mode for names, ship titles, product varieties).
    - Polarity/orientation lints for patterns: face up/down, nod/shake, increase/reduce.
  - Glossary enforcement using regex/lexicon substitution for high‑risk terms before finalization.
  - Flag and strip meta headers or non‑target language artifacts; detect and translate any residual foreign words.

## Confidence and Coverage
- Confidence: medium-high. The error patterns recur across diverse judges and domains, with multiple independent citations for polarity flips, term drift, and literary domestication.
- Coverage limits: Evidence is from lowest‑scored slices, skewed toward error cases and literary/technical stress tests; fewer pure numeric/unit failures observed; Hindi‑specific diagnosis inferred from back‑translation behavior rather than direct source ↔ target pairs.