# Provenance

This derived portfolio retains five literature/evidence modes and their
distinctness rules. Platform search calls, managed execution, internal paths,
branded reports, and image/report tooling were excluded.

## Source records

| source skill | normalized path | raw source path | source URL | retrieved_at |
|---|---|---|---|---|
| `literature-review` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z |
| `literature-deep-review` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace | 2026-08-14T12:03:57.399Z |
| `literature-preclinical` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_9a35bb68f5c948ca828a10aa8ea2d667?section=marketplace | 2026-08-14T12:15:09.719Z |
| `methods-landscape-review` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_685b09d4f2d509ab9198bf01121d7c7d?section=marketplace | 2026-08-14T12:16:02.995Z |
| `evidence-synthesis-meta-analysis` | `<private-local-path>` | `<private-local-path>` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z |

## Rule and mode mapping

| id | distilled rule or mode | source skill | source_section | source URL | retrieved_at |
|---|---|---|---|---|---|
| LES-1 | Use focused multi-query retrieval, DOI/title deduplication, complete records, and no invented citations. | `literature-review` | `Step 2 — Search with LiteratureSearch (multi-query + dedup)`; `Step 3 — Ground the Synthesis in Retrieved Records` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z |
| LES-2 | Use lawful open full text when enabled; otherwise label abstract evidence. | `literature-review` | `Step 3.5 — Read Open-Access Full Text (optional, only if enabled in Step 1)` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z |
| LES-3 | Require exact anchors and passing blinded entailment for deep-mode displayed claims. | `literature-deep-review` | `When to Use This Skill`; `Step 3 — Blind-review anchors and build the report` | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace | 2026-08-14T12:03:57.399Z |
| LES-4 | Make methods recommendations regime-conditional, evidence-thickness-aware, and citation-verified. | `methods-landscape-review` | `Step 3 — Screen & extract`; `Step 5 — CITATION-VERIFICATION GATE (mandatory, blocking)`; `Scientific Caveats & Integrity Rules` | https://biomni.phylo.bio/skills/skill_685b09d4f2d509ab9198bf01121d7c7d?section=marketplace | 2026-08-14T12:16:02.995Z |
| LES-5 | Pool only compatible measures with random-effects/uncertainty diagnostics. | `evidence-synthesis-meta-analysis` | `Scope`; `3. Run the meta-analysis` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z |
| LES-6 | Investigate influence; require at least 10 studies before interpreting Egger/funnel asymmetry as publication bias. | `evidence-synthesis-meta-analysis` | `4. Robustness (already produced by the script — interpret it)` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z |
| LES-P1 | Narrative review mode: scoped state-of-knowledge synthesis and evidence table, not pooling. | `literature-review` | `When to Use This Skill`; `Step 4 — Deliverables` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z |
| LES-P2 | Preclinical mode: extract model/assay/dose/endpoints and expose in-vitro/in-vivo concordance and gaps. | `literature-preclinical` | `Step 3 — Ground the Synthesis in Retrieved Records`; `Step 4 — Deliverables` | https://biomni.phylo.bio/skills/skill_9a35bb68f5c948ca828a10aa8ea2d667?section=marketplace | 2026-08-14T12:15:09.719Z |
| LES-P3 | Deep mode: canonical evidence and claim/anchor/entailment ledger. | `literature-deep-review` | `Outputs`; `Step 3 — Blind-review anchors and build the report` | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace | 2026-08-14T12:03:57.399Z |
| LES-P4 | Methods mode: comparison/topic switch, foundational plus recent queries, and no execution of compared methods. | `methods-landscape-review` | `Step 0 — Align (brief)`; `Step 1 — Plan queries (anti-recency-bias)`; `When to Use This Skill` | https://biomni.phylo.bio/skills/skill_685b09d4f2d509ab9198bf01121d7c7d?section=marketplace | 2026-08-14T12:16:02.995Z |
| LES-P5 | Meta mode: one compatible effect measure, duplicate-cohort protection, and structured risk-of-bias. | `evidence-synthesis-meta-analysis` | `1. Get the data`; `5. Risk of bias`; `Scope` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z |
| LES-M1 | Sparse/off-topic/full-text unavailable or unverifiable record/citation. | `literature-review` | `Common Issues` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z |
| LES-M2 | Incompatible meta measures, duplicate cohorts, unsupported design, or unverified effect. | `evidence-synthesis-meta-analysis` | `Scientific caveats`; `1. Get the data`; `2. VERIFY every number and citation (non-negotiable)` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z |

## Resource provenance

| registry/resource key | source skill | source_section | source URL | retrieved_at | role/status |
|---|---|---|---|---|---|
| `Europe PMC`; `Unpaywall` | `literature-review` | `Step 3.5 — Read Open-Access Full Text (optional, only if enabled in Step 1)` | https://biomni.phylo.bio/skills/skill_9b0361e33e5541bbb2b43f671dc0d5a5?section=marketplace | 2026-08-14T12:15:37.101Z | lawful OA adapters; current registry status must be checked and may be `UNKNOWN` |
| `Europe PMC` | `literature-preclinical` | `Step 3.5 — Read Open-Access Full Text (optional, only if enabled in Step 1)` | https://biomni.phylo.bio/skills/skill_9a35bb68f5c948ca828a10aa8ea2d667?section=marketplace | 2026-08-14T12:15:09.719Z | OA/repository adapter; `UNKNOWN` if not registry-recorded |
| `Europe PMC`; `EasyOCR`; `PyTorch` | `literature-deep-review` | `Step 1 — Plan, search, and freeze the corpus`; `Installation` | https://biomni.phylo.bio/skills/skill_bed70a7f98864bd48324eb9295c4fdd0?section=marketplace | 2026-08-14T12:03:57.399Z | full-text/OCR adapters, only with explicit access/license |
| `pandas`; `reportlab`; `pypdf`; `ggplot2`; `ggprism` | `methods-landscape-review` | `Environment Notes` | https://biomni.phylo.bio/skills/skill_685b09d4f2d509ab9198bf01121d7c7d?section=marketplace | 2026-08-14T12:16:02.995Z | corpus/figure/output adapters |
| `meta`; `metafor`; `ggplot2`; `ggrepel`; `patchwork` | `evidence-synthesis-meta-analysis` | `Environment resources this skill uses` | https://biomni.phylo.bio/skills/skill_c8c810255403eb169f5a580f37c919fb?section=marketplace | 2026-08-14T12:16:36.137Z | meta-analysis/diagnostic adapters |

