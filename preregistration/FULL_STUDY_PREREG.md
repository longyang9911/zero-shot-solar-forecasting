# Full study — LLM-read forecaster discussions vs cheap text features across latency and horizon
(registered and sealed BEFORE any confirmation-period HRRR/AFD download and before any confirmation forecast)

Background: scale pilot (RESULT_pilot.md, dev 2024-04..06, 12 BAs) gave GO. At lag 14, H1: LLM2day +3.96% vs no text,
+3.14 points over the best cheap control (11/12 BAs). H2: +0.77%, +1.09 points over the best control (8/12).
Earlier periods (C1-C3, old prompt, 7 BAs, H1 only) showed a smaller LLM-vs-keyword gap, and in C3 keyword was better.
This study tests the pilot pattern on unseen data and unseen BAs.

## Data and protocol (identical to the pilot except where stated)
- Retrospective confirmation window: TEST 2026-04-01..2026-07-29 (120 origins); days 2026-02-01..07-31; predictions from 03-01
  (cal30 window). Qwen2.5 was released 2024-09-19, before the window. TimesFM-3 was released 2026-08, after it (TSFM pretraining
  overlap cannot be excluded; it affects all arms equally). 2026-02..03 data were C1 test/history and are used here only as
  history and calibration. 2026-06..07 values were C3 history context and were never forecast targets. No reserved window is used.
- Prospective confirmation window: origins 2026-10-17..2026-11-30 (data that did not exist at sealing time). It is evaluated once,
  after EIA has published 2026-12-02, with the identical sealed code (a new period entry with dates only).
- BAs: 12 development BAs (config.BAS) + 5 holdout BAs never used before: TEC, DUK, IPCO, TEPC, PACW (config.HOLDOUT_*).
- Inputs: EIA-930 solar (sum of with/without storage); HRRR 12z f01-f42 from the latest run issued <= origin (leakage
  unit test); AFDs of 28 offices, as-of afd_asof_v2; Qwen2.5-7B-Instruct, prompt v2 (extract_v2.py), greedy.
- Arms (study.py): six numeric {tfm3, chronos2, ens} x {raw, cal30}; ens + {LLM2day, LLMtoday, KEYWORD, TFIDF, CONST, SHUF0-2}.
  Text-control statistics are fitted on development data only (2024-02-01..06-30; TF-IDF vocabulary/SVD, standardisation to
  dev per-BA LLM mean/sd, CONST = dev mean). SHUF permutes the period's own LLM today scores across days.
- Statistics: 7-day block bootstrap over test days (2000 reps, seed 7, starts 0..n-7); BA equal-weight mean relative pinball;
  paired bootstrap for differences between arms.

## Primary tests (12 development BAs, lag 14 h, retrospective window)
- P1 (H1 = hour-ends 19..06 UTC): ens_LLM2day vs ens gain >= 1% with CI lower > 0, AND ens_LLM2day minus the best cheap control
  (max of KEYWORD, TFIDF, CONST, mean SHUF, selected on this window) >= 0.5 points with paired CI lower > 0.
- P2 (H2 = next solar day, steps 13-36): ens_LLM2day minus best cheap control >= 0.5 points with paired CI lower > 0.

## Secondary (reported, no separate claim)
Same tests on the 5 holdout BAs and all 17; lag 2; gains vs the strongest numeric configuration; LLMtoday vs LLM2day; cov90; per BA.
The prospective window repeats P1/P2 on all 17 BAs.

Stated expectation: P1 likely passes (the pilot margin is large); P2 is uncertain (pilot margin +1.09 points, wide CI, humid SE BAs negative).
Results are reported whatever they are; no re-tuning on confirmation data.
