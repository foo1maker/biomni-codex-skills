---
name: literature-and-evidence-synthesis
description: "Use when literature must be searched, screened, extracted, compared, audited, or quantitatively synthesized with claim-level citations, access-aware provenance, and explicit evidence limits."
---

# Literature and evidence synthesis

## Purpose

Route five distinct evidence modes: narrative review, preclinical extraction,
audit-ready deep review, methods landscape comparison, and quantitative
meta-analysis. Keep their evidence units and claims separate while sharing
retrieval, screening, citation, and provenance discipline.

## When to use

- **Narrative:** scoped state-of-knowledge review with a structured evidence table.
- **Preclinical:** per-study in-vitro/in-vivo model, assay, direction, and
  translational-gap extraction.
- **Deep audit:** exact sentence/page/figure anchors, claim-to-evidence matrix,
  contradiction/null search, and entailment re-check.
- **Methods landscape:** regime-conditional comparison or topic synthesis of
  published method evidence; do not execute the compared methods on user data.
- **Meta-analysis:** compatible between-group effect estimates with uncertainty.

Do not use narrative or preclinical modes for quantitative pooling. Do not use a
meta-analysis mode for single-arm, network, multivariate, individual-level, or
diagnostic-test-accuracy synthesis unless a dedicated contract is supplied.

## Inputs

- Question, decision, population/model, intervention/exposure, outcome, design,
  date window, scope boundaries, and desired evidence depth.
- Search facets, synonyms, candidate methods, or user-supplied citations/records.
- Narrative/preclinical/deep: returned records, paper identifiers, optional lawful
  open full text, and study-level fields.
- Deep: atomic claims with stable IDs and a locator/entailment ledger.
- Meta: one compatible effect measure per analysis, effect/CI or SE, sample
  sizes, estimand, design, source identifier, duplicate-cohort decisions, and
  risk-of-bias notes.

## Workflow

1. Clarify only missing scope and defaults. Choose one evidence mode and record
   filters, depth, access policy, and output contract.
2. Search multiple focused facets with synonyms; deduplicate by DOI, then
   normalized title. Keep inclusion/exclusion reasons and access status.
3. Ground claims in complete returned records, not search-result highlights.
   Use lawful open full text only when enabled; record full-text versus
   abstract-only support.
4. **Preclinical:** extract model, assay, dose/endpoints, direction, findings,
   and in-vitro/in-vivo concordance/translational gaps.
5. **Deep:** freeze a canonical corpus, bind every displayed claim to an exact
   resolvable supporting/contradicting anchor, and require a passing blind
   entailment re-check before display.
6. **Methods:** separate foundational, benchmark, and recent queries; rank
   evidence thickness and make recommendations conditional on data regime,
   sample size, quality, and design.
7. **Meta:** verify compatible measures and independent cohorts, fit the declared
   random-effects model, inspect heterogeneity/influence/leave-one-out/small-
   study diagnostics, and write structured risk-of-bias notes.

## Resource selection

- Prefer user-supplied records or a directly authoritative literature source
  whose registry/access record meets the scope contract.
- For lawful full-text location, the normalized sources identify `Europe PMC`
  and `Unpaywall`; use them only when the current resource registry records
  access and terms. Otherwise mark the adapter `UNKNOWN` and use record/abstract
  evidence without implying full text.
- Meta adapters include registry-recorded `meta`, `metafor`, and compatible
  random-effects/Hartung–Knapp methods. A documented DerSimonian–Laird option is
  a declared alternative, not a silent default.

See [resource selection policy](../_shared/resource_selection.md).

## Decision rules

- **LES-1:** Use multiple focused queries and DOI/title deduplication; do not
  write from one-line highlights or invent citations, identifiers, numbers, or
  study details.
- **LES-2:** Cite only a record that supports the adjacent claim. Label
  abstract-only evidence and never bypass access controls/paywalls.
