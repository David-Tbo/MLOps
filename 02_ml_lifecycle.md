# The Machine Learning Lifecycle

This notebook introduces the end-to-end machine learning lifecycle using a simplified credit scoring example.

## Learning objectives

By the end of this notebook, you will be able to:

- Describe the main stages of the ML lifecycle.
- Build a simple analytical dataset.
- Train and evaluate a baseline credit scoring model.
- Understand how lifecycle stages connect to MLOps controls.
- Identify where reproducibility, monitoring, and governance should be implemented.

## Setup

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score, accuracy_score, confusion_matrix, classification_report

RANDOM_SEED = 42
np.random.seed(RANDOM_SEED)

```

## Business Understanding

The business objective is to support credit application decisions.

A bank wants to estimate whether a credit applicant is likely to default.

The target variable is:

```text
default = 1 if the customer defaults
default = 0 otherwise

```

Typical business goals:

* Reduce default losses.
* Maintain acceptable approval rates.
* Improve decision speed.
* Ensure explainability and auditability.

## Simulated Data Collection

In a real project, data would come from multiple systems:

* Customer information.
* Loan application data.
* Credit bureau data.
* Historical repayment data.

Here we create a simplified synthetic dataset.

```python
n = 3000

data = pd.DataFrame({
    "age": np.random.randint(21, 70, n),
    "income": np.random.normal(50000, 15000, n).clip(15000, 150000),
    "loan_amount": np.random.normal(15000, 6000, n).clip(1000, 50000),
    "employment_years": np.random.exponential(5, n).clip(0, 35),
    "credit_utilization": np.random.beta(2, 5, n),
    "missed_payments": np.random.poisson(0.6, n),
    "recent_inquiries": np.random.poisson(1.2, n)
})

data.head()

```

## Target Generation

We generate a synthetic default probability.

This is only for educational purposes. In a real project, the target must be defined from historical repayment behavior.

```python
logit = (
    -3.0
    + 2.8 * data["credit_utilization"]
    + 0.45 * data["missed_payments"]
    + 0.25 * data["recent_inquiries"]
    + 0.000025 * data["loan_amount"]
    - 0.000015 * data["income"]
    - 0.02 * data["employment_years"]
)

prob_default = 1 / (1 + np.exp(-logit))
data["default"] = np.random.binomial(1, prob_default)

data["default"].mean()

```

## Data Preparation

Data preparation transforms raw data into a modeling-ready dataset.

Typical activities:

* Missing value treatment.
* Outlier handling.
* Data type correction.
* Variable transformations.
* Quality checks.

```python
data.isna().sum()

```

```python
data.describe().T

```

## Feature Engineering

Feature engineering creates more informative predictors.

For credit scoring, common features include:

* Debt-to-income ratio.
* Loan-to-income ratio.
* Behavioral payment indicators.
* Credit utilization metrics.

```python
data["loan_to_income"] = data["loan_amount"] / data["income"]
data["has_missed_payment"] = (data["missed_payments"] > 0).astype(int)

features = [
    "age",
    "income",
    "loan_amount",
    "employment_years",
    "credit_utilization",
    "missed_payments",
    "recent_inquiries",
    "loan_to_income",
    "has_missed_payment"
]

target = "default"

data[features + [target]].head()

```

## Train/Test Split

The dataset is split into training and testing samples.

This separation is essential to estimate model performance on unseen data.

```python
X = data[features]
y = data[target]

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.3,
    random_state=RANDOM_SEED,
    stratify=y
)

X_train.shape, X_test.shape

```

## Model Development

We train a simple logistic regression model.

Logistic regression is widely used in credit scoring because it is interpretable and relatively easy to validate.

```python
model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

pred_proba = model.predict_proba(X_test)[:, 1]
pred_class = (pred_proba >= 0.5).astype(int)

auc = roc_auc_score(y_test, pred_proba)
acc = accuracy_score(y_test, pred_class)

auc, acc

```

## Model Validation

Validation assesses whether the model is appropriate for its intended use.

Common validation dimensions:

* Predictive performance.
* Stability.
* Explainability.
* Fairness.
* Robustness.

```python
print(classification_report(y_test, pred_class))

