# Polygenic Risk Score Prs Catalog

Source workflow: `polygenic-risk-score-prs-catalog`  
Parent Claude Science skill: `genetic-causal-and-risk-analysis`

## Purpose

Calculate one or more polygenic risk scores from target genotypes using published PGS Catalog weights, with optional multi-trait profiling and population-level comparisons.

## When to use

- Score a trait with published, peer-reviewed PGS Catalog weights.
- Build a multi-trait risk profile and summarize correlations or composite risk.
- Compare score distributions across ancestry super-populations.

## Inputs

- Target genotypes in PLINK binary BED/BIM/FAM format, or the documented 1000 Genomes Phase 3 example data. (required)
- One or more PGS Catalog score identifiers. (required)
- Genome build, with GRCh37 as the documented default and GRCh38 also supported. (optional)

## Outputs

- Per-trait individual PRS z-scores, percentiles, and population labels.
- Per-trait distribution and population plots in PNG and SVG.
- Combined multi-trait score table, trait-correlation matrix, population summary, and variant-match reports.
- Dashboard correlation, composite-risk, and population heatmap figures.
- Reusable RDS analysis object containing combined scores, per-trait results, correlation matrix, match reports, weights, and trait metadata.
- Optional PDF report assembled from the markdown summary, exported tables, and figures.

## Workflow

1. Load the reference or user genotypes and obtain the selected PGS Catalog weights with the packaged loaders.
2. Score every selected trait using the packaged allele-matching and scoring workflow.
3. Generate all per-trait and dashboard visualizations.
4. Export score tables, match reports, summaries, and the reusable analysis object.
5. When requested, build the final PDF from the completed analysis artifacts and disclose any fallback report path.

## Decision rules

- Use this workflow for pre-computed published PGS weights; use the de novo LDpred2-auto skill when raw GWAS summary statistics must be converted into a new score.
- Ask first whether the user has genotype files or wants the 1000 Genomes example data.
- For example data, recommend the full five-trait cardiometabolic panel unless the user selects particular traits.
- Use matching genome builds for genotypes and PGS weights.
- Discover a valid score with search_pgs_catalog before downloading and scoring a custom trait.
- Use the packaged scoring, plotting, and export functions instead of reimplementing their logic inline.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_948aae13efecedf0` — PGS Catalog score files: A published score exists for the requested trait.
- `rr_643650ceeedd3919` — User-supplied PLINK genotypes: The user has target BED/BIM/FAM files to score.

### Secondary resources

- `rr_d5bda41934774c76` — 1000 Genomes Phase 3 example genotypes: A demonstration or population-comparison example is requested.
- `rr_fb4b59e4a44b36c1` — PGS Catalog search: The user knows the trait but not the current PGS identifier.

### Fallback resources

- `rr_3f590fd0a7e772f8` — polygenic-risk-score de novo workflow: No suitable pre-computed PGS Catalog score exists and GWAS summary statistics are available.
- `rr_44dc15f0c606e6eb` — Packaged markdown, HTML, or script report: The dedicated PDF-report capability is unavailable.

### Optional resources

- `rr_5c6d86a52ea80e04` — PGS Catalog: Source of published score definitions and variant weights.
- `rr_07ab965f6d0e9604` — 1000 Genomes Phase 3: Documented example genotype panel with 2,490 individuals and five super-populations.
- `rr_b0e2eea08100f3a1` — bigsnpr: Genotype handling and PRS scoring.
- `rr_95d0e2e781dcf6e9` — data.table: Efficient score and genotype table processing.
- `rr_1467782cb6a8c25f` — ggplot2 and ggprism: PRS distributions, population comparisons, and dashboards.
- `rr_7fcab1ffb8b5ac4f` — dplyr, jsonlite, and R.utils: Data manipulation, metadata parsing, and compressed-file handling.
- `rr_01467917df7b6274` — scripts/load_reference_data.R: Load reference or target genotype data.
- `rr_a6e9ff86f4219119` — scripts/load_pgs_weights.R: Search and download PGS Catalog weights.
- `rr_2344d68362022532` — scripts/score_traits.R: Perform allele matching and multi-trait score calculation.
- `rr_24af7177c03e868e` — scripts/generate_plots.R and scripts/export_results.R: Generate plots and export all result artifacts.
- `rr_abbf99b8d8602a79` — pdf-report-generation: Create the requested final PDF from completed artifacts.
- `rr_d6736d2e3c56e016` — Published polygenic score: Weighted combination of target-genotype alleles using a pre-computed PGS Catalog scoring file.
- `rr_4d34ff550bf09740` — Composite multi-trait risk score: Combined summary included with the wide multi-trait output.

## Validation / QC

- Export a variant-matching report for every scored trait.
- Treat a match rate below 50% as a likely genome-build mismatch and reconcile builds before interpreting the score.
- Require the documented stage-completion checks for genotype/weight loading, trait scoring, plotting, and export.
- Evidence requirement: Use published, peer-reviewed PGS weights rather than inventing weights.
- Evidence requirement: Record the PGS identifiers and genome build used for each score.
- Evidence requirement: Retain match reports, per-trait score metadata, and the reusable analysis object so downstream results remain auditable.

## Failure handling

- The core bigsnpr dependency is unavailable.
- A scoring-file download times out.
- Variant matching falls below 50% because score weights and genotypes use different genome builds.
- A PGS identifier is invalid or deprecated.
- A large genome-wide score exhausts available memory.
- Fallback rule: Increase the download timeout to 900 seconds for large or slow scoring-file downloads.
- Fallback rule: Use PGS Catalog search to replace a wrong or deprecated score identifier.
- Fallback rule: Align PGS weights and genotypes to the same genome build when match rate is low.
- Fallback rule: Allow the packaged plotter to fall back automatically when optional SVG dependencies are unavailable.
- Fallback rule: Use the packaged report fallback and disclose it when pdf-report-generation is unavailable.

## Limitations

- This workflow depends on pre-computed PGS Catalog weights and does not derive a new score from GWAS summary statistics.
- The documented quick workflow performs no LD computation.
- Score validity is compromised when genotype and scoring-file genome builds do not match.
- Genome-wide score files may require at least 8 GB of memory.

## Important domain-specific rules

- Trait-to-PGS discovery followed by explicit score-ID selection.
- Allele-matched multi-trait PRS scoring with per-trait variant-match reports.
- Wide multi-trait score matrix, correlation analysis, population summaries, and reusable analysis object.

## Portability boundary

- Low-freedom packaged-script orchestration and Biomni verification-message contract. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation routing and packaged report fallback. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
