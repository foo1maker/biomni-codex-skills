---
name: computational-method-development
description: "Use when a computational method must be improved or compared against a frozen evaluator with leakage controls, reproducible validation, and resource-aware iteration."
---

# Computational method development

## Purpose

Develop, audit, and simplify a computational method against a fixed scientific
contract. This skill preserves benchmark integrity, evaluator noise estimates,
controlled experiments, and clean finalist reconstruction. It is a method
development and evaluation contract, not an autonomous execution service.

## When to use

- A baseline and a measurable endpoint exist, or a development benchmark can be
  explicitly scaffolded.
- You need controlled ablations, leakage auditing, robustness checks, or a
  reproducible method package.
- A claim must distinguish model selection from locked assessment.

Do not proceed to optimization when the evaluator cannot exercise the method or
when the scientific endpoint, protected data, or split contract is unresolved.

## Inputs

- Scientific problem, input/output contract, metric direction, constraints, and
  failure costs.
- Baseline implementation and evaluator, including splits, labels, allowed
  data, seeds, stopping rules, and resource limits.
- Optional auxiliary datasets, representations, models, or literature with
  identifiers, versions, license/access status, and leakage assessment.
- Authorized compute/cost budget and a durable experiment record format.

## Workflow

1. Fingerprint the task, evaluator, baseline, and protected inputs. Freeze the
   scientific and evaluation contracts before changing the method.
2. Run the baseline repeatedly (at least three repetitions when feasible), plus
   a no-signal or shuffled sanity candidate. Quantify score noise, runtime,
   memory, and evaluator sensitivity.
3. Write a method blueprint. For each candidate record one hypothesis, one
   isolated change, primary and guardrail metrics, diagnostics, and an ablation.
4. Iterate within the declared budget. Checkpoint hypotheses, inputs, outputs,
   hashes, and decisions so an interrupted run can resume without changing
   scope.
5. Inspect finalists for leakage, hidden-label access, evaluator exploitation,
   nondeterminism, brittle dependencies, and unnecessary complexity.
6. Rebuild finalists from protected source and re-run the baseline and finalists
   over repeated seeds, relevant folds, or the full assessment data. Select the
   simplest robust candidate whose gain exceeds measured evaluator noise.

## Resource selection

- `primary`: the user-supplied or otherwise frozen baseline and evaluator that
  satisfy the exact endpoint and split contract.
- `secondary`: a registry-recorded dataset, representation, or implementation
  adapter only after its provenance, license, coverage, and leakage risk are
  recorded. Keep auxiliary evidence separate from the primary assessment.
- `fallback`: a simpler or next-best reproducible candidate after the primary
  finalist fails clean reconstruction. A synthetic or cached smoke test cannot
  supply headline empirical performance.

Use [resource selection policy](../_shared/resource_selection.md) for role,
substitution, and access records. A missing or unknown resource record is an
access decision, not permission to guess or silently substitute.

## Decision rules

- **CMD-1:** Never alter scoring code, splits, labels, held-out data, benchmark
  definitions, test expectations, seeds, precision, or convergence criteria to
  improve a score.
- **CMD-2:** Candidate methods may not read hidden labels, evaluator internals,
  expected outputs, filenames, row order, timestamps, or network data that are
  unavailable under the contract.
- **CMD-3:** Promote a gain only when it exceeds measured evaluator noise and
  survives clean reconstruction and relevant robustness checks.
- **CMD-4:** Preserve primary and guardrail metrics, seed variance, resource use,
  ablations, and the reason for every substantive decision.
- **CMD-5:** A plateau, one favorable run, or a failed worker is not completion
  while the authorized budget or declared validation remains open.

## Validation

Apply the shared gates before each promotion:

- input contract, protected-hash, split, and license/access checks;
- baseline repetition, sanity-candidate degradation, finite metric, and known
  timeout/memory envelope checks;
- train-only preprocessing, leakage audit, subgroup review, repeated-seed or
  full-data revalidation, and ablation checks;
- output existence, schema, hashes, uncertainty/domain status, and exact
  reproduction instructions.

Record `PASS`, `WARN`, or `BLOCK`; do not convert a warning into a result claim.
See [validation policy](../_shared/validation_policy.md) and
[evidence policy](../_shared/evidence_policy.md).

## Failure handling

Stop if the evaluator is unstable, a candidate exploits protected information,
or a required artifact cannot be reconstructed. If state is lost but hashes
match, resume from the durable record. If the principal finalist fails, use the
declared simpler fallback only after the same integrity checks. Keep partial
artifacts and disclose the affected claim, rather than reporting a clean score.
Use [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Frozen benchmark/evaluation card and baseline noise report.
- Method blueprint, experiment/ablation ledger, candidate history, and leakage
  or robustness register.
- Final method source or patch, manifest, hashes, validation table, resource
  summary, accepted-method rationale, and unresolved uncertainties.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`tusoai`](references/source_workflows/tusoai/WORKFLOW.md) — Run the bundled iterative method system system inside platform-specific as a durable scientific optimization service that builds a faithful evaluator, honors a global budget, survives lifecycle events, and returns a revalidated method.
- [`tusoskill`](references/source_workflows/tusoskill/WORKFLOW.md) — Develop computational methods natively in platform-specific by freezing a trustworthy benchmark, gathering evidence, iterating across method families, auditing leakage and robustness, and packaging a reproducible method.

<!-- END MANAGED: SOURCE WORKFLOWS -->
