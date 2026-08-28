# Genetic Constraint Gating

Source workflow: `genetic-constraint-gating`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Triage candidate genes with gnomAD loss-of-function constraint, cross-version stability checks, deterministic risk and strategy flags, and ClinGen-grounded disease notes.

## When to use

- Loss-of-function constraint gating for candidate genes
- Cross-version gnomAD constraint comparison
- ClinGen-grounded disease-relationship annotation

## Inputs

- One or more gene symbols, supplied directly or in CSV or text form (required)
- Optional Ensembl identifiers or deprecated aliases for resolution (optional)

## Outputs

- A per-gene CSV containing constraint metrics, disease evidence, and deterministic interpretation fields
- Four constraint and comparison figures
- A PDF report

## Workflow

1. Resolve gene symbols, Ensembl identifiers, and deprecated aliases with MyGene.info.
2. Retrieve gnomAD loss-of-function constraint metrics and establish v2.1.1 as the primary basis with v4.1 as a comparison.
3. Apply the deterministic intolerance and cross-version-shift rules.
4. Retrieve ClinGen disease relationships and generate disease notes only when evidence is present.
5. Assign the stated knockout-tolerance, systemic-risk, and strategy fields and export auditable results.

## Decision rules

- Flag loss-of-function intolerance when gnomAD v2.1.1 LOEUF is below 0.35 or pLI is at least 0.90.
- Flag a version shift when the intolerance call changes between versions or the LOEUF difference is at least 0.15.
- Do not create a disease note when no ClinGen disease relationship is returned.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_984fb86361c56202` — gnomAD v2.1.1 loss-of-function constraint: Applying the primary intolerance gate

### Secondary resources

- `rr_eb9b4462a47436a3` — gnomAD v4.1 constraint: Checking stability of the primary v2.1.1 call across versions

### Fallback resources

- `rr_54b8ab47809f91fd` — Explicit unavailable constraint row: The gene remains unresolved or gnomAD returns no usable record after retry

### Optional resources

- `rr_35ab27166fd8a660` — gnomAD: Gene-level loss-of-function constraint metrics for versions 2.1.1 and 4.1
- `rr_81972d8ae31becf2` — MyGene.info: Gene-identifier and alias resolution
- `rr_a6b43a1163e33dfd` — ClinGen: Grounded gene-disease relationship notes
- `rr_f6ca7235b953bedf` — Python: Execution environment for the packaged analysis scripts

## Validation / QC

- Retain unresolved genes and genes without records as explicit rows with unavailable values.
- Verify that the reported flag basis is consistent with non-null primary metrics.
- Keep the primary version and the comparison version explicit in every cross-version interpretation.
- Evidence requirement: Ground disease notes in returned ClinGen evidence and state explicitly when no such evidence is present.
- Evidence requirement: Do not invent specific drug examples from constraint metrics.

## Failure handling

- A transient gnomAD query failure or null gene response can leave constraint fields unavailable.
- Deprecated or ambiguous identifiers can fail to resolve to a supported gene record.
- Fallback rule: Retry transient or null gnomAD responses with backoff; if retrieval still fails, keep the row with unavailable constraint values.
- Fallback rule: When ClinGen returns no evidence, record that absence rather than synthesizing a disease note.

## Limitations

- Constraint reflects depletion of germline heterozygous loss-of-function variation and is not clinical or target validation.
- Recessive, somatic, gain-of-function, and copy-number mechanisms may be underrepresented.
- Small genes can be underpowered, and SNV constraint may be unreliable at copy-number-prone loci.
- The constraint gate does not establish druggability or efficacy.

## Important domain-specific rules

- Version-explicit primary constraint gate with an independent cross-version shift flag.
- Deterministic interpretation fields that preserve their metric basis.
- Explicit unavailable rows and no-evidence states instead of silent dropping or invention.

## Portability boundary

- Bundled-script execution and orchestration instructions — migration action: `exclude_or_capability_map`
- Fixed /mnt/results output location — migration action: `exclude_or_capability_map`
- Phylo-branded PDF production and platform media checks — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
