# Tissue Expression Specificity

Source workflow: `tissue-expression-specificity`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Profile one human protein-coding target across GTEx and Human Protein Atlas tissues, quantify tissue specificity, compare atlases, and synthesize on-target safety signals.

## When to use

- Profile target expression across human tissues in two independent bulk atlases.
- Calculate tissue-specificity scores and flag high-baseline tissues.
- Assess organ-level cross-atlas concordance and synthesize an on-target safety matrix.

## Inputs

- One human gene symbol, Ensembl gene identifier, or UniProt accession. (required)

## Outputs

- Per-tissue GTEx and Human Protein Atlas expression tables.
- Tissue-specificity scores, high-baseline flags, organ concordance, and a safety-organ matrix.
- Ranked-tissue, concordance, specificity-summary, and safety-heatmap figures plus a report.

## Workflow

1. Resolve the query to exactly one human protein-coding gene with canonical symbol, Ensembl identifier, and UniProt cross-reference.
2. Retrieve per-tissue GTEx median TPM, preferring the curated median-TPM resource and falling back to the Portal API, while recording version and source.
3. Stream-parse the Human Protein Atlas record for per-tissue nTPM and its native specificity call.
4. Calculate log-transformed and linear tau specificity and apply absolute-or-top-decile high-baseline flags within each atlas.
5. Collapse fine GTEx sites to organs, harmonize labels, and calculate Spearman rank and log-scale Pearson concordance over shared organs.
6. Build a safety matrix from vital organs plus every high-baseline organ and add cited literature context for leading signals.

## Decision rules

- Stop and request a precise identifier when the query is ambiguous, unmapped, or not a single human protein-coding gene.
- Use rank and log-scale concordance because HPA nTPM and GTEx TPM are not directly comparable magnitudes.
- Include heart, brain or CNS, liver, kidney, and lung in the safety panel and add every data-driven high-baseline organ.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_87207fe0320d977c` — Curated GTEx median-TPM resource: The curated datalake resource is available and its release can be recorded.
- `rr_c9fe37cf2781c3e9` — Human Protein Atlas tissue expression: The target is covered by the atlas.

### Secondary resources

- `rr_aeabd456fd6abdd0` — GTEx Portal v8 API: The curated GTEx resource is absent.

### Fallback resources

- `rr_5e773addcee2369c` — Single available atlas: One atlas is unavailable or lacks target coverage.

### Optional resources

- `rr_1daed7d2bf6230cb` — GTEx: Open-access per-tissue bulk median TPM summaries.
- `rr_29c26e96edaa2f32` — Human Protein Atlas: Per-tissue nTPM and native specificity call with attribution and release-specific share-alike obligations.
- `rr_7e0f694aa63e0cb1` — Ensembl, UniProt, and NCBI: Identifier-resolution cross-references with resource-specific attribution.
- `rr_42bc4f5de0dd382f` — matplotlib and seaborn: Ranked bars, concordance scatter, specificity summary, and safety heatmap.

## Validation / QC

- Record the exact GTEx source and version and the exact Human Protein Atlas release used.
- Report log-transformed tau as primary together with the atlas and number of tissues.
- Keep atlas-native values separate and compare patterns rather than raw magnitudes.
- Evidence requirement: Cite the GTEx source and date, dbGaP accession, Human Protein Atlas source and publication, and identifier resources used.
- Evidence requirement: Cite every external safety-context claim and do not invent references when no relevant literature record is returned.

## Failure handling

- The target cannot be resolved uniquely to a human protein-coding gene.
- The GTEx API returns no data after versioned-identifier retries.
- The target is absent from Human Protein Atlas coverage.
- Fallback rule: Fall back from the curated GTEx resource to the GTEx Portal API and record the change.
- Fallback rule: When only one atlas is available, continue with its specificity, ranked expression, and safety results and mark cross-atlas concordance unavailable.
- Fallback rule: When no relevant literature is returned, disclose the gap and omit unsupported context.

## Limitations

- HPA nTPM and GTEx TPM are not directly comparable magnitudes.
- Bulk tissue values mask cell-type heterogeneity.
- Tau depends on the tissue set and normalization and is calculated from median profiles.
- Target expression in an organ does not prove that a therapy reaches or perturbs that organ.
- The data sources and identifier logic are human-only.

## Important domain-specific rules

- Triangulate expression patterns across independent atlases while preserving native scales and versions.
- Pair a global specificity score with tissue-level absolute-or-relative high-baseline flags.
- Construct a vital-organ core plus data-driven expansion for safety review.

## Portability boundary

- Bundled skill-local tissue-expression and figure scripts and their fixed orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, ToolSearch, GenerateImage, Read media-output-check, and pdf-report-generation calls. — migration action: `exclude_or_capability_map`
- Biomni-specific /mnt/results paths, Phylo palette, and report branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
