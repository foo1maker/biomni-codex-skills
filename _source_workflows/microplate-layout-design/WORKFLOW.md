# Microplate Layout Design

Source workflow: `microplate-layout-design`  
Parent Claude Science skill: `experimental-design-and-plate-layout`

## Purpose

Design randomized 96-well or 384-well plate layouts that reduce positional bias, manage edge effects, balance treatment and covariates, distribute controls, distinguish technical from biological replication, and export auditable lab-ready maps.

## When to use

- Design a layout for a 96-well or 384-well experiment.
- Randomize sample placement to prevent positional confounding.
- Handle edge effects by reserving outer wells or strategically placing controls.
- Balance treatment, replicate, batch, and other covariates across plate positions.
- Distribute controls across plate quadrants.
- Generate image, Excel, and CSV plate maps for bench use.

## Inputs

- Plate format: 96 or 384 wells. (required)
- At least two treatment-group labels. (required)
- Technical replicates per treatment per plate. (required)
- Named positive, negative, and blank control definitions, with counts when present. (required)
- Edge strategy: controls_only, empty, or include. (required)
- Number of plates. (required)
- Assay type: general, dose_response, qpcr, elisa, or cell_viability. (required)
- Number of independent biological preparations. (optional)
- Sample metadata with sample identifier, treatment, replicate, and optional covariates. (optional)
- Reserved-well definitions. (optional)
- Measurands, normalization mode, reference measurands, and interplate calibrator for ratiometric or multi-measurand designs. (optional)
- Multi-plate batch design from an upstream experimental-design analysis. (optional)

## Outputs

- PNG and SVG treatment, sample-type, replicate, edge-risk, and quality-dashboard visualizations.
- Tidy and plate-shaped CSV layouts; multi-plate designs receive one grid export per plate.
- Color-coded Excel workbook when export succeeds, with explicit status and CSV-only fallback on failure.
- Human-readable experiment-parameter, quality, well-census, provenance, and export-status artifacts.
- Serialized complete layout object for downstream analysis.
- Power curve and power metrics, including technical and biological replication context.
- PDF report containing methods, results, conclusions, figures, references, and next steps.

## Workflow

1. Ask first whether the user has a sample-metadata file or wants an example design; only then collect plate, assay, treatment, control, edge, and optional constraints.
2. Define the experiment with explicit plate format, treatments, controls, replication, edge strategy, assay type, covariates, and any reserved wells.
3. For ratiometric or multi-measurand assays, define measurands, normalizers, and an interplate calibrator while keeping each sample's complete block together.
4. Generate a spatially balanced randomized layout with a recorded seed and method.
5. Assess technical power, biological-replication requirements, and sensitivity across effect sizes and biological standard deviations.
6. Check quadrant, row, column, edge, plate, and co-location confounding; regenerate and repeat all dependent checks after any seed or method change.
7. Generate data-derived plate maps and quality visualizations.
8. Export all layouts and verify every artifact, workbook, and well census.
9. Build and verify a final report in which every plate-like figure is either the authoritative layout or clearly marked as a schematic.

## Decision rules

