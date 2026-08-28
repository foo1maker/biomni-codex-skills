# Provenance

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:05:53.373Z`
- Source URL: https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Required scope clarification and query matrix | omics-dataset-retrieval | Step 1 — Ask upfront clarification questions (MANDATORY before any search); Step 2 — Build search term matrix | 2026-08-14T12:05:53.373Z | Converted interactive questions to portable pre-search inputs. |
| Tiered repository discovery | omics-dataset-retrieval | Repository Coverage Map; Tier 1 — High-yield, programmatic APIs (always query); Tier 2 — Specialized APIs (query when relevant to disease/omics type); Tier 3 — Web search + manual curation (always attempt via WebSearch tool) | 2026-08-14T12:05:53.373Z | Retained repository names and conditional routing, excluding platform tool orchestration. |
| Omics classification and four-tier relevance audit | omics-dataset-retrieval | Step 13 — Classify omics type; Step 14 — Relevance audit (4-tier classification) | 2026-08-14T12:05:53.373Z | Retained title/summary evidence and REMOVE review. |
| Accession/date normalization, deduplication, and access status | omics-dataset-retrieval | Step 15 — Assemble master catalog; Scientific Caveats | 2026-08-14T12:05:53.373Z | Retained native identifiers, aliases, dates, controlled-access labels, and duplicate uncertainty. |
| API limitations and bounded fallbacks | omics-dataset-retrieval | Known API Limitations and Workarounds | 2026-08-14T12:05:53.373Z | Retained documented workarounds and partial-result disclosure. |
| Catalog evidence and report contents | omics-dataset-retrieval | Outputs; Step 16 — Generate Markdown summary report | 2026-08-14T12:05:53.373Z | Kept metadata-only scope and coverage reporting. |

The shared policies linked in `SKILL.md` supply the portable evidence,
resource, provenance, validation, and failure controls.
