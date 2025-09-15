# Failure Model — Kimi K2-0905 — Swahili

## Summary
Overall, Kimi K2-0905 shows unstable fidelity in Swahili round‑trip translation, with systematic semantic substitutions, time/date inversions, truncations, and occasional degeneration. The most salient weaknesses are: (1) critical time/ schedule errors (AM/PM flips, day shifts); (2) consistent term-swaps in technical/scientific domains (“pollinators”→“seed dispersers,” “museum”→“souvenir”); (3) omissions of sections in longer/formal documents; (4) invented/garbled content under stress (rare but severe); and (5) loss of idiomatic/poetic nuance accompanied by role/pronoun drift in dialogue.

## Top Failure Modes
- Systematic time/day inversions — Schedules and event times frequently flip AM/PM, shift days, or alter durations; likely from ambiguity in Swahili time expressions and weak copy-through of numerals. Why: Swahili often uses contextual time words (asubuhi/jioni) and the model appears to normalize or “fix” times rather than preserve them. Frequency: common; Severity: high. Evidence: “9 PM–5 AM became 3 AM–1 AM”; “Friday→Saturday, 4:45 PM→4:45 a.m.”
- Technical term substitution drift — Key domain terms are replaced with near-neighbors, flipping meaning across an entire text. Why: limited domain lexicon alignment in Swahili and overuse of semantic neighbors during decoding. Frequency: common; Severity: high. Evidence: “pollinators” rendered as “seed dispersers” across a study; “museum” consistently mistranslated as “souvenir,” corrupting instructions.
- Major content omissions/truncations — Long/structured texts drop whole sections (Assessment/Plan, safety notes) or detailed observations. Why: max-length/decoding compression and tendency to summarize under low certainty. Frequency: common; Severity: high. Evidence: “entire Assessment and Plan omitted”; “curfew notice lost a critical warning.”
- Hallucinations and repetition glitches — Inserted nonsense strings, invented terms/names, or garbled last lines. Why: decoding instability and low confidence under rare vocabulary or dense description. Frequency: occasional; Severity: high. Evidence: “repeated nonsense string; creature renamed ‘Palahala’”; “last poetic line mangled into non-sense.”
- Role, entity, and attribute flips — Character gender, job titles, object types and properties change; puzzle mechanics and tool names misrendered. Why: Swahili’s neutral pronouns and noun class ambiguity, plus weak term anchoring for proper nouns and specialized tools. Frequency: occasional; Severity: high. Evidence: “aunt→cousin; ‘Eat’→‘Sleep’ line inverted”; “bolts became dowels; tape became board; pressing became breaking.”
- Units, symbols, and delimiter corruption — Feet↔meters, punctuation in specs (semicolon→dot), protocol modals weakened/strengthened (SHOULD/MAY→MUST). Why: normalization bias and limited sensitivity to formal syntax in Swahili→English backpass. Frequency: occasional; Severity: medium-high. Evidence: “feet vs. meters wrong”; “header uses dot instead of semicolon; MUST used where SHOULD/MAY.”
- Poetic/idiomatic flattening — Imagery, metaphors, and tone degrade or invert, with lexical oddities. Why: preference for literal paraphrase and synonym drift in low-frequency imagery. Frequency: occasional; Severity: medium. Evidence: “mirages→miracles; envy→trust”; “flame symbol became comb; sirens→banners.”

## Language‑Specific Factors
- Time expressions and AM/PM: Swahili often uses contextual markers (asubuhi, mchana, jioni, usiku) and can avoid explicit AM/PM; the model frequently “interprets” rather than preserves numeric times, leading to 3 p.m.↔3 a.m. flips and day shifts.
- Gender-neutral pronouns: “yeye” does not encode gender; back-translation to English often mis-assigns he/she, causing character gender swaps.
- Noun-class agreement and number: Class mismatches trigger singular/plural drifts (“driver”→“drivers”) and role/title changes.
- Lexical polysemy and derivation: Roots like -kumbusho cause “makumbusho” (museum) to be confused with “kumbukumbu/zikumbusho” (souvenir/memento).
- Technical/scientific lexicon gaps: Low-frequency domain terms (e.g., uchavushaji for pollination, temper in ceramics) push the model to nearby but wrong Swahili terms, then back to incorrect English.

