# Molecular Property Admet

Source workflow: `molecular-property-admet`  
Parent Claude Science skill: `small-molecule-modeling-and-safety`

## Purpose

Profile small molecules from SMILES for physicochemical properties, drug-likeness, structural alerts, and predicted absorption, distribution, metabolism, excretion, and toxicity endpoints, with standardized inputs, comparative plots, and auditable triage outputs.

## When to use

- Profile small molecules for physicochemical properties, drug-likeness, and predicted ADMET endpoints.
- Assess Lipinski and Veber compliance and quantitative estimate of drug-likeness.
- Flag PAINS and Brenk/NIH structural alerts.
- Predict cytochrome P450 inhibition and hERG, AMES, and drug-induced-liver-injury liabilities.
- Triage screening hits or compare a compound series by developability.

## Inputs

- SMILES strings in a CSV or TSV smiles column, a text file, or a SMILES file. (required)
- Optional molecule names. (optional)
- Optional built-in clean-drug or messy-hit-list example set. (optional)

## Outputs

- Complete per-molecule physicochemical, drug-likeness, structural-alert, and ADMET result table.
- Drug-likeness summary with pass/fail, structural-alert, and input-sanity flags.
- Per-compound PAINS and Brenk/NIH toxicophore substructures.
- Predicted ADMET endpoint table when the prediction model is enabled.
- Triage table for compounds with input-quality, PAINS, Lipinski, hERG, AMES, or liver-injury concerns.
- Physicochemical overview, Lipinski-space, developability, and optional ADMET heatmap figures in PNG and SVG.
- Markdown summary and optional PDF report with reference-percentile context when percentiles are available.
- Serialized complete analysis object for downstream use.

## Workflow

1. Confirm whether the user supplied a SMILES file or wants an example set, then choose full prediction or physicochemical-and-drug-likeness-only analysis and the scientific focus.
2. Load molecules and preserve a unique identifier for every output row.
3. Strip salts and counterions to a drug-like parent, neutralize, canonicalize, deduplicate, and retain invalid or non-drug-like entries with explicit sanity flags.
4. Compute physicochemical descriptors, Lipinski and Veber rules, quantitative estimate of drug-likeness, and PAINS plus Brenk/NIH alerts.
5. When enabled, predict the registered ADMET endpoints and compute commercially permissive approved-drug reference percentiles.
6. Generate scale-aware comparative plots and export all result, summary, alert, flag, and serialized-analysis artifacts.
7. When requested, package the analysis tables, summary, figures, and reference-percentile context into a report.

## Decision rules

