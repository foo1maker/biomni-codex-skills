# Provenance

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:20:03.674Z`
- Source URL: https://biomni.phylo.bio/skills/skill_80991743e52842abb92207cd7ff8c29e?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Intent, trigger, scope, and output capture | phylo-create-skill | Step 1: Capture Intent; Process Overview | 2026-08-14T12:20:03.674Z | Retained portable authoring rationale and explicit authorization boundary. |
| Domain interview for formats, resources, identifiers, compute, and failures | phylo-create-skill | Step 2: Bio-Specific Interview; Data & Formats; Databases & APIs; Compute & Environment | 2026-08-14T12:20:03.674Z | Retained questions as portable inputs and excluded platform architecture. |
| Lean document template and description guidance | phylo-create-skill | Step 3: Write the SKILL.md; Required sections; Optional sections; Description writing tips for biology skills | 2026-08-14T12:20:03.674Z | Mapped to portable frontmatter and required sections only. |
| Test design and observed evaluation | phylo-create-skill | Step 4: Test Cases; Step 5: Evaluate & Iterate | 2026-08-14T12:20:03.674Z | Retained normal/edge/risky prompts and revision loop. |
| Scientific correctness, citations, artifacts, formats, and license review | phylo-create-skill | Scientific Correctness; What to look for in bio skill outputs | 2026-08-14T12:20:03.674Z | Retained checks without platform-specific schemas or calls. |
| Platform exclusion boundary | phylo-create-skill | Agent Architecture (if Biomni-facing); Biomni Resources & Reuse; Presentation and report delegation; Step 6: Package & Present | 2026-08-14T12:20:03.674Z | Explicitly excluded platform creation, packaging, APIs, brand, orchestration, and publication. |

The source limitation that the archived workflow is platform-tailored is why
this derived skill retains only portable authoring, testing, documentation,
provenance, and license-review behavior.
