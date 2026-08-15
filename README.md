# Research Taste Eval

Data and analysis code for **"Eliciting Research Taste in LLMs through Future Research Direction Choice"**, COLM 2026 (LM4SCI workshop).

The evaluation presents a model with a paper's research goal, the hypotheses it tested and their key results, and two candidate future research directions - one recommended by the paper's authors, one plausible distractor drawn from a similar paper. Each pair is shown in both orderings.

## Files

**`primary_queries_and_responses/`**

- `main_pairwise_queries.json` — the evaluation set. 80 pairwise comparisons from 19 ML papers, each presented in both orderings (160 prompts). Every record holds the research goal, hypotheses and key results, the author-recommended direction, the distractor, `gt_position`, and the exact rendered prompt.
- `main_responses/*.json` — one file per model (8 models), each covering all 160 prompts. Contains a `metadata` block (model ID as called, temperature, timestamp, cost) and per-prompt responses with reasoning traces.
- `pair_similarities.json` — embedding and TF-IDF cosine similarities between the author-recommended direction, the distractor, and the query context, for each of the 80 pairs. Embeddings from `openai/text-embedding-3-small`.

**`prompt_variations_queries_and_responses/`**

- `stable_divergence_queries.json` — the 45 pairs where at least one model stably chose the distractor, with the diverging models listed per pair (169 model × pair events in total).
- `no-role.json`, `no-goal.json`, `reframed-task.json` — responses to the three prompt ablations, each re-running all 169 events in both orderings (338 records per file). Each record carries the modified prompt it was run with.
- `author_pref_prediction.json` — responses under the author-preference framing ("which would the authors choose?"). Excludes Gemini 3 Pro, so covers 157 events (314 records).

**`taxonomy/`**

- `taxonomy_labels.json` — category labels for the author-recommended direction and the distractor in each of the 45 stable-divergence pairs, with the diverging models per pair.

**`evaluation_scripts/`**

- `main_results_and_agreement.ipynb` — per-model author alignment, stable divergence and unstable choice rates; divergence prevalence across models; and three-state agreement for all 28 model pairs.
- `wilson_confidence_intervals.ipynb` — Wilson 95% confidence intervals for the main outcome rates and the prompt-retention rates.
