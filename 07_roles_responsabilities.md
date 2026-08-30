# Roles and Responsibilities

This notebook explores the key roles required to deliver and operate production-grade machine learning systems.

## Learning objectives

By the end of this notebook, you will be able to:

- Identify the main roles involved in MLOps.
- Understand responsibilities across the ML lifecycle.
- Build a RACI matrix.
- Analyze team collaboration in a credit scoring project.

```python
import pandas as pd
import matplotlib.pyplot as plt

```

## Main MLOps Roles

```python
roles = pd.DataFrame({
    "role": [
        "Business Owner",
        "Product Owner",
        "Data Engineer",
        "Data Scientist",
        "ML Engineer",
        "DevOps Engineer",
        "Model Validator",
        "Risk Manager",
        "Security Officer",
        "Operations Team"
    ],
    "primary_focus": [
        "Business value and strategic objectives",
        "Requirements and prioritization",
        "Data pipelines and data quality",
        "Model development and analysis",
        "Operationalization of ML models",
        "Infrastructure, CI/CD, platform reliability",
        "Independent review and challenge",
        "Governance, policy, and compliance",
        "Security controls and access management",
        "Production support and incident management"
    ]
})

roles

```

## Responsibilities by Lifecycle Stage

```python
lifecycle_roles = pd.DataFrame({
    "stage": [
        "Business Understanding",
        "Data Collection",
        "Data Preparation",
        "Feature Engineering",
        "Model Development",
        "Validation",
        "Deployment",
        "Monitoring",
        "Retraining"
    ],
    "main_roles": [
        "Business Owner, Product Owner",
        "Data Engineer, Data Owner",
        "Data Engineer, Data Scientist",
        "Data Scientist, ML Engineer",
        "Data Scientist",
        "Model Validator, Risk Manager",
        "ML Engineer, DevOps Engineer",
        "ML Engineer, Operations Team, Risk Manager",
        "ML Engineer, Data Scientist, Product Owner"
    ]
})

lifecycle_roles

```

## RACI Matrix Example

```python
raci = pd.DataFrame({
    "activity": [
        "Define business objective",
        "Build data pipeline",
        "Create features",
        "Train model",
        "Track experiments",
        "Validate model",
        "Deploy model",
        "Monitor model",
        "Approve retraining"
    ],
    "Business Owner": ["A", "I", "I", "I", "I", "C", "A", "C", "A"],
    "Data Engineer": ["C", "R", "C", "I", "I", "I", "I", "C", "I"],
    "Data Scientist": ["C", "C", "R", "R", "R", "C", "I", "C", "C"],
    "ML Engineer": ["I", "C", "C", "C", "R", "I", "R", "R", "R"],
    "DevOps Engineer": ["I", "I", "I", "I", "C", "I", "R", "R", "I"],
    "Risk Manager": ["C", "I", "I", "I", "I", "A", "A", "A", "A"]
})

raci

```

Legend:

* R = Responsible
* A = Accountable
* C = Consulted
* I = Informed

## Credit Scoring Team Scenario

```python
scenario = pd.DataFrame({
    "situation": [
        "Model performance has decreased",
        "Dataset schema has changed",
        "A new model is proposed for production",
        "An auditor asks for model evidence",
        "The API has high latency"
    ],
    "lead_role": [
        "ML Engineer",
        "Data Engineer",
        "Model Validator / Risk Manager",
        "Risk Manager",
        "DevOps Engineer"
    ],
    "supporting_roles": [
        "Data Scientist, Operations Team",
        "ML Engineer, Data Scientist",
        "Data Scientist, Business Owner",
        "Data Scientist, ML Engineer",
        "ML Engineer, Operations Team"
    ]
})

scenario

```

## Responsibility Distribution

```python
role_counts = pd.Series(
    " ".join(lifecycle_roles["main_roles"]).replace(",", "").split()
).value_counts()

role_counts = role_counts.reset_index()
role_counts.columns = ["token", "count"]
role_counts

```

The quick count above is intentionally simple. In real governance documents, responsibilities should be defined explicitly, not inferred from text.

## Exercise — Build Your Own RACI Matrix

```python
raci_template = pd.DataFrame({
    "activity": [
        "Dataset versioning",
        "Feature documentation",
        "Model training",
        "Model approval",
        "Production deployment",
        "Drift monitoring",
        "Incident management"
    ],
    "Business Owner": [""] * 7,
    "Data Engineer": [""] * 7,
    "Data Scientist": [""] * 7,
    "ML Engineer": [""] * 7,
    "DevOps Engineer": [""] * 7,
    "Risk Manager": [""] * 7
})

raci_template

```

## Suggested Reflection Questions

1. Which role should be accountable for production deployment?
2. Which role should be responsible for model monitoring?
3. Why should model validation be independent?
4. What risks appear when Data Scientists deploy models manually?
5. How does a RACI matrix improve governance?

## Key Takeaways

* Production ML requires multiple roles.
* Data Scientists should not be solely responsible for production systems.
* ML Engineers bridge experimentation and deployment.
* Risk Managers and Model Validators are essential in regulated industries.
* RACI matrices clarify ownership and reduce operational risk.
