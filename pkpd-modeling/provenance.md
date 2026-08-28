# Provenance

- Normalized source: `<private-local-path>`
- Raw source: `<private-local-path>`
- Raw metadata: `<private-local-path>`
- Retrieved at: `2026-08-14T12:15:36.763Z`
- Source URL: https://biomni.phylo.bio/skills/skill_1c6f8253c75e4fca5754cfe885207a35?section=marketplace

| Rule group retained | Source skill | Source section | Retrieved at | Mapping note |
|---|---|---|---|---|
| Canonical concentration-time schema and loud exclusion accounting | pkpd-pharmacometrics | Inputs — the schema contract (validate loudly, never drop silently) | 2026-08-14T12:15:36.763Z | Retained required fields, event/endpoint labels, and no-silent-drop rule. |
| Staged PK and delay-aware PD workflow | pkpd-pharmacometrics | Workflow (staged; degrades gracefully to PK-only) | 2026-08-14T12:15:36.763Z | Retained PK-only branch and direct/effect/indirect response choices. |
| Target-in-required dose guardrail | pkpd-pharmacometrics | HARD GUARDRAIL — dose is target-in-required, never target-invented | 2026-08-14T12:15:36.763Z | Retained refusal to emit an unsourced numeric dose. |
| Mandatory diagnostics and evaluation tiers | pkpd-pharmacometrics | Model evaluation tiers; Scientific caveats (carry into every run) | 2026-08-14T12:15:36.763Z | Retained convergence, precision, condition number, VPC, and uncertainty limits. |
| Runtime/access and graceful failures | pkpd-pharmacometrics | CRITICAL environment note — the stack is NOT preinstalled; Workflow (staged; degrades gracefully to PK-only) | 2026-08-14T12:15:36.763Z | Excluded platform bootstrap, report branding, and orchestration. |

The shared policy links define portable evidence, resource, provenance,
validation, and failure requirements.
