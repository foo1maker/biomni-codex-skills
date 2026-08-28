---
name: molecular-design-and-structure
description: Route protein structure prediction, observed ligand-contact analysis, structure-based virtual screening, generative small-molecule design, and bifunctional-degrader triage with explicit confidence and evidence boundaries.
---

# Purpose

Provide a framework-neutral router for molecular structure and design tasks.
Keep structure prediction, observed binding-mode analysis, virtual screening,
de novo generation, and degrader prioritization as distinct modes with their
own inputs, validation gates, and claims.

# When to use

Use when the user asks for one of these modes:

- predict a protein structure or compare predictor confidence;
- characterize contacts from an observed protein–ligand co-crystal;
- dock a labeled or unlabeled compound library to a protein;
- generate target-focused small-molecule hypotheses; or
- assemble and triage bifunctional degraders.

Choose the mode before selecting a resource. Do not use an apo structure for an
observed binding-mode question, and do not treat a docking or heuristic score
as measured affinity or degradation potency.

# Inputs

- Structure prediction: sequence, accession, FASTA, or explicitly described
  multi-chain/ligand system; record trimming and coordinate mapping.
- Observed binding mode: a structure containing the intended bound ligand,
  optional ligand code, and optional comparison structures.
- Virtual screening: receptor PDB, compound library, co-crystal ligand or
  explicit box, and optional activity labels.
- Generative design: target, seed/reference molecules, and a defensible
  activity backend or data to build one; optional structure/pocket.
- Degrader design: target identifier, optional E3/warhead/linker constraints,
  and optional target structure.

# Workflow

1. Identify the mode and record the input contract, target identifiers,
   structures, sequences, libraries, and requested deliverables.
2. Resolve identity and provenance through the resource registry. Record
   versions, access, license status, and whether evidence is measured,
   literature-derived, computed, or prior-encoded.
3. For structure prediction, use the size-first route: ESMCFold2 for a
   single-chain monomer at or below 400 aa by default and AlphaFold v2 for a
   longer monomer. Route complexes or ligand/nucleic-acid systems to a
   documented Boltz-2/Chai-1 manual path. Export pLDDT bands, available pTM,
   domain summaries, and a run/fallback manifest.
4. For observed binding mode, verify the ligand is present, compute
   charge/angle-aware contacts, attach confidence and interaction source to
   every row, and optionally compare structures. Never substitute docking.
5. For virtual screening, prepare receptor/library, define a pocket-scale box,
   redock the native ligand, then screen. Use enrichment metrics only when
   labels exist; cluster and triage hits after recording preparation failures.
6. For generative design, select activity backend in this order: target oracle,
   QSAR with labeled actives/inactives, Vina with structure/pocket, or stop and
   gather data. Generate valid connected structures, score normalized
   objectives, filter novelty/chemistry, and run retrosynthesis only on a
   bounded top subset.
7. For degrader design, build a precedent dossier, canonicalize attachment
   representations, dock only actually modeled warheads, assemble valid
   warhead-linker-recruiter combinations, and publish transparent score
   components. Keep computed, literature, and prior evidence separate.

# Resource selection

Consult `../../03_resource_registry/resource_registry.yaml`; no resource is
assumed to be installed, reachable, or licensed for redistribution. Select
only entries whose input contract and access status match the mode.

- Identity/structure: RCSB Protein Data Bank and UniProt; AlphaFold v2,
  Boltz-2, Chai-1, or ESMCFold2 for documented prediction routes.
- Observed contacts: PLIP with RDKit/Biopython; use a charge/angle-aware
  geometry fallback and Matplotlib rendering if optional visualization is not
  available.
- Virtual screening: AutoDock Vina with RDKit and Meeko; use DUD-E labels for
  benchmarking only when present. Record the receptor-preparation route.
- Generation: RDKit, a target-specific oracle or labeled QSAR, AutoDock Vina,
  and AiZynthFinder only when each is available and its model status is known.
- Degrader evidence: ChEMBL, PubChem, UniProt, RCSB PDB, and Human Protein
  Atlas as applicable. ADFR Suite is non-commercial unless separately
  licensed; use an open preparation path when needed.

