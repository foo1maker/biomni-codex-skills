# Grn Pyscenic

Source workflow: `grn-pyscenic`  
Parent Claude Science skill: `regulatory-network-analysis`

## Purpose

Infer transcription-factor regulons de novo from single-cell RNA-seq expression and calculate cell-level regulon activity with the pySCENIC pipeline.

## When to use

- Infer transcription-factor target relationships from single-cell expression.
- Score regulon activity per cell.
- Compare regulatory programs across cell types, states, conditions, tissues, or species.

## Inputs

- A single-cell expression matrix as AnnData, Loom, or genes-by-cells CSV or TSV. (required)
- A species-matched cisTarget motif-ranking Feather database. (required)
- A species-matched motif-annotation table and transcription-factor list. (required)
- At least 500 cells and 2,000 expressed genes; 1,000 or more cells is recommended. (required)

## Outputs

- Raw TF-target adjacencies and motif-pruned regulons.
- Cell-by-regulon AUCell activity matrix and regulon summaries.
- Integrated AnnData containing regulon activities.
- Regulon heatmap, network visualization, and analysis summary.

## Workflow

1. Load and quality-filter the single-cell expression data.
2. Infer TF-target co-expression modules with GRNBoost2.
3. Prune co-expression targets with cisTarget motif enrichment to form regulons.
4. Score regulon activity in each cell with AUCell.
5. Integrate activity scores into AnnData, visualize major regulons, and export machine-readable results.

## Decision rules

- Do not use de novo pySCENIC inference for bulk RNA-seq with few samples, fewer than 500 cells, or severely constrained compute.
- Select hg38 resources for human, mm10 for mouse, and verify database availability before analyzing another species.
- For datasets larger than 10,000 cells, consider subsampling while preserving the declared analysis scope.
- Require TF-list nomenclature to match expression gene identifiers.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_e0666be472094899` — Species-matched SCENIC motif rankings, motif annotations, and TF list: Running de novo regulon inference for the supplied species and genome build.

### Secondary resources

- `rr_f308f5ab9c9836fd` — PBMC 3k example data: Testing or demonstrating the workflow before user-data analysis.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_2d9711ce444e4388` — SCENIC resources: Species-specific cisTarget motif rankings, motif annotations, and TF lists.
- `rr_0f08ddbd84f60fca` — pyscenic: End-to-end regulon inference framework.
- `rr_0d27dcea47005c7f` — arboreto: GRNBoost2 implementation dependency.
- `rr_c3853c2b9473c34b` — ctxcore: cisTarget database and motif-pruning dependency.
- `rr_063ec900fce6482b` — scanpy: Single-cell AnnData processing and integration.
- `rr_6390f795a84f033d` — NetworkX: Regulon network visualization.
- `rr_de2624584033cfed` — GRNBoost2: Tree-based TF-target co-expression inference.
- `rr_1465f1f261b9f6a0` — cisTarget: Motif-enrichment pruning of candidate TF targets.
- `rr_bd720917d97d0bfd` — AUCell: Cell-level regulon activity scoring.

## Validation / QC

- Apply basic low-quality cell and gene filtering before SCENIC.
- Require at least 500 cells, 2,000 expressed genes, and species-matched reference files.
- Check TF naming, regulon size, input normalization, and Feather database integrity when regulons or AUCell scores are poor.
- Verify TF-target, regulon, and AUCell stages complete before integration and export.
- Evidence requirement: Retain raw adjacencies, motif-pruned regulons, cell-level AUCell scores, and integrated data.
- Evidence requirement: Validate key regulons against literature, ChIP-seq, or perturbation experiments.

## Failure handling

- GRN inference exceeds available memory or runtime.
- TF names do not match expression gene symbols and no regulons are recovered.
- The cisTarget database is missing, corrupted, or uses the wrong Feather format.
- AUCell scores are uniformly low because regulons are weak or input normalization is unsuitable.
- Fallback rule: For memory pressure, subsample to 5,000–10,000 cells or filter to 2,000–5,000 variable genes.
- Fallback rule: Re-download and verify Feather v2 reference databases when cisTarget loading fails.
- Fallback rule: Increase worker count or filter variable genes when inference is excessively slow.
- Fallback rule: If dedicated report generation is unavailable, disclose use of the packaged markdown or script fallback.

## Limitations

- Fewer than 500 cells are insufficient for robust de novo network inference.
- The workflow is not intended for bulk RNA-seq with few samples or quick TF activity from differential-expression results.
- The workflow requires substantial memory and may take several hours.
- Inference depends on species- and genome-build-specific motif databases and TF lists.

## Important domain-specific rules

- Use a three-stage GRNBoost2, cisTarget, and AUCell chain to separate co-expression inference, motif support, and cell-level activity.
- Gate de novo GRN inference on cell count, gene count, prior QC, and reference-database compatibility.
- Preserve adjacencies, regulons, activity matrices, and integrated data as separate evidence products.
- Treat inferred regulons as hypotheses requiring orthogonal literature, binding, or perturbation validation.

## Portability boundary

- Mandatory skill-local Python scripts and exact success-message checks. — migration action: `exclude_or_capability_map`
- Biomni example loaders and fixed output-directory conventions. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation routing and packaged report fallback. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
