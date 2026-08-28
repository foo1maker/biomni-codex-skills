# Provenance

This portable skill is distilled only from the following archived, visible
source records. Platform execution wrappers and presentation shells were not
copied; the rules below are capability mappings supported by the cited source
sections.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `real-world-evidence` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_ea1afce75ae2b4fc030bbe787188145e?section=marketplace | 2026-08-14T12:16:36.613Z | `Inputs`; `Standard Workflow`; `Design Philosophy (read this first)`; `Clarification Questions`; `Agent Summary Guidelines`; `Common Issues`; `Interpretation Guidelines`; `Suggested Next Steps`. Retains configuration-driven cohort/comparator construction, Table 1 and denominator checks, survival/landmark estimation, the EPV-gated Cox rule, phenotype validation, and descriptive-not-causal interpretation. |

## Rule-to-source map

- Cohort and comparator configuration, canonical tables, and treatment maps ->
  `real-world-evidence.yaml` sections `Inputs`, `Standard Workflow`, and
  `Design Philosophy (read this first)`.
- Rest-of-population versus active-comparator selection -> section
  `Clarification Questions`.
- Survival, confidence intervals, numbers at risk, and EPV gating -> sections
  `Design Philosophy (read this first)` and `Agent Summary Guidelines`.
- Missing exposure maps, unreached medians, and insufficient events -> section
  `Common Issues`.
- Non-causal interpretation and phenotype validation -> sections
  `Interpretation Guidelines` and `Suggested Next Steps`.

`retrieved_at` is the capture timestamp recorded in the normalized source;
resource availability, access, and licensing must be checked at run time.
