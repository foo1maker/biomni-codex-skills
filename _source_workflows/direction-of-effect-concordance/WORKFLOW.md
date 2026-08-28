# Direction Of Effect Concordance

Source workflow: `direction-of-effect-concordance`  
Parent Claude Science skill: `target-evidence-and-tractability`

## Purpose

Determine whether target activation or inhibition is therapeutically favored for a specified indication by reconciling directional evidence across independent axes.

## When to use

- Call the therapeutic direction of one or more targets in a named disease context.
- Assess whether human genetics, functional perturbation, drug mechanism, and mouse knockout evidence agree.
- Report genuine cross-axis conflicts with confidence tiers and mechanistic flags.

## Inputs

- One or more human gene symbols or Ensembl, UniProt, or HGNC identifiers. (required)
- An indication or disease context for every target, as free text or EFO or MONDO identifier. (required)
- Axis set, retrieval filters, and report depth. (optional)

## Outputs

- Target-by-axis evidence matrix containing raw readout, therapeutic-direction vote, source, and citations.
- Per-target consensus call, concordance, confidence tier, and discordance flags.
- Raw structured-source pulls with release or version provenance.
- Citation-verification status and verified references.
- Data-driven evidence-matrix and consensus figures.

## Workflow

1. Confirm target identifiers, indication context, evidence axes, and report depth.
2. Resolve target and indication identifiers, flagging ambiguous or deprecated symbols rather than guessing.
3. Retrieve drug-mechanism, mouse, safety, functional, CRISPR, and human-genetics evidence from the documented structured sources.
4. Retrieve directional literature for each target and axis without excluding foundational older studies.
5. Translate each axis readout with the fixed direction rule, then derive the consensus and confidence tier.
6. Review every automatically assigned vote, especially allele-specific gain-of-function cases.
7. Verify every quantitative value and citation field before the synthesis can proceed.
8. Present the evidence matrix, consensus, conflicts, caveats, and verified references.

## Decision rules

- If loss or reduction of target function is beneficial for the indication, vote INHIBIT; if gain or restoration is beneficial, vote ACTIVATE.
- A silent loss-of-function phenotype is not evidence for ACTIVATE.
- Interpret negative DepMap gene-effect scores as essentiality and add a toxicity caveat for broad pan-essentiality.
- Treat on-target safety signals as safety flags, not therapeutic-direction reversals.
- Exclude not-informative axes from the consensus denominator.
- Report CONTESTED and both sides when an informative axis genuinely opposes the majority.
- Use evidence-strength ordering only as a tie-breaker and tier rationale, not as an opaque numerical weighting model.
- Restrict structured-source direction calls to human targets.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_5ebd92bc886805b4` — Open Targets: Drug mechanism, genetics association, mouse phenotype, and safety evidence are required.
- `rr_3417e358fe964443` — DepMap CRISPR gene-effect and dependency matrices: Functional dependency evidence is required.
- `rr_bb671f63cbf20f98` — GeneBass, GWAS Catalog, and gnomAD: Human loss-of-function, burden, association, and constraint evidence are required.

### Secondary resources

- `rr_f79b5c42112e2539` — Primary directional literature: Structured evidence needs mechanistic interpretation or exact directional support.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_d2ee5ce6f038e497` — DepMap: CRISPR gene-effect and dependency evidence.
- `rr_68cf2c6ee6caf3de` — GeneBass: Gene-burden direction evidence.
- `rr_81374f2a82ece6c2` — GWAS Catalog: Human association and direction-of-effect evidence.
- `rr_35ab27166fd8a660` — gnomAD: Loss-of-function intolerance and constraint context.
- `rr_178baeb270b10c13` — pandas: Evidence-matrix and summary-table processing.
- `rr_78609fcc8c7914c6` — numpy: Light numerical processing.
- `rr_f48c55f08412b6c6` — requests: Structured-source retrieval.
- `rr_f3a4936deaf955aa` — ggplot2: Data-driven evidence figures.
- `rr_cb67117e40f55076` — majority vote across informative axes: Produces a target-level therapeutic-direction consensus while excluding non-informative axes.
- `rr_718de81ca41aa545` — confidence tier: Classifies concordant, caveated, thin, and contested evidence configurations.

## Validation / QC

- Flag ambiguous or deprecated identifiers instead of resolving them by guess.
- Read only target rows or columns from large DepMap matrices.
- Human-review every automatically assigned direction vote before synthesis.
- Block reporting until quantitative values, citation fields, and citation indices are verified.
- Evidence requirement: Every axis must retain the raw readout, mapped vote, source, and citations.
- Evidence requirement: Every opposing or interpretation-sensitive axis must be surfaced with a mechanistic explanation.
- Evidence requirement: Citation titles, authors, year, journal, DOI, NCT, PMID, and accession values must match retrieved records.
- Evidence requirement: Unverified claims must be removed or explicitly flagged rather than guessed.

## Failure handling

- A silent germline knockout is misread as evidence for activation.
- DepMap essentiality is mis-signed because negative scores are interpreted incorrectly.
- Recency-biased retrieval misses foundational loss-of-function or knockout papers.
- An automatic vote is accepted without allele- and context-specific review.
- A false consensus is manufactured despite genuine cross-axis disagreement.
- Fallback rule: Drop or explicitly flag a claim that cannot pass citation verification.
- Fallback rule: Report CONTESTED with both sides when conflicting axes cannot be mechanistically reconciled.
- Fallback rule: Use fewer axes only when the omitted axes are explicitly documented and the remaining evidence is sufficient.

## Limitations

- Direction is indication-specific and cannot be transferred automatically across diseases.
- The workflow synthesizes evidence and does not execute CRISPR screens, GWAS, or animal studies.
- Structured-source direction calls are limited to human targets.
- Fewer than two informative axes are too thin for a confident direction call.

## Important domain-specific rules

- Fixed raw-readout-to-therapeutic-direction mapping for independent evidence axes.
- Informative-axis majority consensus with transparent confidence tiers.
- Strict discordance and context-specific interpretation flags.
- Blocking quantitative and bibliographic verification gate.

## Portability boundary

- Biomni LiteratureSearch, WebSearch, and WebFetch discovery and full-text orchestration. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage and Read media-output-check tools. — migration action: `exclude_or_capability_map`
- Biomni resource catalog, hpc_search_tools, Skill calls, sibling-skill inventory, and platform follow-up mapping. — migration action: `exclude_or_capability_map`
- Packaged scripts, execution-trace files, internal /mnt/results paths, and pdf-report-generation orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
