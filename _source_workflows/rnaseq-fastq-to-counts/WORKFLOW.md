# Rnaseq Fastq To Counts

Source workflow: `rnaseq-fastq-to-counts`  
Parent Claude Science skill: `bulk-rna-and-omics-analysis`

## Purpose

Convert raw bulk RNA-seq reads into a differential-expression-ready integer gene-by-sample count matrix with QC, strandedness inference, gene metadata, and a load check, without running the differential-expression contrast.

## When to use

- Validate bulk RNA-seq FASTQ files, align or quantify reads with STAR or Salmon, infer strandedness, construct gene counts, and prepare a DESeq2 or edgeR hand-off.

## Inputs

- Paired-end or single-end compressed FASTQ files, or a public accession that resolves to FASTQ. (required)
- For STAR, matching genome FASTA and GTF; for Salmon, transcript FASTA, optional genome decoy, and transcript-to-gene map. (required)
- Organism, genome build, reference release, optional chromosome subset, and optional read-subset size. (optional)

## Outputs

- Integer gene-count matrix, gene metadata, strandedness inference, assignment summary, and STAR or Salmon quantification outputs.
- FastQC results, QC and mapping figures, a report, and a README containing the downstream hand-off.

## Workflow

1. Resolve input files or accessions, detect read layout and length, validate compressed FASTQ integrity, and confirm organism, build, and reference release.
2. Verify that FASTA and annotation sequence names and builds match, build the selected index, and run raw-read quality control.
3. Align and count with STAR or quantify with Salmon and aggregate transcripts to genes, without mixing count types between engines.
4. Infer strandedness, construct the gene matrix and metadata, verify DE-readiness, and export QC summaries and the downstream hand-off.

## Decision rules

- Confirm organism, genome build, and annotation release before acquiring references, and require exact FASTA/GTF sequence-name and build compatibility.
- Set STAR sjdbOverhang to read length minus one.
- Call a library forward-stranded when the forward fraction is at least 0.8 and reverse-stranded when it is at most 0.2.
- Do not combine STAR gene counts with Salmon transcript-derived estimates in one matrix, and require biological replicates for downstream differential expression.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_c8580edbbaad58ad` — Local self-contained execution: Use for validation, demonstrations, and small samples.

### Secondary resources

- `rr_97f05eb5fe3ea4d1` — HPC production execution: Use for full genomes, real analyses, or many samples.

### Fallback resources

- `rr_58eb0338f4335dfe` — Chromosome or read-subset validation mode: Use to keep demonstration or continuous-integration runtime small; do not interpret subset counts biologically.

### Optional resources

- `rr_6271ff79c623d4f4` — ENA: Resolve accessions and retrieve FASTQ metadata.
- `rr_ec2c9dff0cb1a776` — GEO: Map GEO identifiers to sequencing runs.
- `rr_c9b25374cd581dd2` — Ensembl and GENCODE: Genome FASTA and matching annotation references.
- `rr_8a1eeef9e6869b22` — STAR: Splice-aware alignment and gene counting.
- `rr_8503e4978420b0fa` — Salmon and tximport: Transcript quantification and gene-level aggregation.
- `rr_fd76f4313668ac71` — FastQC, samtools, seqkit, and Trimmomatic: Input, alignment, and optional adapter-trimming quality control.
- `rr_8a7a79093107ed07` — DESeq2: Integer-count load check and downstream hand-off.

## Validation / QC

- Validate gzip integrity, read layout and length, FASTA/GTF compatibility, raw-read QC, alignment integrity, and mapping summaries.
- The DE-readiness check must confirm integer, non-negative counts with no missing values or duplicate gene identifiers.
- Evidence requirement: Report the inferred library type, exact reference build and release, count-generation engine, and downstream hand-off.
- Evidence requirement: Do not assign biological meaning to subset-mode counts and require at least two replicates per group for downstream differential expression.

## Failure handling

- Reference-build or sequence-name mismatch produces zero or unusable counts.
- Strandedness inference is unreliable when too few reads are assigned.
- A single-sample matrix passes a load check but cannot support a valid differential-expression contrast.
- Fallback rule: Use local subset mode for validation and demonstration, while clearly excluding it from biological interpretation.
- Fallback rule: Use Salmon as the optional faster quantification path and trim adapters only when QC shows clearly problematic adapter content.

## Limitations

- The workflow does not perform the differential-expression contrast and is not intended for single-cell RNA-seq, de-novo assembly, or fusion calling.
- STAR counts and Salmon estimates represent different quantities and must not be mixed.

## Important domain-specific rules

- FASTQ integrity and layout validation, reference compatibility checks, STAR or Salmon count generation, strandedness inference, gene metadata assembly, and DE-readiness validation.

## Portability boundary

- Biomni HPC search and execution tool IDs, LiteratureSearch and image/report orchestration, internal database and result paths, Phylo branding, and downstream skill chain. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
