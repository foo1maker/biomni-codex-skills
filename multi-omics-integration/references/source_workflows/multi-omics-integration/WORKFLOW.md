# Multi Omics Integration

Source workflow: `multi-omics-integration`  
Parent Claude Science skill: `multi-omics-integration`

## Purpose

Integrate two or more omics views with MOFA+ to learn interpretable latent factors that capture shared and view-specific variation, tolerate incomplete sample overlap, quantify variance decomposition, and provide factor scores and feature weights for downstream analysis.

## When to use

- Identify shared sources of variation across multiple omics layers.
- Determine which omics views contribute to each latent factor.
- Integrate views with incomplete sample overlap.
- Generate factor scores for patient stratification, clinical association, or survival analysis.
- Rank features by their factor weights within each omics view.

## Inputs

- Named list of at least two feature-by-sample matrices, one matrix per omics view. (required)
- At least ten samples per view. (required)
- Optional sample metadata with sample identifiers and clinical variables. (optional)
- Per-view data-type information, including whether any view is binary and should use a Bernoulli likelihood. (optional)
- Requested number of latent factors; fifteen is the stated starting point and a custom value is allowed. (optional)

## Outputs

- Serialized trained MOFA model for downstream reuse.
- Sample-by-factor score table.
- Per-view feature-weight tables.
- Variance explained per factor and view and total variance explained per view.
- Top features per factor and view.
- Variance-decomposition, factor-scatter, factor-correlation, top-weight, factor-heatmap, and optional clinical-association figures in PNG and SVG.
- Markdown summary and optional PDF report.

## Workflow

1. Confirm the omics matrices, factor count, view types, binary views, and optional clinical metadata.
2. Load each feature-by-sample view, align sample identifiers, and preserve incomplete overlap for native missing-data handling.
3. Train an unsupervised MOFA+ model with the chosen factor count.
4. Generate variance-decomposition, factor-score, feature-weight, correlation, heatmap, and clinical-association visualizations as applicable.
5. Export the trained model, factor scores, weights, variance-explained tables, and top features.
6. Interpret each factor by its view-level variance, feature weights, sample scores, and optional clinical associations.
7. When requested, package the summary, tables, and generated figures into a report.

## Decision rules

- Require at least two omics views and at least ten samples per view; do not use this workflow for a single omics matrix.
- Use a Bernoulli likelihood for binary views.
- Allow incomplete sample overlap because MOFA+ handles missing data across views natively.
- Start with fifteen factors unless a different factor count is chosen; use five factors for the stated quick example.
- If the model does not converge, reduce the factor count to approximately five to ten and confirm adequate sample size.
- If memory is limiting, filter each view to its 5,000 most variable features before model fitting.
- Interpret a factor with high explained variance in one view as view-specific variation and one with high explained variance across views as shared cross-omics signal.
- Treat a low total explained-variance value as evidence that the model explains little of that view and consider adding features or views.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_701c7432cc435e2e` — MOFA2: Use for unsupervised latent-factor integration of two or more omics views, including incomplete overlap.

### Secondary resources

- `rr_be2f20c0b2e2cc10` — ComplexHeatmap: Use for factor-by-sample heatmaps with annotations.
- `rr_b6e8fe811cc71411` — MOFAdata CLL example: Use for the built-in multi-view example analysis.
- `rr_168be771db70bfdc` — ggprism, circlize, reshape2, and RColorBrewer: Use as plotting and data-shaping support packages.

### Fallback resources

- `rr_14d87a203624be0c` — Reduced factor count: Use when training does not converge or the sample count is too small for the initial factor count.
- `rr_a52ba8c533a68967` — Top 5,000 most variable features per view: Use when the full dataset causes a memory error.
- `rr_53d91093139c15e6` — Analysis without trait plots: Use when optional clinical metadata cannot be loaded.
- `rr_e5eee8a085e60a52` — PNG-only figures: Use when SVG export support is unavailable.

### Optional resources

- `rr_133070ab4f544c1e` — MOFA2: MOFA+ model construction, training, and result extraction.
- `rr_5c443cd6e2004110` — MOFAdata: Example multi-omics data.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Annotated factor heatmaps.
- `rr_9c40795c5beef2c4` — ggprism: Plot styling.
- `rr_77b4f130e7222be2` — circlize: Color and heatmap support.
- `rr_e67988bf391aceea` — reshape2: Data reshaping.
- `rr_c50aaccbb6322ac4` — RColorBrewer: Color palettes.
- `rr_05511b62ca7c332a` — rmarkdown: Optional fallback PDF generation.
- `rr_e7e4458f66082dd1` — MOFA+: Unsupervised latent-factor model that decomposes multi-view variation into shared and view-specific factors and handles missing observations.

## Validation / QC

- Confirm at least two views and report each view's feature and sample dimensions before training.
- Check model convergence and the variance-explained summary after training.
- Preserve factor scores, per-view weights, per-factor variance, and total variance so every interpretation can be traced to exported values.
- Use clinical-association plots only when matching metadata are available.
- Use graceful SVG fallback and retain PNG output when SVG dependencies are unavailable.
- Evidence requirement: Support claims of shared signal with high explained variance across multiple views rather than factor score patterns alone.
- Evidence requirement: Support factor interpretation jointly with its view-level variance, high-weight features, sample scores, and clinical association when metadata are available.
- Evidence requirement: Use the exported factor scores as the data source for downstream stratification or survival analysis.
- Evidence requirement: Use top weighted features per factor as the data source for downstream biomarker or pathway analysis.

## Failure handling

- Using a multi-view integration model on only one omics layer.
- Attempting to train too many factors for the available sample size, causing non-convergence.
- Treating a view-specific factor as shared cross-omics biology.
- Interpreting a low-variance view as well explained.
- Losing samples unnecessarily by requiring complete overlap even though the model handles missing view entries.
- Fallback rule: If training does not converge, retry with five to ten factors and ensure each view has at least ten samples.
- Fallback rule: If clinical metadata retrieval fails, continue without clinical trait plots and disclose that metadata were optional.
- Fallback rule: If the dataset exceeds memory, select the 5,000 most variable features in each view.
- Fallback rule: If SVG export fails, keep PNG figures.
- Fallback rule: If the dedicated reporting capability is unavailable, use the packaged RMarkdown output and disclose the fallback.

## Limitations

- The workflow is unsupervised and is not a supervised prediction pipeline.
- It is not recommended for fewer than ten samples per view.
- Although the software supports single-cell multimodal data, the archived workflow directs single-cell questions elsewhere.
- A factor active in only one view may represent technical or biological variation specific to that view rather than shared biology.
- Low total explained variance means the fitted factors capture little of that view.

## Important domain-specific rules

- A multi-view input contract that permits incomplete sample overlap and records likelihood choices per view.
- An adaptive factor-count gate that reduces model complexity when convergence or sample size is limiting.
- Variance-decomposition-first interpretation separating shared cross-view from view-specific factors.
- A factor evidence bundle combining scores, weights, explained variance, and optional clinical associations.
- Downstream handoff through exported factor scores and top feature weights.

## Portability boundary

- The packaged R script entry points, exact calls, verification strings, and low-freedom execution policy. — migration action: `exclude_or_capability_map`
- The built-in CLL example loader and package-relative interpretation guide. — migration action: `exclude_or_capability_map`
- The Biomni pdf-report-generation skill and platform report orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