## Numbers, Units, and Formatting
- Frequent AM/PM inversions and wrong start/end times; day-of-week changes; event windows compressed/expanded.
- Unit confusion and normalization: feet↔meters; counts and quantities altered; schedules shifted.
- Formal syntax drift: semicolon→dot in protocol headers; MUST/SHOULD modality changed; list headers/titles mislabeled; section names (“Fixed”→“Found,” “Notes”→“Texts”).
- Puzzles/specs: symbols and buttons changed (flame→comb); instructions “press”→“break.”

Whether conversions/inventions occur: yes—spontaneous conversions of units, punctuation, and modality; invention of dish/ingredient names and device parts in technical and culinary contexts.

## Meta/Disclaimer Behavior
- Occasional template/meta contamination rather than safety policy injections: section labels normalized or reworded (“Fixed” misread, “notes”→“texts”); some protocol language strengthened (MUST) despite source.
- Safety disclaimers are not a dominant issue; the harm comes from authoritative-sounding rewrites that change requirements.

## Recommendations
- Prompting
  - Use a constrained translation prompt: “Translate verbatim into Swahili/English. Do not add, omit, or interpret. Preserve numbers, dates, times, units, symbols, section headers, roles, and names exactly. Use 24-hour time; if AM/PM appears, copy it unchanged.”
  - Add a post-pass checklist in the prompt: “Before finalizing, verify: times/dates unchanged; units/delimiters preserved; names/titles identical; all sections present.”
  - For technical texts, include a termbase snippet: uchavushaji=pollination, wachavushaji=pollinators; makumbusho=museum; kiungo=fastener (avoid generic ‘bao/ubao’ for tape/board), n.k.
- Decoding
  - Lower temperature/top‑p; enable repetition penalty guardrails; increase max tokens to avoid truncation; prefer beam search with coverage penalty to reduce omissions; activate noising constraints that forbid edits in protected spans (numbers, units, named entities).
  - Use placeholders for protected items: wrap times, dates, numbers, symbols, and proper nouns in tags during translation and restore after.
- Data and fine‑tuning
  - Curate parallel Swahili corpora in domains that failed: scientific ecology, archaeology/ceramics, medical SOAP notes, protocols/specs, culinary menus/techniques.
  - Add contrastive pairs for confusable terms: makumbusho vs zawadi ya ukumbusho; wachavushaji vs wasambazaji wa mbegu; semicolon vs dot; MUST/SHOULD distinctions.
  - Include style-focused parallel data for poetry and figurative prose to reduce metaphor collapse and tone drift.
- Post‑processing checks
  - Automated validators: regex match for times/dates; diff detector for named entities and numbers; modality checker for RFC keywords; unit consistency checker (ft/m, °C/°F).
  - Section completeness: assert presence/order of required headers (Assessment/Plan; Fixed/Notes).
  - Terminology QA: glossary enforcement with flagging of near-synonyms for critical terms.
  - Back-translation spot checks on high-risk content (schedules, safety notices, specs).
- Workflow
  - For schedules/safety content, require dual rendering: keep original times verbatim plus a 24‑hour mirror line; prohibit interpretation.
  - Human-in-the-loop for technical/policy translations with a termbase and a “no-change” review of protected spans.

## Confidence and Coverage
Confidence: medium-high. The evidence set is dense and consistent across judges, genres, and tasks, with repeated patterns (time inversions, term swaps, omissions) observed in multiple samples. Coverage limits: source texts vary widely; some behaviors (repetition glitch, extreme hallucinations) are less frequent but severe; true Swahili source specifics are inferred from round‑trip evidence rather than direct monolingual error annotation.