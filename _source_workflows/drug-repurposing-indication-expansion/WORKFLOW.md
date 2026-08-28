# Drug Repurposing Indication Expansion

Source workflow: `drug-repurposing-indication-expansion`  
Parent Claude Science skill: `drug-repurposing-from-signatures`

## Purpose

Rank existing drugs for a new disease indication by identifying perturbation signatures that reverse a disease transcriptional signature.

## When to use

- Run a connectivity-map or LINCS disease-signature reversal screen.
- Convert a disease name, up/down gene list, or differential-expression table into a ranked repurposing shortlist.
- Annotate and validate approved or clinical reversal candidates.

## Inputs

- Disease name matched to a supported disease signature. (optional)
- Explicit human up- and down-regulated gene-symbol lists. (optional)
- Differential-expression table with fold-change and adjusted-p-value columns. (optional)
- Optional perturbation-library override and expected reverser or mimic controls. (optional)

## Outputs

- A canonically ranked table of all scored perturbations with reversal statistics, FDR, enrichment, and annotations.
- Approved repurposing candidates in the same canonical order.
- Control validation and literature-evidence tables.
- Data-driven score, candidate, signature, and mechanism-validation figures.
- Optional target-nomination, ADMET, and clinical-trial novelty tables.

## Workflow

1. Resolve a disease name, explicit gene lists, or differential-expression table into human up- and down-signature sets.
2. Map murine perturbation genes to human orthologs, remove ambiguous genes and weak signatures, and establish one shared background.
3. Score reversal and mimic overlap with size correction, a permutation null, and FDR control.
4. Compute an independent enrichment score and derive one deterministic canonical ranking.
5. Annotate candidates with clinical phase, mechanism, target, and SMILES from the repurposing hub.
6. Evaluate expected reverser and mimic controls and summarize mechanism enrichment.
7. Ground the highest-ranked and approved candidates in primary literature, including a rationale for the canonical first-ranked hit.
8. Optionally add target nomination, drug-likeness descriptors, and clinical-trial novelty checks.

## Decision rules

- Require the disease name match to be confirmed when the match is ambiguous.
- Use a common gene background for fair size correction and cross-signature overlap.
- Interpret positive size-corrected reversal score as disease-signature reversal.
- Use one deterministic canonical rank for every table, figure, literature slate, and report statement.
- Treat Broad Hub phase Launched as approved.
- Count a control as matching expectation only when its direction and FDR significance both agree.
- Always include and rationalize the canonical first-ranked hit, even when it is non-approved or likely nonspecific.
- Use intervention-verified clinical-trial counts rather than loose full-text query totals.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_f161b2600542f077` — LINCS L1000 disease and perturbation signatures: A transcriptomic connectivity-reversal screen is required.
- `rr_3d7530dbe4d4d92e` — Broad Repurposing Hub: Clinical phase, mechanism, target, and SMILES annotations are required.

### Secondary resources

- `rr_04f1061fb9af09a5` — MGI mouse-to-human ortholog mapping: Murine perturbation signatures must be harmonized to human disease genes.
- `rr_173e1c1b4d0bb143` — ClinicalTrials.gov: An optional indication-specific trial novelty check is requested.

### Fallback resources

- `rr_aee0cca7a24d0430` — Bundled MGI ortholog fallback table: The current MGI ortholog table cannot be retrieved at runtime.

### Optional resources

- `rr_40f53a0b744cf7fb` — LINCS L1000: Disease and single-drug up/down perturbation signatures.
- `rr_dccf06e079b40580` — MGI: Mouse-to-human ortholog mapping.
- `rr_5d8529d3ad92c7ab` — rdkit: Optional Lipinski and Veber drug-likeness descriptors from annotated SMILES.
- `rr_52851b58827148a2` — Size-corrected reversal score S_reversal = z_reversal − z_mimic: Combines reversal and mimic overlap z-scores and calibrates significance with a permutation null.
- `rr_8c81dbcef69d1418` — Enrichment cross-check + canonical ranking: Provides a second connectivity statistic and consensus ranking.

## Validation / QC

- Remove ambiguous genes, signatures with fewer than five genes, and mismatched organism identifiers before scoring.
- Compare overlap-based reversal with the independent enrichment statistic.
- Require statistically significant expected-direction controls for a strong validation verdict.
- Reconcile all downstream artifacts to the same canonical integer rank.
- Require a grounded rationale for the canonical first-ranked hit.
- Evidence requirement: Retain reversal and mimic p-values and FDR values with each control verdict.
- Evidence requirement: Ground every narrative claim and reference in retrieved records.
- Evidence requirement: Every reference must include a verifiable PMID, DOI, or URL locator.
- Evidence requirement: Frame results as hypothesis-generating rather than proof of clinical efficacy.

## Failure handling

- The disease signature is incorrect, weak, or mixes incompatible up/down definitions.
- Murine perturbation signatures are compared without ortholog mapping.
- Different outputs silently use different sorting keys.
- A control is labeled successful based on sign despite nonsignificant FDR.
- A high-scoring nonspecific compound is presented as an efficacious clinical candidate.
- Fallback rule: Use the bundled ortholog table when current MGI retrieval is unavailable.
- Fallback rule: When controls fail or are weak, label the candidate slate exploratory and lead with the validation failure.
- Fallback rule: Use an explicit nonspecific or assay-artifact rationale when that is the best-supported explanation for the top hit.
- Fallback rule: Flag drugs absent from the perturbation library as unscorable rather than assigning a score.

## Limitations

- Transcriptomic reversal is hypothesis-generating and does not prove clinical efficacy.
- The workflow uses gene sets rather than the continuous L1000 z-score matrix.
- Perturbation signatures vary across cell lines, doses, and species, and only library drugs can be scored.
- A high connectivity score can reflect an irrelevant, promiscuous, or harmful mechanism.

## Important domain-specific rules

- Three-mode disease-signature input contract.
- Cross-species identifier harmonization and common-background construction.
- Permutation-calibrated reversal scoring with an independent enrichment cross-check.
- One canonical rank propagated across all downstream consumers.
- Significance-aware positive and negative control validation.

## Portability boundary

- Packaged internal scripts, kernel cache invalidation, and /mnt/results path orchestration. — migration action: `exclude_or_capability_map`
- Biomni LiteratureSearch and ToolSearch calls for evidence grounding. — migration action: `exclude_or_capability_map`
- Biomni GenerateImage and Read media-output-check tool identifiers. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation skill, Phylo report gates, and internal report builder. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
