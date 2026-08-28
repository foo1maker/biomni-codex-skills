# Cell Therapy Qc Scorecard

Source workflow: `cell-therapy-qc-scorecard`  
Parent Claude Science skill: `cell-therapy-design-and-qc`

## Purpose

Convert single-cell RNA-seq measurements of a cell-therapy product into per-unit identity and safety module scores, GREEN/AMBER/RED module calls, and an overall release-oriented scorecard.

## When to use

- QC, characterize, release-test, or compare cell-therapy product units measured by single-cell RNA-seq.
- Assess target-cell identity and purity, residual pluripotency when applicable, off-target lineages, maturity when defined, and technical QC.

## Inputs

- Product description and target cell type, which drive marker resolution and adaptive module selection. (required)
- Product source: ipsc, esc, or primary; it may be inferred from the product description and determines whether residual-pluripotency scoring runs. (optional)
- One or more local 10x directories, CellRanger H5 files, H5AD files, or a GEO accession. (required)
- Engineering or transgene description for report annotation and feature detection. (optional)
- Optional metadata CSV mapping units to type, label, expected species, and condition. (optional)
- Species, human or mouse, with human as the documented default; multi-species settings may define the product-species retention fraction. (optional)
- Optional overrides for active modules and module-specific GREEN/AMBER/RED thresholds. (optional)

## Outputs

- A release-oriented report with a first-page per-unit call summary, module results, scorecard, discussion, limitations, next steps, and references.
- Seven machine-readable tables covering species composition, filtering, per-unit metrics, per-cell module scores, module and overall calls, threshold provenance, and a readable unit summary.
- A processed H5AD object per unit containing normalized data and module flags.
- PNG and SVG figures for QC distributions, scorecard heatmap, module overlays, and cross-unit comparisons.

## Workflow

1. Build a run configuration from the product description, target type, source, species, inputs, module set, and threshold overrides.
2. Load every unit, standardize gene symbols, and record raw cell counts.
3. For multi-species references, calculate per-cell species fractions and retain the configured product species; skip this step for single-species data.
4. Resolve target and off-target marker panels from CellMarker2, refined with product-specific literature, and emit the exact panels used.
5. Run per-unit QC, outlier detection, doublet detection, filtering, and normalization; clustering and UMAP are optional for figures.
6. Calculate per-cell flags for all active modules using the audited panels and product-conditional module set.
7. Aggregate flags to per-unit metrics, apply module thresholds, and assign the worst active module as the overall unit call.
8. Create QC and scorecard figures and assemble the final report after validating rendered output.

## Decision rules

- Always run identity and purity, off-target lineage, and technical QC modules; run residual pluripotency only for iPSC/ESC products and maturity only when a target-specific maturity axis exists.
- Assign the overall unit call as the worst active module; one RED call fails the unit.
- Anchor identity on raw expression of a small target-marker set rather than gating on a positive background-corrected signature score.
- Call residual pluripotency only with specific pluripotency-factor co-expression, exclude target-identity-positive cells, and validate against a per-unit shuffled-null threshold.
- Count off-target cells only when they co-express at least two markers of another lineage and are target-anchor-negative.
- When no cells meet a validated residual-pluripotency call, report below detection at this depth and cell count, not zero or absent.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_234055b40b781128` — CellMarker2 marker panels.: Resolving target-cell and off-target-lineage marker panels.

### Secondary resources

- `rr_672b3bbfa77e2e9f` — Product-specific literature for identity anchors, maturity axes, pluripotency panels, and threshold grounding.: Refining the primary marker source for the actual product context.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_937281a8af390f92` — CellMarker2: Primary source for target and off-target marker panels.
- `rr_ec2c9dff0cb1a776` — GEO: Optional input source for supplementary single-cell matrices.
- `rr_063ec900fce6482b` — scanpy: Single-cell QC, normalization, optional clustering and UMAP, and expression-based module calculations.
- `rr_379ab3b524dc4975` — scrublet: Per-unit doublet detection.

## Validation / QC

- Identity and purity defaults are GREEN at or above 90%, AMBER from 75% to 90%, and RED below 75%.
- Residual pluripotency defaults are GREEN below 0.01%, AMBER from 0.01% to 0.1%, and RED above 0.1%.
- Off-target lineage defaults are GREEN below 2%, AMBER from 2% to 10%, and RED above 10%.
- Maturity defaults are GREEN at or above 60%, AMBER from 40% to 60%, and RED below 40% when the module is active.
- Coerce observation masks to Boolean before applying bitwise negation or subsetting.
- Report actual per-unit doublet and cross-species contamination rates without rounding them away.
- Evidence requirement: Emit the exact marker panels used so that identity, maturity, pluripotency, and off-target calls remain auditable.
- Evidence requirement: Print the exact thresholds and their provenance in both the report and threshold-reference table.
- Evidence requirement: For residual-pluripotency release claims, state the single-cell detection limit and recommend an orthogonal assay.

## Failure handling

- A background-corrected signature score can become negative in obvious target cells when the target program dominates the transcriptome, causing false identity failures.
- Single-marker or nonspecific pluripotency rules can misclassify proliferating target cells as residual undifferentiated cells.
- Counting off-lineage transcripts in target-positive cells as contamination inflates the off-target fraction.
- Integer observation columns combined with bitwise NOT produce invalid indices unless explicitly converted to Boolean.
- Fallback rule: If metadata are absent, treat each input file or GEO sample as a separately named unit.
- Fallback rule: If a maturity axis does not exist for the target cell type, leave the maturity module inactive rather than inventing a proxy.

## Limitations

- The documented default module thresholds are guidance, not universal standards; product-specific release criteria require sponsor sign-off.
- Single-cell RNA-seq has a much coarser limit of detection for rare residual pluripotent cells than dedicated ddPCR or qPCR release assays.
- Generic exploratory single-cell analysis without a product or release framing is outside this skill.

## Important domain-specific rules

- Activate safety and maturity modules conditionally from product source and availability of a target-specific maturity axis.
- Roll module calls up by the worst-active-module rule and expose every threshold and its provenance.
- Use raw-expression identity anchors, specificity-gated residual calls, and target-negative-only off-target calls.
- Phrase negative rare-event findings as below detection and pair them with an orthogonal-assay recommendation.

## Portability boundary

- Package-local scorecard scripts and the delegated scrnaseq-scanpy-core-analysis script orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and package-to-package skill calls for marker and threshold grounding. — migration action: `exclude_or_capability_map`
- Phylo report branding, pdf-report-generation conventions, Read media checks, pypdf checks, and fixed /mnt/results paths. — migration action: `exclude_or_capability_map`
- worker-0, ManageMachine, persistent-kernel, and platform runtime assumptions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
