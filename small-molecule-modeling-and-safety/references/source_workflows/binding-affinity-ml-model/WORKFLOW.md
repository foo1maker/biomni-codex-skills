# Binding Affinity Ml Model

Source workflow: `binding-affinity-ml-model`  
Parent Claude Science skill: `small-molecule-modeling-and-safety`

## Purpose

Curate target-specific ChEMBL small-molecule affinity data, benchmark regression models under scaffold splits, and rank novel-scaffold library candidates with explicit applicability-domain tiers.

## When to use

- Build a curated single-target small-molecule affinity regression dataset.
- Benchmark fingerprint, optional graph-neural-network, and explicitly requested DeepPurpose models.
- Screen a clinical or user-supplied compound library for novel scaffolds.

## Inputs

- A target gene or protein symbol, or explicit ChEMBL target identifiers. (required)
- A configuration overriding endpoints, molecular-weight cutoff, CV repeats, applicability-domain thresholds, framework settings, or library definition. (optional)
- An external CSV library containing a smiles column. (optional)

## Outputs

- Curated compound, scaffold, and assay-group dataset.
- Cross-validation summary, fold metrics, and selected-model out-of-fold predictions.
- Framework provenance recording requested, executed, and fallback modeling frameworks.
- All scored candidates with applicability-domain tiers and a high-confidence shortlist.
- Applicability-domain tier summary, figures, and a target-specific report.

## Workflow

1. Resolve and confirm the intended ChEMBL target record before data retrieval.
2. Fetch and curate exact-relation nanomolar IC50, Ki, Kd, and EC50 measurements, standardize structures, aggregate replicates, assign scaffolds and assay groups, and enforce a minimum-data gate.
3. Benchmark available models on identical scaffold and random folds with leakage-free inner validation for deep models.
4. Select the production model by scaffold-split Spearman and train it on all curated compounds.
5. Screen the external or ChEMBL clinical library, assign high, borderline, or out-of-domain tiers, and shortlist only high-tier novel scaffolds.
6. Generate figures and a report that discloses CV design, applicability-domain tiers, data regime, and any framework fallback.

## Decision rules

- Attempt an explicitly requested modeling framework or disclose prominently that it was unavailable and identify the actual fallback; never present fallback results as the requested framework.
- If fewer than 100 curated compounds remain, do not run a GNN; re-scope the dataset or use fingerprint-only modeling.
- With fewer than about 200 compounds, proceed only with discovery-grade expectations and do not assume a deep model will outperform fingerprints.
- Select the model with the best scaffold-split Spearman rather than R².
- Treat random-split performance as an optimistic reference rather than the headline estimate.
- Shortlist only high applicability-domain candidates; borderline and out-of-domain compounds remain low-confidence transparency rows.
- Confirm whether the resolved ChEMBL record is the intended single protein, PPI target, mutant, or ortholog.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_3382acd1406cf997` — EBI ChEMBL bioactivity and clinical or approved compound data: Building the target-specific training set and no external screening library replaces the default library.

### Secondary resources

- `rr_2bfaeb478a8dda14` — User-supplied SMILES library: The user provides an external compound collection for scoring.

### Fallback resources

- `rr_812e848632bffa2a` — Native fingerprint and graph models: An explicitly requested external framework cannot run and the failure is recorded and disclosed.

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Only external source for bioactivity, structures, and the default clinical or approved screening library.
- `rr_5d8529d3ad92c7ab` — rdkit: Structure standardization, molecular descriptors, scaffolds, and Morgan fingerprints.
- `rr_6aba97b6dceeffd7` — scikit-learn: Fingerprint Random Forest and Gradient Boosting benchmarks.
- `rr_b445da126d4e26a7` — PyTorch Geometric: Optional message-passing graph neural network.
- `rr_0d8a4ec7c0d18e90` — deeppurpose: Explicitly requested deep compound-property model framework.
- `rr_5a8043c56ba93655` — Morgan-fingerprint Random Forest: Fingerprint baseline and preferred screening model when it wins scaffold-split evaluation.
- `rr_cc3c01dafb57f5c3` — Gradient Boosting regression: Fingerprint-based benchmark competitor.
- `rr_023ea4715332a5e4` — Message-passing graph neural network: Optional graph model when dataset size supports it.
- `rr_775191876438adfc` — DeepPurpose compound-property model: First-class competitor when explicitly requested; default drug encoding is CNN.

## Validation / QC

- Inspect per-target and per-endpoint counts and stop when the curated set is below the minimum-compound gate.
- Use identical scaffold and random folds across model competitors.
- For deep models, derive early-stopping validation only from training indices and fit target scaling on training data.
- Write framework request, execution status, failure reason, and fallback to framework provenance.
- Validate report pages and confirm figures are not blank or clipped.
- Evidence requirement: Use scaffold-split Spearman and fold variability as the primary model-selection evidence.
- Evidence requirement: Retain nearest-neighbor similarity, prediction-range, and disagreement flags for every novel-scaffold candidate.
- Evidence requirement: Record the ChEMBL release and preserve ChEMBL identifiers and license obligations in redistributed derivative data.
- Evidence requirement: Present screened potencies as discovery-grade rankings requiring experimental confirmation.

## Failure handling

- The resolved target contains fewer than the minimum number of drug-like small-molecule measurements.
- An explicitly requested framework fails to import or train.
- A deep model leaks test-fold information through early stopping or target scaling.
- Screened molecules are weakly similar, outside the training prediction range, trivial near analogs, or highly uncertain.
- The automatically resolved target record is not the intended biological entity.
- Fallback rule: When a requested framework cannot run, record the reason, use native models only if appropriate, and disclose the substitution in the report.
- Fallback rule: For fewer than 100 compounds, pool supported endpoints, add a relevant single-protein target, or re-evaluate small-molecule tractability before modeling.
- Fallback rule: For small datasets, disable the GNN and retain fingerprint baselines.
- Fallback rule: When no user library is supplied, use ChEMBL clinical or approved small molecules.

## Limitations

- The workflow is limited to single-endpoint small-molecule affinity regression and excludes classification, docking, ADMET, de novo generation, and multi-target selectivity.
- Small datasets can produce modest scaffold-split performance and high fold variance.
- Pooling IC50, Ki, Kd, and EC50 mixes functional and direct-binding measurements despite assay-group tracking.
- The molecular-weight filter excludes peptides and macrocycles and may remove most relevant chemistry for PPI targets.
- Applicability-domain tiering reduces but does not eliminate uncertainty; predictions remain unvalidated hypotheses.

## Important domain-specific rules

- Gate modeling on the reality of the curated chemical dataset rather than raw activity counts.
- Use scaffold-split model comparison with leakage-free inner validation and random splits only as an optimistic reference.
- Record and disclose requested-framework execution and fallback provenance.
- Separate high, borderline, and out-of-domain candidates and shortlist only the supported tier.
- Carry data-source version, identifiers, attribution, and share-alike obligations into derivative outputs.

## Portability boundary

- Bundled skill-local curation, benchmarking, screening, figure, and report scripts. — migration action: `exclude_or_capability_map`
- Biomni /mnt/results/qsar_run output convention and Phylo report styling. — migration action: `exclude_or_capability_map`
- Skill-specific Python, uv, CPU, PyTorch Geometric, and DeepPurpose runtime assumptions. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
