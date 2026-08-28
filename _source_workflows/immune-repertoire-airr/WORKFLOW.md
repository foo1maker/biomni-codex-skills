# Immune Repertoire Airr

Source workflow: `immune-repertoire-airr`  
Parent Claude Science skill: `immune-repertoire-and-cytometry-analysis`

## Purpose

Perform descriptive TCR or BCR repertoire profiling across clonality, diversity, V/J gene usage, and pairwise overlap, with interpretation adapted to single-cell versus bulk data.

## When to use

- Per-sample immune-repertoire clonality and diversity profiling
- V/J gene-segment usage and cross-sample similarity analysis
- Pairwise clonotype-overlap analysis and optional exploratory group comparison

## Inputs

- One called-repertoire file or Cell Ranger output folder per sample in a format supported by immunarch repLoad (required)
- Optional tab-separated metadata whose first column is Sample (optional)
- Optional receptor, chain, modality, group columns, and species overrides (optional)

## Outputs

- Sample-summary, diversity, clonality, gene-usage, overlap, and optional Wilcoxon-test tables
- Clonality, diversity, rarefaction, gene-usage, and overlap figures in raster and editable vector formats
- Machine-readable receptor, chain, modality, and metric summaries
- A modality- and receptor-aware PDF report

## Workflow

1. Stage one called-repertoire input per sample, add metadata when group comparisons are wanted, and confirm the source format.
2. Load the repertoire files and determine receptor, chain, and modality, allowing explicit overrides.
3. Compute clonality, diversity, rarefaction, V/J usage, and pairwise overlap metrics.
4. Interpret single-cell abundance as cells and bulk abundance as templates or reads, applying the corresponding diversity branch.
5. Flag unusually high pairwise overlap as a sample-provenance consistency signal requiring review.
6. When a two-level group exists, run two-sided Wilcoxon rank-sum tests and label small groups exploratory.

## Decision rules

- For single-cell repertoire data, do not use Chao1 as a richness estimate; rank samples with evenness indices and rarefaction.
- For bulk repertoire data, interpret abundance as templates or reads and depth-normalize before comparing richness.
- For BCR data, do not equate exact-CDR3 matches with clonal lineages because somatic hypermutation is not modeled.
- Label group comparisons exploratory when the smaller group has fewer than four samples.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_38c7c8808e9c4c89` — immunarch 0.9.1: Running the stated in-memory AIRR repertoire workflow

### Secondary resources

- `rr_dc244376442e4837` — McPAS-TCR or VDJdb: Optional antigen-specificity annotation is requested and the selected database is available

### Fallback resources

- `rr_4c0299f9d7d516a5` — Bounded 10x Genomics demonstration V(D)J samples: The user has no data and explicitly accepts a workflow demonstration that is clearly separated from user biology

### Optional resources

- `rr_cc032b7896ea1102` — McPAS-TCR: Optional antigen-specificity annotation for public or expanded TCR sequences
- `rr_5d10ec2caf6d5ece` — VDJdb: Optional antigen-specificity annotation for public or expanded TCR sequences
- `rr_74cd37e8975fad79` — immunarch 0.9.1: Repertoire loading and core clonality, diversity, gene-usage, and overlap analysis

## Validation / QC

- Inspect sample depth and use rarefaction before comparing diversity across samples.
- Review flagged high-overlap pairs against donor, replicate, and contamination provenance before any biological interpretation.
- Make receptor, chain, modality, and group definitions explicit and allow knowledgeable overrides of automatic detection.
- Evidence requirement: Treat the default workflow as descriptive or exploratory unless there is an adequately powered biological contrast.
- Evidence requirement: Do not fabricate antigen calls when McPAS-TCR or VDJdb is unavailable or unresolved.

## Failure handling

- Single-cell singleton dominance inflates Chao1 and makes it unsuitable as a richness estimate.
- Sequencing depth can create spurious diversity differences across samples.
- High pairwise overlap can reflect shared source or contamination rather than shared antigen biology.
- Fallback rule: If antigen databases are unavailable, skip antigen annotation and report the omission without creating calls.
- Fallback rule: If no metadata are supplied, run descriptively and optionally derive simple grouping from sample-name tokens.

## Limitations

- The workflow assumes clonotypes have already been called and does not assemble raw FASTQ data.
- The workflow does not reconstruct B-cell somatic-hypermutation lineages or model isotype and class-switch structure.
- The workflow does not integrate paired single-cell gene-expression or phenotype data.

## Important domain-specific rules

- Modality-aware branch that separates cell abundance from template or read abundance.
- Evenness and rarefaction emphasis for single-cell data with an explicit Chao1 prohibition.
- Pairwise-overlap outlier flag as a sample-provenance consistency check.
- Sample-size-aware exploratory labeling of optional group tests.

## Portability boundary

- ManageMachine and hpc_search_tools compute orchestration — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, GenerateImage, and PDF-report-generation capability calls — migration action: `exclude_or_capability_map`
- Platform media-output-check invocation — migration action: `exclude_or_capability_map`
- Hard-coded /workspace and /mnt paths, S3 FUSE workarounds, and mandatory bundled-script entry points — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
