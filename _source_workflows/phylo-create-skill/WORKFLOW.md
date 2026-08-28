# Phylo Create Skill

Source workflow: `phylo-create-skill`  
Parent Claude Science skill: `portable-skill-authoring`

## Purpose

Create and evaluate bioinformatics skill packages tailored to Phylo's Biomni platform through intent capture, a domain-specific interview, SKILL.md drafting, representative tests, iterative review, platform packaging, and private skill creation.

## When to use

- Determine what a proposed Biomni skill should do, when it should trigger, and what outputs it should produce.
- Elicit domain requirements for data formats, databases, computation, platform integration, and scientific correctness.
- Draft a bioinformatics SKILL.md and only useful supporting scripts, references, or assets.
- Design and run two or three realistic prompts covering a simple case, an edge case, and a scientifically risky case.
- Review scientific correctness, completeness, format fidelity, platform integration, and material licensing constraints, then revise.
- Package the completed skill for Biomni validation and private-skill preview or save.

## Inputs

- The intended capability and prompts that should trigger it. (required)
- Expected output as a file, report, protocol, code, or database-query result. (required)
- Input file formats, identifier types, expected sizes, and single-sample or batch behavior. (optional)
- Databases and APIs, identifier reconciliation needs, and access or licensing constraints. (optional)
- Compute intensity, execution environment, long-job requirements, and downloadable artifacts. (optional)
- Biomni tool-registry, session-storage, streaming, and blocking-result requirements. (optional)
- Known scientific failure modes and required output sanity checks. (optional)
- An existing draft, if the user wants testing or revision rather than a new draft. (optional)

## Outputs

- SKILL.md with YAML frontmatter, concise trigger description, scope, inputs, outputs, workflow, and scientific caveats.
- Only supporting scripts, references, or assets that materially improve reliability or reuse.
- Two or three representative test prompts and a record of correctness, error detection, and output-format results.
- A revised skill that addresses scientific, completeness, formatting, integration, and licensing findings.
- A Biomni skill package rooted at /mnt/results/skills/<slug>/ with SKILL.md at the folder root.
- A private personal-skill preview and save UI after platform validation; public publication is separate.

## Workflow

1. Extract observed tools, corrections, inputs, and outputs from the conversation, then confirm the intended Biomni capability and triggers.
2. Ask targeted questions about formats, scale, database access, identifiers, compute, platform architecture, scientific failure modes, and validation.
3. Inspect existing skills, active tools, software, helpers, and mounted resources before selecting or duplicating capabilities.
4. Draft the required frontmatter and sections, adding optional database, file-format, or error-handling sections only when relevant.
5. Write a one- or two-sentence description with the domain vocabulary, tools, databases, formats, task, and platform context needed for reliable triggering.
6. Propose two or three realistic test prompts, obtain user confirmation, run them sequentially, and record output quality and error handling.
7. Audit scientific correctness, completeness, format fidelity, Biomni integration where applicable, and material licensing constraints; revise and rerun until reviewed.
8. Write the finished package to the Biomni results skill folder, then call the platform create action last for validation and private preview or save.

## Decision rules

