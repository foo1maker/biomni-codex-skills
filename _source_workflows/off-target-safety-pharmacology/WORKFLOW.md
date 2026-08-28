# Off Target Safety Pharmacology

Source workflow: `off-target-safety-pharmacology`  
Parent Claude Science skill: `small-molecule-modeling-and-safety`

## Purpose

Produce an auditable off-target and secondary-pharmacology liability profile for one small molecule by separating intended targets from off-targets, combining orthogonal prediction engines with ADMET context, benchmarking only against held-out measured compound data, and gating validation claims by data sufficiency.

## When to use

- Triage compound selectivity and likely off-target liabilities.
- Assess secondary-pharmacology and safety-panel risk for a hit or lead.
- Compare liability profiles across analogs.
- Assess cardiac and broader ADMET concerns while disclosing engine coverage.
- Benchmark predictions against the compound's measured bioactivity without contaminating panel construction.

## Inputs

- One compound identifier supplied as a name, SMILES, InChIKey, or ChEMBL identifier. (required)
- Optional adaptive nearest-neighbor panel expansion for novel chemotypes. (optional)
- Optional ADMET-engine selection; automatic mode prefers ADMET-AI and has a narrower fallback. (optional)

## Outputs

- Machine-readable compound, primary-target, prediction-panel, per-engine prediction, measured-ground-truth, agreement, and benchmark artifacts.
- Three data figures showing per-engine off-target rankings, engine-aware ADMET flags, and a benchmark visualization appropriate to the evidence tier.
- Figure-quality-control record and optional workflow infographic.
- Final compound-liability PDF report containing methods, results, conclusions, figures, references, and next steps.

## Workflow

1. Resolve and standardize the compound, then resolve its intended primary target or targets from mechanism annotations.
2. Construct a fixed safety-target panel and optionally expand it from neighbors while keeping source labels and excluding intended targets from the off-target set.
3. Run an engine-stamped ADMET assessment and carry an explicit flag indicating whether approved-drug percentiles are available.
4. Score the panel with a leave-query-out ECFP4 similarity engine and an orthogonal sequence-based DTI engine.
5. Retrieve the query compound's measured potency data, collapse measurements per target, and keep them strictly separate from panel construction.
6. Compute per-engine evidence, four-state agreement, primary-target controls, core-versus-adaptive counts, and a data-sufficiency evidence tier.
7. Consolidate the run into one analysis record and create engine- and tier-aware figures with blank or degenerate-figure checks.
8. Generate the final liability report from the same consolidated data and verified figures.

## Decision rules

