# ECEN766Project


## Attention-Enhanced DeepSurv for Interpretable Gene-Expression Survival Modeling 

This repository contains the code, models, and experimental configurations for extending the DeepSurv framework with a feature-level attention mechanism. This work aims to bridge the gap between predictive performance and biological interpretability in deep survival modeling for precision oncology.

### Abstract
Survival models are fundamental tools in clinical medicine for quantifying how a patient's clinical and molecular characteristics influence time-to-event outcomes. While deep neural extensions of the Cox proportional hazards model, such as DeepSurv, improve predictive flexibility by capturing nonlinear interactions in high-dimensional gene-expression data, they often function as black boxes. 

We propose an attention-enhanced DeepSurv framework that introduces a feature-level attention mechanism to improve transparency. The model jointly optimizes attention weights and Cox partial likelihood to provide adaptive gene-importance scores while preserving predictive performance.

### Repository Contents
This repository will include the following components:
* **Full DeepSurv PyTorch implementation:** Modular neural network design with configurable depth and width.
* **Data preprocessing pipeline:** Scripts for standardizing features, removing near-zero variance genes, and constructing survival objects.
* **Training and evaluation scripts:** Support for GPU-accelerated training, dropout regularization, and batch normalization.
* **Documentation for reproducibility:** Detailed guides to replicate experiments.

### Dataset
This study utilizes the **METABRIC** (Molecular Taxonomy of Breast Cancer International Consortium) dataset, a large-scale breast cancer cohort.
* **Features:** Illumina microarray-based mRNA expression profiles for approximately 20,000 genes per patient.
* **Outcomes:** Overall survival time (in months) and vital status.

**Note:** Due to dataset size constraints, large-scale data (e.g., METABRIC gene-expression data) will be hosted via Zenodo, and the corresponding link will be provided. 

### Methodology
We evaluate three survival modeling approaches:
1.  **Linear Baseline (Cox PH):** A standard Cox proportional hazards model where gene-expression features are directly used as covariates.
2.  **Nonlinear Baseline (DeepSurv):** A multi-layer perceptron (MLP) replacing the linear predictor, optimized using the negative Cox partial likelihood.
3.  **Proposed Model (Attention-DeepSurv):** Incorporates a feature-level attention module prior to the survival network to assign adaptive importance weights to gene-expression features. 

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

### Team
* **Irene Tsai** - Data and Statistical Modeling Lead 
* **Ananya Patel** - Deep Learning and Attention Model Lead 
* **Sang Hoon Chung** - Evaluation and Experimental Analysis Lead 

*(Texas A&M University - ECEN 766 Final Project)* 
