# Topological Genomics of Rice — A topological and population-genetics investigation of the Saltol locus and surrounding haplotype landscape (3K Rice Genomes)

**Short project summary (≤350 characters):**
A reproducible pipeline that applies topological data analysis, population genetics, and machine-learning-guided GWAS to identify and visualize a putative ancestral "bridge" haplotype at the Saltol locus in the 3000 Rice Genomes dataset. Includes notebooks, genotype files, and figures.

## 📌 Abstract

Standard phylogenetic models (like trees) often fail to capture reticulate evolutionary events such as hybridization and introgression. This project models the **3000 Rice Genomes (3K-RG)** dataset as a high-dimensional manifold using **Topological Data Analysis (TDA)**. We successfully identified a persistent **Topological Loop ()** connecting the *Indica* and *Japonica* subspecies. A subsequent Topological GWAS revealed that this loop is driven by the **Saltol QTL** (Salinity Tolerance) on Chromosome 1, proving that ancient hybridization events preserved critical stress-adaptation traits in specific landraces.

## 📖 Introduction

Rice (*Oryza sativa*) evolution is defined by complex domestication histories involving extensive cross-breeding. Traditional Principal Component Analysis (PCA) and hierarchical clustering force these complex relationships into linear or tree-like structures, often discarding hybrid individuals as "noise."

This project tests the hypothesis that **evolution is reticulate (network-like)**. By applying **Persistent Homology**, we detect "geometric loops" in the genomic data that represent evolutionary bridges—varieties that act as reservoirs for genetic diversity lost in modern breeding lines.

---

## Table of Contents

