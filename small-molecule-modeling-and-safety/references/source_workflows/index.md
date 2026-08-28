# Source workflows for small-molecule-modeling-and-safety

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`adme-ml-modeling`](adme-ml-modeling/WORKFLOW.md) — Build, honestly evaluate, save, and apply supervised single-endpoint small-molecule in-vitro ADME models from labelled assay tables.
- [`binding-affinity-ml-model`](binding-affinity-ml-model/WORKFLOW.md) — Curate target-specific ChEMBL small-molecule affinity data, benchmark regression models under scaffold splits, and rank novel-scaffold library candidates with explicit applicability-domain tiers.
- [`drug-bioactivity-chembl`](drug-bioactivity-chembl/WORKFLOW.md) — Resolve a compound or target in ChEMBL and produce a provenance-tracked molecular potency, off-target, selectivity, and separate cellular-activity profile.
- [`molecular-property-admet`](molecular-property-admet/WORKFLOW.md) — Profile small molecules from SMILES for physicochemical properties, drug-likeness, structural alerts, and predicted absorption, distribution, metabolism, excretion, and toxicity endpoints, with standardized inputs, comparative plots, and auditable triage outputs.
- [`off-target-safety-pharmacology`](off-target-safety-pharmacology/WORKFLOW.md) — Produce an auditable off-target and secondary-pharmacology liability profile for one small molecule by separating intended targets from off-targets, combining orthogonal prediction engines with ADMET context, benchmarking only against held-out measured compound data, and gating validation claims by data sufficiency.
