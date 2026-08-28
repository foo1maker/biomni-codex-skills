# Source workflows for molecular-design-and-structure

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`generative-molecule-design`](generative-molecule-design/WORKFLOW.md) — Generate target-focused small-molecule hypotheses with graph-based evolution, multi-objective scoring, novelty and chemistry filters, and optional retrosynthesis assessment.
- [`large-scale-virtual-screening`](large-scale-virtual-screening/WORKFLOW.md) — Dock a compound library against a protein target with AutoDock Vina, validate the docking setup by native-ligand redocking, rank and triage hits, assess enrichment when labels exist, and summarize scaffold-level structure-activity patterns.
- [`ligand-binding-mode-analysis`](ligand-binding-mode-analysis/WORKFLOW.md) — Map a small-molecule ligand's binding mode from an observed protein–ligand co-crystal structure, including pocket residues, geometric interactions, ligand-fragment contacts, optional cross-structure concordance, and auditable tabular and report outputs.
- [`protein-structure-prediction`](protein-structure-prediction/WORKFLOW.md) — Predict a protein structure with AlphaFold v2, Boltz-2, Chai-1, or ESMCFold2 and report normalized per-residue confidence, available global confidence, confidence-band composition, domain-resolved confidence from UniProt annotations, and predictor/fallback provenance.
- [`targeted-degrader-design`](targeted-degrader-design/WORKFLOW.md) — Design and triage bifunctional degraders while keeping computed chemistry, docking, and property results separate from precedent-based prioritization heuristics.