- If the user already has a draft, begin with testing rather than redrafting; the process order is flexible.
- Ask before creating a skill unless the user explicitly requested creation.
- Offer skill creation only for a workflow that is repeated or likely to recur, procedural with stable steps, and worth saving for future tasks.
- Prefer an existing Biomni capability when it fits, but permit external packages, APIs, and custom scripts when they are the better practical choice.
- Read exact usage and schemas before relying on any tool, package, API, or result; never invent a capability.
- Choose material resources based on scientific fit, maintenance, platform compatibility, reproducibility, access, and license terms.
- Add database, file-format, and error-handling sections only when relevant.
- Add supporting files only when they make the skill more reliable or reusable; do not add empty scaffolding.
- Call the platform skill-create action only after the package is written, and make it the final action.
- Do not request public publication automatically; private saving and public publication are separate workflows.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_7bcba92e59fd0609` — Existing Biomni skills, capabilities, tool schemas, installed packages, documented helpers, and mounted data resources.: Inspect first and reuse when they fit the intended workflow.

### Secondary resources

- `rr_f8793421f1b10068` — External packages, public APIs, and custom scripts.: Use when they are the better practical choice after scientific, maintenance, compatibility, reproducibility, access, and license review.
- `rr_249ee1d93b6450a0` — Dedicated Biomni PDF, Word-document, or presentation generation skills.: Use to assemble a formatted deliverable from the domain skill's scientific content and artifacts.

### Fallback resources

- `rr_4eb5cf4a44bc8927` — Testing an existing user draft.: Use when the user already has a draft and does not need a new one.
- `rr_9abea03a722f3fda` — A minimal package containing SKILL.md only.: Use when supporting files would not materially improve reliability or reuse.

### Optional resources

- `rr_46b2dcb0234708bc` — Skill: Discover and load relevant existing Biomni skills, then create the completed private skill package as the final platform action.
- `rr_fc65fe3fccc56254` — ToolSearch: Load a deferred tool only when it is listed by the active Biomni system prompt.
- `rr_5b4465042dd0cd55` — Biomni execution and file tools: Inspect packages, documented helpers, and mounted resources and write the package.
- `rr_ab46658d8f557cff` — pdf-report-generation, docx-generation, and pptx-generation: Assemble formatted reports from scientific content and supporting artifacts.

## Validation / QC

- Identify failure modes that could silently produce scientifically wrong results and require corresponding sanity checks.
- Verify consistent gene or protein identifiers and the intended coordinate build.
- Verify that database citations and accessions are real and that claims match produced artifacts.
- Verify required fields, no-result handling, ambiguous names, and deprecated identifiers.
- Validate generated FASTA, VCF, report, or protocol formats.
- When Biomni-facing, verify tool schemas, streaming event structure, storage paths, and session identifiers.
- Review material licenses, terms, redistribution, and commercial-use restrictions without claiming legal clearance.
- Use two or three realistic prompts spanning a simple request, a difficult edge case, and a scientifically risky case.
- Evidence requirement: Inspect exact tool and resource schemas before depending on them and do not invent tools, APIs, packages, or results.
- Evidence requirement: Support database citations with accurate identifiers and avoid fabricated literature or database accessions.
- Evidence requirement: Make every claim about generated output traceable to an artifact actually produced.
- Evidence requirement: Surface unresolved or restrictive license terms and select an alternative when the intended use is explicitly prohibited.
- Evidence requirement: Record for each test whether the correct output was produced, scientific errors were caught, and the output format was valid.

## Failure handling

- The skill ignores strand or mixes coordinate builds.
- The skill uses non-canonical or inconsistently resolved identifiers.
- The skill fabricates citations or database accessions.
- Claims about outputs do not match the artifacts produced.
- No-result, ambiguous-name, or deprecated-identifier cases are not handled.
- Generated files or protocols do not conform to their expected formats.
- A Biomni-facing skill uses incorrect tool schemas, streaming events, storage paths, or session identifiers.
- A material resource has unresolved or restrictive terms that are not disclosed.
- Fallback rule: When a draft already exists, test and revise it instead of beginning from a blank package.
- Fallback rule: When the user did not request creation, ask before creating and continue only with authorization.
- Fallback rule: When an existing capability does not fit, consider an external package, API, or custom script after resource review.
- Fallback rule: Omit empty supporting directories and ship only files that improve reliability or reuse.
- Fallback rule: Keep scientific content in the domain skill and delegate presentation assembly to the appropriate Biomni generation skill.
- Fallback rule: Revise and rerun tests after user or evaluator feedback until the observed gaps are addressed.

## Limitations

- The workflow is explicitly tailored to Phylo's Biomni platform and its bioinformatics execution model.
- The testing loop is manual and sequential in the archived workflow.
- Test coverage is intentionally limited to two or three representative prompts.
- The prescribed package path and final create action are Biomni platform conventions.
- Saving creates a private personal skill and does not publish it publicly.
- License review should surface constraints but must not be presented as legal clearance.

## Important domain-specific rules

- Intent and trigger capture from observed workflow, corrections, inputs, and outputs.
- Domain interview covering formats, scale, databases, identifiers, computation, outputs, and scientific failure modes.
- Lean skill-document template with frontmatter, scope, inputs, outputs, workflow, and scientific caveats.
- Concise trigger-description guidance using concrete domain vocabulary.
- Two- or three-case evaluation set spanning a typical request, an edge case, and a scientifically risky case.
- Evaluation checklist for correctness, completeness, format fidelity, evidence, and licensing constraints.
- User-reviewed revision loop grounded in observed test failures.

## Portability boundary

- The Phylo Biomni-specific skill-creator orchestration and agent-session assumptions. — migration action: `exclude_or_capability_map`
- Biomni agent-session and E2B sandbox execution context. — migration action: `exclude_or_capability_map`
- Biomni A2 agent tool registry, S3 and Firestore session storage, and SSE streaming architecture. — migration action: `exclude_or_capability_map`
- Biomni Skill discovery, ToolSearch loading, active-session tool inspection, documented biomni.tool helpers, and mounted datalake resources. — migration action: `exclude_or_capability_map`
- Biomni report-skill delegation to pdf-report-generation, docx-generation, and pptx-generation. — migration action: `exclude_or_capability_map`
- Biomni package location /mnt/results/skills/<slug>/ and Biomni local, mounted, or chat output conventions. — migration action: `exclude_or_capability_map`
- Final Skill(action="create") call with skills/<slug>, validation, preview/save UI, and private-skill semantics. — migration action: `exclude_or_capability_map`
- Biomni tool, agent-session, lab-automation, and platform-trigger language. — migration action: `exclude_or_capability_map`
- Biomni-specific validation of tool schemas, SSE event structures, storage paths, and session identifiers. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
