# Provenance

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:19:35.198Z`
- Source URL: https://biomni.phylo.bio/skills/skill_fb055d3413196f5e948f5159c1207321?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Drug/class/target input modes and optional comparator | pharmacovigilance-ae-signal-detection | When to Use This Skill; Inputs; Clarification Questions | 2026-08-14T12:19:35.198Z | Retained mode resolution and background declaration. |
| OpenFDA/FAERS retrieval and complete contingency statistics | pharmacovigilance-ae-signal-detection | Standard Workflow; Outputs | 2026-08-14T12:19:35.198Z | Retained source-of-truth fields without copying packaged orchestration. |
| Standard signal thresholds and FDR | pharmacovigilance-ae-signal-detection | Clarification Questions | 2026-08-14T12:19:35.198Z | Retained defaults as user-overridable criteria. |
| Noise, label, low-confidence, and count hierarchy | pharmacovigilance-ae-signal-detection | Interpretation Guidelines | 2026-08-14T12:19:35.198Z | Retained full-result versus top-table distinction. |
| Non-causal interpretation and validation | pharmacovigilance-ae-signal-detection | Interpretation Guidelines; Outputs; Standard Workflow | 2026-08-14T12:19:35.198Z | Retained differential-reporting boundary and artifact reconciliation. |
| Access and graceful fallback | pharmacovigilance-ae-signal-detection | Common Issues; Standard Workflow; IF SCRIPTS FAIL — Failure Hierarchy | 2026-08-14T12:19:35.198Z | Mapped to declared access/resource fallbacks, excluding platform tools and branding. |

The five shared policy files linked by `SKILL.md` define evidence classes,
resource roles, lineage, validation gates, and non-silent recovery.
