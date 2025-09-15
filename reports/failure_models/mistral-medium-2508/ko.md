# Failure Model — Mistral Medium 3.1 — Korean

## Summary
Mistral Medium 3.1 produces fluent Korean but shows recurrent fidelity failures: severe truncation on longer inputs; unstable handling of proper nouns and domain terms (species, legal/safety, aviation); unit/temporal misreads (hours↔days, ft↔thousands of ft) and unsolicited metric conversions; and meaning drift from Korean-specific homonyms and register/pro-drop ambiguities causing role flips and tone shifts. Poetic and technical passages often get “smoothed,” losing specificity.

## Top Failure Modes
- Hard truncation/omission on long segments — Output cuts mid‑sentence, dropping major portions of content; when present, the translated part can be faithful, but large tails are missing. Why: length/decoding cutoff, weak long‑context persistence. Frequency: common. Severity: high. Evidence: “omits the vast majority after ‘Sigil Weave III’”; “final half missing with narrative/dialogue dropped.”

- Proper noun and taxonomy drift — Names, species, places, and product/skill terms are swapped with near neighbors or genericized. Why: sparse term coverage in ko data; preference for high‑probability synonyms; transliteration uncertainty. Frequency: common. Severity: high. Evidence: “curlew→cuckoo; cotton‑grass→cattail”; “Farafra misspelled, ‘Tropic of Cancer’ → ‘Amjadido’.”

- Domain‑term conflation (legal/safety/tech) — Key terms map to adjacent but different concepts, altering obligations or hazards. Why: Korean near‑synonyms collapse categories; model normalizes register. Frequency: common. Severity: high. Evidence: “venue→jurisdiction; ‘shall’ tone added”; “combustible→flammable; eye→face protection”; “tent ‘fly’ → bug netting.”

- Numbers/units misread or invented conversions — Magnitudes and timelines shift (ft vs thousands of ft; hours vs days); miles→km added without instruction. Why: Korean number formatting (천, 억), feet/AGL unfamiliarity; model “helpfulness” adding conversions. Frequency: common. Severity: high. Evidence: “aviation ceilings 1000–1500 ft rendered as 100–150 ft”; “4–6 business hours→4–6 business days”; “miles→kilometers.”

- Homonym/semantic misfire from Korean polysemy — Critical nouns/verbs map to wrong senses, flipping meaning. Why: Korean homonyms and Sino‑Korean variants; sparse disambiguation cues. Frequency: occasional. Severity: high. Evidence: “harm (해) read as ‘sun’ (해) in poem”; “parallax→time zones.”

- Voice, role, and modality shifts — Person and agency flip (“they→you,” narrator ‘we→I’); modals tightened/loosened (“should→must”), evidentials strengthened (“consistent with→matches”). Why: Korean pro‑drop and flexible honorific/register; model prefers normalized assertive tone. Frequency: occasional. Severity: medium. Evidence: “They→You alters dialogue dynamics”; “no known risk→no risk; will complete→must complete.”

- Object misidentification in material culture/instruments — Specialized artifacts mapped to more common analogs. Why: sparse ko lexicon for niche items. Frequency: occasional. Severity: medium. Evidence: “astrolabe→sextant”; “horns↔antlers; wading→swimming.”

## Language‑Specific Factors
- Homonym traps: 해 (sun/harm), 비행용 ‘플라이’ (tent fly) vs 모기장; 가연성(Combustible) vs 인화성(Flammable) are close but not identical.
- Pro‑drop and topic/subject marking invite agent/patient and person shifts; English speaker/addressee can be neutral in Korean and re‑expanded incorrectly in back‑translation.
- Register mapping: legal “shall/venue” vs Korean 법률체 tone; model often over‑formalizes or generalizes (“헌장” vs “헌법”).
- Taxonomy naming: Korean common names vary by region; model backs off to more familiar fauna/flora or food terms (e.g., sea bass variants), harming precision.
- Numerals: Korean uses 만/천 units and context markers (영업시간 vs 영업일; ft AGL vs 층/미터) that the model confuses, leading to zero‑drop or unit category errors.

## Numbers, Units, and Formatting
- Units: ft interpreted as tens/hundreds, not thousands; hours↔days; unsolicited miles→km conversions. Conversions/inventions occur.
- Dates/times: “business hours” vs “business days” swapped; forecast timing advanced.
- Structured text: headings rephrased, bullet labels altered; occasional translator notes or parentheses added.
- Tables/Specs: precise terms replaced by synonyms (“stipend→scholarship”, “blockers→risk factors”) reducing spec fidelity.

## Meta/Disclaimer Behavior
- Occasional translator notes and added parentheticals (explanations, acronyms) in otherwise plain translations.
- Instructional tone injected into neutral notices (imperatives in accessibility/safety content).
- Triggered by headings, policy/legal content, and glossary-like passages.

## Recommendations
- Prompting
  - “Translate completely, preserving all content order and line breaks. Do not omit or add. Do not convert units or timelines. Preserve proper nouns and technical terms verbatim; if uncertain, keep source term in parentheses. Maintain modality exactly (‘should/must’), and keep person/voice.”
  - For poetry/quotes: “Preserve metaphors and key images; avoid synonym substitution.”
  - For safety/legal: “Use venue=재판 관할 위치, combustible=가연성, flammable=인화성. Do not upgrade/downgrade risk language.”
- Decoding
  - Increase max output tokens and enable length penalties that favor completion; disable early stopping on section markers. Use constrained decoding with source-aligned sentence counts for long documents.
- Data/lexicons
  - Augment ko-en parallel corpora for: aviation weather (AGL, ceilings), legal venue/FOIA, outdoor gear (tent fly), taxonomy/culinary species, musical/piano pedagogy.
  - Build a Korean homonym disambiguation list: 해(해악 vs 태양), 플라이(텐트 외피), 면류 vs 스튜/차우더; integrate as term constraints.
  - Maintain glossaries for names, places, product/skill terms; enforce copy-through for matches.
- Fine-tuning targets
  - Penalize unsolicited conversions/additions; train contrastive pairs where unit/time alterations are negative.
  - Style retention for modality and evidential strength (“consistent with” vs “matches”).
  - Long-context continuation robustness on narrative and procedural docs.
- Post‑processing checks
  - Completeness: sentence/paragraph count parity vs source.
  - Named entity and number diff: exact match check for names, dates, units, ranges, and modality (“should/must”).
  - Domain lint rules: aviation ft magnitudes; “business hours” vs “days”; safety term pairs (가연성/인화성).
  - Flag high‑risk homonyms (해, 플라이) and require human review.
  - Optionally block km/°C insertions unless explicitly requested.

## Confidence and Coverage
Confidence: medium-high. Evidence spans many judges and domains, with repeated patterns on truncation, units/timelines, domain terms, and homonym-driven misreads. Limits: back-translation artifacts may amplify person/register issues; not all ko-specific errors are observable per sample, and some drift may stem from source variability rather than Korean morphology alone.