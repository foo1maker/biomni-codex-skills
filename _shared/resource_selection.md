# Resource selection policy

## Role contract

Every external resource used by a workflow has exactly one role for that run:

- `primary`: the preferred resource for the stated task and input contract.
- `secondary`: a comparable reference or independent source used when the
  primary is unavailable, insufficient, or needed for cross-checking.
- `fallback`: a deliberately lower-scope, lower-confidence, synthetic, cached,
  or manual route used only under a named condition.

Role is contextual. The same resource may be primary for one endpoint and
secondary for another, but the run record must say which role was chosen.

## Selection procedure

1. Define the task, endpoint or estimand, input representation, access needs,
   date/version boundary, and acceptable evidence level.
2. Inspect candidate identity, version or release date, stable identifier,
   source URL, schema, units, coverage, access restrictions, and license terms.
3. Choose the primary only if it satisfies the contract. Record why it was
   preferred and what was checked.
4. If the primary fails, test a declared secondary against the same contract.
   Do not substitute a resource merely because it is convenient.
5. Use a fallback only when its trigger is recorded. State the changed scope,
   estimand, representation, confidence, or evidence class before use.
6. If no candidate satisfies the contract, stop with `BLOCKED_RESOURCE` or
   return a clearly marked partial result.

## Required selection record

Each selected resource must be represented by a record equivalent to:

```yaml
resource_role: primary | secondary | fallback
name: "stable human-readable name"
identifier: "accession, DOI, release, registry key, or not_provided"
version_or_retrieved_at: "version/date or unknown"
source_url: "https://..."
selection_condition: "why this role was selected"
access_status: open | controlled | login_required | unavailable | unknown
license_status: permitted | restricted | unknown
input_contract: "format, units, endpoint, or query scope"
substituted_for: "identifier or null"
disclosure: "what changed relative to the preferred route"
```

`unknown` is an auditable state, not permission to proceed as if the field were
known. A resource with unknown terms may be cataloged, but use requires an
explicit license/access decision. Keep the original source identifier when a
mirror, cache, or normalized alias is used.

## Derived selection rules

- Prefer a user-provided or directly authoritative resource when its endpoint,
  assay signature, units, and labels are compatible.
- Keep benchmark, cached, or synthetic resources visibly separate from primary
  observations; a synthetic smoke test cannot supply headline empirical
  performance.
- Use a tiered discovery strategy when repositories differ in search coverage,
  metadata completeness, rate limits, or access behavior; preserve repository,
  accession, and access-status provenance.
- Do not conclude that a landscape is empty until the documented query scope,
  status filters, and any required broadening have been recorded.
- A fallback may change the question. If it changes the endpoint, population,
  data representation, or evidence class, start a new labeled run rather than
  combining results with the primary run.

### Provenance of derived rules

| Normalized source skill | Source section(s) used | Source URL |
|---|---|---|
| `adme-ml-modeling` | Inputs; Default demonstration (real public benchmark); Operating rules | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace |
| `omics-dataset-retrieval` | Repository Coverage Map; Known API Limitations and Workarounds; Step 15 — Assemble master catalog | https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace |
| `clinicaltrials-landscape` | Standard Workflow; Common Issues; Outputs | https://biomni.phylo.bio/skills/skill_546a8868863342c093eb2570dcd538f4?section=marketplace |
| `molecular-property-admet` | Standard Workflow; Common Issues; Outputs | https://biomni.phylo.bio/skills/skill_c30c5d550c6b897400e363e8d1664a3c?section=marketplace |

## Selection disclosure

Place the chosen role, identifier, version/date, source URL, and substitution
reason next to the result or in its manifest. Never report a secondary or
fallback result under the primary resource's name.

## Related policies

- [Provenance policy](provenance_policy.md) defines identifiers, versions,
  URLs, hashes, and lineage fields for the selection record.
- [Failure handling policy](failure_handling.md) defines disclosed substitution
  and blocked-resource states.
