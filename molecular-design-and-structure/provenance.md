# Provenance

This skill consolidates five reviewed source skills into one router while
keeping their modes distinct. The captured sources are visible rendered text;
platform-specific scripts, orchestration, paths, report branding, and tool-call
contracts are excluded.

| Rule group retained | Source skill | Source section | Source URL | Retrieved at | Normalized path | Raw source path |
|---|---|---|---|---|---|---|
| Predictor selection, confidence bands, manifests, and domain omission | protein-structure-prediction | Method selection (size-first default); Confidence metrics — and what the numbers do and do not mean; Workflow | https://biomni.phylo.bio/skills/skill_6e83f49077c908119c713fef84e79b6f?section=marketplace | 2026-08-14T12:15:59.206Z | `<private-local-path>` | `<private-local-path>` |
| Observed-ligand identity, contact geometry, confidence, and no-apo gate | ligand-binding-mode-analysis | When to Use This Skill; Standard Workflow; Interpretation Guidelines; Scientific Caveats | https://biomni.phylo.bio/skills/skill_56f6a5b3514ba76baf5bca5c0c83d704?section=marketplace | 2026-08-14T12:12:43.88Z | `<private-local-path>` | `<private-local-path>` |
| Native redock validation, labeled enrichment branch, and docking limitations | large-scale-virtual-screening | Decision Guide; Workflow; Scientific caveats (MUST appear in every report's Limitations) | https://biomni.phylo.bio/skills/skill_b1cf4aefacef741d340cf8a7d522ab0b?section=marketplace | 2026-08-14T12:19:30.409Z | `<private-local-path>` | `<private-local-path>` |
| Backend ordering, molecule validity/novelty filters, bounded retrosynthesis, and hypothesis caveat | generative-molecule-design | Step 0 — Choose the activity backend (the only target-specific decision); Step 3 — Generate (graph GA); Step 4 — Score, filter, select; Honest-reporting caveats (include these in every report) | https://biomni.phylo.bio/skills/skill_0008d582238d9a1f65a2322b3bf71728?section=marketplace | 2026-08-14T12:17:56.787Z | `<private-local-path>` | `<private-local-path>` |
| Degrader dossier, canonical assembly, evidence-class separation, and heuristic boundary | targeted-degrader-design | 1. Target & precedent dossier; 2. Select & validate warheads; 5. Combinatorially assemble in RDKit; 7. Prioritize (transparent score) + shortlist; Scientific caveats (always surface these) | https://biomni.phylo.bio/skills/skill_1de0c57c38270f6702c80157011ab521?section=marketplace | 2026-08-14T12:22:03.137Z | `<private-local-path>` | `<private-local-path>` |

Source-specific retrieval metadata are preserved in each normalized YAML and
raw `metadata.json`; this table is a compact mapping of the rules carried into
the portable router.
