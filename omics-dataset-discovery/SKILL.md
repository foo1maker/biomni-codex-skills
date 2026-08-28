---
name: omics-dataset-discovery
description: Discover, normalize, deduplicate, classify, and relevance-audit public omics dataset metadata across repositories without downloading raw data or performing downstream analysis.
---

# Purpose

Build an auditable cross-repository catalog for a disease, phenotype, gene, or
biological process. Preserve native and normalized accessions, visible metadata,
relevance evidence, access constraints, and search coverage.

# When to use

Use before downstream omics analysis when the user needs public dataset
discovery, repository coverage, metadata triage, or a relevance-ranked catalog.
This skill catalogs metadata only: it does not download raw files, analyze
counts, or infer biological results.

# Inputs

- Required: disease, phenotype, gene, or process.
- Optional: synonyms/abbreviations, gene symbols, omics types, organism,
  earliest year, tissue/cell focus, scientific goal, controlled-access policy,
  output directory, and report/figure preference.
- Record unanswered scope questions before searching, query date, search terms,
  repositories, status filters, and access policy.

# Workflow

1. Ask only unanswered questions about organism, omics scope, tissue/cell
   context, goal, controlled access, and outputs.
2. Build a query matrix combining the topic, synonyms, mechanistic terms, and
   omics-specific terms. Search the high-yield repository tier first.
3. Query NCBI GEO/SRA, EBI BioStudies/ArrayExpress, PRIDE/ProteomeXchange,
   OmicsDI, and CELLxGENE as relevant. Query GDC/TCGA only for cancer topics;
   use ENCODE, Expression Atlas, and other specialized repositories when the
   topic warrants them.
4. Use targeted web curation for repositories without a suitable public API.
   Retain controlled-access candidates and mark their access requirements.
5. Classify omics type from title and summary evidence, then assign
   CORE/DIRECT, ADJACENT, WEAK, or REMOVE. Manually review REMOVE candidates.
6. Normalize accession aliases and dates at assembly, deduplicate mirrors while
   preserving native accessions, selectively backfill high-relevance metadata,
   and sort by relevance then date.
7. Export the master catalog, validated relevance subset, coverage report, and
   optional overview figure. Report gaps, access restrictions, and unrecognized
   dates.

# Resource selection

Use `../../03_resource_registry/resource_registry.yaml` to record each
repository or parser's role, access, version/retrieval time, and license status.
Do not assume an API is stable merely because a repository is named.

- Tier 1: GEO/SRA, BioStudies/ArrayExpress, PRIDE/ProteomeXchange, OmicsDI,
  CELLxGENE, and GDC/TCGA when relevant.
- Tier 2: ENCODE, Expression Atlas, Human Cell Atlas, MetaboLights,
  Metabolomics Workbench, MassIVE/GNPS, jPOST, iProX, ENA, and cBioPortal as
  topic-specific adapters.
- Tier 3: targeted searches of general-purpose, domain, population,
  cloud-hosted, or controlled-access repositories. Record the actual URL and
  access condition for every manually curated record.

# Decision rules

- Search all organisms unless the user explicitly restricts organism.
- Always attempt the documented Tier 1 scope; query Tier 2 only when relevant;
  attempt targeted Tier 3 curation and disclose what was unreachable.
- Classify from title/summary rather than trusting inconsistent native assay
  labels. Include CORE/DIRECT in the primary catalog, ADJACENT with a
  mechanistic-relevance flag, and WEAK for review; do not silently discard
  REMOVE candidates.
- Normalize `E-GEOD-NNN` to the corresponding `GSENNN` key only for
  deduplication at assembly while retaining the native ArrayExpress accession.
- Normalize recognized dates to YYYY-MM-DD and retain unrecognized source
  strings. Mark controlled access explicitly; cataloging is not authorization.

# Validation

- Check that every record retains accession, title/summary, assay or study
  type, organism, sample count, date, repository, relevance, and access status
  when available, with missingness explicit.
- Verify title/summary evidence for relevance and preserve repository/native and
  normalized accession provenance after joins and deduplication.
- Check repository yields, query scope, rate/result limits, controlled-access
  counts, duplicate decisions, and date normalization.
- Keep catalog output separate from any future downstream analysis. A non-empty
  catalog does not establish dataset quality or biological relevance beyond the
  recorded evidence.

# Failure handling

If broad GEO searches are too noisy, replace them with 20–40 targeted queries.
Use PRIDE v3 with client-side filtering when older search behavior fails; use
OmicsDI for metabolomics discovery when direct search is unsuitable; mine
BioStudies content and selectively fetch detailed records for high-relevance
rows. Use ontology identifiers or manual browsing when free-text matching
misses results. Preserve failed query scope, access condition, and partial
catalog; never report an empty landscape merely because a repository failed.

# Outputs

Return a master CSV, CORE/DIRECT plus ADJACENT validation subset, intermediate
repository tables when useful, and a Markdown coverage/relevance/access report.
Optionally return an overview figure. Every output must state repositories
searched, query date, yields, deduplication method, relevance evidence, access
status, gaps, and limitations.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`omics-dataset-retrieval`](references/source_workflows/omics-dataset-retrieval/WORKFLOW.md) — Systematically retrieve, deduplicate, classify, and relevance-audit publicly available omics datasets for a disease, phenotype, gene, or biological process without downloading raw data or performing downstream analysis.

<!-- END MANAGED: SOURCE WORKFLOWS -->
