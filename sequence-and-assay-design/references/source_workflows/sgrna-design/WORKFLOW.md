# Sgrna Design

Source workflow: `sgrna-design`  
Parent Claude Science skill: `sequence-and-assay-design`

## Purpose

Find or design guide RNAs by prioritizing experimentally validated sequences, then precomputed designs, and using de-novo design only as a last resort.

## When to use

- Select guide RNAs for knockout, activation, inhibition, arrayed screening, or pooled screening.
- Select guides for SpCas9, SaCas9, AsCas12a, or enAsCas12a.

## Inputs

- Gene symbol. (required)
- Organism, with human as the default. (optional)
- Application, with knockout as the default. (optional)
- Cas enzyme, with SpCas9 as the default. (optional)

## Outputs

- Unified table of three to four recommended guides with sequence, source, score or rank, exon or position, PAM, citation or dataset, and notes.
- Summary stating which evidence tier was used, why it was selected, the chosen guides, and scientific caveats.

## Workflow

1. Search both the validated Addgene sequence collection and peer-reviewed or web-accessible validation evidence before considering predicted designs.
2. If no usable validated guides are found, resolve the organism, enzyme, and application-specific CRISPick dataset and select ranked guides across distinct exons.
3. If the gene is not covered by the first two tiers, apply enzyme-specific length, PAM, GC, sequence-complexity, and target-location rules for de-novo candidates.
4. Export the selected guides in a unified table with the chosen tier and rationale.

## Decision rules

- Complete both validated-sequence search methods before descending to CRISPick, and use de-novo design only when the first two tiers yield nothing usable.
- Keep AsCas12a and enAsCas12a datasets separate because guides for one enzyme may not work with the other.
- Prefer lower Combined Rank by default and distribute selected predicted guides across distinct exons for redundancy.
- Match genome coordinates to the correct reference build.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_860e118a4379f28e` — Validated Addgene sequences and published experimental evidence: Always search both sources first and use usable validated guides when found.

### Secondary resources

- `rr_4d3543687abf39b7` — CRISPick precomputed designs: Use when no validated guide exists, when genome-wide coverage is needed, or when ranked alternatives are requested.

### Fallback resources

- `rr_7a9e9a1e51babb56` — De-novo sequence-rule design with Cas-OFFinder or CRISPOR follow-up: Use for genes or organisms not covered by validated or precomputed resources.

### Optional resources

- `rr_9194ecc904e4151a` — Addgene: Validated guide-sequence metadata and original-publication links.
- `rr_8070f7289fd51d3a` — CRISPick: Precomputed guide designs and ranking data.
- `rr_7532fcd18b5551fa` — Cas-OFFinder: Genome-wide off-target assessment for de-novo candidates.
- `rr_0c2fa5ccefc5cae0` — CRISPOR: Genome-wide off-target assessment for de-novo candidates.

## Validation / QC

- For each literature-derived validated guide, record sequence, PMID or DOI, cell line, and validation details.
- For de-novo SpCas9 or SaCas9 guides use 20 bases; for Cas12a use 23-25 bases, apply the correct PAM, target 40-60 percent GC, and avoid TTTT and homopolymers longer than four bases.
- Test three to four guides per gene experimentally and validate edits; prediction scores do not replace empirical validation.
- Evidence requirement: Cite the original publication for each validated Addgene guide and preserve the guide-specific validation context.
- Evidence requirement: Label Addgene plasmid acquisition and CRISPick commercial use as requiring commercial review under the applicable terms.

## Failure handling

- The bundled validated-sequence snapshot and dataset-link snapshot may not cover the requested gene or organism.
- Using the wrong genome build or Cas12a variant can invalidate coordinate or enzyme compatibility.
- Simple rule checking does not provide genome-wide off-target assessment.
- Fallback rule: If the validated tier yields no usable guides, use the matched CRISPick dataset.
- Fallback rule: If the gene is absent from CRISPick, apply de-novo design rules and qualify the result until genome-wide off-target assessment is performed.

## Limitations

- Bundled sequence and dataset files are fixed snapshots and are incomplete.
- The workflow does not perform genome-wide off-target alignment beyond precomputed CRISPick ranks.

## Important domain-specific rules

- Validated-first three-tier selection strategy with explicit descent criteria.
- Enzyme-specific PAM, length, GC, target-location, exon-redundancy, citation, and empirical-validation rules.

## Portability boundary

- Biomni LiteratureSearch and WebSearch orchestration plus internal bundled-resource and script paths. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation handoff, Phylo report branding, and platform result-directory conventions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
