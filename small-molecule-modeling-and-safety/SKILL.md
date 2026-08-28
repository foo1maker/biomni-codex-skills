---
name: small-molecule-modeling-and-safety
description: Curate and analyze small-molecule bioactivity, affinity or potency models, physicochemical and ADMET properties, and off-target safety evidence. Use when a user needs an auditable small-molecule prediction or measured-evidence workflow with explicit mode boundaries, applicability limits, and no implied clinical claim.
---

# Small-molecule modeling and safety

## Purpose

Route a compound question to the correct data-generating mode and keep
measured activity, supervised affinity/potency prediction, descriptor/property
prediction, and off-target safety evidence separate. Preserve chemical
standardization, units, censoring, applicability domain, calibration, and
license provenance.

## When to use

Use for SMILES or structure libraries, measured bioactivity curation,
structure-based or descriptor-based supervised models, physicochemical/ADMET
screening, and primary/off-target safety panels. Do not treat a predicted score
as a measured assay result or use this skill to recommend a clinical dose.

## Inputs

- Declare one mode: `measured_bioactivity`, `supervised_affinity`,
  `property_admet`, or `off_target_safety`.
- Structures as SMILES, InChI, SDF, or stable compound identifiers; preserve
  original strings and salts/tautomers before standardization.
- Measured modes: endpoint, assay type, units, activity class, censoring,
  target, species/cell context, and record identifiers.
- Predictive modes: labelled training data, endpoint definition, split policy,
  model/configuration, applicability-domain criteria, and calibration plan.
- Safety mode: primary target, off-target panel, measured versus predicted
  evidence, safety endpoint, concentration/exposure context, and thresholds.

## Workflow

1. Validate structure syntax, normalize and deduplicate while retaining every
   original/invalid record and transformation in a ledger. Resolve identifiers
   only with a pinned release and license status.
2. In `measured_bioactivity`, harmonize units and assay context, preserve
   censored values and exclusions, aggregate only compatible measurements, and
   report potency/selectivity with record-level provenance.
3. In `supervised_affinity`, define the endpoint and split before training;
   prefer scaffold or time splits where appropriate, evaluate calibration and
   uncertainty, and flag out-of-domain compounds.
4. In `property_admet`, calculate descriptors and predict each endpoint with
   model/version and reference-population metadata. Retain malformed,
   inorganic, duplicate, and out-of-range records as flagged rows.
5. In `off_target_safety`, keep primary and off-target evidence in separate
   tables, distinguish measured from predicted panel hits, and rank only under
   an explicit decision criterion. Export a model/data manifest.

## Resource selection

Use the resource registry as an adapter catalog. Candidate adapters include
public activity databases, compound catalogs, cheminformatics libraries,
descriptor calculators, and endpoint models. Prefer user-supplied labels and
structures; use a public release only when access, attribution, share-alike,
and commercial-use terms are compatible. If a database or model is unavailable,
return an unassessed endpoint or use a documented local fixture; never infer a
license or silently substitute an endpoint.

## Decision rules

- Keep measured bioactivity, supervised affinity/potency, property/ADMET, and
  off-target safety modes separate in data, validation, and claims.
- Preserve units, assay context, censoring, aggregation rules, and exclusions;
  do not pool incompatible endpoints merely because names are similar.
- Report calibration, uncertainty, scaffold/generalization regime, and
  applicability-domain status for predictions. A ranking is not proof of
  activity, selectivity, safety, or efficacy.
- Keep core/primary target evidence separate from adaptive off-target panels to
  expose circular evidence and panel-selection bias.
- Apply chemical-space and standardization flags to every downstream ranking.

## Validation

Check structure parse rates, canonicalization and deduplication counts,
unit/endpoint consistency, censoring, leakage-safe splits, baseline metrics,
calibration, uncertainty, applicability domain, and external or temporal
validation where available. Verify that measured records retain identifiers and
that predicted values point to model versions and input hashes.

## Failure handling

- If structures or endpoint units cannot be resolved, preserve the row as
  invalid/unassessed and stop that branch rather than guessing.
- If labels are too sparse or a split has no usable test set, return a curation
  report and suppress model performance claims.
- If a compound is out of domain or calibration is poor, label the prediction
  low-confidence and do not use it for a hard safety decision.
- If a public activity or model resource is unavailable or restricted, return a
  documented adapter failure and use only authorized local evidence.
- If measured and predicted signals disagree, expose both and investigate assay,
  endpoint, unit, and chemical-context differences; do not average silently.

## Outputs

Return a structure/standardization ledger, mode-labelled measured or predicted
tables, model metrics and calibration, applicability flags, primary/off-target
evidence, ranking criteria, figures, license/access records, and limitations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to every mode.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`adme-ml-modeling`](references/source_workflows/adme-ml-modeling/WORKFLOW.md) — Build, honestly evaluate, save, and apply supervised single-endpoint small-molecule in-vitro ADME models from labelled assay tables.
- [`binding-affinity-ml-model`](references/source_workflows/binding-affinity-ml-model/WORKFLOW.md) — Curate target-specific ChEMBL small-molecule affinity data, benchmark regression models under scaffold splits, and rank novel-scaffold library candidates with explicit applicability-domain tiers.
- [`drug-bioactivity-chembl`](references/source_workflows/drug-bioactivity-chembl/WORKFLOW.md) — Resolve a compound or target in ChEMBL and produce a provenance-tracked molecular potency, off-target, selectivity, and separate cellular-activity profile.
- [`molecular-property-admet`](references/source_workflows/molecular-property-admet/WORKFLOW.md) — Profile small molecules from SMILES for physicochemical properties, drug-likeness, structural alerts, and predicted absorption, distribution, metabolism, excretion, and toxicity endpoints, with standardized inputs, comparative plots, and auditable triage outputs.
- [`off-target-safety-pharmacology`](references/source_workflows/off-target-safety-pharmacology/WORKFLOW.md) — Produce an auditable off-target and secondary-pharmacology liability profile for one small molecule by separating intended targets from off-targets, combining orthogonal prediction engines with ADMET context, benchmarking only against held-out measured compound data, and gating validation claims by data sufficiency.

<!-- END MANAGED: SOURCE WORKFLOWS -->
