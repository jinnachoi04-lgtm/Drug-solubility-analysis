# 🧪 Predictive Modeling of Small Molecule Solubility ($\text{LogS}$)

## 📌 Executive Summary

Aqueous solubility ($\text{LogS}$) is a critical pharmacokinetic parameter in drug discovery, directly influencing oral bioavailability, absorption, and overall efficacy of small molecule candidates.

This project establishes an end-to-end Machine Learning pipeline utilizing the **Delaney (ESOL) dataset**. By evolving from a single-descriptor baseline linear regression model to a multi-feature **Random Forest Regressor**, the model's explanatory power improved from an **$R^2$ score of 0.41 to 0.90**, reducing prediction error ($\text{RMSE}$) by over 50%.

---

## 📊 Model Progression & Performance Metrics

| Model Stage | Features / Descriptors Used | Algorithm | $R^2$ Score | $\text{RMSE}$ | Status |
| --- | --- | --- | --- | --- | --- |
| **Baseline** | `Molecular Weight` | Simple Linear Regression | **0.41** | ~1.32 | Completed |
| **Intermediate** | Multi-Descriptor (`LogP`, `MW`, `Rotatable Bonds`, `Aromatic Proportion`) | Multiple Linear Regression | **0.77** | ~0.88 | Completed |
| **Final** | All Numeric Descriptors | **Random Forest Regressor** | **0.90** | **~0.60** | **Production** |

---

## 🛠️ Technical Challenges & Problem Solving

### 1. OS-Dependent Environment Alignment

* **Challenge:** Environment paths diverged between OS shell configurations (`zsh` on macOS vs. Windows PowerShell), causing `ModuleNotFoundError` when the VS Code Jupyter extension targeted global system Python instead of the isolated virtual environment (`.venv`).
* **Solution:** Standardized dependency injection within Jupyter execution contexts via inline package installation (`!pip install pandas scikit-learn matplotlib seaborn`) and manually bound the VS Code Jupyter kernel to the active `.venv` environment.

### 2. Ingestion & Server Rate Limiting (HTTP 429)

* **Challenge:** Direct web downloads from external patent/chemical databases encountered HTTP Error 429 (Too Many Requests) due to automated IP throttling during batch queries.
* **Solution:** Redesigned the ingestion pipeline to stream validated raw CSV datasets directly from open-source GitHub repositories via HTTPS requests, ensuring 100% reproducible and unblocked execution.

### 3. Feature Matrix Preprocessing & Type Safety

* **Challenge:** Passing raw molecular tabular data into `scikit-learn` algorithms triggered `ValueError` exceptions caused by string-encoded SMILES identifiers (e.g., `'Cc1cc(C)cc(O)c1'`) and exact header mismatches.
* **Solution:** Implemented dynamic type isolation via Pandas (`df.select_dtypes(include=[np.number])`), automatically isolating continuous quantitative descriptors while stripping out string-based identifier columns prior to model fitting.

### 4. Non-Linear Feature Optimization

* **Challenge:** The single-variable linear model (`Molecular Weight` vs. `Solubility`) yielded an $R^2$ score of **0.41**, failing to account for complex molecular interactions such as hydrophobicity and structural rigidity.
* **Solution:** Transitioned to an ensemble decision tree framework (**Random Forest**) trained on multi-dimensional molecular descriptors (`MolLogP`, `AromaticProportion`, `NumRotatableBonds`, etc.), capturing non-linear interactions and boosting explained variance to **90%**.

---

## 🔮 Future Improvements & Strategic Roadmap

To advance this project into an enterprise-grade computational chemistry pipeline, the following upgrades are planned:

### 1. Feature Engineering via RDKit (SMILES Parsing)

* **Objective:** Move beyond pre-calculated tabular features by parsing raw **SMILES** (Simplified Molecular Input Line Entry System) strings directly using the `rdkit` library.
* **Impact:** Compute advanced molecular fingerprints (e.g., **Morgan Fingerprints / ECFP4**) and topological descriptors (e.g., Topological Polar Surface Area — TPSA, Hydrogen Bond Donors/Acceptors) to capture fine-grained 3D structural characteristics.

### 2. Cross-Validation & Hyperparameter Tuning

* **Objective:** Implement **$k$-Fold Cross-Validation** ($k=5$ or $10$) alongside **GridSearchCV** or **RandomizedSearchCV**.
* **Impact:** Optimize Random Forest hyperparameters (`n_estimators`, `max_depth`, `min_samples_split`) to prevent overfitting and guarantee robust performance on external drug discovery datasets.

### 3. Advanced Ensemble & Deep Learning Models

* **Objective:** Benchmark the Random Forest regressor against **Gradient Boosting algorithms** (`XGBoost`, `LightGBM`) and **Graph Neural Networks (GNNs)** using `PyTorch Geometric`.
* **Impact:** GNNs operate directly on molecular graph structures (atoms as nodes, bonds as edges), potentially surpassing traditional tree-based models on complex polycyclic structures.

### 4. Web Application Deployment (Streamlit + Docker)

* **Objective:** Wrap the trained Random Forest model into an interactive **Streamlit** micro-app containerized with **Docker**.
* **Impact:** Allow researchers to input a custom SMILES string or draw a chemical structure in a web interface to get real-time solubility predictions ($\text{LogS}$) and confidence intervals.

---

## 💻 How to Run

1. **Clone the repository:**
```bash
git clone https://github.com/your-username/solubility-prediction.git
cd solubility-prediction

```


2. **Open in VS Code:**
```bash
code .

```


3. **Execute Notebook:**
Open `analysis.ipynb` and execute all cells sequentially.

---

## 🛠️ Tech Stack

* **Language:** Python 3.9+
* **Data Manipulation:** Pandas, NumPy
* **Machine Learning:** Scikit-Learn
* **Data Visualization:** Matplotlib, Seaborn

---