# Provenance

This portable skill was distilled from the normalized source snapshot captured
on 2026-08-14. Platform-specific orchestration, managed jobs, private paths,
agent shells, and package-specific command contracts were excluded. The rules
below are capability mappings, not claims that any named adapter is available.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `tusoai` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| `tusoskill` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_3433ea4640ad46929510cc17c8e4d606?section=marketplace | 2026-08-14T12:22:58.46Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| CMD-1 | Protect scoring code, splits, labels, held-out data, and evaluator invariants. | `tusoai` | `F. The evaluator and hidden data are protected` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-2 | Do not let candidates read hidden labels, evaluator internals, expected outputs, or network data. | `tusoai` | `F. The evaluator and hidden data are protected` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-3 | Quantify evaluator noise and require clean revalidation before accepting an improvement. | `tusoai` | `Phase 1 — Build and prove the evaluator`; `Phase 6 — Select, revalidate, and report` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-4 | Record hypotheses, metrics, resources, hashes, validation evidence, and uncertainty. | `tusoai` | `Phase 6 — Select, revalidate, and report` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-5 | Continue under the authorized budget; a plateau or one good result is not completion. | `tusoai` | `Mission and completion standard` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-6 | Freeze a benchmark card, run trivial/shuffled sanity candidates, and use controlled ablations. | `tusoskill` | `Phase 1: understand the task and freeze the benchmark`; `Diagnostics and ablations` | https://biomni.phylo.bio/skills/skill_3433ea4640ad46929510cc17c8e4d606?section=marketplace | 2026-08-14T12:22:58.46Z |
| CMD-7 | Audit protected hashes, train-only preprocessing, subgroup behavior, and repeated seeds. | `tusoskill` | `Anti-gaming and leakage audit` | https://biomni.phylo.bio/skills/skill_3433ea4640ad46929510cc17c8e4d606?section=marketplace | 2026-08-14T12:22:58.46Z |
| CMD-8 | Select the simplest robust finalist and package exact reproduction evidence. | `tusoskill` | `Final selection and delivery` | https://biomni.phylo.bio/skills/skill_3433ea4640ad46929510cc17c8e4d606?section=marketplace | 2026-08-14T12:22:58.46Z |
| CMD-M1 | Evaluator cannot faithfully exercise the method or produce a stable finite metric. | `tusoai` | `Phase 1 — Build and prove the evaluator` | https://biomni.phylo.bio/skills/skill_4824c98eebb04d268bd7434b34682102?section=marketplace | 2026-08-14T12:22:46.43Z |
| CMD-M2 | Candidate relies on benchmark artifacts or hidden information. | `tusoskill` | `Anti-gaming and leakage audit` | https://biomni.phylo.bio/skills/skill_3433ea4640ad46929510cc17c8e4d606?section=marketplace | 2026-08-14T12:22:58.46Z |

## Resource provenance

No named implementation is required by this portable contract. Any baseline,
evaluator, dataset, model, or package selected at run time must be a
provenance-policy record with registry key, version, access/license status,
input hash, and role (`primary`, `secondary`, or `fallback`). The normalized
sources explicitly require provenance and leakage review for auxiliary
resources (`tusoskill`, `Biological data and knowledge integration`).
