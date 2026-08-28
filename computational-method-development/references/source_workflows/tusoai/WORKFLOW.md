# Tusoai

Source workflow: `tusoai`  
Parent Claude Science skill: `computational-method-development`

## Purpose

Run the bundled TusoAI system inside Biomni as a durable scientific optimization service that builds a faithful evaluator, honors a global budget, survives lifecycle events, and returns a revalidated method.

## When to use

- Build and validate a benchmark evaluator for method development.
- Search for improved computational methods under an authorized global cost and wall-clock budget.
- Select, inspect, cleanly rebuild, and revalidate finalist methods before delivery.

## Inputs

- A method source repository or baseline implementation. (required)
- An evaluator defining the exact metric and execution contract. (required)
- Scientific, API, shape, ordering, seed, privacy, and leakage constraints. (required)
- A global cost and wall-clock budget, or the documented default execution policy. (optional)

## Outputs

- Final method source and exact patch.
- Environment, commands, task specification, source and evaluator hashes, and launch configuration.
- Search history, final validation table, resource and cost summary, accepted-method rationale, and remaining uncertainties.

## Workflow

1. Fingerprint source, evaluator, and task identity and resume an existing matching run before creating a new one.
2. Build the smallest faithful evaluator, repeat the baseline at least three times, measure score noise, runtime, and memory, and prove candidate edits can affect the score.
3. Freeze the scientific and evaluator contract before optimization and encode every user constraint into the task context.
4. Checkpoint history and state throughout bounded search epochs and continue from the same history while the authorized budget remains.
5. Inspect finalist code for leakage, evaluator exploitation, hidden labels, nondeterminism, brittle imports, and unnecessary complexity.
6. Rebuild finalists from protected source and rerun baseline and finalists over multiple repetitions and relevant folds, seeds, or full datasets.

## Decision rules

- Do not modify scoring code, splits, labels, held-out data, benchmark definitions, test expectations, validation strength, sample counts, seeds, precision, or convergence criteria to improve a score.
- Candidate methods may not read hidden labels, evaluator internals, expected outputs, or network data.
- Continue while authorized budget remains; a plateau, one good result, or a failed worker is not completion.
- Accept an improvement only when it exceeds measured evaluator noise and survives clean revalidation.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_4eb004f8de831f58` — Protected baseline source and frozen evaluator: Running baseline audits, candidate evaluation, and clean finalist reconstruction.

### Secondary resources

- `rr_baf82a18b5b554e7` — External or orthogonal training-safe data: It is not the original seed repository, not internal baseline seeding, and can be staged without changing the evaluator contract.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- None declared in the normalized source record.

## Validation / QC

- Require a reproducible baseline, noise-informed minimum improvement, candidate reachability, and known timeout and memory envelopes before search.
- Record hashes rather than only paths for source, evaluator, task bundle, and final artifacts.
- Revalidate finalists from clean protected source under repeated runs and relevant folds, seeds, or full datasets.
- Evidence requirement: Preserve baseline repetitions, score-noise estimates, resource measurements, candidate history, and final validation results.
- Evidence requirement: Document the exact accepted-method rationale and unresolved uncertainties alongside reproducible commands and hashes.

## Failure handling

- The evaluator cannot faithfully exercise the method or produce a stable finite metric.
- A candidate exploits evaluator internals, hidden data, or benchmark artifacts.
- A selected candidate fails clean reconstruction, reproducibility, or integrity checks.
- Fallback rule: When the primary complexity-aware finalist fails revalidation, use the raw best or next-best reproducible candidate only after the same integrity checks.
- Fallback rule: When process state is lost but hashes match, resume from durable state and shared history rather than restarting search.

## Limitations

- None declared in the normalized source record.

## Important domain-specific rules

- Freeze evaluator and scientific contracts before optimization and protect held-out data and scoring logic.
- Repeat the baseline to quantify evaluator noise, runtime, and memory before defining an improvement threshold.
- Inspect candidates for leakage and brittleness, then reconstruct and revalidate finalists from protected source.
- Preserve exact specifications, hashes, history, resource use, validation evidence, and uncertainties for reproducibility.

## Portability boundary

- The bundled TusoAI system, its optimize and selector interfaces, and TusoAI-specific task bundle. — migration action: `exclude_or_capability_map`
- Biomni-managed leader and follower jobs, machine lifecycle rules, callback watchdog behavior, and multi-machine shared search. — migration action: `exclude_or_capability_map`
- TusoAI process-local budget variables, cluster-deadline arithmetic, and platform-specific budget fan-out. — migration action: `exclude_or_capability_map`
- Biomni-specific persistent directory tree, managed checkpoints, helper scripts, machine IDs, job handles, and exact file paths. — migration action: `exclude_or_capability_map`
- TusoAI-specific complexity-aware selector implementation and history-file format. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
