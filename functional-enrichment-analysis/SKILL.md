---
name: functional-enrichment-analysis
description: "Use when differential-expression results must be interpreted with GSEA, ORA, or both using explicit identifiers, background, ranking, gene-set versions, and multiple-testing controls."
---

# Functional enrichment analysis

## Purpose

Interpret differential-expression results through rank-based gene-set enrichment
(GSEA), over-representation analysis (ORA), or a defensible complementary pair.
Make identifier mapping, species, ranking, background, gene-set release/license,
and interpretation limits explicit.

## When to use

- A complete genome-wide ranking supports GSEA.
- A thresholded gene list and tested-gene universe support ORA.
- Both inputs are defensible and the user wants cross-method pathway validation.

Do not infer a mechanism from enrichment alone, and do not invent a ranking
metric or background universe.

## Inputs

- Differential-expression table with gene identifiers, species, identifier type,
  effect/statistic, and adjusted p-values.
- Complete ranking metric for GSEA, or significance rule and tested-gene
  background for ORA.
- Gene-set collection, release/version, license status, and biological question.
- Optional pathway annotations, literature, and visualization requirements.

## Workflow

1. Validate species and identifier type; resolve or exclude unusable IDs with
   counts and reasons.
2. Build a complete ranked vector for GSEA, preferring a test statistic over
   log2 fold change when available. If only a defensible list exists, choose ORA.
3. Select gene sets consistent with the question and record release/version and
   license. Use GSEA as default when a complete ranking exists.
4. Run the selected analysis, retaining mapped/unmapped IDs, tested universe,
   gene-set counts, enrichment scores, adjusted p-values, and leading-edge genes.
5. When both modes are defensible, compare them as complementary evidence rather
   than forcing agreement. Export tables, plots, ranked/list inputs, and a
   method summary.
6. Interpret direction and leading-edge genes with literature or complementary
   evidence; label pathway enrichment as an association/inference.

## Resource selection

- `primary`: a registry-recorded gene-set collection appropriate to the question
  and license, commonly `Gene Ontology`, `Reactome`, or Hallmark gene sets from
  `MSigDB`.
- `secondary`: `clusterProfiler`, `enrichplot`, `msigdbr`, or species annotation
  packages as replaceable execution adapters with recorded versions.
- `fallback`: ORA when a complete ranking is unavailable, or a base plotting
  device when preferred vector export fails. Disclose both changes.
- `KEGG` is opt-in only after applicable commercial-use terms are confirmed;
  otherwise use the stated alternatives.

See [resource selection policy](../_shared/resource_selection.md). An unknown
license or version blocks redistribution claims but does not erase the source
record.

## Decision rules

- **FEA-1:** Use GSEA for a complete, defensible ranked vector; use ORA for a
  selected list or explicit complementary validation.
- **FEA-2:** Prefer the test statistic over fold change as the GSEA ranking
  metric when available.
- **FEA-3:** ORA must report the tested-gene universe; an unspecified default
  background is not acceptable.
- **FEA-4:** Record species, identifier mapping, gene-set release, software
  versions, ranking metric, thresholds, and multiple-testing procedure.
- **FEA-5:** Interpret pathways with leading-edge genes and signal direction;
  enrichment is not mechanistic proof.

## Validation

- Confirm input columns, species/IDs, mapping yield, ranking completeness/order,
  universe, gene-set identity/version/license, and analysis mode.
- Check adjusted p-values, enrichment scores, leading-edge membership, duplicate
  or empty sets, and the denominator used for ORA.
- Ensure plots correspond to exported results and retain serialized inputs and
  analysis metadata.

See [validation policy](../_shared/validation_policy.md) and
[evidence policy](../_shared/evidence_policy.md).

## Failure handling

If mapping or species consistency fails, stop or report an explicitly reduced
scope. If only a list is available, switch to ORA rather than fabricate ranks.
If vector export fails, retain a declared PNG/base-device fallback. Preserve
empty/low-yield results and explain whether they reflect data, mapping, or gene
set coverage.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Validated ranked vector/list, mapping and universe report, and gene-set
  release/license record.
- GSEA/ORA result tables, adjusted significance, leading-edge genes, and plots.
- A concise interpretation that distinguishes database facts, calculations,
  inferences, and limitations.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`functional-enrichment-from-degs`](references/source_workflows/functional-enrichment-from-degs/WORKFLOW.md) — Interpret differential-expression results with gene-set enrichment analysis, over-representation analysis, pathway visualizations, and documented database and ranking choices.

<!-- END MANAGED: SOURCE WORKFLOWS -->
