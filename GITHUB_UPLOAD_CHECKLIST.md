# GitHub Upload Checklist

Use this checklist for the current public ChemRisk-AI repository package.

## Recommended to include

### Root

- `README.md`
- `GITHUB_UPLOAD_CHECKLIST.md`
- `.gitignore`
- `LICENSE`

### Notebook

- `notebooks/01_chemrisk_ai_phase1_mvp_final_public_v11.ipynb`

Use the public notebook only. Do not upload private notebook versions, private site-mapping files, private R&D evidence, or notebook copies with full private outputs.

### Documentation

- `docs/ChemRisk_AI_Capstone_Design_Doc_Final_GitHub_Review_v1_4.pdf`
- `docs/ChemRisk_AI_Capstone_Design_Doc_Final_GitHub_Review_v1_4.docx`

### Figures

- `figures/01_chemrisk_ai_architecture.png`
- `figures/02_v12b_class_imbalance.png`
- `figures/03_v12b_audit_confusion_matrix.png`
- `figures/04_v12b_grouped_cv_metrics.png`
- `figures/05_pair_level_recommendation_distribution.png`
- `figures/06_directional_scenario_recommendation_distribution.png`
- `figures/07_stored_service_recommendation_distribution.png`
- `figures/08_rtd_candidate_stored_service_priorities.png`
- `figures/figure_index.csv`

Regenerate figures only if rerunning the notebook changes the final metrics, recommendation counts, or chart labels.

### Outputs

- `outputs/chemrisk_ai_phase1_final_summary.csv`
- `outputs/chemrisk_ai_phase1_model_card.csv`

Regenerate these output files after any logic change that affects labeling functions, weak labels, model metrics, final recommendation counts, or explanation-layer outputs.

### Data documentation and schemas

- `data/README.md`
- `data/public_colab_values_schema.csv`
- `data/gold_set_audit_schema.csv`
- `data/prospective_audit_schema.csv`
- `data/generic_chemical_vocabulary.csv`
- `data/CHEMICAL_MAPPING_GUIDE.md`

The public data files should be schema/reference artifacts only. They should not contain private source data, site chemical names, tank IDs, private audit comments, or site-specific mapping rows.

## Recommended to keep private

Do not upload:

- Private/internal chemical-pair source workbooks.
- Private tank mapping files, including files similar to `site_tank_generic_mapping_private*.csv` or private tank-level design-basis exports.
- Private tank-level audit workbooks, including candidate/non-candidate scenario detail tabs generated from actual site mappings.
- R&D/lab reports, proprietary test results, Ashley review notes, internal emails, or private project tables.
- Private notebook versions, including `*_private_*.ipynb`.
- Any notebook copy with executed outputs showing private row-level audit notes, site-specific chemical names, real tank IDs, or private mapping comments.
- `gold_set_audit_template_50_first11v4_with_severity_LFs_v4*.xlsx`.
- `gold_set_audit_template_next_15_frozen_v1dot1_AUDIT_IMPORTED_FORMULAS_RESTORED*.xlsx`.
- `chemical_pair_risk_dataset_public_v1_with_RTD_and_severity_LFs_v4*.xlsx`, unless manually sanitized and confirmed safe to publish.
- Any workbook with manual audit notes, raw import tabs, supplier/site-specific notes, LLM-generated audit details, or proprietary model-calibration evidence.
- Any generated CSV or Excel workbook containing actual site/tank mappings, real tank names, material numbers, SAP references, site comments, normal inventory levels, or private SDS-review notes.

## Public/private boundary checks before upload

Before committing files, check that the public package:

- Uses generic chemical labels/classes rather than real site chemical names.
- Uses sanitized pair IDs rather than private tank IDs or project-specific identifiers.
- Describes LF17 generically as an acidic/sulfonic surfactant plus alkanolamine clean-exotherm pathway.
- Describes LF18 generically as an alkali-base plus chlorinated-oxidizer heat-of-dilution / no-significant-reaction pathway.
- Does not include private R&D-confirmed pair flags, private pair IDs tied to real chemicals, or internal report text.
- Does not include private Florence tank/service mapping rows.
- Does not include long-form private audit exports generated from real site mappings.
- Does not include actual SDS files unless they are explicitly approved for publication.

## Expected repository structure

```text
chemrisk-ai/
├── README.md
├── GITHUB_UPLOAD_CHECKLIST.md
├── LICENSE
├── .gitignore
├── notebooks/
│   └── 01_chemrisk_ai_phase1_mvp_final_public_v11.ipynb
├── docs/
│   ├── ChemRisk_AI_Capstone_Design_Doc_Final_GitHub_Review_v1_4.docx
│   └── ChemRisk_AI_Capstone_Design_Doc_Final_GitHub_Review_v1_4.pdf
├── figures/
│   ├── 01_chemrisk_ai_architecture.png
│   ├── 02_v12b_class_imbalance.png
│   ├── 03_v12b_audit_confusion_matrix.png
│   ├── 04_v12b_grouped_cv_metrics.png
│   ├── 05_pair_level_recommendation_distribution.png
│   ├── 06_directional_scenario_recommendation_distribution.png
│   ├── 07_stored_service_recommendation_distribution.png
│   ├── 08_rtd_candidate_stored_service_priorities.png
│   └── figure_index.csv
├── outputs/
│   ├── chemrisk_ai_phase1_final_summary.csv
│   └── chemrisk_ai_phase1_model_card.csv
└── data/
    ├── README.md
    ├── public_colab_values_schema.csv
    ├── gold_set_audit_schema.csv
    ├── prospective_audit_schema.csv
    ├── generic_chemical_vocabulary.csv
    └── CHEMICAL_MAPPING_GUIDE.md
```

## Optional future additions

- A fully synthetic runnable dataset.
- A synthetic-data notebook branch.
- A Streamlit or CLI demo using synthetic scenarios.
- Row-level explanation CSV exports generated only from synthetic data.
- A small example tank-mapping file using fictional tank IDs and fictional chemical services.
- A model-governance note explaining how future SME reviews would be versioned.

## Final upload QA

After uploading, confirm:

- The root `README.md` renders correctly on GitHub.
- All README figure links display correctly.
- The notebook opens in GitHub preview.
- The `docs/` PDF opens.
- The data folder contains only schema/reference files.
- No private or site-specific CSV/XLSX/PDF/DOCX files were accidentally committed.
