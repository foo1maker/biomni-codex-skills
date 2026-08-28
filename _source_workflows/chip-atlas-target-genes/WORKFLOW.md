# Chip Atlas Target Genes

Source workflow: `chip-atlas-target-genes`  
Parent Claude Science skill: `regulatory-network-analysis`

## Purpose

Retrieve and rank genes near peaks for a specified transcription factor from precomputed ChIP-Atlas public ChIP-seq matrices, with binding, coverage, interaction, cell-context, and co-location information.

## When to use

- Identify and rank candidate target genes for a specific transcription factor across public ChIP-seq experiments.
- Compare transcription-factor binding across cell types and cross-reference preembedded STRING interaction evidence.

## Inputs

- Case-sensitive transcription-factor or protein symbol. (required)
- Genome assembly; hg38 is the documented default and the source lists human, mouse, rat, fly, worm, and yeast alternatives. (optional)
- TSS distance of 1 kb, 5 kb, or 10 kb; 5 kb is the documented default. (optional)
- Optional minimum average binding score, top-N limit, cell-type subset, minimum STRING score, and minimum binding rate. (optional)

## Outputs

- A serialized analysis object containing target genes, per-experiment data, cell types, query protein, parameters, and metadata.
- CSV tables for all targets, the top 50 by average binding score, targets with STRING evidence, and wide-format experiment scores for the top 50.
- PNG and SVG plots for top targets, score distribution, target-by-experiment heatmap, and STRING-versus-binding comparison.
- A human-readable analysis summary.

## Workflow

1. Ask for and validate the case-sensitive transcription-factor or protein query before asking for secondary parameters.
2. Resolve genome, TSS distance, cell-type filter, and score threshold for a user-supplied query.
3. Download and parse the static precomputed ChIP-Atlas target-gene matrix, optionally subset experiment columns by cell type, recalculate averages, and apply requested filters.
4. Generate the documented plots and export the serialized object, all-target table, ranked subsets, experiment matrix, and summary.

## Decision rules

- Preserve query case because the protein name is case-sensitive.
- Rank genes from generated data files by average binding score; never substitute famous targets or construct rankings from background knowledge.
- Use max score and binding rate alongside average binding score because the average includes zero-valued experiments.
- Do not interpret a STRING score of zero as evidence that a gene is not a target.
- Flag co-located gene groups and consider collapsing them before pathway enrichment to avoid double-counting loci.
- Label gene-function or pathway descriptions as general knowledge rather than ChIP-Atlas output.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_b0b70827f4038a93` — Precomputed ChIP-Atlas target-gene TSV files.: Ranking candidate target genes for a non-histone antigen or transcription factor.

### Secondary resources

- `rr_c6b8e2594bbd69d3` — STRING interaction-score columns embedded in the ChIP-Atlas TSV.: Cross-referencing binding evidence with interaction evidence; no separate STRING API query is performed.

### Fallback resources

- `rr_e391bca8de6ad41c` — Binding-score results without STRING support.: The queried protein has no STRING data and all embedded interaction scores are zero.

### Optional resources

- `rr_b4baa70e28fd2a99` — ChIP-Atlas: Precomputed per-gene, per-experiment MACS2 binding-score matrices.
- `rr_5e0ba980ac54aeea` — STRING: Preembedded interaction-score columns used as an independent evidence layer without a live API query.
- `rr_91f1dbec0f86343a` — pandas, requests, NumPy, plotnine, plotnine_prism, matplotlib, and seaborn: Static-file retrieval, data processing, filtering, and visualization dependencies listed by the skill.

## Validation / QC

- Interpret average MACS2 binding scores as minus ten times log10 q-value; reported categories are at least 500 very strong, 100–500 strong, 50–100 moderate, and below 50 weak.
- Report binding rate as a percentage and numerator/denominator; above 50% is described as broadly consistent and below 10% as cell-type-specific.
- Use the generated co-located-group field to flag targets that share identical binding scores at the same locus.
- Evidence requirement: Read exact gene names, ranks, average scores, binding rates, and STRING scores from the generated summary or CSV.
- Evidence requirement: Report scores to one decimal place and percentages to one decimal place, matching the source reporting rule.
- Evidence requirement: If a well-known target is outside the requested top-N, state its actual rank and score rather than moving it into the list.

## Failure handling

- An invalid, unavailable, incorrectly cased, or histone-mark query can return HTTP 404.
- Large static matrices can time out or exceed memory because they contain hundreds of experiment columns.
- Strict score, cell-type, interaction, or binding-rate filters can remove all results.
- Fallback rule: If the matrix is too large, restrict top_n or use a cell-type filter to reduce the processed table width.
- Fallback rule: If filters produce no targets, lower min_score, remove the cell-type filter, or increase top_n.
- Fallback rule: If STRING data are all zero, retain and interpret the ChIP-Atlas binding results without treating missing interaction coverage as negative evidence.
- Fallback rule: If SVG export fails, retain the automatically generated PNG plots.

## Limitations

- The workflow requires internet access and supports non-histone antigens rather than histone-mark targets.
- Experiment coverage is uneven across cell types, so well-studied contexts can dominate averages.
- Binding proximity and scores do not by themselves establish functional regulation; biological role descriptions are external annotations.

## Important domain-specific rules

- Preserve data-derived rankings exactly and separate them from background biological interpretation.
- Triangulate average score with maximum score, binding rate, interaction coverage, and cell-type composition.
- Detect and disclose co-located genes before downstream locus-counting or pathway analysis.

## Portability boundary

- Package-local query loader, target-gene workflow, plotting, and export scripts together with the mandated no-inline-code agent orchestration. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation skill calls and package-specific markdown/HTML/script fallback for a requested PDF. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
