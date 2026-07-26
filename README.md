# ChemRisk-AI Phase 1 MVP

Mechanism-aware weak-supervision workflow for chemical incompatibility screening, RTD safeguard recommendation, and direction-aware tank/service review.

## What this repository shows

This portfolio package documents an industrial AI capstone focused on one engineering decision: when an RTD is useful, creditable, and practical as a safeguard for an incompatible chemical mixing scenario.

The workflow demonstrates:

- spreadsheet-to-Python parity checks,
- mechanism-aware labeling functions,
- audit-calibrated weak labels,
- leakage-safe grouped cross-validation,
- model-assisted review routing,
- directionality expansion (`A_into_B` and `B_into_A`),
- stored-service RTD reliability review,
- and tank/service-level aggregation.

## Important data/privacy note

The raw gold-set audit workbooks are not included. They contain detailed engineering audit notes and calibration judgments that are not necessary for public portfolio review. This repository includes the final summary tables, model card, design document, figures, and a public source notebook with outputs stripped.

To run the full notebook end-to-end, provide private or synthetic input files matching the schemas in `data/`.

## Repository structure

```text
notebooks/
  01_chemrisk_ai_phase1_mvp_final_public.ipynb

docs/
  ChemRisk_AI_Capstone_Design_Doc_Directionality_Aware_v4.docx
  ChemRisk_AI_Capstone_Design_Doc_Directionality_Aware_v4.pdf

figures/
  01_chemrisk_ai_architecture.png
  02_v12b_class_imbalance.png
  03_v12b_audit_confusion_matrix.png
  04_v12b_grouped_cv_metrics.png
  05_pair_level_recommendation_distribution.png
  06_directional_scenario_recommendation_distribution.png
  07_stored_service_recommendation_distribution.png
  08_rtd_candidate_stored_service_priorities.png

outputs/
  chemrisk_ai_phase1_final_summary.csv
  chemrisk_ai_phase1_model_card.csv

data/
  README.md
  *_schema.csv
```

## Final model framing

This is not a production-validated chemistry predictor. It is a Phase 1 MVP and decision-support workflow. The model is trained on weak labels and audit-calibrated rules, not incident or laboratory outcome data. Safety-critical decisions remain human-in-the-loop and require site validation.

## Key results

See `outputs/chemrisk_ai_phase1_final_summary.csv` and the figures folder for the final summarized metrics and recommendation distributions.
