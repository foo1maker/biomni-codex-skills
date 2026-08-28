---
name: genetic-causal-and-risk-analysis
description: "Use when GWAS summary statistics or genotypes require MR, locus fine-mapping, TWAS/colocalization, or published polygenic-risk scoring with explicit ancestry, build, and causal-interpretation gates."
---

# Genetic causal and risk analysis

## Purpose

Route four related but non-interchangeable modes: two-sample Mendelian
randomization (MR), one-locus fine-mapping, GWAS-to-function TWAS, and scoring
published polygenic risk weights. Preserve their distinct estimands, input
contracts, and causal-interpretation limits.

## When to use

- MR: two exposure/outcome GWAS summary-statistic datasets and genetic
  instruments are available.
- Fine-mapping: one locus, ancestry, signed allele-aligned LD, and GWAS
  summary statistics are available.
- TWAS: genome-wide GWAS summary statistics and tissue-weight panels are
  available for association and optional colocalization.
- PRS: target genotypes and published score definitions/weights are available.

Do not treat association, fine-mapping, or a risk score as clinical advice or
causal proof. Route unsupported one-sample/nonlinear/multivariable MR, genome-
wide fine-mapping, trans-ancestry single-run fine-mapping, or de-novo PRS
construction to a method that supports them.

## Inputs

- Mode, trait/region, population/ancestry, genome build, effect-allele
  conventions, sample sizes, and estimand.
- MR: exposure/outcome summary statistics, instrument threshold, LD-clumping
  threshold, allele frequencies, and outcome type.
- Fine-mapping: locus, GWAS statistics, ancestry-matched or in-sample LD, and
  optional eQTL/regulatory annotations.
- TWAS: genome-wide statistics, tissue rationale, prediction-weight release,
  ancestry/build, and optional local eQTL or causal-validation inputs.
- PRS: genotype format, score IDs, published weights, build, and population
  labels; never invent weights.

## Workflow

1. Validate identifiers, genome build, alleles, ancestry, sample sizes, and
   access/license before selecting a mode-specific adapter.
2. **MR:** select/clump instruments, harmonize alleles, run IVW plus robust
   estimators, then heterogeneity, pleiotropy, directionality, single-SNP, and
   leave-one-out diagnostics.
3. **Fine-mapping:** subset one locus, build signed allele-aligned ancestry-
   matched LD, run the fine-mapping model, inspect convergence/mismatch, and
   add optional eQTL/regulatory evidence without dropping unannotated variants.
4. **TWAS:** validate genome-wide inputs, choose tissues and weights, run a
   comprehensive or lower-resource association route, then refine with
   colocalization/multi-tissue and optional causal validation.
5. **PRS:** resolve valid published score IDs, align builds and alleles, score
   each trait, and retain per-trait variant-match reports before population or
   multi-trait summaries.
6. Report effect estimates, PIPs/credible sets, TWAS associations, or risk scores
   with uncertainty, method concordance, provenance, and interpretation limits.

## Resource selection

- MR: `OpenGWAS`, `TwoSampleMR`, `MR-PRESSO` (optional), and named sensitivity
  tests; preserve the trait IDs and summary-statistic versions.
- Fine-mapping/TWAS: `GWAS Catalog`, `GTEx v8`, `eQTL Catalogue`, `FUSION`,
  `MetaXcan`, `susieR`, and compatible LD panels as optional adapters.
- PRS: `PGS Catalog`, `1000 Genomes Phase 3`, and `bigsnpr` for published score
  definitions and allele-matched scoring.

Use [resource selection policy](../_shared/resource_selection.md). A resource
with unknown access/license or an incompatible ancestry/build is unavailable
for the stated claim until explicitly resolved.

## Decision rules

- **GCR-1:** Harmonize alleles before MR; report IVW and robust estimator
  concordance, F statistics, heterogeneity, pleiotropy, directionality, and
  leave-one-out evidence. F<10 is a weak-instrument warning.
- **GCR-2:** Do not choose default ancestry for fine-mapping. Use signed,
  allele-aligned LD; an estimated mismatch around >0.1 requires reconciliation
  before interpreting credible sets.
- **GCR-3:** Treat fine-mapping PIPs/credible sets and eQTL/regulatory annotation
  as probabilistic/mechanism-nominating evidence, not a unique causal claim.
- **GCR-4:** TWAS requires genome-wide statistics and explicit tissue/weight
  versions. Colocalization is required before therapeutic directionality; a
  shared-causal posterior above 0.8 is strong support, not proof.
- **GCR-5:** PRS uses published peer-reviewed weights only. Match genotype and
  weight builds; a variant-match rate below 50% triggers reconciliation before
  interpretation.
- **GCR-6:** Keep causal estimation, locus prioritization, association, and
  population risk profiling as separate evidence classes and modes.

## Validation

- Check duplicate/ambiguous variants, allele/build/ancestry compatibility, LD
  quality, sample-size metadata, instrument strength, and harmonization loss.
- MR: inspect estimator agreement and every sensitivity result.
- Fine-mapping: inspect convergence, mismatch, signed LD, PIPs, and credible-set
  diagnostics.
- TWAS: inspect genomic inflation/LDSC context, tissue/weight version,
  colocalization, and LD-driven alternatives.
- PRS: record score IDs, build, allele matches, per-trait match reports, and
  reusable score metadata. Label score distributions as population summaries,
  not individual clinical recommendations.

See [validation policy](../_shared/validation_policy.md),
[evidence policy](../_shared/evidence_policy.md), and
[provenance policy](../_shared/provenance_policy.md).

## Failure handling

No instruments, failed clumping, mismatched alleles/builds, unavailable LD,
restricted data, missing colocalization, or low PRS match rates are visible
warnings or blocks, not reasons to silently substitute data. Optional annotation
failure may preserve statistical outputs with an omitted-layer record. If a
dedicated adapter is unavailable, retain only the supported partial mode and
state the lost claim.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Mode-specific input/harmonization manifests and resource records.
- MR estimates and sensitivity tables; fine-mapping credible sets/PIPs;
  TWAS/tissue/colocalization tables; or PRS scores/match reports.
- Diagnostic plots, uncertainty, limitations, and a claim ledger distinguishing
  association, prediction, inference, and recommendation.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`fine-mapping-susie`](references/source_workflows/fine-mapping-susie/WORKFLOW.md) — Fine-map one GWAS locus with ancestry-matched signed LD and SuSiE, produce credible sets and posterior inclusion probabilities, and optionally annotate variants with cis-eQTL and regulatory evidence.
- [`gwas-to-function-twas`](references/source_workflows/gwas-to-function-twas/WORKFLOW.md) — Map genome-wide association signals to genetically predicted gene-expression associations with TWAS, refine them with colocalization and multi-tissue analyses, and support cautious therapeutic-directionality assessment.
- [`mendelian-randomization-twosamplemr`](references/source_workflows/mendelian-randomization-twosamplemr/WORKFLOW.md) — Assess causal direction between an exposure and outcome using genetic instruments and two-sample GWAS summary statistics.
- [`polygenic-risk-score-prs-catalog`](references/source_workflows/polygenic-risk-score-prs-catalog/WORKFLOW.md) — Calculate one or more polygenic risk scores from target genotypes using published PGS Catalog weights, with optional multi-trait profiling and population-level comparisons.

<!-- END MANAGED: SOURCE WORKFLOWS -->
