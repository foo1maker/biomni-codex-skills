# Omics Dataset Retrieval

Source workflow: `omics-dataset-retrieval`  
Parent Claude Science skill: `omics-dataset-discovery`

## Purpose

Systematically retrieve, deduplicate, classify, and relevance-audit publicly available omics datasets for a disease, phenotype, gene, or biological process without downloading raw data or performing downstream analysis.

## When to use

- Discover public transcriptomics, proteomics, metabolomics, epigenomics, genomics, single-cell, spatial, lipidomics, and multi-omics datasets.
- Build a cross-repository catalog with normalized metadata, access status, omics classification, and relevance labels.
- Audit dataset relevance as CORE/DIRECT, ADJACENT, WEAK, or REMOVE.
- Summarize repository coverage, relevance counts, access constraints, and catalog gaps.
- Optionally visualize the dataset landscape by omics type, repository, organism, or time.

## Inputs

- Disease, phenotype, gene, or biological process to search. (required)
- Alternative names, abbreviations, and gene symbols. (optional)
- Requested omics types. (optional)
- Organism, defaulting to all organisms unless explicitly restricted. (optional)
- Earliest publication year. (optional)
- Output directory. (optional)
- Optional tissue or cell-type focus, scientific goal, controlled-access preference, and desired report or figure format. (optional)

## Outputs

- Master CSV catalog containing all retrieved records and relevance labels.
- Validated CSV restricted to CORE/DIRECT and ADJACENT records.
- Repository-specific intermediate CSV files.
- Markdown summary report covering methods, repository yields, dataset breakdown, high-priority results, access notes, and caveats.
- Optional overview figure and supporting tabular summaries.

## Workflow

1. Ask only unanswered questions about organism, omics scope, tissue or cell focus, scientific goal, controlled-access inclusion, and outputs.
2. Combine the primary topic with synonyms, mechanistic terms, and omics-specific terms to form repository queries.
3. Query GEO with bounded targeted searches and parse study metadata.
4. Query SRA for sequencing studies not represented in GEO and parse study XML.
5. Query BioStudies and mine its search content for assay, organism, and description metadata.
6. Query PRIDE and related ProteomeXchange resources for proteomics studies.
7. Query OmicsDI for metabolomics and cross-repository study records.
8. Query CELLxGENE collections for single-cell datasets.
9. Query GDC or TCGA only when the topic is cancer-relevant.
10. Query ENCODE for epigenomics experiments.
11. Query Expression Atlas for baseline and differential expression experiments.
12. Run targeted web searches for general-purpose, controlled-access, population, and domain repositories lacking a suitable programmatic search.
13. Classify each record from title and summary text rather than trusting repository assay labels alone.
14. Assign CORE/DIRECT, ADJACENT, WEAK, or REMOVE from title and summary evidence, then manually review removal candidates.
15. Concatenate sources, normalize accessions and dates, deduplicate cross-repository mirrors, backfill selected high-relevance metadata, and sort by relevance then date.
16. Generate a transparent report of the search, repository coverage, relevance audit, access restrictions, and gaps.
17. Create the requested overview graphic from the assembled catalog.

## Decision rules

