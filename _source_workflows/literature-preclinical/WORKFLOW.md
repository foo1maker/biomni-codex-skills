# Literature Preclinical

Source workflow: `literature-preclinical`  
Parent Claude Science skill: `literature-and-evidence-synthesis`

## Purpose

Search and narratively synthesize peer-reviewed non-clinical in vitro and in vivo evidence for a target–disease question, with structured study details, concordance analysis, translational-gap assessment, and traceable deliverables.

## When to use

- Survey preclinical target–disease evidence.
- Extract in vitro models, assays, findings, and direction of effect.
- Extract in vivo models, doses/routes, endpoints, and findings.
- Compare in vitro and in vivo concordance.
- Assess efficacy, pharmacokinetic/pharmacodynamic, toxicity, and translational evidence gaps.

## Inputs

- Molecular target and disease/indication. (required)
- Key questions or review angle. (optional)
- Time window, breadth, depth, and quality filters. (optional)
- Evidence depth: abstracts only or selected open-access full text. (optional)
- Requested deliverables. (optional)

## Outputs

- Narrative preclinical review with inline citations and explicit agreement, conflict, and evidence-gap statements.
- Per-paper evidence table containing bibliographic fields, experiment type, model systems, direction of effect, and evidence source.
- PDF report of the review.

## Workflow

1. Confirm only missing scope, evidence-depth, and deliverable choices; state defaults for delegated choices.
2. Run multiple focused target–disease queries covering in vitro, in vivo, model-system, mechanism, efficacy, pharmacokinetic/pharmacodynamic, and toxicity facets; then deduplicate by DOI and normalized title.
3. Triage summaries, then read the complete returned metadata and abstracts before extracting study details.
4. Capture in vitro models and assays, in vivo models and dosing/endpoints, perturbation direction, and key findings for each included study.
5. When enabled, read lawful open-access full text for the most pivotal studies and retain abstract-only status for the rest.
6. Synthesize in vitro and in vivo landscapes, model-system concordance, IND-enabling readiness, and translational gaps into the selected deliverables.

## Decision rules

- Do not apply a human-only filter or clinical study-design filters to a preclinical search unless the user explicitly requests a clinical subset.
- If strict filters starve the preclinical corpus, loosen them and prioritize relevance during triage.
- Do not write the synthesis from one-line highlights; read full returned records.
- Cite only actually retrieved records and place the citation immediately after the supported claim.
- Use only lawful open-access copies and fall back to the abstract when no open full text is available.
- Narratively distinguish in vitro-only, in vivo-only, and combined evidence and expose missing efficacy, pharmacokinetic/pharmacodynamic, or toxicity coverage.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- None declared in the normalized source record.

### Secondary resources

- `rr_e831e0349748e1f2` — Unpaywall: Primary route for locating a lawful open-access PDF from a DOI.
- `rr_167434c4f9e24365` — Europe PMC: Fallback route for open biomedical full text and repository records.

### Fallback resources

- `rr_1a6f829b476dc57e` — Retrieved abstract: Use when no lawful open full text can be obtained; mark the evidence source as abstract.

### Optional resources

- None declared in the normalized source record.

## Validation / QC

- Deduplicate records by DOI first and normalized title second.
- Never invent a PMID, DOI, title, model system, cell line, dose, endpoint, or finding.
- Track whether every included paper was reviewed from full text or abstract.
- Explicitly report agreements, conflicts, thin evidence, and missing translational components.
- Evidence requirement: Ground every claim and evidence-table row in the complete returned record or lawfully retrieved full text.
- Evidence requirement: Use inline citations only for records that actually support the adjacent claim.
- Evidence requirement: Do not infer exact doses, cell lines, model construction, effect sizes, or toxicity details when they are absent from the available evidence.

## Failure handling

- Search queries are too narrow or omit target synonyms and preclinical facets.
- Human-only or clinical-design filters exclude the intended preclinical evidence.
- The synthesis is based on one-line highlights rather than full records.
- Pivotal experimental details are unavailable in abstracts.
- No lawful open full text is available for a selected paper.
- Fallback rule: Broaden focused queries, add target synonyms and model/assay terms, and loosen filters when results are sparse.
- Fallback rule: Use abstract evidence and label it when full text is unavailable; never bypass a paywall.
- Fallback rule: Reduce full-text depth to the most pivotal studies when the full-text pass is too slow.

## Limitations

- The workflow is narrative synthesis and does not pool effect sizes.
- Abstract-only papers may lack exact models, doses, endpoints, effect sizes, and toxicity details.
- Strict bibliometric or sample-size filters can remove relevant preclinical studies.

## Important domain-specific rules

- Facet-based preclinical search with synonym expansion and DOI/title deduplication.
- Per-study extraction of in vitro and in vivo models, assays, endpoints, perturbation direction, and findings.
- Cross-level concordance analysis and IND-enabling readiness/gap assessment.
- Evidence-depth provenance that distinguishes full-text and abstract-only support.

## Portability boundary

- Biomni LiteratureSearch tool, provider filters, record indices, and execution-trace reference file. — migration action: `exclude_or_capability_map`
- Biomni WebFetch invocation for selected full-text documents. — migration action: `exclude_or_capability_map`
- Biomni formatting skills, GenerateImage, and platform-specific PDF packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
