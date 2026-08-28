# Clinicaltrials Landscape

Source workflow: `clinicaltrials-landscape`  
Parent Claude Science skill: `clinical-trial-analysis`

## Purpose

Map the registered clinical-trial landscape for a disease by mechanism, phase, sponsor, geography, and study design using ClinicalTrials.gov data.

## When to use

- Map the competitive trial landscape across therapeutic mechanisms for a disease.
- Track a mechanism class or sponsor portfolio and summarize phase distribution.

## Inputs

- Disease or condition search terms. (required)
- Disease configuration defining a mechanism taxonomy. (optional)
- Mechanism, sponsor, recruitment-status, or phase filters. (optional)

## Outputs

- A trial-level table with mechanism, phase, sponsor, geography, design, endpoints, and regulatory fields.
- Mechanism-by-phase and sponsor summary tables.
- Landscape and supplementary figures in PNG and SVG formats.
- A markdown or PDF landscape report and a reusable analysis object.

## Workflow

1. Clarify whether to use live ClinicalTrials.gov data or the cached demonstration dataset, then define the disease conditions and optional focus.
2. Query ClinicalTrials.gov API v2 for the selected conditions and trial statuses.
3. Classify interventions into mechanism categories using the selected disease taxonomy and compile the trial-level frame.
4. Aggregate mechanism, phase, sponsor, geography, and study-design summaries.
5. Generate the landscape figures and export all result tables and analysis parameters.

## Decision rules

- Use recruiting, active-not-recruiting, enrolling-by-invitation, and not-yet-recruiting studies as the default active-status set.
- Use a curated disease configuration when one exists; otherwise use generic intervention-type classification.
- If no trials are returned, broaden condition, status, or phase filters before concluding that the landscape is empty.
- Query the ClinicalTrials.gov API rather than scraping its HTML pages.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_3ef7c2ef85f468b4` — ClinicalTrials.gov API v2 live data: A current disease landscape is required.

### Secondary resources

- `rr_e4dda73b1ca0e4c5` — A disease-specific mechanism configuration: A curated taxonomy is available for the requested disease.

### Fallback resources

- `rr_83d4fbb421a0ed9e` — Cached inflammatory-bowel-disease demonstration snapshot: A quick demonstration is requested or live access is unavailable.

### Optional resources

- `rr_173e1c1b4d0bb143` — ClinicalTrials.gov: Source of registered trial records, sponsors, phases, conditions, interventions, and study metadata.
- `rr_178baeb270b10c13` — pandas: Trial-table processing and export.
- `rr_f48c55f08412b6c6` — requests: ClinicalTrials.gov API requests.
- `rr_78609fcc8c7914c6` — numpy: Numerical summaries.
- `rr_8ed55c1c99c2e88c` — plotnine: Landscape visualizations.
- `rr_665edbde6fc3cd1c` — matplotlib: Visualization support.
- `rr_ae5d2965f1c73447` — reportlab: Packaged PDF fallback.

## Validation / QC

- Record the number of trials retrieved and verify that trial compilation and all exports complete.
- Treat vague intervention descriptions as potentially unclassified rather than assigning an unsupported mechanism.
- Normalize sponsor subsidiaries consistently before sponsor-level aggregation.
- Evidence requirement: Base all landscape counts on registered ClinicalTrials.gov records returned for the documented conditions and filters.
- Evidence requirement: Retain the query parameters with the analysis object so downstream summaries can be reproduced.

## Failure handling

- ClinicalTrials.gov is unreachable or rate-limits repeated requests.
- Overly restrictive filters produce zero trials.
- A missing or outdated disease taxonomy yields many unclassified mechanisms.
- Fallback rule: Retry a transient connection failure after a delay.
- Fallback rule: Broaden filters when the initial query returns no trials.
- Fallback rule: Retain PNG output when SVG export is unavailable.

## Limitations

- This workflow is not for detailed single-trial protocol analysis or efficacy and safety comparison.
- The landscape covers registered trials and omits unregistered preclinical and pre-IND pipeline programs.
- Mechanism classification is derived from intervention names and descriptions and may remain unclassified when descriptions are vague.

## Important domain-specific rules

- Condition and status query definition for a trial registry.
- Disease-specific or generic intervention-mechanism classification.
- Mechanism-by-phase and sponsor aggregation with explicit query provenance.

## Portability boundary

- Mandatory execution through packaged Biomni scripts and their exact verification messages. — migration action: `exclude_or_capability_map`
- Biomni pdf-report-generation orchestration and platform-managed report packaging. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
