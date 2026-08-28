# Provenance

This skill preserves the signature-reversal and indication-expansion contracts
while excluding platform execution, branded packaging, private paths, and
tool-specific calls. All named resources are optional adapters and must be
resolved through the resource registry before use.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `drug-repurposing-indication-expansion` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| `signature-reversal-lincs` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| DRS-1 | Confirm disease/signature identity, remove ambiguous mappings, and keep a shared background. | `drug-repurposing-indication-expansion` | `Scope`; `1. Resolve the disease signature — resolve_inputs.py`; `2. Build the ortholog map — build_orthologs.load_ortholog_map(workdir)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| DRS-2 | Use size-corrected reversal, permutation/null calibration, independent enrichment, and FDR. | `drug-repurposing-indication-expansion` | `4. Score connectivity — connectivity_score.run(bundle_pickle, out_csv)`; `5. Enrichment cross-check + canonical ranking — enrichment_crosscheck.run(conn_csv, bundle_pickle, out_csv)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| DRS-3 | Propagate one deterministic canonical rank across all downstream artifacts. | `drug-repurposing-indication-expansion` | `5. Enrichment cross-check + canonical ranking — enrichment_crosscheck.run(conn_csv, bundle_pickle, out_csv)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| DRS-4 | Require expected direction and FDR for a strong control verdict. | `drug-repurposing-indication-expansion` | `7. Validate with controls + MOA — controls_and_moa.py` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| DRS-5 | Warn below approximately 85% L1000 coverage, require two signatures for Tier-1, and keep context/specificity checks. | `signature-reversal-lincs` | `Stage 2 — Resolve genes to L1000 space + QC`; `Stage 5 — Rank + robustness`; `Mandatory vs optional robustness (quick reference)` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z |
| DRS-6 | Treat output as an in-silico hypothesis and report commercial-use review flags. | `signature-reversal-lincs` | `Scientific caveats (state these in every report)`; `Commercial-use caveats (needs_commercial_review)` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z |
| DRS-M1 | Weak/incorrect signature, unmapped organism, failed controls, or nonspecific top hit. | `drug-repurposing-indication-expansion` | `Scientific caveats (state these honestly in every report)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z |
| DRS-M2 | Signature service returns malformed/empty data or compound IDs remain unresolved. | `signature-reversal-lincs` | `Error handling` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | use condition |
|---|---|---|---|---|---|
| `LINCS L1000 disease and perturbation signatures` | `drug-repurposing-indication-expansion` | `0. Confirm resources (optional)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z | primary only when query space and access contract match |
| `LINCS L1000` | `signature-reversal-lincs` | `Engine & data (READ THIS FIRST — two distinct assets)` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z | primary/secondary adapter; retain source version |
| `Broad Repurposing Hub` | both source skills | `6. Annotate with the Broad Hub — annotate_hub.annotate(consensus_df, out_csv)`; `Database reference table` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace; https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:14:58.534Z; 2026-08-14T12:20:43.351Z | metadata only; never efficacy proof |
| `MGI mouse-to-human ortholog mapping` | `drug-repurposing-indication-expansion` | `2. Build the ortholog map — build_orthologs.load_ortholog_map(workdir)` | https://biomni.phylo.bio/skills/skill_9406c66f225cadbbb3cf0c85b95a7e5a?section=marketplace | 2026-08-14T12:14:58.534Z | required for cross-species inputs; bundled fallback must be disclosed |
| `ChEMBL`; `TxGNN` | `signature-reversal-lincs` | `Database reference table` | https://biomni.phylo.bio/skills/skill_3a9f742f9ccb071b1720a6c41dfdbb38?section=marketplace | 2026-08-14T12:20:43.351Z | optional mechanism/name cross-check only |

