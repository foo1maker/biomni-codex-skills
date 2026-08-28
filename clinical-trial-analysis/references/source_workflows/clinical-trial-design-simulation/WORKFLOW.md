# Clinical Trial Design Simulation

Source workflow: `clinical-trial-design-simulation`  
Parent Claude Science skill: `clinical-trial-analysis`

## Purpose

Simulate and evaluate a two-arm confirmatory design with one primary endpoint, estimate operating characteristics across configured scenarios, require type-I/FWER and analytic-benchmark validation gates, and produce auditable tables, figures, and a report.

## When to use

- Power and simulate a two-arm confirmatory design with one primary endpoint.
- Estimate power, expected sample size, expected duration, and adaptive-decision probabilities across effect scenarios.
- Validate simulated type-I/FWER behavior and agreement with an rpact analytic benchmark before reporting operating characteristics.

## Inputs

- A JSON configuration defining endpoint, design and adaptation parameters, effect scenarios, validation grids, runtime preset, and report narrative. (required)
- Configured assumptions for control-arm parameters, treatment effects by subgroup when applicable, subgroup prevalence, accrual, and dropout. (required)
- Optional one-parameter or two-parameter sensitivity sweep for uncertain design assumptions. (optional)

## Outputs

- FWER/type-I and power-versus-rpact validation-gate tables.
- An operating-characteristics table with applicable hypothesis power, expected sample size, expected duration, adaptive-decision probabilities, and Monte Carlo standard error by scenario.
- An optional one-parameter sensitivity-analysis table.
- PNG and editable SVG figures for power, expected sample size or duration, adaptive decisions, and sensitivity.
- A report with an at-a-glance summary, introduction, methods, validation and operating-characteristic results, conclusions, references, and next steps.

## Workflow

1. Write or adapt one configuration defining all endpoint, design, effect, prevalence, adaptation, runtime, and reporting assumptions.
2. Run the global and least-favorable-null FWER gate and the reduced single-hypothesis power-versus-rpact gate before computing reportable operating characteristics.
3. Simulate every configured effect scenario, always including a null scenario, and summarize power, expected sample size and duration, adaptive-decision probabilities, and Monte Carlo error.
4. Sweep the least certain design assumptions and assess whether conclusions are robust.
5. Generate figures and a report derived from the same configuration and validated output tables.
6. Use 1,000 simulations for quick iteration and 10,000 for the thorough final run; quote proportions only from the thorough run.

## Decision rules

- Infer a single full-population hypothesis when prevalence equals 1.0 or enrichment is disabled; infer the closed full-population and subgroup hypothesis set when prevalence is below 1 and enrichment is enabled, unless explicitly overridden.
- Derive interim timing and dropout reporting from the exact simulated configuration rather than duplicating hard-coded prose.
- Treat report.effect_label as authoritative and fail the report build if a simultaneously supplied design.effect_label disagrees.
- Stop when either validation gate fails; do not report operating characteristics for an unvalidated design.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_b06918eb391ad5e0` — rpact analytic design calculations.: Benchmarking reduced single-hypothesis simulated power and computing boundaries.

### Secondary resources

- `rr_07e4838fcaeb255c` — Package design-methods, validation-guide, configuration-schema, and caveat references.: Defining endpoint mappings, combination tests, validation tolerances, configuration fields, and scope limitations.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_5bfccc3167ade2fc` — R: Simulation, validation gates, operating-characteristic grids, sensitivity analysis, and figures.
- `rr_39c84fffe38ec420` — rpact: Design boundaries and analytic benchmark.
- `rr_9e7f0335ca50709d` — survival, data.table, ggplot2, svglite, and jsonlite: R dependencies for simulation data handling, figures, SVG, and configuration parsing.
- `rr_6c86225661991b07` — Python, reportlab, and pypdf: Report rendering and PDF validation.

## Validation / QC

- Require simulated FWER at or below alpha under the global and deliberately chosen least-favorable nulls.
- Require simulated power to agree with rpact for the reduced single-hypothesis design.
- Include a null scenario in every operating-characteristic grid.
- Report Monte Carlo standard error as sqrt(p(1-p)/nsim) for every simulated proportion.
- Evidence requirement: State all design assumptions and show sensitivity because operating characteristics are conditional on the configuration.
- Evidence requirement: Quote numerical results only from the thorough run and include their Monte Carlo uncertainty.
- Evidence requirement: Describe simulation-based type-I control as verified under tested nulls rather than mathematically proven.

## Failure handling

- A failed FWER or analytic-benchmark gate indicates an unvalidated design or implementation and must halt reporting.
- A finite administrative follow-up cap that prevents target events can create an apparent power shortfall that is a sizing artifact.
- Conflicting effect labels in design and report configuration can produce inconsistent interpretation; the documented build fails loudly.
- Fallback rule: Use the quick preset for design iteration, then rerun the accepted configuration with the thorough preset before quoting results.

## Limitations

- The skill does not analyze completed-trial data or calculate a final p-value for real participants.
- Multi-arm or platform, Bayesian, dose-finding, and basket or umbrella designs are out of scope.
- The generators are parametric and do not cover non-proportional hazards, over-dispersion, or informative dropout; asymptotic tests can degrade at very small information.
- Adaptive enrichment or sample-size re-estimation preserves alpha only through the specified fixed-weight inverse-normal combination and must be revalidated for the actual design.

## Important domain-specific rules

- Make one configuration the single source of truth for hypotheses, effect labels, timing, dropout, tables, figures, and report prose.
- Gate all operating-characteristic reporting on both error-control and independent analytic-benchmark validation.
- Separate fast iteration from thorough final simulation and report Monte Carlo uncertainty with every proportion.
- Require sensitivity sweeps on the assumptions most likely to affect the design conclusion.

## Portability boundary

- Package-local R driver, validation, grid, report scripts, example-config paths, and prescribed shell commands. — migration action: `exclude_or_capability_map`
- Phylo report branding and the /workspace and /mnt/results runtime path conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
