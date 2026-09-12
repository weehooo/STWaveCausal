# TKDE Submission Reproducibility Guide

This file maps every main paper table and audit claim to the machine-readable
artifact that generated it. It is part of the P3 submission-readiness package.

## Current package state (2026-09-09)

- `paper_tkde/main.tex` was clean-rebuilt with `latexmk -C` followed by
  `latexmk -pdf -interaction=nonstopmode -halt-on-error`.
- Result: **18 pages**, **0 LaTeX errors**, **0 undefined references**, and
  **0 missing glyphs**. Only cosmetic underfull hboxes are reported by TeX and
  do not affect PDF validity.
- `TKDE_submission_package/paper/main.pdf` is byte-for-byte identical to the
  rebuilt `paper_tkde/main.pdf`.
- The PEMS08 12-step table now carries a joint-capability note and the PSTCGCN
  and MGFGCN baseline rows; the package source and
  `TKDE_submission_package.zip` were rebuilt to match.

## Hardware and software

- Cloud host: Linux 5.15, NVIDIA RTX 3080 Ti 12 GB
- Python 3.10.12, PyTorch 2.3.1+cu121, NumPy, SciPy, scikit-learn
- Local PDF compile: TeX Live 2020 / IEEEtran
- Exact versions are recorded in each result JSON and in
  `audit/audit_manifest.json`

## Prediction tables

| Paper element | Artifact |
|---|---|
| `tab:main_results`, `tab:baselines_ms` | `eval_h1_flow_baselines_results.json` |
| `tab:pems08`, `tab:pems04` | `eval_h1_flow_baselines_results.json` |
| `tab:encoders`, `tab:test`, `tab:test_p` | `eval_h1_flow_baselines_results.json` |
| `tab:single_step` (standard 60/20/20 single-step) | `standard_1step_pems08.json`, `standard_1step_pems04.json` |
| Uncertainty and efficiency paragraph | `h1_uncertainty_efficiency.json` |
| Cluster-robust sensitivity | `h1_cluster_robust.json` |
| Training cost table | `paper_tkde/main.tex`, logs under `cloud_logs/` |

## Causal and counterfactual audits

| Paper element | Artifact |
|---|---|
| Synthetic recovery, protocol shift | `synthetic_recovery_new.json`, `synthetic_recovery_ood.json` |
| `thm:transfer` (transferred recovery bound) | `direction1_transfer_bound.json`, `direction1_transfer_bound.py` |
| `thm:transfer` misspecification bound (observable-equivalent family) | `direction2_misspec_bound.json`, `direction2_misspec_bound.py` |
| `cor:transfer_finite` (finite-sample transferred recovery bound) | `direction3_transfer_finite_sample.json`, `direction3_transfer_finite_sample.py` |
| `thm:partial_identification` (sharp partial-identification envelope) | `direction4_partial_identifiability.json`, `direction4_partial_identifiability.py` |
| `prop:minimax_lower` (observable-equivalence minimax lower bound) | `direction4_partial_identifiability.json`, `direction4_partial_identifiability.py` |
| `prop:cert_budget` (composed statistical certificate) | `direction5_certificate_composition.json`, `direction5_certificate_composition.py` |
| Matching balance and fitted PS | `psm_propensity_results.json`, `psm_fitted_closedloop_results.json` |
| Known-DAG recovery | `dag_recovery_results.json` |
| Placebo calibration | `placebo_calibration_results.json` |
| NOAA external audit | `real_event_registry.json`, `real_event_validation_results.json` |
| NYC collision stress test | `collision_stress_results.json`, `localized_stress_protocol.json` |
| PEMS-BAY incident audit | `pemsbay_incident_results.json`, `pemsbay_incident_protocol.json` |
| PEMS-BAY model-free event-study (`fig:pemsbay_eventsudy`) | `pemsbay_eventsudy_results.json`, `eval_pemsbay_eventsudy.py`, `plot_pemsbay_eventsudy.py` |
| PEMS-BAY donor-matched DiD summary (`tab:pemsbay_did`) | `pemsbay_eventsudy_results.json` |

## Protocols

- `localized_stress_protocol.json`
- `pemsbay_incident_protocol.json`
- `eval_pemsbay_eventsudy.py` (repo root)
- `audit/README.md`
- `audit/run_all.sh`
- `audit/make_manifest.py`

## Data attribution

- PEMS-BAY traffic speeds and incidents come from the Kaggle package
  `khoibut/pems-bay`, licensed MIT at the package level.
- The traffic tensor is 52,116 x 325 speed windows from 2017-01-01 to
  2017-06-30 and matches the processed PEMS-BAY dataset used in the paper.
- The incident CSV is a curated mirror of Caltrans/PeMS incident records with
  `nearest_sensor_id` mapping and GPS locations.
- Before public release, confirm whether the underlying Caltrans/PeMS data
  requires separate attribution or a data-use acknowledgment in addition to
  the Kaggle package MIT license.

## Author metadata

The submission is anonymized with `Anonymous Authors` and an anonymous-review
thanks block. The following fields are still required for the camera-ready
version:

- Author names, order, affiliations, and emails
- Funding statement
- Anonymous repository URL or DOI (the main text currently defers this to the
  camera-ready version)

## Submission checklist

- [ ] Replace the `TODO` author/funding placeholders with real metadata
- [ ] Add the repository URL and DOI in `main.tex`
- [ ] Run `python audit/make_manifest.py` and refresh
  `audit/audit_manifest.json` before final upload
- [x] Recompile `paper_tkde/main.tex` and verify 18 pages
- [x] Check no undefined references
- [x] Sync `TKDE_submission_package/paper/main.pdf` and rebuild
  `TKDE_submission_package.zip`
- [ ] Run final two-reviewer revision pass
- [ ] Create the final upload zip and checksum after metadata is filled
