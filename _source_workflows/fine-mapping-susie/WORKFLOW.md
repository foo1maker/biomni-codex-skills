# Fine Mapping Susie

Source workflow: `fine-mapping-susie`  
Parent Claude Science skill: `genetic-causal-and-risk-analysis`

## Purpose

Fine-map one GWAS locus with ancestry-matched signed LD and SuSiE, produce credible sets and posterior inclusion probabilities, and optionally annotate variants with cis-eQTL and regulatory evidence.

## When to use

- Ancestry-aware statistical fine-mapping of one GWAS locus
- Credible-set and posterior-inclusion-probability interpretation
- Optional functional annotation of fine-mapped variants

## Inputs

- One genomic region, or a lead variant with a flanking interval (required)
- GWAS summary statistics supplied by the user or identified by a GWAS Catalog accession (required)
- Study ancestry and either ancestry-matched reference LD or in-sample LD (required)
- Optional tissues, genes, and functional-annotation layers (optional)

## Outputs

- A 95% credible-set table with posterior inclusion probabilities
- A table of all analyzed variants and their posterior inclusion probabilities
- A machine-readable report containing convergence and mismatch diagnostics
- Optional eQTL and regulatory annotation tables, figures, and a PDF report

## Workflow

1. Acquire and subset the GWAS summary statistics to the requested locus.
2. Confirm column mapping, genome build, effect alleles, ancestry, trait type, and sample size.
3. Harmonize the locus to GRCh38 when needed without mixing genome builds.
4. Construct an ancestry-matched, effect-allele-aligned signed correlation LD matrix.
5. Run SuSiE and retain convergence and estimated mismatch diagnostics.
6. Add optional eQTL and regulatory annotations while retaining variants lacking those annotations.
7. Create locus and posterior-probability figures and package the results.

## Decision rules

- Do not choose a default ancestry; ancestry is required before selecting LD.
- Use signed allele-aligned correlation coefficients for LD, not squared correlation values.
- Treat high estimated mismatch, approximately above 0.1, as evidence that the fine-mapping result is not trustworthy without reconciliation.
- Do not silently substitute a different dataset when access to the requested dataset is gated.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_33d1bdd7f6208e13` — SuSiE with ancestry-matched signed LD: Fine-mapping a single GWAS locus with compatible summary statistics and LD

### Secondary resources

- `rr_b345107ddd3efb90` — GTEx v8, eQTL Catalogue, and ENCODE SCREEN annotations: Functional follow-up layers are requested for fine-mapped variants

### Fallback resources

- `rr_d344b0842cf4dbf7` — In-sample LD: An ancestry-matched reference panel is not the selected LD source and compatible in-sample LD is available

### Optional resources

- `rr_81374f2a82ece6c2` — GWAS Catalog: Source of harmonized GWAS summary statistics when an accession is used
- `rr_df62b1ccbf286302` — GTEx v8: Optional tissue-specific cis-eQTL annotation
- `rr_09104d0b7d67839b` — eQTL Catalogue: Optional cis-eQTL annotation
- `rr_e9917997ff8ceef6` — ENCODE SCREEN: Optional regulatory-element annotation
- `rr_8cd7279de319d3ec` — susieR: SuSiE statistical fine-mapping
- `rr_a30aa4629727af5a` — PLINK 2: LD construction and genotype operations
- `rr_17530cd533a333a5` — htslib: Indexed genomic data processing

## Validation / QC

- Confirm genome-build consistency before combining GWAS, LD, and annotation data.
- Align effect alleles between GWAS statistics and the signed LD matrix.
- Review SuSiE convergence and estimated mismatch diagnostics before interpreting credible sets.
- Evidence requirement: Report posterior inclusion probabilities and credible-set membership rather than treating the lead association as uniquely causal.
- Evidence requirement: Treat eQTL and regulatory annotations as mechanism-nominating evidence, not proof of causality.

## Failure handling

- Ancestry mismatch or allele misalignment between summary statistics and LD can invalidate the fit.
- A high estimated mismatch statistic indicates incompatibility between z-scores and LD.
- Long-range LD regions such as the MHC can produce difficult or unstable interpretation.
- Fallback rule: If an optional annotation layer fails, preserve the statistical fine-mapping outputs and explicitly record the omitted layer.
- Fallback rule: If the requested GWAS dataset is inaccessible, stop or request an authorized input instead of silently substituting another dataset.

## Limitations

- The workflow fine-maps one region per run rather than performing a genome-wide analysis.
- The workflow is ancestry-aware but is not a single-run trans-ancestry fine-mapping method.
- Fine-mapping requires a compatible LD matrix.

## Important domain-specific rules

- Require ancestry as an explicit gate before LD selection.
- Harmonize genome build and effect alleles before constructing signed LD.
- Use convergence and z-score-versus-LD mismatch diagnostics as interpretation gates.
- Layer optional functional evidence without dropping unannotated fine-mapped variants.

## Portability boundary

- Instructions that require the bundled Biomni scripts as the execution path — migration action: `exclude_or_capability_map`
- Phylo-specific PDF report generation and branding — migration action: `exclude_or_capability_map`
- Platform media-output-check invocation — migration action: `exclude_or_capability_map`
- Hard-coded Biomni runtime paths under /mnt and /workspace — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
