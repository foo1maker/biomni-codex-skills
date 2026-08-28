# Source workflows for variant-analysis-and-interpretation

Select the narrowest applicable workflow. Each deployment copy is self-contained and mirrors the canonical package under `04_claude_science_skills/_source_workflows/`.

- [`clinical-variant-allelic-series`](clinical-variant-allelic-series/WORKFLOW.md) — Build a position-resolved catalogue for one human gene by reconciling clinically observed ClinVar alleles with curated CIViC evidence, assigning actionability and mechanism annotations, adding UniProt domain context, and producing auditable tables, adaptive figures, and a report.
- [`genetic-variant-annotation`](genetic-variant-annotation/WORKFLOW.md) — Annotate variants in a VCF with functional consequences and available clinical or population evidence, then produce filtered tables, gene summaries, and auditable reports.
- [`variant-calling-from-sequencing`](variant-calling-from-sequencing/WORKFLOW.md) — Produce a quality-controlled germline SNV and indel call set from sequencing reads, alignments, or existing VCFs, with dual-caller concordance and optional truth-set accuracy benchmarking.
