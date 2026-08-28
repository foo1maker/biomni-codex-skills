# Single Cell Census Query

Source workflow: `single-cell-census-query`  
Parent Claude Science skill: `single-cell-rna-seq-analysis`

## Purpose

Query the CZ CELLxGENE Census for gene-panel expression across cell types and perform donor-level pseudobulk differential expression for a case-versus-control comparison.

## When to use

- Summarize mean expression and the percentage of expressing cells for a gene panel by cell type in selected tissues.
- Aggregate raw counts to donor-by-cell-type pseudobulk samples and compare a case group with a control group.
- Assess global and cell-type-specific differential expression with donor as the replicate unit.

## Inputs

- A panel of HGNC gene symbols to resolve to Ensembl feature identifiers. (required)
- One or more Census tissue_general values. (required)
- Case and control disease labels that can be verified in the selected tissue scope. (required)
- Organism, pinned Census release, minimum cells per pseudobulk sample, minimum donors per group, donor batch size, and optional covariates. (optional)
- A user-supplied single-cell AnnData object or donor-by-cell-type raw-count matrix for the alternate input path. (optional)

## Outputs

- Per-cell-type gene-panel expression summary containing mean expression and percentage of cells expressing.
- Pseudobulk count, sample metadata, and feature metadata tables.
- Complete and significant differential-expression tables plus a gene-panel-by-cell-type differential-expression table.
- Visual summaries and a final report describing filters, thresholds, results, limitations, references, and next steps.

## Workflow

1. Resolve every requested gene symbol to an Ensembl feature identifier, report unresolved symbols, and pin the Census release.
2. Enumerate tissue and disease labels and donor counts before committing to the comparison.
3. Query the normalized layer for the selected gene panel and tissues and calculate expression summaries by cell type.
4. Stream raw counts in donor batches, aggregate by donor, cell type, and group, and retain samples meeting the cell-count threshold.
5. Run global and eligible per-cell-type DESeq2 comparisons with a case-versus-control contrast and false-discovery-rate control.
6. Create gene-panel, global differential-expression, and cell-type summary figures, then ground interpretation in verified literature.

## Decision rules

- Do not silently replace an absent case label; disclose the absence and obtain explicit confirmation before using a documented proxy.
- Prefer a comparison within one shared dataset when both groups are available; otherwise adjust for dataset or assay or flag the batch confound.
- Treat donors, not individual cells, as the replicate unit for differential expression.
- Use normalized expression for the atlas summary and integer raw counts for DESeq2.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_b1c2531817d2edcc` — CZ CELLxGENE Census: The requested tissues and both comparison labels are represented with primary-data cells and adequate donors.

### Secondary resources

- `rr_9fae15649a2c3662` — User-supplied single-cell AnnData or donor-by-cell-type raw-count matrix: The user already has compatible data and the Census acquisition step is unnecessary.

### Fallback resources

- `rr_13b9202c3c7a948c` — A public repository containing the requested condition: The requested case label is absent from the Census and no confirmed proxy is acceptable.

### Optional resources

- `rr_ad0cf995f0dbd3e1` — cellxgene-census and TileDB-SOMA: Python access to Census metadata and expression matrices.
- `rr_8a7a79093107ed07` — DESeq2: Donor-level global and cell-type-specific pseudobulk differential expression.

## Validation / QC

- Verify tissue and disease labels and report donor counts by cell type and group before analysis.
- Warn on unresolved or deprecated symbols instead of silently dropping them.
- Retain only pseudobulk samples meeting the minimum-cell threshold and test only cell types meeting the minimum-donor threshold in both groups.
- Evidence requirement: Record the pinned Census release, filters, group labels, donor counts, thresholds, and any proxy or dataset confound.
- Evidence requirement: Ground interpretation of the leading differential-expression findings and gene-panel biology in verified literature.

## Failure handling

- The requested case or control label is absent for the selected tissues.
- A requested symbol cannot be resolved to a Census feature identifier.
- A cell type has fewer than the required donors in either group and is therefore not testable.
- The pseudobulk accumulator exceeds available memory for a large dataset.
- Fallback rule: When the case label is absent, propose a documented proxy and proceed only after explicit confirmation.
- Fallback rule: When no acceptable Census label exists, source compatible data from another public repository.
- Fallback rule: For compatible user data, bypass the Census acquisition step and begin with pseudobulk construction or differential expression.

## Limitations

- Public atlases can have severe group imbalance and small case-donor counts.
- Cross-dataset comparisons can be confounded by assay or batch differences.
- A proxy condition is not equivalent to the originally requested condition.
- Gene-symbol resolution and the genome build depend on the pinned Census release.
- The non-human organism path is described as an untested extension.

## Important domain-specific rules

- Verify labels, tissue coverage, and donor counts before selecting a comparison.
- Stream raw counts and aggregate at the donor-by-cell-type level to control memory and avoid pseudoreplication.
- Separate normalized expression summaries from raw-count differential expression.
- Pin the public data release and record filters, thresholds, and comparison provenance.

## Portability boundary

- TodoWrite-based step tracking and routing to named sibling Biomni skills. — migration action: `exclude_or_capability_map`
- Bundled skill-local script names and package layout. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, Read media-output-check, ManageMachine, and pdf-report-generation calls. — migration action: `exclude_or_capability_map`
- Biomni-specific /mnt/results output paths and Phylo report branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
