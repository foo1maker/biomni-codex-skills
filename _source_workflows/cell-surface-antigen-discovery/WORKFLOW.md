# Cell Surface Antigen Discovery

Source workflow: `cell-surface-antigen-discovery`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Nominate antibody-accessible cell-surface antigens for ADC, CAR-T, bispecific, and radioligand development by integrating tumor-compartment specificity, extracellular topology, normal-tissue safety, tractability, and a validation harness.

## When to use

- Rank surface antigens for antibody-based modalities in a defined tumor type.
- Compare malignant or epithelial expression with stromal, immune, and endothelial compartments across single-cell atlases.
- Assess normal-tissue therapeutic index and antibody tractability while keeping essentiality as annotation only.

## Inputs

- A verified CZ CELLxGENE Census disease label or list of labels, or an annotated AnnData file with compartment and preferably malignant-cell annotations. (required)
- A surface-gene universe from the bundled seed or a genome-scale SURFY surfaceome. (required)
- Human Protein Atlas RNA and IHC normal-tissue baseline. (required)
- A pre-registered validated-target and cautionary-negative-control harness. (required)
- Optional modality emphasis and user-provided compartment labels. (optional)

## Outputs

- A tiered ranked surface-target table containing expression, specificity, topology, surface confirmation, safety, tractability, known-drug, essentiality-annotation, score, and tier fields.
- Evidence cards and literature-evidence records for selected candidates.
- Intermediate cohort, compartment-expression, topology, annotation, safety, validation, stability, coverage, manifest, and report-facts artifacts.
- Compartment heatmap, therapeutic-index map, validation-recall plot, and ranked-target chart in PNG and SVG formats.
- A validated report with methods, results, conclusions, figures, references, next steps, and an infographic.

## Workflow

1. Verify required packages and available data resources before installing or fetching anything.
2. Load the selected surfaceome, disease label or annotated data, and validation harness.
3. Discover whole-cell atlases, compute cross-dataset compartment expression, apply the extracellular-topology gate, and annotate tractability, locations, drugs, and essentiality.
4. Build the dual-signal Human Protein Atlas baseline and compute the normal-tissue therapeutic index.
5. Score candidates with topology, tumor quality, safety, cross-dataset consensus, and the validation harness.
6. Retrieve and persist literature evidence for top candidates and disclose any post-ranking harness additions.
7. Generate visualizations, export all results, enforce consistency gates, and build the report from persisted report facts.

## Decision rules

- Prioritize tumor-surface specificity and normal-tissue therapeutic index; never gate candidates on DepMap essentiality.
- Exclude cytoplasmic, organelle, secreted-ECM, and no-ectodomain candidates before scoring.
- Use the conservative minimum of protein-derived and RNA-derived safety values; mark missing HPA safety as unassessed and apply the documented neutral factor.
- Keep the pre-registered recall as the headline; if the harness is augmented after ranking, report both pre-registered and augmented recall.
- Treat an empty high-tier set, a negative-control failure, and an unconfirmed surface prediction as reportable outcomes rather than hiding or softening them.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_54508f6e69a96992` — CZ CELLxGENE Census or user-provided annotated AnnData: Estimating tumor-compartment expression and cross-atlas consensus.
- `rr_47d1a657fda1331e` — Human Protein Atlas dual-signal RNA and IHC baseline: Estimating normal-tissue therapeutic index and vital-organ liability.

### Secondary resources

- `rr_1cd8978f9c801055` — Open Targets Platform: Annotating antibody tractability, subcellular location, and known drugs.
- `rr_d2ee5ce6f038e497` — DepMap: Annotating broad essentiality without filtering candidates.
- `rr_4e898a4245532e0b` — Literature search: Grounding candidate-specific clinical and mechanistic claims.

### Fallback resources

- `rr_e95acd1fa3b14b36` — Curated whole-cell annotated AnnData: The Census disease label is thin or represented only by single-nucleus data.

### Optional resources

- `rr_b1c2531817d2edcc` — CZ CELLxGENE Census: Tumor single-cell expression across datasets and compartments.
- `rr_29c26e96edaa2f32` — Human Protein Atlas: Normal-tissue RNA and IHC safety baseline.
- `rr_1daed7d2bf6230cb` — GTEx: Optional orthogonal normal-tissue RNA baseline.
- `rr_8bcb2f44aeb3b626` — cellxgene-census: Census discovery and expression retrieval.
- `rr_063ec900fce6482b` — scanpy: Single-cell data handling.
- `rr_13889777971f5bd5` — anndata: Annotated matrix handling.

## Validation / QC

- For genome-scale analysis, require the topology gate to exclude at least one candidate; a zero-exclusion gate is treated as inert and invalid.
- Report pre-registered recall, negative-control verdict, stability verdict, topology-confirmation counts, and annotation coverage from exported artifacts.
- Require export consistency checks and visual validation of the therapeutic-index and ranking figures.
- Require a multi-page, nontrivial, text-extractable report with a visual pass.
- Evidence requirement: Read every quantitative report value from persisted tables or report_facts.json; never reconstruct values from memory.
- Evidence requirement: Keep missing expression, safety, tractability, and literature values missing or explicitly unassessed; never fabricate them.
- Evidence requirement: Map every clinical or mechanistic claim to a verified literature record.

## Failure handling

- A mismatched disease or tissue label can return no Census cells.
- Single-nucleus-only coverage can suppress surface transcripts and lower validation recall.
- A blanket plasma-membrane annotation makes the topology gate inert.
- Augmenting the validation harness after ranking without dual recall reporting creates circular validation.
- Fallback rule: Use a curated whole-cell annotated dataset when the Census label is thin or single-nucleus-only.
- Fallback rule: For genes missing from HPA, keep safety unassessed, apply the documented neutral factor, and disclose the count.
- Fallback rule: When the primary SURFY download fails, use the documented byte-identical mirror or a cited local Table S3 file.

## Limitations

- The workflow does not distinguish tumor-specific isoforms or glycoforms, quantify shedding, stratify by mutation or fusion, or estimate absolute protein copy number.
- The bundled surfaceome is a subset rather than a genome-wide screen.
- The epithelial-versus-stroma specificity axis is not a verified malignant-versus-normal comparison when malignant labels are absent.
- Held-out recall uses a small fixed split without repeated cross-validation or bootstrap uncertainty.

## Important domain-specific rules

- Specificity-plus-safety prioritization for surface-target modalities, with essentiality retained as annotation.
- Extracellular-topology gating combined with explicit confirmation status.
- Dual-signal normal-tissue safety scoring with explicit unassessed handling.
- Pre-registered positive and negative validation harness with anti-circularity reporting.
- Disk-grounded report facts and consistency gates that prevent narrative drift.

## Portability boundary

- Biomni environment inventory checks and data-lake assumptions. — migration action: `exclude_or_capability_map`
- Mandatory package-specific script names and no-inline-code orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, GenerateImage, Read media checks, pdf-report-generation, Phylo branding, and results-directory conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
