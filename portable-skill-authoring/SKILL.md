---
name: portable-skill-authoring
description: Author and review portable Agent Skills with concise frontmatter, explicit inputs and outputs, executable workflows, representative tests, provenance, and license-aware failure handling.
---

# Purpose

Turn a recurring, procedural workflow into a small portable Agent Skill or
revise an existing draft. Preserve the scientific contract and evidence
boundaries while avoiding platform-specific tools, APIs, package shells,
orchestration, branding, and publication actions.

# When to use

Use when the user explicitly asks for a reusable skill or for testing/revision
of an existing skill draft. A candidate should have a stable trigger, repeatable
steps, useful inputs/outputs, and enough recurrence to justify maintenance. For
a one-off answer, document the workflow directly instead.

# Inputs

- Capability, trigger language, scope, non-goals, and expected outputs.
- Input formats, identifier types, sizes, batch behavior, and required
  assumptions.
- Candidate resources, schemas, versions, access/licensing constraints, and
  known scientific failure modes.
- Existing draft when the task is revision; otherwise a clear request to create.

# Workflow

1. Capture intent from the request: what the skill does, when it triggers, what
   it must not do, and which artifacts it returns.
2. Interview only missing domain requirements: inputs, formats, identifiers,
   resources, compute needs, outputs, failure modes, and validation checks.
3. Inspect available documentation and resource records before naming a tool or
   API. Use only capabilities whose schema and access are documented.
4. Write a minimal `SKILL.md` with frontmatter, Purpose, When to use, Inputs,
   Workflow, Resource selection, Decision rules, Validation, Failure handling,
   Outputs, and explicit limitations. Add supporting files only when they
   materially improve reliability or reuse.
5. Propose two or three representative tests: a normal case, an edge case, and
   a scientifically risky case. For each, record expected behavior, failure
   detection, output format, evidence, and provenance.
6. Review scientific correctness, completeness, format fidelity, resource
   access/license terms, and claim-to-artifact traceability. Revise and rerun
   tests until observed gaps are addressed.

# Resource selection

Use the local resource registry and authoritative documentation as primary
evidence. External packages or public APIs are optional secondary adapters only
after scientific fit, maintenance, compatibility, reproducibility, access, and
license review. Never invent a resource, schema, identifier, result, or citation.
Record unknown access/license status as unknown and stop if intended use is
prohibited. Do not create or publish platform-specific packages from this skill.

# Decision rules

- If a draft exists, test and revise it before starting from a blank document.
- Keep the description one or two precise sentences with domain vocabulary and
  concrete trigger conditions; keep it within the portable spec limit.
- Keep `name` lowercase kebab-case, 1–64 characters, and identical to the
  folder name. Use only portable frontmatter fields required by the target
  Agent Skills specification.
- Make workflows executable by a capable agent without assuming hidden tools,
  sessions, paths, or credentials.
- Link claims to produced artifacts and sources. A test pass does not establish
  scientific validity beyond the declared test scope.

# Validation

- Check frontmatter syntax, name/folder match, description length, required
  sections, relative links, and absence of platform-only fields or calls.
- Check that input/output contracts, no-result behavior, ambiguous identifiers,
  deprecated IDs, units/builds, and expected file formats are explicit.
- Run normal, edge, and risky test prompts. Record whether the correct output
  was produced, scientific errors were caught, and the output format was valid.
- Verify resource URLs/identifiers, citation anchors, license/access notes, and
  provenance records. Claims must match artifacts actually produced.

# Failure handling

Block authoring when intent or output contract is unresolved. If a named
resource lacks a documented schema or access/license decision, mark it unknown
and provide a declared alternative or stop. Preserve failed tests and partial
drafts with their reasons. Never conceal fabricated citations, mismatched
artifacts, identifier ambiguity, format errors, or restrictive terms.

# Outputs

Return the portable `SKILL.md`, a provenance record, and a test/evaluation record
when requested. Include scope, non-goals, inputs, outputs, workflow, decision
rules, validation gates, failure behavior, resource disclosures, and revision
history. Do not emit a platform package, private-skill save, publication, API
call, or brand-specific report as an implicit side effect.

# Shared policies

- [Evidence policy](../_shared/evidence_policy.md)
- [Resource selection policy](../_shared/resource_selection.md)
- [Provenance policy](../_shared/provenance_policy.md)
- [Validation policy](../_shared/validation_policy.md)
- [Failure handling policy](../_shared/failure_handling.md)

<!-- BEGIN MANAGED: SOURCE WORKFLOWS -->

## Available source workflows

For a specialized request, select the narrowest matching workflow below before using this Skill's generic route. Read its `WORKFLOW.md`, `resources.yaml`, and `provenance.md`; registry presence does not imply runtime availability.

- [`phylo-create-skill`](references/source_workflows/phylo-create-skill/WORKFLOW.md) — Create and evaluate bioinformatics skill packages tailored to platform-specific's platform-specific platform through intent capture, a domain-specific interview, SKILL.md drafting, representative tests, iterative review, platform packaging, and private skill creation.

<!-- END MANAGED: SOURCE WORKFLOWS -->
