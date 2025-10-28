
# ⚙️ FICO Score Quantization and Default Prediction

## 📘 1. Introduction and Objective

This repository implements a machine-learning pipeline to predict mortgage default probability using **FICO score quantization**. The workflow converts continuous FICO scores into interpretable risk buckets, optimizes bucket boundaries using a log-likelihood approach, and trains a probabilistic classifier to estimate default risk.

**Objectives**

* Create a quantization strategy to convert FICO scores into categorical risk ratings.
* Optimize bucket boundaries to maximize separation in observed default rates.
* Build and evaluate a default prediction model using quantized scores.
* Provide an interactive notebook (Colab) for real-time prediction and visualization.

---

## 🧮 2. Data Loading and Exploration

**Steps performed**

1. Loaded dataset into a `pandas.DataFrame`.
2. Checked for null values and inspected data types.
3. Explored FICO score distribution and default frequency.
4. Visualized relationship between FICO and default — confirming lower FICO correlates with higher default probability.

**Quick checks**

* `df.info()` and `df.describe()` to inspect structure and ranges.
* `df.isnull().sum()` to identify missing data.
* `sns.histplot(df['fico_score'])` and `sns.barplot` for category default rates.

---

## 🔢 3. FICO Score Quantization

Quantization transforms continuous FICO scores into discrete buckets (ratings) to improve model interpretability and performance.

### 🧠 Quantization Strategy

We used a **log-likelihood optimization** to search for bucket boundaries that best separate default behaviour across buckets. The approach:

* For each candidate bucket, compute the observed default probability (p_i = k_i/n_i) where (k_i) is defaults and (n_i) is borrowers.
* Compute total log-likelihood across buckets and maximize it (implemented as minimizing negative log-likelihood).
* Optimization performed with SLSQP.

### 📏 Optimal Boundaries (found)

```
[408, 587, 623, 653, 688, 850]
```

These define five risk buckets and their inclusive ranges.

### 🗺️ Rating Map

| Rating    | FICO Range | Risk Level |
| --------- | ---------- | ---------- |
| Excellent | 688 – 850  | Lowest     |
| Very Good | 653 – 688  | Low        |
| Good      | 623 – 653  | Moderate   |
| Fair      | 587 – 623  | High       |
| Poor      | 408 – 587  | Highest    |

A new column `fico_score_quantized` was added to the dataset with these categorical ratings.

---

## 📊 4. Visualization (recommended)

* Histogram of original FICO distribution with vertical lines showing bucket boundaries.
* Bar chart of observed default rate per rating (shows monotonic decline as FICO increases).

* ## 📊 FICO Score Quantization Visuals

### 1️⃣ FICO Distribution with Bucket Boundaries
![FICO Distribution with Buckets](https://github.com/Rishabh1108ch/JP_Morgan_Quantitative_Research/blob/main/Task4%3A-FICO_score_quantization-%26-default_prediction/FICO%20distribution%20with%20vertical%20lines%20showing%20bucket%20boundaries..png)

### 2️⃣ Default Rate per Rating
![Default Rate per Rating](https://github.com/Rishabh1108ch/JP_Morgan_Quantitative_Research/blob/main/Task4%3A-FICO_score_quantization-%26-default_prediction/default%20rate%20per%20rating.png)


*Note: charts are available in the Colab notebook under `/notebooks`.*

---

## 🧰 5. Data Preparation for Modeling

**Preprocessing steps**

1. Selected features including `fico_score_quantized` plus relevant numerical/categorical fields.
2. One-hot encoded categorical variables (including the quantized rating) using `pd.get_dummies()` or `OneHotEncoder`.
3. Scaled numerical features with `StandardScaler`.
4. Train/test split: 80% train / 20% test (stratified by target if class imbalance present).

**Example pipeline (sketch)**

```python
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer

num_cols = ['loan_amount', 'income', 'term_months']
cat_cols = ['fico_score_quantized', 'loan_purpose']

preprocessor = ColumnTransformer([
    ('num', StandardScaler(), num_cols),
    ('cat', OneHotEncoder(drop='first'), cat_cols)
])

pipeline = Pipeline([('pre', preprocessor)])
```

---

## 🤖 6. Model Building and Evaluation

**Model chosen**: Logistic Regression (interpretable, outputs probabilities).

**Training details**

* Fitted on preprocessed training data.
* Increased `max_iter` as needed for convergence.

**Evaluation metrics (example results)**

| Metric    | Value  |
| --------- | ------ |
| AUC-ROC   | 0.9999 |
| Accuracy  | 0.9955 |
| Precision | 0.9971 |
| Recall    | 0.9770 |
| F1-Score  | 0.9869 |

> These metrics demonstrate very strong discrimination on the provided dataset. Be cautious of potential data leakage, class imbalance, or overly optimistic evaluation. Recommended checks: cross-validation, calibration plots, and holdout validation on a temporally separated sample.

**Plots**

* Confusion matrix (train vs test)
* ROC curve (plot probabilities)
* Calibration curve (reliability diagram)

---

## 📤 7. Example Predictions (sample table)

| Customer | FICO | Rating    | Predicted Default Probability | Actual Default (0/1) | Actual Default (Yes/No) |
| -------- | ---- | --------- | ----------------------------: | -------------------: | ----------------------: |
| A        | 450  | Poor      |                          0.86 |                    1 |                     Yes |
| B        | 720  | Excellent |                          0.12 |                    0 |                      No |
| C        | 780  | Excellent |                          0.02 |                    0 |                      No |

The notebook includes a bar plot comparing predicted probabilities to actual defaults.

---

## 🌳 8. Comparison with Decision Tree Quantization

| Aspect | Log-Likelihood Quantization  | Decision Tree Quantization        |
| ------ | ---------------------------- | --------------------------------- |
| Basis  | Statistical likelihood       | Impurity reduction (Gini/Entropy) |
| Output | Smooth, interpretable ranges | Dynamic, possibly uneven buckets  |
| Pros   | Stable and explainable       | Captures nonlinear splits         |
| Cons   | Requires optimization        | Can overfit, less interpretable   |

**Conclusion**: Log-likelihood optimization gives interpretable buckets that generalize well; decision trees can be used for exploratory analysis or when complex interactions dominate.

---

## 🏁 9. Conclusion & Recommendations

This project:

* Built a quantization framework to convert FICO scores into 5 risk categories.
* Used log-likelihood optimization to identify boundaries: `[408, 587, 623, 653, 688, 850]`.
* Trained a Logistic Regression model that performs excellently on the current dataset.


**Core quantization snippet**

```python
# boundaries found via optimization
boundaries = [408, 587, 623, 653, 688, 850]
labels = ['Poor','Fair','Good','Very Good','Excellent']

df['fico_score_quantized'] = pd.cut(df['fico_score'], bins=boundaries, labels=labels, include_lowest=True, right=False)
```



## 👤 Author Information

**Rishabh Chandrakar**
Data Analytics | Power BI | Python | SQL | Supply Chain Analytics
📍 Raipur, Chhattisgarh, India
🔗 LinkedIn: `www.linkedin.com/in/rishabh-chandrakar`
📞 8963976273


