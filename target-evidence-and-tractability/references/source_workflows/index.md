# Source workflows for target-evidence-and-tractability

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`cell-surface-antigen-discovery`](cell-surface-antigen-discovery/WORKFLOW.md) — Nominate antibody-accessible cell-surface antigens for ADC, CAR-T, bispecific, and radioligand development by integrating tumor-compartment specificity, extracellular topology, normal-tissue safety, tractability, and a validation harness.
- [`direction-of-effect-concordance`](direction-of-effect-concordance/WORKFLOW.md) — Determine whether target activation or inhibition is therapeutically favored for a specified indication by reconciling directional evidence across independent axes.
- [`genetic-constraint-gating`](genetic-constraint-gating/WORKFLOW.md) — Triage candidate genes with gnomAD loss-of-function constraint, cross-version stability checks, deterministic risk and strategy flags, and ClinGen-grounded disease notes.
- [`knowledge-graph-target-reasoning`](knowledge-graph-target-reasoning/WORKFLOW.md) — Prioritize human therapeutic targets for a disease by propagating disease seeds through PrimeKG with random walk with restart, explain top hits with multi-hop evidence paths, and validate ranking face validity and literature support.
- [`open-targets`](open-targets/WORKFLOW.md) — Query Open Targets target–disease associations, supporting evidence, genetics, and target, disease, drug, variant, or study annotations through the public GraphQL API.
- [`target-tractability-druggability`](target-tractability-druggability/WORKFLOW.md) — Assess whether a human protein-coding target is druggable and identify the most viable and emerging therapeutic modalities by combining tractability, structural, clinical, safety, and literature evidence.
- [`tissue-expression-specificity`](tissue-expression-specificity/WORKFLOW.md) — Profile one human protein-coding target across GTEx and Human Protein Atlas tissues, quantify tissue specificity, compare atlases, and synthesize on-target safety signals.
