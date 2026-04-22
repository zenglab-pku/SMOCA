# HGSOC spatial multi-omics analysis

This repository contains the analysis code used for the study of primary and metastatic high-grade serous ovarian cancer (HGSOC) using paired spatial transcriptomics, spatial metabolomics, spatial proteomics, and single-cell multi-omic profiling.

# Overview

The goal of this repository is to provide an organized and reproducible framework for the computational analyses described in the manuscript, including:

* integration of spatial transcriptomics and spatial metabolomics
* pathology-guided patch-level representation learning
* spatial niche analysis and tissue architecture inference
* single-cell RNA-seq, TCR/BCR and cell-state analysis
* spatial immune organization analysis
* humoral immunity and B-cell repertoire analysis
* spatial metabolic reprogramming analysis
* figure generation and summary statistics

This repository is organized by analysis module so that each major result section in the manuscript corresponds to a defined code directory.

⸻

# Repository structure

HGSOC-spatial-multiomics/
├── README.md
├── LICENSE
├── environment/
│   ├── conda_env.yml
│   ├── requirements.txt
│   └── renv.lock
├── data/
│   ├── raw/                  # raw or external input files (not tracked if large/private)
│   ├── processed/            # processed matrices, objects, annotations
│   ├── metadata/             # sample sheets, clinical annotations, file manifests
│   └── public/               # derived public-release tables if applicable
├── notebooks/
│   ├── exploratory/
│   └── figure_panels/
├── scripts/
│   ├── 00_preprocessing/
│   ├── 01_spatial_patch_integration/
│   ├── 02_spatial_niche_analysis/
│   ├── 03_scrna_analysis/
│   ├── 04_cellcell_communication/
│   ├── 05_immune_repertoire/
│   ├── 06_hotspot_modules/
│   ├── 07_spatial_metabolomics/
│   ├── 08_cross_modal_integration/
│   └── 09_figures/
├── workflow/
│   ├── Snakefile            # or main.nf / run_all.sh
│   ├── config.yaml
│   └── rules/
├── results/
│   ├── qc/
│   ├── intermediate/
│   ├── final_tables/
│   └── figures/
└── docs/
    ├── methods_notes.md
    ├── sample_overview.md
    └── reproducibility.md

⸻

Analysis modules

1. Preprocessing

Directory: scripts/00_preprocessing/

This module contains code for preprocessing all primary data modalities.

Included tasks

* sample metadata harmonization
* Visium HD count matrix loading and filtering
* DESI-MSI spectral preprocessing and feature alignment
* CODEX preprocessing and marker normalization
* scRNA-seq quality control, filtering and integration
* TCR/BCR contig preprocessing and clonotype assignment

Example scripts

* 01_visium_hd_qc.py or .R
* 02_msi_preprocess.py
* 03_codex_preprocess.R
* 04_scrna_qc_integration.py
* 05_tcr_bcr_preprocess.py

Inputs

* raw count matrices or feature tables
* sample-level metadata
* spatial coordinates
* histology images

Outputs

* filtered Seurat/AnnData objects
* processed metabolite matrices
* harmonized sample annotation tables
* QC summary plots and metrics

⸻

2. Pathology-guided patch-level integration

Directory: scripts/01_spatial_patch_integration/

This module implements the patch-based integration framework linking spatial transcriptomic and spatial metabolomic tissue architecture.

Included tasks

* histology patch extraction from spatial transcriptomic and registered metabolomic sections
* feature encoding using CONCH or other pathology foundation models
* batch correction across modalities using Harmony
* latent embedding construction
* unsupervised clustering of integrated patch embeddings
* benchmarking clustering strategies and parameter choices

Example scripts

* 01_extract_patches.py
* 02_encode_patches_conch.py
* 03_harmony_integration.R
* 04_patch_clustering.py
* 05_cluster_benchmarking.py

Outputs

* patch-level embeddings
* integrated low-dimensional coordinates
* patch cluster labels
* modality mixing and benchmarking plots

⸻

3. Spatial niche analysis

Directory: scripts/02_spatial_niche_analysis/

This module resolves higher-order spatial neighborhoods and tissue organization.

Included tasks

* cell-bin annotation using reference models such as Scimilarity
* neighborhood graph construction
* CellCharter-based spatial clustering
* mapping of TCGA-like proliferative, differentiated, mesenchymal and immunoreactive programs
* niche composition quantification
* niche proximity analysis between primary and metastatic lesions
* niche-to-patch correspondence analysis

Example scripts

* 01_cellbin_annotation.py
* 02_run_cellcharter.py
* 03_niche_program_scoring.R
* 04_niche_proximity_analysis.py
* 05_patch_niche_mapping.R

Outputs

* annotated spatial cell bins
* niche labels and niche composition matrices
* pathway enrichment by niche
* niche proximity statistics and heatmaps

⸻

4. Single-cell RNA-seq analysis

Directory: scripts/03_scrna_analysis/

This module contains single-cell analyses used to define cellular states across epithelial, stromal and immune compartments.

Included tasks

* dimensionality reduction and clustering
* lineage annotation
* subclustering of epithelial, fibroblast, myeloid and lymphoid compartments
* differential expression analysis
* CNV inference in epithelial cells
* trajectory analysis for fibroblast states
* cross-platform label transfer from scRNA-seq to spatial data

Example scripts

* 01_global_clustering.R
* 02_epithelial_subclustering.py
* 03_fibroblast_subclustering.py
* 04_myeloid_subclustering.py
* 05_tcell_subclustering.py
* 06_cnv_inference.py
* 07_celltypist_transfer.py

