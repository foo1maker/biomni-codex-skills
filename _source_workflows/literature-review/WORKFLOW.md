# Literature Review

Source workflow: `literature-review`  
Parent Claude Science skill: `literature-and-evidence-synthesis`

## Purpose

Search and narratively synthesize peer-reviewed literature on a scoped topic, grounding claims in retrieved records and producing a cited review, structured evidence table, and report.

## When to use

- Survey what is known about a topic, method, mechanism, target, disease, or technology.
- Summarize the state of the art or recent advances.
- Synthesize key papers and compare agreements, conflicts, and open gaps.
- Build a structured evidence table.

## Inputs

- Topic and review scope. (required)
- Key questions or review angle. (optional)
- Time window, breadth, depth, and study-quality filters. (optional)
- Evidence depth: abstracts only or selected open-access full text. (optional)
- Requested deliverables. (optional)

## Outputs

- Narrative review with inline citations and explicit agreements, conflicts, and evidence gaps.
- Per-paper evidence table containing bibliographic metadata, study type, citation count, relevance, and evidence source.
- PDF report of the review.

## Workflow

1. Confirm only missing scope, key-question, time-window, evidence-depth, filter, and deliverable choices; state defaults for delegated choices.
2. Decompose the topic into focused facets, include synonyms, run multiple searches, and deduplicate by DOI then normalized title.
3. Use one-line highlights only for triage; read complete returned records and abstracts before writing.
4. When enabled, deepen the pivotal-paper subset with lawful open-access full text and retain evidence-depth provenance.
5. Organize the synthesis around the user's questions and make agreements, conflicts, uncertainties, and missing evidence explicit.

## Decision rules

- Use multiple focused queries rather than relying on one small result set.
- If filters produce too little evidence, loosen them and rely on relevance triage.
- Do not write from one-line search highlights.
- Cite only records actually retrieved and place each citation immediately after the claim it supports.
- Do not bypass paywalls; fall back to the abstract and label the evidence source.
- Use specialized workflows for quantitative meta-analysis, deep preclinical extraction, methods benchmarking, or clinical-trial landscape mapping.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- None declared in the normalized source record.

### Secondary resources

- `rr_e831e0349748e1f2` — Unpaywall: Primary route for locating a lawful open-access PDF from a DOI.
- `rr_167434c4f9e24365` — Europe PMC: Fallback route for open biomedical full text and repository records.

### Fallback resources

- `rr_1a6f829b476dc57e` — Retrieved abstract: Use when full text is unavailable; mark the evidence source as abstract.

### Optional resources

- None declared in the normalized source record.

## Validation / QC

- Deduplicate by DOI first and normalized title second.
- Verify title, authors, year, journal, DOI/URL, study type, and abstract before using a record.
- Never invent a PMID, DOI, title, or finding.
- Track full-text versus abstract-only evidence for every included paper.
- Evidence requirement: Ground the review and evidence table in complete returned records rather than search-result highlights.
- Evidence requirement: Use a citation only when the adjacent claim is supported by the retrieved record.
- Evidence requirement: Do not infer specific numbers, methods, or subgroup results that are absent from the available evidence.

## Failure handling

- Too few results because the query is narrow or uses incomplete names.
- Off-topic results because the query is broad or ambiguous.
- Thin synthesis caused by writing from one-line highlights.
- Important quantitative or subgroup details are absent from the abstract.
- No lawful open full text is available.
- Fallback rule: Add synonyms, split the topic into focused queries, broaden retrieval, and loosen strict filters.
- Fallback rule: Use the abstract and label it when no open full text is available.
- Fallback rule: Restrict the full-text pass to the most pivotal papers when time or access is limiting.

## Limitations

- The workflow is narrative synthesis and does not perform quantitative meta-analysis.
- Abstract-only evidence may omit exact numerical, methodological, and subgroup details.
- Provider-specific filters are not uniformly enforced across all search providers.

## Important domain-specific rules

- Concise scope clarification with explicit defaults for delegated choices.
- Multi-facet synonym-aware literature retrieval and DOI/title deduplication.
- Record-grounded narrative synthesis with claim-adjacent citations.
- Selective lawful full-text deepening with evidence-depth provenance.

## Portability boundary

- Biomni LiteratureSearch tool, provider-specific filters, record indices, and execution-trace reference file. — migration action: `exclude_or_capability_map`
- Biomni WebFetch invocation for selected open full text. — migration action: `exclude_or_capability_map`
- Biomni reporting and document-generation skills plus platform-specific reference rendering. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
