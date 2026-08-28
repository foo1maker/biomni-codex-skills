# Source workflows for biomarker-and-signature-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`consensus-disease-signature`](consensus-disease-signature/WORKFLOW.md) — Derive a direction-consistent consensus transcriptional signature across two or more independent human bulk-transcriptome cohorts using random-effects effect-size meta-analysis.
- [`disease-progression-longitudinal`](disease-progression-longitudinal/WORKFLOW.md) — Reconstruct disease-progression trajectories from longitudinal patient omics, identify trajectory-associated features, stratify progression rates, and validate computational staging.
- [`elastic-net-biomarker-panel`](elastic-net-biomarker-panel/WORKFLOW.md) — Select a minimal, interpretable binary-outcome biomarker panel from high-dimensional omics data using elastic-net or LASSO logistic regression, nested cross-validation, stability selection, and optional independent validation.
- [`signature-response-enrichment`](signature-response-enrichment/WORKFLOW.md) — Test whether residual on-treatment gene-signature activity marks patients who fail a drug and whether the direction reproduces across independently discovered cohorts.
