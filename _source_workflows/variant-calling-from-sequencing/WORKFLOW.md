# Variant Calling From Sequencing

Source workflow: `variant-calling-from-sequencing`  
Parent Claude Science skill: `variant-analysis-and-interpretation`

## Purpose

Produce a quality-controlled germline SNV and indel call set from sequencing reads, alignments, or existing VCFs, with dual-caller concordance and optional truth-set accuracy benchmarking.

## When to use

- Align germline sequencing reads and call short variants.
- Compare normalized calls from two variant callers.
- Benchmark caller accuracy against a GIAB truth set when eligible truth data are supplied.
- Perform WES target-aware or WGS whole-genome coverage and call-set QC.

## Inputs

- Paired or single FASTQ reads. (optional)
- A coordinate-compatible BAM or CRAM with index. (optional)
- One or more bgzipped and indexed VCFs for benchmark-only analysis. (optional)
- A build-matched reference FASTA with index and sequence dictionary for read or alignment entry points. (required)
- The capture-kit target BED for WES. (optional)
- A GIAB truth VCF and confident-region BED for an eligible sample. (optional)
- Sample name, assay type, read type, and optional region subset. (required)

## Outputs

- Normalized, PASS-filtered, indexed per-caller VCFs.
- Concordance counts and Jaccard metrics overall and by variant type.
- Per-caller QC and WES on-target or WGS whole-genome depth and breadth metrics.
- Optional hap.py precision, recall, and F1 by caller and variant type.
- QC figures and a report containing an explicit accuracy-versus-concordance caveat.

## Workflow

1. Verify the reference build, optional known-site resources, target BED, truth data, and required command-line callers.
2. Route by entry point: align FASTQ, call from analysis-ready BAM or CRAM, or skip directly to normalized comparison for existing VCFs.
3. For reads, align, sort, mark duplicates, and apply BQSR only when compatible dbSNP and known-indel resources are supplied.
4. Call GATK HaplotypeCaller and choose the deep-learning caller by read type.
5. Keep PASS calls, split multiallelics, left-align, and compare normalized VCFs to quantify shared and caller-only variants.
6. When an eligible truth set is supplied, benchmark each caller within the intersection of target and confident regions.
7. Run target-aware WES or whole-genome WGS QC, annotate dbSNP identifiers when measuring known-rate, and generate figures and a report.

## Decision rules

