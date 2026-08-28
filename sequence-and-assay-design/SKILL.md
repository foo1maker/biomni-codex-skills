---
name: sequence-and-assay-design
description: Design and audit PCR primers or CRISPR guide sequences from a declared reference sequence, genome build, assay target, and nuclease. Use when a user needs candidate oligos, specificity and thermodynamic checks, guide scoring, or evidence-backed assay selection; keep primer and guide modes separate.
---

# Sequence and assay design

## Purpose

Turn a declared target and reference into auditable candidate oligos while
keeping PCR primer design and CRISPR guide design as distinct modes. Report
constraints, failed candidates, reference provenance, and predicted versus
validated evidence.

## When to use

Use for PCR/qPCR, amplicon, sequencing-validation, or CRISPR guide design.
Route to a sequence-alignment or wet-lab planning workflow when the target
sequence, genome build, nuclease/PAM, or intended assay cannot be specified.
This skill proposes candidates; it does not claim experimental performance.

## Inputs

- Declare `pcr_primer` or `crispr_guide` mode and the assay objective.
- PCR: target sequence or accession, strand/orientation, amplicon interval or
  size, primer length/Tm/GC constraints, and any overhang or multiplex rules.
- CRISPR: genome build, target locus/transcript, nuclease and PAM, edit type,
  guide length, acceptable off-target profile, and cell/organism context.
- Optional validated oligo pool or literature records; retain accession,
  version, PMID/DOI, validation context, and sequence exactly.

## Workflow

1. Normalize the target identifier and retrieve or validate the reference
   sequence. Freeze the sequence hash, build, transcript choice, and strand.
2. Generate candidates under the declared mode. For PCR, enforce amplicon
   boundaries, primer Tm/GC/length, hairpin/dimer checks, and product size.
   For guides, enforce PAM/nuclease rules, target-window constraints, and edit
   orientation.
3. Align candidate primers or guides against the relevant reference and report
   predicted off-targets, mismatches, amplicons, or alternate alleles.
4. Rank candidates with transparent criteria. Separate computational scores,
   database observations, and literature-validated candidates.
5. Export a candidate table, rejected-candidate reasons, reference manifest,
   and an assay-readiness checklist for human review.

## Resource selection

Resolve optional sequence databases, aligners, primer-design libraries, guide
libraries, and genome annotations through the resource registry. Prefer a
user-supplied sequence or an official accession with a pinned version. If a
remote resource is unavailable, use a documented local reference or return
unresolved candidates; do not silently switch organism, build, transcript, or
nuclease. Record access and license status for any redistributed sequence or
validated guide list.

## Decision rules

- Never merge PCR and CRISPR candidate scores or quality thresholds.
- Choose a transcript and amplicon boundary before primer ranking; choose
  nuclease, PAM, strand, and edit type before guide ranking.
- Treat predicted specificity, activity, and off-target scores as model
  predictions. A validated guide or primer requires a source record and assay
  context, not just a high score.
- Penalize unresolved reference bases, alternate loci, common variants, and
  untested multiplex interactions; preserve them as explicit flags.
- Do not report a candidate as experimentally verified without a cited record.

## Validation

Verify sequence hashes, coordinates, strand, build, transcript, PAM, oligo
length/Tm/GC constraints, amplicon size, uniqueness, off-target enumeration,
and duplicate candidates. For validated oligos, verify sequence, source
identifier, organism/cell context, and reported assay details. Include a manual
review gate before ordering or experimentation.

## Failure handling

- If the reference or build is missing, stop with the exact unresolved
  identifier and do not infer coordinates.
- If no candidate satisfies hard constraints, return the nearest rejected
  candidates with failed checks and ask which constraint may be relaxed.
- If off-target search is incomplete, label specificity as unassessed and do
  not rank it as safe.
- If a guide library or primer service is unavailable, fall back to transparent
  sequence-based candidate generation only when the user accepts a prediction;
  otherwise stop.
- If transcript, allele, or nuclease choices conflict, preserve both choices
  as separate design branches rather than combining them.

## Outputs

Return mode-labelled candidate tables, sequence/build/reference manifests,
thermodynamic or guide scores, off-target/amplicon checks, rejected-candidate
reasons, evidence citations, and a clear predicted-versus-validated label.

## Shared policies

Apply [evidence policy](../_shared/evidence_policy.md),
[resource selection](../_shared/resource_selection.md),
[provenance policy](../_shared/provenance_policy.md),
[validation policy](../_shared/validation_policy.md), and
[failure handling](../_shared/failure_handling.md) to both design modes.

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`pcr-primer-design`](references/source_workflows/pcr-primer-design/WORKFLOW.md) — Design PCR, qPCR, TaqMan, multiplex, sequencing, or allele-specific primers and probes with application-specific constraints, explicit specificity status, structural checks, and MIQE-oriented documentation.
- [`sgrna-design`](references/source_workflows/sgrna-design/WORKFLOW.md) — Find or design guide RNAs by prioritizing experimentally validated sequences, then precomputed designs, and using de-novo design only as a last resort.

<!-- END MANAGED: SOURCE WORKFLOWS -->
