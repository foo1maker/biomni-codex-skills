# Consensus Disease Signature

Source workflow: `consensus-disease-signature`  
Parent Claude Science skill: `biomarker-and-signature-analysis`

## Purpose

Derive a direction-consistent consensus transcriptional signature across two or more independent human bulk-transcriptome cohorts using random-effects effect-size meta-analysis.

## When to use

- Meta-analyze disease-versus-control differential expression across independent cohorts and platforms.
- Define consensus and high-confidence core up- and down-regulated gene sets.
- Characterize the consensus signature with pathway enrichment and sensitivity analysis.

## Inputs

- A YAML configuration defining the disease, output directory, two-group contrast, FDR threshold, and core log-fold-change threshold. (required)
- At least two cohorts, each specifying identifier, source, platform, data type, group mapping, and optional tissue or timepoint filters. (required)
- A control_type label for each cohort when comparator groups differ. (required)

## Outputs

- Full per-gene random-effects meta-analysis with pooled estimate, uncertainty, heterogeneity, cohort effects, direction, consensus, and core flags.
- Consensus up- and down-regulated gene tables and a machine-readable summary.
- A non-inflammatory-control sensitivity meta-analysis when at least two eligible cohorts exist.
- GO Biological Process, Reactome, and Hallmark enrichment results.
- Cross-cohort quality-control, concordance, forest, heatmap, enrichment, and sensitivity figures.

## Workflow

1. Ingest each independent cohort from GEO or a user-supplied expression matrix and metadata.
2. Filter each cohort to the requested tissue, timepoint, and two contrast groups, recording the comparator type.
3. Inspect sample distributions and detect near-duplicate cohorts by cross-cohort mean-expression correlation.
4. Map platform features to gene symbols and collapse multiple probes to the highest-mean probe.
5. Estimate per-cohort log2 fold change and standard error with limma or limma-voom according to data type.
6. Pool each gene across at least two cohorts with a random-effects effect-size meta-analysis and control FDR.
7. Define consensus genes by FDR and sign agreement, then define the core set with an additional effect-size threshold.
8. When controls are heterogeneous, run the documented non-inflammatory-control sensitivity analysis when possible.
9. Run direction-separated GO and Reactome over-representation and Hallmark GSEA using the tested genes as background.

## Decision rules

- Require at least two independent human bulk-transcriptome cohorts and a two-group contrast.
- Pool effect sizes and standard errors rather than combining p-values or vote-counting.
- Define consensus as FDR below the configured threshold with the same effect direction in every contributing cohort.
- Define core genes as consensus genes whose absolute pooled log2 fold change meets the configured floor.
- Keep FDR-significant and direction-consistent consensus counts distinct, with consensus never exceeding FDR-significant counts.
- Use the full meta-tested gene universe as the background for over-representation analysis.
- Load only Hallmark or small MSigDB collections to avoid memory exhaustion.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_4d1d3f0f0617cd62` — Independent human bulk-transcriptome cohorts from GEO: Cohorts are publicly deposited and can be retrieved automatically.
- `rr_4ab74038ee73a6c7` — User-supplied expression matrices and metadata: The cohort is proprietary or supplied as a flat matrix.

### Secondary resources

- `rr_602e7173acc05456` — Gene Ontology Biological Process and Reactome pathways: Direction-separated over-representation analysis is required.
- `rr_be408b918e6cf8f4` — MSigDB Hallmark collection: Rank-based gene-set enrichment is required.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_ec2c9dff0cb1a776` — GEO: Source of public bulk-transcriptome cohorts.
- `rr_5848a81c5e8e9bf9` — GO: Biological Process over-representation analysis.
- `rr_cf6342a3273e2fcd` — Reactome: Commercially usable pathway over-representation analysis replacing KEGG.
- `rr_0d8b71a2a66e1d47` — MSigDB Hallmark: Hallmark gene sets for GSEA, subject to the documented commercial-use review.
- `rr_7a50dd7d85c224fe` — limma: Per-cohort microarray differential expression.
- `rr_de49180956a9de1d` — edgeR: Filtering and TMM normalization before limma-voom for RNA-seq counts.
- `rr_5a49cf84729a19e3` — metafor: Random-effects gene-level effect-size meta-analysis.
- `rr_57b3ef434334f606` — GEOquery: GEO cohort retrieval.
- `rr_a487141e81e96829` — clusterProfiler: GO over-representation analysis.
- `rr_66807d29733768d5` — ReactomePA: Reactome pathway enrichment.
- `rr_ac92de44ccdb7ec2` — fgsea: Hallmark GSEA.
- `rr_2c63f1530faeb318` — random-effects effect-size meta-analysis: Pools per-cohort log2 fold changes and standard errors while allowing true effects to vary.
- `rr_bc9f626758dea69f` — DerSimonian-Laird fallback: Fallback estimator when the REML model cannot be fit.
- `rr_fb8aa0f717efb03a` — limma moderated t-test for microarray (log2 intensities) or limma-voom for rnaseq: Produce per-cohort effect sizes and precision for microarray and RNA-seq cohorts.

## Validation / QC

- Flag cohort pairs with cross-cohort mean-expression correlation above 0.999 as likely duplicate deposits and remove duplicates.
- Record every cohort's data type and control type so methods and heterogeneous baselines are described correctly.
- Assert that the consensus count does not exceed the total FDR-significant count.
- Interpret the non-inflammatory-control sensitivity result whenever comparator types differ.
- Evidence requirement: Retain each cohort's log2 fold change and standard error as the effect and precision inputs to meta-analysis.
- Evidence requirement: Report I-squared heterogeneity and the number of contributing cohorts for each pooled gene.
- Evidence requirement: Literature validation must report only named markers and citations recovered from records; missing evidence must not be invented.

## Failure handling

- A re-deposited cohort is counted twice and creates false replication.
- Different tissues, visits, treatments, or controls are silently mixed into one contrast.
- Loading the full MSigDB C2 collection exhausts memory.
- Naive concatenation of cohort matrices confounds cross-cohort batch structure.
- Fallback rule: Use the DerSimonian-Laird estimator when REML cannot be fit for a gene.
- Fallback rule: If fewer than two non-inflammatory-control cohorts exist, flag heterogeneous controls and skip the subset meta-analysis with a reason.
- Fallback rule: If literature records yield few named markers, report that limitation rather than fabricating markers or citations.

## Limitations

- The default implementation is human and requires manual annotation-package changes for other organisms.
- Only a two-group contrast is modeled; multi-level and interaction designs are out of scope.
- The workflow does not combine p-values, vote-count studies, or provide a single-study mode.
- Pooling non-equivalent control groups yields an average over different baselines and requires explicit sensitivity interpretation.

## Important domain-specific rules

- Cross-cohort duplicate detection before evidence pooling.
- Platform-specific differential expression reduced to a common effect-size and standard-error contract.
- Direction-consistent FDR consensus and effect-size-defined core signatures.
- Comparator-type sensitivity analysis for heterogeneous controls.
- Direction-separated enrichment with the tested-feature universe as background.

## Portability boundary

- Biomni LiteratureSearch validation and execution-trace reference collection. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage, Read media checks, pdf-report-generation, and internal report assembly paths. — migration action: `exclude_or_capability_map`
- Packaged script entry points, persistent library paths, machine-management instructions, and /mnt/results runtime paths. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