- Ask required clarification questions before any search unless the user already supplied the answers.
- Always query the high-yield Tier 1 programmatic repositories.
- Query Tier 2 specialized repositories only when relevant to the topic and requested omics types.
- Always attempt targeted web search for Tier 3 resources and manually curate candidate records.
- Search all organisms by default and restrict organism only when explicitly requested.
- Skip GDC and TCGA for non-cancer topics.
- Classify omics type from title and summary text because native assay fields can be unreliable.
- Include CORE/DIRECT in the primary catalog, include ADJACENT with a mechanistic relevance flag, retain WEAK for user review, and manually review REMOVE before exclusion.
- Normalize E-GEOD-NNN to GSENNN only at assembly and retain native ArrayExpress accessions.
- Label controlled-access repositories explicitly and retain them unless the user requested open-only results.
- Normalize recognized dates to YYYY-MM-DD, retaining unrecognized values instead of dropping data.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_0db097987c8e2d99` — GEO and SRA through NCBI E-utilities.: Always query for expression and sequencing studies.
- `rr_81c5ecf1f8d7b90e` — BioStudies and ArrayExpress.: Always query for EBI-hosted functional genomics studies.
- `rr_6982daee2cd6ba46` — PRIDE and ProteomeXchange.: Always query when proteomics is in scope.
- `rr_00d664cec5b6a6c1` — OmicsDI.: Always query as a metabolomics and cross-repository aggregator.
- `rr_88798c77a63271d1` — CZ CELLxGENE.: Always query when single-cell data are in scope.
- `rr_58e1e48487992041` — GDC and TCGA.: Query for cancer topics.

### Secondary resources

- `rr_20a65393080db2eb` — ENCODE and IHEC or EpiRR.: Use for epigenomics topics.
- `rr_df671de135bdd8bf` — Expression Atlas and Human Protein Atlas.: Use for expression and tissue-context questions.
- `rr_c4ca1299c72835a3` — Human Cell Atlas.: Use for human single-cell atlas coverage.
- `rr_ea01e7fd4dee8900` — MetaboLights and Metabolomics Workbench.: Use for metabolomics studies.
- `rr_016cdad645fc98d3` — MassIVE/GNPS, jPOST, iProX, and ENA.: Use for specialized proteomics, metabolomics, or sequence-archive coverage.
- `rr_b979605fb64ec34a` — cBioPortal.: Use for cancer genomics topics.

### Fallback resources

- `rr_fb75895c8aa34edd` — Targeted searches of Zenodo, Figshare, Dryad, OSF, Harvard Dataverse, and Synapse.: Use to find datasets in general-purpose repositories.
- `rr_fa0a9975544d42bd` — Targeted searches of ICGC, CPTAC, AWS Open Data, UK Biobank, and FinnGen.: Use for domain, population, or cloud-hosted datasets not captured by APIs.
- `rr_2bf6515b27e852f9` — Targeted searches of dbGaP, EGA, and JGA.: Use when controlled-access resources are included; catalog and flag access requirements.
- `rr_b5eff7fb04df1e98` — ProteomeCentral.: Use when jPOST or iProX programmatic access is unstable.
- `rr_d62016478d3b1690` — OmicsDI keyword search.: Use when direct MetaboLights search or Metabolomics Workbench title search is unsuitable.

### Optional resources

- `rr_ff0eb71971de1eaa` — NCBI GEO: Primary functional-genomics study discovery.
- `rr_30404e98489964bc` — NCBI SRA: Raw sequencing study discovery outside GEO.
- `rr_806bf5363c39fb26` — EBI BioStudies and ArrayExpress: Functional-genomics study discovery and metadata.
- `rr_56d2adae6cd6e9df` — PRIDE and ProteomeXchange: Proteomics study discovery.
- `rr_ef70dec98bdfcdb6` — OmicsDI: Metabolomics and cross-repository dataset aggregation.
- `rr_02aa9baf0345362b` — CZ CELLxGENE: Single-cell collection discovery.
- `rr_605495c14739aba7` — GDC and TCGA: Cancer genomics discovery.
- `rr_db427e595511e267` — ENCODE: Epigenomics experiment discovery.
- `rr_1b7ed987f668a10c` — Expression Atlas: Expression experiment discovery.
- `rr_d9a4f0de0700b7eb` — MetaboLights and Metabolomics Workbench: Metabolomics study discovery.
- `rr_cba693c6f810e089` — dbGaP, EGA, and JGA: Controlled-access genomics dataset discovery.
- `rr_c4c7a867d0096226` — Python requests: Call public repository APIs.
- `rr_178baeb270b10c13` — pandas: Assemble, normalize, classify, deduplicate, and export the catalog.
- `rr_41025f4e8b148cd7` — xml.etree.ElementTree: Parse SRA XML metadata.
- `rr_b795d40dc995bb72` — re and json: Parse text, identifiers, and API responses.
- `rr_665edbde6fc3cd1c` — matplotlib: Generate optional overview figures.
- `rr_10754bcee0094ddc` — AskUserQuestion: Collect unanswered search-scope and output preferences.
- `rr_6e925186673f01a8` — WebSearch: Search repositories without a suitable high-yield API and support manual curation.
- `rr_7dea48e31aa8a6f3` — cellxgene_census: Query CELLxGENE collection metadata.

## Validation / QC

- Reclassify records from title and summary rather than trusting inconsistent native repository type fields.
- Base relevance on title and summary and manually review every REMOVE candidate before deletion.
- Normalize cross-repository accession aliases before deduplication, but do not pre-filter mirrors at query time.
- Backfill expensive organism metadata only for CORE/DIRECT and ADJACENT BioStudies rows.
- Normalize heterogeneous date formats while retaining invalid or unrecognized source values.
- Carry open-versus-controlled access status into the catalog.
- Review likely cross-repository duplicates and assay misclassification after automated processing.
- Evidence requirement: Each catalog record should retain accession, title, assay or study type, organism, sample count, date, summary, repository, relevance, and access status when available.
- Evidence requirement: Support relevance labels with visible title and summary evidence.
- Evidence requirement: Preserve repository and normalized-accession provenance for deduplication decisions.
- Evidence requirement: Mark controlled-access records explicitly rather than implying that data are immediately downloadable.
- Evidence requirement: Report repositories searched, per-repository yields, high-priority datasets, coverage gaps, caveats, and access restrictions.

## Failure handling

- GEO broad searches exceed practical retmax or return large noisy result sets.
- A deprecated PRIDE v2 endpoint returns 404 or v3 keyword parameters do not filter server-side.
- Direct MetaboLights search is unavailable or Metabolomics Workbench requires an exact title.
- BioStudies search results omit structured organism, description, and sample-count fields and may truncate content.
- ArrayExpress GEO mirrors duplicate GEO records under E-GEOD accessions.
- A repository API requires payment, login, controlled access, or is unstable.
- A disease-trait search fails because repository terminology or ontology identifiers do not match the query.
- CELLxGENE misses results because its ontology terms differ from the free-text disease name.
- Repository assay labels produce incorrect omics classifications.
- Fallback rule: Replace one overly broad GEO query with 20–40 targeted topic, synonym, and omics queries.
- Fallback rule: Use PRIDE v3 and apply client-side word-boundary filtering over title and description.
- Fallback rule: Use OmicsDI for keyword discovery when direct metabolomics repository search is unavailable.
- Fallback rule: Mine BioStudies content and selectively fetch detailed records for high-relevance organism backfill.
- Fallback rule: Use ontology identifiers or manual browsing when disease-trait string matching fails.
- Fallback rule: Use targeted web search and record access requirements when a repository lacks a usable public API.
- Fallback rule: Use ProteomeCentral when jPOST or iProX access is unstable.
- Fallback rule: Skip GDC and TCGA for non-cancer topics.
- Fallback rule: Retain unrecognized date strings instead of discarding the associated records.

## Limitations

- The workflow catalogs public metadata but does not download raw files or perform downstream analysis.
- Repository APIs vary in availability, search behavior, metadata completeness, rate limits, and result caps.
- BioStudies search content can be truncated and does not directly expose every desired field.
- Some repositories require a login, institutional agreement, or paid access.
- Native assay-type fields can be misleading, so automatic omics classification remains imperfect.
- Cross-repository duplicates are common and accession normalization cannot guarantee perfect deduplication.
- Controlled-access records can be cataloged but still require external authorization before download.
- CELLxGENE disease discovery may depend on matching ontology terms rather than free text.

## Important domain-specific rules

- Tiered repository strategy separating always-query APIs, topic-specific APIs, and targeted web curation.
- Synonym-by-omics query matrix for broad but auditable dataset discovery.
- Text-based omics classification using title and summary evidence.
- Four-tier relevance audit with explicit inclusion actions and manual review before removal.
- Cross-repository accession normalization and deduplication that preserves native accessions.
- Access-status and controlled-data labeling.
- Heterogeneous date normalization with loss-avoiding fallback.
- Catalog and search-coverage reporting with repository yields, gaps, and caveats.

## Portability boundary

- Biomni AskUserQuestion tool invocation for the mandatory pre-search interview. — migration action: `exclude_or_capability_map`
- Biomni WebSearch tool orchestration for Tier 3 discovery. — migration action: `exclude_or_capability_map`
- Biomni mounted-results output path convention. — migration action: `exclude_or_capability_map`
- Optional handoff to Biomni report-generation capabilities for packaged deliverables. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
