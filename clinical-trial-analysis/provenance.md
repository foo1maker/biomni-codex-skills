# Provenance

This derived router preserves three source modes: design simulation, registry
landscape description, and right-censored survival analysis.

## Archive inputs

| Source skill | Normalized record | Raw visible source | Raw metadata |
|---|---|---|---|
| clinical-trial-design-simulation | `../../02_normalized/clinical-trial-design-simulation.yaml` | `../../00_raw/clinical-trial-design-simulation/source.md` | `../../00_raw/clinical-trial-design-simulation/metadata.json` |
| clinicaltrials-landscape | `../../02_normalized/clinicaltrials-landscape.yaml` | `../../00_raw/clinicaltrials-landscape/source.md` | `../../00_raw/clinicaltrials-landscape/metadata.json` |
| survival-analysis-clinical | `../../02_normalized/survival-analysis-clinical.yaml` | `../../00_raw/survival-analysis-clinical/source.md` | `../../00_raw/survival-analysis-clinical/metadata.json` |

## Derived-rule mappings

| Derived mode/rule | Source skill | Visible source section | Source URL | Retrieved at |
|---|---|---|---|---|
| Use one configuration as the source of truth for design assumptions and outputs | clinical-trial-design-simulation | `Scope`; `Workflow`; `Analysis population & internal consistency (define once, use everywhere)` | https://biomni.phylo.bio/skills/skill_70b5de4b518cd101e23530cd587523c4?section=marketplace | 2026-08-14T12:18:24.989Z |
| Gate operating-characteristic reporting on global/least-favorable FWER and analytic benchmark checks | clinical-trial-design-simulation | `Workflow`; `Statistical caveats (read before quoting results)` | https://biomni.phylo.bio/skills/skill_70b5de4b518cd101e23530cd587523c4?section=marketplace | 2026-08-14T12:18:24.989Z |
| Include null scenarios, sensitivity, quick/thorough distinction, and Monte Carlo error | clinical-trial-design-simulation | `Step-by-step (what each step is for, and why it matters)`; `Outputs (saved under an output dir; copy user-facing files to results)` | https://biomni.phylo.bio/skills/skill_70b5de4b518cd101e23530cd587523c4?section=marketplace | 2026-08-14T12:18:24.989Z |
| Query the registry API with explicit conditions/statuses and retain query scope | clinicaltrials-landscape | `Standard Workflow`; `Inputs`; `Outputs` | https://biomni.phylo.bio/skills/skill_546a8868863342c093eb2570dcd538f4?section=marketplace | 2026-08-14T12:18:49.404Z |
| Broaden filters only after no-result diagnosis and classify vague mechanisms as unclassified | clinicaltrials-landscape | `Common Issues`; `Interpretation Guidelines`; `⚠️ CRITICAL — DO NOT:` | https://biomni.phylo.bio/skills/skill_546a8868863342c093eb2570dcd538f4?section=marketplace | 2026-08-14T12:18:49.404Z |
| Normalize sponsor names and retain retrieval counts/parameters | clinicaltrials-landscape | `Standard Workflow`; `Interpretation Guidelines` | https://biomni.phylo.bio/skills/skill_546a8868863342c093eb2570dcd538f4?section=marketplace | 2026-08-14T12:18:49.404Z |
| Enumerate every survival endpoint and export canonical metrics before reporting | survival-analysis-clinical | `Standard Workflow`; `Outputs`; `When to Use This Skill` | https://biomni.phylo.bio/skills/skill_827c499e76084524a2e098d80383a3a4?section=marketplace | 2026-08-14T12:21:39.333Z |
| Pair KM/Cox estimates with PH diagnostics, missingness, events-per-variable, and optimism correction | survival-analysis-clinical | `Standard Workflow`; `Scientific Caveats`; `Agent Summary Guidelines` | https://biomni.phylo.bio/skills/skill_827c499e76084524a2e098d80383a3a4?section=marketplace | 2026-08-14T12:21:39.333Z |
| Use landmark rates when median is not reached and disclose complete-case/in-sample limitations | survival-analysis-clinical | `Agent Summary Guidelines`; `Common Issues`; `Scientific Caveats` | https://biomni.phylo.bio/skills/skill_827c499e76084524a2e098d80383a3a4?section=marketplace | 2026-08-14T12:21:39.333Z |

## Resource and exclusion notes

The catalog authority is `../../03_resource_registry/resource_registry.yaml`
(registry version `2026-08-14`, 1,161 final unique records). ClinicalTrials.gov
API v2, survival software, and example cohorts are replaceable adapters. Access,
version, and license state are run-specific. Protocol approval, efficacy/safety
claims, and clinical advice were deliberately excluded.
