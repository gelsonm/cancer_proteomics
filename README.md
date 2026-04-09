# Cancer Proteomics EDA + ML Classifier

Exploratory analysis of real CPTAC lung adenocarcinoma (LUAD) proteomics data, followed by a trained XGBoost classifier with SHAP-based interpretability.

## Project Overview

| Item | Detail |
|------|--------|
| Dataset | CPTAC LUAD — Clinical Proteomic Tumor Analysis Consortium |
| Task | Binary classification: Tumor vs. Normal |
| Model | XGBoost |
| Explainability | SHAP TreeExplainer |
| Data access | `cptac` Python library (no manual download) |

## Key Outputs

| Output | File |
|--------|------|
| Protein abundance clustermap (top 30) | `protein_abundance_heatmap.png` |
| ROC-AUC curve + confusion matrix | `roc_auc_curve.png` |
| SHAP beeswarm plot | `shap_beeswarm.png` |
| SHAP global importance bar chart | `shap_importance_bar.png` |

## Setup

### Option A — pip (Windows, no C++ Build Tools required)

`ncls` and `sorted_nearest` (transitive deps of `cptac`) have no pre-compiled wheels on PyPI for Windows.  
The `--prefer-binary` flag skips source builds and avoids the C++ compiler requirement:

```powershell
# Run from this directory
.\setup_windows.ps1
```

Or manually:

```powershell
pip install --prefer-binary numpy pandas scipy matplotlib seaborn scikit-learn xgboost shap jupyter ipykernel
pip install --prefer-binary cptac
```

### Option B — conda (recommended for reproducibility)

```bash
conda env create -f environment.yml
conda activate cancer-proteomics
jupyter notebook cancer_proteomics_classifier.ipynb
```

### Option C — install Microsoft C++ Build Tools

Download from https://visualstudio.microsoft.com/visual-cpp-build-tools/ then:

```bash
pip install -r requirements.txt
```

## Pipeline

1. **Load data** — `cptac.download('luad')` fetches ~110 tumor + matched normal samples with 8,000+ quantified proteins
2. **Preprocess** — drop proteins >20% missing, median impute
3. **EDA** — seaborn clustermap of top 30 most variable proteins, Z-score normalised
4. **Train** — XGBoost with stratified 5-fold cross-validation
5. **Evaluate** — ROC-AUC curve and confusion matrix on held-out test set
6. **Explain** — SHAP beeswarm and bar chart pinpoint top tumor-associated proteins

## References

- CPTAC library: https://cptac.readthedocs.io
- SHAP: https://github.com/shap/shap
- Original dataset: Edwards et al., *Nature*, 2015
