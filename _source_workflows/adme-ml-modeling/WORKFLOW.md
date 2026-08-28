# Adme Ml Modeling

Source workflow: `adme-ml-modeling`  
Parent Claude Science skill: `small-molecule-modeling-and-safety`

## Purpose

Build, honestly evaluate, save, and apply supervised single-endpoint small-molecule in-vitro ADME models from labelled assay tables.

## When to use

- Audit a labelled single-endpoint ADME assay table for modeling readiness.
- Train and prospectively assess an ADME regression or binary-classification model.
- Score new small molecules with uncertainty and applicability-domain status.

## Inputs

- A CSV, TSV, SDF, or SMI/SMILES table with a valid SMILES column. (required)
- A numeric regression label or a two-valued classification label for one endpoint. (required)
- Measurement date, assay-context fields, units, censor qualifiers, series labels, and compound identifiers. (optional)
- A run configuration defining split, feature sets, confidence level, calibration fraction, bootstrap count, and seed. (optional)

## Outputs

- Dataset audit and split assignments.
- Model comparison, locked-outer evaluation, and outer-test predictions with uncertainty and domain columns when supported.
- Serialized model bundle, model card, and a run manifest with input, artifact, configuration, and dependency hashes or versions.
- Prediction table and prediction manifest with applicability-domain counts, interval-width statistics, and any domain warning.

## Workflow

1. Construct an explicit dataset specification covering endpoint semantics, units, assay context, and censoring.
2. Audit and standardize the dataset before training; stop on blocked mixed assays, units, invalid mappings, or contradictory censoring.
3. Lock an outer partition first, select features and models only on outer-training data, and keep the locked test set out of calibration and threshold selection.
4. Read the model card, evaluation, and locked-test predictions while distinguishing inner selection from outer assessment.
5. Score new structures with the deployment bundle and surface out-of-domain rows, wide intervals, ambiguous class sets, and domain warnings before summarizing predictions.

## Decision rules

- Model one endpoint and one compatible assay signature per run; do not bypass mixed-assay or mixed-unit blocking without documented harmonization.
- Prefer temporal splitting when complete dates exist; otherwise use scaffold splitting, use deployment/MOOD only with actual prospective structures, and treat random splitting as an interpolation diagnostic.
- Preserve censor qualifiers as intervals and use censor-aware AFT fitting for censored positive quantities.
- Report locked-outer results, the 1-nearest-neighbor baseline, uncertainty coverage, and domain status rather than presenting inner-CV performance as prospective performance.
- When the prediction manifest reports a domain warning, lead the summary with that warning.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_438784fda4143b4d` — User-provided labelled assay table: The endpoint, assay signature, units, and labels are auditable and compatible.

### Secondary resources

- `rr_32d3c350d055e9c5` — Lipophilicity_AstraZeneca benchmark: A real public demonstration of the ordinary audit, train, and predict workflow is needed.

### Fallback resources

- `rr_82772eef38cc3f74` — Synthetic offline smoke-test generator: Only for network-free software smoke testing; its metrics must not be reported as real-assay performance.

### Optional resources

- `rr_1d51aff3c785c707` — MoleculeNet Lipophilicity_AstraZeneca: Real measured logD7.4 benchmark used for the default demonstration.
- `rr_0967ef8e69dbb761` — AqSolDB: External aqueous-solubility source used in the bundled example asset and exposed by the benchmark fetcher.
- `rr_4523b9e5501b8ada` — XGBoost AFT: Censor-aware fitting for qualified positive endpoint measurements.
- `rr_44f364c180bdeaba` — MAPIE: Conformal intervals or prediction sets.
- `rr_36d9c53a20997e27` — Splito MOOD: Select a split whose distance profile matches supplied prospective structures.
- `rr_6f9462850ac65945` — Dummy baseline: Baseline in the maintained uncensored model ladder.
- `rr_ba3a253afd8f6f97` — Morgan/Tanimoto 1-nearest-neighbor: Reference baseline for interpolation performance.
- `rr_d0c376fab60086e1` — Ridge or logistic model: Linear maintained candidate for uncensored tasks.
- `rr_f18674becf190529` — XGBoost model: Maintained nonlinear candidate; AFT variants are used for censored tasks.

## Validation / QC

- Fit the locked-test applicability-domain threshold and flags using outer-training molecules only.
- Preserve invalid structures in prediction outputs and label them invalid_structure rather than silently dropping them.
- Treat under-coverage on time or scaffold tests as a distribution-shift warning, not as a guarantee.
- State assay, unit, censoring, sample-size, and distribution-shift limitations when communicating results.
- Evidence requirement: Use locked-outer metrics, baseline lift, uncertainty coverage, and domain status as the evidence for model quality.
- Evidence requirement: Use real measured benchmark labels for demonstration headline numbers; never substitute synthetic data silently.
- Evidence requirement: Retain a run manifest with provenance and hashes or versions for inputs, artifacts, configuration, and dependencies.

## Failure handling

- Mixed assays, units, invalid class mappings, or contradictory censoring block the dataset audit.
- The pinned runtime is absent or an active-session environment is unsafe.
- The real public benchmark cannot be fetched unattended.
- Predictions are extrapolative when the scored structures are outside the training applicability domain.
- Fallback rule: Route execution to an isolated pinned environment when preflight reports an unsafe active-session environment or missing dependencies.
- Fallback rule: If the real demonstration fetch fails, stop and disclose the failure; use the synthetic generator only as an offline smoke test.

## Limitations

- An assay signature does not transfer automatically across protocols, species, matrices, or pH conditions.
- Conformal coverage relies on exchangeability and weakens under temporal or chemical distribution shift.
- An in-domain flag indicates support, not correctness; experimental confirmation is still required.
- Censored limits are intervals rather than exact measurements, so exact-error metrics apply only to exact observations.

## Important domain-specific rules

- Audit endpoint semantics, units, assay compatibility, structures, and censoring before model fitting.
- Use nested selection with a locked prospective-style outer assessment and leakage-free calibration.
- Attach uncertainty and applicability-domain status to every scored molecule and foreground extrapolation warnings.
- Preserve machine-readable artifacts, model documentation, and hashed run provenance.

## Portability boundary

- Registration of inspect_adme_dataset, train_adme_model, and predict_adme_model through Biomni agent.add_tool. — migration action: `exclude_or_capability_map`
- Bundled scripts/biomni_tools.py orchestration and skill-local runtime layout. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation and GenerateImage terminal reporting step. — migration action: `exclude_or_capability_map`
- Biomni-specific /workspace and /mnt/results path assumptions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
