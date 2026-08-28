# Binder Antibody Design

Source workflow: `binder-antibody-design`  
Parent Claude Science skill: `antibody-and-protein-engineering`

## Purpose

Design and computationally prioritize de novo mini-protein binders or framework-based antibody and nanobody CDR designs against a protein target.

## When to use

- Design a de novo mini-protein binder against a target surface.
- Design antibody or nanobody CDR loops on an existing framework.
- Rank computational candidates by predicted interface confidence and on-target epitope recovery.

## Inputs

- A target PDB or mmCIF structure, or a UniProt/AlphaFold model, with target chain and residue range. (required)
- Target epitope or hotspot residues and any functional site to track. (optional)
- Binder length range for the de novo mini-protein track. (optional)
- An HLT-formatted antibody framework and CDR loop-length ranges for the antibody or nanobody track. (optional)
- A campaign scale preset. (optional)

## Outputs

- Ranked candidate table with confidence, interface, hotspot-recovery, and epitope-status fields.
- Per-candidate metrics, native-numbered target contacts, sequences, and construct-scope metadata.
- Fetched structure metadata with provenance.
- Figures, validated report content, final report, and selected FASTA sequences.

## Workflow

1. Choose the mini-protein or antibody/nanobody track from the requested modality; ask when the modality is unclear.
2. Crop and validate the target construct while preserving required residues and recording hotspots and native scope.
3. Generate de novo backbones, redesign binder sequences with the target fixed, and filter for sequence complexity, cysteine burden, and interface contacts.
4. Co-fold selected candidates with the target, analyze directional interface PAE, map native residue numbering, score hotspot recovery, and rank candidates.
5. For antibody or nanobody designs, prepare an HLT framework, diversify CDR backbones, design CDR interface sequences, and filter with RF2 pAE and self-consistency RMSD.
6. Derive report claims from construct, structure, ranking, and metrics artifacts; run a consistency gate before rendering the final report.

## Decision rules

- Use the mini-protein track for binders or de novo proteins and the framework-based track for antibodies, nanobodies, VHH, scFv, or CDR design.
- Ask whether to target a defined epitope or run unrestrained design when no hotspot set is provided; hotspots are effectively required for the antibody track.
- Treat ipTM and PAE as predicted geometry confidence rather than experimental affinity.
- Report hotspot recovery separately from confidence; a high-confidence design with zero declared hotspots recovered is OFF_TARGET.
- Use native target numbering by setting the construct start correctly before reporting epitope residues.
- Pilot antibody campaigns before scaling to large design counts.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_79bb928e6ba23051` — Experimental PDB or mmCIF target structure: A suitable target structure, chain, and domain or epitope are available.

### Secondary resources

- `rr_5e3c7e84dd88629d` — UniProt or AlphaFold target model: No suitable local or PDB target structure is supplied.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_d12d65769eea989c` — RCSB Protein Data Bank: Source for structure resolution, method, deposition date, title, chain composition, and provenance.
- `rr_66e05683356bf73d` — UniProt: Identifier and model source when a PDB or local structure is not supplied.
- `rr_c0fd048a6b44a52d` — AlphaFold Protein Structure Database: Predicted target structure source when used instead of an experimental structure.
- `rr_8b81221d6634a785` — RFdiffusion: Generate de novo mini-protein binder backbones.
- `rr_67b81caacb4ec5f3` — ProteinMPNN: Design binder or CDR interface sequences while holding target or framework chains fixed.
- `rr_05b204abaf4052d3` — Boltz-2: Co-fold and score mini-protein binder–target complexes.
- `rr_69d869c5544a6f36` — RFantibody: Antibody-finetuned CDR backbone, sequence, and RF2 prediction pipeline.
- `rr_112dd815c618da5a` — RFdiffusion Complex_base checkpoint: Protein–protein binder backbone generation.
- `rr_b5b07fd06b51e236` — RFdiffusion_Ab checkpoint: Antibody-finetuned CDR backbone generation.
- `rr_f98ee4f9d1976086` — RF2: Predict and filter antibody or nanobody designs by pAE and self-consistency.

## Validation / QC

- Verify each stage output before launching the next compute-intensive job.
- Use construct_scope.json as the source of construct span and structure_metadata.json as the source of external structure facts.
- Require candidate, metric, hotspot, figure, construct-scope, and structure-provenance consistency before reporting.
- Flag OFF_TARGET and NOT_ASSESSED candidates separately from geometry-confidence tiers.
- Evidence requirement: Derive campaign counts, structure facts, construct span, and candidate metrics from generated artifacts rather than hand-typing them.
- Evidence requirement: Present computational candidates as hypotheses requiring expression, binding, and functional or competition assays.
- Evidence requirement: Retain the raw sign of MPNN-score versus ipTM correlation and explain that a negative correlation reflects metric agreement.

## Failure handling

- The wrong target face, blocking domain, or crop discards a required residue.
- The wrong chain is redesigned, silently altering the target instead of the binder.
- Incorrect native-domain start mislabels every reported epitope residue.
- A structurally confident design is off-target because it recovers none of the declared hotspots.
- The requested modality is routed to the wrong design track.
- Fallback rule: If no epitope is specified for the mini-protein track, clarify whether to use unrestrained design rather than assuming a target surface.
- Fallback rule: If fetched structure facts are unavailable, report them as unavailable instead of hand-typing values.

## Limitations

- The workflow does not predict experimental binding affinity, optimize small molecules, design multispecifics or conjugates, or perform wet-lab validation.
- ProteinMPNN score measures sequence recoverability, not binding.
- Antibody-track developability, immunogenicity, aggregation, and expressibility are not assessed.
- All outputs are computational hypotheses rather than proof of binding.

## Important domain-specific rules

- Route design by modality before selecting a structural generation workflow.
- Preserve native construct scope and declared hotspot provenance across crop, design, validation, ranking, and reporting.
- Keep geometry confidence, epitope recovery, and experimental affinity as separate evidence axes.
- Generate report claims from machine-readable artifacts and enforce cross-artifact consistency.
- Pilot hotspot and loop-length choices before scaling an antibody campaign.

## Portability boundary

- Biomni HPC helper functions and named HPC tool wrappers. — migration action: `exclude_or_capability_map`
- Bundled skill-local scripts, container flags, concurrency limits, and fixed command recipes. — migration action: `exclude_or_capability_map`
- Biomni /mnt/results output convention and Phylo report branding. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation and GenerateImage orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
