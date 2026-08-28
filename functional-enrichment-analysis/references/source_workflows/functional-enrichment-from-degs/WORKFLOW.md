# Functional Enrichment From Degs

Source workflow: `functional-enrichment-from-degs`  
Parent Claude Science skill: `functional-enrichment-analysis`

## Purpose

Interpret differential-expression results with gene-set enrichment analysis, over-representation analysis, pathway visualizations, and documented database and ranking choices.

## When to use

- Rank-based gene-set enrichment analysis of differential-expression results
- Over-representation analysis of a selected gene list against an explicit background
- Cross-method pathway validation using both GSEA and ORA

## Inputs

- Differential-expression results with gene identifiers (required)
- A ranking metric such as a test statistic or log2 fold change for GSEA (optional)
- Adjusted p-values and a significance rule for constructing an ORA gene list (optional)
- Species and gene-identifier type (required)

## Outputs

- GSEA and ORA result tables as applicable
- Enrichment plots, dot plots, and pathway visualizations
- Serialized analysis objects and the ranked gene list
- A Markdown summary of methods and interpretable pathway findings

## Workflow

1. Validate the input columns, species, and identifier type, then remove or resolve unusable identifiers.
2. Construct a complete ranked gene vector for GSEA, preferring a test statistic when available.
3. Select gene-set collections consistent with the biological question and licensing constraints.
4. Run GSEA as the default analysis when a valid genome-wide ranking is available.
5. Run ORA when only a selected list is available or when it is requested as a complementary validation.
6. Interpret normalized enrichment scores, adjusted p-values, and leading-edge genes, and export tables and plots.

## Decision rules

- Use GSEA by default when a complete ranked gene vector is available.
- Use ORA when only a significant-gene list is available or when the user explicitly requests it.
- Prefer a differential-expression test statistic over log2 fold change as the GSEA ranking metric when the statistic is available.
- Use KEGG only after confirming the applicable commercial-use license; Hallmark, Reactome, and GO are the stated default alternatives.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_3bd8787a4dd3450c` — MSigDB Hallmark and Reactome gene sets: Running the default broadly interpretable and commercial-friendly enrichment workflow

### Secondary resources

- `rr_a6380ad9877cf2e9` — Gene Ontology Biological Process: A broader process-level ontology analysis is needed

### Fallback resources

- `rr_3c14e8e48622694a` — Base R SVG graphics device: The preferred SVG graphics device is unavailable

### Optional resources

- `rr_9c3f70ee11a158b8` — MSigDB: Gene-set collections including Hallmark and optional C6 or C7 collections
- `rr_cf6342a3273e2fcd` — Reactome: Curated pathway gene sets
- `rr_d9855dfc52ef33e3` — Gene Ontology: Biological-process enrichment
- `rr_b01ad7aa9e2487e3` — KEGG: Opt-in pathway collection subject to license confirmation
- `rr_a487141e81e96829` — clusterProfiler: GSEA and ORA execution
- `rr_0d82df84b9b509f4` — enrichplot: Enrichment visualization
- `rr_bb1dd15f6427077a` — msigdbr: Access MSigDB gene sets in R
- `rr_d2d74b6fd539c024` — org.Hs.eg.db and org.Mm.eg.db: Human and mouse identifier mapping

## Validation / QC

- Verify species and gene-identifier type before enrichment.
- For ORA, use and report the tested-gene universe rather than an unspecified default background.
- Record gene-set database and software versions with the analysis.
- Evidence requirement: Interpret enriched pathways together with their leading-edge genes and the direction of the ranked signal.
- Evidence requirement: Use literature and complementary analyses to validate pathway interpretation rather than presenting enrichment as mechanistic proof.

## Failure handling

- Identifier or species mismatch can yield few mapped genes and misleadingly empty enrichment results.
- Weak or poorly ordered ranking signals can produce few significant GSEA results.
- An unavailable SVG graphics device can prevent preferred vector-plot export.
- Fallback rule: If only a gene list is available, run ORA instead of inventing a ranking metric.
- Fallback rule: If the preferred SVG device fails, use the base R SVG device.

## Limitations

- Enrichment results depend on the ranking metric, gene universe, identifier mapping, and selected gene-set collection.
- ORA discards information outside the thresholded gene list and is sensitive to the chosen background.

## Important domain-specific rules

- Choose GSEA versus ORA based on whether a complete, defensible ranking is available.
- Make species, identifier mapping, ranking metric, gene universe, and database version explicit.
- Use GSEA and ORA together as complementary validation when both inputs are defensible.

## Portability boundary

- Mandatory execution through bundled Biomni scripts and relative-path conventions — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation capability and platform-specific report fallback — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
