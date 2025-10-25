# 💳 Loan Default Prediction Analysis

## 📘 Project Overview

Analyzing loan data to predict **default risk** using **machine learning** models.

---

## 📊 Data

- **Dataset:** [Insert Data Source Link Here if publicly available]  
- **Shape:** 10,000 rows × 8 columns  
- **Missing Values:** None detected ✅

---

## 🔍 Exploratory Data Analysis (EDA)

### 🎯 Target Variable (`default`) Distribution

| Status | Count | Percentage |
| :------ | -----:| ----------:|
| Non-default (0) | 8,149 | 81.5% |
| Default (1) | 1,851 | 18.5% |

> The dataset is **imbalanced** toward non-default cases.

---

### 📈 Numerical Feature Summary (after outlier capping)

| Feature | Mean | Std. Dev |
| :------- | ----:| --------:|
| `loan_amt_outstanding` | ~4153 | ~1400 |
| `total_debt_outstanding` | ~8448 | ~5883 |
| `income` | ~70042 | ~19917 |
| `years_employed` | ~4.55 | ~1.57 |
| `fico_score` | ~637.60 | ~60.30 |

---

### ⚠️ Potential Outliers (Z-score > 3)

| Feature | Outlier Count |
| :------- | --------------:|
| `loan_amt_outstanding` | 41 |
| `total_debt_outstanding` | 143 |
| `income` | 30 |
| `years_employed` | 11 |
| `fico_score` | 26 |

---

### 🔁 Multicollinearity (VIF Scores)

| Feature | VIF Score | Interpretation |
| :------- | ---------:| :--------------|
| `credit_lines_outstanding` | 10.95 | High |
| `loan_amt_outstanding` | 35.09 | High |
| `total_debt_outstanding` | 21.94 | High |
| `income` | 59.93 | Very High |
| `years_employed` | 11.90 | High |
| `fico_score` | 22.40 | High |

> Significant **multicollinearity** observed among numerical features — suggesting potential for **dimensionality reduction (PCA)**.

---

## 🧱 Feature Engineering

Created new features to enhance model performance:

- `debt_to_income_ratio` = `total_debt_outstanding / income`
- `loan_amount_to_income_ratio` = `loan_amt_outstanding / income`
- `fico_score_bin`: Categorical bins ('Poor', 'Fair', 'Good', 'Excellent') from `fico_score`

---

## ⚙️ Preprocessing

- Outliers capped using **IQR method**
- `fico_score_bin` **one-hot encoded**
- Numerical features scaled using **StandardScaler**
- Dropped **`customer_id`** column (non-predictive)
- Split data into **75% Train / 25% Test**, **stratified** by `default`

---

## 🤖 Model Building and Evaluation

Machine learning models trained and evaluated:

- **Logistic Regression**
- **Random Forest Classifier**
- **Gradient Boosting (LightGBM)**
- **Support Vector Machine (SVM)**

---

### 📊 Performance Metrics (Test Set)

| Model | Accuracy | Precision (Default) | Recall (Default) | F1-score (Default) | ROC-AUC |
| :----- | :-------- | :------------------ | :--------------- | :----------------- | :------ |
| Logistic Regression | 0.9956 | 0.9768 | 1.0000 | 0.9883 | 0.999986 |
| Random Forest | 0.9944 | 0.9956 | 0.9741 | 0.9847 | 0.999734 |
| Gradient Boosting (LightGBM) | 0.9972 | 0.9893 | 0.9957 | 0.9925 | 0.999962 |
| SVM | 0.9972 | 0.9851 | 1.0000 | 0.9925 | 0.999999 |

> All models achieved **exceptionally high performance**.  
> **SVM** and **LightGBM** slightly outperformed others based on **ROC-AUC** and **F1-score**.

---

## 🏁 Conclusion

**Key Predictive Features:**
- Credit lines outstanding  
- Total debt outstanding  
- Years employed  
- Income  
- FICO score  
- Debt-to-income ratio  
- Loan amount-to-income ratio  

**Findings:**
- Models achieved **very high accuracy and robustness**
- Indicates strong relationships between features and loan default behavior

---

## 🔮 Next Steps

- Perform **k-fold cross-validation** for model validation  
- Conduct **hyperparameter tuning** for optimization  
- Carry out **error analysis** for misclassifications  
- Explore **PCA or feature selection** to reduce multicollinearity  
- Evaluate **feature importance / SHAP values** for interpretability  

---

## 🧠 PCA Insight

> **Perform PCA *after encoding* categorical variables.**

PCA requires numeric data — categorical features should be encoded first.  
Then scale numerical data before applying PCA.

Example pipeline:

```python
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer
from sklearn.decomposition import PCA
from sklearn.pipeline import Pipeline

numeric_features = ['income', 'loan_amt_outstanding', 'years_employed']
categorical_features = ['fico_score_bin']

preprocessor = ColumnTransformer([
    ('num', StandardScaler(), numeric_features),
    ('cat', OneHotEncoder(handle_unknown='ignore'), categorical_features)
])

pca = PCA(n_components=5)

pipeline = Pipeline([
    ('preprocessor', preprocessor),
    ('pca', pca)
])
