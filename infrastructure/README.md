# Brahma Data Engineering Infrastructure

This repository contains the Kubernetes infrastructure for a minimal data engineering platform.

## Directory Structure

```
DE-Infrastructure/
├── infrastructure/     # Helm charts and infrastructure definitions
│   ├── values/         # Value files for Helm charts
│   ├── helmfile.yaml   # Main infrastructure definition
│   └── ...
└── applications/       # Application code and configuration
    ├── airflow-dags/   # Airflow DAG definitions
    ├── dbt/            # DBT models and configuration
    │   ├── profiles/   # DBT profiles
    │   └── project/    # DBT project code
    └── notebooks/      # Jupyter notebooks
```

## Components

- **MinIO**: Object storage compatible with S3
- **JupyterHub**: Interactive computing environment
- **Apache Airflow**: Workflow orchestration
- **Apache Spark**: Distributed computing
- **PostgreSQL**: Relational database
- **Apache Superset**: Data visualization
- **DBT**: Data transformation

## Setup Instructions

### Prerequisites

- Kubernetes cluster (e.g., Minikube, MicroK8s)
- Helm and Helmfile installed
- kubectl configured to access your cluster

### Installation

1. **Update paths in configuration files**

   Update the following files to point to your actual application code paths:
   - `infrastructure/values/dbt-runner.yaml`
   - `infrastructure/values/airflow.yaml`
   - `infrastructure/values/jupyterhub.yaml`

2. **Deploy the infrastructure**

   ```bash
   # From the infrastructure directory
   helmfile sync
   ```

### Accessing Services

- **MinIO**: http://minio.brahma (user: minioadmin, password: minioadmin)
- **JupyterHub**: http://jupyter.brahma
- **Airflow**: http://airflow.brahma
- **Spark**: http://spark.brahma
- **Superset**: http://superset.brahma (user: admin, password: admin)

### Working with Applications

#### Airflow

DAGs placed in the `applications/airflow-dags/` directory will be automatically picked up by Airflow.

#### DBT

To run DBT commands:

```bash
kubectl exec -it -n dbt $(kubectl get pods -n dbt -l app=dbt-runner -o jsonpath='{.items[0].metadata.name}') -- dbt run
```

#### Jupyter

Notebooks are stored in `applications/notebooks/` and will be available in the JupyterHub interface.

## Notes

- For production use, secure the services with proper authentication and TLS
- Add actual domain names to your local /etc/hosts file or use a proper DNS setup
- Replace default credentials with secure ones 