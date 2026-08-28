# Target Tractability Druggability

Source workflow: `target-tractability-druggability`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Assess whether a human protein-coding target is druggable and identify the most viable and emerging therapeutic modalities by combining tractability, structural, clinical, safety, and literature evidence.

## When to use

- Assess small-molecule, antibody, degrader, and other clinical-modality tractability for one target.
- Detect and score structural pockets on an experimental or predicted target structure.
- Produce a transparent modality scorecard and distinguish the most viable modality from an emerging frontier.

## Inputs

- A human target gene symbol or Ensembl gene identifier. (required)
- An optional disease name or EFO/MONDO identifier for target-disease context. (optional)
- An optional PDB identifier to override automatic structure selection. (optional)
- An optional modality focus or disease-specific narrative scope. (optional)

## Outputs

- An evidence bundle containing target identity, tractability buckets, essentiality, known drugs, safety liabilities, data version, and optional disease association.
- A per-modality scorecard with most-viable and frontier verdicts.
- Pocket-analysis results when a usable structure is available.
- Tractability, pocket, and modality-score visualizations plus a final evidence report.

## Workflow

1. Resolve the target, verify that it is a human protein-coding gene, and retrieve tractability, essentiality, drug, safety, and optional disease evidence.
2. Select a ligand-bound, high-resolution experimental structure when possible and fall back to an AlphaFold model when no suitable PDB structure exists.
3. Clean the selected structure, identify a reference-ligand centroid when available, and detect structural pockets.
4. Combine tractability, structure, and clinical precedent in a transparent zero-to-three score for each modality.
5. Gather target-specific literature on modality, mechanism, and structural context and include only real citations.
6. Report the evidence, scorecard, structural findings, caveats, references, and next steps, omitting unavailable structural figures.

## Decision rules

- Stop rather than force a verdict when the target is non-human, non-protein, or not protein-coding.
- Force the antibody modality score to zero when no surface-accessibility evidence exists.
- Report both the highest-scoring mature modality and the strongest emerging modality.
- State the Open Targets data version because field names and evidence change between releases.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_ae5593ad15b85ec6` — Ligand-bound, high-resolution experimental PDB structure: A suitable experimental structure exists for the target.
- `rr_32630d3a91f70230` — Open Targets Platform evidence: A human protein-coding target can be resolved.

### Secondary resources

- `rr_0150835f9df7de41` — AlphaFold structural model: No suitable experimental PDB structure is available.

### Fallback resources

- `rr_cceb727c38e76f09` — Non-structural tractability, clinical, safety, and literature evidence: Neither an experimental structure nor an AlphaFold model is available.

### Optional resources

- `rr_1cd8978f9c801055` — Open Targets Platform: Target identity, tractability buckets, essentiality, known drugs, safety liabilities, and optional disease association.
- `rr_d2ee5ce6f038e497` — DepMap: Gene-effect essentiality evidence, where more negative values indicate stronger essentiality.
- `rr_d12d65769eea989c` — RCSB Protein Data Bank: Experimental target structures and bound-ligand context.
- `rr_c0fd048a6b44a52d` — AlphaFold Protein Structure Database: Predicted-structure fallback.
- `rr_283ffe06e058e90e` — fpocket: Geometry-based pocket detection and druggability scoring.
- `rr_4865da24bde00d31` — AlphaFold model: Lower-confidence structural fallback when no experimental structure is available.

## Validation / QC

- Confirm target biotype and data version before interpreting the evidence.
- Classify bound ligands by heavy-atom count rather than relying on unreliable structure-service ligand flags.
- Treat pocket numbers as one-indexed when parsing fpocket outputs.
- Evidence requirement: Base the verdict on named tractability buckets, structural evidence when available, clinical precedent, safety, essentiality, and real literature citations.
- Evidence requirement: Label a favorable pocket or tractability signal as a hypothesis for experimental follow-up rather than a validated site.

## Failure handling

- The target is not a human protein-coding gene.
- No experimental or predicted structure is available for pocket analysis.
- fpocket is unavailable and cannot be installed.
- Fallback rule: Use an AlphaFold model when no suitable experimental structure exists and label its pocket results as less reliable.
- Fallback rule: Skip the structural section explicitly when no structure or pocket detector is available and continue with non-structural evidence.

## Limitations

- Tractability buckets are heuristic flags and do not prove that a program will succeed.
- Pocket scores depend on structure geometry and conformation; a ligand-bound structure can capture an already-open pocket.
- Predicted-structure pockets are less reliable than experimental-structure pockets.
- The known-drug list is a snapshot rather than an exhaustive competitive landscape.

## Important domain-specific rules

- Verify that a target fits the biological scope before scoring tractability.
- Use an explicit experimental-structure-to-predicted-structure-to-no-structure fallback chain.
- Combine independent evidence dimensions in a transparent modality scorecard while retaining the underlying evidence and caveats.

## Portability boundary

- Bundled skill-local Open Targets, structure, fpocket, scorecard, figure, and report scripts and their fixed paths. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch, GenerateImage, and Read media-output-check calls. — migration action: `exclude_or_capability_map`
- Biomni-specific /workspace and /mnt/results paths and Phylo report branding. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
