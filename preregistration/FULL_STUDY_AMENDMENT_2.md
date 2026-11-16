# Amendment 2 to FULL_STUDY_PREREG.md (written 2026-10-20, before ANY EIA value dated 2026-10-17 or later was read; only inputs
# (HRRR, AFD texts, LLM scores) for 2026-08-17..10-19 have been prepared, see prosp_partial.sh)
1. New PRIMARY endpoint for the prospective window (origins 2026-10-17..11-04, all 23 BAs, H1, lag 14):
   E1 = BA-mean of 1 - L(ens+NWPCLOUD+LLM2day) / L(ens+NWPCLOUD), 7-day block bootstrap (as in study2.py).
   Reported outcome: point estimate and 95% CI. "Directional confirmation" if CI lower bound > 0; "substantive" if additionally >= 1%.
   Secondary: E1 on H2; E1 per region set (dev 12, holdout 1, holdout 2); E1 with a Chronos-2-only backbone (not TimesFM-3);
   P1/P2/Q1/Q2 as in Amendment 1; lag 2 (replay only).
2. REAL-TIME generation (strengthens Amendment 1): for origins 2026-10-21..11-04 the lag-14 forecasts are produced each day shortly after
   18 UTC by scale_pilot/realtime_forecast.py, from inputs available at that moment (EIA six-month file snapshot downloaded at run time,
   HRRR 12z of the day, AFDs issued <= origin-30 min). Each day's prediction file is hashed (SHA-256) and the hash is e-mailed to the user
   as an external timestamp. Origins 2026-10-17..10-20 (already past when this amendment was written) are produced later as an as-of
   replay with the same code and are reported separately as well as pooled. Arms that need future texts (SHUF) or other periods are
   computed only at evaluation time.
3. All cal30 arms use the as-of-correct window (framework/calib_asof.py; audit/RESULT_cal30_fix.md).
