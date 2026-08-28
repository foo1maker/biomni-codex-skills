# Literature Deep Review

Source workflow: `literature-deep-review`  
Parent Claude Science skill: `literature-and-evidence-synthesis`

## Purpose

Produce an audit-ready literature review in which every displayed claim is linked to an exact, resolvable supporting or contradicting source anchor and every displayed anchor receives a blinded entailment re-check.

## When to use

- Retrieve exact source sentences with page, section, or figure locators.
- Build an audit-ready claim-to-evidence table.
- Hunt contradictions, null results, and rescue evidence.
- Support a scoped mechanism, direction-of-effect, or target-evidence decision.
- Create a comprehensive report that must pass deterministic verification.

## Inputs

- Review question and decision scope, including population/model, perturbation, outcome, date, and design boundaries. (required)
- Literature records from the configured literature-search capability. (required)
- Atomic claims with stable claim identifiers and claim text. (required)
- Directly accessible or user-supplied full text when sentence-, locator-, or figure-level evidence is required.
- Review brief. (optional)
- Run settings, including quick, deep, or broad mode. (optional)

## Outputs

- Frozen literature corpus and canonical paper-flow ledger.
- Canonical accepted evidence ledger with raw adjudications, evidence lineage, and entailment verdicts.
- Human-readable evidence table, claim–evidence matrix, and grounded exact quotes.
- Deterministically built review and verified PDF report.
- Final reconciliation, quality summary, hashes, and delivery attestation.

## Workflow

1. Clarify decision scope, mode, paper-count preference, access constraints, figure density, OCR policy, figure-reuse policy, and presentation package.
2. Search distinct evidence facets, deduplicate version families, define atomic claims, record exclusions, and freeze one canonical corpus.
3. Acquire only permitted full text, classify access and parse quality, extract text/caption/OCR blocks, and adjudicate each claim against exact anchors.
4. Blindly re-check every displayed anchor for entailment and reject partial, scope-mismatched, or unsupported anchors.
5. Select source figures under the recorded reuse policy, verify figure entailment and crop quality, and distinguish evidential figures from illustrations.
6. Build narratives and report sections only from canonical verified claim and evidence identifiers.
7. Reconcile all artifacts, run blocking gates, verify source/destination hashes, and attest delivery.

## Decision rules

- Use an ordinary literature review when sentence-level grounding and deterministic verification are not required.
- Never treat an abstract as full-text evidence.
- A result is shippable only when every displayed claim has a resolvable supporting or contradicting anchor and every displayed anchor has a passing blinded entailment verdict.
- Treat a requested paper count as a planning preference until the retrieved unique-paper count is shown and confirmed.
- Broad mode includes every relevant unique record after deduplication and scope filtering unless the user explicitly narrows it.
- Separate preprint access routes from the version-of-record citation identity; do not combine a journal venue with a preprint DOI.
- Only entailment=yes with all scope-match axes true and no scope overreach may carry a displayed claim.
- Illustrative figures and generated summaries do not increase an evidence-support tier.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_7c7b4ca1f41d5f12` — Directly accessible publisher or repository PDFs: Use for full-text grounding when an unauthenticated request can retrieve the document.
- `rr_b2a50f2b313bcae6` — User-supplied PDFs: Use when the user provides lawful local full text.

### Secondary resources

- `rr_167434c4f9e24365` — Europe PMC: Use as a repository and metadata route for accessible biomedical full text.

### Fallback resources

- `rr_cd39c17aeccda0bd` — Abstract or provider-text context: Use only as discovery/context when verified full text cannot be obtained; never upgrade it to full-text evidence.

### Optional resources

- `rr_2528b91acd54f521` — EasyOCR: Figure text and provenance-box OCR.
- `rr_84fc2414e9ad3100` — PyTorch: Runtime for the OCR pipeline.
- `rr_dadd41f0e573a376` — marker-pdf: Optional difficult-PDF parser fallback.
- `rr_814bb48413bb279e` — qpdf: Optional PDF integrity check.

## Validation / QC

- Require a resolvable exact anchor and blinded entailment verdict for every displayed claim.
- Track retrieval route, parse quality, evidence disposition, and evidence lineage for every selected paper and decision.
- Reject anchors that are partial, scope-mismatched, unsupported, or overreaching.
- Verify selected figure relevance, reuse status, crop integrity, caption/OCR provenance, and scientific entailment.
- Require final cross-artifact reconciliation, PDF structural/visual checks, protected-input drift checks, and destination hash verification.
- Evidence requirement: Every displayed claim must be bound to at least one exact supporting or contradicting source anchor.
- Evidence requirement: Abstracts and provider-extracted text are discovery/context only, not full-text grounding.
- Evidence requirement: Label secondary descriptions and unretrieved pivotal results rather than presenting them as directly verified primary evidence.
- Evidence requirement: Convergence requires primary support from at least two independent studies and not merely multiple publications from one cohort.
- Evidence requirement: Preserve claim identifiers across evidence, figures, narratives, Markdown, and PDF.

## Failure handling

- Files on one compute worker are assumed to exist on another worker.
- Coordinator prose is treated as durable state and evidence disappears after context compaction.
- Transient retrieval failures are incorrectly classified as paywalls.
- The requested figure floor cannot be met under the selected reuse policy.
- A PDF opens but fails structural, provenance, or stale-build gates.
- Canonical sources or delivered artifacts change after verification.
- Fallback rule: When full text is unavailable, retain the record as abstract/provider context and label the evidence depth explicitly.
- Fallback rule: When all cited evidence for a conclusion is secondary or indirect, hedge the statement and label the limitation.
- Fallback rule: If the figure floor is infeasible under reuse-cleared-only policy, ask whether the user wants explicit user-directed inclusion without relabelling the rights status.
- Fallback rule: Use partial delivery only for a genuine closed-list blocker; do not relax reconciliation, drift, fatal-error, or destination-verification gates.

## Limitations

- The blinded entailment verdict is a within-agent coordinator re-check, not independent third-party adjudication.
- The workflow does not perform meta-analysis, effect-size pooling, or systematic-review protocol compliance.
- Unretrieved full text cannot support sentence-level claims and must remain abstract/provider context.
- Illustrative and generated figures have no evidential weight.
- Observed population-specific evidence does not establish exclusivity in untested populations.

## Important domain-specific rules

- Facet-based search planning, version-family reconciliation, and immutable corpus freezing.
- Canonical evidence lineage from retrieval route through claim adjudication.
- Blind entailment re-check for every displayed source anchor.
- Rights-aware, entailment-aware figure selection with crop and OCR quality control.
- Deterministic cross-artifact reconciliation and hash-attested delivery.

## Portability boundary

- Biomni LiteratureSearch records and execution-trace ingestion. — migration action: `exclude_or_capability_map`
- Managed-machine shards, native coordinator packs, object-store exchange, and internal workspace/checkpoint paths. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage and Read media-output-check tool boundaries. — migration action: `exclude_or_capability_map`
- Biomni Bash background finalizer, run-state scripts, and internal Results delivery paths. — migration action: `exclude_or_capability_map`
- Dedicated platform PDF skill and Phylo-branded report requirements. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
