# Antibody Developability Humanization

Source workflow: `antibody-developability-humanization`  
Parent Claude Science skill: `antibody-and-protein-engineering`

## Purpose

Assess antibody sequence liabilities, aggregation propensity, MHC-II immunogenicity, framework humanness, and—when the input is non-human—humanization candidates.

## When to use

- Assess developability and sequence liabilities of an antibody variable region.
- Humanize a non-human paired variable fragment by comparing human framework acceptors.
- Evaluate MHC-II immunogenicity and framework humanness.

## Inputs

- Paired VH and VL amino-acid sequences, a two-record FASTA, or a single variable domain. (required)
- A validated reference VH/VL pair for an optional blind post-hoc benchmark. (optional)
- Numbering scheme, back-mutation level, and HLA-DR panel overrides. (optional)

## Outputs

- PDF report for the assessed or humanized constructs.
- Machine-readable master frontier table and serialized report payload.
- Per-construct liability, biophysical, aggregation, humanness, and immunogenicity tables.

## Workflow

1. Classify the input as paired non-human, paired human, or single-domain and choose the corresponding assessment branch.
2. Ingest and number the supplied sequences, recording warnings and any need for confirmation.
3. For non-human paired inputs, create consensus- and nearest-germline grafts with framework-only back-mutations.
4. Reassess every construct for sequence liabilities, named aggregation propensity, humanness, and MHC-II immunogenicity when available.
5. Optionally score blind convergence against a supplied reference after design; do not use the reference to choose the design.
6. Generate figures, serialize a report payload, build the report, and validate the PDF.

## Decision rules

- Humanize non-human paired variable fragments, assess already-human pairs without proposing humanization, and assess single domains as supplied.
- Never fabricate immunogenicity values; mark the axis unavailable when no predictor can run.
- Use the named AGGRESCAN a3v scale for aggregation and keep GRAVY, pI, and charge as context rather than substitutes.
- Graft CDRs verbatim and restrict back-mutations to framework Vernier, interface, or canonical positions.
- Compare constructs only when they use the same HLA-DR panel and numbering scheme.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_963cffc96da6ab99` — Local NetMHCIIpan: A licensed local installation is available and sequences must remain on-machine.

### Secondary resources

- `rr_62043b184157ba93` — Hosted IEDB prediction API: No local NetMHCIIpan is installed and external transmission of the sequences is acceptable.

### Fallback resources

- `rr_0ec5f0a570db75c2` — Unavailable immunogenicity axis: Neither a local predictor nor hosted API access is available; continue without invented epitope numbers.

### Optional resources

- `rr_a126ea8bfce4d752` — Immune Epitope Database (IEDB): MHC-II epitope data and binding prediction service.
- `rr_9d9b8479911a4202` — ANARCI: Antibody-chain numbering backend.
- `rr_9aa371db6fca1a6a` — abnumber: Antibody numbering interface.
- `rr_4396346d6297ce81` — HMMER: ANARCI backend dependency.
- `rr_2c3b62ba0b4eb716` — pyteomics: Biophysical context calculations such as pI and charge.
- `rr_7ca7452d33a1bf1c` — AggreScan3D: Optional structure-aware aggregation assessment when a folded variable fragment exists.
- `rr_f0e8496a76f29ebe` — AGGRESCAN a3v predictor: Sequence-based aggregation profile and aggregation-prone-region calling.
- `rr_8f61fe6bf3e52682` — NetMHCIIpan: Preferred local MHC-II binding predictor when installed.

## Validation / QC

- Keep the optional reference benchmark blind and use it only for post-hoc scoring.
- Do not retune the a3v scale, hotspot threshold, or minimum aggregation-prone-region length without documenting the deviation.
- Validate the generated PDF before delivery.
- Make reports branch-aware and show no immunogenicity numbers when that axis is unavailable.
- Evidence requirement: Report the sequence-based scope of aggregation results and do not present them as three-dimensional patch measurements.
- Evidence requirement: Acknowledge IEDB when its data or MHC-II prediction service is used.
- Evidence requirement: Separate reference-absent analysis from optional reference-present blind scoring.

## Failure handling

- The antibody-numbering stack is absent.
- No local or hosted immunogenicity predictor is available.
- Hosted prediction would transmit proprietary VH/VL sequences externally.
- Sequence-only aggregation assessment misses buried residues and three-dimensional or colloidal effects.
- Fallback rule: Use the hosted IEDB API only when local NetMHCIIpan is unavailable; otherwise prefer local prediction.
- Fallback rule: If no predictor is available, report immunogenicity as unavailable and rank using developability and humanness only.
- Fallback rule: Upgrade to a structure-based aggregation predictor when a suitable folded variable-fragment structure is available.

## Limitations

- The default aggregation predictor cannot detect three-dimensional aggregation patches, burial, or colloidal effects.
- Hosted IEDB fallback transmits the supplied antibody sequences to an external service.
- Reference-present benchmarking scores convergence but does not validate experimental developability or binding.

## Important domain-specific rules

- Branch antibody handling by non-human paired, already-human paired, and single-domain input types.
- Keep CDR grafting and framework back-mutation rules separate and explicit.
- Degrade unavailable prediction axes honestly instead of fabricating scores.
- Report predictor scope and route to structure-aware confirmation when structural evidence is available.

## Portability boundary

- Bundled scripts/run_pipeline.py orchestration and its skill-local command-line interface. — migration action: `exclude_or_capability_map`
- Skill-local Python module names and packaged example aliases. — migration action: `exclude_or_capability_map`
- Phylo-branded report and skill-local output-path conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
