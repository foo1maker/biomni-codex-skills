# Flow Cytometry Analysis

Source workflow: `flow-cytometry-analysis`  
Parent Claude Science skill: `immune-repertoire-and-cytometry-analysis`

## Purpose

Process flow-cytometry or mass-cytometry data through modality-aware quality control, clustering, cell-population annotation, quantification, optional manual-gate benchmarking, and conditional differential-abundance analysis.

## When to use

- Quality control, compensation, transformation, and pre-gating of cytometry measurements
- Unsupervised cell clustering and marker-based population annotation
- Conditional manual-gate benchmarking and differential-abundance analysis

## Inputs

- FCS files from flow cytometry or CyTOF (required)
- Sample metadata table (required)
- Optional compensation controls, FMO controls, external spillover matrix, thresholds, or OpenCyto gating template (optional)
- Optional manual-gating export or labels for independent validation (optional)

## Outputs

- Annotated SingleCellExperiment object
- Quality-control logs and reviewable threshold templates
- Cell-abundance, benchmark, differential-abundance, and manual-validation tables when applicable
- Quality-control and analysis figures plus a PDF report

## Workflow

1. Load FCS files, identify the acquisition modality, apply appropriate compensation and transformation, and run quality control.
2. Apply built-in pre-gating by default, or an OpenCyto template when explicitly provided.
3. Cluster the post-QC cells with the automated clustering workflow.
4. Assign marker-based labels and refine annotations in two tiers.
5. Quantify cell populations and calculate dimensionality-reduction embeddings.
6. When a manual-gating export exists, reconcile identifiers and validate automated abundances against it.
7. Run differential abundance only when the sample design satisfies the stated replication requirements.

## Decision rules

- Use the built-in gating backend by default and OpenCyto only when the opt-in template path is selected.
- If compensation is singular, skip compensation and mark the data as uncompensated; if it is ill-conditioned, retain a warning.
- Run differential abundance only with at least three samples per group and at least two groups; otherwise report descriptive abundance only.
- When a manual-gating export exists, treat validation against it as mandatory.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_f35a5862a90a7020` — CATALYST and FlowSOM: Running the default cytometry transformation, clustering, and annotation workflow

### Secondary resources

- `rr_9b2318d8ec3c6114` — OpenCyto: A compatible gating template is explicitly provided

### Fallback resources

- `rr_e69b4c4e7997039a` — Descriptive abundance summaries: The cohort does not meet the strict differential-abundance replication threshold

### Optional resources

- `rr_937281a8af390f92` — CellMarker2: Optional marker knowledge for cell-population annotation
- `rr_2b63a0c8e4822081` — HDCytoData: Optional cytometry reference or example data resource
- `rr_c51276ee0b0c551e` — CATALYST: Cytometry preprocessing, clustering support, and visualization
- `rr_8ce7a842d00b8edd` — FlowSOM: Self-organizing-map clustering
- `rr_3592e4771e39187d` — diffcyt: Conditional differential-abundance testing
- `rr_68ec190f171efc92` — flowCore: FCS input and flow-cytometry data structures

## Validation / QC

- Treat quality control as a first-class stage before clustering or annotation.
- Use reviewable, data-driven two-pass pre-gating cutoffs and flag removal of more than 30 percent of events as possible over-gating.
- Use all post-QC cells for analysis; balanced subsampling is limited to UMAP or t-SNE visualization.
- Prefer independent manual labels or statistics over self-consistency checks when judging annotation quality.
- Evidence requirement: Preserve compensation status, transformation choice, gating thresholds, and QC counts as auditable evidence.
- Evidence requirement: When manual-gating data exist, report identifier reconciliation and agreement metrics rather than relying only on automated labels.

## Failure handling

- A singular compensation matrix prevents valid compensation and leaves the dataset explicitly marked uncompensated.
- Aggressive pre-gating can remove a large fraction of events and distort downstream populations.
- Under-replicated cohorts cannot support the specified differential-abundance model.
- Fallback rule: If compensation is singular, skip it, label the dataset uncompensated, and continue only with that limitation visible.
- Fallback rule: If the design lacks sufficient replication for differential abundance, return descriptive abundance summaries instead.

## Limitations

- The workflow does not provide interactive manual gating.
- The workflow does not perform spectral unmixing.
- The workflow is not intended for single-cell RNA-seq or bulk RNA-seq data.

## Important domain-specific rules

- Diagnostics-first compensation, transformation, and pre-gating with explicit warnings.
- Two-tier marker-based annotation refinement after unsupervised clustering.
- Independent manual-gate reconciliation as a validation gate when such data exist.
- Replication-aware switch between inferential and descriptive abundance analysis.

## Portability boundary

- ManageMachine provisioning and platform compute orchestration — migration action: `exclude_or_capability_map`
- GenerateImage and Phylo-specific PDF-report generation — migration action: `exclude_or_capability_map`
- Platform media-output-check calls — migration action: `exclude_or_capability_map`
- Hard-coded /workspace and /mnt/results runtime paths plus mandatory bundled-script entry points — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