- Do not apply the SMILES-based workflow to biologics, peptides, or antibodies.
- Use full ADMET profiling by default; select physicochemical-and-drug-likeness-only mode when the prediction dependency is unavailable or a faster analysis is requested.
- Never silently drop malformed, inorganic, out-of-range, duplicate, or otherwise non-drug-like records; retain them with sanity and standardization provenance.
- Key all outputs with a unique molecule identifier so blank or duplicate names do not collide.
- Compute endpoint percentiles against the bundled ChEMBL approved-small-molecule reference, not the noncommercial DrugBank reference.
- When the ChEMBL reference is missing, disable percentiles with a warning and never silently fall back to DrugBank.
- Treat the ChEMBL reference values as model predictions on approved compounds rather than experimental measurements.
- Treat Brenk/NIH toxicophore alerts as review prompts rather than automatic exclusions; use PAINS counts as the stronger triage flag described by the source.
- Do not render an ADMET heatmap when prediction columns are absent.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_95a6dd57ea90d518` — RDKit: Use for molecular standardization, descriptors, drug-likeness rules, QED, and structural alerts.
- `rr_7f705bf67ffd73c1` — ADMET-AI: Use for the default full predicted-ADMET profile.
- `rr_6cce0f1364e25ddf` — ChEMBL approved small-molecule reference set: Use for commercially permissive percentile context for ADMET-AI endpoints.

### Secondary resources

- `rr_aa9de1c3e2e51274` — seaborn and matplotlib: Use for comparative distribution, scatter, ranking, and endpoint-heatmap figures.
- `rr_0e510cea0ef604a0` — adjustText: Use optionally to reduce label collisions in small-set scatter plots.

### Fallback resources

- `rr_1fd4e93e806a4c7a` — Physicochemical-and-drug-likeness-only analysis: Use when ADMET-AI or its runtime is unavailable or the user requests the faster scope.
- `rr_44aa278999aa68ec` — Absolute ADMET predictions without percentiles: Use with an explicit warning when the ChEMBL approved-drug reference file is missing.
- `rr_2491c341eff14f20` — PNG figures: Retain when SVG export is unavailable.

### Optional resources

- `rr_806458b94ba25e08` — ChEMBL: Source of the approved-small-molecule percentile reference set.
- `rr_5d8529d3ad92c7ab` — rdkit: Molecular standardization, descriptors, rules, QED, and structural alerts.
- `rr_178baeb270b10c13` — pandas: Tabular data handling.
- `rr_78609fcc8c7914c6` — numpy: Numerical data handling.
- `rr_8710bac9b55761af` — seaborn: Statistical visualization.
- `rr_665edbde6fc3cd1c` — matplotlib: Figure rendering.
- `rr_d2c8df897725078b` — ADMET-AI: Pretrained molecular-property and ADMET prediction software.
- `rr_f48c55f08412b6c6` — requests: Optional access used only when rebuilding the ChEMBL reference.
- `rr_af381e2795322d05` — adjustText: Optional scatter-label de-collision.
- `rr_fe870c90ceecfa70` — ADMET-AI pretrained models: Predict approximately 41 ADMET endpoints without training a new QSAR model.

## Validation / QC

- Preserve original and standardized SMILES, standardization notes, unique molecule identifiers, aggregated names, and sanity flags.
- Retain invalid structures and exclude them only from calculations that require a valid standardized molecule.
- Inspect the standardization note and molecular-weight change when a largest-fragment salt rule may have selected a large organic counterion.
- Verify that percentile-column names identify the ChEMBL approved-drug source rather than DrugBank.
- Build the ADMET heatmap only from registered classification endpoints that are present.
- Evidence requirement: Report both absolute predicted ADMET values and ChEMBL approved-drug percentile context when the reference is available.
- Evidence requirement: Attribute ChEMBL data and its CC BY-SA 3.0 terms in percentile-bearing reports.
- Evidence requirement: Describe percentile-reference values as ADMET-AI predictions on the reference compounds, not experimental measurements.
- Evidence requirement: Use experimental hERG or AMES follow-up for safety confirmation of flagged compounds.

## Failure handling

- Applying the workflow to a molecular modality for which SMILES-based small-molecule models do not apply.
- Silently dropping invalid, non-drug-like, or duplicate inputs.
- Using a DrugBank-derived reference in a commercial workflow despite its noncommercial license.
- Reporting percentile context when the approved-drug reference was unavailable.
- Treating a structural alert or model prediction as experimental proof of toxicity.
- Treating a model-predicted reference percentile as an experimental population percentile.
- Fallback rule: If ADMET-AI is unavailable, run physicochemical, drug-likeness, QED, and structural-alert analysis without ADMET prediction.
- Fallback rule: If the approved-drug reference is missing, disable percentiles, issue a warning, and keep absolute predictions.
- Fallback rule: If an input cannot be standardized, retain its row, mark standardization failure, and exclude it from ADMET prediction.
- Fallback rule: If the ADMET heatmap prerequisites are absent, return the remaining three plot families.
- Fallback rule: If SVG export fails, retain PNG output.

## Limitations

- The workflow is not intended for biologics, protein–ligand docking, model training, reaction prediction, or retrosynthesis.
- ADMET values are predictions from pretrained models, not measured endpoints.
- The largest-fragment desalting rule can choose the wrong parent when a large organic counterion outweighs the active component.
- Brenk/NIH toxicophore alerts are common among approved drugs and are advisory rather than exclusion criteria.
- Reference percentiles describe position among model predictions for a ChEMBL approved-drug set and depend on reference provenance and model version.

## Important domain-specific rules

- A standardize-but-retain input policy that canonicalizes and deduplicates valid structures while preserving every invalid or unusual record with provenance.
- A two-scope analysis gate separating lightweight physicochemical triage from full predicted ADMET profiling.
- A commercially permissive reference-percentile layer that leaves absolute predictions unchanged and disables cleanly when its reference is unavailable.
- A triage separation between PAINS flags, advisory toxicophore alerts, rule-based drug-likeness, and predicted safety endpoints.
- Scale-aware visualization that switches from labeled scatter to density representation for large compound sets.

## Portability boundary

- The packaged Python modules, exact function calls, verification strings, and low-freedom script-execution policy. — migration action: `exclude_or_capability_map`
- The package-relative ChEMBL reference asset, metadata, license document, report implementation, and endpoint registry. — migration action: `exclude_or_capability_map`
- The Biomni pdf-report-generation skill and platform report orchestration. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
