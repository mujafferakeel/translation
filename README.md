# LLM Round‑Trip Translation Benchmark

When a model translates out of English and then back to English, how much meaning and voice does it keep? Each model does both steps (English → target language → English). A judge compares the back‑translation to the original and scores closeness on a 0–10 scale.

## Key Results

![Overall Leaderboard (Ensemble, zoom)](images/translation_leaderboard_ensemble_zoom.png)

Ensemble aggregates across judges per item, then averages across all items (all languages); the zoom view overlays µ±3·SEM bands.

The chart ranks models by average round‑trip score (higher is better).

Overall winner share across all languages:

![Overall Winner Pie](images/translation_winner_pie.png)

## Methods in Brief

10 languages × 200 sources per language (2000 items). Each model translates every item (EN→LANG→EN). Five judges score each item; ensemble averages per item across judges, then across items. Eight models → 16,000 model×item pairs and 80,000 total judgments.

### Top Models (snapshot)

| Rank | Model | Mean Score |
|-----:|-------|-----------:|
| 1 | GPT-5 (medium reasoning) | 8.690 |
| 2 | Grok 4 | 8.573 |
| 3 | Claude Opus 4.1 (no reasoning) | 8.559 |
| 4 | Gemini 2.5 Pro | 8.529 |
| 5 | Qwen 3 Max Preview | 8.324 |
| 6 | DeepSeek V3.1 Reasoner | 8.298 |
| 7 | Mistral Medium 3.1 | 8.285 |
| 8 | Kimi K2-0905 | 8.285 |

## By Language

![Normalized Heatmap (z-scored within each language)](images/translation_leaderboard_heatmap_normalized.png)

- Z-scored per language so each column shows relative strength vs. peers (0 = average for that language).
- Prefer this normalized view; it controls for language mix and difficulty.

Per‑language leaderboards (zoom) and distributions (strip plots):

Full tables for each language are generated under `reports/`. See the index: `reports/leaderboard_by_language.md`.

### Per‑language charts — zoomed leaderboards + strip plots

#### Arabic

![Arabic Leaderboard (zoom)](images/translation_leaderboard_lang_ar_zoom.png)

![Arabic Strip](images/translation_strip_lang_ar.png)

