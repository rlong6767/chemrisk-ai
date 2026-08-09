# Data Folder

This folder contains public schema files only for the ChemRisk-AI Phase 1 capstone project.

The raw source workbooks, site-specific chemical lists, tank mappings, and detailed audit notes are intentionally omitted from the public repository. The files in this folder are meant to document the expected input structures so the notebook and project design can be reviewed without exposing private engineering data.

## Files

| File | Purpose | Contains real project rows? |
|---|---|---:|
| `public_colab_values_schema.csv` | Schema for the structured chemical pair table used by the public notebook. It documents the expected columns for chemical pair descriptors, mechanism flags, consequence/severity fields, engineered features, labeling function outputs, and weak labels. | No |
| `gold_set_audit_schema.csv` | Schema for the initial audited review table. It documents the columns used to capture expert/auditor review fields, RTD detectability judgments, RTD required labels, control failure mode notes, and labeling function diagnostics. | No |
| `prospective_audit_schema.csv` | Schema for the prospective/stress test audit table. It uses the same column structure as the gold set audit schema but is intended for later review rows selected after earlier rule versions were frozen. | No |

## How these files are used

The public notebook expects input files with the same column structure, generalized chemical labels, generic chemical classes, and categorical value conventions used in the Phase 1 ChemRisk-AI workflow.

These schema files are included to document the expected structure of the private development inputs. They are not public training datasets, and they are not enough by themselves to make the notebook generalize to arbitrary site chemicals.

In the public repository, these CSVs are intentionally empty except for headers. They are documentation artifacts that show the shape of the private inputs while avoiding release of private source workbooks, site identifiers, tank IDs, raw engineering notes, and detailed audit comments.

To run the notebook end-to-end, one of the following would be needed:

1. the original private/internal dataset prepared with the same generalized chemical-label and class conventions,
2. a synthetic dataset created to match the same schema, chemical-class vocabulary, and allowed categorical values, or
3. a new site dataset that has first been mapped, reviewed, and normalized into the same generic ChemRisk-AI feature format.

A user should not assume that simply renaming their own chemical compatibility file to match these column headers is sufficient. Actual chemicals must be mapped to the generic labels/classes used by the model, and that mapping would require SME review, SDS or compatibility-matrix interpretation, concentration/grade checks, and validation of whether the modeled generic class is appropriate.

Do not commit private source workbooks, real site identifiers, tank IDs, raw engineering notes, or detailed audit comments to the public repository.

## Generic chemical vocabulary and mapping

The model does not operate directly on raw chemical names from a site inventory. During the Phase 1 workflow, actual chemical identities were converted into sanitized generic chemical labels and broader generic chemical classes.

For example, a private site chemical may be represented publicly as a generic label such as `strong_base_01`, `alkaline_silicate_01`, or `sulfonic_acid_surfactant_01`, with a corresponding class such as `alkali_base`, `alkaline_silicate`, or `acidic_anionic_surfactant`.

A separate vocabulary file, `generic_chemical_vocabulary.csv`, documents the public generic labels/classes used by the ChemRisk-AI workflow. That file is intended to help reviewers understand the modeled chemical universe and how actual site chemicals would need to be mapped before using the notebook.

A new site cannot simply rename its chemicals to match these labels. Mapping actual chemicals into the generic vocabulary requires SME review of SDS information, chemical function, concentration/grade, incompatibility behavior, and whether the modeled generic class is appropriate. If no appropriate generic class exists, the chemical should be treated as outside the current model scope until the vocabulary, labeling logic, and validation set are expanded.

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
