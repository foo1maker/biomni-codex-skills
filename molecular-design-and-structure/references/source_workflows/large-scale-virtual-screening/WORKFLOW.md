# Large Scale Virtual Screening

Source workflow: `large-scale-virtual-screening`  
Parent Claude Science skill: `molecular-design-and-structure`

## Purpose

Dock a compound library against a protein target with AutoDock Vina, validate the docking setup by native-ligand redocking, rank and triage hits, assess enrichment when labels exist, and summarize scaffold-level structure-activity patterns.

## When to use

- Structure-based virtual screening with AutoDock Vina
- Native-ligand redocking validation and docking-box quality assessment
- Labeled-library enrichment benchmarking with ROC-AUC, enrichment factor, and BEDROC
- Hit triage and scaffold-aware preliminary SAR summarization

## Inputs

- A PDB identifier or local protein PDB file (required)
- A compound library from user SMILES or CSV, a ChEMBL target pull, DUD-E actives and decoys, or an Enamine subset (required)
- A co-crystal ligand or an explicit docking-box override (optional)
- Optional compound activity labels, ADMET toggle, and literature-context toggle (optional)

## Outputs

- Master library, merged docking scores, and molecular-descriptor tables
- Docking-box and native-ligand redock validation records
- Top-hit and scaffold-cluster tables
- For labeled libraries, enrichment metrics and a ROC curve table
- Analysis figures and a standalone virtual-screening PDF report

## Workflow

1. Fetch or load the receptor, isolate a clean protein chain, and extract a native ligand when present.
2. Prepare receptor PDBQT and define a pocket-scale docking box around the native ligand or explicit coordinates.
3. Redock the native ligand and apply the core-aware pose-sanity gate before screening.
4. Build the library, auto-detect whether activity labels are present, and record source provenance.
5. Generate reproducible ligand conformers and PDBQT files, targeting at least 95 percent preparation success and logging failures.
6. Pilot docking throughput, produce an execution plan, obtain confirmation for a large fan-out, and then run local or distributed docking.
7. Run enrichment metrics only for labeled libraries, then perform property-aware hit triage and scaffold clustering.

## Decision rules

- If the library has activity labels, run enrichment benchmarking; otherwise omit enrichment, state that no ground truth exists, and deliver triage, SAR, and property flags.
- Use local docking for a small library or when distributed compute is unavailable; use pilot-measured fan-out for a larger library only after confirmation.
- A redock passes when the buried rigid-core RMSD is below 2 angstroms; a warning must remain visible when the pose is not reproduced.
- Use optional ADMET only as advisory context and never as a hard screen filter.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_a2bf1eb6eb45d281` — AutoDock Vina with a native-ligand-defined or explicitly specified docking box: Running the structure-based virtual-screening workflow

### Secondary resources

- `rr_c8075f719b794326` — DUD-E-style labeled actives and decoys: Benchmarking enrichment behavior with an explicitly labeled library

### Fallback resources

- `rr_eb0e5b5c415f0a9d` — Single-machine local docking: The library is small or distributed compute is unavailable

### Optional resources

- `rr_d12d65769eea989c` — RCSB Protein Data Bank: Protein structures and co-crystal ligands
- `rr_806458b94ba25e08` — ChEMBL: Optional target-based compound-library source with attribution and share-alike obligations
- `rr_6ccbae9c3762cb84` — DUD-E: Labeled actives and decoys for benchmarking
- `rr_fe1817636475391d` — Enamine REAL: Optional larger compound-library source subject to Enamine terms
- `rr_4e25afd1b941072b` — AutoDock Vina: Ligand docking and scoring
- `rr_5d8529d3ad92c7ab` — rdkit: Ligand preparation, descriptors, Bemis-Murcko scaffolds, and Butina clustering
- `rr_9212b5e89489b4d2` — Meeko: Ligand and fallback receptor PDBQT preparation
- `rr_03dfb2d6ce62ed06` — gemmi: Structure-processing dependency
- `rr_943eb66d93b88ec4` — TDC or DeepPurpose ADMET models: Optional advisory property context for top hits

## Validation / QC

- Run every script self-check before a real screen.
- Validate the docking box with native-ligand redocking and record the exact core atoms used for RMSD.
- Target at least 95 percent ligand-preparation success and retain an auditable failure log.
- Measure throughput with a pilot rather than assuming a constant per-ligand docking rate.
- Evidence requirement: Present docking scores as ranking signals rather than absolute binding affinities.
- Evidence requirement: Keep optional literature context walled off from computed numeric results.
- Evidence requirement: State redock warnings prominently and require orthogonal rescoring and experimental validation for top hits.

## Failure handling

- Failure to reproduce a native pose indicates that the box, protonation, chain choice, or scoring setup may be unsuitable.
- Ligand-preparation failures can reduce library coverage and bias the screened set.
- Streaming writes to the S3-backed result paths can produce invalid or zero-byte outputs in the stated runtime.
- Fallback rule: If distributed compute is unavailable, run the docking workload locally.
- Fallback rule: If labels are absent, omit enrichment and explicitly state that ground-truth benchmarking was not possible.
- Fallback rule: If ADFR receptor preparation is unavailable, use the stated Meeko or OpenBabel fallback and record the route used.

## Limitations

- The receptor is treated as a single rigid conformation without induced fit or side-chain flexibility.
- Vina scores have approximately 2 to 3 kcal/mol error and are unreliable as absolute affinities.
- The workflow uses one tautomer, protonation state, and conformer per ligand and does not include explicit waters.
- Decoys in labeled runs are presumed inactive rather than confirmed non-binders, so enrichment can be optimistic.
- Docking prioritizes compounds for follow-up and does not confirm binding or potency.

## Important domain-specific rules

- Native-ligand redock validation as a visible gate before screening.
- Automatic branch between labeled enrichment benchmarking and label-free triage.
- Pilot-measured capacity planning and confirmation before large-scale fan-out.
- Scaffold-aware hit triage with property-bias diagnostics and advisory-only ADMET.
- Walled-off literature context that cannot overwrite computed results.

## Portability boundary

- ManageMachine provisioning and multi-worker orchestration — migration action: `exclude_or_capability_map`
- Biomni predict_admet_properties and LiteratureSearch tool calls — migration action: `exclude_or_capability_map`
- GenerateImage, Phylo-specific PDF styling, and platform media-output checks — migration action: `exclude_or_capability_map`
- Hard-coded /opt/conda, /workspace, /mnt/results, and /mnt/shared-workspace runtime assumptions plus S3 FUSE workarounds — migration action: `exclude_or_capability_map`
- Mandatory bundled-script entry points and fixed run-directory conventions — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