Outputs

* annotated cell-state objects
* DE gene tables
* CNV score tables
* lineage-specific plots
* transferred labels in spatial data

⸻

5. Cell–cell communication analysis

Directory: scripts/04_cellcell_communication/

This module analyzes intercellular communication and local interaction programs.

Included tasks

* ligand–receptor inference between major cell types
* outgoing and incoming signaling program analysis
* comparison of communication programs between primary and metastatic lesions
* epithelial–myeloid and stromal interaction summaries

Example scripts

* 01_run_cellchat.R
* 02_compare_primary_metastatic.py
* 03_signal_program_summary.R

Outputs

* interaction score matrices
* signaling pathway summaries
* communication network visualizations

⸻

6. Immune repertoire and humoral immunity analysis

Directory: scripts/05_immune_repertoire/

This module focuses on TCR/BCR clonotypes, B-cell/plasma-cell states and humoral remodeling.

Included tasks

* clonotype calling and expansion analysis
* T-cell clonal expansion statistics
* BCR isotype distribution analysis
* clonotype sharing across lesions
* co-occurrence or class-switching network analysis
* plasma-cell differential expression and pathway enrichment

Example scripts

* 01_tcr_clonotype_analysis.py
* 02_bcr_isotype_analysis.py
* 03_bcr_sharing_network.R
* 04_plasma_deg_analysis.py

Outputs

* clonotype abundance tables
* repertoire diversity metrics
* isotype usage summaries
* clonotype-sharing plots

⸻

7. Local spatial gene module analysis

Directory: scripts/06_hotspot_modules/

This module identifies spatially autocorrelated gene programs and local tissue modules.

Included tasks

* Hotspot analysis on spatial transcriptomic data
* local module detection
* module pathway enrichment
* module matching across samples
* module-to-structure alignment with CODEX and niche labels

Example scripts

* 01_run_hotspot.py
* 02_module_annotation.R
* 03_cross_sample_module_matching.py

Outputs

* spatial gene modules
* enrichment results
* module activity maps

⸻

8. Spatial metabolomics analysis

Directory: scripts/07_spatial_metabolomics/

This module contains analyses of DESI-MSI data under positive and negative ion modes.

Included tasks

* metabolite peak curation and annotation
* ion-mode-specific clustering
* metabolic niche definition
* differential metabolite analysis between primary and metastatic lesions
* pathway-level and subsystem-level metabolic analysis
* spatial visualization of representative metabolites

Example scripts

* 01_peak_annotation.py
* 02_metabolic_niche_clustering.py
* 03_primary_vs_metastatic_diff.py
* 04_pathway_analysis.R
* 05_spatial_metabolite_plotting.py

Outputs

* metabolite annotation tables
* spatial metabolic niche labels
* differential metabolite tables
* pathway enrichment summaries

⸻

9. Cross-modal integration and correlation analysis

Directory: scripts/08_cross_modal_integration/

This module links transcriptomic, metabolomic, spatial and single-cell results.

Included tasks

* transcriptome–metabolome pathway integration
* patch-to-niche and niche-to-cell-state correspondence
* gene-level and pathway-level correlation analyses
* Mantel tests and cross-platform DEG overlap
* integration with external resources such as CRISPR screening datasets

Example scripts

* 01_multiomic_gsea.py
* 02_mantel_test.R
* 03_cross_platform_deg_overlap.py
* 04_icraft_overlap_analysis.py

Outputs

* integrated pathway networks
* correspondence matrices
* cross-modal overlap statistics
* candidate target summary tables

⸻

10. Figure generation

Directory: scripts/09_figures/

This module reproduces manuscript figures and supplementary figures.

Included tasks

* panel-specific plotting
* statistical annotations
* export of publication-ready vector graphics
* generation of source-data tables

Example scripts

* fig1_patch_integration.R
* fig2_spatial_niches.R
* fig3_scrna_epithelial_stromal.R
* fig4_falc_inflammation.R
* fig5_humoral_immunity.R
* fig6_metabolomics.R
* extended_data_figures/

Outputs

* PDF/SVG/PNG figures
* source-data files

⸻

Reproducibility

Software

Please list the exact software versions used in the analysis. For example:

* Python >= 3.10
* R >= 4.3
* Scanpy
* Seurat
* Harmony
* CellCharter
* CellTypist
* Hotspot
* inferCNV or CopyKAT
* scirpy / immunarch
* tidyverse
* ggplot2

All required packages should be recorded in:

* environment/conda_env.yml
* environment/requirements.txt
* environment/renv.lock

Running the workflow

A minimal example:

# create environment
conda env create -f environment/conda_env.yml
conda activate hgsoc_multiomics
# run the full workflow
snakemake --snakefile workflow/Snakefile --cores 8

Or, for module-specific analysis:

python scripts/01_spatial_patch_integration/01_extract_patches.py
python scripts/01_spatial_patch_integration/02_encode_patches_conch.py
Rscript scripts/01_spatial_patch_integration/03_harmony_integration.R
python scripts/01_spatial_patch_integration/04_patch_clustering.py

⸻

Data availability

Because patient-derived multi-omic datasets may contain protected information, raw data may not be fully distributed in this repository.

We recommend organizing data access as follows:

* processed matrices and non-identifiable derived tables: deposited in a public repository when permitted
* raw sequencing or imaging data: available through controlled-access repositories or upon reasonable request
* sample-level clinical metadata: de-identified and shared in compliance with institutional approval and patient consent

Large intermediate files should not be committed directly to GitHub. Instead:

* store them externally
* provide download instructions or accession numbers
* include file manifests in `data