1. [Background & Motivation](#background--motivation)
2. [Repository Contents](#repository-contents)
3. [Results & Figures](#results--figures)
4. [Reproducible Analysis (Notebook)](#reproducible-analysis-notebook)
5. [Data and Formats](#data-and-formats)
6. [Software & Installation](#software--installation)
7. [How to run](#how-to-run)
8. [Methods (summary)](#methods-summary)
9. [Interpretation & Key Findings](#interpretation--key-findings)
10. [Citation & License](#citation--license)
11. [Contact & Contributions](#contact--contributions)

---

## Background & Motivation

Rice (Oryza sativa) comprises genetically distinct subpopulations (e.g., Indica, Japonica). The Saltol locus is a major target for salinity-tolerance breeding. Standard tree-based or PCA-only approaches can miss reticulate signals such as ancestral bridge haplotypes or introgression. This repository demonstrates how topological data analysis and LD/GWAS convergence can reveal a small set of bridge varieties that connect otherwise separated haplotype clusters.

---

## Repository Contents

Listed files (as present in the repo root):

* `Rice_Topological_Atlas.ipynb` — Primary Jupyter notebook containing the analysis, visualizations, and code used to produce figures.
* `core_v0.7.bim`, `core_v0.7.fam` — PLINK-format genotype metadata used in the analysis (trimmed/core dataset).
* `bridge_samples.csv` — List of bridge/ carrier samples identified in the study.
* `gene_candidates.csv` — Candidate gene list for the Saltol region (collected from GWAS / annotation overlap).
* `haplotype_network.png` — Haplotype network / circular visualization of the ancestral bridge.
* `pca_plot.png` — PCA scatterplot illustrating population structure of the 3K genomes subset used.
* `ld_triangle_plot.png` — Pairwise LD (r^2) triangle for the focal Saltol SNP block.
* `rice_topological_network.html` — Interactive topological network visualization (HTML export).
* `requirements.txt` — Python package list and pinned versions used to reproduce the environment.
* `README.md` — (this file)
* `LICENSE` — Project license (GPL-3.0 as present in the repo).

---

## Results & Figures

### Figure 1 | Corrected genealogy of the Saltol locus (ancestral bridge)

<p align="center">
  <img src="haplotype_network.png" width="750">
</p>

*Circular haplotype network highlighting rare bridge varieties (red) that connect otherwise separated common haplotypes (blue). This visualization captures reticulate ancestry not resolved by tree-based models.*

---

### Figure 2 | Population structure of the 3000 Rice Genomes

<p align="center">
  <img src="pca_plot.png" width="750">
</p>

*Principal Component Analysis (PC1 vs PC2) computed from ~20,000 SNPs showing strong Indica–Japonica separation and intermediate samples corresponding to Saltol bridge carriers.*

---

### Figure 3 | LD structure of the Saltol haplotype block

<p align="center">
  <img src="ld_triangle_plot.png" width="750">
</p>

*Pairwise linkage disequilibrium (r²) heatmap across candidate SNPs defining the Saltol locus, revealing a compact haplotype block underlying the detected bridge signal.*

---

## Reproducible Analysis (Notebook)

The primary workflow is implemented in `Rice_Topological_Atlas.ipynb`. It contains the following sections:

1. Data loading and basic QC (PLINK/BED parsing)
2. SNP filtering, MAF and pruning for PCA
3. PCA computation and plotting (Matplotlib)
4. Haplotype block extraction around Saltol and LD computation
5. Haplotype network construction and visualization (networkx / igraph-style layouts)
6. Topological data analysis / Mapper pipeline (if present) and interactive network export
7. Integration with GWAS or AI-based candidate prioritization (summary and overlap tables)

Open the notebook in JupyterLab or NBViewer to inspect the code and re-run cells. Large genotype files are expected to be pre-downloaded in PLINK format.

---

## Data and Formats

* Genotype metadata: PLINK `.bim` / `.fam` files (SNP coordinates and sample labels included).
* `bridge_samples.csv` and `gene_candidates.csv` are CSV tables with sample IDs and annotation fields respectively.
* The analysis expects either a PLINK binary (`.bed/.bim/.fam`) trio or pre-extracted VCF-derived genotype matrices.

> **Data privacy note:** This repository contains derived files used for method demonstration. If you add raw genotype/VCF files, ensure they do not violate data sharing agreements for the 3K Rice Genomes.
---

## Software & Installation

A `requirements.txt` is included. Suggested installation steps (Linux/macOS):

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Typical packages used in the notebook include:

* numpy, pandas, scikit-learn
* matplotlib, seaborn
* networkx or igraph
* plinkio or pyplink (if reading PLINK files directly)
* scikit-tda / kepler-mapper (optional, for mapping/tda)

If you use conda, create an environment with the same package versions for reproducibility.

---

## How to run

1. Clone the repository:

```bash
git clone https://github.com/sayandeep520/Topological-Genomics-Rice.git
cd Topological-Genomics-Rice
```

2. Install dependencies (see above) and start Jupyter:

```bash
jupyter lab
# or
jupyter notebook
```

3. Open `Rice_Topological_Atlas.ipynb` and run cells sequentially. If large genotype files are not present, adjust the data paths at the top of the notebook to point to your local PLINK/VCF files.

4. Export figures by re-running the plotting cells; use `plt.savefig('myfig.png', dpi=300)` for publication quality.

---

## Methods (brief)

* **PCA:** Standardized genotype matrix → PCA (scikit-learn) to visualize structure and annotate subpopulations.
* **LD:** Pairwise r² computed across focal SNPs to identify compact haplotype blocks.
* **Haplotype network:** Haplotype clustering (distance-based) and circular layout to visualize bridge carriers linking clusters.
* **Topological analysis:** Mapper / TDA used to detect non-tree-like relationships and candidate bridge haplotypes not obvious from trees or PCA alone.
* **GWAS/AI convergence:** Overlap of significant GWAS hits and ML/AI-prioritized SNPs to recall candidate genes.

Full method detail and parameter choices are documented inline in the notebook.

---

## Interpretation & Key Findings

* The combination of TDA and classical LD/GWAS analysis pinpoints a localized haplotype block near Saltol with elevated LD and a small set of varieties acting as a likely ancestral bridge.
* These "bridge" varieties can explain reticulate patterns and provide targets for breeders interested in introgressing salinity tolerance.
* Results are hypothesis-generating and should be validated via targeted sequencing and experimental phenotyping.

---

## Citation & License

If you use this work, please cite the repository and the primary analysis notebook. This repository is licensed under the **GPL-3.0** license (see `LICENSE`).

Suggested citation format :

```
Sayan Deep Bera. Topological-Genomics-Rice. GitHub repository, https://github.com/sayandeep520/Topological-Genomics-Rice
```

---

## Contact & Contributions

* Maintainer: sayandeep520 (GitHub)
* Issues and pull requests are welcome. For major changes, please open an issue first to propose the change.