# Decision rules

- Keep confidence from different predictors labeled by predictor; pLDDT is
  self-estimated confidence, not experimental accuracy. Use bands: very high
  >=90, confident 70–<90, low 50–<70, very low <50.
- Observed contacts are static geometry. Core contacts use minimum heavy-atom
  distance <=4.0 Å and the wider shell <=4.5 Å; charge-dependent calls require
  formal charge and halogen-bond calls require directional support.
- A native redock passes the stated gate when buried rigid-core RMSD <2 Å;
  retain a visible warning otherwise. Docking scores rank candidates and are
  not absolute affinities.
- For generation, reject invalid/disconnected structures, track validity,
  uniqueness, novelty, PAINS/ring sanity, and synthetic-accessibility; the
  archived default novelty and SA cutoffs are Tanimoto <0.4 and SA <=4.5.
- For degraders, a transparent score is not a calibrated DC50/Dmax or potency
  prediction, and a prior-matching molecule is not independent rediscovery.

# Validation

- Confirm sequence/structure identity, chain/ligand selection, box definition,
  library coverage, attachment representations, and source versions.
- Require a run/fallback manifest for prediction; omit domain breakdowns when
  UniProt feature ranges are unavailable rather than inventing boundaries.
- Record interaction source/confidence for contact rows, redock core atoms and
  RMSD, ligand-preparation success/failures, and labeled-versus-unlabeled
  screening status.
- For generated and degrader candidates, export every score component and
  evidence class, plus model domain/uncertainty where available.
- Apply the output gate to tables, figures, and reports. State that designs,
  docking ranks, contacts, and predicted properties require follow-up.

# Failure handling

Block an apo binding-mode input, absent target identity, invalid molecule, or
missing required structure/library. Use only declared fallbacks: bounded
predictor fallback, hardened contact geometry, local docking, Meeko/OpenBabel
preparation, a lower-tier activity backend, or synthetic-accessibility in lieu
of unavailable retrosynthesis. Preserve failed rows and changed evidence class.
If access or license terms are unknown or restrictive, catalog the resource but
do not use it until the decision is resolved.

# Outputs

Return mode-specific tables and artifacts: structures/confidence and manifests;
contact tables and plots; docking scores, redock records, enrichment and
scaffold tables; generated molecules and score/provenance tables; or degrader
building-block/library/shortlist artifacts. Include methods, limitations,
source URLs, versions, access/license decisions, and validation status.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`generative-molecule-design`](references/source_workflows/generative-molecule-design/WORKFLOW.md) — Generate target-focused small-molecule hypotheses with graph-based evolution, multi-objective scoring, novelty and chemistry filters, and optional retrosynthesis assessment.
- [`large-scale-virtual-screening`](references/source_workflows/large-scale-virtual-screening/WORKFLOW.md) — Dock a compound library against a protein target with AutoDock Vina, validate the docking setup by native-ligand redocking, rank and triage hits, assess enrichment when labels exist, and summarize scaffold-level structure-activity patterns.
- [`ligand-binding-mode-analysis`](references/source_workflows/ligand-binding-mode-analysis/WORKFLOW.md) — Map a small-molecule ligand's binding mode from an observed protein–ligand co-crystal structure, including pocket residues, geometric interactions, ligand-fragment contacts, optional cross-structure concordance, and auditable tabular and report outputs.
- [`protein-structure-prediction`](references/source_workflows/protein-structure-prediction/WORKFLOW.md) — Predict a protein structure with AlphaFold v2, Boltz-2, Chai-1, or ESMCFold2 and report normalized per-residue confidence, available global confidence, confidence-band composition, domain-resolved confidence from UniProt annotations, and predictor/fallback provenance.
- [`targeted-degrader-design`](references/source_workflows/targeted-degrader-design/WORKFLOW.md) — Design and triage bifunctional degraders while keeping computed chemistry, docking, and property results separate from precedent-based prioritization heuristics.

<!-- END MANAGED: SOURCE WORKFLOWS -->