- **LES-3:** Deep mode is shippable only when every displayed claim has an exact
  resolvable anchor and every displayed anchor has a passing entailment verdict;
  an abstract is not full-text evidence.
- **LES-4:** Methods recommendations are conditional, evidence-thickness-aware,
  and never an unconditional winner; do not execute compared methods in review
  mode.
- **LES-5:** Meta-analysis must not mix incompatible effect measures. Log-transform
  OR/RR/HR before pooling, prefer one independent arm/ITT-style estimand, and
  use random-effects REML with Hartung–Knapp confidence intervals and a prediction
  interval by default.
- **LES-6:** Investigate influential studies rather than deleting them; interpret
  funnel/Egger asymmetry as publication-bias evidence only with at least 10
  studies.

## Validation

- Verify scope, search coverage, duplicate/version decisions, record fields,
  access status, citation locators, evidence depth, and claim support.
- Preclinical: retain exact model/assay/direction fields and separate in-vitro,
  in-vivo, combined, and missing translational evidence.
- Deep: check claim/evidence IDs, exact anchors, scope match, entailment, and
  canonical artifact hashes.
- Methods: verify every quantitative value/citation field, source-bound figures,
  evidence thickness, and unresolved flags before reporting.
- Meta: check compatible measure/uncertainty, duplicate cohorts, heterogeneity,
  prediction interval, influence/leave-one-out, study count, and risk-of-bias
  narrative.

See [validation policy](../_shared/validation_policy.md),
[evidence policy](../_shared/evidence_policy.md), and
[provenance policy](../_shared/provenance_policy.md).

## Failure handling

If the corpus is sparse, add synonyms or loosen filters with a new scope note.
If full text is unavailable, fall back to abstract evidence and label it. If a
claim/effect/citation cannot be verified, drop it or mark it unresolved. Stop
meta-analysis on incompatible measures, duplicate cohorts, unsupported design,
or missing uncertainty. Preserve failed retrievals, exclusions, and partial
artifacts; never convert an empty search into an empty evidence conclusion.

See [failure handling policy](../_shared/failure_handling.md).

## Outputs

- Scope/search/corpus ledger with deduplication and inclusion decisions.
- Mode-specific evidence table and cited narrative; preclinical model/effect
  extraction; deep claim/evidence/entailment matrix; methods comparison and
  conditional decision table; or meta fitted model and diagnostics.
- Access/evidence-depth, risk-of-bias, uncertainty, limitations, and provenance
  records suitable for independent review.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection policy](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling policy](../_shared/failure_handling.md).

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`evidence-synthesis-meta-analysis`](references/source_workflows/evidence-synthesis-meta-analysis/WORKFLOW.md) — Pool compatible effect estimates across studies with a random-effects meta-analysis, robustness diagnostics, risk-of-bias assessment, and verified citations.
- [`literature-deep-review`](references/source_workflows/literature-deep-review/WORKFLOW.md) — Produce an audit-ready literature review in which every displayed claim is linked to an exact, resolvable supporting or contradicting source anchor and every displayed anchor receives a blinded entailment re-check.
- [`literature-preclinical`](references/source_workflows/literature-preclinical/WORKFLOW.md) — Search and narratively synthesize peer-reviewed non-clinical in vitro and in vivo evidence for a target–disease question, with structured study details, concordance analysis, translational-gap assessment, and traceable deliverables.
- [`literature-review`](references/source_workflows/literature-review/WORKFLOW.md) — Search and narratively synthesize peer-reviewed literature on a scoped topic, grounding claims in retrieved records and producing a cited review, structured evidence table, and report.
- [`methods-landscape-review`](references/source_workflows/methods-landscape-review/WORKFLOW.md) — Turn a method-choice or evidence-landscape question into a citation-verified, regime-conditional comparison or topic synthesis with structured screening, source-bound figures, decision guidance, and a validated report.

<!-- END MANAGED: SOURCE WORKFLOWS -->
