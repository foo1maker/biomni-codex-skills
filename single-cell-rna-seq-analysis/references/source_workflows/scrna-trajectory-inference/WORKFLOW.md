# Scrna Trajectory Inference

Source workflow: `scrna-trajectory-inference`  
Parent Claude Science skill: `single-cell-rna-seq-analysis`

## Purpose

Order preprocessed single-cell RNA-seq cells along differentiation or disease trajectories, identify branches and terminal fates, discover transition-associated genes, and optionally estimate RNA velocity and fate probabilities.

## When to use

- Infer pseudotime, branching structure, terminal fates, trajectory-associated genes, RNA velocity, and CellRank fate probabilities from preprocessed single-cell data.

## Inputs

- Preprocessed AnnData object with PCA, UMAP, and cluster annotations, at least 200 cells, at least 100 genes, and a cluster label in observation metadata. (required)
- Spliced and unspliced layers for RNA velocity. (optional)
- Root cell type, cluster key, and expected trajectory structure. (optional)

## Outputs

- Trajectory-annotated AnnData object and serialized trajectory results.
- Pseudotime, trajectory, velocity, fate, and driver-gene tables; PNG and SVG figures; analysis metadata; and a report or markdown fallback.

## Workflow

1. Load and validate the preprocessed AnnData object and confirm the root population, cluster key, and requested analysis scope.
2. Infer the core PAGA graph and diffusion pseudotime; add RNA velocity and CellRank fate mapping only when their inputs and packages are available.
3. Generate trajectory, pseudotime, velocity, fate, and driver-gene visualizations appropriate to the completed analysis scope.
4. Export the annotated object, tables, model results, figures, and analysis metadata.

## Decision rules

- Choose core-only, core plus RNA velocity, or full CellRank scope based on the user's request and the presence of spliced and unspliced layers.
- Confirm that the root cell type exactly matches the available annotation and inspect the PAGA graph before accepting terminal states.
- Use the Scanpy-only core when optional velocity or fate packages are unavailable.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_28575fec5bcfb4ab` — Preprocessed AnnData with PCA, UMAP, clusters, and annotations: Required for the core trajectory analysis.

### Secondary resources

- `rr_9e73d9505ac23280` — Spliced and unspliced count layers: Use for RNA velocity and fate mapping.

### Fallback resources

- `rr_08cc481d1b2c75f7` — Scanpy PAGA and diffusion pseudotime: Use when scVelo or CellRank inputs or packages are unavailable.

### Optional resources

- `rr_09c420e41332986a` — scanpy and anndata: Core object handling, PAGA, and diffusion pseudotime.
- `rr_8fe6d31c89d686a3` — scvelo: Dynamical or stochastic RNA-velocity estimation.
- `rr_89a306d4509dff5b` — CellRank: Terminal-state and fate-probability estimation.
- `rr_4b314561cb412cbc` — PAGA and diffusion pseudotime: Core graph abstraction and pseudotemporal ordering.
- `rr_2b299949b3fa68ef` — scVelo dynamical or stochastic model: RNA-velocity estimation.
- `rr_b0fbc2030a5d0aa1` — CellRank fate mapping: Terminal-state and fate-probability inference.

## Validation / QC

- Require the minimum cell and gene counts and verify the presence of PCA, UMAP, and cluster annotations before analysis.
- Inspect the PAGA graph and verify the selected root and terminal states against expected biology.
- Evidence requirement: Retain pseudotime, trajectory genes with direction and FDR, model objects, fate probabilities, and analysis metadata.

## Failure handling

- The requested root cell type is absent or terminal states are incorrectly detected.
- scVelo or CellRank is missing, or the dynamical velocity model fails.
- Too few cells make trajectory inference unsuitable.
- Fallback rule: Use Scanpy-only PAGA and pseudotime when optional velocity or fate components are unavailable.
- Fallback rule: Fall back from the scVelo dynamical model to the stochastic model when fitting fails.

## Limitations

- The workflow requires preprocessed single-cell data and is not appropriate for bulk data, terminally differentiated data without a trajectory, or datasets with fewer than 200 cells.
- Velocity and fate outputs depend on spliced or unspliced layers and optional packages, and terminal-state auto-detection requires manual graph review.

## Important domain-specific rules

- PAGA and diffusion pseudotime core, optional scVelo velocity, CellRank fate mapping, root and terminal-state validation, trajectory-gene export, and graceful-degradation rules.

## Portability boundary

- Biomni pdf-report-generation, internal packaged analysis functions, fixed completion-message orchestration, report fallback convention, and named upstream or downstream skill chain. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
