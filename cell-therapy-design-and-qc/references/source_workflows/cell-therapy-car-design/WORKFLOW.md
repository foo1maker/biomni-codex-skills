# Cell Therapy Car Design

Source workflow: `cell-therapy-car-design`  
Parent Claude Science skill: `cell-therapy-design-and-qc`

## Purpose

Combine pooled loss-of-function screen reanalysis and broad-essentiality filtering with construction of validated-scFv, second-generation CAR designs, producing both prioritized editing candidates and annotated receptor constructs.

## When to use

- Reanalyze a pooled CRISPR-knockout proliferation or fitness screen and nominate editing targets.
- Design one or both second-generation CAR variants from an experimentally validated scFv, including codon-optimized sequence and GenBank deliverables.
- Recover a missing guide-to-gene library from sequencing reads before screen analysis.

## Inputs

- Target antigen and an experimentally validated scFv source such as an Addgene deposit, PDB structure, or published clinical sequence. (required)
- Pooled-screen dataset accession or raw sequencing reads, together with the phenotype contrast and sample design. (required)
- Costimulatory-domain choice: 4-1BB, CD28, or both; the documented default is both. (optional)
- Non-targeting controls, when present, for screen normalization and null-model estimation. (optional)
- Preferred hit-validation reference; if unspecified, the documented default is a DepMap broad-essentiality check. (optional)

## Outputs

- Reconstructed sgRNA library, raw and normalized count matrices, count summary, gene and sgRNA summaries, positive and negative hit tables, broad-essentiality cross-check table, and screen QC figures.
- For each CAR: protein FASTA, codon-optimized ORF FASTA, CAR-ORF GenBank, full-cassette GenBank, domain-boundary table, and architecture figure.
- A report containing background, methods, CAR design results, screen QC and hits, cross-check results, conclusions, figures, references, next steps, and limitations.

## Workflow

1. Confirm the screen phenotype, treatment fraction, donor structure, library, and direction of the contrast from literature before analysis.
2. Resolve series, sample, and run linkage; download reads; inspect read structure before deciding how to locate the spacer.
3. If the guide table is unavailable, find the vector anchor, extract and orient the spacer, match it to the reference library, retain adequately observed genes, and recover non-targeting controls.
4. Run MAGeCK count and treatment-versus-control testing with control normalization when non-targeting guides are available, plus a median-normalization sensitivity run.
5. Check mapping, zero-count guides, library evenness, control centering, and expected positive and negative biological controls.
6. Cross-check nominated hits against the selected essentiality reference and flag broadly essential genes without treating that check as proof of primary-cell biology.
7. Obtain the validated scFv, assemble signal peptide, scFv, hinge, transmembrane, costimulatory, and CD3z domains, and build each requested construct.
8. Codon-optimize the ORF for human expression, verify translation identity, construct the expression cassette, and emit sequence, annotation, domain, and architecture outputs.

## Decision rules

