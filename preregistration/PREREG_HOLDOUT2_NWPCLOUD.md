# Holdout set 2 + numeric cloud control (registered before any holdout-2 or HRRR-cloud download)
Sealing: step 1 (this file + config.py) before any download; step 2 (code) before any holdout-2 EIA label is read and before any forecast.

Why: (1) Generalisation of the confirmed P1 result (FULL_STUDY_CONFIRM: H1 lag 14 +1.62%, +1.20 points over the best cheap text
control) to six further BAs never used before: SCEG, TVA, FPC, EPE, IID, BANC (config.HOLDOUT2_*; new offices CAE, CHS, MEG, HUN,
OHX, JAX, EPZ). (2) The most important reviewer question: does the LLM merely restate cloud and precipitation information that NWP
already provides? We add a same-information NUMERIC control from the same leakage-guarded HRRR 12z runs.

Window and protocol: identical to FULL_STUDY_PREREG.md (TEST 2026-04-01..07-29, lag 14 primary, 36-h horizon, ensemble
TimesFM-3 + Chronos-2, LLM v2 prompt, controls fitted on 2024-02..06 development data, 7-day block bootstrap, paired differences).
New arms:
- ens_NWPCLOUD: two hourly channels at the target hour, TCDC(entire atmosphere)/100 and log1p(3600*PRATE[mm/h])/log1p(10),
  taken from the latest 12z run issued <= origin (as DSWRF). Fixed transform, no fitting.
- ens_LLM2day_NWPCLOUD: LLM2day and NWPCLOUD channels together (complementarity).
Primary (6 holdout-2 BAs, lag 14, H1):
- Q1: ens_LLM2day vs ens >= 1% with CI lower > 0.
- Q2: ens_LLM2day minus the best control among {KEYWORD, TFIDF, CONST, mean SHUF, NWPCLOUD} (selected on this window)
  >= 0.5 points with paired CI lower > 0.
Secondary: Q1/Q2 on all 23 BAs (the 17 earlier BAs' window results are already known, so these are labelled post-hoc for the new
arms); ens_LLM2day_NWPCLOUD vs ens_NWPCLOUD (does the LLM add beyond NWP cloud?); H2; lag 2; cov90.
Stated expectation: Q1 likely (consistent across 17 BAs); Q2 uncertain, because NWPCLOUD is a stronger control than lexical features.
