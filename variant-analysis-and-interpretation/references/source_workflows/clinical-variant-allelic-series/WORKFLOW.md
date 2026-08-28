# Clinical Variant Allelic Series

Source workflow: `clinical-variant-allelic-series`  
Parent Claude Science skill: `variant-analysis-and-interpretation`

## Purpose

Build a position-resolved catalogue for one human gene by reconciling clinically observed ClinVar alleles with curated CIViC evidence, assigning actionability and mechanism annotations, adding UniProt domain context, and producing auditable tables, adaptive figures, and a report.

## When to use

- Retrieve the full ClinVar and CIViC record sets for one human gene and reconcile them into a single allele table.
- Create actionability and mechanism summaries with position- and protein-domain-aware visualizations.

## Inputs

- One HGNC human gene symbol. (required)
- Optional NCBI email and API-key configuration to raise the E-utilities request rate. (optional)
- Optional UniProt accession override when gene-symbol resolution is ambiguous. (optional)
- A report configuration containing title, narrative blocks, key-allele table, limitations, next steps, and grounded references. (required)

## Outputs

- A full ClinVar catalogue and classification/consequence summary for the requested gene.
- CIViC variant and long-form evidence tables, including a flag for single-variant evidence.
- A master allelic-series table with one row per allele, reconciled position, actionability tier, and mechanism, plus summary statistics and join diagnostics.
- Adaptive protein lollipop and actionability-landscape figures, plus evidence and therapy figures when their source evidence exists.
- A figure manifest recording which figures were produced and the UniProt metadata used.
- A self-validated human-readable allelic-series report.

## Workflow

1. Fetch all matching ClinVar UIDs and batched DocumentSummary records, preserving the tripartite germline, oncogenicity, and clinical-impact classifications.
2. Download CIViC nightly variant, molecular-profile, and clinical-evidence tables; retain single-variant profiles as the primary allele-level evidence and tabulate complex profiles separately.
3. Reconcile ClinVar and CIViC using Variation ID, normalized protein change, combined evidence, and categorical descriptors; derive positions and require zero position discrepancies.
4. Resolve the reviewed human UniProt entry and generate single- or two-panel protein-context and evidence figures according to the available hotspot and evidence structure.
5. Write a grounded report configuration and build the report from the reconciled data, adaptive figures, documented limitations, and real references.

## Decision rules

- Use the ClinVar UID, equivalent to the numeric part of the VCV accession, as the Variation ID bridge to CIViC; do not use measure_id.
- Derive residue position from the variant name, using the first residue number after a letter in the HGVS protein change; never copy a position from a linked record.
- Read molecular consequence from the ClinVar DocumentSummary level and use the first token of a comma-separated top-level protein change.
- Normalize string-encoded booleans explicitly and treat the string nan as missing before tiering or mechanism inference.
- Interpret Tier 3 as not evidenced in the assembled sources, not as benign or safe.
- Prefer UniProt Domain and Transmembrane features over broad Region or Topological-domain spans for the protein track.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_ab2e070b7ab4fb1d` — ClinVar NCBI E-utilities esearch and esummary records.: Building the comprehensive catalogue of observed variants and asserted germline or somatic significance.
- `rr_d0d71614a57480a9` — CIViC nightly TSV exports.: Adding curated therapy, prognosis, and diagnostic evidence and bridging it to individual alleles.

### Secondary resources

- `rr_d28f5db1ff4f7188` — Reviewed human UniProt records and structural features.: Adding protein length, domain, and transmembrane context to position-resolved figures.

### Fallback resources

- `rr_80cdaa49c213d80a` — ClinVar-only allelic series with empty CIViC artifacts.: The gene is absent from CIViC; skip CIViC-dependent figures and retain the catalogue and report.
- `rr_4b04c203c86c8c65` — Allele-position figures without a UniProt domain track, with protein length inferred from the data.: UniProt resolution fails and no override resolves it.

### Optional resources

- `rr_5e71673aea7cf480` — ClinVar: Comprehensive variant records, identifiers, molecular consequences, conditions, and asserted classifications.
- `rr_fda4e5c79c7315b7` — CIViC: Curated clinical evidence connected to ClinVar Variation IDs through nightly exports.
- `rr_66e05683356bf73d` — UniProt: Reviewed human protein accession, length, domains, and transmembrane features for figure context.
- `rr_f6ca7235b953bedf` — Python: Retrieve, reconcile, tier, visualize, and report the allelic series through the documented package scripts.

## Validation / QC

- Assert zero residue-position discrepancies after cross-source reconciliation.
- Keep single-variant CIViC profiles as the primary allele-level evidence and tabulate multi-variant profiles separately.
- Cap and deterministically space lollipop labels so hotspot clusters remain readable.
- Use ASCII-clean text for the generated PDF.
- Evidence requirement: Ground functional classes, key alleles, therapies, and report references in retrieved literature; do not invent citations or compute functional class de novo.
- Evidence requirement: Disclose that CIViC evidence is deeper for recurrent hotspots and that evidence absence is not benign evidence.
- Evidence requirement: State that actionability can depend on lineage and that the assembled series is tumor-type-agnostic.

## Failure handling

- Using ClinVar measure_id instead of the Variation-ID UID breaks the CIViC join.
- Taking residue position from a linked record instead of the variant name creates silent cross-source mismatches.
- CSV string conversion of booleans and missing values can silently collapse tier assignments if not normalized.
- Transient NCBI E-utilities requests can fail during batched retrieval.
- Fallback rule: Retry transient E-utilities summary failures three times with backoff.
- Fallback rule: When CIViC has no record for the gene, emit empty CIViC files, skip CIViC-dependent figures, set Tier 1 count to zero, and continue the build and report.
- Fallback rule: When UniProt resolution fails, omit the domain track and infer protein length from the allele data, or use a user-supplied accession override.

## Limitations

- The workflow does not call variants, interpret a specific patient genotype, replace guideline-based clinical interpretation, or compute functional classes de novo.
- Identifier resolution is human-only and non-human genes are outside scope.
- The allelic series is tumor-type-agnostic even though therapeutic actionability can be lineage-dependent.
- Rare alleles can be under-annotated because CIViC coverage is deepest for recurrent hotspots.

## Important domain-specific rules

- Reconcile heterogeneous sources with an ordered set of exact-ID, normalized-name, combined, and categorical keys, then assert cross-source positional consistency.
- Normalize Boolean and missing-value strings after CSV round-trips before decision logic.
- Make figures adaptive to hotspot concentration and available evidence, with an explicit manifest of generated artifacts.
- Preserve graceful degradation by emitting schema-valid empty evidence artifacts and continuing source-independent outputs.

## Portability boundary

- Package-local ClinVar, CIViC, reconciliation, figure, and report scripts plus their fixed command sequence. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, GenerateImage, pdf-report-generation, and in-environment helper verification calls. — migration action: `exclude_or_capability_map`
- Phylo report styling and the /mnt/shared-workspace, /workspace, and /mnt/results runtime path conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
