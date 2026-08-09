# ChemRisk-AI Chemical Mapping Guide

This guide explains how to use the public ChemRisk-AI generic chemical vocabulary. It is intended for reviewers who want to understand how the public notebook represents chemicals after sanitization.

The Phase 1 model does **not** operate directly on raw site chemical names. Actual site chemicals were converted into sanitized generic labels and broader generic chemical classes before modeling.

Examples of public generic labels include:

- `strong_base_01`
- `alkaline_silicate_01`
- `sulfonic_acid_surfactant_01`
- `chlorinated_oxidizer_01`
- `glycol_ether_solvent_01`

Examples of public generic classes include:

- `alkali_base`
- `alkaline_silicate`
- `acidic_anionic_surfactant`
- `chlorinated_oxidizer`
- `glycol_ether_solvent`

The file `generic_chemical_vocabulary.csv` lists the sanitized labels/classes used in the public Phase 1 workflow and gives plain-language mapping notes.

---

## Important boundary

Matching the CSV column headers is **not enough** to make a new site dataset valid for ChemRisk-AI.

A new dataset must also use the same generalized chemical-label vocabulary, generic chemical classes, allowed categorical values, and feature conventions expected by the Phase 1 notebook. A user should not assume that simply renaming a site compatibility file to match the schema headers is sufficient.

ChemRisk-AI is a decision-support workflow, not an automatic chemical truth engine or safety approval tool. Mapping actual chemicals into the public generic vocabulary requires subject-matter review.

---

## Mapping workflow

Use the following workflow before attempting to run a site dataset through the notebook:

```text
Actual site chemical
→ SDS / formulation / compatibility review
→ generic chemical class
→ generic chemical label
→ chemical-pair scenario
→ direction-specific A_into_B / B_into_A scenario
→ stored-service / tank-level recommendation
→ SME validation before crediting or rejecting RTD
```

The mapping should be documented. At minimum, a private site crosswalk should include:

```text
actual_site_chemical_name
CAS_number_or_product_identifier_if_available
site_concentration_or_grade
generic_chemical_label
generic_chemical_class
mapping_basis
mapping_confidence
mapping_reviewer
mapping_review_date
mapping_notes
```

Do **not** publish that private site crosswalk if it contains real chemical names, tank IDs, supplier information, proprietary formulations, or site-specific audit notes.

---

## Suggested mapping confidence levels

Use a controlled mapping-confidence field when building a private site crosswalk.

| Confidence | Meaning | Recommended use |
|---|---|---|
| High | SDS, formulation knowledge, and SME review clearly support the generic class and label. | Reasonable for model screening, still subject to site validation. |
| Medium | Chemistry appears to fit the generic class, but concentration, formulation detail, or mechanism evidence is incomplete. | Run as Review; do not treat as automatic recommendation. |
| Low | Mapping is uncertain or only based on weak name similarity. | Do not rely on model output without SME review. |
| Out of scope | No current generic class fits the chemical or mechanism. | Do not force-map; expand vocabulary and validation basis first. |

---

## How to choose a generic class

Use the chemical's function, chemistry, SDS hazards, incompatibility behavior, concentration/grade, and process use. Do not use only the product name.

Examples:

- A strong caustic/alkaline material may map to `alkali_base`.
- A silicate-containing alkaline material may map to `alkaline_silicate`, not generic `alkali_base`.
- A mineral/inorganic acid may map to `mineral_acid`.
- An acid with oxidizing behavior may map to `oxidizing_acid`, not generic `mineral_acid`.
- A hypochlorite/chlorinated oxidizer may map to `chlorinated_oxidizer`.
- A nonchlorinated oxidizer may map to `inorganic_oxidizer`.
- A surfactant should be mapped by surfactant type where possible: `anionic_surfactant`, `acidic_anionic_surfactant`, `nonionic_surfactant`, `amphoteric_surfactant`, or `amine_oxide_surfactant`.
- A polymer emulsion or dispersion should not be treated as a simple liquid unless the phase, viscosity, coagulation, and fouling behavior have been reviewed.

