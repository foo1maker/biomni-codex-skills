---
name: microbiome-functional-analysis
description: Analyze processed 16S feature counts for modular diversity, taxonomic differential abundance, predicted functional potential, and enzyme-module interpretation with subject-aware designs and explicit prediction caveats.
---

# Purpose

Compare two groups from processed 16S data while keeping diversity, taxonomic
associations, predicted function, and inferred metabolite potential as separate
analysis stages. This skill does not turn 16S predictions into measured
metabolites or functional validation.

# When to use

Use when integer feature counts and sample metadata are available and the user
wants one or more of the stages below:

- alpha or beta diversity;
- taxonomic differential-abundance consensus;
- predicted enzyme-number function; or
- curated enzyme-module potential.

If only raw reads or an accession is available, stop and route to a raw-read
workflow. If the question is about shotgun confirmation or measured
metabolites, state that this skill is insufficient.

# Inputs

- Required: feature-by-sample integer count table and metadata with matching
  sample IDs and a two-level group variable.
- Strongly recommended: subject ID, covariates, and a declared reference group.
- Optional: representative-sequence FASTA, taxonomy table, and Newick tree.
  Without a tree, omit Faith phylogenetic diversity and UniFrac.
- Record selected stages, contrast, repeated-measures structure, taxonomy and
  functional-reference versions, and the intended output format.

# Workflow

1. Inspect dimensions, integer status, feature/sample IDs, group sizes,
   missingness, tree availability, and independent-subject counts. Confirm the
   reference level and whether samples repeat subjects.
2. If interpretation is requested, retrieve literature before analysis and use
   only retrieved records for mechanistic or background claims.
3. Run only requested modules. For diversity, compute selected alpha/beta
   metrics and use subject-blocked tests or subject means when appropriate.
4. For taxonomy, apply prevalence filtering, run complementary methods, and
   join results by feature and contrast. Preserve method-specific estimates.
5. For predicted function, use representative sequences and counts, produce
   unstratified enzyme-number profiles, and record placement quality including
   mean weighted NSTI.
6. For metabolite modules, verify enzyme-number mappings, keep biologically
   distinct routes separate, and audit the contribution of the top enzyme.
7. Export tables and figures, then separate data-derived results,
   literature-supported interpretation, and confirmatory next steps.

# Resource selection

Use `../../03_resource_registry/resource_registry.yaml` as the resource
authority. Resources are adapters, not guaranteed capabilities; record role,
version, access, license status, and retrieval time for every selected item.

- Primary taxonomy association: ANCOM-BC2 when covariates or repeated subjects
  require a subject-aware model.
- Complementary checks: ALDEx2 and MaAsLin2; use scikit-bio for an explicitly
  documented diversity/ordination implementation when appropriate.
- Predicted function: PICRUSt2 only when sequences and counts satisfy its
  input contract and compute is available.
- Enzyme verification: ExplorEnz, IUBMB Enzyme Nomenclature, or Rhea. Keep
  SILVA taxonomy versioned. Do not assume KEGG use is permitted; the archived
  source identifies it as non-default for commercial use.

# Decision rules

- Treat repeated samples as non-independent. Use subject random effects,
  subject-blocked permutations, or subject-mean sensitivity tests.
- Call a taxon a consensus hit only when at least two differential-abundance
  methods agree in direction. Apply Benjamini–Hochberg control; use q < 0.05
  by default unless the user specifies another threshold.
- Run functional prediction without stratified output by default and report
  the observed NSTI value. A mean weighted NSTI below 0.15 is only the stated
  reliability cue, not validation.
- Report the `but` and `buk` butyrate routes separately. Keep the
  secondary-bile-acid module disabled by default and exclude EC:1.3.1.114.
- Describe functional and metabolite results as predicted genomic potential.
  If a module is dominated by one enzyme, disclose its fraction and temper the
  interpretation.

# Validation

- PASS only when required table and metadata contracts, matching IDs, chosen
  contrast, and subject counts are explicit.
- Preserve method-level estimates, filtering thresholds, reference versions,
  and denominators. Check diversity figures for blank or clipped output.
- For repeated measures, verify that the replication unit is the subject.
- For prediction, report sequence/reference version, placement quality, NSTI,
  and the limitation that confirmatory shotgun or targeted metabolomics may be
  needed.
- Every literature or database statement must have a retrievable source and
  claim class; unknown access or license status remains unknown.

# Failure handling

Stop on non-integer counts, mismatched IDs, unresolved groups, or an absent
required input. If a tree is missing, omit tree-dependent metrics and continue
with non-phylogenetic metrics. If a method is unavailable, use only a declared
secondary or fallback implementation and disclose the changed method. Never
silently treat samples as independent, silently drop taxa, or present a
predicted function as a measured metabolite.

# Outputs

Return selected-stage CSV tables, figures, and a concise methods/limitations
summary. Include subject and sample denominators, method-specific statistics,
consensus criteria, reference versions, NSTI and module-dominance diagnostics,
retrieved citations, and confirmatory next steps. A PDF is an optional output
adapter and must not replace the underlying tables.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`microbiome-analysis`](references/source_workflows/microbiome-analysis/WORKFLOW.md) — Compare two groups from processed 16S feature data using modular community-diversity, taxonomic differential-abundance, predicted-function, and enzyme-module metabolite-potential analyses while preserving subject-level design and explicit prediction caveats.

<!-- END MANAGED: SOURCE WORKFLOWS -->
