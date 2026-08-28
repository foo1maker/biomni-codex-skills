# Provenance

This derived skill consolidates four normalized routes while keeping their
estimands and mode boundaries explicit. Platform scripts, private paths, and
report orchestration were excluded.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `mendelian-randomization-twosamplemr` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_6655c68c5e9249eba44e6519b7c0b11e?section=marketplace | 2026-08-14T12:16:25.048Z |
| `fine-mapping-susie` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace | 2026-08-14T12:17:02.237Z |
| `gwas-to-function-twas` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_6313334ee7324c7099fa65c5f5ee9590?section=marketplace | 2026-08-14T12:18:50.213Z |
| `polygenic-risk-score-prs-catalog` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_7ef1858e91dd4b7fbe5daddf31e5002a?section=marketplace | 2026-08-14T12:14:58.975Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| GCR-1 | Harmonize alleles, use multiple MR estimators, and expose weak-instrument/pleiotropy/directionality diagnostics. | `mendelian-randomization-twosamplemr` | `Standard Workflow`; `Interpreting Results` | https://biomni.phylo.bio/skills/skill_6655c68c5e9249eba44e6519b7c0b11e?section=marketplace | 2026-08-14T12:16:25.048Z |
| GCR-2 | Require ancestry before signed LD selection and reconcile estimated mismatch around >0.1. | `fine-mapping-susie` | `Standard Workflow`; `Interpreting Results` | https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace | 2026-08-14T12:17:02.237Z |
| GCR-3 | Report PIPs/credible sets and treat optional functional annotation as non-causal evidence. | `fine-mapping-susie` | `Standard Workflow`; `Interpreting Results` | https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace | 2026-08-14T12:17:02.237Z |
| GCR-4 | Require colocalization before therapeutic-direction inference; keep TWAS association non-causal by itself. | `gwas-to-function-twas` | `Standard Workflow`; `Phase 3: Statistical Refinement (Tier 2+)`; `Phase 4: Therapeutic Directionality` | https://biomni.phylo.bio/skills/skill_6313334ee7324c7099fa65c5f5ee9590?section=marketplace | 2026-08-14T12:18:50.213Z |
| GCR-5 | Use published weights, match genome builds, and reconcile PRS match rates below 50%. | `polygenic-risk-score-prs-catalog` | `Standard Workflow`; `Common Issues` | https://biomni.phylo.bio/skills/skill_7ef1858e91dd4b7fbe5daddf31e5002a?section=marketplace | 2026-08-14T12:14:58.975Z |
| GCR-6 | Keep the four evidence modes separate and disclose their limits. | all four sources | `When to Use This Skill`; `Common Issues` | https://biomni.phylo.bio/skills/skill_6655c68c5e9249eba44e6519b7c0b11e?section=marketplace; https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace; https://biomni.phylo.bio/skills/skill_6313334ee7324c7099fa65c5f5ee9590?section=marketplace; https://biomni.phylo.bio/skills/skill_7ef1858e91dd4b7fbe5daddf31e5002a?section=marketplace | 2026-08-14T12:16:25.048Z; 2026-08-14T12:17:02.237Z; 2026-08-14T12:18:50.213Z; 2026-08-14T12:14:58.975Z |
| GCR-M1 | No instruments, clumping/access failure, allele/build mismatch, LD mismatch, missing colocalization, or low PRS match. | all four sources | `Common Issues`; `⚠️ CRITICAL — DO NOT`; `Phase 3: Statistical Refinement (Tier 2+)` | https://biomni.phylo.bio/skills/skill_6655c68c5e9249eba44e6519b7c0b11e?section=marketplace; https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace; https://biomni.phylo.bio/skills/skill_6313334ee7324c7099fa65c5f5ee9590?section=marketplace; https://biomni.phylo.bio/skills/skill_7ef1858e91dd4b7fbe5daddf31e5002a?section=marketplace | 2026-08-14T12:16:25.048Z; 2026-08-14T12:17:02.237Z; 2026-08-14T12:18:50.213Z; 2026-08-14T12:14:58.975Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | role |
|---|---|---|---|---|---|
| `OpenGWAS`; `TwoSampleMR`; `MR-PRESSO` | `mendelian-randomization-twosamplemr` | `Installation`; `Interpreting Results` | https://biomni.phylo.bio/skills/skill_6655c68c5e9249eba44e6519b7c0b11e?section=marketplace | 2026-08-14T12:16:25.048Z | MR data/estimator/sensitivity adapters |
| `GWAS Catalog`; `GTEx v8`; `eQTL Catalogue`; `susieR`; `PLINK 2` | `fine-mapping-susie` | `What's in this skill (self-contained)`; `Installation` | https://biomni.phylo.bio/skills/skill_f55142b5be96e79ec64484a53f8087a4?section=marketplace | 2026-08-14T12:17:02.237Z | locus/LD/annotation adapters |
| `GTEx`; `PredictDB`; `TWAS Hub`; `FUSION`; `MetaXcan`; `GTEx expression-prediction weights` | `gwas-to-function-twas` | `Phase 2: TWAS Association Testing`; `Required Software`; `Phase 5: Causal Validation (Tier 3+)` | https://biomni.phylo.bio/skills/skill_6313334ee7324c7099fa65c5f5ee9590?section=marketplace | 2026-08-14T12:18:50.213Z | tissue-weight/TWAS adapters |
| `PGS Catalog`; `1000 Genomes Phase 3`; `bigsnpr` | `polygenic-risk-score-prs-catalog` | `Installation`; `Standard Workflow` | https://biomni.phylo.bio/skills/skill_7ef1858e91dd4b7fbe5daddf31e5002a?section=marketplace | 2026-08-14T12:14:58.975Z | published score/genotype/scoring adapters |
