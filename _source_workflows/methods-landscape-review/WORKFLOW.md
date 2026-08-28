# Methods Landscape Review

Source workflow: `methods-landscape-review`  
Parent Claude Science skill: `literature-and-evidence-synthesis`

## Purpose

Turn a method-choice or evidence-landscape question into a citation-verified, regime-conditional comparison or topic synthesis with structured screening, source-bound figures, decision guidance, and a validated report.

## When to use

- Compare named methods or tools for a task and recommend among them.
- Synthesize the evidence landscape for a methodological topic.
- Catalog benchmark designs, performance claims, trade-offs, and disagreements.

## Inputs

- Task or question in free text. (required)
- Candidate methods or tools for comparison mode. (optional)
- Scope boundaries and optional retrieval filters. (optional)

## Outputs

- Consolidated literature corpus and screening log with include/exclude reasons.
- Comparison matrix, benchmark catalog, and performance claims for comparison mode; evidence table for topic mode.
- Blocking citation-verification result.
- Source-bound figures and figure manifest.
- Verified narrative synthesis and final PDF report.

## Workflow

1. Confirm comparison versus topic mode, candidate methods, scope boundaries, and evidence depth.
2. Plan separate foundational, benchmark/comparison, and recent-advance queries without excluding classic method papers.
3. Retrieve peer-reviewed records, consolidate them, and deduplicate the corpus.
4. Record inclusion/exclusion reasons and extract method, benchmark, evidence-thickness, or topic-theme fields.
5. Read selected pivotal full text only when exact quantitative values are required.
6. Verify every quantitative value and citation field against retrieved records; drop or flag unverifiable material.
7. Generate only source-bound data figures, label qualitative scorecards as qualitative, and verify every figure.
8. Author regime-conditional synthesis from verified evidence and validate the final report.

## Decision rules

- Use comparison mode when competing methods or a method choice is explicit; use topic mode for a non-comparative survey.
- Do not execute the compared methods on user data within this review workflow.
- Do not over-restrict the year range because foundational method papers may be much older than recent benchmarks.
- Rank evidence thickness as head-to-head evidence above multi-benchmark evidence above single-benchmark evidence.
- Do not proceed to reporting until citation verification is clean or every partial flag has been consciously resolved.
- Never declare an unconditional winner; make recommendations conditional on data regime, sample size, quality, and design.
- Present genuine disagreements as disagreements and surface benchmark assumptions and biases.
- Exclude non-commercial resources from recommendations and disclose attribution/share-alike obligations for share-alike resources.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- None declared in the normalized source record.

### Secondary resources

- None declared in the normalized source record.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_178baeb270b10c13` — pandas: Structured corpus, screening, and evidence-table processing.
- `rr_ae5d2965f1c73447` — reportlab: Report generation support.
- `rr_c625bc4be2dc2ab7` — pypdf: PDF structural validation.
- `rr_f3a4936deaf955aa` — ggplot2: Data-driven figure generation.
- `rr_9c40795c5beef2c4` — ggprism: Figure styling support.

## Validation / QC

- Maintain a PRISMA-style screening log with include/exclude reasons.
- Verify quantitative values and title, author, year, journal, DOI, and accession fields against retrieved records.
- Copy titles verbatim and drop or explicitly flag unverifiable claims.
- Trace every plotted value to a paper and media-check every figure.
- Validate page count, extractable text, figure presence, and visual rendering.
- Evidence requirement: Record every exact benchmark number with its source DOI.
- Evidence requirement: Every quantitative statement and citation identity must match a retrieved record.
- Evidence requirement: Label ordinal scorecards as qualitative syntheses rather than re-measured performance metrics.
- Evidence requirement: Attach regime conditions and benchmark caveats to every method recommendation.

## Failure handling

- Recency-biased retrieval buries foundational method papers.
- Citation fields are hallucinated or paraphrased after context compaction.
- Figures contain empty panels, noisy auto-extraction, or values without source provenance.
- Overly strict year or study-type filters starve the corpus.
- Fallback rule: Use targeted full-text retrieval only for already identified pivotal papers, not as a primary discovery channel.
- Fallback rule: Drop a claim rather than guessing when citation or quantitative verification fails.
- Fallback rule: Prefer curated source-bound values over noisy automatic abstract token extraction.

## Limitations

- The workflow synthesizes published evidence but does not run the compared methods on data.
- Benchmark conclusions can depend on self-referential gold standards, simulation assumptions, permutation nulls, and preprocessing choices.
- Qualitative ordinal scores are not newly measured performance metrics.

## Important domain-specific rules

- Anti-recency-bias query planning that separates foundational, benchmark, and recent literature.
- Mode-aware structured extraction for comparisons or topic landscapes.
- Blocking verification of quantitative claims and bibliographic identity.
- Regime-conditional decision guidance with explicit disagreement and benchmark caveats.
- Source-traceable visual synthesis with qualitative-scorecard labelling.

## Portability boundary

- Biomni LiteratureSearch tool and execution-trace file routing. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage and Read media-output-check calls. — migration action: `exclude_or_capability_map`
- Biomni resource catalog, HPC discovery, Skill lookup, and sibling-skill routing. — migration action: `exclude_or_capability_map`
- Packaged script names, exact commands, and internal results/run paths. — migration action: `exclude_or_capability_map`
- Phylo-branded platform PDF skill and report layout engine. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
