# Pcr Primer Design

Source workflow: `pcr-primer-design`  
Parent Claude Science skill: `sequence-and-assay-design`

## Purpose

Design PCR, qPCR, TaqMan, multiplex, sequencing, or allele-specific primers and probes with application-specific constraints, explicit specificity status, structural checks, and MIQE-oriented documentation.

## When to use

- Design MIQE-oriented qPCR primer pairs.
- Design standard PCR primers for amplification, cloning, or genotyping.
- Design TaqMan probes for probe-based qPCR assays.
- Design compatible primers for multiplex PCR, sequencing, or SNP genotyping.
- Validate primer thermodynamics, dimers, secondary structure, and specificity at a stated validation level.

## Inputs

- Target DNA sequence supplied as FASTA, GenBank or RefSeq accession, raw sequence, or gene name plus organism. (required)
- PCR application: qPCR, standard PCR, TaqMan, multiplex, sequencing, or SNP genotyping. (required)
- Optional excluded regions such as variants, repeats, or splice sites. (optional)
- Optional custom melting-temperature, GC-content, and amplicon-size ranges. (optional)
- Optional organism or genome resource for specificity checking. (optional)
- Requested Basic, Standard, Complete, or MIQE-compliant validation level. (optional)
- Requested export formats, visualizations, and number of primer pairs. (optional)

## Outputs

- Primer or probe sequences with melting temperature, GC content, length, position, quality score, and QC flags.
- Validation results for dimers, secondary structures, and specificity.
- An explicit specificity_status describing exactly which specificity check ran and whether it passed.
- CSV, Excel, JSON, IDT-order, and qPCR MIQE-checklist exports as requested.
- Optional binding-site, melting-temperature, and secondary-structure visualizations.
- A primer-design report and optional PDF assembled from generated artifacts.

## Workflow

1. Load the target from FASTA, a resolved accession, or pasted sequence, and ensure it satisfies length and alphabet requirements.
2. Select the application-specific design function and defaults, then generate ranked primer pairs or TaqMan assays.
3. Run dimer and secondary-structure checks for candidate primers.
4. At the requested validation level, run transcript-only in-silico PCR or a genome-wide local BLAST or Primer-BLAST check.
5. Assign and propagate the exact specificity_status, including a qualified warning for high-risk targets that lack a genome-wide check.
6. Generate the requested report and exports, including the MIQE checklist for qPCR and the specificity status in every applicable artifact.
7. Experimentally verify amplification specificity, amplicon size, efficiency, and assay conditions before relying on the design.

## Decision rules

