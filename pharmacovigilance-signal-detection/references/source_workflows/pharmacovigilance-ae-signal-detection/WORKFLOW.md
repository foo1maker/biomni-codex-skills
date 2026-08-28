# Pharmacovigilance Ae Signal Detection

Source workflow: `pharmacovigilance-ae-signal-detection`  
Parent Claude Science skill: `pharmacovigilance-signal-detection`

## Purpose

Detect and triage post-market adverse-event reporting signals for a drug, drug class, or molecular target using OpenFDA/FAERS disproportionality analysis, with explicit noise, label, and confidence qualification.

## When to use

- Scan post-market adverse-event reporting signals for one drug or a pooled explicit drug list.
- Compare or pool safety signals across a drug class.
- Resolve a molecular target to drugs and perform target-anchored signal detection.
- Triage labeled, potentially unlabeled, unknown, noisy, and low-confidence signals.
- Compute ROR, PRR, chi-square, confidence intervals, and multiplicity-adjusted signal flags against whole-FAERS or active-comparator backgrounds.

## Inputs

- A drug name, explicit list of drug names, drug class, or molecular target symbol. (required)
- Optional explicit, class, or target mode override. (optional)
- Optional active-comparator drug list; otherwise use the whole FAERS background. (optional)
- Optional custom SignalCriteria thresholds. (optional)
- Optional OpenFDA API key for higher request limits. (optional)
- Optional number of reaction terms per drug, with a default and OpenFDA facet cap of 500. (optional)
- Requested depth: signal tables alone or a full report with figures, literature grounding, and infographic. (optional)

## Outputs

- Full disproportionality CSV containing contingency counts, ROR and confidence interval, PRR, chi-square, p-value, FDR q-value, signal, label, noise, category, SOC, and confidence fields.
- Overview, top genuine signal, and potentially unlabeled signal tables.
- Five figure families covering top signals, volcano view, forest estimates, multi-drug heatmap, and summary panel.
- A report with executive summary, methods, results, figures, tables, limitations, conclusions, references, and next steps.
- A single-source-of-truth count breakdown separating criteria-passing signals, genuine signals, artifacts, and label classes.

## Workflow

1. Resolve the input mode and member drugs, query OpenFDA, compute disproportionality, annotate rows, create figures and tables, and build a draft report with the packaged deterministic pipeline.
2. Optionally use the orchestrator-provided query to ground top signals in literature records.
3. Optionally use the orchestrator-provided prompt to create a mechanism or summary infographic.
4. Rebuild the final report with the literature records and infographic, then validate that the PDF has at least two pages and extractable text.
5. Skip additive literature and infographic steps when the user requests only tables or figures.

## Decision rules

- Confirm ambiguous input as a single drug, drug class, or molecular target; otherwise allow automatic mode detection.
- Use whole FAERS as the standard background unless an active-comparator drug set is requested.
- Use the standard rule of ROR 95% CI lower bound above 1, PRR at least 2, chi-square at least 4, at least 3 cases, and FDR q below 0.05 unless custom thresholds are requested.
- Treat disproportionality as differential reporting, not incidence, absolute risk, or causal effect.
- Exclude flagged noise from top-signal tables but retain it in the full CSV.
- Retain low-confidence signals and visibly mark them rather than silently deleting or over-interpreting them.
- Use the genuine noise-excluded count as the headline and do not mix noise-included totals with noise-excluded label breakdowns.
- For pooled class or target results, use a representative member label for grounding; use each drug's own label for per-drug rows.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_db606433dd34ae39` — Packaged run_analysis pipeline over OpenFDA/FAERS.: Use for deterministic resolution, querying, disproportionality, annotation, figures, tables, and draft reporting.
- `rr_8984c786242259eb` — Whole-FAERS background.: Use for the standard disproportionality comparison.

### Secondary resources

- `rr_0cd80cefdbb36443` — Active-comparator drug set.: Use to compare reporting against a same-class or same-indication background and reduce some indication confounding.
- `rr_9557f29e64a1ce14` — Open Targets target-to-drug resolution.: Use when the query is anchored to a molecular target.
- `rr_b18dda3cd61625c8` — Literature records and a summary infographic.: Use for the full report when evidence grounding and a visual summary are requested.

### Fallback resources

- `rr_fb9cfeace254c03e` — Explicit drug list or canonical class synonym.: Use when automatic class resolution yields no drugs.
- `rr_e8ac711c6926246a` — A class or explicit-drug workflow.: Use when a target symbol cannot be resolved or has no approved or clinical drugs.
- `rr_6449ed79528f1680` — Tables, figures, and draft report without additive literature or infographic.: Use when the user only requests core signal outputs or additive agent tools are unavailable.
- `rr_b7fb3195e1f23bad` — Packaged scripts as a documented reference.: Use only after fixing, retrying, and documented script modification fail.

### Optional resources