![Arabic Winner Pie](images/translation_winner_pie_lang_Arabic.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.688 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.616 | 5 |
| 3 | Gemini 2.5 Pro | 8.566 | 5 |

#### Chinese

![Chinese Leaderboard (zoom)](images/translation_leaderboard_lang_zh_zoom.png)

![Chinese Strip](images/translation_strip_lang_zh.png)

![Chinese Winner Pie](images/translation_winner_pie_lang_Chinese.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.672 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.651 | 5 |
| 3 | Grok 4 | 8.634 | 5 |

#### Spanish

![Spanish Leaderboard (zoom)](images/translation_leaderboard_lang_es_zoom.png)

![Spanish Strip](images/translation_strip_lang_es.png)

![Spanish Winner Pie](images/translation_winner_pie_lang_Spanish.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.794 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.743 | 5 |
| 3 | Grok 4 | 8.680 | 5 |

#### Hindi

![Hindi Leaderboard (zoom)](images/translation_leaderboard_lang_hi_zoom.png)

![Hindi Strip](images/translation_strip_lang_hi.png)

![Hindi Winner Pie](images/translation_winner_pie_lang_Hindi.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.731 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.676 | 5 |
| 3 | Gemini 2.5 Pro | 8.579 | 5 |

#### Russian

![Russian Leaderboard (zoom)](images/translation_leaderboard_lang_ru_zoom.png)

![Russian Strip](images/translation_strip_lang_ru.png)

![Russian Winner Pie](images/translation_winner_pie_lang_Russian.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.706 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.647 | 5 |
| 3 | Grok 4 | 8.635 | 5 |

#### Japanese

![Japanese Leaderboard (zoom)](images/translation_leaderboard_lang_ja_zoom.png)

![Japanese Strip](images/translation_strip_lang_ja.png)

![Japanese Winner Pie](images/translation_winner_pie_lang_Japanese.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | Grok 4 | 8.703 | 5 |
| 2 | GPT-5 (medium reasoning) | 8.678 | 5 |
| 3 | Claude Opus 4.1 (no reasoning) | 8.670 | 5 |

#### Korean

![Korean Leaderboard (zoom)](images/translation_leaderboard_lang_ko_zoom.png)

![Korean Strip](images/translation_strip_lang_ko.png)

![Korean Winner Pie](images/translation_winner_pie_lang_Korean.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.655 | 5 |
| 2 | Grok 4 | 8.614 | 5 |
| 3 | Claude Opus 4.1 (no reasoning) | 8.612 | 5 |

#### Polish

![Polish Leaderboard (zoom)](images/translation_leaderboard_lang_pl_zoom.png)

![Polish Strip](images/translation_strip_lang_pl.png)

![Polish Winner Pie](images/translation_winner_pie_lang_Polish.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.720 | 5 |
| 2 | Claude Opus 4.1 (no reasoning) | 8.637 | 5 |
| 3 | Grok 4 | 8.636 | 5 |

#### Turkish

![Turkish Leaderboard (zoom)](images/translation_leaderboard_lang_tr_zoom.png)

![Turkish Strip](images/translation_strip_lang_tr.png)

![Turkish Winner Pie](images/translation_winner_pie_lang_Turkish.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.680 | 5 |
| 2 | Grok 4 | 8.605 | 5 |
| 3 | Claude Opus 4.1 (no reasoning) | 8.578 | 5 |

#### Swahili

![Swahili Leaderboard (zoom)](images/translation_leaderboard_lang_sw_zoom.png)

![Swahili Strip](images/translation_strip_lang_sw.png)

![Swahili Winner Pie](images/translation_winner_pie_lang_Swahili.png)

#### Top Models (snapshot)

| Rank | Model | Mean | # Judges |
|-----:|-------|-----:|---------:|
| 1 | GPT-5 (medium reasoning) | 8.573 | 5 |
| 2 | Gemini 2.5 Pro | 8.346 | 5 |
| 3 | Grok 4 | 8.220 | 5 |

## Normalized View

![Language‑Normalized Leaderboard](images/translation_leaderboard_language_normalized.png)

Controls for language difficulty/mix by z-scoring within each language, then averaging per model.

Interpretation: 0 = language mean; >0 above‑language average; <0 below.

## Reliability and Uncertainty

![Judge Agreement (Pearson)](images/judge_grader_correlation_pearson.png)

- Multi‑judge runs add error bars in the leaderboard and a judge‑agreement heatmap. Where judges disagree, treat small rank gaps as ties.
- We compute averages as “mean of per‑sample means,” optionally averaged across judges, so no single judge or language dominates.

Also useful for sanity‑checking judges vs. translators (normalized by judge):

![Judge × LLM — Normalized Means](images/grader_vs_llm_normalized_means.png)

## Failure Rationales

We summarize judge rationales into a lightweight taxonomy and synthesize per‑model failure models.

- Taxonomy index: `reports/error_taxonomy.md`
- Failure models (LLM‑summarized): `reports/failure_models/<translator>/<lang>.md`

Taxonomy snapshot (GPT‑5 Medium — Chinese):

| Tag | Count | Share |
|-----|------:|------:|
| tone_shift | 583 | 45.2% |
| omission | 352 | 27.3% |
| addition | 332 | 25.7% |
| numbers_units | 18 | 1.4% |
| disclaimer_meta | 5 | 0.4% |

Failure‑model excerpt (GPT‑5 — Chinese):

- Mixed‑language leakage in structured sections; headings/bullets left in Chinese in back‑translation.
- Tone/imagery flattening and metaphor drift (poetic language becomes literal; metaphor swaps).
- Domain term substitution (legal/technical terms normalized to near neighbors; scope changes).

Example rationale quotes (selected across models/languages):

- Model DeepSeek Reasoner — Arabic: “Multiple critical meaning reversals (e.g., "nods" becomes "shakes his head") make key character interactions nonsensical and contradict the original narrative.”
- Model DeepSeek Reasoner — Arabic: “The back‑translation fundamentally misidentifies the "wheeled lunar rover" as a "winged asteroid probe," a major factual error, though most other technical details are preserved.”
- Model DeepSeek Reasoner — Arabic: “Contains untranslated words and major meaning errors ("metronome" to "mazurka," "on my knees" to "on her lap"), corrupting key images and memories in the original narrative.”
- Model DeepSeek Reasoner — Arabic: “Loses specific football jargon, reverses a key instruction (“jump it” → “jump on him”), and renders the final motivational line nonsensical (“every quick enemy”).”
- Model Qwen 3 Max Preview — Chinese: “Several normative shifts (SHOULD→must) and minor terminology changes (“intermediaries”→“middleware”) alter strength of requirements and tone.”
- Model Qwen 3 Max Preview — Chinese: “Repeatedly narrows ‘hearing/minute order’ to ‘trial/trial minute,’ altering scope; minor phrasing shifts in examples and directives.”
- Model Qwen 3 Max Preview — Chinese: “Specific branding language in the taglines is paraphrased or altered, losing key terms like "Consciously" and the direct "sole/soul" pun.”
- Model Qwen 3 Max Preview — Chinese: “Recurring shifts from ‘late afternoon/dusk’ to ‘evening/twilight,’ and sign wording changes alter motifs, specificity, and tone.”
- Model Qwen 3 Max Preview — Arabic: “The meaning is preserved well, but there are many lexical substitutions and a few minor shifts, like changing the partner's pronoun from gender‑neutral "their" to masculine "his".”
- Model Qwen 3 Max Preview — Arabic: “The translation alters several specific details, such as 'Good afternoon' to 'Good evening,' 'bodega' to 'small shop,' and 'business partner' to 'coworker,' losing some nuance.”

## What’s Measured

- Round‑trip fidelity: meaning, tone/register, and stylistic alignment between original and back‑translation.
- Languages covered: Polish, Chinese, Spanish, Arabic, Hindi, Russian, Japanese, Korean, Turkish, Swahili.
- Diagnostics: distributions by language, winner share, and length ratios (back/original) to catch omissions or verbosity.

## How Scoring Works

- Judge rubric: compares original vs. back‑translation on a 0–10 scale.
- Anchors: 10.0 ≈ indistinguishable; 7.0 ≈ minor losses; 5.0 ≈ noticeable omissions/additions or tone/register shifts; 0.0 ≈ unrelated.
- Penalties: invented/missing content, tone/register drift, meta‑disclaimers. Trivial mechanics (e.g., punctuation) don’t matter if meaning is intact.
- Aggregation: per‑item means (optionally across multiple judges), then averaged across items and languages.


## Notes

- The benchmark uses the same model for forward and back translation to stress internal consistency.
- A short narrative of findings and additional figures are available in the project’s report.

## Bonus Views

Per‑language score distributions are included above (strip plots for all languages).

Language comparison aggregates:

<!-- Top‑3 removed: biased and redundant; prefer all‑models and normalized heatmaps. -->

All‑Models Mean by Language (zoom):

![All‑Models Mean by Language (zoom)](images/language_comparison_all_zoom.png)

Per‑language mean of per‑model means (equal weight per model). Error shading indicates uncertainty.

Strip Plot — Languages Colored:

![Strip Plot — Languages Colored](images/translation_strip_languages_colored.png)

Each dot = per‑story mean across judges; colors encode language; models on x‑axis with light jitter.


## Other multi-agent benchmarks
- [PACT - Benchmarking LLM negotiation skill in multi-round buyer-seller bargaining](https://github.com/lechmazur/pact)
- [BAZAAR - Evaluating LLMs in Economic Decision-Making within a Competitive Simulated Market](https://github.com/lechmazur/bazaar)
- [Public Goods Game (PGG) Benchmark: Contribute & Punish](https://github.com/lechmazur/pgg_bench/)
- [Elimination Game: Social Reasoning and Deception in Multi-Agent LLMs](https://github.com/lechmazur/elimination_game/)
- [Step Race: Collaboration vs. Misdirection Under Pressure](https://github.com/lechmazur/step_game/)

## Other benchmarks
- [LLM Creative Story-Writing Benchmark](https://github.com/lechmazur/writing/)
- [Extended NYT Connections](https://github.com/lechmazur/nyt-connections/)
- [LLM Thematic Generalization Benchmark](https://github.com/lechmazur/generalization/)
- [LLM Confabulation/Hallucination Benchmark](https://github.com/lechmazur/confabulations/)
- [LLM Deceptiveness and Gullibility](https://github.com/lechmazur/deception/)
- [LLM Divergent Thinking Creativity Benchmark](https://github.com/lechmazur/divergent/)
---


## Updates
- Sep 15, 2025: Initial version.

Follow [@lechmazur](https://x.com/LechMazur) on X for other upcoming benchmarks and more.
