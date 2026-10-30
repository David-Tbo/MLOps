# Credit Scoring Mini Project

This notebook defines the Module 1 mini project: designing a production-grade MLOps platform for credit scoring.

## Learning objectives

By the end of this notebook, you will be able to:

- Design a target MLOps architecture for credit scoring.
- Define data sources, features, training, deployment, monitoring, and governance controls.
- Build a simplified credit scoring dataset.
- Train a baseline model.
- Produce a mini project architecture and governance checklist.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score, classification_report

RANDOM_SEED = 42
np.random.seed(RANDOM_SEED)

```

## Business Context

A retail bank wants to deploy a machine learning model to support credit application decisions.

Business objectives:

* Reduce default losses.
* Maintain acceptable approval rates.
* Improve decision speed.
* Ensure model auditability.
* Provide explainable decisions.

## Target Architecture

| Layer | Credit scoring component |
| :--- | :--- |
| Data Sources | Customer, loan, bureau, payment, macroeconomic data |
| Data Pipelines | Validation, cleaning, integration, dataset publication |
| Feature Store | Debt-to-income, utilization, inquiries, missed payments |
| Training Pipeline | Automated model training and evaluation |
| Experiment Tracking | MLflow run tracking |
| Model Registry | Approved model versions |
| Deployment | Batch scoring or real-time API |
| Monitoring | AUC, drift, latency, approval rate, default rate |
| Responsible AI | Fairness, SHAP explanations, bias assessment |
| Governance | Documentation, approval workflow, audit trail |

## Simulated Credit Scoring Dataset

```python
n = 4000

df = pd.DataFrame({
    "age": np.random.randint(21, 70, n),
    "income": np.random.normal(55000, 18000, n).clip(15000, 180000),
    "loan_amount": np.random.normal(18000, 7000, n).clip(1000, 70000),
    "employment_years": np.random.exponential(6, n).clip(0, 40),
    "credit_utilization": np.random.beta(2.2, 4.5, n),
    "missed_payments": np.random.poisson(0.5, n),
    "recent_inquiries": np.random.poisson(1.1, n)
})

df["loan_to_income"] = df["loan_amount"] / df["income"]
df["has_missed_payment"] = (df["missed_payments"] > 0).astype(int)

logit = (
    -3.2
    + 3.0 * df["credit_utilization"]
    + 0.5 * df["missed_payments"]
    + 0.25 * df["recent_inquiries"]
    + 2.0 * df["loan_to_income"]
    - 0.02 * df["employment_years"]
)

prob_default = 1 / (1 + np.exp(-logit))
df["default"] = np.random.binomial(1, prob_default)

df.head()

```

```python
df["default"].mean()

```

## Baseline Model Training

```python
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

X = df[features]
y = df["default"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.3,
    random_state=RANDOM_SEED,
    stratify=y
)

model = LogisticRegression(max_iter=1000)
model.fit(X_train, y_train)

pred_proba = model.predict_proba(X_test)[:, 1]
pred_class = (pred_proba >= 0.5).astype(int)

auc = roc_auc_score(y_test, pred_proba)
auc

```

```python
print(classification_report(y_test, pred_class))

```

## Model Card Draft

```python
model_card = {
    "model_name": "credit_scoring_baseline",
    "model_type": "LogisticRegression",
    "business_owner": "Credit Risk Department",
    "dataset_version": "synthetic_credit_v1",
    "training_date": "YYYY-MM-DD",
    "auc": round(float(auc), 4),
    "status": "candidate",
    "intended_use": "Credit application risk assessment",
    "limitations": "Synthetic educational dataset; not suitable for production use"
}

model_card

```

## Monitoring Plan

| Monitoring area | Metric | Action if alert |
| --- | --- | --- |
| Operational | Latency and error rate | Investigate API or infrastructure |
| Data Quality | Missing values and schema violations | Block scoring or trigger data incident |
| Data Drift | Feature distribution shift | Investigate population change |
| Model Performance | ROC-AUC, recall, calibration | Trigger model review or retraining |
| Business KPI | Default rate and approval rate | Escalate to business owner |
| Responsible AI | Group disparity and explainability | Review fairness and governance controls |



## Simulated Drift Scenario

| Feature | Training mean | Production mean |
| :--- | :---: | :---: |
| credit_utilization | *mu*<sub>train</sub> | *mu*<sub>train</sub> + 0.25 *(max 1.0)* |
| loan_to_income | *mu*<sub>train</sub> | *mu*<sub>train</sub> |

## Governance Checklist

| Control | Status |
| :--- | :--- |
| Dataset version retained | To be completed |
| Training code version retained | To be completed |
| Hyperparameters logged | To be completed |
| Validation report produced | To be completed |
| Approval workflow completed | To be completed |
| Monitoring dashboard configured | To be completed |
| Responsible AI assessment completed | To be completed |
| Rollback plan documented | To be completed |

## Final Project Deliverables

| Deliverable | Expected content |
| :--- | :--- |
| Architecture diagram | End-to-end MLOps components and data flows |
| Technology selection report | Justification of selected tools |
| Model card | Purpose, data, model, metrics, limitations |
| Monitoring plan | Metrics, thresholds, alert actions |
| Governance checklist | Auditability and compliance controls |
| RACI matrix | Roles and responsibilities |
| Risk assessment | Key risks and mitigations |

## Exercises

### Exercise 1 — Architecture Design

Complete an architecture for the credit scoring platform using the layers shown above.

### Exercise 2 — Tool Selection

Choose one tool for each layer and justify your choice.

### Exercise 3 — Governance

Complete the governance checklist with ownership and evidence.

### Exercise 4 — Monitoring

Define thresholds for:

* AUC degradation.
* Missing values.
* Feature drift.
* API latency.
* Approval rate.

### Exercise 5 — Responsible AI

Define two fairness metrics that could be monitored for credit scoring.

## Key Takeaways

* The mini project connects all concepts introduced in Module 1.
* A credit scoring platform requires data, model, deployment, monitoring, governance, and Responsible AI layers.
* Model performance alone is not sufficient for production readiness.
* Auditability and monitoring must be designed from the beginning.
* This project will be progressively implemented in later modules.
