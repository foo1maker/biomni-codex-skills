# Tusoskill

Source workflow: `tusoskill`  
Parent Claude Science skill: `computational-method-development`

## Purpose

Develop computational methods natively in Biomni by freezing a trustworthy benchmark, gathering evidence, iterating across method families, auditing leakage and robustness, and packaging a reproducible method.

## When to use

- Create, improve, optimize, audit, or package a computational method against a frozen benchmark.
- Diagnose model, data, representation, objective, robustness, and efficiency bottlenecks through controlled experiments and ablations.
- Select, simplify, re-evaluate, and reproducibly package finalists.

## Inputs

- A scientific problem with an input and output contract, allowed data, target users, evaluation metric, constraints, and failure costs. (required)
- A benchmark or evaluator, or enough information to create a development-benchmark scaffold. (required)
- Optional external datasets, biological knowledge, pretrained representations, ontologies, literature, and packages whose provenance and leakage risk can be audited. (optional)

## Outputs

- A reproducible method package with source, manifest, benchmark card, method card, risk register, report, and reproduction command.
- Experiment, ablation, diagnostic, resource, provenance, and decision records.
- A final report describing problem, data, benchmark, baseline, final method, rationale, auxiliary resources, experiments, ablations, diagnostics, resource use, leakage audit, limitations, and exact reproduction commands.

## Workflow

1. Define and freeze the primary metric, direction, tie-breakers, validation protocol, guardrails, limits, protected files, leakage rules, seeds, baseline command, and candidate interface.
2. Run the baseline and a trivial sanity candidate and verify that the evaluator distinguishes real signal from constant, shuffled, or leakage-prone outputs.
3. Gather actionable biological and computational evidence and convert each useful source into a testable feature, prior, objective, baseline, diagnostic, or guardrail.
4. Write a method blueprint containing the baseline, bottlenecks, interfaces, protected invariants, candidate components, and diverse method families.
5. For each candidate, record a hypothesis, implement it in isolation, run checks, evaluate primary and guardrail metrics, diagnose failures, ablate meaningful components, and record a decision.
6. Build a close set of finalists from different method families and simplification levels, rerun them under the frozen benchmark, and select the simplest robust method with understood failure modes.

## Decision rules

- Freeze the evaluator before optimization and preserve a user-supplied evaluator contract.
- Reject or quarantine improvements that rely on hidden labels, protected files, evaluator internals, sample order, filenames, row identifiers, timestamps, duplicate leakage, or preprocessing leakage.
- Record provenance, license, preprocessing, join keys, coverage, leakage risk, and training-safety status for every auxiliary resource.
- Prefer defaults estimated from data, validation-only selection, or theoretically grounded constants and remove components that fail ablation.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_4c8feb8815911444` — Frozen benchmark and protected evaluation data: Comparing every baseline, candidate, ablation, and finalist under the same scientific contract.

### Secondary resources

- `rr_3d456e880a74c00c` — Training-safe literature, ontologies, curated databases, pretrained embeddings, and public datasets: The resource can be converted into a testable method component and evaluated without leakage.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- None declared in the normalized source record.

## Validation / QC

- Track the primary metric, guardrail metrics, runtime, memory, seed variance, baseline comparison, and decision for every substantive candidate.
- Use shuffled-label degradation, no-signal baseline comparison, train-only preprocessing, subgroup review, repeated seeds when relevant, and protected-hash verification before promotion.
- Rerun finalists on the full dataset and reject a candidate whose validation-subset gain does not hold up.
- Evidence requirement: Maintain an auditable record of every hypothesis, code change, metric, runtime, memory result, diagnostic, ablation, and decision.
- Evidence requirement: Require the final method to pass reproducibility, leakage, robustness, and simplification audits under the frozen benchmark.

## Failure handling

- The benchmark cannot score a new candidate and cannot be repaired within the run.
- An improvement depends on a benchmark artifact or hidden information.
- A component fails ablation or does not reproduce across relevant seeds or the full evaluation data.
- Fallback rule: When the benchmark is incomplete, build a development-benchmark scaffold instead of optimizing blindly.
- Fallback rule: When an added component does not help robustly, remove or simplify it.

## Limitations

- None declared in the normalized source record.

## Important domain-specific rules

- Freeze a benchmark card covering metrics, resampling, guardrails, protected files, leakage rules, seeds, limits, and interfaces before search.
- Validate evaluator sensitivity with a real baseline and trivial or shuffled sanity candidates.
- Record one controlled hypothesis, implementation, evaluation, diagnosis, ablation, and decision per substantive experiment.
- Audit leakage with protected hashes, train-only preprocessing, no-signal baselines, subgroup review, and repeated seeds.
- Select the simplest robust finalist, rerun it under the frozen benchmark, trim nonessential components, and package exact reproduction evidence.

## Portability boundary

- Biomni-native method-developer role and use of the Biomni capability stack. — migration action: `exclude_or_capability_map`
- The fixed 50-substantive-iteration round, stop-check command, STOP-GATE transcript line, and Biomni-specific hard-blocker ledger. — migration action: `exclude_or_capability_map`
- ManageMachine and Gpu fan-out policy, platform concurrency caps, machine lifecycle controls, and utilization reporting. — migration action: `exclude_or_capability_map`
- Biomni-specific /mnt/shared-workspace, /mnt/results, and /workspace state, status, checkpoint, and cache layouts. — migration action: `exclude_or_capability_map`
- Bundled Biomni helper scripts, template package, exact command paths, auto-resume behavior, and status-file contracts. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
