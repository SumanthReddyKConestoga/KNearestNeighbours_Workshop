```
# K-Nearest Neighbours (KNN) Workshop — Winter Olympics Medals (WinterSD.csv)

## 0) Business Framing (Why this matters)
This repository treats historical Winter Olympics medal records as a **supervised multi-class classification** problem.  
Our objective is to **predict medal type (Gold / Silver / Bronze)** using event context such as **Year, Sport, Discipline, Country, Gender, and Event**.

**Why we’re doing this:**  
- To practice an end-to-end ML workflow (EDA → preprocessing → modeling → evaluation) in a way that is **reproducible, explainable, and defensible**.
- To build a **strong baseline** model (KNN) that we can later compare against more advanced models.
- To demonstrate that **“similar historical contexts”** can be leveraged to make predictions—while staying academically honest.

> Academic safety line: **Correlation ≠ causation**. This model learns statistical patterns; it does not prove causal drivers of medals.

---

## 1) Goal & Problem Statement
### Goal
Predict the target label:

- **Target:** `Medal` ∈ {Gold, Silver, Bronze}

### Inputs (Features)
Typical event-context features used in the notebook:
- `Year`
- `Sport`
- `Discipline`
- `Country`
- `Gender`
- `Event`

> The notebook validates the exact column names using `df.columns` and `df.head()`.

---

## 2) Why KNN?
KNN (K-Nearest Neighbours) is a **distance-based** classifier:
- It predicts a class for a new record by looking at the **k most similar historical records**.
- It is excellent for workshops because it makes one thing very clear:

✅ **Your preprocessing *is* your model** (for distance-based methods).  
If encoding/scaling is wrong, “similarity” becomes meaningless.

### When KNN is a strong baseline
- patterns are **local** (similar contexts → similar outcomes),
- features are well-encoded,
- numeric features are scaled appropriately,
- dataset is not dominated by missing values.

---

## 3) EDA (Exploratory Data Analysis)
EDA is the “professional due diligence” step—before training any model, we must understand the data.

### What we check and why
1. **Dataset shape & schema**
   - `df.shape`, `df.info()`, `df.head()`
   - Confirms we have the expected fields and correct dtypes.

2. **Missing values**
   - `df.isna().sum()`
   - KNN cannot handle missing values directly → must be handled upstream.

3. **Target distribution (Medal balance)**
   - `df['Medal'].value_counts()`
   - Prevents misleading results from class imbalance (e.g., always predicting the majority medal).

4. **Category cardinality**
   - unique counts for `Country`, `Event`, `Discipline`, etc.
   - High-cardinality features can explode dimensions after one-hot encoding and affect distance quality.

5. **Duplicates / leakage checks**
   - `df.duplicated().sum()`
   - Ensures no trivial signals that inflate accuracy artificially.

---

## 4) Preprocessing (Why we do what we do)
Because this dataset is categorical-heavy, preprocessing is not optional—it’s mission critical.

### 4.1 Encoding
We use **One-Hot Encoding** for categorical variables:
- Converts categories into binary columns (0/1)
- Enables distance calculations in numeric space

**Why:**  
KNN measures distance; raw strings are not usable.

### 4.2 Scaling (if numeric features exist)
If numeric columns like `Year` are used alongside one-hot features:
- Use `StandardScaler` (or equivalent) for numeric columns.

**Why:**  
Without scaling, a numeric feature with larger magnitude can dominate distance, distorting “similarity.”

---

## 5) Modeling Approach
### 5.1 Split Strategy
We split into train/test sets (e.g., 80/20) with a fixed random seed.

**Why:**  
- Honest evaluation on unseen data  
- Reproducible results (same split every run)

### 5.2 Choosing `k` (hyperparameter)
We test multiple values of `k` (e.g., 3, 5, 7, 9, 11…).

**Why:**  
- Small `k` → can overfit noise (high variance)  
- Large `k` → can oversmooth (high bias)

---

## 6) Evaluation (How we prove it worked)
We evaluate with:
- **Accuracy** (high-level performance)
- **Confusion matrix** (where the model confuses classes)
- **Classification report** (Precision / Recall / F1 per class)

**Why not only accuracy:**  
Accuracy can hide class-specific failures; F1/Recall/Precision provide deeper truth.

---

## 7) Talking Points (Presentation-ready narrative)
Use these in class or when writing your submission reflection.

### Talking Point A — Business Value
- We frame medal prediction as a classification task to show how historical context can inform outcomes.
- This provides a baseline that is transparent and easy to explain.

### Talking Point B — KNN as a baseline model
- KNN is interpretable: *“show me the neighbors.”*
- It highlights the importance of preprocessing more than most models.

### Talking Point C — EDA as governance
- EDA is how we confirm data quality and avoid misleading modeling.
- It’s the difference between “running code” and “doing data science.”

### Talking Point D — Encoding is everything for distance models
- Good encoding = meaningful similarity  
- Bad encoding = random distance = random predictions

### Talking Point E — Tuning `k` is disciplined engineering
- We select `k` through evaluation—not guesswork.
- This makes the result defensible.

### Talking Point F — Interpretability via confusion matrix
- Confusion matrix shows which medals are being mixed up.
- It guides feature improvement ideas (e.g., better event context).

### Talking Point G — Academic integrity line
- The model captures associations, not causality.
- The output is a predictive pattern, not a causal explanation.

---

## 8) How to Run This Project (Step-by-step)
### 8.1 Clone the repository
```bash
git clone https://github.com/SumanthReddyKConestoga/KNearestNeighbours_Workshop.git
cd KNearestNeighbours_Workshop
```
```
### 8.2 Create and activate a virtual environment

**Windows PowerShell:**

```bash
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 8.3 Install dependencies

```bash
pip install -r requirements.txt
```

### 8.4 Run the notebook

Option A (VS Code):

1. Open the folder in VS Code
2. Select interpreter: `.venv`
3. Open `knearest.ipynb`
4. Click **Run All**

Option B (Jupyter):

```bash
jupyter notebook
```

Then open `knearest.ipynb` and run all cells.

---

## 9) Reproducibility Notes 

* Dependencies are captured in `requirements.txt`.
* The dataset file `WinterSD.csv` is included in the repo for repeatable execution.
* Random seeds are set (where applicable) to stabilize results.
* The workflow follows a consistent pipeline:
  **EDA → Preprocessing → Train/Test Split → KNN Training → Evaluation**

---

## 10) Git Workflow (All commands in one place)

### First-time push (if starting from your local folder)

```bash
git init
git add .
git commit -m "Initial commit - KNN workshop notebook + dataset + requirements"
git branch -M main
git remote add origin https://github.com/SumanthReddyKConestoga/KNearestNeighbours_Workshop.git
git push -u origin main
```

### If push is rejected (non-fast-forward)

```bash
git pull --rebase origin main
git push -u origin main
```

### Regular update cycle

```bash
git status
git add .
git commit -m "Update notebook: improved EDA + evaluation"
git push
```

---

## 11) Common Troubleshooting

### “Jupyter Variables” / notebook UI missing in VS Code

* Ensure VS Code extensions are installed:

  * Python
  * Jupyter
* Restart VS Code and re-select `.venv` interpreter.

### Matplotlib backend errors (e.g., “widget not recognised”)

* Use a standard backend and
```
