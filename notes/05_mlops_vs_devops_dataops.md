# MLOps vs DevOps vs DataOps

This notebook compares DevOps, DataOps, and MLOps.

It explains how these disciplines complement one another when building production-grade machine learning systems.

## Learning objectives

By the end of this notebook, you will be able to:

- Define DevOps, DataOps, and MLOps.
- Compare their objectives, assets, and responsibilities.
- Understand why MLOps extends both DevOps and DataOps.
- Map roles and activities in a credit scoring project.
- Build a simple responsibility matrix.

## Setup

```python
import pandas as pd
import matplotlib.pyplot as plt

```

## High-Level Definitions

### DevOps

DevOps combines software development and IT operations to improve software delivery.

### DataOps

DataOps applies DevOps-inspired principles to data pipelines and analytics workflows.

### MLOps

MLOps extends DevOps and DataOps concepts to machine learning systems.

| Discipline | Primary focus | Primary asset |
| :--- | :--- | :--- |
| DevOps | Software delivery and infrastructure operations | Code and applications |
| DataOps | Reliable data pipelines and data quality | Data and pipelines |
| MLOps | Machine learning lifecycle management | Models, features, and experiments |


## Main Objectives

Each discipline improves reliability, automation, and collaboration, but the operational target is different.

| Discipline | Objectives |
| :--- | :--- |
| DevOps | Accelerate software delivery, automate deployment, improve application reliability |
| DataOps | Improve data quality, automate data pipelines, manage data lineage |
| MLOps | Improve reproducibility, automate training/deployment, monitor model behavior |

## Primary Assets Managed

DevOps, DataOps, and MLOps manage different types of assets.

Understanding these assets helps define responsibilities and tool choices.

| Asset | DevOps | DataOps | MLOps |
| :--- | :---: | :---: | :---: |
| Source code | Yes | Partial | Yes |
| Applications | Yes | No | Partial |
| Infrastructure | Yes | Partial | Partial |
| Datasets | No | Yes | Yes |
| Data pipelines | Partial | Yes | Yes |
| Features | No | Partial | Yes |
| Experiments | No | No | Yes |
| Models | No | No | Yes |
| Model registry | No | No | Yes |


## Tooling Comparison

The tooling landscape overlaps, but each discipline has a different center of gravity.

| Category | Example tools | Main discipline |
| :--- | :--- | :--- |
| Source control | Git, GitHub, GitLab | DevOps / MLOps |
| CI/CD | GitHub Actions, Azure DevOps, Jenkins | DevOps / MLOps |
| Infrastructure | Docker, Kubernetes, Terraform | DevOps |
| Data orchestration | Airflow, Azure Data Factory, Databricks Workflows | DataOps |
| Data quality | Great Expectations, dbt tests | DataOps |
| Experiment tracking | MLflow, Weights & Biases, Azure ML | MLOps |
| Model registry | MLflow Registry, Azure ML Registry | MLOps |
| Model monitoring | Evidently AI, Azure Monitor, Grafana | MLOps |

## Why MLOps Is More Complex

Traditional software is mostly deterministic.

Machine learning systems depend on both code and data.

This introduces additional complexity:

* Model drift.
* Dataset versioning.
* Experiment tracking.
* Feature consistency.
* Model retraining.
* Fairness and explainability.

| Challenge | Concept | Main owner |
| :--- | :--- | :--- |
| Changing input data | Data drift | MLOps / DataOps |
| Changing target relationship | Concept drift | MLOps / Data Science |
| Multiple candidate models | Experiment management | MLOps / Data Science |
| Training-serving skew | Feature consistency | MLOps / Data Engineering |
| Regulatory explainability | Governance / Responsible AI | Risk / MLOps |
| Performance degradation | Monitoring | MLOps / Operations |


## Credit Scoring Project Example

A bank wants to build and deploy a credit scoring platform.

Different disciplines contribute at different stages.

| Activity | Main discipline |
| :--- | :--- |
| Collect customer data | DataOps |
| Build data pipelines | DataOps |
| Train credit scoring model | MLOps |
| Track experiments | MLOps |
| Deploy prediction API | DevOps / MLOps |
| Monitor infrastructure | DevOps |
| Monitor data drift | MLOps |
| Validate model governance | MLOps / Risk |


## Responsibility Matrix

The following matrix is simplified.

In practice, responsibilities depend on the organization.

| Activity | Data Engineer | Data Scientist | ML Engineer | DevOps Engineer | Risk Manager |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Data ingestion | R | C | C | I | I |
| Data quality checks | R | C | C | I | C |
| Model development | C | R | C | I | I |
| Experiment tracking | I | R | R | C | I |
| Model validation | I | C | C | I | A |
| API deployment | I | I | R | R | A |
| Production monitoring | C | C | R | R | A |
| Governance review | I | C | C | I | R |


Legend:

* R = Responsible
* A = Accountable
* C = Consulted
* I = Informed

## Visual Comparison

The following chart counts the number of activities primarily owned by each discipline in the credit scoring example.

| Main discipline | Count |
| :--- | :---: |
| MLOps | 3 |
| DataOps | 2 |
| DevOps / MLOps | 1 |
| DevOps | 1 |
| MLOps / Risk | 1 |

## Practical Exercise

You are asked to assess an organization with the following situation:

```text
- Git is used for source code.
- Data pipelines are manual.
- No experiment tracking exists.
- Models are deployed manually.
- Monitoring only covers infrastructure.
- Model validation is performed in Excel.

```

Questions:

1. Which DevOps capabilities already exist?
2. Which DataOps capabilities are missing?
3. Which MLOps capabilities are missing?
4. What should be implemented first?
5. Which tools would you recommend?

| Area | Current state | Target state | Recommended tool |
| :--- | :--- | :--- | :--- |
| Source control | | | |
| Data pipelines | | | |
| Experiment tracking | | | |
| Model registry | | | |
| CI/CD | | | |
| Monitoring | | | |
| Governance | | | |

## Suggested Solution

One possible target state is shown below.

| Area | Current state | Target state | Recommended tool |
| :--- | :--- | :--- | :--- |
| Source control | Git exists | Git with pull requests | GitHub |
| Data pipelines | Manual | Automated and validated | Airflow / Azure Data Factory |
| Experiment tracking | Missing | Centralized tracking | MLflow |
| Model registry | Missing | Controlled model lifecycle | MLflow Registry / Azure ML Registry |
| CI/CD | Manual deployment | Automated deployment | GitHub Actions / Azure DevOps |
| Monitoring | Infrastructure only | Infrastructure + data + model monitoring | Prometheus / Grafana / Evidently AI |
| Governance | Excel-based | Formal model documentation and approval workflow | Model cards / validation reports / approval workflow |


## Key Takeaways

* DevOps focuses on software and infrastructure.
* DataOps focuses on data pipelines and data quality.
* MLOps focuses on models, experiments, features, deployment, monitoring, and governance.
* MLOps reuses DevOps and DataOps principles but adds model-specific controls.
* Production ML requires collaboration across all three disciplines.
