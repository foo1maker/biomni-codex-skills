# Provenance

This portable contract distills one normalized functional-enrichment source and
excludes platform scripts, runtime paths, and report orchestration.

## Source record

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `functional-enrichment-from-degs` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |

## Rule and mode mapping

| id | distilled rule or mode | source_section | source URL | retrieved_at |
|---|---|---|---|---|
| FEA-1 | Select GSEA when a complete ranking exists; otherwise use ORA. | `Decision Guide`; `Standard Workflow` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |
| FEA-2 | Prefer a test statistic to log2 fold change for GSEA ranking. | `Decision Guide` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |
| FEA-3 | Use and report the tested-gene universe for ORA. | `1. Interpret and Validate Results`; `Decision Guide` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |
| FEA-4 | Record species, ID mapping, database/software versions, and licensing. | `Inputs`; `Database Licenses`; `Decision Guide` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |
| FEA-5 | Use leading-edge/directional evidence and do not present enrichment as mechanistic proof. | `1. Interpret and Validate Results`; `Decision Guide` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |
| FEA-M1 | Identifier/species mismatch, weak ranking, or failed vector export. | `Common Errors` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z |

## Resource provenance

| registry/resource key | source_section | source URL | retrieved_at | role |
|---|---|---|---|---|
| `MSigDB`; `Reactome`; `Gene Ontology`; `KEGG` | `Database Licenses` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z | gene-set adapters; KEGG opt-in after license review |
| `clusterProfiler`; `enrichplot`; `msigdbr` | `Software Requirements` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z | execution/plot adapters |
| `org.Hs.eg.db and org.Mm.eg.db` | `Software Requirements` | https://biomni.phylo.bio/skills/skill_eb2ad58c6e5a40fa9328874443eef99d?section=marketplace | 2026-08-14T12:17:44.204Z | species identifier mapping |

