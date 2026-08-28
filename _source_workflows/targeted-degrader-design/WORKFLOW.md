# Targeted Degrader Design

Source workflow: `targeted-degrader-design`  
Parent Claude Science skill: `molecular-design-and-structure`

## Purpose

Design and triage bifunctional degraders while keeping computed chemistry, docking, and property results separate from precedent-based prioritization heuristics.

## When to use

- Select target-binding warheads, E3 recruiters, exit vectors, and linker families from evidence.
- Assemble and quality-control a combinatorial bifunctional-degrader library.
- Rank candidates with a transparent heuristic based on precedent, exit-vector quality, beyond-rule-of-five properties, predicted ADMET, and similarity.
- Optionally model target-degrader-E3 ternary complexes with explicit linker-aware structural methods.

## Inputs

- A target gene or protein name, ChEMBL identifier, or UniProt identifier. (required)
- Optional E3 ligase, warhead-source, linker, and administration-route constraints. (optional)
- An optional target structure, otherwise selected from RCSB PDB. (optional)

## Outputs

- Tables for building blocks, the assembled library, descriptors, predicted properties, heuristic scores, shortlist, and cited reference degraders.
- Figures covering building blocks, docking pose, property envelope, predicted-property heatmap, score decomposition, shortlist structures, and linker trends.
- Receptor, pocket, raw predicted-property, score-weight, and property-envelope data artifacts.
- A report that states the prioritization honesty boundary, dependencies, licensing, failures, limitations, references, and validation needs.

## Workflow

1. Confirm the target and build a precedent dossier recording E3 recruiter, warhead chemotype, exit vector, reported degradation measures, and DOI for each precedent.
2. Choose warheads, mark attachment atoms, canonicalize attachment representations, and remove or redesign any duplicate.
3. Dock each actually modeled warhead, compare its pose with a crystallographic ligand, and identify solvent-exposed exit-vector atoms from local receptor contacts.
4. Define E3 recruiters and linker families with junction chemistry that avoids duplicated heteroatoms at attachment points.
5. Assemble every warhead-linker-recruiter combination and retain only sanitized molecules containing both pharmacophore cores and chemically valid junctions.
6. Compute descriptors and predicted properties, treating predictions outside drug-like chemical space as relative rankings.
7. Calculate and publish the weighted prioritization score, form a balanced E3-recruiter shortlist, and flag property liabilities.

## Decision rules

- Never present the heuristic prioritization score as a calibrated potency, DC50, or Dmax prediction.
- Do not treat a prior-driven ranking that matches known best-in-class chemistry as an independent rediscovery.
- Assign a computed exit vector only to a docked warhead; label literature-derived exit vectors explicitly.
- Exclude assembled candidates containing invalid perester, peroxide, hydroxylamine, or hydrazine junctions.
- Treat ADFR Suite receptor and ligand preparation as non-commercial unless a commercial license is obtained, or substitute an open-source preparation method.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_77d11f9e3c53ddd4` — Target and precedent records from ChEMBL, PubChem, UniProt, RCSB PDB, and cited literature: Building the target, warhead, recruiter, exit-vector, and precedent dossier.

### Secondary resources

- `rr_5a2a17316597537d` — Human Protein Atlas normal-tissue expression: Tissue selectivity or off-tissue liability is being assessed.

### Fallback resources

- `rr_04841c1b3a4ecfec` — Model-knowledge precedents labeled as not tool-verified: The literature-search capability is unavailable or returns no relevant records.

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Target confirmation, bioactivity, and molecular representations; CC BY-SA attribution and share-alike obligations apply to redistributed derived data.
- `rr_497c4a00432d3cd4` — PubChem: Public-domain warhead, E3-ligand, and reference-degrader molecular representations.
- `rr_d12d65769eea989c` — RCSB Protein Data Bank: CC0 target structures for docking.
- `rr_66e05683356bf73d` — UniProt: Target identity and accession cross-reference under CC BY 4.0 attribution.
- `rr_29c26e96edaa2f32` — Human Protein Atlas: Optional normal-tissue expression context with release-specific attribution and share-alike obligations.
- `rr_5d8529d3ad92c7ab` — rdkit: Molecular assembly, sanitization, canonicalization, substructure checks, and descriptor calculation.
- `rr_4e25afd1b941072b` — AutoDock Vina: Warhead docking for pose and exit-vector analysis.
- `rr_0d8a4ec7c0d18e90` — deeppurpose: Predicted ADMET endpoints used as relative rankings outside drug-like chemical space.
- `rr_c44febac1bce6796` — ADFR Suite: PDBQT preparation restricted to non-commercial use unless separately licensed.
- `rr_d7c26e6edc13d5c3` — Boltz-2 or Chai-1: Optional cofolding engines for target-degrader-E3 structural hypotheses.
- `rr_b03c057b0f6af1b5` — BoltzGen: Optional binder-design route for peptide or protein-binder modalities.

## Validation / QC

- Require unique canonical attachment representations for all warheads before assembly.
- Report valid-molecule, both-core-intact, and chemically-valid-junction counts against the total library.
- Cross-check each DOI consistently across precedent tables, report references, and figure captions.
- Describe predicted-property failures by canonical molecular representation and all labels sharing that representation.
- Evidence requirement: Every precedent record must carry a DOI or an explicit not-tool-verified label.
- Evidence requirement: Label every score component as computed, literature-derived, or prior-encoded and publish the weights.
- Evidence requirement: Treat all designs as prioritizations requiring experimental validation.

## Failure handling

- Different warhead labels collapse to the same canonical attachment representation.
- Linker attachment definitions create invalid heteroatom junctions.
- Predicted-property models fail or extrapolate for large beyond-rule-of-five candidates.
- The selected preparation dependency is incompatible with the intended commercial use.
- Fallback rule: When a warhead attachment collision is found, choose a distinct attachment point or remove the duplicate warhead and document the decision.
- Fallback rule: When the non-commercial preparation suite cannot be used, substitute Meeko, OpenBabel, or an RDKit-based open-source preparation path.
- Fallback rule: When literature search is unavailable, label fallback precedents as not tool-verified rather than claiming a search was performed.

## Limitations

- The prioritization score embeds literature priors and is not calibrated to degradation potency or efficacy.
- Predicted ADMET values are extrapolations in beyond-rule-of-five chemical space and support relative ranking only.
- Warhead docking does not model the productive target-degrader-E3 ternary complex.
- Direct-bond assembly simplifies real conjugation chemistry and does not guarantee synthetic accessibility.

## Important domain-specific rules

- Separate computed evidence from literature-derived and prior-encoded evidence throughout scoring and reporting.
- Canonicalize component representations before combinatorial assembly to prevent duplicate chemistry under different labels.
- Apply explicit structure, core-retention, and junction-chemistry quality gates to every assembled candidate.
- Publish score weights and distinguish heuristic ranking from predictive calibration.

## Portability boundary

- Biomni LiteratureSearch execution-trace requirements and fixed references.jsonl parsing path. — migration action: `exclude_or_capability_map`
- Biomni Skill, GenerateImage, Read media-output-check, and pdf-report-generation call-order guardrails. — migration action: `exclude_or_capability_map`
- Biomni-specific /workspace, /mnt/results, and execution-trace paths plus FUSE workarounds. — migration action: `exclude_or_capability_map`
- Phylo-specific report branding, filenames, and final-chat response contract. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