- Use qPCR defaults of 70–140 bp amplicons and primer melting temperatures matched within 2°C; use 100–1000 bp amplicons for standard PCR.
- For qPCR, target an exon–exon junction or ensure the intervening intron exceeds 1 kb when applicable.
- Basic validation covers thermodynamics and dimers; Standard adds on-target transcript in-silico PCR; Complete adds a genome-wide check; MIQE-compliant adds full qPCR documentation.
- Never interpret a transcript-only in-silico PCR result as genome-wide specificity.
- If a pseudogene- or paralog-prone target only has transcript-level validation, set flagged_high_risk_unverified rather than a pass.
- If blastn or its database is unavailable, return not_run rather than a misleading specificity pass.
- Always perform dimer and secondary-structure checks.
- Verify Excel output is larger than 1000 bytes and fall back to CSV if the workbook write fails or is suspiciously small.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_404f960c618ed04b` — Packaged primer-design, validation, report, and export scripts.: Use for all standard application-specific designs and validation.
- `rr_282e536bac6ab7f8` — User-supplied target sequence or resolvable accession.: Use as the authoritative template for primer design.

### Secondary resources

- `rr_d7f43d2b8c2547b7` — Local blastn with a genomic or transcriptomic database.: Use for genome-wide specificity checking when local BLAST resources are available.
- `rr_af3eba2e5730373b` — NCBI Primer-BLAST.: Use for genome-wide specificity assessment when local BLAST is unavailable and online submission is performed.
- `rr_739a024d729a028a` — Packaged best-practice, parameter, troubleshooting, MIQE, and code-example references.: Use when adapting constraints or interpreting validation.

### Fallback resources

- `rr_9328ecf2e1b3a66b` — Transcript-only in-silico PCR with qualified specificity_status.: Use when genome-wide resources are unavailable; do not claim genome-wide specificity.
- `rr_6adaa954ebfa1694` — CSV files mirroring workbook sheets.: Use automatically when an Excel workbook write fails validation.
- `rr_7c8003f71ae167a2` — reportlab PDF generation.: Use only when the dedicated PDF-report capability is unavailable, with explicit fallback disclosure.

### Optional resources

- `rr_a61157fe25e71dcd` — NCBI GenBank and RefSeq: Resolve accession or gene-plus-organism inputs to target sequences.
- `rr_6c65403cb4a78a92` — Local BLAST genomic or transcriptomic database: Assess candidate primers for genome-wide off-target matches.
- `rr_f1195c3038f66684` — NCBI Primer-BLAST: Provide online primer specificity evaluation.
- `rr_7015418ef29fadf4` — primer3-py: Generate primer pairs under application-specific constraints.
- `rr_0fde0898fc1f5a5a` — biopython: Read FASTA and sequence records and support sequence handling.
- `rr_665edbde6fc3cd1c` — matplotlib: Create primer-design visualizations.
- `rr_178baeb270b10c13` — pandas: Construct and export tabular results.
- `rr_f48c55f08412b6c6` — requests: Support sequence retrieval and online requests.
- `rr_5a968aec7cfbd11b` — openpyxl: Write multi-sheet Excel workbooks.
- `rr_965f80fb9c57fc26` — blastn: Perform local genome-wide specificity checking when a database is available.
- `rr_ae5d2965f1c73447` — reportlab: Provide a direct PDF fallback if the dedicated report capability is unavailable.
- `rr_b0e5fabec16b40c3` — design_qpcr_primers, design_pcr_primers, and design_taqman_assay: Generate application-specific candidate designs.
- `rr_eb05cbc9afbee09d` — analyze_dimers and analyze_secondary_structures: Detect primer dimers and secondary structures.
- `rr_bff32c47bc794148` — in_silico_pcr_report and check_specificity_local_blast: Perform transcript-only or local genome-wide specificity evaluation.
- `rr_1e341e2eaf53c1b1` — generate_primer_report and export_primers: Generate reports and validated export artifacts.
- `rr_abbf99b8d8602a79` — pdf-report-generation: Create a requested final PDF from primer-design artifacts.

## Validation / QC

- Require at least 150 bp for qPCR or 300 bp for standard PCR and avoid ambiguous bases in primer-binding regions.
- Check dimers and secondary structures for every reported design.
- Record exactly whether validation was transcript-only, local BLAST, Primer-BLAST, failed, or not run.
- Flag pseudogene- and paralog-prone targets when genome-wide specificity has not been established.
- Carry specificity_status and pseudogene-risk information into CSV, JSON, and MIQE outputs.
- Validate non-trivial Excel file size after writing and use the declared CSV fallback on failure.
- Evidence requirement: Report primer sequences together with melting temperature, GC content, length, position, validation results, quality scores, and QC flags.
- Evidence requirement: Qualify every specificity claim with the exact check performed; transcript-only matching is not evidence of genome-wide specificity.
- Evidence requirement: Preserve warnings and an indeterminate specificity conclusion for high-risk targets without genome-wide testing.
- Evidence requirement: Treat computational design as provisional until specificity, amplicon size, efficiency, and assay conditions are verified experimentally.

## Failure handling

- No primers are found because the target is too short or constraints are too strict.
- Candidate primers form high-risk dimers or secondary structures.
- Candidate primers produce multiple off-target amplicons.
- Forward and reverse primers have poorly matched melting temperatures.
- A qPCR assay has poor predicted or measured efficiency because of dimers, structure, or an oversized amplicon.
- NCBI requests are rate limited.
- Packaged modules fail to import because the scripts package is not initialized.
- An Excel export fails or produces a zero-byte or suspiciously small workbook.
- Fallback rule: If no primers are found, relax the melting-temperature range, widen the GC range, or provide a longer sequence.
- Fallback rule: If dimers are excessive, redesign with higher melting temperature and less complementarity.
- Fallback rule: If off-targets are found, move to a unique region, lengthen the primers, and rerun genome-wide specificity checking.
- Fallback rule: On NCBI rate limiting, wait between requests, use an API key where appropriate, or perform transcript-only screening first.
- Fallback rule: When genome-wide specificity cannot be tested, report in_silico_on_target_only, flagged_high_risk_unverified, or not_run instead of a pass.
- Fallback rule: Fall back from an invalid Excel workbook to CSV files and return the path actually written.
- Fallback rule: If the dedicated PDF report capability is unavailable, use reportlab and disclose the fallback.

## Limitations

- The workflow is not intended for in-situ probes, sequencing-library adapters, or CRISPR guide design.
- Short or ambiguous target sequences can prevent valid primer design.
- Transcript-only in-silico PCR cannot detect pseudogenes, paralogs, or other genomic off-targets.
- The packaged Primer-BLAST helper constructs a submission URL but does not itself submit an online job.
- Mismatch tolerance models substitutions at primer sites but not insertions or deletions.
- plotnine-prism has unresolved licensing metadata and must not be reintroduced without license review.

## Important domain-specific rules

- Application-driven selection of primer constraints and amplicon ranges.
- Candidate generation through Primer3-backed application-specific functions.
- Mandatory dimer and secondary-structure validation.
- Explicit, evidence-calibrated specificity status that distinguishes transcript-only and genome-wide checks.
- Pseudogene and paralog risk escalation when genome-wide evidence is absent.
- MIQE-oriented result schema and multi-format export with validation provenance.
- Post-write workbook validation with a transparent CSV fallback.
- Experimental confirmation checklist for computational primer designs.

## Portability boundary

- Delegation of requested PDF creation to the Biomni pdf-report-generation skill. — migration action: `exclude_or_capability_map`
- Biomni package-relative path convention and mandatory use of the packaged commands. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
