# Signature Reversal Lincs

Source workflow: `signature-reversal-lincs`  
Parent Claude Science skill: `drug-repurposing-from-signatures`

## Purpose

Compare a query up/down gene signature with LINCS L1000 perturbation signatures to nominate, rank, validate, and annotate compounds that reverse or mimic the query state.

## When to use

- Connectivity mapping and in-silico compound repurposing from a user-provided or constructed transcriptomic signature.

## Inputs

- User-provided up- and down-regulated HGNC gene lists or a differential-expression table, or a disease or phenotype name from which to construct a consensus signature. (required)
- Optional organism, signature size, cell context, and known-drug positive controls. (optional)

## Outputs

- Tier-1 reproducible compound ranking, Tier-2 single-signature hits, cell-context and known-drug views, and query-signature provenance.
- Machine-readable robustness summary, figures, and a report containing methods, ranked results, evidence, caveats, and next steps.

## Workflow

1. Use a supplied signature or construct a disease signature, normalize gene symbols, remove specified technical genes, and save provenance.
2. Resolve symbols to L1000 entities, report coverage in each direction, and retain unresolved genes.
3. Query the two-sided L1000 chemical-perturbation library for reversers and optionally mimickers; label any offline fallback path.
4. Resolve signature identifiers to compound metadata and aggregate cell-line, dose, and time signatures into compound-level statistics.
5. Tier and rank compounds using reproducibility, reversal strength, significance, and reverser specificity; evaluate positive controls and sensitivity or context stability.
6. Add mechanism, target, clinical phase, and live literature evidence, then compute mechanism-class enrichment.
7. Generate and validate figures and compile a report that exposes validation, references, and experimental next steps.

## Decision rules

- Prefer the user-provided signature; otherwise use a direct disease-signature pair or a recurrence- and consistency-filtered consensus appropriate to the source.
- Warn when L1000 gene coverage is below approximately 85 percent and report unresolved symbols.
- Tier-1 compounds require at least two independent reversing signatures; Tier-2 compounds have only one.
- Positive-control recovery, reproducibility tiering, and promiscuity or specificity control are mandatory; sensitivity and cell-context analyses are default-on.
- Exclude KEGG and limit optional MSigDB use to the Hallmark collection.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_418db836bb2f96d3` — SigCom LINCS API: Use for full-library two-sided L1000 connectivity scoring.

### Secondary resources

- `rr_6bd1c81c0c7340e0` — Broad Drug Repurposing Hub, ChEMBL, TxGNN, MSigDB Hallmark, and Reactome: Use for compound identity, mechanism, target, phase, repurposing context, and optional pathway annotation.

### Fallback resources

- `rr_de5a21c111540a97` — Local LINCS1000 single-drug-perturbation GMT sets: Use when the SigCom API is unreachable, and label the results as fallback connectivity.

### Optional resources

- `rr_40f53a0b744cf7fb` — LINCS L1000: Chemical-perturbation transcriptomic signatures for connectivity scoring.
- `rr_0c5cd85e984a45a9` — Broad Drug Repurposing Hub: Compound-name, mechanism, target, phase, and structure metadata.
- `rr_806458b94ba25e08` — ChEMBL: Target and mechanism annotation.
- `rr_eeeb26e3f45f34d1` — TxGNN: Repurposing predictions and compound-name mapping.
- `rr_7d299005f244dfdd` — seaborn and matplotlib: Connectivity, robustness, mechanism, and validation figures.
- `rr_2428e5633fe662f5` — PIL and numpy: Programmatic non-blank figure validation when visual inspection is unavailable.

## Validation / QC

- Verify that a constructed signature is non-empty and plausible, print recurrent genes, and retain the technical-gene removal list.
- Classify each known positive-control compound by actual membership in both ranking tiers rather than inferring absence.
- Validate figures visually or by file size, dimensions, and non-white-pixel ratio, and record which check was used.
- Evidence requirement: Build candidate references from live literature-search records and verify every cited claim; do not hardcode reference lists.
- Evidence requirement: Report signature provenance, L1000 coverage, unresolved genes, reproducibility tier, positive-control recovery, specificity, sensitivity or context robustness, and commercial-use review flags.

## Failure handling

- SigCom may return a non-success response or malformed or empty data.
- Low gene coverage, unresolved compound names, or unavailable known positive controls reduce interpretability.
- Missing-value values can crash tabular report rendering if they are not converted to safe strings.
- Fallback rule: Switch to local single-drug-perturbation GMT enrichment when SigCom is unreachable and explicitly label the fallback.
- Fallback rule: Retry transient null gene-metadata responses with bounded backoff before treating a gene as missing.
- Fallback rule: Retain unresolved BRD identifiers and qualify them when compound names cannot be resolved.

## Limitations

- Outputs are in-silico repurposing hypotheses, not therapeutic recommendations, and require experimental validation.
- L1000 is dominated by cancer cell lines and can under-represent compounds dependent on tissue, metabolism, or other contexts; ranks are ordinal rather than absolute clinical evidence.
- Current source terms do not constitute commercial clearance; specified resources require case-specific license review.

## Important domain-specific rules

- Signature construction and provenance, L1000 mapping and coverage QC, two-sided connectivity, compound aggregation, tiering, specificity, positive-control, sensitivity, and context robustness logic.
- Live evidence grounding, mechanism annotation, commercial-use review flags, and explicit fallback labeling.

## Portability boundary

- Biomni data-lake asset resolution, LiteratureSearch execution trace, predict_admet_properties, and internal script or reference-file paths. — migration action: `exclude_or_capability_map`
- Skill-loaded pdf-report-generation, Phylo-branded report, infographic requirement, platform result paths, and media-output-check orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
