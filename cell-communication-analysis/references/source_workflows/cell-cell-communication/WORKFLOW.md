# Cell Cell Communication

Source workflow: `cell-cell-communication`  
Parent Claude Science skill: `cell-communication-analysis`

## Purpose

Infer and summarize ligand-receptor communication between annotated single-cell populations, including pathway activity, network structure, and dominant sender and receiver roles.

## When to use

- Identify ligand-receptor interactions among annotated cell types.
- Visualize interaction counts, strengths, pathways, and sender-receiver roles.

## Inputs

- An annotated Seurat object containing normalized RNA expression and cell-type labels. (required)
- Species-specific CellChat database selection. (optional)
- Metadata column containing cell-type annotations. (required)
- Signaling scope: all signaling, secreted signaling only, or cell-cell-contact signaling only. (optional)

## Outputs

- CSV tables for significant interactions, pathways, interaction matrices, centrality roles, and top interactions.
- PNG and SVG network, chord, bubble, heatmap, and sender-receiver visualizations.
- A serialized CellChat analysis object for comparisons and pathway-specific follow-up.
- A Markdown report and, when available, a PDF report.

## Workflow

1. Load an example or user-provided annotated Seurat object and verify cell and cell-type counts.
2. Run CellChat analysis using the selected species database and grouping column.
3. Generate the full set of communication visualizations.
4. Export tables, the analysis object, and reports.

## Decision rules

- Do not run the workflow on unannotated data; complete single-cell annotation first.
- Use the human or mouse CellChat database to match the input species.
- When example data is selected, use predefined parameters and skip the species and cell-type-column questions.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_8e8c99218b66f534` — CellChat v2: Inferring ligand-receptor communication and signaling roles from annotated single-cell data.

### Secondary resources

- `rr_ab8c77c86de26b52` — Seurat: Providing the normalized annotated single-cell object consumed by CellChat.

### Fallback resources

- `rr_fae1356c600ed6e7` — Packaged Markdown or HTML report fallback: Dedicated PDF reporting is unavailable or rendering fails.

### Optional resources

- `rr_2de7dc9af1067925` — CellChatDB.human: Human ligand-receptor interaction database.
- `rr_6c14b514e9a676d3` — CellChatDB.mouse: Mouse ligand-receptor interaction database.
- `rr_3207a85ab2fa3c8d` — CellChat: Core communication-inference and pathway analysis package.
- `rr_7dadc66200d982df` — Seurat: Annotated single-cell object format and preprocessing dependency.
- `rr_77b4f130e7222be2` — circlize: Chord-diagram rendering.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Signaling-pattern heatmaps.

## Validation / QC

- Use at least three cell types and preferably at least ten cells per type.
- Verify data loading, successful CellChat completion, visualization generation, and export completion at their workflow checkpoints.

## Failure handling

- Too few cells per type or stringent filtering can yield no significant interactions.
- Large datasets can exceed available memory.
- Incorrect metadata-column selection causes a group-by-not-found error.
- Fallback rule: If presto is unavailable, use the workflow's standard-test fallback.
- Fallback rule: If SVG export through svglite fails, use the base R SVG fallback while retaining PNG output.
- Fallback rule: If dedicated PDF reporting is unavailable, retain and disclose the Markdown report fallback.

## Limitations

- The workflow requires annotated single-cell data and is not intended for bulk RNA-seq.
- Spatial communication analysis requires spatial coordinates.

## Important domain-specific rules

- Annotated-input gating followed by load, infer, visualize, export, and checkpoint verification.
- Graceful fallbacks for statistical testing, vector graphics, and report packaging.

## Portability boundary

- Mandatory use of package-specific R script names and the instruction not to write inline code. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation skill and platform-specific report packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
