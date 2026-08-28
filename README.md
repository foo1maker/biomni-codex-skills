# Biomni-derived Codex Skills

A portable, public collection of 32 Codex-compatible skills distilled from
visible Biomni research-workflow descriptions. Each skill includes its
`SKILL.md`, provenance notes, and the mapped source workflows. Shared policy
modules and a source-workflow map are included.

This is an independent adaptation, not an official Biomni, Phylo, or Stanford
project.

## Contents

- 32 top-level Codex skills under directories such as
  `single-cell-rna-seq-analysis/` and `molecular-design-and-structure/`.
- `_shared/` with five reusable scientific policy modules.
- `_source_workflows/` with 80 normalized workflow cards.
- `03_resource_registry/` with a portable resource catalog and access audit.
- `SOURCE_WORKFLOW_MAP.yaml` and `SOURCE_WORKFLOW_MAP.csv` with workflow routing.

## What Is Excluded

- Raw Biomni/Phylo page captures and exact web-skill source text.
- Biomni data lake, model weights, package files, MCP configuration, and
  deployment state.
- Credentials, tokens, private keys, personal account details, and local
  filesystem paths.
- Third-party datasets and binaries referenced by the workflows.

## Installation

Copy the skill directories you need into your Codex skills root. For example:

```powershell
Copy-Item single-cell-rna-seq-analysis "$HOME\.codex\skills\"
Copy-Item _shared "$HOME\.codex\skills\"
```

The `_shared` directory is referenced by the skill policies. The repository is
a portable reference package; it does not install tools, MCP servers, or
credentials.

## Attribution And Licensing

This package is provided under Creative Commons Attribution 4.0 International.
The upstream Biomni/Phylo service and third-party resources referenced by the
workflows have their own terms and are not cleared for redistribution by this
repository. See `LICENSE` and `NOTICE`.
