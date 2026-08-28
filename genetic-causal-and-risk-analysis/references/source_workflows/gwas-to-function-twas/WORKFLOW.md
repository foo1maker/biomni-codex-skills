# Gwas To Function Twas

Source workflow: `gwas-to-function-twas`  
Parent Claude Science skill: `genetic-causal-and-risk-analysis`

## Purpose

Map genome-wide association signals to genetically predicted gene-expression associations with TWAS, refine them with colocalization and multi-tissue analyses, and support cautious therapeutic-directionality assessment.

## When to use

- Transcriptome-wide association testing from GWAS summary statistics
- Colocalization and multi-tissue refinement of TWAS signals
- Cautious therapeutic-directionality and druggability assessment

## Inputs

- Genome-wide GWAS summary statistics with SNP, chromosome, position, alleles, effect estimate, uncertainty or p-value, and sample size (required)
- Genome build, trait type, and study ancestry (required)
- Tissue choice or biological rationale for selecting expression-weight panels (required)
- Optional local eQTL data for colocalization and optional causal-validation inputs (optional)

## Outputs

- Tissue-specific TWAS association tables
- Colocalization and multi-tissue refinement results when the required inputs are available
- Therapeutic-directionality and druggability prioritization tables
- Manhattan-style, tissue-comparison, and prioritization figures plus a report

## Workflow

1. Validate genome-wide summary statistics, harmonize fields and alleles, and remove unresolved ambiguous variants.
2. Select tissues using biological relevance, LDSC evidence, or a broad GTEx panel.
3. Acquire compatible expression weights and LD reference panels, then run FUSION or S-PrediXcan.
4. Refine significant signals with colocalization and S-MultiXcan when the required data are available.
5. Infer an intervention direction only after colocalization, using the sign of the TWAS association and the trait-risk direction.
6. Add Mendelian-randomization or other causal-validation layers when available, followed by druggability assessment.

## Decision rules

- Require genome-wide summary statistics rather than a list limited to significant loci.
- The stated minimum GWAS sample size is greater than 5,000.
- Use FUSION for the comprehensive path when more than 32 GB memory is available, and S-PrediXcan for the faster lower-memory path.
- Require colocalization before assigning therapeutic directionality; treat posterior probability for a shared causal variant above 0.8 as strong support.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_8c29d9d25a02d2e5` — FUSION TWAS: A comprehensive analysis is requested and more than 32 GB memory is available

### Secondary resources

- `rr_fde516b1d4e38c36` — S-PrediXcan: A fast, lower-memory summary-statistics workflow is preferred

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_1daed7d2bf6230cb` — GTEx: Tissue-specific expression prediction weights and eQTL context
- `rr_bd152eb5a2f95708` — PredictDB: Prediction models used by MetaXcan methods
- `rr_2f3c34234e95680b` — TWAS Hub: Optional comparison and lookup resource for TWAS results
- `rr_90e2f9b9dce6abb8` — MR-Base: Optional Mendelian-randomization validation resource
- `rr_5a1f563b52c2823f` — FUSION: Transcriptome-wide association testing
- `rr_db9492330ecfa1f0` — MetaXcan: S-PrediXcan and S-MultiXcan association analyses
- `rr_4b4283cfa39c6e1a` — GTEx expression-prediction weights: Map GWAS allelic effects to genetically predicted tissue-specific expression

## Validation / QC

- Inspect duplicates, allele harmonization, ambiguous variants, genomic inflation, and LDSC intercept before interpreting TWAS associations.
- Keep tissue choice and prediction-weight version explicit.
- Use colocalization to distinguish shared causal evidence from LD-driven overlap.
- Evidence requirement: Do not use TWAS association alone as proof of causality or therapeutic direction.
- Evidence requirement: Require colocalization and seek consistency across tissues or independent causal-validation layers before target prioritization.

## Failure handling

- Allele mismatches or ambiguous variants can cause incompatible TWAS inputs or direction errors.
- A significant TWAS association may reflect linkage disequilibrium rather than a shared causal variant.
- Poorly matched tissue or ancestry panels can reduce power and distort interpretation.

## Limitations

- TWAS is an association analysis and does not by itself establish causality.
- Signals can be driven by linkage disequilibrium or correlated predicted expression.
- Power and interpretation depend on tissue selection and the available expression-weight models.

## Important domain-specific rules

- Genome-wide input validation and allele harmonization before association testing.
- Resource-aware choice between comprehensive FUSION and lower-memory S-PrediXcan.
- Colocalization gate before therapeutic-direction inference.
- Tiered refinement from single-tissue association to multi-tissue and causal-validation evidence.

## Portability boundary

- Bundled CLI script paths and fixed tier orchestration — migration action: `exclude_or_capability_map`
- Runtime and report-location path assumptions — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
