# Source workflows for genetic-causal-and-risk-analysis

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`fine-mapping-susie`](fine-mapping-susie/WORKFLOW.md) — Fine-map one GWAS locus with ancestry-matched signed LD and SuSiE, produce credible sets and posterior inclusion probabilities, and optionally annotate variants with cis-eQTL and regulatory evidence.
- [`gwas-to-function-twas`](gwas-to-function-twas/WORKFLOW.md) — Map genome-wide association signals to genetically predicted gene-expression associations with TWAS, refine them with colocalization and multi-tissue analyses, and support cautious therapeutic-directionality assessment.
- [`mendelian-randomization-twosamplemr`](mendelian-randomization-twosamplemr/WORKFLOW.md) — Assess causal direction between an exposure and outcome using genetic instruments and two-sample GWAS summary statistics.
- [`polygenic-risk-score-prs-catalog`](polygenic-risk-score-prs-catalog/WORKFLOW.md) — Calculate one or more polygenic risk scores from target genotypes using published PGS Catalog weights, with optional multi-trait profiling and population-level comparisons.
