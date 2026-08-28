# Evidence policy

## Purpose

Every material statement must be assigned one claim class before it is shown. A
claim class describes what the statement is based on; it does not imply that
the statement is correct. Keep observations, calculations, predictions, and
decisions separate.

| Claim class | Permitted basis | Required wording and minimum record |
|---|---|---|
| `database_fact` | A named database or catalog record returned for a stated query or accession | State the database, record identifier, query/retrieval time, version when available, and source URL. Treat the value as a database observation, not as independent experimental confirmation. |
| `paper_evidence` | A specific publication, preprint, protocol, figure, table, or supplementary item | State the citation identifier (prefer DOI, PMID, or another stable identifier), exact location, source URL, and the claim-to-source relationship. |
| `model_prediction` | A named model, fitted analysis, or predictor applied to an identified input | State model/checkpoint and software versions, input and output artifact identifiers or hashes, calibration/uncertainty and applicability status when available, and the fact that the value is predicted. |
| `inference` | An analyst's synthesis of one or more recorded facts, papers, or predictions | Link the supporting claim IDs, state assumptions and uncertainty, and label the result as an inference rather than a measured fact. |
| `recommendation` | A decision rule applied to the available evidence and constraints | State the objective, decision criteria, alternatives considered, assumptions, and evidence IDs. A recommendation is not evidence of efficacy or truth. |

Do not silently change a claim from one class to another. For example, a
predicted score is not a database fact, a database record is not paper evidence,
and an inference is not a recommendation until a decision criterion is stated.

## Evidence ladder

Use the weakest claim that the recorded evidence supports:

1. Record the direct observation or source anchor.
2. Record the calculation or model output separately, including its input and
   limitations.
3. State the inference with explicit supporting claim IDs.
4. State any recommendation separately, with the decision context and viable
   alternatives.

Do not use a high-level summary as a substitute for the underlying identifier,
source URL, version, or artifact. If a required identifier or version is not
available, write `not_provided` or `unknown`; never invent one.

## Derived operating rules

- A literature claim needs a resolvable source anchor and an entailment check;
  a citation list alone is insufficient.
- A database or dataset catalog record should retain its accession, title or
  summary, repository, access status, and the evidence used for relevance.
- A model result must expose the evaluation regime, uncertainty or domain status
  when supported, and the distinction between model selection and assessment.
- A synthesis must preserve compatible effect definitions, risk-of-bias
  information, heterogeneity, and influence diagnostics; do not turn a
  diagnostic into proof of causation.
- Recommendations must carry the evidence identifiers and limitations that
  would let another analyst disagree or choose an alternative.

### Provenance of derived rules

| Normalized source skill | Source section(s) used | Source URL |
|---|---|---|
| `literature-deep-review` | Step 2 — Acquire, parse, retrieve, and adjudicate; Step 3 — Blind-review anchors and build the report; Outputs | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace |
| `evidence-synthesis-meta-analysis` | 2. VERIFY every number and citation (non-negotiable); 3. Run the meta-analysis; 5. Risk of bias | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace |
| `adme-ml-modeling` | Required outputs; Operating rules; Scientific caveats | https://biomni.phylo.bio/skills/skill_9fd3e0632ea54e148757bb2469292ab8?section=marketplace |
| `omics-dataset-retrieval` | Outputs; Step 14 — Relevance audit; Step 15 — Assemble master catalog | https://biomni.phylo.bio/skills/skill_b12e61063fd8475896b26186626f2426?section=marketplace |

## Communication boundary

Use explicit labels in tables, figures, filenames, and prose. A compact
pattern is `[{claim_class}; {claim_id}]`. Put the full provenance record in the
associated artifact or claim ledger so the displayed statement remains
readable without losing auditability.

## Related policies

- [Provenance policy](provenance_policy.md) defines the full claim and artifact
  record.
- [Validation policy](validation_policy.md) defines the gates for evidence
  quality and output disclosure.
