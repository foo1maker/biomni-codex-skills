# Chip Atlas Peak Enrichment

Source workflow: `chip-atlas-peak-enrichment`  
Parent Claude Science skill: `chromatin-and-epigenomic-analysis`

## Purpose

Identify public ChIP-seq experiments and aggregated factors whose peaks are enriched near an input gene set using the official ChIP-Atlas enrichment analysis, with significance, fold enrichment, overlap metrics, plots, and interpretation safeguards.

## When to use

- Identify transcription factors or chromatin regulators enriched near a gene set.
- Evaluate public ChIP-seq evidence for regulator-to-gene-set relationships, optionally within a selected cell class.

## Inputs

- Gene-symbol list with at least three genes; five to one hundred genes are recommended. (required)
- Genome assembly; hg38 is the documented default and the source lists human, mouse, rat, fly, worm, and yeast alternatives. (optional)
- Antigen class, with TFs and others as the documented default. (optional)
- Cell class, with all cell types as the documented default. (optional)
- Peak-calling stringency threshold; the documented default is 50 and higher values are more stringent. (optional)
- Upstream and downstream TSS windows; the documented default is 5,000 bp in each direction. (optional)

## Outputs

- A serialized analysis object containing enrichment results, input genes and regions, metadata, and parameters.
- CSV tables for all experiments, q-value-significant experiments, and the top 20 experiments by significance with at least two gene overlaps.
- A four-panel PNG and SVG summary figure covering top factors, p-value distribution, overlap versus fold enrichment, and a volcano plot.
- A human-readable summary report with experiment-level and aggregated-factor results.

## Workflow

1. Load and validate the gene list, then resolve genome, antigen class, cell class, peak threshold, and TSS window.
2. Submit the regions derived from the gene list to the ChIP-Atlas enrichment workflow and retain experiment-level significance, enrichment, and overlap results.
3. Compare gene-to-region counts from the independent Ensembl coordinate lookup with the number analyzed by the ChIP-Atlas API.
4. Generate the documented four-panel figure and export all, significant, top, and summary artifacts.

## Decision rules

- Use BH-corrected q-value as the primary significance measure and fold enrichment as effect-size context.
- Explain the selected peak threshold and acknowledge that a higher threshold uses fewer, more confident peaks.
- Distinguish public experiments from unique factors; multiple experiments for one factor are datasets, not independent regulators.
- Use Median FE (Sig), not Median FE (All), when describing a factor's enrichment strength.
- For gene sets of ten or fewer, lead with exploratory framing and tie every moderate-enrichment interpretation to the sensitivity to one gene's inclusion or exclusion.
- Treat biological context from known factor biology as prior knowledge, not as a result of the enrichment analysis.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_7d9ebdaeb9f39325` — Official ChIP-Atlas Enrichment Analysis API and public ChIP-seq experiments.: Testing factor-peak enrichment near an input gene set.

### Secondary resources

- `rr_1fedb298cdab04e9` — Ensembl coordinate lookup.: Independently verifying gene-to-region mapping counts.

### Fallback resources

- `rr_1c0af7477dcd8ba0` — ChIP-Atlas enrichment results without independent Ensembl coordinate verification.: Ensembl maps no genes or fewer genes than the API; report both counts and explain the database-coverage difference.

### Optional resources

- `rr_b4baa70e28fd2a99` — ChIP-Atlas: Public ChIP-seq experiments and enrichment computation.
- `rr_fd17dba06befc3c1` — Ensembl: Independent gene-coordinate verification, separate from the API's mapping system.
- `rr_e8b591bb2a3ba240` — pandas, requests, NumPy, plotnine, and plotnine-prism: Request handling, data processing, and visualization dependencies listed by the skill.

## Validation / QC

- Report exact gene overlap rates from output rather than rounding or generalizing them.
- Report the total number of significant factors and discuss or acknowledge every factor in the aggregated top-factor table.
- If a factor has more than 20 experiments but few significant experiments, state that most tested contexts did not show enrichment and note the data-availability bias.
- State that aggregated-factor best q-values are corrected across experiments, not across the set of factors.
- Evidence requirement: Use generated experiment counts, significant-experiment counts, q-values, fold enrichment, and overlap values rather than biological reputation when describing top factors.
- Evidence requirement: Report any discrepancy between submitted genes, API-analyzed regions, and Ensembl-mapped genes without speculating which unreported gene the API dropped.
- Evidence requirement: Cite ChIP-Atlas publications and validate key factors with orthogonal expression, perturbation, or motif evidence.

## Failure handling

- Gene-coordinate lookup can map fewer genes than the ChIP-Atlas API because Ensembl and the API use different gene databases or a lookup can fail.
- Well-studied factors and common cell types can dominate results because they have more available experiments.
- Very small gene sets can produce unstable overlap and moderate-enrichment results because each gene has high leverage.
- Fallback rule: If independent Ensembl verification fails, retain the valid API enrichment results, disclose the missing verification, and report the API region count.
- Fallback rule: Use a more stringent threshold rerun as the documented sensitivity check for top-factor robustness.

## Limitations

- The workflow requires internet access and is biased toward well-studied factors and common cell types.
- Multiple experiments for one factor are not independent regulator discoveries.
- Results depend on the selected peak-calling threshold and may change at another stringency.

## Important domain-specific rules

- Separate experiment-level evidence from aggregated-factor reporting and disclose study-availability bias.
- Compare independent coordinate-mapping counts while preserving the primary analysis when the verifier differs.
- Use explicit small-input exploratory framing and a more-stringent-threshold sensitivity rerun.

## Portability boundary

- Package-local load, analysis, plotting, and export scripts together with prescribed agent verification messages and no-inline-code orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
