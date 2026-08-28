# Provenance

This derived skill consolidates four source modes while retaining their distinct
estimands and validation contracts. The normalized YAMLs and raw rendered source
captures below are the lineage anchors; no source-local execution shell or
report packaging was copied.

## Archive inputs

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| elastic-net-biomarker-panel | `../../02_normalized/elastic-net-biomarker-panel.yaml` | `../../00_raw/elastic-net-biomarker-panel/source.md` | `../../00_raw/elastic-net-biomarker-panel/metadata.json` |
| consensus-disease-signature | `../../02_normalized/consensus-disease-signature.yaml` | `../../00_raw/consensus-disease-signature/source.md` | `../../00_raw/consensus-disease-signature/metadata.json` |
| disease-progression-longitudinal | `../../02_normalized/disease-progression-longitudinal.yaml` | `../../00_raw/disease-progression-longitudinal/source.md` | `../../00_raw/disease-progression-longitudinal/metadata.json` |
| signature-response-enrichment | `../../02_normalized/signature-response-enrichment.yaml` | `../../00_raw/signature-response-enrichment/source.md` | `../../00_raw/signature-response-enrichment/metadata.json` |

## Derived-rule mappings

| Derived mode/rule | Source skill | Visible source section | Source URL | Retrieved at |
|---|---|---|---|---|
| Use nested CV, stability selection, and fold-contained preprocessing for sparse panels | elastic-net-biomarker-panel | `Standard Workflow`; `Inputs`; `Scope` | https://biomni.phylo.bio/skills/skill_024b2165ef874c1aa509a07168b27d98?section=marketplace | 2026-08-14T12:14:19.544Z |
| Keep discovery CV, locked-panel CV, and independent validation separate | elastic-net-biomarker-panel | `Outputs`; `Scientific Caveats`; `Agent Summary Guidelines` | https://biomni.phylo.bio/skills/skill_024b2165ef874c1aa509a07168b27d98?section=marketplace | 2026-08-14T12:14:19.544Z |
| Do not call a discovery-only panel validated or infer biology from names alone | elastic-net-biomarker-panel | `Interpretation Guidelines`; `Scientific Caveats`; `Common Issues` | https://biomni.phylo.bio/skills/skill_024b2165ef874c1aa509a07168b27d98?section=marketplace | 2026-08-14T12:14:19.544Z |
| Remove likely duplicate deposits and pool per-cohort effects and standard errors with random effects | consensus-disease-signature | `Workflow`; `Consensus Expression Meta-Signature`; `Scientific caveats` | https://biomni.phylo.bio/skills/skill_ba99051fddd1d71feb9c6b74443592be?section=marketplace | 2026-08-14T12:19:56.968Z |
| Define consensus by FDR and sign agreement, core by an effect floor, and preserve heterogeneous-control sensitivity | consensus-disease-signature | `Workflow`; `Inputs`; `Outputs (under output_dir)` | https://biomni.phylo.bio/skills/skill_ba99051fddd1d71feb9c6b74443592be?section=marketplace | 2026-08-14T12:19:56.968Z |
| Use the meta-tested universe for enrichment and report heterogeneity/contributing cohorts | consensus-disease-signature | `Scientific caveats`; `Workflow` | https://biomni.phylo.bio/skills/skill_ba99051fddd1d71feb9c6b74443592be?section=marketplace | 2026-08-14T12:19:56.968Z |
| Require repeated patient/timepoint data and a positive scale for trajectory analysis | disease-progression-longitudinal | `When to Use This Skill`; `Inputs`; `Quality Control` | https://biomni.phylo.bio/skills/skill_26fd7b7e1ad64ad89d97bb183182d4f0?section=marketplace | 2026-08-14T12:20:35.474Z |
| Choose TimeAx, mixed models, or HMM by sampling/stage structure and do not Z-score before TimeAx | disease-progression-longitudinal | `When to Use This Skill`; `Detailed Methodology`; `Standard Workflow` | https://biomni.phylo.bio/skills/skill_26fd7b7e1ad64ad89d97bb183182d4f0?section=marketplace | 2026-08-14T12:20:35.474Z |
| Use within-patient monotonicity, clinical validation, and FDR feature dynamics | disease-progression-longitudinal | `Quality Control`; `Outputs`; `Common Issues` | https://biomni.phylo.bio/skills/skill_26fd7b7e1ad64ad89d97bb183182d4f0?section=marketplace | 2026-08-14T12:20:35.474Z |
| Save all candidate response cohorts and retain explicit inclusion/exclusion reasons and roles | signature-response-enrichment | `Stage 1 — Automated discovery`; `Stage 2 — Agent curation`; `Deliverables (every run)` | https://biomni.phylo.bio/skills/skill_b65a07539d1b4f5e8955b98c7aaca5fe?section=marketplace | 2026-08-14T12:15:34.327Z |
| Gate by GSVA 1.50.5, signature coverage, baseline-to-endpoint response, and dual FDR | signature-response-enrichment | `Environment note (read before running)`; `Stage 3 — Response definition`; `Stage 4 — GSVA scoring`; `Stage 5 — Statistics (full template; extras auto-skip)` | https://biomni.phylo.bio/skills/skill_b65a07539d1b4f5e8955b98c7aaca5fe?section=marketplace | 2026-08-14T12:15:34.327Z |
| Use independence-aware Fisher values, paired testing only for pharmacodynamic pre/post, and categorical fallback explicitly | signature-response-enrichment | `Stage 5 — Statistics (full template; extras auto-skip)`; `Graceful degradation (documented behavior)`; `Stage 7 — Verify-before-trust (MANDATORY gate)` | https://biomni.phylo.bio/skills/skill_b65a07539d1b4f5e8955b98c7aaca5fe?section=marketplace | 2026-08-14T12:15:34.327Z |

## Resource and exclusion notes

The resource catalog is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). GEO,
ArrayExpress/BioStudies, GO, Reactome, Hallmark, and software names are
adapters only; access, versions, and license status must be recorded at use
time. Source-local report builders, platform orchestration, and fixed paths were
not carried into the portable skill.
