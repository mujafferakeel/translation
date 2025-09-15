# Failure Model — Claude Opus 4.1 (no reasoning) — Swahili

## Summary
Overall, the model preserves broad structure but shows systematic fidelity failures around time expressions, specialized terminology, and numeric precision. The most salient weaknesses are: (1) a recurring six‑hour shift and AM/PM flips tied to Swahili time conventions; (2) domain‑term substitution (e.g., pollinators→butterflies; armor→weapon; mulch→fertilizer; margin→revenue); (3) numeric distortions in prices, durations, and years; (4) gender/role drift on back‑translation due to Swahili’s gender‑neutral pronouns; and (5) occasional omissions and tone flattening.

## Top Failure Modes
- Systematic time misinterpretation (6‑hour shift, AM/PM flips) — Descriptions of schedules, timelines, tides, forecasts, and narratives consistently show a ~6‑hour offset and AM/PM inversions. Why: interference from East African Swahili “saa” system (day starting ~6 a.m.) and inconsistent handling of AM/PM vs dayparts (asubuhi/mchana/jioni/usiku). Frequency: common. Severity: high. Evidence: judges note “all reported times wrong by six hours” and “10:00 PM LEGO Club” where evening/morning contexts are broken.

- Domain term over‑generalization/substitution — Technical or specialized lemmas are mapped to more common but wrong neighbors: “pollinators”→“butterflies,” “mulch”→“fertilizer,” “drizzle”→“fog,” “armor”→“weapon,” “margin”→“revenue,” “probe”→“barrier,” “reasonable doubt”→“any doubt.” Why: lexical gaps in general Swahili corpora, weak domain glossaries, and preference for high‑frequency synonyms. Frequency: common. Severity: high. Evidence: “pollinators replaced by butterflies, bees dominating ‘butterfly communities’” and “‘mulch’ price 7.99 but item translated as fertilizer.”

- Numeric distortion (years, prices, durations) — Year and numeric values shift (1750→170), prices flip (7.99→0.99), durations inflate (30 seconds→30 minutes), times in briefings/outages change. Why: token/decimal normalization issues, locale formatting interference, and paraphrase noise when surrounded by text. Frequency: common. Severity: high. Evidence: “1750 vs 170; 6–10 pm→midnight–4 am” and “7.99→0.99; ‘30 seconds’→‘30 minutes’.”

- Role/gender and entity drift — Swahili’s gender‑neutral third person and flexible role marking cause back‑translations to swap genders or roles (courier→captain; he↔she), or misname institutions (state→federal court). Why: lack of explicit gender markers in source Swahili and over‑normalization during back‑translation; entity priors. Frequency: occasional. Severity: medium. Evidence: “protagonist gender swapped; courier→rescue captain” and “state supreme court→federal supreme court.”

- Instructional and legal precision loss — Key standards or instructions invert: “reasonable doubt” softened/strengthened; “face‑down”→“face‑up”; liability “indirect damages”→“direct”; nautical/medical/safety terms simplified or misrendered. Why: register/style smoothing and lack of calibrated legal/technical lexicons in sw↔en. Frequency: occasional. Severity: high. Evidence: “reasonable doubt misstated multiple times” and “orientation reversed in step 7.”

- Omission and tone flattening — Sections omitted (entire horoscope sign), closings/voice altered (“Warmly”→“With love”), poetry/evocative prose simplified. Why: aggressive paraphrase and summarization tendencies; decoding temperature not constrained for literalness. Frequency: occasional. Severity: medium. Evidence: “omission of Libra section” and “evocative language weakened; metaphors altered.”

