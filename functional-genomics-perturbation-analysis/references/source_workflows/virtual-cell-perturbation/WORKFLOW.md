# Virtual Cell Perturbation

Source workflow: `virtual-cell-perturbation`  
Parent Claude Science skill: `functional-genomics-perturbation-analysis`

## Purpose

Benchmark predicted single-cell transcriptional responses against held-out measured perturbations across datasets, models, and generalization regimes.

## When to use

- Predict post-perturbation expression profiles for held-out perturbations.
- Compare perturbation models with a control-mean baseline using per-perturbation and regime-stratified metrics.
- Evaluate generalization to unseen single and combinatorial perturbation regimes.

## Inputs

- A named GEARS dataset or a compatible AnnData object containing condition, cell type or covariate, and gene names. (required)
- One or more model choices among scGPT, GEARS, and the control-mean baseline. (required)
- A train-from-base or checkpoint weight strategy for scGPT. (optional)
- A deterministic simulation split and seed or explicit training and test perturbation lists. (optional)

## Outputs

- Per-perturbation and regime-stratified benchmark metrics with a control-mean baseline.
- Prediction arrays, full benchmark state, and summary metadata retained as intermediate evidence.
- Six benchmark figures in raster and editable vector formats.
- A report covering data, split provenance, methods, metrics, regime-dependent results, caveats, references, and next steps.

## Workflow

1. Load the dataset, verify its license, create the requested split, and record perturbation-regime counts and the random seed.
2. For scGPT, obtain the base checkpoint and either fine-tune it under a recorded budget or load a compatible fine-tuned checkpoint.
3. Generate and persist predictions separately for each selected model.
4. Compute canonical absolute-expression and change-from-control metrics against held-out data and the control-mean baseline.
5. Stratify results by generalization regime, generate figures from real benchmark artifacts, and include a training curve only when training actually occurred.
6. Gather benchmark literature, verify every quantitative citation, and report scientific and compute limitations.

## Decision rules

- Judge perturbation skill primarily with change-from-control and top differential-expression direction metrics rather than absolute-expression correlations.
- Report the control-mean baseline's all-gene direction artifact separately and compare models on top differential-expression directions.
- Report regime-stratified results and label tiny regimes as illustrative.
- Use only open or commercial-use data and weights; flag unknown or restricted user-data licensing before execution.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_6fe6269b01549eb6` — Open GEARS-format perturbation dataset: The dataset license permits the intended use and the required AnnData fields are present.
- `rr_f31958e347b5ebc3` — Train-from-base scGPT_human weights: A license-clean scGPT benchmark is required and fine-tuning resources are available.

### Secondary resources

- `rr_184bffcaf124d061` — Compatible published fine-tuned checkpoint: A faster benchmark path is needed and its provenance and license are acceptable.

### Fallback resources

- `rr_4c54549e91b4595f` — Control-mean baseline: GPU software or model weights are unavailable, or a CPU-feasible reference is needed.

### Optional resources

- `rr_5c597bead53685d9` — GEARS perturbation datasets: Named benchmark datasets including norman, adamson, dixit, replogle_k562_essential, and replogle_rpe1_essential.
- `rr_c89e4f07b9642c65` — GEARS: Dataset handling, splits, graph-neural-network predictor, and canonical evaluation metrics.
- `rr_b3204199993691c1` — scGPT: Foundation-model perturbation predictor with train-from-base or checkpoint paths.
- `rr_8d11e5c5177366d3` — scGPT: Foundation-model predictor for post-perturbation expression.
- `rr_192df89215f98508` — GEARS: Graph-neural-network perturbation predictor.

## Validation / QC

- Record split seed and perturbation-regime composition and assert that the requested split was produced.
- Persist predictions immediately after each model run.
- Report the fraction of benchmark genes covered by a model vocabulary.
- Include a training curve only when a real fine-tuning state artifact exists.
- Evidence requirement: Report absolute-expression metrics, change-from-control metrics, top differential-expression direction agreement, baseline results, and regime-stratified sample counts.
- Evidence requirement: Verify every cited number against the returned literature records and do not fabricate citations.

## Failure handling

- The selected dataset or weights have unknown or restricted licensing.
- A full-gene-panel prediction batch exhausts accelerator memory.
- An execution environment expires before setup, training, or prediction finishes.
- Fallback rule: Use the CPU-feasible control-mean baseline when accelerator-dependent models cannot run.
- Fallback rule: Use a compatible fine-tuned checkpoint when time does not permit train-from-base, retaining checkpoint provenance.

## Limitations

- Absolute-expression correlations can look high even for a no-change baseline because most genes do not move.
- Generalization performance depends strongly on whether perturbed genes were seen during training.
- Vocabulary mismatch prevents some genes from being scored by scGPT.
- Tiny generalization regimes provide illustrative rather than stable estimates.

## Important domain-specific rules

- Benchmark every learned predictor against a no-change baseline on held-out perturbations.
- Prioritize change-from-control and top differential-expression direction metrics over absolute-expression correlation.
- Record deterministic split provenance and stratify results by generalization regime.
- Persist model predictions before downstream metric and visualization steps.

## Portability boundary

- Biomni Gpu sandbox provisioning, timeout semantics, accelerator type, environment bootstrap, and platform memory configuration. — migration action: `exclude_or_capability_map`
- Biomni-specific /workspace, /mnt/results, execution_trace, and S3-FUSE file-handling assumptions. — migration action: `exclude_or_capability_map`
- Bundled skill-local setup, data, training, prediction, metrics, figures, literature-formatting, and report scripts. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, Read media-output-check, and pdf-report-generation calls plus Phylo report branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
