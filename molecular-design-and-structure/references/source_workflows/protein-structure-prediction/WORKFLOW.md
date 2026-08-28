# Protein Structure Prediction

Source workflow: `protein-structure-prediction`  
Parent Claude Science skill: `molecular-design-and-structure`

## Purpose

Predict a protein structure with AlphaFold v2, Boltz-2, Chai-1, or ESMCFold2 and report normalized per-residue confidence, available global confidence, confidence-band composition, domain-resolved confidence from UniProt annotations, and predictor/fallback provenance.

## When to use

- Predict a single-chain monomer structure and extract per-residue confidence.
- Run a documented manual multimer workflow for protein complexes.
- Compare confidence profiles from multiple predictors when explicitly requested.
- Produce confidence-band and UniProt-domain-resolved summaries with an auditable report.

## Inputs

- Raw amino-acid sequence. (optional)
- UniProt accession or gene/protein name resolvable to an accession and canonical sequence. (optional)
- Uploaded FASTA containing one or more sequences. (optional)
- Two or more chains for a manually routed complex workflow, with optional ligands for Boltz-2 or Chai-1. (optional)

## Outputs

- Top-ranked PDB or CIF structure for each completed method.
- Per-residue pLDDT CSV on a normalized 0–100 scale and corresponding confidence plot.
- Canonical confidence-band table and combined band/domain breakdown JSON.
- Run manifest recording the chosen predictor, attempts, statuses, durations, file counts, timeout, and fallback trail.
- Final report plus chat summary of mean pLDDT, available pTM, confidence bands, domain breakdown, and predictor/fallback disclosure.

## Workflow

1. Resolve the input to an auditable submitted sequence and report accession, length, terminal residues, and any trimming.
2. Select a predictor by sequence length and modality while honoring explicit user choices; route complexes and ligand-containing systems to the manual path.
3. Run a bounded submit-and-poll workflow, detect stalls, timeouts, or empty output, and apply the documented non-repeating fallback chain.
4. Extract normalized pLDDT, available pTM, structure paths, plots, confidence-band tables, and the run manifest from the winning output.
5. Compute confidence bands only with the canonical packaged boundary convention.
6. When a UniProt feature table is available, compute overlap-aware domain and sequence-feature confidence; otherwise omit the breakdown with a reason.
7. When fallback occurs, render the predictor disclosure from the manifest and gate the report on the presence of that disclosure.
8. When comparison was requested, align equal-length confidence vectors and report cross-method mean, variation, and high-disagreement residues.
9. Derive every numeric figure label from exported tables, run the value-integrity gate, and complete the final report.

## Decision rules

