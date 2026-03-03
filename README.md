# CDD-ML-Hypertension-QSAR
A QSAR-based machine learning approach for bioactivity prediction against Angiotensin II receptors for hypertension drug discovery
#  CDD-ML-Hypertension-QSAR

> A machine learning-based Computational Drug Discovery (CDD) pipeline using QSAR modeling to predict bioactivity of compounds against **Angiotensin II** receptors — a key target in hypertension management.

---

##  Table of Contents

- [About the Project](#about-the-project)
- [Abstract](#abstract)
- [Project Structure](#project-structure)
- [Notebook Descriptions](#notebook-descriptions)
- [Methodology](#methodology)
- [Results & Model Performance](#results--model-performance)
- [Technologies Used](#technologies-used)
- [How to Run](#how-to-run)
- [License](#license)

---

##  About the Project

Hypertension (high blood pressure) is a leading global health challenge responsible for an estimated **9.4 million deaths annually** (WHO). This project leverages **Artificial Intelligence** and **QSAR (Quantitative Structure-Activity Relationship)** modeling to accelerate the drug discovery process by predicting compound bioactivity against **Angiotensin II** — a pivotal protein in blood pressure regulation via the Renin-Angiotensin-Aldosterone System (RAAS).

By using machine learning, researchers can efficiently screen and rank candidate drug molecules, reducing costs and minimizing the need for exhaustive in vitro or in vivo testing.

---

##  Abstract

The process of drug discovery is intricate, involving meticulous stages of research, development, and testing to ascertain the safety and efficacy of potential therapeutics. Artificial Intelligence (AI) has emerged as a transformative tool in this domain, adept at analyzing vast datasets to uncover intricate patterns and relationships unperceivable to humans.

This study introduces a **bioactivity prediction application** employing the QSAR model to forecast bioactivity against Angiotensin II, a pivotal target in hypertension management. Angiotensin II modulation holds promise for treating a spectrum of diseases, including hypertension, cardiovascular ailments, and renal disorders. Through AI-driven approaches, drug discovery researchers can efficiently identify potential drug candidates, expediting the lead optimization process while reducing costs.

**Keywords:** Drug Discovery, AI In Drug Identification, AI In Pharma, Molecular Descriptors, QSAR, Angiotensin II

---

##  Project Structure

```
CDD-ML-Hypertension-QSAR/
│
├──  notebooks/
│   ├── ML_CDD_HYPERTENSION.ipynb          # Part 1 - Data Collection
│   ├── ML_CDD_HYPERTENSION_2.ipynb        # Part 2 - EDA & Lipinski Descriptors
│   ├── ML_CDD_PART_3_HYPERTENSION.ipynb   # Part 3 - Descriptor Calculation (PaDEL)
│   ├── CDD_ML_4_HYPERTENSION.ipynb        # Part 4 - Random Forest Regression
│   ├── ML_CDD_5_HYPERTENSION.ipynb        # Part 5 - Model Comparison (LazyPredict)
│   └── ML_CDD_6_HYPERTENSION.ipynb        # Part 6 - Final Model & Visualization
│
├──  docs/
│   ├── Intro_abstract.docx                # Project introduction and abstract
│   └── ml_model.pdf                       # Published reference paper
│
├── README.md
└── LICENSE
```

---

##  Notebook Descriptions

### Part 1 — `ML_CDD_HYPERTENSION.ipynb` · Data Collection
Connects to the **ChEMBL database** using the `chembl_webresource_client` Python package to search for and retrieve bioactivity data for the target protein **Angiotensin II (CHEMBL227)**. Filters compounds by IC50 values (inhibitory concentration) and saves the raw dataset for downstream processing.

- Target: Angiotensin II (ChEMBL ID: CHEMBL227)
- Standard type: IC50
- Output: Raw bioactivity dataset

---

### Part 2 — `ML_CDD_HYPERTENSION_2.ipynb` · EDA & Lipinski Descriptors
Performs **Exploratory Data Analysis (EDA)** and calculates **Lipinski's Rule of Five** descriptors for each compound using RDKit. Lipinski descriptors evaluate drug-likeness based on:

- Molecular Weight (MW < 500 Da)
- LogP (< 5)
- Hydrogen Bond Donors (< 5)
- Hydrogen Bond Acceptors (< 10)

IC50 values are converted to **pIC50** (negative log scale) for uniform distribution across the dataset.

---

### Part 3 — `ML_CDD_PART_3_HYPERTENSION.ipynb` · Descriptor Calculation & Dataset Preparation
Uses **PaDEL-Descriptor** software to compute approximately **881 molecular fingerprint descriptors** for each compound — capturing electronic, structural, and topological properties. Low-variance features are removed to reduce dimensionality, resulting in a clean dataset ready for ML training.

- Tool: PaDEL-Descriptor
- Descriptors computed: ~881
- Output: Filtered descriptor matrix

---

### Part 4 — `CDD_ML_4_HYPERTENSION.ipynb` · Random Forest Regression Model
Builds the core **QSAR regression model** using `RandomForestRegressor` from scikit-learn. The model is trained to predict **pIC50 values** from 881 molecular fingerprint features.

- Model: Random Forest Regressor
- Input features: 881 fingerprint descriptors
- Target variable: pIC50
- Train/Test split: 80/20

---

### Part 5 — `ML_CDD_5_HYPERTENSION.ipynb` · Comparing ML Algorithms (LazyPredict)
Uses the **LazyPredict** framework to benchmark approximately **39 regression algorithms** on the prepared dataset with default parameters. Low-variance features are removed using `VarianceThreshold` before comparison. Results are ranked by R² score and RMSE to identify the best-performing model.

- Framework: LazyPredict
- Models tested: ~39
- Feature selection: VarianceThreshold (threshold = 0.8 × 0.2)

---

### Part 6 — `ML_CDD_6_HYPERTENSION.ipynb` · Final Model & Visualization
Trains the final **Random Forest model** (500 estimators) on the cleaned dataset with low-variance features removed. Evaluates performance using MSE and R² metrics, and visualizes the relationship between **experimental vs. predicted pIC50 values** using matplotlib.

- Model: RandomForestRegressor (n_estimators=500)
- Metrics: MSE, R²
- Output: Predicted vs. Experimental pIC50 scatter plot, serialized model & descriptor list

---

##  Methodology

```
ChEMBL Database (Angiotensin II - CHEMBL227)
        ↓
  Data Collection & Preprocessing
  (Remove nulls, duplicates, label active/inactive by IC50)
        ↓
  Lipinski Descriptors (EDA)
  (MW, LogP, H-Bond Donors/Acceptors, pIC50 conversion)
        ↓
  PaDEL Fingerprint Descriptors (~881 features)
        ↓
  Feature Selection (Remove low-variance features)
        ↓
  Train/Test Split (80:20)
        ↓
  QSAR Model Training (Random Forest)
        ↓
  Model Evaluation & Comparison (LazyPredict - 39 models)
        ↓
  Final Model + Visualization
```

---

##  Results & Model Performance

| Metric | Value |
|---|---|
| Model | Random Forest Regressor |
| n_estimators | 500 |
| Training R² | ~0.99 |
| Test R² | ~0.48 |
| Best LazyPredict Model | Decision Tree Regressor |
| Best LazyPredict R² | 0.92 |
| Best LazyPredict RMSE | 0.42 |

The scatter plot of predicted vs. experimental pIC50 values demonstrates the model's ability to capture bioactivity trends across the compound library. The Random Forest model successfully distinguishes **active** from **inactive** compounds against Angiotensin II receptors.

---

##  Technologies Used

| Tool / Library | Purpose |
|---|---|
| Python 3 | Core programming language |
| ChEMBL Web Resource Client | Bioactivity data retrieval |
| RDKit | Lipinski descriptor calculation |
| PaDEL-Descriptor | Molecular fingerprint computation |
| scikit-learn | Machine learning models & feature selection |
| LazyPredict | Automated model comparison |
| pandas / numpy | Data manipulation |
| matplotlib / seaborn | Data visualization |
| Google Colab / Jupyter | Notebook environment |

---

##  How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/your-username/CDD-ML-Hypertension-QSAR.git
   cd CDD-ML-Hypertension-QSAR
   ```

2. **Install dependencies**
   ```bash
   pip install chembl_webresource_client rdkit-pypi lazypredict scikit-learn pandas numpy matplotlib seaborn
   ```

3. **Run notebooks in order**
   - Start with `ML_CDD_HYPERTENSION.ipynb` (Part 1) and follow through to Part 6
   - Each notebook builds on the output of the previous one
   - Recommended environment: **Google Colab** or **Jupyter Notebook**

4. **PaDEL Descriptor** (Part 3 requires Java)
   ```bash
   # PaDEL is automatically downloaded in the notebook
   # Ensure Java is installed on your system
   java -version
   ```

---

##  License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgements

- [ChEMBL Database](https://www.ebi.ac.uk/chembl/) — Bioactivity data source
- [PaDEL-Descriptor](http://www.yapcwsoft.com/dd/padeldescriptor/) — Molecular descriptor software
- [DataProfessor](https://github.com/dataprofessor/bioinformatics) — Bioinformatics project inspiration
- WHO Global Report on Hypertension

---

> ⭐ If you find this project useful, please consider starring the repository!
