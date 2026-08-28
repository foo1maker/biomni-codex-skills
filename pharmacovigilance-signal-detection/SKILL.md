---
name: pharmacovigilance-signal-detection
description: Detect and triage post-market adverse-event reporting signals with disproportionality statistics, label/noise classification, multiplicity control, and explicit non-causal interpretation.
---

# Purpose

Analyze spontaneous post-market adverse-event reports for a drug, explicit drug
list, class, or molecular target. Preserve contingency counts, ROR/PRR and
confidence intervals, chi-square, p-values, FDR, label status, noise reasons,
and confidence so signal triage is auditable.

# When to use

Use for descriptive safety-signal screening against whole-FAERS or a declared
active comparator. Target mode requires a documented target-to-drug resolution.
This is differential reporting, not incidence, absolute risk, or causal effect.

# Inputs

- Required: drug name/list, drug class, or molecular target symbol.
- Optional: mode override, active-comparator list, signal thresholds, request
  depth, and a documented access credential when higher public request limits
  are needed. Keep credentials outside outputs and logs.
- Record query date, drug/member resolution, background universe, event-term
  mapping, MedDRA level, and all threshold choices.

# Workflow

1. Resolve ambiguous input as explicit drug, class, or target mode. If target
   mode is used, record the target-to-drug source and resolved members.
2. Use whole FAERS as the standard background unless a same-class or
   same-indication active comparator is explicitly requested.
3. Retrieve the report facets and label context, retain the full contingency
   table, and calculate ROR with confidence interval, PRR, chi-square, p-value,
   and Benjamini–Hochberg q-value.
4. Apply the declared signal criteria, then classify label status, noise,
   category/SOC, and confidence. Keep full results and filtered views separate.
5. Use genuine noise-excluded counts for headline summaries. For pooled class
   or target results, use a representative member label for grounding and each
   drug's own label for per-drug rows.
6. Add literature grounding or a visual summary only when requested and only
   from retrieved records. Validate tables, figures, and any report.

# Resource selection

Use `../../03_resource_registry/resource_registry.yaml` as the authority. The
primary adapter is the documented OpenFDA/FAERS query and analysis route. Use
whole-FAERS by default; use an active-comparator set for a declared confounding
reduction objective; use Open Targets only for target-to-drug resolution. Treat
all public resource/API access and license states as explicit records, not
assumptions.

# Decision rules

- Default signal rule: ROR 95% CI lower bound >1, PRR >=2, chi-square >=4,
  at least 3 cases, and FDR q <0.05 unless the user supplies criteria.
- Treat ROR/PRR/chi-square as differential-reporting measures without an
  exposure denominator. Never call them incidence or causality.
- Exclude administrative, product-quality, procedure, treated-indication,
  nonspecific, and isolated laboratory terms from top-signal tables but retain
  them in the full CSV.
- Retain low-count and extreme-ROR signals with visible low-confidence flags.
  Maintain the count hierarchy: criteria-passing = genuine + artifacts, and
  genuine = labeled + unlabeled + unknown.
- Classify an unlabeled result as a heuristic text-match status, not proof of
  novelty.

# Validation

- Check drug/member resolution, background universe, MedDRA/event mapping,
  whole-universe totals, and every contingency-table cell.
- Verify the statistical fields, confidence intervals, q-values, label/noise
  categories, and denominators in the full result.
- Reconcile headline counts against the single source-of-truth breakdown and
  separate noise-included totals from noise-excluded labels.
- Preserve query parameters, retrieval time, source URL, software/model
  versions, and a non-causal limitations statement. If a report is requested,
  validate that it is readable and its tables agree with the CSV.

# Failure handling

If automatic class resolution returns no drugs, request an explicit drug list or
canonical class synonym. If target resolution fails or has no relevant drugs,
use explicit/class mode and disclose the change. If additive literature or
visual resources are unavailable, return validated tables and figures without
those layers. Keep low-confidence/noisy rows in full results. Do not replace
failed access with an empty safety conclusion.

# Outputs

Return a full disproportionality table, overview and top-signal views, explicit
noise/label/confidence counts, and requested figures/report. Include all
statistics, filters, background, access state, retrieved citations, and the
statement that signals are hypotheses for follow-up rather than causal risk or
incidence estimates.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`pharmacovigilance-ae-signal-detection`](references/source_workflows/pharmacovigilance-ae-signal-detection/WORKFLOW.md) — Detect and triage post-market adverse-event reporting signals for a drug, drug class, or molecular target using OpenFDA/FAERS disproportionality analysis, with explicit noise, label, and confidence qualification.

<!-- END MANAGED: SOURCE WORKFLOWS -->
