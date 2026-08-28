# Knowledge Graph Target Reasoning

Source workflow: `knowledge-graph-target-reasoning`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Prioritize human therapeutic targets for a disease by propagating disease seeds through PrimeKG with random walk with restart, explain top hits with multi-hop evidence paths, and validate ranking face validity and literature support.

## When to use

- Disease-specific knowledge-graph target prioritization
- Multi-hop evidence-path explanation of ranked targets
- Known-target enrichment or seed-recovery face-validity checking

## Inputs

- A disease name and any relevant subtype or umbrella terms (required)
- PrimeKG CSV (required)
- Edge-license mode indicating commercial or academic use (required)
- Optional TxGNN prediction and name-mapping files for academic mode (optional)

## Outputs

- A complete ranked-target table with scores, known-or-novel status, and provenance fields
- Run metadata recording anchors, seeds, parameters, iterations, licensing mode, and kept or dropped edge provenance
- Enumerated multi-hop evidence paths for top targets and a face-validity self-check record
- Commercial-mode Open Targets seed and known-target provenance records
- Ranking and evidence figures plus a target-prioritization PDF report

## Workflow

1. Resolve the disease name to one or more PrimeKG anchor identifiers and confirm semantic match and nonzero seed coverage.
2. For commercial mode, replace DisGeNET-derived PrimeKG disease seeds with Open Targets genetic-association seeds.
3. Rank genes with random walk with restart using the selected licensing mode and record graph and seed provenance.
4. For commercial mode, construct an Open Targets clinical-evidence label for external face-validity assessment.
5. Run the required face-validity check and stop for anchor review if known-target enrichment or its fallback check fails.
6. Enumerate license-compatible direct-seed, protein-interaction-bridge, and shared-concept evidence paths for the top targets.
7. Gather one grounded disease-specific supporting sentence and citations for each top target, explicitly noting absent support.

## Decision rules

- Use commercial mode by default only when both restricted DrugBank edges are dropped and DisGeNET-derived seeds are replaced with Open Targets genetic-association seeds.
- Use academic mode only for internal or non-commercial work because it retains DisGeNET-derived seeds, DrugBank-derived edges, and optional TxGNN support.
- Use disease-anchor identifiers exactly as resolved because underscores can belong to a single composite PrimeKG identifier.
- If the face-validity check fails, stop and re-examine disease anchors, seed counts, and PrimeKG coverage before reporting.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_6c2d2b2740ea1530` — PrimeKG with Open Targets genetic-association seeds and clinical labels: The output may be used commercially and both seed replacement and restricted-edge removal are verified

### Secondary resources

- `rr_b3b60c838fac80b1` — PrimeKG with DisGeNET-derived seeds and optional TxGNN layer: The work is internal or non-commercial and the output is marked as requiring commercial review

### Fallback resources

- `rr_40c513d6c4c4d4d7` — Seed-recovery face-validity check: Open Targets clinical labels are unavailable in commercial mode

### Optional resources

- `rr_904457b23a696ae0` — PrimeKG: Knowledge-graph backbone for random-walk propagation and evidence paths
- `rr_1cd8978f9c801055` — Open Targets Platform: Commercial-mode genetic-association seeds and independent clinical known-target labels
- `rr_d6f40696aed8ae23` — Random Walk with Restart: Network propagation and ranking of genes from disease seeds
- `rr_07409c0aadfd6d86` — TxGNN prediction layer: Optional academic-mode drug-target support weighted below the PrimeKG random-walk score

## Validation / QC

- Confirm that each selected disease anchor matches the intended disease and has meaningful seed coverage.
- Require and report the face-validity check, including which label kind was used.
- For a commercial-safe claim, verify that no restricted source is retained and that seed replacement is recorded as true with commercial provenance.
- Inspect gene degree for hub bias among high-ranked targets.
- Evidence requirement: Explain each top target with explicit graph paths and one grounded disease-specific literature statement or an explicit no-support finding.
- Evidence requirement: Describe the output as discovery and hypothesis generation, not statistical inference or causal testing.
- Evidence requirement: Report actual restart, score-weight, seed, edge-license, and source-provenance parameters.

## Failure handling

- A semantically wrong or weakly seeded disease anchor can fail the face-validity check.
- Filtering PrimeKG only by node-source columns does not remove DisGeNET-derived disease-gene evidence because node vocabulary and edge-evidence source are different concepts.
- Knowledge-graph hubs can rank highly because of degree rather than disease-specific support.
- Fallback rule: If Open Targets is unreachable for the commercial known-target label, run and identify the source-free seed-recovery check.
- Fallback rule: If no solid literature support is found for a top target, report that absence rather than fabricating a citation.

## Limitations

- The ranking is discovery-oriented and has no p-values or false-discovery-rate interpretation.
- Degree and hub bias are mitigated but not eliminated.
- TxGNN support is repurposing evidence rather than direct target inference and is disabled in commercial mode.
- The ranking inherits gaps in PrimeKG; absence of an edge is not evidence of absent biology.

## Important domain-specific rules

- Disease-anchor resolution with semantic and seed-coverage review.
- License-aware separation of commercial and academic evidence modes.
- Required face-validity gate with a labeled fallback and explicit label kind.
- Per-target convergence of rank, evidence paths, literature, degree, and known-target status.

## Portability boundary

- Biomni LiteratureSearch and predict_admet_properties capability calls — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation skill, Phylo branding, and platform media checks — migration action: `exclude_or_capability_map`
- Fixed /mnt/results paths and mandatory bundled-script sequencing — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
