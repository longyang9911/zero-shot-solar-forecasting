# Scale pilot (development, exploratory) — registered before any pilot forecast is scored

Direction (user-approved 2026-10-16): when and why do LLM-read forecaster discussions add value beyond NWP for TSFM
probabilistic solar forecasting, along two axes, data latency (lag 2 vs 14 h) and horizon (H1 = 1-12 h, H2 = 13-36 h, the next
solar day). The aim is to test robustly whether the LLM beats cheap text features.

Data: 12 BAs (original 7 + FPL, SOCO, CPLE, PACE, PSCO; config.py), EIA-930 2024; HRRR 12z f01-f42 at fixed points,
always from the latest run issued <= origin; AFDs of 22 offices, as-of (afd_asof_v2, which reproduces v1 0/130 mismatches).
Days 2024-02-01..06-30, predictions from 03-01, TEST 2024-04-01..06-28 (89 days). Period chosen to avoid all reserved windows;
it is a NEW development period (Qwen2.5 released 2024-09, so this period precedes it; dev only).
LLM v2 (extract_v2.py): one call per AFD -> today (11-18 local) and tomorrow (08-18 local) sky / precip (+ tomorrow uncertainty).
Arms (ensemble TimesFM-3 + Chronos-2 with HRRR): no text; LLMtoday (H2 reuses the issue-day score); LLM2day (H2 uses the
tomorrow score); KEYWORD; TFIDF (2-dim SVD); CONST; SHUF0-2 (day-permuted LLM today scores). Text-control statistics use
only pre-test days (<= 2024-03-31). Reference: ens without text. 12-BA equal-weight mean gain, 7-day block bootstrap.

Questions and decision rule for the full study (fixed now):
Q1 text value by horizon: gain of LLM2day vs ens in H2 vs H1, at lag 14 and lag 2.
Q2 tomorrow-specific extraction: LLM2day vs LLMtoday in H2.
Q3 LLM vs cheap features: LLM2day minus max(KEYWORD, TFIDF, CONST, mean SHUF) in H2 at lag 14.
GO for an LLM-centred full study if Q3 >= +0.5 points AND LLM2day beats that best control in >= 8/12 BAs (H2, lag 14).
Otherwise the full study is centred on latency and horizon effects of text in general, with the LLM as one representation.
The framework/HRRR leakage guard is unit-tested before scoring (test_no_future_runs).
