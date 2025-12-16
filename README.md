# MLOps Infrastructure & CI/CD

Complete MLOps infrastructure for automated machine learning workflows, including CI/CD pipelines, monitoring, and infrastructure as code.

## Overview

This project provides production-ready MLOps infrastructure for managing machine learning models throughout their lifecycle. It includes automated testing, deployment pipelines, monitoring systems, and infrastructure management.

## Features

- **CI/CD Pipeline**
  - Automated testing (unit, integration, model validation)
  - Automated model training on new data
  - Model registry integration
  - Automated deployment to staging/production
  - Rollback capabilities

- **Infrastructure as Code**
  - Terraform configurations for cloud resources
  - Kubernetes manifests for orchestration
  - Helm charts for deployment
  - Auto-scaling configurations
  - Resource management and cost optimization

- **Monitoring & Observability**
  - Prometheus metrics collection
  - Grafana dashboards
  - Model performance monitoring
  - Data drift detection
  - API latency and error tracking
  - Resource usage monitoring
  - Alerting system (Slack/email notifications)
  - Comprehensive logging and tracing

- **Security**
  - Network security (VPC, security groups, network policies)
  - RBAC (Kubernetes and AWS IAM)
  - Secrets management
  - Access control

- **Data Pipeline Integration**
  - ETL pipeline for training data
  - Data validation and quality checks
  - Automated retraining triggers
  - Data versioning

## Quick Start

### Prerequisites

- Terraform 1.0+
- Kubernetes cluster (or Docker for local)
- Prometheus & Grafana (for monitoring)
- Access to cloud provider (AWS/Azure/GCP)

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd mlops-infrastructure

# Initialize Terraform
cd infrastructure/terraform
terraform init

# Set up monitoring stack
cd ../../monitoring
docker-compose up -d
```

## CI/CD Pipeline

The CI/CD pipeline automatically:
1. Runs tests on code changes
2. Validates model performance
3. Validates data quality
4. Builds Docker images
5. Deploys to staging
6. Runs integration tests
7. Promotes to production on approval

### Running Locally

```bash
# Run CI pipeline locally
./ci-cd/scripts/run-pipeline.sh

# Run specific stage
./ci-cd/scripts/run-pipeline.sh --stage test
```

## Infrastructure

### Terraform Configuration

```bash
# Plan infrastructure changes
cd infrastructure/terraform
terraform plan

# Apply infrastructure
terraform apply

# Destroy infrastructure
terraform destroy
```

### Kubernetes Deployment

```bash
# Apply Kubernetes manifests
kubectl apply -f infrastructure/kubernetes/

# Deploy with Helm
helm install ml-platform ./infrastructure/kubernetes/helm/ml-platform

# Check deployment status
kubectl get deployments
kubectl get services
```

## Monitoring

### Prometheus Metrics

The system exposes metrics for:
- Model inference latency
- Request rates and errors
- Resource utilization
- Model performance metrics
- Data quality scores

### Grafana Dashboards

Pre-configured dashboards for:
- Model performance overview
- API health and latency
- Resource usage
- Data drift detection
- Error rates and alerts
- Cost tracking

### Accessing Dashboards

```bash
# Start monitoring stack
docker-compose -f monitoring/docker-compose.yml up -d

# Access Grafana
open http://localhost:3000
# Default credentials: admin/admin
```

## Project Structure

```
mlops-infrastructure/
├── ci-cd/            # CI/CD pipeline configurations
├── infrastructure/   # Terraform and Kubernetes configs
├── monitoring/       # Prometheus and Grafana setup
├── security/         # Security configurations
├── data-pipeline/   # ETL and validation pipelines
├── tests/           # Infrastructure tests
└── docs/            # Documentation (including learning guide)
```

## Documentation

- **[Learning Guide](docs/learn.md)** - Complete step-by-step learning path
- [Kubernetes Guide](docs/kubernetes_guide.md)
- [CI/CD Guide](docs/cicd_guide.md)
- [Monitoring Guide](docs/monitoring_guide.md)
- [Security Guide](docs/security_guide.md)

## Testing

```bash
# Run infrastructure tests
pytest tests/infrastructure/

# Test CI/CD pipeline
./ci-cd/scripts/test-pipeline.sh

# Validate Terraform
terraform validate
```

## License

MIT License
