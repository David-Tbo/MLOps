# Core Principles of MLOps

This notebook illustrates the major MLOps principles with practical Python examples.

## Learning Objectives

- Reproducibility
- Version Control concepts
- Experiment Tracking
- Automation
- CI/CD concepts
- Monitoring
- Governance

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

np.random.seed(42)

```

## Reproducibility

```python
seed = 42
np.random.seed(seed)

sample = np.random.normal(size=1000)

print('Mean:', sample.mean())
print('Std :', sample.std())

```

### Exercise

Change the seed and compare the results.

Question:

Why should the seed be tracked in ML experiments?

## Versioning Example

```python
dataset_v1 = pd.DataFrame({
    'income': [30000, 50000, 70000],
    'target': [0, 0, 1]
})

dataset_v2 = pd.DataFrame({
    'income': [30000, 50000, 70000, 90000],
    'target': [0, 0, 1, 1]
})

print('Rows v1:', len(dataset_v1))
print('Rows v2:', len(dataset_v2))

```

Discussion:

* Which version trained the model?
* Can results be reproduced without dataset versioning?

## Experiment Tracking

| Run ID | Learning rate | AUC |
| :---: | :---: | :---: |
| run_2 | 0.05 | 0.82 |
| run_3 | 0.10 | 0.80 |
| run_1 | 0.01 | 0.78 |


Typical information stored by MLflow:

* Parameters
* Metrics
* Artifacts
* Dataset version
* User
* Timestamp

## Automation

| Pipeline step |
| :--- |
| Data ingestion |
| Validation |
| Feature engineering |
| Training |
| Evaluation |
| Deployment |


**Automation reduces:**

* Manual errors
* Deployment delays
* Reproducibility issues

## Continuous Integration

| Check | Status |
| :--- | :---: |
| unit_tests | PASS |
| schema_validation | PASS |
| code_quality | PASS |


Typical CI actions:

* Run tests
* Validate data schema
* Build artifacts
* Prepare deployment package

## Continuous Delivery

| Environment |
| :--- |
| Development |
| Testing |
| Staging |
| Production |

```

Promotion flow:

Development → Testing → Staging → Production

## Monitoring Example

```python
months = np.arange(1, 13)

auc = [0.84, 0.84, 0.83, 0.83, 0.82, 0.81, 0.79, 0.77, 0.74, 0.72, 0.69, 0.66]

plt.figure(figsize=(8, 4))
plt.plot(months, auc, marker='o')
plt.title('Model Performance Degradation')
plt.xlabel('Month')
plt.ylabel('AUC')
plt.grid(True)
plt.show()

```

Questions:

* When should an alert be triggered?
* When should retraining occur?

## Governance

| Artifact | Required |
| :--- | :---: |
| Dataset | Yes |
| Model | Yes |
| Validation Report | Yes |
| Deployment Record | Yes |


Governance ensures:

* Auditability
* Traceability
* Compliance
* Accountability

## MLOps Maturity Model

| Level | Description |
| :---: | :--- |
| 0 | Manual |
| 1 | Partial automation |
| 2 | Pipelines |
| 3 | Enterprise MLOps |
| 4 | Continuous Training |


## Final Exercise

A bank has:

* No Git repository
* No experiment tracking
* Manual deployment
* No monitoring

Identify:

1. Risks
2. Missing MLOps principles
3. Recommended tools

## Key Takeaways

* Reproducibility is mandatory.
* Everything should be versioned.
* Experiments must be tracked.
* Automation improves reliability.
* Monitoring is essential.
* Governance supports trust and compliance.