- FASTQ input requires alignment; BAM or CRAM input begins at calling after sorted, indexed, and duplicate-marked checks; VCF-only input skips calling and BAM-dependent coverage QC.
- Use DeepVariant with an assay-matched WES or WGS model for short-read Illumina data; prefer PEPPER-DeepVariant or Clair3 for long reads.
- Skip BQSR and record the limitation when either build-matched dbSNP or known-indel resources are absent.
- Normalize multiallelic representation and left-align before caller comparison.
- Treat concordance as agreement rather than accuracy; only truth-set benchmarking measures correctness.
- For WES, prefer the capture kit BED and report on-target depth and breadth separately from whole-region values; for WGS, use whole-genome metrics.
- Run truth-set accuracy only for GIAB-characterized samples and within confident regions.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_5fe379d87f1fe774` — User-supplied build-matched reference and capture-kit BED: The sequencing build and WES kit resources are known.
- `rr_7b8035544398ffe1` — GIAB truth VCF and confident-region BED: The sample is GIAB-characterized and accuracy benchmarking is requested.

### Secondary resources

- `rr_52cb015931b77e58` — UCSC or Ensembl reference sequence: A documented local reference is not supplied.

### Fallback resources

- `rr_81006b67f00e89f4` — GENCODE exon target BED: No capture-kit BED is available for WES; disclose that it is broader than the baited core.

### Optional resources

- `rr_75fc8abaa608f880` — Genome in a Bottle: Truth VCFs and confident regions for eligible benchmark samples.
- `rr_e6e64f972db51e12` — dbSNP: Known-site support and known-variant-rate annotation when build matched.
- `rr_5c2646c0f638d9f2` — UCSC Genome Browser: Reference and optional GENCODE-exon target retrieval.
- `rr_fd17dba06befc3c1` — Ensembl: Reference sequence source when no local reference is supplied.
- `rr_c105f48f7d88aebc` — BWA-MEM: Short-read alignment.
- `rr_73e865b570406ed9` — minimap2: Long-read alignment.
- `rr_9449b1c712d7c612` — GATK: Duplicate marking, BQSR, HaplotypeCaller, genotyping, and filtering.
- `rr_9c462be9b53affbb` — bcftools: PASS filtering, normalization, intersection, annotation, and region splitting.
- `rr_ebbb28aab2865e76` — mosdepth: Target-aware and whole-genome depth and breadth calculation.
- `rr_e2f1dccf476aba11` — hap.py: Truth-set precision, recall, and F1 benchmarking.
- `rr_b8616d2ebf98a0cd` — samtools: Alignment sorting, indexing, depth calculation, and region extraction.
- `rr_3ffbb72a0a7e4f61` — bedtools: Target and confident-region interval operations.
- `rr_b974baebc57cb42a` — GATK HaplotypeCaller: Germline SNV and indel calling.
- `rr_55050f5535c8667b` — DeepVariant: Short-read deep-learning germline caller with assay-matched WES or WGS model.
- `rr_88fdd06ab8673726` — PEPPER-DeepVariant: Preferred long-read deep-learning caller when available.
- `rr_2ad47aa2f72d1ce6` — Clair3: Alternative long-read caller rather than a short-read default.

## Validation / QC

- Verify that reads or alignments, reference, dbSNP, target BED, truth set, and annotation resources use the same genome build and contig naming.
- Report WES breadth at 1×, 10×, 20×, 30×, 50×, and 100× and separate on-target from off-target call metrics.
- Use expected Ti/Tv ranges as a diagnostic rather than a universal pass criterion.
- Annotate calls against dbSNP before measuring known-rate; omit the metric when dbSNP is unavailable rather than reporting zero.
- Restrict truth benchmarking to the intersection of confident and target regions.
- Evidence requirement: Report shared and caller-only counts, Jaccard, and the fraction of each caller's calls that are shared.
- Evidence requirement: Never infer which caller is more accurate from concordance alone.
- Evidence requirement: Use hap.py precision, recall, and F1 only when a valid truth set is supplied.
- Evidence requirement: Document skipped BQSR, missing capture targets, absent dbSNP metrics, and unavailable BAM-dependent QC.

## Failure handling

- Genome builds or contig naming differ among reads, reference, targets, truth sets, and annotation resources.
- Indel representations are compared before normalization, producing spuriously low concordance.
- The DeepVariant model does not match assay or read type.
- Whole-region depth is reported as exome on-target depth.
- A non-GIAB sample or non-confident region is treated as truth-benchmarkable.
- Fallback rule: If known-site inputs are incomplete, skip BQSR and call from the duplicate-marked BAM with the limitation recorded.
- Fallback rule: If no capture BED is supplied for WES, use a GENCODE-exon target and warn that it is broader than the kit's baited core.
- Fallback rule: If long-read HPC calling is unavailable, use a local caller installation only with the deviation disclosed.
- Fallback rule: If truth data are absent, omit accuracy benchmarking and report only concordance and QC.
- Fallback rule: If dbSNP is absent, omit known-rate rather than substituting zero.

## Limitations

- The workflow excludes clinical or ACMG interpretation, ClinVar or gnomAD annotation, somatic calling, CNV or SV calling, and large-cohort joint genotyping.
- Caller concordance cannot establish correctness.
- GENCODE exon targets are broader than actual capture-kit bait regions and can produce misleading bimodal coverage.
- Truth-set accuracy is meaningful only for GIAB-characterized samples within confident regions.

## Important domain-specific rules

- Route cleanly among reads, alignments, and existing-call entry points.
- Normalize variant representation before any caller concordance calculation.
- Keep caller agreement, truth-set accuracy, and target-aware QC as separate evidence axes.
- Make genome-build, assay-model, and interval compatibility explicit gates.
- Report missing resources and skipped metrics transparently rather than substituting misleading values.

## Portability boundary

- Biomni HPC helper functions, installed-resource catalog, and named caller wrappers. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and pdf-report-generation integrations. — migration action: `exclude_or_capability_map`
- TodoWrite orchestration and skill-specific step tracking. — migration action: `exclude_or_capability_map`
- Biomni /workspace, /mnt/results, S3-FUSE, udocker, PRoot, and installed-path assumptions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
