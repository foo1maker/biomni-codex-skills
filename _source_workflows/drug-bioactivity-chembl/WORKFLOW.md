# Drug Bioactivity Chembl

Source workflow: `drug-bioactivity-chembl`  
Parent Claude Science skill: `small-molecule-modeling-and-safety`

## Purpose

Resolve a compound or target in ChEMBL and produce a provenance-tracked molecular potency, off-target, selectivity, and separate cellular-activity profile.

## When to use

- Profile a compound's IC50, Ki, Kd, and optional functional-potency measurements across targets.
- Quantify on-target versus off-target selectivity windows.
- Rank compounds for a target-centric ChEMBL query.

## Inputs

- Compound name, synonym, ChEMBL identifier, or SMILES. (optional)
- Target ChEMBL identifier for target-centric mode. (optional)
- Optional target restriction and measurement-type set. (optional)

## Outputs

- Per-target potency aggregates containing median, IQR, range, geometric mean, count, and measurement type.
- Record-level curated bioactivity with exclusion and censoring flags.
- Fold-selectivity table relative to the designated primary target or targets.
- Separate cellular-activity summary when cell-line records are present.
- Potency, selectivity, data-composition, and optional cellular figures.

## Workflow

1. Resolve the compound, print all candidate molecules, confirm the intended entity, and exclude close analogues.
2. Retrieve paginated ChEMBL activity records for the selected molecule or target and requested measurement types.
3. Build a tidy activity frame and classify biochemical, cellular target-engagement, and antiproliferation assays.
4. Score candidate primary targets from the observed data, review them against known mechanism, and override explicitly when required.
5. Apply the documented exact-nM Standard filter and reconcile retained, censored, unit-excluded, and transcription-error records.
6. Aggregate each target and measurement type separately using median, IQR, range, geometric mean, record count, and study count.
7. Calculate primary-to-off-target selectivity, retaining censored measurements as lower or upper bounds and flagging single-point targets.
8. Compare the primary median with a literature-informed sanity band and investigate large discrepancies.
9. Summarize antiproliferation measurements separately from molecular-target potency.
10. Save the curated records, aggregates, selectivity tables, cellular summaries, and figures.

## Decision rules

- Prefer an exact compound-name match; otherwise choose the candidate with the highest development phase and fail loudly when none is found.
- Never merge cellular growth measurements with biochemical molecular-target potency.
- Human-review the primary-target candidates and explicitly record whether the primary assignment was automatic or manual.
- Keep exact nM measurements, exclude non-nM and transcription-error records, and retain censored values as bounds.
- Do not average across measurement types.
- Use median and range as headline potency rather than the mean.
- Treat any target supported by a single measurement as provisional.
- If the primary median falls far outside a literature-informed band, investigate units and target assignment before reporting.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_8984eae6e7f66a4e` — ChEMBL public REST API: Measured molecular and cellular bioactivity is required.

### Secondary resources

- `rr_4691613adbc7e086` — Primary literature linked to compound mechanism, discovery potency, and selectivity liabilities: Primary-target assignment and sanity checking require independent grounding.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Molecule resolution, assay and target metadata, and measured IC50, Ki, Kd, EC50, and potency records.
- `rr_178baeb270b10c13` — pandas: Bioactivity table curation and aggregation.
- `rr_78609fcc8c7914c6` — numpy: Numerical aggregation.
- `rr_665edbde6fc3cd1c` — matplotlib: Potency and selectivity figures.
- `rr_08705214f971900b` — support-weighted score: Ranks target records from the observed bioactivity evidence before human review.

## Validation / QC

- Confirm that the selected molecule is the intended compound and record excluded analogues.
- Require the provenance equation raw equals clean plus transcription-error, non-nM, and censored exclusions.
- Inspect candidate primary targets and reconcile ChEMBL records that split one biological target.
- Run the literature-informed primary-potency sanity check before reporting.
- Keep ChEMBL identifiers and numerical ranges intact in tables.
- Evidence requirement: Every redistributed activity table must retain ChEMBL provenance and the documented CC BY-SA 3.0 attribution and share-alike obligation.
- Evidence requirement: Report record counts, measurement types, assay context, target identifiers, and study support for potency aggregates.
- Evidence requirement: Verify every literature-derived numerical claim against a returned record and never fabricate citations.
- Evidence requirement: Label optional ADMET results as predicted rather than measured.

## Failure handling

- The wrong compound or a close analogue is selected.
- ChEMBL splits one biological target across multiple target records and the primary is misassigned.
- Units or measurement types are mixed during aggregation.
- A selectivity claim is built on a single off-target measurement.
- ChEMBL is unavailable or returns only cellular data.
- Fallback rule: When multiple molecules match, list candidates and require an intended entity instead of guessing.
- Fallback rule: When only cell-line data exist, report the cellular section and omit molecular selectivity.
- Fallback rule: Retry transient ChEMBL failures; if the service remains unavailable, report failure rather than inventing data.
- Fallback rule: Treat censored off-target values as selectivity bounds.

## Limitations

- The workflow reports measured ChEMBL bioactivity and does not run docking, QSAR, or machine-learning potency prediction.
- ChEMBL activity is heterogeneous across assays and may split a biological target into several records.
- Single-record target potency and selectivity are provisional.
- Derived ChEMBL datasets inherit attribution and share-alike obligations.

## Important domain-specific rules

- Compound disambiguation and analogue-exclusion gate.
- Assay-class separation between biochemical, target-engagement, and antiproliferation evidence.
- Exact-unit curation with a reconciling provenance ledger.
- Measurement-type-specific robust potency aggregation.
- Human-reviewed primary-target assignment and censored selectivity bounds.

## Portability boundary

- Biomni ExecuteCode persistence and packaged helper-library orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and execution-trace reference collection. — migration action: `exclude_or_capability_map`
- Biomni pharmacology predict_admet_properties tool identifier. — migration action: `exclude_or_capability_map`
- Biomni Read media checks, pdf-report-generation conventions, Phylo branding, and /mnt/results paths. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