- `rr_8fb7d9a0e2ca74c4` — OpenFDA drug API: Provide adverse-event facets and structured product-label text.
- `rr_6f752ecbeae700bf` — FAERS: Provide the spontaneous-reporting data underlying disproportionality analysis.
- `rr_1cd8978f9c801055` — Open Targets Platform: Resolve molecular targets to drugs in target mode.
- `rr_178baeb270b10c13` — pandas: Manage annotated result and export tables.
- `rr_78609fcc8c7914c6` — numpy: Provide numerical operations.
- `rr_18c50dad9eca9c93` — scipy: Provide statistical calculations.
- `rr_980072f6e7547905` — statsmodels: Apply multiplicity adjustment.
- `rr_f48c55f08412b6c6` — requests: Query OpenFDA and Open Targets.
- `rr_665edbde6fc3cd1c` — matplotlib: Create the signal figure suite.
- `rr_ae5d2965f1c73447` — reportlab: Build the packaged PDF report.
- `rr_c625bc4be2dc2ab7` — pypdf: Inspect and validate generated PDFs.
- `rr_a4e628b2aa1496d8` — scripts/run_analysis.py: Orchestrate resolution, data retrieval, statistics, annotation, figures, tables, and draft report.
- `rr_25bc8e12cb3a7e37` — finalize_report: Rebuild the report with optional literature records and infographic.
- `rr_b01b20f1581d0378` — LiteratureSearch: Retrieve literature records using the query emitted by the orchestrator.
- `rr_efe1eebfa737c317` — GenerateImage: Create the requested summary infographic from the orchestrator prompt.
- `rr_29a1799d951c5d7b` — validate_pdf: Check page count and text extractability for the final report.
- `rr_a0fc51e8037bbcdd` — Reporting odds ratio and proportional reporting ratio disproportionality model: Measure event reporting disproportionality relative to a background.
- `rr_5f8e99eeb3ecc828` — Evans-style signal rule with Benjamini–Hochberg FDR: Flag signals using confidence, PRR, chi-square, case-count, and multiplicity criteria.
- `rr_ba275acd6d514d0d` — Per-drug Tukey far-out fence on log ROR: Flag extreme ROR values as potentially low-confidence rather than removing them.

## Validation / QC

- Use the packaged query and statistical pipeline rather than reimplementing OpenFDA encoding or disproportionality calculations.
- Use whole-universe event totals rather than summing drug-set event counts unless the set partitions the entire background.
- Maintain one authoritative count hierarchy in which criteria-passing signals equal genuine plus artifacts and genuine equals labeled plus unlabeled plus unknown.
- Filter administrative, product-quality, procedure, treated-indication, nonspecific, and isolated laboratory terms from top tables while retaining them in full results.
- Mark small-count and extreme-ROR rows as low confidence and keep them in the output.
- Validate the final PDF as at least two pages with extractable text.
- Evidence requirement: Retain every contingency-table cell and each statistical estimate, confidence interval, p-value, adjusted q-value, and signal flag in the full result.
- Evidence requirement: Describe ROR, PRR, and chi-square only as differential-reporting measures and state the absence of an exposure denominator.
- Evidence requirement: Qualify unlabeled status as a heuristic text-match result, not proof of novelty.
- Evidence requirement: Surface noise and low-confidence reasons for every affected signal.
- Evidence requirement: Ground top report signals in literature records when the full literature-grounded report is requested.

## Failure handling

- Class mode resolves no drugs because an informal name is absent from the label vocabulary.
- Target mode resolves no drugs because the symbol is absent or no approved or clinical drugs are linked.
- A resolved drug has no spontaneous reports.
- OpenFDA returns HTTP 429 rate limiting.
- Special characters in an event term trigger an HTTP 400 query failure.
- A multi-drug heatmap contains an all-missing column.
- Every high-ROR row is noise, leaving an empty top-signal table.
- Writing a random-access PDF directly to the mounted output path produces a zero-byte file.
- Fallback rule: Map an unresolved class to a canonical synonym or pass the member drugs explicitly.
- Fallback rule: Verify an unresolved target symbol, then switch to class or explicit-drug mode if no relevant drugs are available.
- Fallback rule: Drop resolved drugs with zero reports and record them in the dropped list.
- Fallback rule: Retry rate-limited OpenFDA calls after backoff or provide an API key.
- Fallback rule: Retry alternate apostrophe forms for event terms that return HTTP 400.
- Fallback rule: Drop all-missing heatmap columns and inspect the full CSV when the top table is empty.
- Fallback rule: Fix and retry first, then modify and document the script, then adapt it as a cited reference, and write from scratch only if genuinely impossible.

## Limitations

- Spontaneous reporting has no exposure denominator, so the workflow cannot estimate incidence or absolute risk.
- Disproportionality signals do not establish causation and can be inflated by indication confounding, notoriety, stimulated reporting, and channelling.
- Product-label grounding is a heuristic text match and can misclassify wording variants.
- SOC assignments use a curated fallback map rather than the licensed MedDRA hierarchy.
- OpenFDA facets expose only the most frequent reaction terms, with a cap of about 500 per drug.
- Small-count and extreme-ROR estimates are fragile even when they pass the standard signal criteria.
- Pooled class or target rows use a representative member's label rather than every member's label.
- The workflow is not intended for controlled-trial safety analysis, individual case processing, efficacy comparison, or causal-risk estimation.

## Important domain-specific rules

- Mode detection and resolution for explicit-drug, class, and target-anchored queries.
- Choice between whole-background and active-comparator disproportionality.
- ROR, PRR, chi-square, confidence-interval, and FDR signal computation.
- Noise classification that separates full evidence retention from headline-signal filtering.
- Low-confidence marking based on small counts and per-drug extreme ROR without deleting rows.
- Single-source-of-truth count accounting across genuine, artifact, and label classes.
- Auditable full results plus focused overview, top-signal, and unlabeled-signal tables.

## Portability boundary

- Biomni LiteratureSearch and GenerateImage orchestration using prompts emitted by the packaged pipeline. — migration action: `exclude_or_capability_map`
- Phylo-branded PDF report conventions. — migration action: `exclude_or_capability_map`
- Biomni mounted-output and workspace-copy requirements for ReportLab and file-edit operations. — migration action: `exclude_or_capability_map`
- Biomni skill-to-skill handoffs for target resolution, literature grounding, clinical landscape, and PDF reporting. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
