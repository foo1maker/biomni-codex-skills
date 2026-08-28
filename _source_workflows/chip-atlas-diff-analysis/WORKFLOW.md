# Chip Atlas Diff Analysis

Source workflow: `chip-atlas-diff-analysis`  
Parent Claude Science skill: `chromatin-and-epigenomic-analysis`

## Purpose

Compare two groups of public experiments through ChIP-Atlas to identify differential peak regions or differentially methylated regions with effect sizes, significance statistics, QC warnings, plots, and tabular results.

## When to use

- Find differential peaks between two conditions or cell types using ChIP-seq, ATAC-seq, or DNase-seq experiments.
- Identify differentially methylated regions from two Bisulfite-seq experiment groups.

## Inputs

- Two groups of SRA SRX/ERX/DRX or GEO GSM experiment identifiers, with at least two identifiers per group and three or more recommended. (required)
- Genome assembly; hg38 is the documented default and the listed alternatives include hg19, mm10, mm9, rn6, dm6, dm3, ce11, ce10, and sacCer3. (optional)
- Analysis type: diffbind for differential peak regions or dmr for differentially methylated regions. (optional)
- Descriptive labels for the two experiment groups. (optional)

## Outputs

- A serialized analysis object containing filtered and unfiltered regions, QC warnings, experiment groups, raw files, logs, and parameters.
- CSV tables for all QC-filtered regions, FDR-significant regions, the top 50 regions, and pre-QC unfiltered regions.
- Original ChIP-Atlas BED, IGV session, and log files.
- PNG and SVG volcano, chromosome-distribution, region-size, and MA plots.
- A human-readable summary with QC warnings, optional nearest-gene annotations, and statistical caveats.

## Workflow

1. Load and validate the two experiment-ID groups and analysis parameters.
2. Submit the selected analysis to ChIP-Atlas and collect filtered regions, unfiltered regions, raw artifacts, logs, parameters, and QC warnings.
3. Generate the four documented visualizations with a PNG fallback for SVG export failure.
4. Export serialized, tabular, raw, and summary artifacts, preserving QC warnings and optional annotations.

## Decision rules

- Use diffbind for ChIP-seq, ATAC-seq, or DNase-seq differential peaks and dmr for Bisulfite-seq methylation differences.
- Treat q-value below 0.05 as significant and interpret positive log2 fold change as enriched in Group A and negative as enriched in Group B.
- Do not interpret overwhelmingly one-directional sex-chromosome regions with near-zero depleted-group counts as genuine differential binding.
- For ChIP-seq or ATAC-seq, treat significant mitochondrial regions as likely contamination or nonspecific signal; do not apply that rule to DMR mode.
- Claim MA-plot asymmetry only when the automated check flags a greater-than-one-log2-unit difference in median mean count between directions.
- Present top regions in strict q-value order; if only annotated regions are selected, label them as selected highlights rather than a strict ranking.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_e6b0d38c9ba93ae2` — ChIP-Atlas Diff Analysis API and its public experiment data.: Comparing two groups directly from experiment accessions without downloading raw sequencing data.

### Secondary resources

- `rr_065583dd356d27bd` — UCSC REST gene annotation.: Adding optional overlapping or nearby gene labels to top regions.

### Fallback resources

- `rr_505f1837691ee20e` — Unannotated statistical region results.: UCSC gene annotation is unavailable; statistical results remain complete.

### Optional resources

- `rr_b4baa70e28fd2a99` — ChIP-Atlas: Public experiment data and server-side differential analysis.
- `rr_37d1bdc2b1f75241` — UCSC: Optional overlapping or nearest-gene annotation through its REST API.
- `rr_de49180956a9de1d` — edgeR: Server-side statistical framework for differential peak regions.
- `rr_553f9602e9fcfdfd` — metilene: Server-side method for differentially methylated regions.
- `rr_e8b591bb2a3ba240` — pandas, requests, NumPy, plotnine, and plotnine-prism: Request handling, data processing, and visualization dependencies listed by the skill.

## Validation / QC

- Filter non-standard contigs and regions shorter than 10 bp by default while retaining an unfiltered export.
- Flag dense one-directional clusters of at least 10 significant regions within 2 Mb for possible copy-number or structural effects.
- Treat edgeR FDR values as approximate rankings at n at or below 3 and potentially unstable below n of 5; emphasize effect sizes and validation.
- Flag a median significant-region size below 100 bp in ChIP-seq or ATAC-seq as possible peak fragmentation.
- Evidence requirement: Copy coordinates, log fold changes, and q-values directly from the generated summary or CSV rather than re-deriving or recalling them.
- Evidence requirement: State prominently whether within-group experiments are true biological replicates; heterogeneity is treated as noise by the analysis.
- Evidence requirement: Describe nearest-gene results as proximity, not proof of functional regulation, and validate key regions orthogonally.

## Failure handling

- Invalid experiment identifiers or parameters can produce an API 400 response.
- Large jobs or server load can exceed the documented 15-minute timeout.
- The result archive can lack a BED file or contain no differential regions when the accessions are unavailable, incompatible with the genome, or the groups are too similar.
- Changes to the API text response can cause request-identifier parsing failure.
- Fallback rule: If UCSC annotation is unavailable, keep complete statistical results and omit gene labels.
- Fallback rule: If SVG export fails, retain the automatically generated PNG output.

## Limitations

- The workflow requires internet access and depends on the availability and quality of public ChIP-Atlas data.
- Experiments within each group are treated as biological replicates, so within-group design heterogeneity enters the analysis as noise.
- Nearest-gene proximity does not establish direct regulation or causation.

## Important domain-specific rules

- Pair filtered results with a preserved pre-QC export and explicit automated artifact warnings.
- Gate interpretive claims on quantitative QC thresholds instead of visual impressions.
- Preserve strict significance rankings, exact output values, and explicit replicate-design caveats.

## Portability boundary

- Package-local load, run, plot, and export script IDs together with the mandated no-inline-code agent orchestration. — migration action: `exclude_or_capability_map`
- Package-specific script-failure percentage hierarchy and verification-message protocol. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
