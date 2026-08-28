# Provenance

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:17:47.987Z`
- Source URL: https://biomni.phylo.bio/skills/skill_b022f7a4010244ac8956c13d9dd60967?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Two-view and ten-sample input gate | multi-omics-integration | Inputs; Standard Workflow | 2026-08-14T12:17:47.987Z | Retained the minimum view/sample contract. |
| Incomplete-overlap alignment and likelihood choice | multi-omics-integration | Standard Workflow; Interpretation Guide | 2026-08-14T12:17:47.987Z | Retained missing-view handling and Bernoulli rule for binary views. |
| Factor fitting and exported evidence bundle | multi-omics-integration | Standard Workflow; Variance Decomposition (Key MOFA Output); Factor Interpretation | 2026-08-14T12:17:47.987Z | Retained scores, weights, variance, and feature evidence. |
| Convergence/memory fallback | multi-omics-integration | Common Issues; Standard Workflow | 2026-08-14T12:17:47.987Z | Retained lower-factor retry and variable-feature disclosure. |
| Unsupervised interpretation limits | multi-omics-integration | Interpretation Guide; Suggested Next Steps | 2026-08-14T12:17:47.987Z | Excluded platform scripts and report orchestration. |

The five shared policy files linked from `SKILL.md` govern claim classes,
resource roles, lineage, validation gates, and disclosed failures.