## Language‑Specific Factors
- Time system interference: Swahili commonly uses dayparts (asubuhi/mchana/jioni/usiku) and a local “saa” clock starting at ~6 a.m. Mapping to 12‑hour English with AM/PM often triggers 6‑hour offsets and contextual mismatches. This is the single most damaging recurrent error.
- Gender neutrality: Swahili 3rd‑person pronouns lack gender, leading to he/she flips and role misattribution on back‑translation unless constrained by context.
- Lexical gaps and polysemy: 
  - pollinators: correct “wachavushaji” is rarer than “vipepeo” (butterflies); 
  - mulch: “matandazo” vs “mbolea” (fertilizer);
  - drizzle vs fog: “manyunyu” vs “ukungu”;
  - armor vs weapon: “ngao/zana za kujikinga” vs “silaha.”
- Idioms/register: Legal and technical registers in Swahili vary regionally; without domain anchoring, the model defaults to general vocabulary that dilutes precision.

## Numbers, Units, and Formatting
- Numerals and times: frequent AM/PM flips, 6‑hour shifts, wrong briefing/outage updates, wrong end times; sometimes 24h↔12h conversions implied.
- Prices/decimals: 7.99→0.99; “seven ninety‑nine” read as ninety‑nine; potential decimal/word‑form mismatch.
- Durations: “30 seconds”→“30 minutes”; “2:30 a.m.” vs “8:30 a.m.”
- Dates/years: “1750”→“170”; day/date mismatches in forecasts.
- Tables/steps: sporadic omissions or reversed orientation; headers dropped; minor markup simplification.

No evidence of unit system conversions (metric/imperial) as a systematic issue, but individual term swaps (e.g., tools/soil types) occur.

## Meta/Disclaimer Behavior
- Limited explicit safety/policy intrusions, but occasional authorial tone shifts (“newsletter,” “With love”) and policy term drift (“bond measure”→ambiguous phrasing). These appear when genre/voice is informal or civic.

## Recommendations
- Prompt patterns:
  - “Translate literally for fidelity. Do NOT localize time. Preserve all numeric values exactly. Keep AM/PM as in source. If unsure, copy the number and unit verbatim.”
  - “Use these Swahili glossaries: pollinators=‘wachavushaji’; mulch=‘matandazo’; drizzle=‘manyunyu’; armor=‘ngao’; margin (finance)=‘pato la uendeshaji/marjin’ (not ‘mapato’); reasonable doubt=‘shaka ya msingi.’”
  - “Maintain roles, titles, genders as in source; if unspecified, do not infer.”
  - “Do not omit sections, headers, or steps.”
- Decoding:
  - Lower temperature/top‑p for determinism in translation tasks.
  - Enable constrained decoding with glossary/term locking for high‑risk domains (legal, medical, finance, technical ops).
- Data/finetuning:
  - Augment sw↔en corpora with parallel examples emphasizing:
    - Time expressions across 12h/24h/‘saa’ systems with explicit AM/PM mappings.
    - Legal standards (reasonable doubt, liability clauses), finance (margin vs revenue), weather (drizzle vs fog), ecology (pollination), and technical manuals.
  - Build domain lexicons and enforce during training (adaptive soft constraints).
- Post‑processing checks:
  - Regex diff on all numerals, times, and units; flag deviations for review (e.g., year, price, hh:mm, AM/PM).
  - Heuristic for 6‑hour offset detection: if all times shift by +/‑6h, trigger corrective review.
  - Term consistency checker: source/target keyword lists (pollinator, mulch, armor, margin, probe) to verify aligned equivalents.
  - Gender/role audit when back‑translating from Swahili: if source lacked gender, avoid imposing one; preserve titles.
- Workflow:
  - For schedules/alerts/legal notices, require bilingual QA or automated numeric/timestamp validation before release.
  - Maintain project‑specific glossaries and pass them to the model each call.

## Confidence and Coverage
Confidence: medium‑high. The error families (time shifts, domain term drift, numeric distortions) recur across many judges and samples with consistent patterns. Coverage limits: we infer language‑specific time issues from consistent 6‑hour offsets; not all domains equally represented, and some issues (tables/markup) appear sparsely. Judges and topics are diverse enough to generalize core failure modes, but fine‑grained subdomain nuances may need additional evidence.