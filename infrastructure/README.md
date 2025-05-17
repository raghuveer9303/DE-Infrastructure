# Brahma Data Engineering Infrastructure (Docker Compose)

This repository contains a Docker Compose-based infrastructure for a data engineering platform that supports multiple projects.

## Directory Structure

```
DE-Infrastructure/
├── infrastructure/           # Docker Compose and infrastructure definitions
│   ├── docker-compose.yml   # Main infrastructure definition
│   ├── .env                 # Environment variables
│   └── projects/           # Project-specific configurations
│       ├── project1/       # Configuration for Project 1
│       │   ├── dbt/       # DBT configuration for Project 1
│       │   └── airflow/   # Airflow DAGs for Project 1
│       └── project2/      # Configuration for Project 2
│           ├── dbt/      # DBT configuration for Project 2
│           └── airflow/  # Airflow DAGs for Project 2
└── shared/                 # Shared resources across projects
    ├── minio/             # MinIO data and configuration
    ├── postgres/          # PostgreSQL data
    └── notebooks/         # Shared Jupyter notebooks
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

- Docker and Docker Compose installed
- Git LFS (for large file handling)
- At least 8GB RAM available for Docker

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/your-org/DE-Infrastructure.git
   cd DE-Infrastructure
   ```

2. **Configure environment variables**

   ```bash
   cp .env.example .env
   # Edit .env with your specific configurations
   ```

3. **Start the infrastructure**

   ```bash
   docker-compose up -d
   ```

### Project Setup

1. **Create a new project**

   ```bash
   ./scripts/create-project.sh my-project
   ```

   This will create a new project structure in `infrastructure/projects/my-project/`

2. **Configure project-specific settings**

   - Edit `infrastructure/projects/my-project/dbt/profiles.yml`
   - Add DAGs to `infrastructure/projects/my-project/airflow/dags/`
   - Configure project-specific variables in `infrastructure/projects/my-project/.env`

3. **Mount project volumes**

   Add the following to your project's section in `docker-compose.yml`:

   ```yaml
   volumes:
     - ./projects/my-project/dbt:/opt/dbt
     - ./projects/my-project/airflow/dags:/opt/airflow/dags
   ```

### Accessing Services

- **MinIO**: http://localhost:9000 (user: minioadmin, password: minioadmin)
- **JupyterHub**: http://localhost:8000
- **Airflow**: http://localhost:8080
- **Spark**: http://localhost:4040
- **Superset**: http://localhost:8088 (user: admin, password: admin)

### Working with Projects

#### Running DBT for a Specific Project

```bash
docker-compose exec dbt dbt run --project-dir /opt/dbt/my-project
```

#### Managing Airflow DAGs

DAGs are automatically loaded from the project's `airflow/dags` directory.

#### Using Jupyter

Notebooks are stored in `shared/notebooks/` and will be available in the JupyterHub interface.

### Project Management

#### Adding a New Project

1. Create project directory:
   ```bash
   mkdir -p infrastructure/projects/new-project/{dbt,airflow/dags}
   ```

2. Copy template files:
   ```bash
   cp templates/dbt/* infrastructure/projects/new-project/dbt/
   cp templates/airflow/* infrastructure/projects/new-project/airflow/
   ```

3. Update docker-compose.yml with new project volumes

#### Removing a Project

1. Stop project services:
   ```bash
   docker-compose stop project-name
   ```

2. Remove project directory:
   ```bash
   rm -rf infrastructure/projects/project-name
   ```

3. Update docker-compose.yml to remove project configuration

## Security Notes

- Change all default credentials in production
- Use Docker secrets for sensitive information
- Implement proper authentication for all services
- Use HTTPS in production
- Regularly update Docker images for security patches

## Troubleshooting

### Common Issues

1. **Port Conflicts**
   - Check if ports are already in use: `netstat -tulpn | grep LISTEN`
   - Modify ports in docker-compose.yml if needed

2. **Volume Mount Issues**
   - Ensure proper permissions on mounted directories
   - Check Docker volume configuration

3. **Memory Issues**
   - Adjust Docker memory limits in Docker Desktop settings
   - Reduce resource allocation in docker-compose.yml

### Logs

View logs for specific services:
```bash
docker-compose logs -f [service-name]
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details. 