- If example data are selected, skip the remaining clarification questions and proceed with the predefined example parameters.
- Use controls_only by default when edge protection is needed and quantitative edge-behavior data are useful; use empty when edge wells should generate no data; use include only when assay conditions make edge risk acceptable.
- Treat technical replicates as within-plate measurement precision and biological replicates as independent preparations; never substitute technical wells for biological replication.
- For ratiometric designs, keep every measurand and technical replicate from one biological sample on the same plate and include the calibrator on every plate.
- Do not use a Latin-square method for multi-plate ratiometric designs because it can violate sample-block co-location.
- A layout-quality score below 80% triggers a different seed, more optimization iterations, or a different generation method.
- Biological power requires a biological standard deviation; technical standard deviation cannot validate a biological-power claim, and an effect size without a standard deviation leaves biological power unverified.
- If technical power is below 0.80, stop and present choices to add plates, increase replication, explicitly accept underpowering, or test a larger effect size.
- Do not claim power for an effect size that was not tested; present the sensitivity table rather than only the selected value.
- If confounding is detected and the layout changes, rerun power assessment, the power curve, and every confounding check on the replacement layout.
- Any report infographic that depicts a plate must be explicitly labeled as an illustrative schematic and must not resemble the authoritative well assignment.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_5ffd721de239824e` — designit: Use for OSAT plus spatial optimization of plate assignments.
- `rr_7a28d462bb64fb1d` — pwr: Use for mandatory parametric power analysis and replicate planning.
- `rr_81351fec0dd2d5ee` — ggplate: Use for authoritative plate-map visualizations with plate-shaped round wells.

### Secondary resources

- `rr_edbbceb59b8ff47b` — openxlsx: Use for a color-coded lab-bench workbook when available.
- `rr_29fd4402e569eeea` — agricolae: Use as an optional enhanced experimental-design package.
- `rr_cb3548961efe1e7b` — plater: Use as an optional enhanced plate-data package.

### Fallback resources

- `rr_a6c58ad9a3a8f073` — CSV-only export: Use when Excel workbook export fails or openxlsx is unavailable; surface the failure.
- `rr_e86226cbc548dfd5` — Base R SVG export or PNG-only output: Use when SVG support is unavailable.
- `rr_0f351aa1a33d0d30` — A different random seed, more iterations, or another spatial method: Use when layout quality is below 80% or a confounding test fails.

### Optional resources

- `rr_62138e9226cb4ee2` — designit: Spatially optimized experimental design.
- `rr_f3a4936deaf955aa` — ggplot2: General plotting dependency.
- `rr_9c40795c5beef2c4` — ggprism: Plot styling dependency.
- `rr_e623901f1dfcf95c` — jsonlite: JSON serialization of design parameters.
- `rr_33932e0b88956593` — pwr: Parametric power calculations.
- `rr_65b8f1378efd3040` — ggplate: Plate-map visualization.
- `rr_70e2dd7e084ea3ec` — openxlsx: Optional Excel workbook export.
- `rr_b3913b3076e8c9c3` — agricolae: Optional experimental-design utilities.
- `rr_39184bc85edf8b49` — plater: Optional plate-data utilities.
- `rr_8db715ce7dbe586f` — patchwork: Optional multi-panel figure assembly.

## Validation / QC

- Require a layout-quality score of at least 80%, or regenerate with a different seed, more iterations, or a different method.
- Always perform the technical-power assessment, biological-replication plan, effect-size sensitivity table, and power curve.
- Test quadrant, row, column, edge, plate-level, and ratiometric co-location confounding.
- Verify every export is non-empty, confirm that an Excel workbook is valid and larger than 1 KB, and preserve warnings and offenders in the export status.
- Confirm the well census exists before report generation.
- Before report assembly, verify that each plate-depicting figure is data-derived or visibly marked as a schematic, and confirm PDF page count and embedded figures.
- Evidence requirement: Report declared biological and technical replicate counts consistently across summaries, parameter files, and quality reports.
- Evidence requirement: Present the required number of independent preparations for 80% biological power and the power at three and five independent preparations.
- Evidence requirement: Present the full effect-size sensitivity table and discuss whether the selected effect size is realistic for the assay.
- Evidence requirement: Use the actual layout table as the source for authoritative plate maps and disclose any schematic as illustrative.
- Evidence requirement: Tag every parameter as user-provided, example default, function default, or inferred, and do not present inferred or example values as user facts.

## Failure handling

- Positional confounding between treatment and row, column, quadrant, edge, or plate.
- Pseudoreplication caused by treating wells on one plate as independent biological experiments.
- Using technical variability to claim biological power.
- Proceeding silently with power below 0.80.
- Splitting the measurands or technical replicates of one ratiometric sample across plates.
- Presenting a schematic plate image as if it were the real assignment.
- Silently claiming clean export after an Excel failure or empty artifact.
- Fallback rule: If available wells are insufficient, reduce replication, add plates, or switch to a 384-well format.
- Fallback rule: If a layout score is low, increase optimization iterations, try another seed, or use the spatially optimized method.
- Fallback rule: If control distribution is inadequate, increase control counts; the archived guidance suggests at least four per control type for a 96-well plate.
- Fallback rule: If Excel export fails, preserve CSV exports and disclose the workbook failure.
- Fallback rule: If SVG export fails, retain PNG and use the built-in SVG fallback where possible.

## Limitations

- The workflow does not replace genomics-specific power analysis, cross-experiment batch assignment, or downstream plate-reader analysis.
- Technical and biological power answer different questions; plate-level power alone cannot justify a generalizable biological claim.
- The parametric power calculations assume normal residuals and equal variance; heavy tails or mean-dependent variance can reduce realized power.
- The layout-quality score is an unvalidated internal heuristic suited to comparing candidate layouts, not publication evidence.
- Dose-response headline power uses a one-degree-of-freedom monotone linear trend assumption rather than fitting a four-parameter logistic curve.
- Published edge-effect ranges may differ from the user's assay.

## Important domain-specific rules

- A first-question input gate that separates metadata-driven designs from example or manually specified designs.
- Explicit biological-versus-technical replicate vocabulary propagated through all artifacts.
- A co-location constraint for ratiometric sample blocks and interplate calibrators.
- Spatial randomization followed by quantitative power, confounding, and well-census gates.
- An underpowered-design stop gate with explicit redesign or acknowledgment choices.
- A provenance-aware export and figure-integrity gate that distinguishes authoritative data figures from schematics.

## Portability boundary

- The packaged R script entry points, exact commands, verification strings, and low-freedom execution policy. — migration action: `exclude_or_capability_map`
- Built-in example identifiers and package-relative reference documents under scripts/ and references/. — migration action: `exclude_or_capability_map`
- The platform pdf-report-generation, GenerateImage, Read media-output-check, and Skill orchestration used for the terminal report. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
