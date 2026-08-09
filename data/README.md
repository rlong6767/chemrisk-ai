# Data Folder

This folder contains **public schema files only** for the ChemRisk-AI Phase 1 capstone project.

The raw source workbooks, site-specific chemical lists, tank mappings, and detailed audit notes are intentionally omitted from the public repository. The files in this folder are meant to document the expected input structures so the notebook and project design can be reviewed without exposing private engineering data.

## Files

| File | Purpose | Contains real project rows? |
|---|---|---:|
| `public_colab_values_schema.csv` | Schema for the structured chemical-pair table used by the public notebook. It documents the expected columns for chemical pair descriptors, mechanism flags, consequence/severity fields, engineered features, labeling-function outputs, and weak labels. | No |
| `gold_set_audit_schema.csv` | Schema for the initial audited review table. It documents the columns used to capture expert/auditor review fields, RTD detectability judgments, RTD-required labels, control-failure-mode notes, and labeling-function diagnostics. | No |
| `prospective_audit_schema.csv` | Schema for the prospective/stress-test audit table. It uses the same column structure as the gold-set audit schema but is intended for later review rows selected after earlier rule versions were frozen. | No |

## How these files are used

The public notebook expects private or synthetic input files with the same column headers as these schema files.

In the public repository, these CSVs are intentionally empty except for headers. They are documentation artifacts, not training datasets.

To run the notebook end-to-end, provide one of the following:

1. a private internal dataset matching these schemas, or
2. a fully synthetic dataset with the same column names and compatible values.

Do **not** commit private source workbooks, real site identifiers, tank IDs, raw engineering notes, or detailed audit comments to the public repository.

## Schema overview

### `public_colab_values_schema.csv`

This schema represents the structured chemical-pair input used by the public notebook.

Major column groups include:

- pair identity fields such as `pair_id`, `chemical_a_label`, and `chemical_b_label`,
- generic chemical-class fields such as `chemical_a_class` and `chemical_b_class`,
- mechanism and setting fields such as `reaction_type`, `reaction_speed`, `runaway_potential`, `self_limiting`, and `setting`,
- source hazard flags such as `toxic_gas`, `generates_gas`, `generates_heat`, `explosive_potential`, `flammable`, and `corrosive`,
- source consequence/severity fields such as `legacy_theoretical_severity`, `expert_adjusted_severity`, and `final_severity`,
- engineered feature flags such as oxidizer, acid/base, chelant, silicate, metal/complex, diluent, and detectability-proxy fields,
- RTD labeling-function outputs such as `LF1_NoHeat_or_LowSeverity_NoRTD` through `LF9_Thermal_Default_RTD`,
- weak-supervision summary fields such as `weak_vote_prob`, `weak_vote_count`, `weak_rtd_label`, and `lf_conflict_flag`,
- and severity-demotion diagnostic fields such as `weak_gold_lt_c5_label` and `severity_lf_conflict_flag`.

### `gold_set_audit_schema.csv`

This schema represents the initial expert-reviewed audit table.

Major column groups include:

- pair and mechanism descriptors,
- source severity and scoring fields,
- audit-selection fields such as `audit_bucket`, `candidate_hypothesis`, and `suggested_audit_focus`,
- expert-review fields such as `gold_severity_num`, `gold_lt_c5`, `reviewed_mechanism_summary`, `audit_confidence`, and `final_auditor_notes`,
- RTD-specific review fields such as `rtd_detectable`, `rtd_required`, and `control_failure_mode`,
- mechanism-credibility fields such as `toxic_gas_credible`, `exotherm_present`, `exotherm_hazardous`, `runaway_credible`, and `pressure_hazard_credible`,
- and the same labeling-function diagnostic fields used to compare weak labels with audit conclusions.

### `prospective_audit_schema.csv`

This schema uses the same column structure as `gold_set_audit_schema.csv`.

The intended use is different: prospective audit rows are selected after a rule/model version is frozen so they can provide a cleaner stress test than rows used during initial labeling-function development.

## Column notes

- Fields beginning with `LF` are RTD labeling-function votes or diagnostics.
- Fields beginning with `SLF` are severity-demotion labeling-function votes or diagnostics.
- `weak_rtd_label` is a weak-supervision target used for model development. It is not a final engineering approval.
- `rtd_required` is an audited or review-derived field used to evaluate/calibrate the RTD decision logic.
- `gold_lt_c5` and `weak_gold_lt_c5_label` relate to severity-demotion review and are secondary to the main RTD-usefulness target.
- If an export includes a placeholder column such as `Unnamed: 41`, treat it as a spreadsheet/export artifact rather than a model feature.

## Privacy boundary

The public data folder intentionally excludes:

- real site names,
- tank IDs,
- chemical supplier or customer-specific identifiers,
- raw compatibility-matrix exports,
- detailed private audit notes,
- proprietary operating context,
- and internal engineering workbooks.

This boundary is deliberate. ChemRisk-AI is presented as a reproducible decision-support workflow and portfolio project, not as a release of private process-safety data.

## Recommended public workflow

1. Keep these schema files in the public repository.
2. Keep private or site-specific workbooks outside the public repository.
3. Use synthetic data for public demos.
4. Use private data only in controlled internal environments.
5. Treat model outputs as engineering review support, not automatic safeguard approval.
