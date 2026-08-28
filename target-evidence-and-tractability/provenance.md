# Provenance

Seven source workflows are consolidated as evidence modes. No mode is treated
as a substitute for another, and no source adapter is assumed to be available.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `cell-surface-antigen-discovery` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_37c83b1a2ba2ef32921254e95c376919?section=marketplace | 2026-08-14T12:18:14.659Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Scoring Methodology`; `Data Integrity Rules`; `Scientific Caveats`; `Safety Honesty (READ BEFORE WRITING THE REPORT)`; `Common Issues`; `References`. Retains surfaceome/accessibility, tumor specificity, normal-tissue safety, missing-value handling, and validation controls. |
| `direction-of-effect-concordance` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_967e65dbbeb85ee774732a6211f1000c?section=marketplace | 2026-08-14T12:20:24.18Z | `When to Use This Skill`; `Inputs`; `Step 0 — Align (brief)`; `Step 1 — Resolve identifiers`; `Step 2 — Pull structured evidence (per axis, per target)`; `Step 4 — Build evidence matrix + consensus calls`; `Step 5 — CITATION-VERIFICATION GATE (mandatory, blocking)`; `Confidence Tiers & Discordance`; `The Direction-Mapping Rule (fixed, documented)`; `Scientific Caveats & Integrity Rules`; `Failure Modes to Avoid`. Retains independent evidence axes, direction/discrepancy flags, confidence tiers, and citation verification. |
| `genetic-constraint-gating` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_70fa3fe61a8d4640c4a520641092cfcd?section=marketplace | 2026-08-14T12:18:08.179Z | `Scope`; `Inputs`; `Workflow (what the code does, and why)`; `Database reference`; `Scientific caveats`; `Error handling`; `Self-test`. Retains constraint thresholds, missing-gene reporting, gating use, and non-causal interpretation. |
| `knowledge-graph-target-reasoning` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_66117a119805611838ba0a0cdde6edb2?section=marketplace | 2026-08-14T12:19:16.727Z | `When to use`; `Inputs`; `Workflow`; `Licensing & edge-license modes`; `Step 0 — Resolve disease name → PrimeKG anchor id(s)`; `Step 0b — Commercial-safe seeds (Open Targets genetics; commercial mode only)`; `Step 1 — Rank targets`; `Step 2 — Face-validity self-check (REQUIRED)`; `Step 3 — Enumerate evidence paths for top hits`; `Scientific caveats (surface these; never hide them)`. Retains graph seeds/edges, propagation parameters, licensing mode, known/novel status, and edge provenance. |
| `open-targets` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_032628bba2f145cfba0f7c5acb46ee45?section=marketplace | 2026-08-14T12:18:42.19Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `Open Targets Platform GraphQL API`; `Step 1 — Helper`; `Step 2 — Resolve names → IDs (only if needed)`; `Step 3 — Run the actual query`; `Step 4 — Iterate / paginate`; `Best Practices`; `Common Issues`; `References`. Retains target/evidence retrieval, release and evidence-type fields, and query provenance. |
| `target-tractability-druggability` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9375050e400a2b52a377933c2f508d68?section=marketplace | 2026-08-14T12:21:51.163Z | `When to Use This Skill`; `Inputs`; `Workflow`; `Step 1 — Open Targets evidence`; `Step 2 — Structure retrieval (auto PDB -> AlphaFold)`; `Step 3 — Pocket detection with fpocket`; `Step 4 — Modality scorecard`; `Interpretation notes`; `Common pitfalls`; `Mandatory caveats (include in every report)`. Retains modality-aware tractability, pocket/essentiality/safety evidence, confidence components, and non-binary interpretation. |
| `tissue-expression-specificity` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_1770b7f9d13954ae16b07b3ec91515bf?section=marketplace | 2026-08-14T12:22:33.523Z | `Scope`; `Inputs`; `1. Resolve the target`; `2. Ingest GTEx (datalake-first, API fallback)`; `3. Ingest HPA (stream-parse one entry)`; `4. Tau specificity + high-baseline flags`; `5. Cross-atlas concordance (organ-collapsed)`; `6. On-target safety synthesis + literature`; `Error handling`; `Scientific caveats (state the relevant ones in every report)`; `Data sources & licenses (attribution required)`. Retains cross-atlas concordance, specificity metrics, vital-organ panel, missingness, and expression-as-liability limits. |

## Rule-to-source map

- Surface-antigen accessibility and normal-tissue gating ->
  `cell-surface-antigen-discovery.yaml`.
- Independent direction axes and discordance ->
  `direction-of-effect-concordance.yaml`.
- Constraint gating and thresholds -> `genetic-constraint-gating.yaml`.
- Seed/edge provenance, propagation, and licensing mode ->
  `knowledge-graph-target-reasoning.yaml`.
- Evidence-platform query/release provenance -> `open-targets.yaml`.
- Modality-specific tractability and confidence components ->
  `target-tractability-druggability.yaml`.
- Tissue specificity, vital-organ panel, and on-target liability ->
  `tissue-expression-specificity.yaml`.

All source URLs, timestamps, and raw/normalized paths are copied from the
normalized `source` records.
