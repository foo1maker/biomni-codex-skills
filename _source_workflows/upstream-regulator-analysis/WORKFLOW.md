# Upstream Regulator Analysis

Source workflow: `upstream-regulator-analysis`  
Parent Claude Science skill: `regulatory-network-analysis`

## Purpose

Identify candidate transcriptional regulators by integrating ChIP-Atlas binding enrichment with bulk RNA-seq differential-expression results and directional target concordance.

## When to use

- Rank transcription factors that may drive an observed bulk differential-expression signature.
- Integrate binding enrichment, target overlap, and directionality into a combined regulatory score.
- Classify candidate regulators as activator-like, repressor-like, or mixed from directional concordance.

## Inputs

- A CSV or TSV differential-expression table containing gene symbol, log2 fold change, and adjusted p-value. (required)
- A supported genome identifier, with human hg38 as the default example. (required)
- Optional column-name overrides and enrichment thresholds. (optional)

## Outputs

- All and top-ranked regulator tables with combined score, Fisher p-value, concordance, and direction.
- Per-regulator target overlap and separate up- and down-gene ChIP-Atlas enrichment tables.
- A serialized analysis object, evidence visualizations, and human-readable reports.

## Workflow

1. Load and standardize the differential-expression table, auto-detecting supported column conventions when possible.
2. Run ChIP-Atlas enrichment independently for upregulated and downregulated gene sets and retrieve target genes for enriched transcription factors.
3. Calculate target differential-expression overlap with Fisher's exact test and directional concordance.
4. Rank regulators with the combined evidence score and assign activator-like, repressor-like, or mixed direction.
5. Generate ranked-score, target-overlap, evidence-scatter, and metric-heatmap figures and export all analysis artifacts.

## Decision rules

- Use this workflow for bulk RNA-seq differential-expression results, not single-cell differential expression or histone-mark-only analysis.
- Call a regulator activator-like or repressor-like only when concordance exceeds sixty percent; otherwise label it mixed.
- Interpret the combined score as a heuristic ranking rather than a formal multiple-testing correction.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_868a2a740cac4da2` — User-provided bulk differential-expression table: The user has a compatible gene-level differential-expression result.
- `rr_b4baa70e28fd2a99` — ChIP-Atlas: The selected genome is supported and internet access is available.

### Secondary resources

- `rr_bea33acc8f0d6f7e` — Estrogen-response or airway example differential-expression data: A real example analysis is requested.

### Fallback resources

- `rr_51e978dc662f3171` — Synthetic TP53-driven differential-expression data: A fast offline example is required.

### Optional resources

- `rr_e29ad26fff133343` — EBI Expression Atlas: Source of the real estrogen and airway example differential-expression datasets.
- `rr_d4bbf183d34fba1b` — pandas, NumPy, and SciPy: Table processing, numerical analysis, and Fisher's exact test.
- `rr_f48c55f08412b6c6` — requests: ChIP-Atlas API and data-server access.
- `rr_42bc4f5de0dd382f` — matplotlib and seaborn: Evidence visualizations.

## Validation / QC

- Require at least three genes in one differential-expression direction for enrichment.
- Restrict enrichment to transcription factors and related antigens rather than histone marks when target-gene integration is required.
- Evidence requirement: Retain binding enrichment, target-overlap significance, directional concordance, and target counts for each ranked regulator.
- Evidence requirement: Treat binding enrichment as mechanistic evidence to validate, not proof of regulatory causation.

## Failure handling

- A required sibling enrichment or target-gene component is missing.
- Both directional enrichment analyses fail because too few genes pass filtering.
- No transcription factors pass enrichment or have downloadable target-gene data.
- Fallback rule: When no regulator passes a strict enrichment threshold, relax the q-value cutoff to 0.1 or expand the differential-expression set and disclose the change.
- Fallback rule: When target-gene downloads repeatedly time out, reduce the number of transcription factors processed.

## Limitations

- Results are biased toward well-studied transcription factors and cell types represented in ChIP-Atlas.
- Directional labels simplify context-dependent regulation into activator, repressor, or mixed classes.
- Fisher's exact test assumes independent targets even though target genes can cluster in pathways.

## Important domain-specific rules

- Integrate independent binding, overlap, and directionality evidence instead of ranking regulators from a single enrichment statistic.
- Separate upregulated and downregulated gene enrichment before calculating target concordance.
- Label combined scores as heuristic and preserve each underlying evidence dimension.

## Portability boundary

- Mandatory bundled skill-local data-loading, integration, plotting, and export script calls with platform success tokens. — migration action: `exclude_or_capability_map`
- Biomni sibling-skill directory assumptions for ChIP-Atlas enrichment and target-gene retrieval. — migration action: `exclude_or_capability_map`
- Phylo-specific report presentation and platform PDF packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
