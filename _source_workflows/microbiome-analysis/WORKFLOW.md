# Microbiome Analysis

Source workflow: `microbiome-analysis`  
Parent Claude Science skill: `microbiome-functional-analysis`

## Purpose

Compare two groups from processed 16S feature data using modular community-diversity, taxonomic differential-abundance, predicted-function, and enzyme-module metabolite-potential analyses while preserving subject-level design and explicit prediction caveats.

## When to use

- Compare community diversity between two groups.
- Identify differentially abundant taxa using a three-method consensus.
- Predict functional potential from 16S data.
- Infer curated microbial metabolite-module potential.
- Produce literature-grounded scientific interpretation and an optional report.

## Inputs

- Integer feature-count table in TSV form with features as rows and samples as columns. (required)
- Representative sequences in FASTA form with headers matching feature identifiers. (optional)
- Taxonomy table containing feature identifiers and taxonomic ranks. (optional)
- Sample metadata containing sample identifiers and a two-level group column; subject identifiers and covariates are strongly recommended. (required)
- Phylogenetic tree in Newick form for Faith's phylogenetic diversity and UniFrac analyses. (optional)

## Outputs

- CSV result tables for selected analysis stages.
- PNG and SVG data figures for selected analysis stages.
- Optional PDF report with an infographic summary page.

## Workflow

1. Confirm the two groups, reference level, repeated-measures structure, covariates, selected stages, and available processed inputs; report independent-subject counts rather than sample counts.
2. When interpretation is requested, retrieve literature once before analysis and use only retrieved records for background, mechanistic claims, and citations.
3. Optionally compute alpha diversity, beta diversity, subject-aware significance tests, and robustness analyses.
4. Optionally perform prevalence filtering and three complementary differential-abundance analyses, then form a directionally concordant consensus.
5. Optionally infer enzyme-number functional potential, record placement quality, and test unstratified functional profiles.
6. Optionally score curated enzyme-number metabolite modules, keep biologically distinct routes separate, and audit single-enzyme domination.
7. Optionally assemble a report that distinguishes data-derived results, literature-supported interpretation, and confirmatory next steps.

## Decision rules

- Run only the stages the user selects; the diversity, taxonomic, functional, metabolite, literature, and reporting stages are modular.
- If subjects contribute multiple samples, use subject random effects, subject-blocked permutation, or subject-mean tests and never treat samples as independent.
- Treat a taxon as a consensus hit only when at least two differential-abundance methods flag it with concordant direction.
- Apply Benjamini–Hochberg false-discovery-rate control; the default taxonomic significance threshold is q < 0.05.
- Run functional prediction without stratified output and use unstratified enzyme-number profiles by default.
- Interpret a mean weighted NSTI below 0.15 as the stated reliability cue, while still reporting the observed value.
- Report the but and buk butyrate routes separately; show an aggregate only alongside the route-specific results.
- Keep the secondary-bile-acid module disabled by default, exclude EC:1.3.1.114, and do not claim secondary-bile-acid depletion from 16S prediction.
- If one enzyme dominates a multi-enzyme module, temper the claim and disclose the top-enzyme fraction.
- Describe functional and metabolite results as predicted genomic potential, not measured metabolites or functional validation.
- Use free enzyme-number sources for reaction verification and do not use KEGG as the default functional source because the archived workflow states it is not licensed for commercial use.

## Resource selection

Use only registry-backed resources listed in [resources.yaml](resources.yaml). A registry entry is knowledge about an adapter, not evidence that it is connected or installed.

### Primary resources

- `rr_9391adbfde83962c` — ANCOM-BC2: Use as the primary covariate-adjusted, subject-aware taxonomic differential-abundance method.
- `rr_c5a89363a3a837d8` — PICRUSt2: Use when representative sequences and feature counts are available and predicted enzyme-number functional potential is requested.
- `rr_f7cfc8e3bbdc4b3e` — ExplorEnz, IUBMB Enzyme Nomenclature, or Rhea: Use to verify or refresh enzyme-number-to-reaction mappings before trusting or extending a metabolite module.

### Secondary resources

- `rr_4e7b5a13f94f21d4` — ALDEx2: Use as a complementary centered-log-ratio differential-abundance method and for unstratified enzyme-profile testing.
- `rr_996168972fe3a7b8` — MaAsLin2: Use as a complementary covariate- and subject-aware taxonomic association model.
- `rr_33004bc5bce0b637` — scikit-bio: Use as an alternative implementation for diversity metrics, ordination, and PERMANOVA, with the chosen implementation stated.

### Fallback resources

- `rr_0d5f5dad49f3aa03` — DADA2 or Deblur: Refer upstream when the user has raw reads rather than processed feature data; raw-read processing is outside this workflow.
- `rr_2cde60f1489a6c6c` — Subject-mean Wilcoxon sensitivity analysis: Use as a repeated-measures sensitivity check for alpha diversity and as the metabolite-module test.

### Optional resources

