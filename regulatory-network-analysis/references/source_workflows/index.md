# Source workflows for regulatory-network-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`chip-atlas-target-genes`](chip-atlas-target-genes/WORKFLOW.md) — Retrieve and rank genes near peaks for a specified transcription factor from precomputed ChIP-Atlas public ChIP-seq matrices, with binding, coverage, interaction, cell-context, and co-location information.
- [`coexpression-network`](coexpression-network/WORKFLOW.md) — Build weighted gene co-expression networks to identify co-expressed modules, module-trait associations, and highly connected hub genes.
- [`grn-pyscenic`](grn-pyscenic/WORKFLOW.md) — Infer transcription-factor regulons de novo from single-cell RNA-seq expression and calculate cell-level regulon activity with the pySCENIC pipeline.
- [`upstream-regulator-analysis`](upstream-regulator-analysis/WORKFLOW.md) — Identify candidate transcriptional regulators by integrating ChIP-Atlas binding enrichment with bulk RNA-seq differential-expression results and directional target concordance.