```

```python
cm = confusion_matrix(y_test, pred_class)
cm_df = pd.DataFrame(
    cm,
    index=["Actual non-default", "Actual default"],
    columns=["Predicted non-default", "Predicted default"]
)
cm_df

```

## Feature Importance

For logistic regression, coefficients provide a simple view of variable contribution.

Positive coefficients increase predicted default probability.

Negative coefficients decrease predicted default probability.

```python
coef_df = pd.DataFrame({
    "feature": features,
    "coefficient": model.coef_[0]
}).sort_values("coefficient", ascending=False)

coef_df

```

```python
plt.figure(figsize=(8, 5))
plt.barh(coef_df["feature"], coef_df["coefficient"])
plt.title("Logistic Regression Coefficients")
plt.xlabel("Coefficient")
plt.tight_layout()
plt.show()

```

## Deployment Considerations

Before deployment, the model should be packaged and registered.

Typical deployment questions:

* Which model version will be deployed?
* Which dataset trained the model?
* Which validation report approved the model?
* What endpoint or batch process will serve predictions?
* What rollback mechanism exists?

```python
model_card = {
    "model_name": "baseline_credit_scoring_logistic_regression",
    "model_type": "LogisticRegression",
    "dataset_version": "synthetic_credit_dataset_v1",
    "auc": round(float(auc), 4),
    "accuracy": round(float(acc), 4),
    "random_seed": RANDOM_SEED,
    "status": "candidate"
}

model_card

```

## Monitoring Simulation

Once deployed, the model must be monitored.

Monitoring dimensions:

* Operational metrics.
* Data quality.
* Data drift.
* Model performance.
* Business KPIs.

Below we simulate a production population with higher credit utilization.

```python
production = data.sample(1000, random_state=RANDOM_SEED).copy()
production["credit_utilization"] = (production["credit_utilization"] + 0.20).clip(0, 1)
production["loan_to_income"] = production["loan_amount"] / production["income"]

prod_pred_proba = model.predict_proba(production[features])[:, 1]

monitoring_report = pd.DataFrame({
    "metric": [
        "Training default rate",
        "Production predicted default rate",
        "Training avg credit utilization",
        "Production avg credit utilization"
    ],
    "value": [
        data["default"].mean(),
        prod_pred_proba.mean(),
        data["credit_utilization"].mean(),
        production["credit_utilization"].mean()
    ]
})

monitoring_report

```

```python
plt.figure(figsize=(8, 4))
plt.hist(data["credit_utilization"], bins=30, alpha=0.6, label="Training")
plt.hist(production["credit_utilization"], bins=30, alpha=0.6, label="Production")
plt.title("Monitoring Example: Credit Utilization Drift")
plt.xlabel("Credit utilization")
plt.ylabel("Frequency")
plt.legend()
plt.tight_layout()
plt.show()

```

## Lifecycle Mapping to MLOps Controls

| Lifecycle stage | MLOps control |
| :--- | :--- |
| Business Understanding | Business KPIs and acceptance criteria |
| Data Collection | Data lineage and ownership |
| Data Preparation | Data quality checks |
| Feature Engineering | Feature versioning and documentation |
| Model Development | Experiment tracking |
| Validation | Independent validation report |
| Deployment | Model registry and CI/CD |
| Monitoring | Drift and performance monitoring |
| Retraining | Automated retraining pipeline |


## Exercises

### Exercise 1 — Business Requirements

Define three business KPIs for the credit scoring model.

### Exercise 2 — Data Preparation

Add a new data quality check to the dataset.

### Exercise 3 — Feature Engineering

Create a new feature and evaluate whether it improves the model.

### Exercise 4 — Monitoring

Modify the production population and observe how the monitoring report changes.

### Exercise 5 — Governance

Complete the `model_card` dictionary with at least five additional governance fields.

## Key Takeaways

* The machine learning lifecycle starts with a business problem.
* Data preparation and feature engineering are critical.
* Model validation must happen before deployment.
* Deployment is not the end of the lifecycle.
* Monitoring and retraining are essential for long-term reliability.
* MLOps provides controls across all lifecycle stages.