- For a monomer of 400 amino acids or fewer, default to ESMCFold2; for a longer monomer, default to AlphaFold v2 under the timeout/fallback guard.
- Honor an explicitly requested predictor or highest-accuracy request, but retain bounded polling and fallback handling.
- Reject complexes from the single-chain automated path and route them to AlphaFold-multimer, Boltz, or Chai manually; never use ESMCFold2 for a complex.
- Use Boltz-2 or Chai-1 for systems containing ligands or nucleic acids.
- Never describe pLDDT as experimental accuracy or treat identically valued scores from different predictors as directly interchangeable.
- Use the fixed bands very high at 90 or above, confident at 70–<90, low at 50–<70, and very low below 50.
- Omit the domain breakdown rather than inventing boundaries when no UniProt feature table is available.
- Run cross-method comparison and structural superposition only when the user explicitly requests them.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_d2873a835f39c1f6` — ESMCFold2: A single-chain monomer is 400 amino acids or fewer and no method override was requested.
- `rr_75a8f27f324e2caa` — AlphaFold v2: A single-chain monomer exceeds 400 amino acids or the user explicitly requests AlphaFold.
- `rr_ae2c91e2cbd147a1` — UniProt feature table: An accession is available and a domain-resolved breakdown is required.

### Secondary resources

- `rr_d7c26e6edc13d5c3` — Boltz-2 or Chai-1: A complex, ligand, nucleic acid, or explicit predictor request requires the manual path.
- `rr_b88fffef70a06f6c` — Multiple completed predictors: The user explicitly asks for a cross-method comparison.

### Fallback resources

- `rr_6bbc935091659ecd` — Boltz-2 then ESMCFold2: A longer-monomer primary job stalls, times out, or produces no output.
- `rr_b1a3628a2fa8cecc` — Completed partial method output: The preferred predictor fails but another requested method completed.
- `rr_b2616968cae96957` — Omitted domain breakdown with a stated reason: No accession or UniProt feature table is available.

### Optional resources

- `rr_66e05683356bf73d` — UniProt: Resolve accessions and canonical sequences and provide annotated feature ranges for domain-resolved confidence.
- `rr_b6be29004ee14db4` — AlphaFold v2: MSA-based monomer or documented manual multimer prediction.
- `rr_05b204abaf4052d3` — Boltz-2: Structure prediction with pLDDT, pTM, and optional complex/interface support.
- `rr_4c8f82c9fa8d5d63` — Chai-1: Structure prediction with pLDDT, pTM, and optional complex/interface support.
- `rr_99e986ba87c692d4` — ESMCFold2: Fast MSA-free single-chain prediction.
- `rr_26cf028e225fc26b` — scripts/fold_orchestrate.py: Resolve, select, submit, poll, fall back, extract, and write run provenance for monomers.
- `rr_bca22cad09d0cee5` — scripts/extract_plddt.py: Normalize and export per-residue pLDDT, plots, and canonical bands.
- `rr_e6f2a2cc49c51902` — scripts/confidence_breakdown.py: Compute canonical bands and UniProt-derived overlap-aware domain summaries.
- `rr_6c82165ad0d59ef0` — scripts/run_provenance.py: Render and gate predictor/fallback disclosure.
- `rr_8c87ea0cbd9b0192` — scripts/figure_value_guard.py: Derive and verify numeric figure and infographic labels.
- `rr_03384c8f2b611b89` — Boltz-2: Predictor supporting monomers and documented manual complex or ligand workflows.
- `rr_bc6401e99eff0b42` — Chai-1: Predictor supporting monomers and documented manual complex or ligand workflows.

## Validation / QC

- Record the exact submitted sequence, accession, length, terminal residues, and any coordinate-shifting trimming.
- Require equal confidence-vector lengths before cross-method per-residue comparison.
- Use only the packaged band-breakdown function and fetched UniProt feature ranges.
- Gate reports on complete fallback disclosure whenever the run manifest contains a fallback trail.
- Derive every figure number from exported tables and fail when infographic text disagrees with canonical values.
- Evidence requirement: Report the predictor that produced each confidence value and explain that pLDDT is self-estimated confidence, not experimental correctness.
- Evidence requirement: Use the run manifest to report requested method, actual delivered method, timeout used, attempts, and fallback trail.
- Evidence requirement: Use fetched UniProt annotations verbatim for domain ranges, including overlaps, sequence features, gaps, and uncovered residues.
- Evidence requirement: State when low pLDDT may indicate disorder or flexibility and when cross-method band comparisons are only approximate.

## Failure handling

- An MSA-based job remains in genetic search with zero output until the poll bound.
- A prediction job times out, fails, or returns an empty output directory.
- A complex is submitted to the single-chain automated path.
- No accession or UniProt feature table is available for domain analysis.
- A trimmed sequence is compared directly with unshifted UniProt coordinates.
- GPU job submission is rate-limited or redundant jobs exceed the documented concurrency limit.
- Fallback rule: Use a bounded, non-repeating fallback chain and never wait for a completion callback indefinitely.
- Fallback rule: For longer monomers, fall back from the primary MSA-based attempt to Boltz-2 and then ESMCFold2.
- Fallback rule: Proceed with a completed alternative method when available; otherwise return a structured failure.
- Fallback rule: Omit domain analysis with a reason instead of approximating feature boundaries.
- Fallback rule: Inspect MSA-job logs when confidence is unexpectedly low and disclose any method substitution.

## Limitations

- The workflow does not primarily dock small molecules, run molecular dynamics, compare to experiment by default, or design sequences.
- The automated orchestrator supports only single-chain monomers; complexes require a manual path.
- pLDDT is confidence rather than proof of correctness or biological conformation.
- Confidence bands are AlphaFold-calibrated and transfer only approximately across predictors.
- ESMCFold2 is single-sequence, lacks pTM, is monomer-only, and may be weaker on hard or low-homology targets.
- Domain analysis depends on UniProt annotations and coordinate agreement with the submitted sequence.

## Important domain-specific rules

- Modality- and size-aware predictor routing with explicit user override.
- Canonical confidence normalization, fixed band boundaries, and honest interpretation.
- Overlap-aware, annotation-derived domain confidence that never invents feature boundaries.
- Manifest-based predictor and fallback provenance plus report gating.
- Table-derived figure values with an automated consistency check.

## Portability boundary

- Biomni HPC job submission helpers and platform job orchestration. — migration action: `exclude_or_capability_map`
- Biomni cluster paths, job IDs, GPU concurrency and HTTP 429 handling, and verified platform command flags. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation and GenerateImage report packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
