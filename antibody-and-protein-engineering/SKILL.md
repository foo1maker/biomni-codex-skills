---
name: antibody-and-protein-engineering
description: "Route antibody developability and humanization or structure-guided binder design from explicit sequence, target-structure, and epitope inputs. Use for antibody liability and humanness assessment, non-human framework comparison, de novo mini-protein or antibody/nanobody binder hypotheses, and candidate ranking with auditable uncertainty; do not treat computational scores as binding or experimental validation."
---

# Antibody and protein engineering

## Purpose

Produce a mode-labelled, evidence-aware analysis for either antibody
developability/humanization or structure-guided binder design. Keep the two
modes separate because their inputs, hypotheses, and validation claims differ.

## When to use

Choose exactly one mode before analysis:

| Mode | Use when | Minimum contract |
|---|---|---|
| `developability-humanization` | Assess a variable region, compare human frameworks, or score a humanization candidate | Paired VH/VL FASTA or one variable domain; numbering and species state recorded |
| `binder-design` | Design a mini-protein binder or antibody/nanobody CDR hypothesis | Target structure/model, target chain and construct span, and an epitope/hotspot policy |

Do not merge a humanization score with a de novo binder score or silently switch
from a structure hypothesis to an affinity claim.

## Inputs

- For developability: paired VH/VL amino-acid sequences or a single domain,
  species/branch, numbering scheme, optional back-mutation level, HLA panel,
  and an optional validated reference reserved for blind post-hoc scoring.
- For binder design: PDB/mmCIF or a documented structure model, target chain
  and residue span, hotspot/epitope residues when available, modality
  (`mini-protein` or `antibody/nanobody`), framework/CDR constraints, and
  campaign scale.
- Record sequence and structure identifiers, source URLs, build/numbering
  versions, and any proprietary-data transmission constraint.

## Workflow

1. Classify the request and block if the modality, sequence branch, target
   chain, or epitope policy is ambiguous.
2. Validate identifiers, sequence alphabet, chain/span, and required metadata;
   preserve warnings and unresolved mappings.
3. In `developability-humanization`, number the chains, classify paired
   non-human, paired human, or single-domain input, and keep CDR grafting
   verbatim. For a non-human pair, compare declared human-framework routes and
   restrict back-mutations to declared framework positions.
4. Reassess each construct for sequence liabilities, aggregation propensity,
   humanness, and MHC-II immunogenicity only when the predictor is available.
   Keep the named aggregation scale and predictor scope visible.
5. In `binder-design`, preserve the target construct and native numbering,
   generate or obtain candidate hypotheses, evaluate interface geometry and
   declared-hotspot recovery, and keep modality-specific design tracks apart.
6. Compare candidates only under the same numbering, HLA panel, structure
   scope, and scoring context. Treat confidence metrics as model predictions;
   use an optional reference only after design and never for selection.
7. Export mode-tagged machine-readable tables, candidate sequences/metadata,
   warnings, and provenance before writing a narrative summary.

## Resource selection

- Prefer user-provided sequences and target structures when their identifiers,
  scope, and access terms satisfy the contract.
- Use the registry at `../../03_resource_registry/resource_registry.yaml` as a
  catalog, not as proof of availability or permission. Observed adapters
  include RCSB PDB, UniProt, AlphaFold Protein Structure Database, IEDB,
  ANARCI/abnumber, AGGRESCAN a3v, NetMHCIIpan, and structure/design model
  families. Each is replaceable and must be recorded with role, version,
  access, and license status for the run.
- Prefer a local immunogenicity predictor for confidential sequences. Use a
  hosted predictor only after explicit approval to transmit the sequence. If
  neither route is available, keep that axis unavailable; do not fabricate a
  number.

## Decision rules

- Assess an already-human pair without proposing humanization; assess a single
  domain as supplied.
- Preserve CDRs verbatim and document every framework back-mutation.
- Keep sequence-based aggregation separate from structure-aware aggregation;
  GRAVY, pI, and charge are context, not substitutes for the named predictor.
- Report hotspot recovery separately from geometry confidence. A confident
  candidate with no declared hotspot recovery is `OFF_TARGET`, not successful.
- Preserve target chain and native residue numbering. A missing structure fact
  is `not_provided`, never a hand-typed value.

## Evidence

Label sequence/structure observations, predictor outputs, cross-candidate
inferences, and design recommendations separately. Attach source identifiers,
versions, input artifacts, and uncertainty to every material claim.

## Validation

- Input gate: sequence/structure parse, branch or modality, chain/span,
  numbering/build, hotspot provenance, and predictor privacy checks.
- Intermediate gate: verify every construct/candidate artifact before ranking;
  recompute table-to-summary counts and retain predictor versions and warnings.
- Output gate: confirm mode, sequences, metrics, target scope, reference status,
  and limitations are present. Keep computational hypotheses distinct from
  measured binding, expression, or developability.

## Failure handling

Classify the first actionable failure using the shared policy. Stop on missing
numbering, wrong target chain, malformed sequence, or unresolved structure
scope. If the numbering stack, immunogenicity predictor, or structure service
is unavailable, return a partial result with an explicit unavailable axis or
declared fallback. Never transmit proprietary sequences silently and never use
an optional benchmark as training or selection data.

## Outputs

Return a mode-labelled manifest, input and resource provenance, per-construct or
per-candidate tables, confidence/limitation flags, selected sequences or
structure metadata, and a concise interpretation that labels predictions,
inferences, and recommendations separately.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`antibody-developability-humanization`](references/source_workflows/antibody-developability-humanization/WORKFLOW.md) — Assess antibody sequence liabilities, aggregation propensity, MHC-II immunogenicity, framework humanness, and—when the input is non-human—humanization candidates.
- [`binder-antibody-design`](references/source_workflows/binder-antibody-design/WORKFLOW.md) — Design and computationally prioritize de novo mini-protein binders or framework-based antibody and nanobody CDR designs against a protein target.

<!-- END MANAGED: SOURCE WORKFLOWS -->
