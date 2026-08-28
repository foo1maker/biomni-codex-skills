# Generative Molecule Design

Source workflow: `generative-molecule-design`  
Parent Claude Science skill: `molecular-design-and-structure`

## Purpose

Generate target-focused small-molecule hypotheses with graph-based evolution, multi-objective scoring, novelty and chemistry filters, and optional retrosynthesis assessment.

## When to use

- Target-focused de novo small-molecule generation
- Multi-objective scoring and chemistry-aware candidate triage
- Optional top-candidate retrosynthesis feasibility assessment

## Inputs

- A molecular target and a target-specific activity backend or the data needed to build one (required)
- Seed or reference molecules from ChEMBL, the Broad Drug Repurposing Hub, or user-provided SMILES (required)
- Optional labeled active and inactive compounds for a QSAR activity backend (optional)
- Optional target structure and pocket definition for a Vina activity backend (optional)

## Outputs

- A ranked set of generated molecular designs
- Per-candidate property and score tables
- Optional retrosynthesis routes or an explicit skipped status
- Design-space figures and a PDF report

## Workflow

1. Select an activity backend from a TDC oracle, a fitted QSAR model, or Vina according to available target evidence.
2. Assemble seed molecules and ground the target rationale in literature.
3. Transform activity, QED, and synthetic-accessibility components to the zero-to-one interval and combine them with a geometric mean.
4. Generate candidates with graph genetic-algorithm crossover and mutation while rejecting invalid or disconnected structures.
5. Deduplicate, enforce novelty, rank by activity and QED, apply PAINS and ring-sanity checks, and retain candidates with acceptable synthetic-accessibility scores.
6. Run heavy retrosynthesis only on the top candidates and preserve a graceful skipped state when models are unavailable.

## Decision rules

- Use a TDC oracle when one exists for the target; otherwise use QSAR when labeled actives and inactives exist; otherwise use Vina when a structure and pocket exist; otherwise gather data before generating.
- Use a geometric mean of normalized scoring components rather than a raw weighted sum.
- Reject invalid, disconnected, or out-of-range structures during generation.
- Apply the stated novelty cutoff of Tanimoto similarity below 0.4 and the synthetic-accessibility cutoff of at most 4.5 during final selection.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_03d59d8726a45265` — TDC oracle, QSAR, or AutoDock Vina activity backend: Selected according to the ordered backend decision rule and the available target evidence

### Secondary resources

- `rr_44cdbee5c2e73e56` — AiZynthFinder: Retrosynthesis assessment is requested for the top-ranked subset and the required models are available

### Fallback resources

- `rr_1b587f1c2b9fb6a4` — Synthetic accessibility score: Retrosynthesis models are missing or the heavy retrosynthesis tier is skipped

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Seed and reference molecules
- `rr_0c5cd85e984a45a9` — Broad Drug Repurposing Hub: Seed and reference molecules
- `rr_5d8529d3ad92c7ab` — rdkit: Molecular graph operations, BRICS operations, descriptors, fingerprints, and chemistry filters
- `rr_b09421cb72f34b4a` — pytdc: Access to target-specific activity oracles when available
- `rr_4e25afd1b941072b` — AutoDock Vina: Structure-based activity surrogate when a target structure and pocket are available
- `rr_d28e584352dc8b5e` — AiZynthFinder: Optional retrosynthesis planning for the top candidate subset
- `rr_2483ba516c173aaf` — Target-specific TDC oracle: Preferred activity backend when available
- `rr_2146aa7042caa628` — QSAR model trained on labeled actives and inactives: Activity backend when no TDC oracle exists and suitable labeled data are available

## Validation / QC

- Track molecular validity, uniqueness, novelty, property scores, PAINS flags, ring sanity, and synthetic-accessibility scores through selection.
- Constrain generated molecules to 8 through 50 heavy atoms.
- Run expensive retrosynthesis only on a bounded top-N subset.
- Evidence requirement: Present generated candidates as computational hypotheses that have not been synthesized or experimentally tested.
- Evidence requirement: Report the activity backend and all scoring components so surrogate-model exploitation remains visible.
- Evidence requirement: Treat retrosynthesis routes as unvalidated computational proposals.

## Failure handling

- No defensible activity backend is available for the requested target.
- An activity surrogate can be exploited by the generator and yield high predicted scores without reliable real activity.
- Missing AiZynthFinder models prevent the optional retrosynthesis tier.
- Fallback rule: If no activity backend can be justified, gather target-specific data before generation.
- Fallback rule: If retrosynthesis models are unavailable, record the tier as skipped and retain the synthetic-accessibility heuristic.

## Limitations

- Generated candidates are unsynthesized and experimentally untested computational hypotheses.
- QED and synthetic-accessibility scores are heuristic proxies.
- The workflow is not intended for existing-library screening or tight lead-optimization SAR.

## Important domain-specific rules

- Ordered activity-backend selection based on available oracle, labeled data, or structural evidence.
- Geometric-mean multi-objective scoring on normalized components.
- Sequential validity, novelty, activity, PAINS, ring, and synthetic-accessibility triage.
- Graceful optional retrosynthesis tier limited to the top candidate subset.

## Portability boundary

- Biomni database query and LiteratureSearch tool calls for seed and literature collection — migration action: `exclude_or_capability_map`
- GenerateImage, Phylo-branded PDF generation, and platform media checks — migration action: `exclude_or_capability_map`
- Fixed /mnt/shared-workspace cache paths and bundled-script orchestration — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
