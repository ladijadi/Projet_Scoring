# Credit Risk Scoring - Default Prediction

This project builds a **credit scoring model** to estimate the probability of default (PD) for credit card clients.

The analysis is based on the **Default of Credit Card Clients dataset (Taiwan, 2005)** and follows a complete risk modeling workflow:

- Exploratory Data Analysis
- Feature Engineering
- Statistical Analysis
- Feature Selection
- Logistic Regression Modeling
- Model Evaluation and Interpretation

The objective is to develop an **interpretable predictive model** that can support **credit decision processes**.

---

# Project Objectives

- Understand the profile of clients with a higher risk of default
- Identify the most relevant variables explaining default behavior
- Build an interpretable **credit scoring model**
- Estimate the **probability of default (PD)** for each client
- Evaluate model performance using standard **credit risk metrics**

---

# Dataset

The dataset used in this project is the **Default of Credit Card Clients Dataset (Taiwan, 2005)**.

It contains **30,000 observations** and **25 variables** describing client characteristics and credit behavior.

Target variable:

- `1` = Default
- `0` = No default

---

# Main Variables

### Demographic Variables

- `SEX`
- `EDUCATION`
- `MARRIAGE`
- `AGE`

### Credit Information

- `LIMIT_BAL` (credit limit)

### Payment History

- `PAY_0` to `PAY_6` (repayment status over the last months)

### Billing Amounts

- `BILL_AMT1` to `BILL_AMT6`

### Payment Amounts

- `PAY_AMT1` to `PAY_AMT6`

---

# Project Structure

credit-risk-scoring-logistic-regression

│

├── 1_Construire_Un_Score.ipynb
│ Exploratory data analysis and dataset understanding
│

├── 2_Feature_Engineering_Selction_Variables.ipynb
│ Feature engineering, statistical analysis and feature selection
│

├── 3_Regression_Logistique.ipynb
│ Logistic regression modeling and model evaluation
│

├── 4_Machine_Learning_comparaison.ipynb
│ Random Forrest modeling and model evaluation
│

└── UCI_Credit_Card.csv
Dataset

---

# Methodology

## 1. Data Understanding and Exploration

Initial exploration of the dataset includes:

- Data structure inspection
- Descriptive statistics
- Distribution analysis of numerical variables
- Frequency analysis of categorical variables

Visualizations include:

- Histograms
- Boxplots
- QQ-plots

---

## 2. Feature Engineering

Several preprocessing steps were applied:

- Grouping rare categories in `EDUCATION` and `MARRIAGE`
- Data type conversions for categorical variables
- Exploratory analysis of payment behavior variables

This step improves model interpretability and data quality.

---

## 3. Statistical Analysis

Different statistical techniques were used to understand relationships between variables.

### Numerical Variables

Correlation analysis using:

- **Pearson**
- **Spearman**
- **Kendall**

Scatterplots were used to visualize relationships between numerical variables.

---

### Categorical Variables

Association between categorical variables was analyzed using:

- **Chi-Square test**
- **Cramér’s V**
- **Tschuprow’s T**

Heatmaps were generated to visualize the strength of associations.

---

### Categorical vs Numerical Variables

Relationships between categorical and numerical variables were analyzed using:

- **T-test** (for binary categorical variables)
- **Kruskal-Wallis test** (for variables with more than two categories)

Boxplots were used for visualization.

---

## 4. Feature Selection

Feature selection combined multiple approaches:

- Statistical significance tests
- Association measures (Cramér’s V / Tschuprow’s T)
- Random Forest feature importance
- Domain interpretation (credit risk logic)

Random Forest was used to identify the most relevant predictors.

Key variables identified include:

- Payment behavior (`PAY_0`, `PAY_2`, `PAY_3`)
- Credit limit (`LIMIT_BAL`)
- Billing and payment amounts

---

## 5. Modeling

A **logistic regression model** was used to estimate the probability of default.

Steps included:

- Train/test split (stratified)
- One-hot encoding of categorical variables
- Model training using maximum likelihood estimation

Logistic regression was chosen because it is widely used in **credit risk modeling due to its interpretability**.

---

# Model Performance

The model was evaluated using several metrics commonly used in **credit scoring**.

| Metric | Value |
|------|------|
| AUC (ROC) | ~0.71 |
| Gini Coefficient | ~0.43 |
| KS Statistic | ~0.37 |

These results indicate a **reasonable discriminatory power**, which is consistent with typical performance levels for this dataset.

---

# Model Interpretation

Analysis of model coefficients and feature importance reveals that:

- **Payment behavior variables (`PAY_0`, `PAY_2`, `PAY_3`) are the strongest predictors of default**
- Higher repayment amounts reduce the probability of default
- Demographic variables have a weaker predictive effect

This aligns with **credit risk modeling practices**, where **payment history is usually the most informative predictor of default risk**.

---

# Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- Statsmodels
- SciPy
- Matplotlib
- Seaborn
- Jupyter Notebook

---

# Limitations

Several limitations should be noted:

Some feature engineering steps remain exploratory

Results may vary depending on train/test split

Hyperparameters of machine learning models may influence feature importance

Real-world credit scoring models typically require additional validation and regulatory constraints