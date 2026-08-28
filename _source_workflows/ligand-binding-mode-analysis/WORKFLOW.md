# Ligand Binding Mode Analysis

Source workflow: `ligand-binding-mode-analysis`  
Parent Claude Science skill: `molecular-design-and-structure`

## Purpose

Map a small-molecule ligand's binding mode from an observed protein–ligand co-crystal structure, including pocket residues, geometric interactions, ligand-fragment contacts, optional cross-structure concordance, and auditable tabular and report outputs.

## When to use

- Characterize a ligand binding mode from a co-crystal structure.
- Enumerate binding-pocket residues and geometric contacts.
- Compare pocket contacts across structures.
- Create a contact-map analysis and report for a protein–ligand complex.

## Inputs

- One primary structure supplied as a PDB accession, a local PDB/CIF/ENT file, or a target-plus-ligand lookup request. (required)
- Ligand chemical-component code when automatic ligand selection is ambiguous. (optional)
- Optional comparison structures for concordance analysis. (optional)
- Interaction-depth choice: core interactions or extended interaction classes. (optional)

## Outputs

- Machine-readable pocket-contact table with distances, ligand atoms/fragments, interaction type, confidence, source, and optional comparison fields.
- Interaction diagram, contact-distance plot, fragment–residue heatmap, and optional three-dimensional pocket views.
- Validated PDF report containing methods, results, figures, conclusions, references, and next steps.

## Workflow

1. Confirm the primary structure, intended ligand, interaction depth, comparison structures, and desired deliverables.
2. Validate that the selected structure contains the intended bound small molecule and identify the relevant protein protomer.
3. Compute residue–ligand contacts and classify interactions using angle- and charge-aware geometry.
4. Tier interaction calls by confidence and retain the interaction engine/source for every call.
5. Map ligand fragments to contacted residues and optionally annotate conserved kinase motifs.
6. When comparison structures are supplied, quantify residue identity and contact-distance concordance.
7. Ground biological interpretation in retrieved literature and validate the table, figures, and report before delivery.

## Decision rules

- Do not use an apo structure for binding-mode analysis and do not substitute docking or pose prediction when no observed ligand pose exists.
- Treat all contacts as geometric assignments from a static structure rather than energies or affinity estimates.
- Use minimum heavy-atom distance; classify contacts within 4.0 Å as core and contacts within 4.5 Å as the wider shell.
- Require genuine formal charge for salt-bridge and pi-cation calls, and require sigma-hole directionality for high-confidence halogen bonds.
- Treat tentative interactions cautiously and confirm important donor/acceptor assignments in the three-dimensional structure.
- Copy quoted distances and counts from the generated contact table or report payload rather than re-deriving them from memory.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_d12d65769eea989c` — RCSB Protein Data Bank: Use for structure download or target-plus-ligand co-crystal selection.
- `rr_e5abacbbeacbd0d6` — Protein–Ligand Interaction Profiler (PLIP): Prefer for protonated, angle-aware interaction typing.

### Secondary resources

- `rr_95a6dd57ea90d518` — RDKit: Use for bond perception, ligand fragmentation, and formal-charge-aware interaction checks.
- `rr_709a33942840dc92` — PyMOL: Use for publication-quality three-dimensional pocket rendering when available.

### Fallback resources

- `rr_d2ae4d29497fbdfa` — Hardened angle- and charge-aware geometry classifier: Use when PLIP is unavailable.
- `rr_db175b6eb439f57e` — Graph-based ligand fragmenter: Use when RDKit bond perception or sanitization fails.
- `rr_61c42361d9f211c1` — Matplotlib three-dimensional view: Use when PyMOL is unavailable.

### Optional resources

- `rr_6f7f70ba8fad9b3a` — PLIP: Primary interaction-typing engine.
- `rr_5d8529d3ad92c7ab` — rdkit: Ligand chemistry, formal charges, and fragmentation.
- `rr_0fde0898fc1f5a5a` — biopython: Structure parsing and handling.
- `rr_0a3d723696169967` — openbabel: PLIP dependency and structure protonation support.
- `rr_f5811a5674b112f5` — PyMOL: Optional three-dimensional rendering.
- `rr_665edbde6fc3cd1c` — matplotlib: Plotting and fallback three-dimensional rendering.

## Validation / QC

- Record interaction_confidence and interaction_source for every contact row.
- Validate report structure, extractable text, minimum page count, and non-empty figures.
- Force the intended ligand code when multiple ligands or cofactors make automatic selection unreliable.
- Check important hydrogen bonds and charge-dependent interactions in the three-dimensional view.
- Evidence requirement: Describe interaction calls as geometry-based candidates, not energetic or affinity evidence.
- Evidence requirement: Use only real retrieved biological references; leave the reference list empty when no relevant records are found.
- Evidence requirement: State that crystallographic hydrogen positions are usually unobserved and that protonation-dependent ionic calls remain conditional.

## Failure handling

- No drug-like ligand is present or the structure is apo.
- The wrong ligand is selected in a structure with multiple ligands or cofactors.
- PLIP installation fails because its package build attempts to rebuild OpenBabel.
- Interaction classes are over-called because of missing formal charge or borderline geometry.
- Kinase motifs are mislabelled in atypical or non-standard sequences.
- Fallback rule: Pass an explicit ligand code when automatic ligand selection fails.
- Fallback rule: Fall back from PLIP to the hardened geometry classifier while preserving confidence tiers.
- Fallback rule: Use generic graph-derived fragment labels when chemical bond perception fails.
- Fallback rule: Use Matplotlib rendering when PyMOL cannot be loaded.

## Limitations

- A single static structure contains no dynamics, solvent screening, or affinity information.
- Hydrogen-bond assignments remain candidates because most X-ray structures do not observe proton positions.
- Salt-bridge and pi-cation calls depend on the modeled ligand protonation state.
- Ordered waters are excluded from the quantitative residue-contact map.
- Fragmentation and kinase annotation can be heuristic when structure chemistry or sequence context is atypical.

## Important domain-specific rules

- Observed-pose pocket-contact extraction with machine-readable residue-level provenance.
- Confidence-tiered, angle- and formal-charge-aware interaction classification.
- Ligand-fragment-to-residue mapping for structure-guided SAR hypotheses.
- Cross-structure contact concordance to distinguish reproducible contacts from packing artifacts.

## Portability boundary

- Packaged scripts, exact execution commands, verification strings, and internal output paths. — migration action: `exclude_or_capability_map`
- Platform LiteratureSearch invocation and reference-payload formatting. — migration action: `exclude_or_capability_map`
- Phylo-branded PDF layout plus platform-specific report and media-output-check tools. — migration action: `exclude_or_capability_map`
- Biomni environment assumptions and the /workspace PLIP installation recipe. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
