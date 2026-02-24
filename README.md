# Breast Cancer Subtype Classification — ML Pipeline (METABRIC Dataset)

A machine learning project for multi-class breast cancer subtype classification using the METABRIC dataset. Built end-to-end supervised pipelines integrating gene expression (mRNA z-scores) and clinical data, with systematic model comparison and biological validation of feature importance.

🌐 [Project Website](https://acasavilca.github.io/metabric-breast-cancer-ml/)

---

## Problem Statement

Breast cancer is highly heterogeneous at the molecular level. This project frames subtype classification as a supervised multi-class problem using the PAM50 + Claudin-low subtype labels (Basal-like, HER2-enriched, Luminal A, Luminal B, Claudin-low, Normal-like). The PAM50 + claudin-low subtype labels show direct association with the ‘aggressiveness’ of the cancer and can offer better biological insights in terms of treatment options. 

---

## Dataset

**METABRIC** (Molecular Taxonomy of Breast Cancer International Consortium)
- ~2,000 breast cancer patients
- 489 gene expression features (mRNA z-scores)
- 30 clinical features (age, tumor size, ER/PR/HER2 status, survival outcomes, etc.)
- 173 binary mutation features (excluded in this study due to feature-to-sample ratio)

Source: [Kaggle — Breast Cancer Gene Expression Profiles (METABRIC)](https://www.kaggle.com/datasets/raghadalharbi/breast-cancer-gene-expression-profiles-metabric)

---

## Pipeline Overview

### 1. Exploratory Data Analysis
- Stacked histograms and boxplots for clinical features stratified by subtype
- PCA and UMAP for gene expression dimensionality reduction and class separability assessment
- Unsupervised clustering (K-Means, GMM) as a baseline to assess intrinsic data structure

### 2. Preprocessing
- Train/test split prior to all preprocessing (80/20, stratified) to prevent data leakage
- **Clinical data:** KNN imputation (numerical) + mode imputation (categorical), IQR clipping for outliers, standard scaling, correlation-based feature removal, one-hot encoding
- **Gene data:** Already z-score normalized — no additional cleaning applied

### 3. Feature Selection
- Mutual information score used to select top 50 features from ~600 combined features
- Selected features are predominantly gene expression variables with a few high-signal clinical columns (e.g., ER status, 3-gene classifier subtype)

### 4. Models Trained
All models tuned via grid/random search with cross-validation:

| Model | Weighted AUROC | Accuracy |
|---|---|---|
| Support Vector Machine (RBF kernel) | **0.9514** | 76% |
| Logistic Regression | 0.9475 | 76% |
| Random Forest | 0.95 | 77% |
| Gradient Boosted Decision Trees | 0.94 | 77% |
| Decision Tree | 0.84 | 52% |

### 5. Model Explainability
- Logistic regression: coefficient magnitude and sign analysis per class
- Tree-based models: SHAP values for feature contribution per class
- Biological validation: top features cross-referenced against clinical literature for known up/downregulation in each subtype

---

## Key Results

- SVM and Logistic Regression delivered the best overall performance, consistent with the largely **linearly separable** structure of the gene expression space (confirmed by UMAP and PCA)
- The **Basal-like** subtype was most robustly classified across all models — consistent with its distinct transcriptional profile
- The **Normal-like** subtype was the hardest to classify, frequently confused with Luminal A
- Top predictive genes for Basal-like subtype (e.g., `EGFR`, `KIT`, `CHEK1` upregulated; `BCL2`, `GATA3`, `NRIP1` downregulated) are well-supported in the clinical literature, validating that the models capture biologically meaningful patterns

---

## Repository Structure

```
.
├── Notebooks/
│   ├── 1_preprocessing_EDA.ipynb       # Run first: EDA, imputation, feature selection → generates train.csv / test.csv
│   ├── 2_logisticRegression.ipynb      # Logistic Regression with hyperparameter tuning + explainability
│   ├── 3_GBDT.ipynb                    # Gradient Boosted Decision Trees
│   ├── 4_DT.ipynb                      # Decision Tree
│   ├── 5_SVM.ipynb                     # Support Vector Machine
│   └── 6_RF.ipynb                      # Random Forest
└── docs/
    └── Final_Report.pdf                # Full project report
```

**Run order:** Start with `1_preprocessing_EDA.ipynb` to generate `train.csv` and `test.csv`. All model notebooks depend on these files.

---

## Skills Demonstrated

- End-to-end ML pipeline design with strict train/test separation
- Multiclass classification with imbalanced classes
- Feature engineering and mutual information-based feature selection
- Hyperparameter tuning (grid search + cross-validation)
- Model explainability (coefficients, SHAP, feature importance)
- Biological interpretation and literature validation of model outputs
- Python: `scikit-learn`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `shap`, `umap-learn`

---

## Authors

Luis Andrés Casavilca · Abhishek Vijeev · Celine M. Al-Noubani · FNU Naga Nishkala · Nikhil Sundaram

*Originally developed as a course project for CS7641 Machine Learning at Georgia Institute of Technology.*
