---
name: cell-communication-analysis
description: "Infer and summarize ligand-receptor communication among annotated single-cell groups, with species-matched interaction resources, sender-receiver/pathway views, uncertainty, and explicit input/QC gates. Use for cell-cell communication or signaling-network questions from annotated single-cell data; do not apply to unannotated or bulk data."
---

# Cell communication analysis

## Purpose

Infer candidate ligand-receptor interactions from annotated single-cell
expression and summarize interaction counts, strengths, pathways, and
sender-receiver roles. These are model-based hypotheses, not direct evidence of
physical interaction or causality.

## When to use

Use for an annotated single-cell object with normalized expression, a cell-type
column, a species, and a declared signaling scope (`all`, `secreted`, or
`cell-contact`). Spatial extensions require coordinates and must be labelled as
such. Complete annotation first when cell labels are absent.

## Inputs

- Normalized expression object and metadata with cell identifiers and labels.
- Species and a matching interaction database/adapter.
- Signaling scope, optional pathway or sender/receiver focus, and group-size
  thresholds.
- Optional spatial coordinates and platform provenance for spatial analysis.

## Workflow

1. Validate object readability, expression layer, metadata alignment, species,
   label completeness, and cell counts per type.
2. Require at least three cell types and preferably at least ten cells per type;
   record excluded or sparse groups rather than silently dropping them.
3. Select the species-matched interaction resource and declared statistical
   settings. Keep database release and filtering parameters in the manifest.
4. Infer interactions, summarize pathways and network roles, and retain
   significance/effect measures and uncertainty for each interaction.
5. Generate network, chord, bubble, heatmap, and sender-receiver views only when
   their underlying tables are non-empty and interpretable.
6. Export tables, model object, figures, and a concise report with checkpoint
   results and limitations.

## Resource selection

Prefer user-provided annotated data and a species-matched adapter. The registry
records CellChatDB.human, CellChatDB.mouse, CellChat, Seurat, and visualization
packages as observed resources; consult
`../../03_resource_registry/resource_registry.yaml` for access/license status
and versions. These names are optional adapters, not guaranteed runtime tools.
If an adapter is unavailable, preserve the input/QC result and disclose that
communication inference was not run.

## Decision rules

- Never run on unannotated data or use a bulk matrix as if it were single-cell
  communication input.
- Match interaction database species to the input; a species mismatch is a
  blocked input condition unless an explicit, documented mapping exists.
- Treat sparse-group or no-interaction output as a data/evidence limitation,
  not proof that signaling is absent.
- Keep interaction strength, statistical significance, pathway aggregation, and
  sender/receiver summaries distinct; do not infer causality from a network.

## Evidence

Label database observations, inferred interactions, model-based scores, and
biological recommendations separately. Retain the object, labels, database
release, statistical settings, and uncertainty for each interaction claim.

## Validation

- Input gate: object/layer, cell and label alignment, species, group size,
  coordinate availability for spatial claims, and resource version/access.
- Intermediate gate: interaction table schema, multiple-testing settings,
  non-empty groups, checkpoint completion, and reproducible parameters.
- Output gate: every figure maps to an exported table; absent interactions,
  sparse groups, and fallback renderers are visible in the result status.

## Failure handling

Stop on unreadable or unannotated input, label-column mismatch, species
incompatibility, or insufficient cells. For no significant interactions, report
the thresholds and sparse-data limitation. Use only declared statistical or
graphics fallbacks; retain a Markdown/text result if an optional renderer is
unavailable. Do not turn a fallback into a claim of interaction.

## Outputs

Return interaction, pathway, matrix, centrality, and sender-receiver tables;
figures tied to those tables; model/resource provenance; QC and uncertainty;
and a conclusion that labels results as inferred communication hypotheses.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`cell-cell-communication`](references/source_workflows/cell-cell-communication/WORKFLOW.md) — Infer and summarize ligand-receptor communication between annotated single-cell populations, including pathway activity, network structure, and dominant sender and receiver roles.

<!-- END MANAGED: SOURCE WORKFLOWS -->
