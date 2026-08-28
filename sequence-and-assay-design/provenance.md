# Provenance

The two source workflows are consolidated only at the portable sequence and
assay-contract level. Primer-specific and guide-specific constraints remain
separate.

| Source skill | Normalized record | Raw visible source | Source URL | Retrieved at | Sections and rules retained |
|---|---|---|---|---|---|
| `pcr-primer-design` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_62f7fe9a2d3e475983cadecafd0547e3?section=marketplace | 2026-08-14T12:19:07.924Z | `When to Use This Skill`; `Inputs`; `Standard Workflow`; `PCR Primer Design`; `Step 1: Load Target Sequence`; `Step 2: Design Primers`; `Step 3: Validate Primers`; `Specificity: what was actually checked`; `Common Issues`; `Outputs`. Retains target/reference declaration, thermodynamic and specificity checks, amplicon validation, MIQE-oriented evidence, and multi-format candidate export. |
| `sgrna-design` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_57ac6963ab7d26bdd50823638a00473e?section=marketplace | 2026-08-14T12:18:25.195Z | `When to use`; `Inputs`; `Option 1 — Validated sequences (ALWAYS try first)`; `Option 2 — CRISPick precomputed designs`; `Option 3 — De-novo design (last resort)`; `Method 2 — Literature & web search (REQUIRED)`; `Outputs`; `Scientific caveats`; `Universal best practice`; `Citations & acknowledgments (preserve in user methods)`. Retains nuclease/PAM/build declaration, library or de novo candidate routes, off-target flags, and PMID/DOI evidence for validated guides. |

## Rule-to-source map

- PCR primer thermodynamics, amplicon boundaries, specificity, and validation
  -> `pcr-primer-design.yaml` sections listed above.
- CRISPR nuclease/PAM, guide ranking, off-target analysis, and validated-guide
  provenance -> `sgrna-design.yaml` sections listed above.

All timestamps and URLs are copied from normalized `source` records. Any
resource named by a source is an optional adapter and must be rechecked before
use.