- Never reconstruct an scFv; use an experimentally validated source.
- If the costimulatory domain is unspecified, build both 4-1BB and CD28 variants.
- For a CFSE proliferation screen, use dividing cells as treatment and non-dividing cells as control; confirm this direction from the paper because reversing it inverts every hit.
- Use non-targeting controls for normalization when present and always include a median-normalization sensitivity analysis.
- Treat positive-selection hits as knockout-enhanced proliferation candidates; treat negative-selection hits as essential boundaries that should not be knocked out.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_5ac26494d5be53b7` — Published screen design, guide library, and experimentally validated CAR-part sources.: Establishing phenotype direction, guide provenance, and scFv provenance.
- `rr_32827ecb9ea1a718` — DepMap broad-essentiality data.: The user does not request a different hit-validation reference.

### Secondary resources

- `rr_c49cec36e8c9e57e` — A lineage-restricted DepMap subset, published primary-T-cell screen, or pathway enrichment.: The user selects an alternative to the default broad-essentiality reference.

### Fallback resources

- `rr_35008451773531e5` — Read-driven guide-library reconstruction using the vector anchor and a reference library.: The original guide-to-gene table is missing or unavailable.

### Optional resources

- `rr_ec2c9dff0cb1a776` — GEO: Series and sample metadata with SRA linkage for the pooled screen.
- `rr_d2ee5ce6f038e497` — DepMap: Broad-essentiality cross-check for screen hits.
- `rr_9194ecc904e4151a` — Addgene: Experimentally deposited CAR plasmids, parts, and sequence files.
- `rr_9c01c8128e7fdba4` — RCSB PDB: Validated scFv structure source; the worked example uses FMC63 structure 7URV chain D.
- `rr_bf640371fd3c9050` — mageck: Guide counting and treatment-versus-control screen statistics.
- `rr_938b8adc8d3ff182` — GEOparse: Resolve GEO metadata and SRA linkage.
- `rr_0fde0898fc1f5a5a` — biopython: Write annotated GenBank deliverables.
- `rr_2d5916535d41e5d9` — pandas and NumPy: Tabular and numerical processing.
- `rr_d639546fd15be664` — sra-tools: External fallback for FASTQ retrieval because the source states there is no native equivalent.

## Validation / QC

- Inspect read structure rather than assuming that the spacer starts at position zero.
- Evaluate mapping rate, zero-count guides, Gini index, and non-targeting-control centering; the documented typical values are about 70–80% mapping, few zero-count guides, Gini below about 0.1, and median control LFC near zero.
- Confirm expected brakes enrich and expected TCR-essential genes deplete as an internal biological check.
- After codon optimization, verify that translation is identical to the intended CAR protein.
- Evidence requirement: Ground the screen design, phenotype direction, donor structure, and top-hit biology in retrieved literature rather than memory.
- Evidence requirement: Use verified computed values only in the summary infographic.
- Evidence requirement: Describe a read-driven, reference-subsetted guide library as a transparent reconstruction rather than the original supplementary table.

## Failure handling

- An incorrect treatment-control direction silently inverts all screen hits.
- Assuming a fixed read position for the spacer can prevent correct guide recovery.
- Treating a broadly essential hit as a strong editing target can confuse general fitness effects with context-specific regulation.
- Fallback rule: When a guide table is missing, reconstruct it transparently from reads using the vector anchor and a reference library.
- Fallback rule: When non-targeting controls are unavailable, retain the documented median-normalization sensitivity analysis rather than inventing controls.

## Limitations

- A targeted pilot sub-library is discovery-grade rather than genome-wide.
- Few donors make stringent genome-wide FDR underpowered; non-significant hits should be reported only as ranked nominations supported by effect size and cross-donor concordance.
- DepMap cancer cell lines flag broad essentiality but do not confirm primary-T-cell-specific biology.
- The documented environment has no native guide-design tool, so guides must come from a published library, Addgene library, or validated published set.
- Computationally assembled CAR constructs require experimental validation for expression, cytotoxicity, and in-vivo potency.

## Important domain-specific rules

- Confirm phenotype direction and experimental design from source literature before setting a contrast.
- Use control-normalized primary analysis plus median-normalized sensitivity analysis.
- Recover a missing guide library from observed read structure and disclose it as a reconstruction.
- Filter screen nominations through a broad-essentiality check while preserving its biological-scope caveat.
- Require validated part provenance and verify amino-acid identity after codon optimization.

## Portability boundary

- Biomni-native LiteratureSearch, DepMap access, biomni.tool.integrations helper IDs, and biomni.tool.molecular_biology helper IDs. — migration action: `exclude_or_capability_map`
- Package-local reference files, helper scripts, and the prescribed agent-shell execution sequence. — migration action: `exclude_or_capability_map`
- The pdf-report-generation skill, Phylo branding, Read media checks, and the fixed /mnt/results delivery path. — migration action: `exclude_or_capability_map`
- Native environment inspection commands and package/CLI availability assumptions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
