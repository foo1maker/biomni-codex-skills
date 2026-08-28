# Failure handling policy

## Failure classes

Classify the first actionable failure as one of: `INPUT`, `ACCESS`, `RESOURCE`,
`DATA`, `METHOD`, `RUNTIME`, `OUTPUT`, `LICENSE`, or `INTERPRETATION`. Record the
stage, condition, impact on the question, attempted fixes, and the next safe
action. Preserve the failing input identifier and any partial artifacts.

## Control flow

1. Stop at the failed validation gate; do not continue on an invalid contract.
2. Make the failure visible in the run state and preserve non-empty partial
   outputs with their provenance.
3. Apply only a predeclared fallback whose role and trigger match the resource
   selection record. Re-run the affected gate after substitution.
4. If the fallback changes the endpoint, population, representation, estimand,
   evidence class, or license/access basis, begin a new labeled run and do not
   merge its results with the original.
5. If no safe recovery exists, return `BLOCKED` with the exact missing condition.

## Non-silent fallback rules

- Never drop invalid input rows without an exclusion status and count.
- Never present synthetic, cached, or exploratory output as measured, primary,
  or confirmatory evidence.
- Never delete an influential study or observation only because it changes the
  result; investigate it and show the sensitivity.
- Never turn an unavailable resource into an empty landscape conclusion. Record
  the failed query, scope, access condition, and any documented broadening.
- Never bypass a license or controlled-access condition. Mark the resource
  unavailable or obtain an explicit authorization before use.
- Never conceal a model-domain warning, failed uncertainty check, underpowered
  design, or suppressed model.

## Recovery record

Use a record equivalent to:

```yaml
failure_id: "F-0001"
class: INPUT | ACCESS | RESOURCE | DATA | METHOD | RUNTIME | OUTPUT | LICENSE | INTERPRETATION
stage: "input_gate or named workflow stage"
condition: "observable failure"
impact: "what cannot be claimed"
attempted_fixes: ["... or none"]
fallback_role: primary | secondary | fallback | none
fallback_trigger: "declared condition or null"
disclosure: "what changed, if anything"
next_action: "repair, request, retry, or stop"
provenance: "prov-v1 claim or artifact reference"
status: open | recovered | blocked
```

Retry only when the cause is transient and the retry is bounded. A retry does
not authorize broader scope, a different endpoint, or a silent resource swap.

## Derived handling rules

- Stop on mixed assay/unit conflicts, incompatible effect measures, unresolved
  inputs, or unsupported analysis classes; use a declared alternative only when
  its limitations are carried forward.
- If a real public benchmark or authoritative source cannot be retrieved,
  disclose the failure and keep any synthetic or offline smoke test separate.
- Preserve access restrictions, repository gaps, unrecognized dates, and
  cross-repository duplicate decisions instead of deleting the associated
  records.
- If a model is suppressed by an event, replication, or applicability gate,
  report the suppression and its reason; do not substitute an unjustified
  estimate.
- When a search returns no records, broaden filters only according to the
  declared workflow and report the original and broadened scope.

### Provenance of derived rules

| Normalized source skill | Source section(s) used | Source URL |
|---|---|---|
| `adme-ml-modeling` | Operating rules; Default demonstration (real public benchmark); Required outputs; Scientific caveats | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace |
| `evidence-synthesis-meta-analysis` | 0. Confirm the analysis is in scope; 2. VERIFY every number and citation (non-negotiable); Scientific caveats | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace |
| `omics-dataset-retrieval` | Known API Limitations and Workarounds; Scientific Caveats; Step 15 — Assemble master catalog | https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace |
| `real-world-evidence` | Common Issues; Interpretation Guidelines; Suggested Next Steps | https://biomni.phylo.bio/skills/skill_ea1afce75ae2b4fc030bbe787188145e?section=marketplace |
| `clinicaltrials-landscape` | Common Issues; Standard Workflow; ⚠️ CRITICAL — DO NOT: | https://biomni.phylo.bio/skills/skill_546a8868863342c093eb2570dcd538f4?section=marketplace |

## Completion rule

A workflow is complete only when every open failure is either recovered and
revalidated or explicitly carried as a limitation. “No error was displayed” is
not evidence that the failure was handled.

## Related policies

- [Resource selection policy](resource_selection.md) defines declared fallback
  roles and substitution conditions.
- [Validation policy](validation_policy.md) defines the gates that open or
  close a failure state.
- [Provenance policy](provenance_policy.md) defines the lineage for partial,
  recovered, and blocked artifacts.
