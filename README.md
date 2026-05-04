# ECEN766Project


## Attention-Enhanced DeepSurv for Interpretable Gene-Expression Survival Modeling 

This repository contains the code, models, and experimental configurations for extending the DeepSurv framework with a feature-level attention mechanism. This work aims to bridge the gap between predictive performance and biological interpretability in deep survival modeling for precision oncology.


### 📝 Abstract
Survival analysis is fundamental in precision oncology for modeling time-to-event outcomes using clinical and molecular data. While deep learning extensions of the Cox model, such as DeepSurv, improve predictive flexibility by capturing nonlinear interactions in high-dimensional gene-expression data, they operate as uninterpretable "black boxes." 

In this project, we developed an **attention-enhanced DeepSurv framework** to bridge the gap between predictive performance and biological interpretability. We explored various attention mechanisms to provide adaptive, patient-specific gene-importance scores (feature weighting) while predicting survival risk. Our models were evaluated on the METABRIC breast cancer cohort against L2-regularized Cox PH and baseline DeepSurv models.

### Repository Contents
This repository will include the following components:
* **Full DeepSurv PyTorch implementation:** Modular neural network design with configurable depth and width.
* **Data preprocessing pipeline:** Scripts for standardizing features, removing near-zero variance genes, and constructing survival objects.
* **Training and evaluation scripts:** Support for GPU-accelerated training, dropout regularization, and batch normalization.
* **Documentation for reproducibility:** Detailed guides to replicate experiments.


### 🗂️ Dataset: METABRIC
The model is trained and evaluated using the **Molecular Taxonomy of Breast Cancer International Consortium (METABRIC)** dataset.
* **Patients:** 1,980 (after matching and processing)
* **Features:** Top 3,000 most variable gene-expression features (from an original ~20,385)
* **Outcomes:** Overall survival time (months) and censoring status (Death=1, Censored=0)
* **Splitting:** Stratified 70% / 15% / 15% (Train / Validation / Test)

**Note: Due to dataset size and access constraints, the raw METABRIC gene-expression data is not directly included in this repository. Please refer to the documentation for data access instructions.**

### 🧠 Methodology & Architectures

Our pipeline evaluates standard baselines and proposes three attention-based architectures to solve the "black box" problem:

1. **Cox PH Baseline:** Traditional linear model with L2 regularization to handle high-dimensional ($p \gg n$) data.
2. **DeepSurv Baseline:** Standard neural network optimized via the Cox partial log-likelihood.
3. **Feature-Wise Self-Attention DeepSurv:** Attempted to use standard query-key-value self-attention. However, computing a $3000 \times 3000$ attention matrix caused Out-of-Memory (OOM) failures, proving unscalable for high-dimensional tabular omics data. 
4. **Gated Attention DeepSurv:** Resolves the OOM issue by learning a single, patient-specific continuous gate value per gene ($g_i \in [0,1]$). The input is reweighted ($x' = g \odot x$) before being passed to the DeepSurv MLP.
5. **Residual Gated Attention DeepSurv:** To combat the validation instability and overfitting observed in the Gated Attention model, this architecture introduces a residual skip connection. It fuses a gated branch with a residual branch, allowing the original gene signals to pass forward and stabilizing the training process.


### Evaluation & Interpretability
* **Predictive Performance:** Evaluated using the concordance index (C-index).
* **Risk Stratification:** Patients are stratified into high-risk and low-risk groups, evaluated using Kaplan-Meier survival curves and log-rank tests.
* **Interpretability Benchmarks:** Validated via statistical consistency (Spearman correlation with univariate Cox hazard ratios) and Pathway-Level Validation (Gene Set Enrichment Analysis).

### Preliminary Results
Initial results evaluating the re-implemented models on the METABRIC cohort using the C-index:

| Model | Split | C-index |
| :--- | :--- | :--- |
| DeepSurv (PyTorch) | Train | 0.9082 |
| DeepSurv (PyTorch) | Validation | 0.6303 |
| DeepSurv (PyTorch) | Test | 0.6543 |
| Cox PH | Train | 0.8721 |
| Cox PH | Validation | 0.6333 |
| Cox PH | Test | 0.6500 |
### 🔬 Biological Interpretability
To ensure our attention weights provided meaningful biological insights, we conducted:
* **Attention vs. Cox Ranking:** Compared model-learned gene rankings to traditional Cox hazard ratios.
* **Perturbation Validation:** Shuffled top candidate genes to measure the resulting drop in C-index (predictive influence).
* **Pathway Enrichment Analysis (ORA):** Evaluated top attention-ranked genes against KEGG, GO, Reactome, and MSigDB Hallmark databases to identify biologically meaningful pathway enrichment.

### ⚙️ Repository Contents
* Data preprocessing and survival outcome construction scripts
* PyTorch implementations of Cox PH, DeepSurv, Gated Attention, and Residual Gated Attention models
* Training, evaluation, and interpretability analysis notebooks
  
### Team
* **Irene Tsai** - Data and Statistical Modeling Lead 
* **Ananya Patel** - Deep Learning and Attention Model Lead 
* **Sang Hoon Chung** - Evaluation and Experimental Analysis Lead 

*(Texas A&M University - ECEN 766 Final Project)* 
### Large data upload
HiSeqV2 file and data_mma_illlumina_microarray file is in Zenodo
*https://doi.org/10.5281/zenodo.19359791
*https://doi.org/10.5281/zenodo.19866910 (MATABRIC pickle file)
