---
name: target-evidence-and-tractability
description: Integrate target evidence for surface antigens, direction of effect, genetic constraint, knowledge-graph reasoning, tractability, and tissue expression specificity. Use when a user needs an auditable target-prioritization or modality-fit assessment with source versions, missingness, and evidence classes kept separate; do not turn it into a binary druggability or clinical-safety claim.
---

# Target evidence and tractability

## Purpose

Build a traceable target-evidence matrix and modality-aware prioritization.
Keep surface-antigen, direction-of-effect, constraint, graph, target-platform,
tractability, and tissue-expression modes distinct while exposing missing and
conflicting evidence.

## When to use

Use when comparing candidate targets or evaluating whether a target is
compatible with a modality, tissue context, or safety hypothesis. Route a
single database lookup to a database-specific workflow; route structure design,
variant calling, or clinical decision-making elsewhere.

## Inputs

- Target identifiers, species, genome/build, disease/phenotype, modality, and
  intended decision criteria.
- Evidence records with source, release/version, query, identifier, effect
  direction, tissue/cell context, and access/license status.
- Optional modes: `surface_antigen`, `direction_of_effect`, `constraint`,
  `knowledge_graph`, `target_platform`, `tractability`, and
  `expression_specificity`.
- Thresholds, weights, missingness policy, and whether the result is an
  exploratory ranking or a decision gate.

## Workflow

1. Normalize gene/protein identifiers and freeze species/build and source
   releases. Deduplicate records without erasing conflicting evidence.
2. Assemble an evidence matrix. Keep database facts, model predictions,
   calculations, inferences, and recommendations in separate columns with
   source identifiers and retrieval metadata.
3. Run the selected mode: surface-antigen accessibility/specificity; direction
   concordance across independent axes; constraint as a gating signal; graph
   propagation with explicit seeds/edges; platform evidence retrieval;
   modality-specific tractability; or cross-tissue expression specificity.
4. Normalize only when justified, preserve raw components, and report
   missingness, discordance, coverage bias, and sensitivity to thresholds or
   weights. Rank candidates only after the decision objective is declared.
5. Export the matrix, component scores, evidence ledger, conflicts, sensitivity
   analyses, and a provenance-aware interpretation.

## Resource selection

Use the resource registry as an adapter catalog and verify release, source
coverage, license, and access before use. Candidate adapters may include target
evidence platforms, genetic constraint catalogs, tissue-expression atlases,
surface-protein annotations, interaction/knowledge graphs, and literature.
Prefer direct records and user-supplied evidence. If an adapter is unavailable,
mark that axis unassessed and do not impute a neutral or favorable value without
an explicit, documented rule.

## Decision rules

- Keep the seven modes separate; a high expression score is not tractability,
  a constraint signal is not causal proof, and graph rank is not efficacy.
- For surface-antigen work, require extracellular/accessibility, tumor or target
  specificity, and normal-tissue safety evidence before a modality-fit claim.
- For direction-of-effect, retain each independent axis and flag discordance;
  do not convert concordance into an intervention recommendation automatically.
- For graph reasoning, record seed definition, edge provenance, propagation
  parameters, licensing mode, and known-versus-novel status.
- Treat expression specificity as on-target liability evidence, not clinical
  safety proof. Preserve missing gene/organ values and threshold sensitivity.

## Validation

Verify identifier mapping, source release/version, record counts, duplicate and
conflict handling, tissue/species coverage, graph edge provenance, score
components, missingness, sensitivity to thresholds/weights, and citation
resolvability. Reconcile any high-priority result against an independent source
or orthogonal evidence axis before recommending follow-up.

## Failure handling

- If target IDs, species/build, or release versions conflict, stop the affected
  axis and return the mapping conflict.
- If a source is unavailable or restricted, report the axis as unassessed and
  preserve the query for later replay; never fabricate a record.
- If evidence axes disagree, retain both, label discordance, and lower
  confidence rather than averaging away the conflict.
- If graph licensing or seed provenance is incompatible with the intended use,
  switch to a documented source-compatible graph or return no graph score.
- If normal-tissue or accessibility evidence is missing, suppress a safety or
  surface-antigen conclusion and state the missingness.

## Outputs

Return an evidence matrix, mode-labelled component tables, ranked candidates
only with declared criteria, missing/conflict ledger, sensitivity analysis,
source/version/access records, and a conservative interpretation separating
facts, predictions, inferences, and recommendations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every mode.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`cell-surface-antigen-discovery`](references/source_workflows/cell-surface-antigen-discovery/WORKFLOW.md) — Nominate antibody-accessible cell-surface antigens for ADC, CAR-T, bispecific, and radioligand development by integrating tumor-compartment specificity, extracellular topology, normal-tissue safety, tractability, and a validation harness.
- [`direction-of-effect-concordance`](references/source_workflows/direction-of-effect-concordance/WORKFLOW.md) — Determine whether target activation or inhibition is therapeutically favored for a specified indication by reconciling directional evidence across independent axes.
- [`genetic-constraint-gating`](references/source_workflows/genetic-constraint-gating/WORKFLOW.md) — Triage candidate genes with gnomAD loss-of-function constraint, cross-version stability checks, deterministic risk and strategy flags, and ClinGen-grounded disease notes.
- [`knowledge-graph-target-reasoning`](references/source_workflows/knowledge-graph-target-reasoning/WORKFLOW.md) — Prioritize human therapeutic targets for a disease by propagating disease seeds through PrimeKG with random walk with restart, explain top hits with multi-hop evidence paths, and validate ranking face validity and literature support.
- [`open-targets`](references/source_workflows/open-targets/WORKFLOW.md) — Query Open Targets target–disease associations, supporting evidence, genetics, and target, disease, drug, variant, or study annotations through the public GraphQL API.
- [`target-tractability-druggability`](references/source_workflows/target-tractability-druggability/WORKFLOW.md) — Assess whether a human protein-coding target is druggable and identify the most viable and emerging therapeutic modalities by combining tractability, structural, clinical, safety, and literature evidence.
- [`tissue-expression-specificity`](references/source_workflows/tissue-expression-specificity/WORKFLOW.md) — Profile one human protein-coding target across GTEx and Human Protein Atlas tissues, quantify tissue specificity, compare atlases, and synthesize on-target safety signals.

<!-- END MANAGED: SOURCE WORKFLOWS -->
