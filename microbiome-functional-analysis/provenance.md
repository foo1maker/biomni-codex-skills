# Provenance

The source is the captured, user-visible Biomni skill. `source.md` is the
rendered source; `metadata.json` records the visible sections and package
inventory. No platform execution or package file body is copied into this
portable skill.

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:16:28.372Z`
- Source URL: https://biomni.phylo.bio/skills/skill_fd517e4fafbb82ec7030a849e0ea0c23?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Processed-count and metadata contract; raw reads route upstream | microbiome-analysis | Inputs (entry point) | 2026-08-14T12:16:28.372Z | Retained input boundary and tree-dependent omission. |
| Subject-aware design and independent-subject reporting | microbiome-analysis | Step 0 — Clarify, discover resources, and inspect (always) | 2026-08-14T12:16:28.372Z | Retained repeated-measure safeguards. |
| Diversity, prevalence filtering, and three-method consensus | microbiome-analysis | Step 1 — Community diversity (optional); Step 2 — Differential abundance of taxa (optional) | 2026-08-14T12:16:28.372Z | Retained modular stages and concordant-direction rule. |
| Predicted function and NSTI disclosure | microbiome-analysis | Step 3 — PICRUSt2 predicted function (optional; compute-heavy) | 2026-08-14T12:16:28.372Z | Retained prediction-only interpretation and observed NSTI. |
| Enzyme-module separation and caveats | microbiome-analysis | Step 4 — Inferred microbial metabolites (optional; the distinctive core); Scientific caveats (state these in outputs) | 2026-08-14T12:16:28.372Z | Retained route separation, enzyme-dominance audit, and no measured-metabolite claim. |
| Enzyme-number resource and licensing decision | microbiome-analysis | Use the Biomni environment, don't reinvent it | 2026-08-14T12:16:28.372Z | Mapped to registry-backed, license-reviewed adapters; no platform calls copied. |

The shared-policy links in `SKILL.md` provide the cross-cutting claim,
resource, provenance, validation, and failure controls.
