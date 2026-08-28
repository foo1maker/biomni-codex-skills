# Provenance policy

## Minimum lineage

Every material claim and every reusable artifact must be traceable through
four links: source, transformation, result, and limitation. The lineage must
retain a stable identifier, source URL, and version or retrieval time. Missing
metadata is recorded as `unknown` or `not_provided`; it is never inferred from a
filename.

## Canonical record

Use this portable record for claims, tables, figures, models, and derived
recommendations. Fields not applicable to a claim remain present with `null`.

```yaml
provenance_schema: "prov-v1"
claim_id: "C-0001"
claim_type: database_fact | paper_evidence | model_prediction | inference | recommendation
statement: "short statement or artifact description"
source_kind: database | paper | dataset | model | analysis | decision
source_id: "accession, DOI/PMID, model/checkpoint, or artifact identifier"
source_url: "https://..."
source_version: "release/checkpoint/software version or unknown"
retrieved_at: "ISO-8601 timestamp"
input_artifacts:
  - path_or_id: "..."
    sha256: "sha256:... or unknown"
method_or_model: "method name/version, or null"
transformation: "filter, normalization, fit, or reasoning step"
supporting_claims: ["C-0000"]
limitations: ["... or none recorded"]
status: observed | computed | predicted | inferred | recommended | blocked
```

For an artifact, `claim_id` is the artifact's lineage anchor. For a paper,
`source_id` should be a DOI, PMID, or stable repository identifier. For a
database record, use its accession or record key. For a model, include the
model family, checkpoint/release, and runtime versions. For an inference or
recommendation, `supporting_claims` is mandatory and `method_or_model` or the
decision rule must be stated.

## Compact inline syntax

When a full record cannot be displayed inline, use:

`[prov:v1; claim=C-0001; type=paper_evidence; id=doi:...; url=https://...; version=...; retrieved=...; input=sha256:...; limit=...]`

The inline form is a pointer, not a replacement for the full record. Do not
omit `id`, `url`, `version`, or `retrieved` merely to shorten a table.

## Lineage rules

- Preserve native identifiers and source URLs when aliases, mirrors, or
  normalized records are introduced.
- Give every transformation a name and retain the input artifact hash or
  identifier. Record units, assay context, split, thresholds, and software
  versions when they affect interpretation.
- Keep model selection, calibration, locked assessment, and deployment scoring
  distinguishable. Attach uncertainty, applicability status, or warning fields
  to predictions when available.
- Link inferences and recommendations to their supporting claims; do not cite a
  generated summary without exposing the underlying sources.
- Treat access, license, version, and retrieval time as provenance, not as
  optional prose.

### Provenance of derived rules

| Normalized source skill | Source section(s) used | Source URL |
|---|---|---|
| `literature-deep-review` | Step 2 — Acquire, parse, retrieve, and adjudicate; Step 3 — Blind-review anchors and build the report; Outputs | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace |
| `omics-dataset-retrieval` | Outputs; Step 15 — Assemble master catalog; Scientific Caveats | https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace |
| `adme-ml-modeling` | Required outputs; Run configuration; Scientific caveats | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace |
| `experimental-design-statistics` | Outputs; Standard Workflow; Common Issues | https://biomni.phylo.bio/skills/skill_40063c54fad9439a9acd36c76a25983f?section=marketplace |

## Verification of provenance

Before release, check that each referenced path exists, each important artifact
hash recomputes, each URL is syntactically present, and each identifier maps to
the stated resource. A provenance record can be complete while the underlying
claim remains weak; provenance is traceability, not validation.

## Related policies

- [Evidence policy](evidence_policy.md) defines claim classes and evidence
  boundaries.
- [Validation policy](validation_policy.md) applies provenance checks at input,
  intermediate, and output gates.
