# Open Targets

Source workflow: `open-targets`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Query Open Targets target–disease associations, supporting evidence, genetics, and target, disease, drug, variant, or study annotations through the public GraphQL API.

## When to use

- Retrieve target, disease, and drug annotation profiles using standardized identifiers.
- Retrieve target–disease association scores, datatype breakdowns, and supporting evidence.
- Retrieve variant, GWAS study, credible-set, locus-to-gene, and colocalisation information.
- Resolve gene, disease, or drug names to Open Targets identifiers.

## Inputs

- An Ensembl gene identifier, EFO or imported disease identifier, ChEMBL compound identifier, genomic variant identifier, or GWAS Catalog study identifier. (optional)
- A free-text gene, disease, or drug name to resolve before the main query. (optional)
- The requested goal: annotation, association, supporting evidence, or genetics. (required)
- Entity scope and expected number of entities. (required)
- Optional association filters, datasource weights, and disease-ontology descendant roll-up. (optional)
- Requested summary, JSON, CSV, or TSV output format. (optional)

## Outputs

- Target–disease association tables with overall scores and per-datatype breakdowns.
- Evidence rows containing datasource, score, and supporting literature or links.
- Target, disease, or drug annotation summaries.
- Variant, GWAS, credible-set, locus-to-gene, and colocalisation data.
- Resolved identifiers from search hits and optional normalized tabular exports.

## Workflow

1. Submit the GraphQL query with variables, raise on HTTP errors, and raise separately on GraphQL errors returned in the response body.
2. Resolve a free-text target, disease, or drug name to a standardized identifier using search.
3. Run a query limited to the entity fields, association rows, evidence, or genetics data required by the user.
4. Paginate list fields with page index and size, except evidences, which uses size and cursor.
5. Switch to bulk Open Targets datasets instead of looping the API when the requested scope reaches thousands of entities.

## Decision rules

- If the user supplied a standard identifier and a clear goal, proceed without asking redundant questions.
- Use the API for single entities and tens to hundreds of entities; recommend bulk datasets for thousands or all entities.
- Resolve free-text names and non-primary identifiers before any entity-specific query.
- Request only the needed GraphQL fields and traverse related objects in one query when possible.
- Use enableIndirect for broad disease terms when descendant ontology evidence should be included.
- Inspect current datatype score identifiers before supplying custom association datasource weights.
- Do not request thousands of rows in a single API call.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_ba5bb83c15e5dad8` — Open Targets Platform GraphQL endpoint.: Use for interactive queries over one to hundreds of entities.

### Secondary resources

- `rr_c590640b3c21b6bd` — Open Targets GraphQL playground and current schema.: Use to inspect field names, types, and query structure when the schema is uncertain or has changed.
- `rr_544900478b80a127` — Open Targets search query.: Use when input is a free-text name or non-primary identifier.

### Fallback resources

- `rr_6664e12a0241fa32` — Open Targets FTP downloads, BigQuery open-targets-prod, or AWS Open Data datasets.: Use for systematic extraction across thousands of entities or the full dataset.
- `rr_eab0b7bb45705c35` — Schema introspection or the GraphQL playground.: Use when a queried field is no longer present or has been renamed.

### Optional resources

- `rr_1cd8978f9c801055` — Open Targets Platform: Integrated source of target, disease, drug, association, evidence, and genetics data.
- `rr_fd17dba06befc3c1` — Ensembl: Identifier system for target genes.
- `rr_9b2037f586bb3157` — EFO and imported disease ontologies: Identifier systems for diseases.
- `rr_806458b94ba25e08` — ChEMBL: Identifier system for drugs and compounds.
- `rr_81374f2a82ece6c2` — GWAS Catalog: Identifier system for GWAS studies.
- `rr_f48c55f08412b6c6` — requests: Submit GraphQL HTTP requests.
- `rr_178baeb270b10c13` — pandas: Optionally normalize JSON and export tabular results.
- `rr_275f07b7c065b8a3` — ot_query: Validate HTTP and GraphQL responses and return the data payload.
- `rr_aa4218c4fbff60e4` — Open Targets GraphQL API: Execute target, disease, drug, association, evidence, and genetics queries.
- `rr_abbf99b8d8602a79` — pdf-report-generation: Create a requested final PDF report from analysis artifacts.

## Validation / QC

- Check both HTTP status and the GraphQL errors field because query errors may be returned with HTTP 200.
- Use standardized Ensembl, EFO, ChEMBL, variant, or GWAS identifiers after name resolution.
- Paginate every list completely for the requested scope, using cursor pagination for evidences.
- Use the current GraphQL schema and query only necessary fields.
- Record the Open Targets data release version in reports.
- Evidence requirement: Preserve datasource identifiers, evidence scores, and supporting literature or links in evidence outputs.
- Evidence requirement: Preserve overall association scores and per-datatype score breakdowns when reporting target–disease associations.
- Evidence requirement: Cite the Open Targets data release version in any report.
- Evidence requirement: Attribute data to the Open Targets Platform and its current public documentation or release paper.

## Failure handling

- A GraphQL error is returned inside an HTTP 200 response.
- A field name has changed in the current schema.
- A broad disease returns no associated targets without descendant roll-up.
- A symbol or name is rejected because the entity query requires a standardized identifier.
- Results are truncated because a list was not paginated.
- Many per-entity calls are slow or time out.
- A workflow uses the obsolete standalone Open Targets Genetics endpoint.
- Fallback rule: Inspect the current playground or run schema introspection when a field no longer exists.
- Fallback rule: Run search first when a symbol or name is not recognized.
- Fallback rule: Enable descendant evidence roll-up for broad disease ontology terms when appropriate.
- Fallback rule: Continue pagination for truncated list fields, using cursor pagination for evidences.
- Fallback rule: Switch to FTP, BigQuery, or AWS bulk data when many identifiers make API iteration unsuitable.
- Fallback rule: Use genetics fields in the main Platform API instead of the retired standalone Genetics endpoint.

## Limitations

- The interactive GraphQL API is not intended for bulk or systematic extraction across thousands of entities.
- The platform is not intended for non-human biology, general literature search, EHR or clinical-trial recruitment data, or proprietary datasets.
- Public documentation does not advertise an API key or rate-limit headers, but maintainers discourage one-entity-at-a-time loops.
- Results and field availability depend on the current Open Targets data release and GraphQL schema.

## Important domain-specific rules

- GraphQL request helper that separates HTTP failures from GraphQL response errors.
- Name-to-standard-identifier resolution before entity queries.
- Minimal-field GraphQL templates for entity profiles, associations, evidence, and genetics.
- Page-index and cursor pagination handling selected by field.
- Scope-based handoff from interactive queries to bulk datasets.
- Normalized JSON-to-table export that retains scores, datasource identifiers, and supporting evidence.

## Portability boundary

- Delegation of a requested PDF to the Biomni pdf-report-generation skill. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