- `rr_008fb590e2cb2de5` — ExplorEnz: Free enzyme database for enzyme-number verification.
- `rr_eb939f51701d252c` — IUBMB Enzyme Nomenclature: Free enzyme nomenclature source for enzyme-number verification.
- `rr_4065d55f4bed5014` — Rhea: Free reaction database for enzyme-number and reaction verification.
- `rr_d99f05f498142f3a` — SILVA: Example taxonomy reference whose version must remain consistent and be reported.
- `rr_69c6e4f7ac9eb675` — phyloseq: R representation and analysis of microbiome data.
- `rr_20af5f57c25ed51a` — vegan: R community-diversity and PERMANOVA analyses.
- `rr_324df3757689e64e` — ANCOM-BC2: Primary taxonomic differential-abundance analysis.
- `rr_828a3dfd69d2fb59` — ALDEx2: Complementary taxonomic and functional differential-abundance analysis.
- `rr_a728d77f2202a7c3` — MaAsLin2: Complementary multivariable taxonomic association analysis.
- `rr_32a338ae79d4e363` — lme4: Subject-random-effect modeling for repeated measures.
- `rr_8ca63df5d6737e9d` — PICRUSt2: Prediction of enzyme-number functional potential from 16S data.
- `rr_de73a6f5980c2857` — scikit-bio: Python diversity, ordination, and PERMANOVA alternative.
- `rr_c8a494b2f458c9b8` — ComplexHeatmap: Taxon-by-sample abundance heatmaps.
- `rr_d302ec2f26b9bfb7` — gseapy: Optional descriptive enzyme/pathway gene-set enrichment.

## Validation / QC

- Inspect table dimensions, integer-count status, metadata columns, group sizes, tree availability, and independent-subject counts before analysis.
- For beta diversity with repeated measures, use subject-blocked permutations and a one-sample-per-subject baseline robustness analysis.
- Report mean weighted NSTI for functional prediction and provision enough memory for the ASV count.
- Audit the top-enzyme fraction for every multi-enzyme metabolite module.
- Keep taxonomy-reference, functional-prediction, and enzyme-database versions and dates consistent and stated.
- Check every data figure for blank, clipped, or unreadable output and regenerate defective figures.
- Evidence requirement: Support background and mechanistic claims with retrieved papers; mark unsupported claims as hypotheses or omit them.
- Evidence requirement: Cite literature inline and carry the same retrieved references into the report reference list.
- Evidence requirement: Triangulate a route-level functional change with coordinated taxonomic direction when possible, while retaining the predicted-potential caveat.
- Evidence requirement: Recommend shotgun metagenomics or targeted metabolomics to confirm predicted functions or metabolites.

## Failure handling

- Treating repeated samples from one subject as independent observations.
- Interpreting a large number of correlated predicted gene-family shifts as independent functional changes.
- Allowing EC:1.3.1.114 to create a spurious secondary-bile-acid depletion result.
- Aggregating but and buk routes so an opposing route switch is hidden.
- Presenting predicted functional or metabolite potential as a measured phenotype.
- Fabricating citations or relying on memory for mechanistic claims.
- Fallback rule: If no phylogenetic tree is available, omit Faith's phylogenetic diversity and UniFrac while retaining non-phylogenetic metrics.
- Fallback rule: If only raw reads or a raw-read accession are available, stop this processed-data workflow and refer to upstream processing.
- Fallback rule: If Python is preferred for diversity analysis, use scikit-bio, state the implementation, and expect agreement with the R path.
- Fallback rule: If an arm has few independent subjects, flag the analysis as underpowered and emphasize the subject count.

## Limitations

- The default scope is a two-group comparison of already processed 16S data; raw-read processing, public-data acquisition, shotgun metagenomics, and default designs with more than two groups or continuous outcomes are excluded.
- Functional and metabolite results are predicted genomic potential and are hypothesis-generating rather than measured function or metabolite abundance.
- Rare functions such as the bai operon are predicted poorly from 16S data.
- Small independent-subject counts can leave group comparisons underpowered even when sample counts appear larger.
- A statistically significant PERMANOVA effect can still explain only a small proportion of variation.

## Important domain-specific rules

- A modular stage-selection gate that runs only the diversity, taxonomic, functional, metabolite, evidence, and reporting stages needed for the question.
- Subject-aware repeated-measures design using random effects, blocked permutations, and subject-mean sensitivity tests.
- Three-method differential-abundance consensus requiring agreement from at least two methods and concordant direction.
- Route-specific metabolite-module scoring with explicit exclusions, module gates, and single-enzyme domination auditing.
- A predicted-versus-measured evidence boundary paired with confirmatory next-step recommendations.

## Portability boundary

- The packaged stage scripts, report builder, curated module files, and internal reference documents under scripts/ and references/. — migration action: `exclude_or_capability_map`
- Biomni/Phylo environment discovery and package routing, including hpc_search_tools and the platform-managed machine workflow. — migration action: `exclude_or_capability_map`
- The platform LiteratureSearch, WebSearch, and WebFetch orchestration and its execution-trace citation file. — migration action: `exclude_or_capability_map`
- The platform pdf-report-generation, GenerateImage, and media-output-check orchestration. — migration action: `exclude_or_capability_map`
- Biomni runtime mount paths for durable results and heavy shared-workspace intermediates. — migration action: `exclude_or_capability_map`

## Provenance

See [provenance.md](provenance.md).