- Never use the query compound's own measured activities to construct its prediction panel or target-active sets.
- Exclude the intended primary target and cross-species orthologs from off-target counts and benchmark positives; report primary-target recovery separately as an on-target control.
- Tag fixed core-panel and adaptive-neighbor targets separately; adaptive similarity hits are hypotheses because expansion and the similarity engine are circular.
- Remove the query from every ligand-similarity active set by canonical SMILES and InChIKey-14.
- Define measured active targets at potency of at most 1 µM and potent targets at potency of at most 100 nM, using per-target median pChEMBL.
- Rank and group targets by per-engine evidence and four-state agreement; do not rely on the cross-engine arithmetic consensus alone.
- Assign Tier A only when at least 15 off-target targets have measured data and at least 5 are positive; only Tier A supports ROC-AUC and average-precision validation claims.
- Treat Tier B as descriptive rather than validation and Tier C as discovery-only and unvalidated.
- If the primary target cannot be resolved, report both the as-is benchmark and a labeled proxy sensitivity excluding the single most potent measured target.
- Use DeepPurpose absolute affinity output only for ranking or voting, never as calibrated nanomolar affinity.
- Describe only ADMET endpoints actually produced by the recorded engine; the narrower fallback has neither hERG nor percentiles.
- Do not interpret a high ROC-AUC as certification of toxicological safety or absence of untested liabilities.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_de1fa4a4e43d1fa4` — Fixed 32-target core safety panel: Use as the independent, clinically motivated denominator for routine secondary-pharmacology triage.
- `rr_065192d732b98e7c` — ECFP4 leave-query-out ligand similarity: Use as the primary off-target prediction engine across panel targets with adequate known active ligands.
- `rr_7ae1dcfa79a3ac35` — ChEMBL measured bioactivity: Use as the compound-specific ground truth for evidence-tier assignment and benchmarking.
- `rr_7f705bf67ffd73c1` — ADMET-AI: Prefer for broad endpoint coverage, hERG prediction, and ChEMBL approved-drug percentile context.

### Secondary resources

- `rr_4c614380f308b8e4` — DeepPurpose morgan_cnn_bindingdb: Use as an orthogonal sequence-based DTI vote; interpret output as ranking rather than calibrated affinity.
- `rr_1e978295e3042f84` — Adaptive nearest-neighbor panel expansion: Use optionally for novel chemotypes, keeping adaptive targets and hits distinct from core-panel independent evidence.

### Fallback resources

- None declared in the normalized source record.

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Compound resolution, mechanism annotations, target active sets, neighbor-target expansion, measured bioactivity, and approved-drug ADMET reference data.
- `rr_5d8529d3ad92c7ab` — rdkit: Compound standardization, canonical structure identity, ECFP4 fingerprints, and query exclusion.
- `rr_0d8a4ec7c0d18e90` — deeppurpose: Secondary sequence-based drug–target-interaction prediction engine.
- `rr_6aba97b6dceeffd7` — scikit-learn: Similarity-probability and benchmark metric support.
- `rr_178baeb270b10c13` — pandas: Tabular pipeline data handling.
- `rr_d2c8df897725078b` — ADMET-AI: Preferred broad ADMET prediction engine.
- `rr_39c264780f6359ac` — ECFP4 maximum-similarity logistic model: Primary predictor using radius-2, 2,048-bit fingerprints and a logistic mapping of maximum similarity to probability.

## Validation / QC

- Record canonical structure identity and exclude the query from similarity training data by both canonical SMILES and InChIKey-14.
- Keep primary-target, core-panel, and adaptive-panel rows explicitly labeled throughout prediction, counting, and benchmarking.
- Collapse ortholog pairs by preferred target name in the benchmark scatter while retaining distinct-protein counts by UniProt accession.
- Record ADMET engine and percentile availability and branch figures and narrative on those fields.
- Fail on blank or degenerate data figures and use a tier-appropriate benchmark panel.
- Validate that the report data include core/adaptive counts, engine agreement, and primary-target handling; then check PDF precision, blank pages, page count, and rendered media.
- Evidence requirement: Use only the query compound's measured ChEMBL potency data as the real validation signal and keep it independent of panel construction.
- Evidence requirement: Show per-engine scores and four-state agreement for each highlighted target.
- Evidence requirement: Split similarity-hit counts into core-panel and adaptive-panel counts and label adaptive hits as hypotheses.
- Evidence requirement: State the evidence tier and measured-target and positive counts beside every validation or discovery claim.
- Evidence requirement: Report the benchmark value calculated from the same run and data as the figures because ChEMBL changes over time.
- Evidence requirement: Treat structure-level follow-up, if performed, as pose plausibility rather than an independent affinity estimate.

## Failure handling

- Data leakage from scoring a compound against target-active sets that contain the same compound.
- Counting the intended primary target as an off-target liability.
- Combining core and adaptive similarity hits into one headline count despite adaptive circularity.
- Calling a Tier B or Tier C result validated.
- Averaging incomparable engine scales and hiding disagreement behind a single consensus score.
- Quoting DeepPurpose output as a calibrated nanomolar affinity.
- Reporting hERG or percentile results from an ADMET engine that did not produce them.
- Equating recovery of known panel actives with proof of toxicological safety.
- Fallback rule: If broad ADMET prediction is unavailable, use the narrower recorded fallback but disable hERG and percentile claims.
- Fallback rule: If the primary target is unresolved, provide an as-is benchmark plus the labeled most-potent-target-excluded proxy sensitivity and state residual contamination risk.
- Fallback rule: When off-target measured overlap is below the Tier A gate, downgrade to descriptive Tier B or discovery-only Tier C rather than forcing validation metrics.
- Fallback rule: If an optional infographic is absent, continue with the verified data figures; the source states the report degrades gracefully without it.

## Limitations

- Every off-target score is a hypothesis rather than an experimentally measured affinity.
- The workflow does not predict primary-target potency and cannot certify toxicological safety.
- Adaptive expansion is circular with ligand similarity, so adaptive hits are not independent corroboration.
- The two prediction engines operate on different scales and are not directly comparable.
- ChEMBL is a living database, so active sets and benchmark values can drift between runs.
- Non-human orthologs can create near-duplicate targets and require explicit collapse rules in some summaries.
- A strong benchmark only measures recovery of known actives among tested panel targets and cannot rule out untested liabilities.

## Important domain-specific rules

- Leave-query-out benchmarking that separates a compound's own measured data from every prediction input.
- Explicit primary-target and ortholog exclusion from off-target panels and metrics, with recovery reported as a separate control.
- Core-versus-adaptive panel provenance and separate hit accounting to expose circular evidence.
- Orthogonal-engine evidence represented by per-engine values and a four-state agreement label.
- A measured-data sufficiency gate that restricts validation claims to a predefined threshold and otherwise downgrades the language.
- Engine-stamped endpoint coverage that prevents unsupported percentile or hERG claims.
- One consolidated run record feeding every number and figure in the report.

## Portability boundary

- The packaged pipeline scripts, one-shot driver, exact command lines, one-line status protocol, and internal output-directory contract. — migration action: `exclude_or_capability_map`
- The package-relative core-panel, ChEMBL approved-reference, report-reference, evaluation, and infographic assets. — migration action: `exclude_or_capability_map`
- The Biomni MPNN ADMET fallback and its narrower endpoint contract. — migration action: `exclude_or_capability_map`
- The Biomni GenerateImage and pdf-report-generation orchestration for infographic and terminal report creation. — migration action: `exclude_or_capability_map`
- The optional Biomni HPC co-folding stage using Boltz-2 or Chai-1. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
