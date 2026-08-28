# Validation policy

## Gate statuses

Use one of three statuses at each gate:

- `PASS`: the stated checks passed for the declared scope.
- `WARN`: the workflow can continue, but a limitation or reduced evidence
  strength must be carried into the result.
- `BLOCK`: a required contract or assumption failed; do not produce an
  apparently complete result.

Record the status, checks performed, counts, identifiers, versions, hashes, and
warnings. A pass means that the declared checks passed; it is not a guarantee of
scientific correctness or external validity.

## Input gate

Check before analysis:

- the path or record exists, is readable, and has the declared format;
- required columns, identifiers, units, labels, dates, and metadata are
  present and internally consistent;
- the representation matches the method (for example, count-scale data versus
  a documented transformed matrix);
- the endpoint, assay signature, population, organism, sample unit, and access
  scope are stated;
- duplicates, invalid records, censoring, missingness, and controlled-access
  constraints are quantified;
- the selected resource role, version/release, source URL, and license/access
  decision are recorded.

Block on a missing required input, incompatible units or endpoint, contradictory
labels/censoring, unresolved identifier mapping, or a representation that the
chosen method does not support. Do not silently coerce data to pass the gate.

## Intermediate gate

After each material stage, validate the declared contract and preserve a
machine-readable checkpoint. At minimum check:

- records retained, excluded, and deduplicated, with reasons and denominators;
- transformations, normalization, batch correction, split assignment, and
  parameters, including the input hash;
- no leakage from locked assessment data into feature selection, calibration,
  thresholds, or interpretation;
- model or statistical assumptions, replication unit, effect definition,
  uncertainty, and diagnostics;
- source identifiers and citations remain attached after joins, mirrors, or
  filtering;
- fallback or access substitutions are explicit and do not change the target
  question without a new run label.

Use `WARN` for declared exploratory or underpowered analyses. Use `BLOCK` when
the active method cannot answer the stated question, when a required diagnostic
is missing, or when a transformation would make the result uninterpretable.

## Output gate

Before handoff, verify that every declared output exists, is non-empty, parses,
and has the expected schema. Also check:

- the result count and denominator agree with the workflow log;
- invalid or excluded records are represented by status or an exclusion log;
- figures and tables correspond to the validated data and parameters;
- uncertainty, applicability/domain warnings, risk-of-bias, access, and
  limitations are visible where applicable;
- citations, source URLs, versions, identifiers, artifact hashes, and provenance
  records are present;
- no synthetic, cached, fallback, or exploratory result is presented as a
  primary measured or confirmatory result.

If an output check fails, preserve the partial artifacts and mark the run
`BLOCK` or `WARN`; never manufacture a clean output by dropping the failed
rows or diagnostics.

## Derived gate rules

- Lock prospective-style assessment before model selection and calibration;
  report uncertainty coverage and domain status with predictions.
- Verify compatible effect measures and source fields before synthesis, and
  inspect heterogeneity, influence, risk of bias, and leave-one-out results.
- Validate batch balance, covariate balance, sensitivity assumptions, and the
  consistency of exported design parameters before accepting an experimental
  design.
- Preserve cohort flow, denominators, event counts, and model-gating decisions
  in retrospective analyses; label exploratory multiplicity and phenotype
  limitations.
- Validate dataset relevance from visible title/summary evidence and retain
  native and normalized accessions through deduplication.

### Provenance of derived rules

| Normalized source skill | Source section(s) used | Source URL |
|---|---|---|
| `adme-ml-modeling` | Workflow; Operating rules; Required outputs | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace |
| `evidence-synthesis-meta-analysis` | 3. Run the meta-analysis; 5. Risk of bias; Outputs (saved under a run folder in /mnt/results/) | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace |
| `experimental-design-statistics` | Standard Workflow; Outputs; Common Issues | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace |
| `real-world-evidence` | Standard Workflow; Interpretation Guidelines; Common Issues | https://biomni.phylo.bio/skills/skill_ea1afce75ae2b4fc030bbe787188145e?section=marketplace |
| `omics-dataset-retrieval` | Step 14 — Relevance audit; Step 15 — Assemble master catalog; Outputs | https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace |

## Validation record

Store one record per gate using the provenance schema. Include `status`,
`checks`, `observed_counts`, `warnings`, `source_url`, `version_or_retrieved_at`,
and `artifact_hashes`. The record must make a failed check reproducible without
relying on an analyst's memory.

## Related policies

- [Provenance policy](provenance_policy.md) defines the validation record and
  artifact lineage.
- [Failure handling policy](failure_handling.md) defines the response to a
  failed gate and the rules for recovery.
