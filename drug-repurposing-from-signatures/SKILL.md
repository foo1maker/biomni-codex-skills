---
name: drug-repurposing-from-signatures
description: "Use when a disease or phenotype gene signature should be compared with perturbation signatures to rank repurposing hypotheses with coverage, controls, robustness, and evidence gates."
---

# Drug repurposing from signatures

## Purpose

Turn a supplied or explicitly constructed up/down gene signature into a
reproducible, hypothesis-generating ranking of perturbations that reverse or
mimic the query state. Keep signature construction, mapping, scoring,
compound-level aggregation, controls, and evidence annotation separate.

## When to use

- The user has a differential-expression table, explicit up/down lists, or a
  disease/phenotype question from which a signature can be constructed.
- A perturbation-signature adapter and a common gene background are available.
- The result is intended for prioritization and follow-up, not a claim of
  clinical efficacy.

Do not use this route for a continuous expression-model analysis without a
defensible signature, or to score compounds absent from the selected library.

## Inputs

- Disease/phenotype scope and signature provenance, organism, up/down definition,
  and technical-gene exclusions.
- Differential-expression table or gene lists with identifiers and ranking
  fields; optional expected reverser/mimic controls.
- Perturbation library, compound metadata, cell context, dose/time coverage,
  and any ortholog map, each with registry key, version, access/license status,
  and retrieval time.
- Intended evidence depth and annotation scope (mechanism, phase, targets,
  structure, or trial novelty).

## Workflow

1. Prefer a supplied signature. Otherwise resolve the disease/phenotype and
   construct a documented up/down pair; normalize symbols, remove only listed
   technical genes, and retain unresolved identifiers.
2. If species differ, map orthologs conservatively, remove ambiguous mappings,
   reject signatures with fewer than five genes per required direction, and
   freeze one shared background.
3. Quantify library coverage in each direction. Warn below approximately 85%
   mapped coverage and retain the unmapped list.
4. Score two-sided reversal and optional mimic overlap with size correction,
   permutation/null calibration, and FDR. Aggregate signatures to compound
   statistics while retaining cell line, dose, and time context.
5. Rank deterministically. Tier-1 requires at least two independent reversing
   signatures; Tier-2 has one. Re-run sensitivity/context and promiscuity checks.
6. Test positive controls using both expected direction and FDR significance,
   cross-check with an independent enrichment statistic, and explain failures.
7. Annotate mechanisms and clinical metadata only from returned records. Ground
   top candidates in verifiable sources and label the result as an inference.

## Resource selection

- `primary`: a registry-recorded perturbation signature resource such as
  `LINCS L1000 disease and perturbation signatures` with a compatible query
  space and access status.
- `secondary`: `Broad Repurposing Hub`, `ChEMBL`, or `TxGNN` only for explicit
  metadata/mechanism annotation and cross-checking; do not merge their scores
  with the primary ranking without a declared transformation.
- `fallback`: a documented local/single-drug enrichment route or ortholog table
  only when the primary is unavailable. Label the changed endpoint and do not
  compare its scores as if they came from the primary library.

Use [resource selection policy](../_shared/resource_selection.md). Unknown
license or access status remains `UNKNOWN` and requires a manual decision.

## Decision rules

- **DRS-1:** Confirm ambiguous disease matches before constructing a signature;
  never mix incompatible up/down definitions.
- **DRS-2:** Use a common gene background and preserve reversal and mimic p/FDR
  values with each control verdict.
- **DRS-3:** Propagate one canonical integer rank to all tables, figures, and
  evidence statements; never sort downstream artifacts independently.
- **DRS-4:** Require direction plus FDR significance for a strong control verdict.
  Failed or weak controls make the candidate slate exploratory.
- **DRS-5:** A high connectivity score is an in-silico hypothesis, not evidence
  of efficacy; disclose cell context, dose/time, specificity, and library scope.
- **DRS-6:** Do not assign a score to an unscorable compound or fabricate an
  unresolved compound identity.

## Validation

- Verify signature non-emptiness, organism, identifier mapping, direction, size,
  common background, library coverage, and all exclusions.
- Record size-corrected reversal, mimic, independent enrichment, permutation/null
  method, FDR procedure, tier, context, and control outcomes.
- Check canonical rank consistency and preserve unresolved compound identifiers.
- Verify every mechanism or literature claim against a retrievable record with a
  locator; label predictions, database facts, and recommendations separately.

See [evidence policy](../_shared/evidence_policy.md),
[provenance policy](../_shared/provenance_policy.md), and
[validation policy](../_shared/validation_policy.md).

## Failure handling

If the disease match, signature, orthologs, coverage, or control contract fails,
stop or return a visibly exploratory partial result. If a query endpoint is
unavailable, use only the declared fallback and start a separately labeled
run. Do not turn an empty query into an empty biological conclusion, and do not
describe the highest-scoring nonspecific candidate as effective. Record all
unresolved symbols, compound IDs, access gaps, and license decisions.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Signature and mapping manifest with common background, coverage, and exclusions.
- Ranked perturbation/compound table with reversal, mimic, FDR, tier, context,
  annotations, controls, and canonical rank.
- Robustness/control summary, literature evidence table, limitations, and a
  hypothesis-generating prioritization note.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`drug-repurposing-indication-expansion`](references/source_workflows/drug-repurposing-indication-expansion/WORKFLOW.md) — Rank existing drugs for a new disease indication by identifying perturbation signatures that reverse a disease transcriptional signature.
- [`signature-reversal-lincs`](references/source_workflows/signature-reversal-lincs/WORKFLOW.md) — Compare a query up/down gene signature with LINCS L1000 perturbation signatures to nominate, rank, validate, and annotate compounds that reverse or mimic the query state.

<!-- END MANAGED: SOURCE WORKFLOWS -->
