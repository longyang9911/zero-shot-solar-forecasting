# A Zero-Shot Foundation Model Ensemble for Regional Probabilistic Solar Power Forecasting

This repository holds the preregistrations, prospective predictions and timestamp proofs of the paper

Y. Long, S. N. Khan, B. Liu, Q. Yang, H. Wu, and Y. Liu, "A Zero-Shot Foundation Model Ensemble for Regional Probabilistic Solar Power Forecasting," submitted to the 2027 IEEE PES General Meeting.

The code will be added upon publication.

## Overview

The study forecasts the hourly solar power generation of 23 U.S. balancing authorities (BAs), reported in EIA-930, for the next 36 h as nine quantiles. The forecasts come from an ensemble of two time series foundation models, TimesFM-3 and Chronos-2, with HRRR covariates; neither model is trained or fine-tuned on BA data. The study also tests whether sky cover and precipitation scores that Qwen2.5-7B-Instruct reads from NWS Area Forecast Discussions add value beyond NWP, and compares them with keyword, TF-IDF, constant and shuffled controls.

## Results

At a data lag of 14 h, the ensemble has 19–26% lower pinball loss than locally trained gradient boosting models. The LLM features reduce pinball loss by 1.52% without the NWP cloud inputs and by 0.56% with them. The latter increment was not confirmed on a second holdout set or in the prospective test (+0.71%, 95% CI [−0.98, +1.70]).

## Contents

| Path | Content | Status |
|---|---|---|
| `preregistration/` | Preregistration documents, amendments, SHA-256 seal files and their OpenTimestamps proofs | available |
| `prospective/<origin>/` | Quantile predictions (`predictions.npz`), run manifest with input and code hashes (`manifest.json`) and their OpenTimestamps proofs (`*.ots`) | available |
| `TIMESTAMPS.md` | Bitcoin block and block time of every proof | available |
| `code/` | Forecasting pipeline and evaluation | upon publication |

Preregistration files by evaluation stage:

| Stage | Files |
|---|---|
| 1. Pilot (2024, 12 development BAs) | `PREREG_pilot.md` |
| 2. Confirmation (Apr. 1 to Jul. 29, 2026) | `FULL_STUDY_PREREG.md`, `FULL_STUDY_SEAL.sha256`; holdout set 2: `PREREG_HOLDOUT2_NWPCLOUD.md`, `HOLDOUT2_SEAL_step1.sha256`, `HOLDOUT2_SEAL_step2.sha256` |
| 3. Prospective test (Oct. 17 to Nov. 4, 2026) | `FULL_STUDY_AMENDMENT_1.md` to `_3.md` with their `.sha256` files, `FULL_STUDY_AMENDMENT_3_ERRATUM.md`, `E1_SCORER_SEAL.sha256`, `E1LOC_SEAL.sha256` |
| Post hoc analysis, registered before it was run | `PREREG_LLM_ABLATION.md` |

The seal files list SHA-256 hashes of the code files. Once the code is released, these hashes can be used to check that it is the sealed version.

## Prediction files

Each `predictions.npz` holds one array per key `variant|BA|origin`, of shape 36 × 9: lead hours 1 to 36 (hours ending 19 UTC on the issue day to 06 UTC two days later) by the quantile levels 0.025, 0.05, 0.1, 0.25, 0.5, 0.75, 0.9, 0.95 and 0.975, in MW. The variant names map to the paper as follows.

| File | Paper |
|---|---|
| `tfm3` | TimesFM-3 alone (base covariates) |
| `chronos2` | Chronos-2 alone (base covariates) |
| `ens` | ENS (base covariates: HRRR DSWRF and cosine of the solar zenith angle) |
| `ens_NWPCLOUD` | ENS+NWP |
| `ens_LLM2day` | ENS+LLM |
| `ens_LLM2day_NWPCLOUD` | ENS+NWP+LLM |
| `c2_NWPCLOUD` | Chronos-2+NWP |
| `c2_LLM2day_NWPCLOUD` | Chronos-2+NWP+LLM |

## Timestamps

The Stage 1 and Stage 2 documents and code hashes were written on Oct. 16 to 18, 2026; each document states what had and had not been done when it was written. Their OpenTimestamps proofs were created later, on Oct. 20, 2026, and are anchored in Bitcoin block 972397 (Oct. 21 00:35 UTC).

Amendments 1 to 3 of Stage 3 are anchored in the same block, before the first real-time origin (Oct. 21, 18 UTC). The scorer seals are anchored in block 974838, after they were sealed on Nov. 6, 2026.

Origins Oct. 17 to 20 come from a prespecified as-of replay run on Oct. 20 and are also anchored in block 972397. The other 15 origins (Oct. 21 to Nov. 4) were produced in real time after 18 UTC on the issue day. `TIMESTAMPS.md` lists the block of each proof.

## Verification

```bash
pip install opentimestamps-client
ots verify prospective/2026-10-21/predictions.npz.ots
```

`ots verify` needs access to Bitcoin block headers, from a local node or a block explorer. A valid proof shows that the file existed no later than the time of the block it is anchored in.

## Citation

Please cite the paper. `CITATION.cff` will be updated with the DOI after publication.

```bibtex
@inproceedings{long2027zeroshot,
  author    = {Long, Yang and Khan, Shahid Nawaz and Liu, Bo and Yang, Qihui and Wu, Hongyu and Liu, Yichen},
  title     = {A Zero-Shot Foundation Model Ensemble for Regional Probabilistic Solar Power Forecasting},
  booktitle = {2027 IEEE Power \& Energy Society General Meeting (PESGM)},
  year      = {2027},
  note      = {Submitted}
}
```

## License

Preregistrations, predictions and proofs: CC BY 4.0.