If a chemical does not fit one of the public generic classes, do not force it into the nearest label just to make the notebook run. Mark it `Out of scope` and expand the vocabulary and validation basis first.

---

## How to choose a numbered generic label

Some classes have multiple sanitized labels, such as:

- `strong_base_01` and `strong_base_02`
- `chelant_01` and `chelant_02`
- `glycol_ether_solvent_01`, `glycol_ether_solvent_02`, and `glycol_ether_solvent_03`
- `polymer_emulsion_01` and `polymer_emulsion_02`

The number does not mean the public repository is disclosing a specific real chemical identity. It means the Phase 1 public dataset preserved separate sanitized modeled services within the same broader class.

For a new site, do not guess the numbered label from the public file alone. Use one of these approaches:

1. map the chemical to an existing numbered label only when a private/internal mapping decision supports it,
2. create a synthetic-demo label if the purpose is only to demonstrate the notebook structure, or
3. mark the chemical as outside the current modeled vocabulary until the vocabulary and labeling logic are expanded.

---

## Directionality and tank context

ChemRisk-AI expands each pair into directional scenarios such as:

```text
A_into_B
B_into_A
```

This matters because an RTD is installed on a specific tank. If chemical A enters a tank storing chemical B, the relevant inventory, dilution, mixing, RTD location, and response basis may differ from the reverse scenario.

A site mapping therefore needs more than chemical labels. It also needs tank/service context, including:

```text
stored chemical
incoming abnormal contaminant
credible transfer direction
tank inventory or level basis
concentration / grade
mixing assumptions
RTD location / thermowell basis
alarm, operator response, or interlock basis
stored-service RTD reliability concerns
```

---

## Controlled categorical values used by the Phase 1 schema

The public Phase 1 dataset used the following categorical values. New data must either use these values or be normalized into them before running the notebook.

### `reaction_type`

```text
acid_base_neutralization
hypochlorite_plus_acid
oxidizer_plus_ammonia_amine
oxidizer_plus_organic_reducer
other_unknown
```

### `reaction_speed`

```text
fast_sec_min
slow_hr_days
unknown
```

### `setting`

```text
indoor_limited_ventilation
mixed_indoor_outdoor
outdoor_well_ventilated
unknown
```

Binary feature columns such as `toxic_gas`, `generates_gas`, `generates_heat`, `explosive_potential`, `flammable`, and `corrosive` should use the same 0/1 convention as the public schemas.

---

## What not to do

Do not:

- treat the public schema files as complete public training datasets,
- assume column names alone are sufficient,
- map chemicals based only on superficial name similarity,
- force chemicals into generic labels when chemistry or concentration is uncertain,
- publish real site chemical-to-generic mappings if they reveal confidential site information,
- use the output as automatic approval to install or omit RTDs,
- replace HAZOP, PHA, MOC, materials engineering, or process safety review.

---

## Practical review checklist

Before using a mapped dataset for ChemRisk-AI screening, confirm:

- [ ] every actual chemical has a documented generic label and class,
- [ ] mapping confidence is recorded,
- [ ] SDS/formulation/SME evidence supports the mapping,
- [ ] concentration and grade are considered,
- [ ] reaction type and hazard flags are normalized to the Phase 1 schema,
- [ ] directionality is represented as `A_into_B` and `B_into_A` where applicable,
- [ ] stored-service/tank context is reviewed before final RTD credit,
- [ ] out-of-scope chemicals are not forced into the current vocabulary,
- [ ] private site identifiers and audit notes are not committed to the public repository.

---

## Bottom line

The generic vocabulary makes the public project more transparent, but it does not make ChemRisk-AI plug-and-play for arbitrary site inventories. It shows the modeled chemical universe used by the Phase 1 MVP and documents how a future site would need to normalize and review its chemicals before using the notebook output.
