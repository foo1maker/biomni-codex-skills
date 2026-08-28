# Source workflows for single-cell-rna-seq-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`scrna-disease-drug-discovery`](scrna-disease-drug-discovery/WORKFLOW.md) — Identify and prioritize disease-relevant drug targets from single-cell RNA-seq by integrating cell-type-resolved differential expression, pathways, ligand-receptor interactions, genetic evidence, and druggability.
- [`scrna-trajectory-inference`](scrna-trajectory-inference/WORKFLOW.md) — Order preprocessed single-cell RNA-seq cells along differentiation or disease trajectories, identify branches and terminal fates, discover transition-associated genes, and optionally estimate RNA velocity and fate probabilities.
- [`scrnaseq-scanpy-core-analysis`](scrnaseq-scanpy-core-analysis/WORKFLOW.md) — Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and analysis-ready visualization using Scanpy and the scverse ecosystem.
- [`scrnaseq-seurat-core-analysis`](scrnaseq-seurat-core-analysis/WORKFLOW.md) — Process single-cell RNA-seq data through quality control, normalization, clustering, cell-type annotation, and visualization using Seurat v5.
- [`single-cell-census-query`](single-cell-census-query/WORKFLOW.md) — Query the CZ CELLxGENE Census for gene-panel expression across cell types and perform donor-level pseudobulk differential expression for a case-versus-control comparison.
