# MLOps Tooling Landscape

This notebook presents the main categories of tools used in modern MLOps platforms.

It maps tools to the machine learning lifecycle and to the architecture layers introduced in previous notebooks.

## Learning objectives

By the end of this notebook, you will be able to:

- Identify the major categories of MLOps tools.
- Map tools to MLOps architecture layers.
- Understand the role of Git, Docker, MLflow, DVC, Airflow, Kubeflow, Azure ML, and monitoring tools.
- Compare tool choices for different organizational contexts.
- Design a coherent MLOps technology stack.

## Setup

```python
import pandas as pd
import matplotlib.pyplot as plt

```

## Tooling Categories

An MLOps platform is usually composed of several categories of tools.

No single tool covers the complete ML lifecycle perfectly.

| Category | Purpose |
| :--- | :--- |
| Source Control | Track code, configuration, and documentation changes |
| Data Versioning | Track datasets and data transformations |
| Experiment Tracking | Record parameters, metrics, artifacts, and metadata |
| Feature Store | Store and serve reusable features |
| Pipeline Orchestration | Automate data, training, and deployment workflows |
| Model Registry | Manage model versions and lifecycle stages |
| Containerization | Package applications and dependencies |
| Deployment | Serve models through APIs, batch jobs, or endpoints |
| CI/CD | Automate testing, packaging, and deployment |
| Monitoring | Track operational, data, model, and business metrics |
| Governance | Ensure auditability, compliance, and approval workflows |


## Source Control

Source control is foundational for MLOps.

It provides traceability, collaboration, rollback capabilities, and auditability.

| Tool | Main use | Typical artifacts |
| :--- | :--- | :--- |
| Git | Distributed version control | Code, configs, docs |
| GitHub | Repository hosting and collaboration | Code, issues, pull requests, workflows |
| GitLab | Repository hosting with integrated DevOps | Code, CI/CD pipelines, security scans |
| Azure Repos | Enterprise repository hosting in Azure DevOps | Code, enterprise projects |


## Data Versioning

Data versioning is necessary to reproduce model training and validation results.

Questions answered by data versioning:

* Which data trained the model?
* Which records were included?
* Which transformations were applied?
* Can the dataset be reconstructed later?

| Tool | Description | Strength |
| :--- | :--- | :--- |
| DVC | Git-like data version control for ML projects | Simple integration with Git workflows |
| LakeFS | Git-like versioning for data lakes | Useful for data lake branching and rollback |
| Delta Lake | Storage layer with ACID transactions and time travel | Strong fit for lakehouse environments |

## Experiment Tracking

Experiment tracking records model development activities.

Typical tracked information:

* Parameters.
* Metrics.
* Artifacts.
* Dataset versions.
* Feature versions.
* Execution metadata.

| Tool | Strength | Best for |
| :--- | :--- | :--- |
| MLflow | Open-source, framework agnostic, widely adopted | General-purpose ML projects |
| Weights & Biases | Strong dashboards and collaboration features | Deep learning and research workflows |
| Azure ML | Enterprise integration with Azure ecosystem | Enterprise Azure environments |
| Neptune | Metadata management and team collaboration | Centralized experiment metadata tracking |

## Model Registry

A model registry manages model versions and lifecycle stages.

Typical stages:

```text
Development → Validation → Staging → Production → Retired

```

| Tool | Main capability | Typical context |
| :--- | :--- | :--- |
| MLflow Registry | Open-source model lifecycle management | Open-source or hybrid environments |
| Azure ML Registry | Enterprise model management in Azure | Microsoft enterprise environments |
| SageMaker Model Registry | AWS-native model registry | AWS environments |
| Vertex AI Model Registry | Google Cloud-native model registry | Google Cloud environments |

## Feature Stores

Feature stores ensure consistency between training and inference.

They help avoid:

* Duplicate feature logic.
* Inconsistent definitions.
* Training-serving skew.
* Poor feature documentation.

| Tool | Type | Main strength |
| :--- | :--- | :--- |
| Feast | Open source | Flexible and cloud-portable |
| Databricks Feature Store | Managed platform | Integrated with Databricks and MLflow |
| Tecton | Managed platform | Enterprise-scale online/offline feature serving |
| Azure Feature Store | Managed platform | Integrated with Azure ML ecosystem |


## Pipeline Orchestration

Pipeline orchestration automates workflows such as:

* Data ingestion.
* Data validation.
* Feature engineering.
* Model training.
* Model evaluation.
* Deployment.

| Tool | Focus | Typical users |
| :--- | :--- | :--- |
| Apache Airflow | General workflow orchestration | Data engineers |
| Kubeflow Pipelines | Kubernetes-native ML pipelines | ML engineers |
| Azure ML Pipelines | Managed enterprise ML pipelines | Enterprise ML teams |
| Databricks Workflows | Databricks-native workflows | Data and ML teams |
| Prefect | Modern workflow orchestration | Data and platform teams |

## Containerization and Deployment

Containerization packages model services with their dependencies.

Deployment tools serve models in production environments.

