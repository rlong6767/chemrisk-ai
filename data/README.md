# Data README

The original Phase 1 notebook used three Excel workbooks:

1. a sanitized public chemical-pair dataset workbook,
2. an initial gold-set audit workbook,
3. a completed prospective 15-row audit workbook.

Those raw audit workbooks are intentionally not included in this GitHub-safe package because they contain detailed engineering review notes. The CSV files in this folder document the expected column schemas only; they do not contain audit rows.

For a fully runnable public demo, create synthetic input files with these same schemas or adapt the notebook to load a generated synthetic dataset.

Expected sheet names in the original private workbooks:

- Public workbook: `Public_Colab_Values`
- Initial audit workbook: `Gold_Set_Audit_50`
- Prospective audit workbook: `Gold_Set_Audit_50` and `v1_1_Candidate_Context`

Recommended publication approach:

- Include this schema documentation, final outputs, figures, and the stripped public notebook.
- Keep raw gold-set audit workbooks private.
- Use synthetic data for any future fully runnable demo.
