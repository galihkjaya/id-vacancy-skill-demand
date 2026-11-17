# Mapping the Geography of Skill Demand: NLP-Based Analysis of Online Job Vacancies in Indonesia

## Research Pipeline and Replication Package

### 1. Project Overview
This repository contains the complete computational workflow and empirical replication package for the research paper:

> **“Mapping the Geography of Skill Demand: NLP-Based Analysis of Online Job Vacancies in Indonesia.”**

The study investigates real-time skill demand across Indonesian provinces by extracting qualification requirements from online job advertisements using Natural Language Processing (NLP), evaluating spatial variations in skill specialization via Regional Location Quotients ($LQ$), clustering regional labor-market profiles, and examining how online vacancy data complement official Indonesian labor-market benchmarks from Badan Pusat Statistik (BPS)—specifically the Open Unemployment Rate (*Tingkat Pengangguran Terbuka*, TPT) and the Labor Force Participation Rate (*Tingkat Partisipasi Angkatan Kerja*, TPAK).

---

### 2. Directory Structure
The repository is organized for strict reproducibility:

```text
├── code_updated.ipynb                              <- Executed primary research notebook
├── data/
│   ├── data.csv                                    <- 15,007 raw online job vacancy postings
│   ├── tpt.csv                                     <- Official BPS Provincial TPT indicators
│   ├── tpak.csv                                    <- Official BPS Provincial TPAK indicators
│   ├── annotation/                                 <- Independent double-blind human annotations
│   │   ├── annoted_1.csv                           <- Annotator 1 (11,200 decisions)
│   │   ├── annoted_2.csv                           <- Annotator 2 (11,200 decisions)
│   │   ├── annotation_reference.csv                <- Reference job text for 400 vacancies
│   │   └── gold_standard.csv                       <- Formally adjudicated human gold standard
│   └── geo/
│       └── indonesia_provinces.geojson             <- 34-province boundary polygon geometries
├── outputs/
│   ├── tables/                                     <- Analytical CSV result tables
│   │   ├── table1_dataset_characteristics_data_quality.csv
│   │   ├── table2_skill_extraction_validation.csv
│   │   ├── table2_per_skill_validation.csv
│   │   ├── table_threshold_optimization_results.csv
│   │   ├── table3_top_demanded_skills.csv
│   │   ├── table4_regional_skill_demand_profiles.csv
│   │   ├── table5_regional_clusters.csv
│   │   ├── table6_robustness_checks.csv
│   │   └── table7_bps_tpt_tpak_integration.csv
│   ├── figures/                                    <- Publication-quality vector PDF & PNG charts
│   │   ├── figure1_top_demanded_skills.pdf
│   │   ├── figure2_choropleth_skill_demand.pdf
│   │   ├── figure3_regional_skill_heatmap.pdf
│   │   ├── figure4_regional_skill_clusters.pdf
│   │   ├── figure5_skill_demand_tpt_tpak.pdf
│   │   ├── figure6_threshold_optimization_evaluation.pdf
│   │   ├── figure6_threshold_optimization_evaluation.png
│   │   ├── fig_description_length.pdf
│   │   └── fig_geographic_vacancy_distribution.pdf
│   ├── validation/                                 <- Human ground-truth and method comparison
│   │   ├── gold_standard.csv
│   │   └── method_comparison_predictions.csv
│   └── processed/                                  <- Cleaned analytical datasets
│       ├── vacancies_cleaned.csv
│       ├── vacancy_skill_matrix.csv
│       ├── regional_skill_lq_matrix.csv
│       └── bps_provincial_merged.csv
├── reports/                                        <- Comprehensive research reports
│   ├── data_quality_report.md
│   ├── validation_report.md
│   ├── results_summary.md
│   └── methodology_limitations.md
└── README.md                                       <- Project documentation and instructions
```

---

### 3. Execution Instructions

#### 3.1 Environment Prerequisites
- Python 3.10+
- Key dependencies:
  ```bash
  pip install pandas numpy scipy scikit-learn matplotlib seaborn geopandas shapely sentence-transformers
  ```

#### 3.2 Executing the End-to-End Pipeline
To run the complete empirical analysis and regenerate all tables, figures, validation files, and processed datasets from scratch:

```bash
# Execute the Jupyter research notebook sequentially:
python -m nbconvert --to notebook --execute --inplace code_updated.ipynb
```

---

### 4. Methodological Highlights and Core Rules
- **Three Extraction Architectures Compared:** Rigorously evaluates Dictionary-Only Baseline (Regex), Standalone Sentence-Embedding Model (`all-MiniLM-L6-v2`), and Hybrid Architecture across 400 human-annotated vacancies ($11,200$ individual classification decisions, Cohen's kappa $\kappa = 0.8956$).
- **Anti-Leakage Protocol:** Evaluated across a strict 70/30 vacancy-level split ($N = 280$ Dev vs $N = 120$ Held-Out Test, `random_state = 42`). Zero overlapping vacancy IDs. Operating thresholds optimized on development data and permanently frozen prior to test evaluation.
- **Model Selection (Occam's Razor):** Dictionary baseline achieves superior test performance ($F_1 = 0.8485$) over standalone sentence embeddings ($F_1 = 0.4412, p < 0.0001$), because whole-document dense vectors dilute discrete tool/skill mentions and trigger semantic bleed across administrative corporate text.
- **English-Only Policy:** All table headers, values, province designations, and chart titles/labels are in academic English.
- **Vector PDF Visualizations:** All figures are saved as standalone vector PDF files; chart titles omit redundant "Figure X" prefixes.
- **Critical Temporal Disclosure:** Vacancy collection occurred predominantly in **Q3 2026**, while official BPS TPT and TPAK benchmarks refer to **February 2026**. Official indicators serve as an earlier structural baseline rather than a contemporaneous causal explanatory variable.
