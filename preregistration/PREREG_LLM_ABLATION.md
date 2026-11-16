# LLM ablation (POST-HOC on the already-evaluated confirmation window; registered before running it)
Question: is the confirmed H1 gain specific to Qwen2.5-7B-Instruct, and does it need a capable language model?
Models: Qwen3-8B (released 2025-04, i.e. before the 2026-04..07 window; non-thinking mode) and Qwen2.5-1.5B-Instruct (small).
Same prompt v2, greedy decoding, same as-of AFDs (17 BAs, asof_afd_confirm.json), extract_v2_abl.py.
Evaluation: replace the LLM channel in ens_LLM2day / ens_LLMtoday with each model's scores (study.py logic); report H1/H2 gains vs
ens and vs TFIDF at lag 14 and 2. Labelled post-hoc (window already used); no claim beyond robustness/scale dependence.
Expectation: Qwen3-8B similar to Qwen2.5-7B; 1.5B weaker (more parse failures, smaller gain).