| Tool | Role | Example use |
| :--- | :--- | :--- |
| Docker | Package code and dependencies | Containerize scoring service |
| Kubernetes | Orchestrate containers at scale | Scale model serving workloads |
| FastAPI | Build prediction APIs | Expose credit scoring prediction API |
| Azure ML Endpoints | Deploy managed online/batch endpoints | Enterprise deployment on Azure |
| BentoML | Package and serve ML models | Standardize model serving |

## CI/CD Platforms

CI/CD platforms automate testing, packaging, and deployment.

For MLOps, CI/CD may include:

* Unit tests.
* Data validation.
* Model validation.
* Docker image build.
* Endpoint deployment.

| Tool | Strength | Typical context |
| :--- | :--- | :--- |
| GitHub Actions | Native GitHub integration and simplicity | GitHub repositories |
| Azure DevOps | Enterprise-grade Microsoft ecosystem integration | Enterprise Azure environments |
| GitLab CI/CD | Integrated repository and CI/CD platform | GitLab environments |
| Jenkins | Highly customizable automation server | Legacy or highly customized environments |

## Monitoring Tools

Monitoring must cover infrastructure, data, models, and business impact.

| Tool | Focus | Example metric |
| :--- | :--- | :--- |
| Prometheus | Metrics collection | CPU, latency, request count |
| Grafana | Dashboards and visualization | Dashboards for APIs and model metrics |
| Azure Monitor | Cloud monitoring in Azure | Endpoint availability and logs |
| Evidently AI | Data drift and model monitoring | Feature drift, data quality, model performance |
| WhyLabs | ML observability | Data quality, drift, prediction monitoring |

## Governance and Responsible AI Tools

Governance tools help document, validate, approve, and audit models.

Responsible AI tools help assess fairness, explainability, transparency, and accountability.

| Tool | Purpose | Output |
| :--- | :--- | :--- |
| Model Cards | Standardized model documentation | Model documentation |
| SHAP | Global and local explainability | Feature contribution explanations |
| Fairlearn | Fairness assessment and mitigation | Fairness metrics |
| Responsible AI Dashboard | Responsible AI analysis interface | Fairness, error analysis, interpretability reports |
| Internal Approval Workflow | Formal validation and deployment approval | Approval evidence and audit trail |

## Reference MLOps Stack

The following table proposes a coherent stack for this training program.

| Architecture layer | Recommended tool | Reason |
| :--- | :--- | :--- |
| Source Control | Git + GitHub | Standard collaboration and versioning |
| Data Versioning | DVC or Delta Lake | Dataset reproducibility |
| Experiment Tracking | MLflow Tracking | Experiment traceability |
| Model Registry | MLflow Registry / Azure ML Registry | Model lifecycle management |
| Containerization | Docker | Environment reproducibility |
| API Serving | FastAPI | Simple production API pattern |
| CI/CD | GitHub Actions | Automated tests and deployments |
| Cloud Platform | Azure Machine Learning | Managed enterprise MLOps capabilities |
| Monitoring | Prometheus / Grafana / Evidently AI | Operational and model observability |
| Responsible AI | SHAP / Fairlearn / Responsible AI Dashboard | Explainability and fairness controls |

## Visual Summary

The chart below shows how many tools are listed per category in this notebook.

| Category | Tool count |
| :--- | :---: |
| Source Control | 4 |
| Data Versioning | 3 |
| Experiment Tracking | 4 |
| Model Registry | 4 |
| Feature Store | 4 |
| Orchestration | 5 |
| Deployment | 5 |
| CI/CD | 4 |
| Monitoring | 5 |
| Governance | 5 |

## Tool Selection Exercise

You are designing an MLOps platform for a bank.

Constraints:

```text
- GitHub is already used.
- Azure is the preferred cloud provider.
- Models must be auditable.
- Credit scoring requires explainability.
- The team wants to start simple.

```

Complete the following selection table.

| Need | Selected tool | Justification |
| :--- | :--- | :--- |
| Source control | | |
| Experiment tracking | | |
| Model registry | | |
| Model deployment | | |
| CI/CD | | |
| Monitoring | | |
| Explainability | | |

## Suggested Solution

| Need | Selected tool | Justification |
| :--- | :--- | :--- |
| Source control | GitHub | Already used by the organization |
| Experiment tracking | MLflow or Azure ML | Tracks experiments and metadata |
| Model registry | Azure ML Registry | Enterprise governance and Azure integration |
| Model deployment | Azure ML Managed Endpoint | Managed deployment and security |
| CI/CD | GitHub Actions | Native GitHub automation |
| Monitoring | Azure Monitor + Evidently AI | Covers cloud and ML-specific monitoring |
| Explainability | SHAP + Responsible AI Dashboard | Supports local/global explanations and fairness analysis |

## Key Takeaways

* MLOps platforms combine multiple tools.
* Tool selection should follow architecture and governance needs.
* Git, Docker, MLflow, CI/CD, monitoring, and cloud platforms are foundational.
* The best stack is not necessarily the most complex stack.
* Start simple, then increase maturity progressively.
