# MLOps Infrastructure & CI/CD - Complete Learning Guide

**Goal:** Build production-ready MLOps infrastructure that demonstrates ALL Tier 1, Tier 2, and Tier 3 skills.

**What You'll Learn:**
- Kubernetes orchestration
- ML-specific CI/CD pipelines
- Prometheus + Grafana monitoring
- Terraform advanced patterns
- Network security (VPC, security groups)
- RBAC and access control
- Cost monitoring integration

**What You'll Demonstrate:**
- Complete Kubernetes deployment
- ML-aware CI/CD pipeline
- Production monitoring dashboards
- Infrastructure as Code
- Security implementation
- Automated retraining

---

## 📋 Learning Checklist

### Tier 1: Must Have (90% Job Guarantee)

- [ ] **Kubernetes** - Complete manifests, Helm charts, Kustomize, auto-scaling
- [ ] **ML-Specific CI/CD** - Model validation, data validation, performance gates
- [ ] **Workflow Orchestration** - AirFlow DAGs for ML pipelines
- [ ] **Prometheus + Grafana** - Actual dashboards, not just setup
- [ ] **Terraform Advanced** - Modules, state management
- [ ] **Network Security** - VPC, security groups, network policies

### Tier 2: High Value (95% Job Guarantee)

- [ ] **RBAC** - Role-based access control
- [ ] **Cost Monitoring** - Integration with AWS Cost Explorer
- [ ] **Alerting** - Model drift, performance degradation

### Tier 3: Differentiators (99% Top Offers)

- [ ] **Advanced Monitoring** - Custom metrics, distributed tracing
- [ ] **Cost Optimization** - Auto-scaling, resource right-sizing

---

## 🎯 Project Structure

```
mlops-infrastructure/
├── ci-cd/
│   ├── .github/
│   │   └── workflows/
│   │       ├── ci.yml              # CI pipeline
│   │       ├── cd.yml              # CD pipeline
│   │       └── ml-validation.yml    # ML-specific validation (Tier 1)
│   ├── scripts/
│   │   ├── validate_model.py        # Model validation (Tier 1)
│   │   ├── validate_data.py        # Data validation (Tier 1)
│   │   └── performance_gate.py    # Performance gates (Tier 1)
│   └── README.md
├── infrastructure/
│   ├── terraform/
│   │   ├── modules/                # Terraform modules (Tier 1)
│   │   │   ├── ecs/
│   │   │   ├── main.tf
│   │   │   ├── variables.tf
│   │   │   └── outputs.tf
│   │   │   ├── kubernetes/        # K8s module
│   │   │   └── monitoring/        # Monitoring module
│   │   ├── aws/
│   │   │   ├── main.tf
│   │   │   ├── vpc.tf              # VPC configuration (Tier 2)
│   │   │   ├── security_groups.tf  # Security groups (Tier 2)
│   │   │   └── iam.tf              # IAM roles (Tier 2)
│   │   └── README.md
│   ├── kubernetes/
│   │   ├── base/
│   │   │   ├── deployment.yaml     # K8s deployment (Tier 1)
│   │   │   ├── service.yaml        # K8s service (Tier 1)
│   │   │   ├── configmap.yaml      # ConfigMap (Tier 1)
│   │   │   ├── secret.yaml         # Secret template (Tier 1)
│   │   │   └── ingress.yaml        # Ingress (Tier 1)
│   │   ├── overlays/
│   │   │   ├── staging/
│   │   │   └── production/
│   │   ├── helm/
│   │   │   └── ml-platform/        # Helm chart (Tier 1)
│   │   │       ├── Chart.yaml
│   │   │       ├── values.yaml
│   │   │       └── templates/
│   │   └── README.md
│   └── docker-compose/
│       └── docker-compose.yml      # Local development
├── monitoring/
│   ├── prometheus/
│   │   ├── prometheus.yml          # Prometheus config (Tier 1)
│   │   ├── rules/
│   │   │   ├── model_alerts.yml    # Alerting rules (Tier 2)
│   │   │   └── performance.yml
│   │   └── README.md
│   ├── grafana/
│   │   ├── dashboards/
│   │   │   ├── model-performance.json  # Dashboard (Tier 1)
│   │   │   ├── api-health.json         # Dashboard (Tier 1)
│   │   │   ├── cost-tracking.json      # Cost dashboard (Tier 2/3)
│   │   │   └── data-drift.json         # Drift dashboard (Tier 1)
│   │   └── README.md
│   ├── cost/
│   │   ├── cost_monitor.py         # Cost monitoring (Tier 2)
│   │   └── alerts.py               # Cost alerts (Tier 2)
│   └── README.md
├── security/
│   ├── rbac/
│   │   ├── kubernetes-rbac.yaml    # K8s RBAC (Tier 2)
│   │   └── aws-iam.yaml            # AWS IAM (Tier 2)
│   ├── network/
│   │   ├── network-policies.yaml   # K8s network policies (Tier 2)
│   │   └── security-groups.tf      # AWS security groups (Tier 2)
│   └── README.md
├── data-pipeline/
│   ├── etl/
│   │   └── run_pipeline.py        # ETL pipeline
│   ├── validation/
│   │   ├── validate_data.py        # Data validation (Tier 1)
│   │   └── schema_validation.py
│   └── triggers/
│       └── retraining_trigger.py   # Auto-retraining (Tier 1)
├── tests/
│   ├── test_ci_cd.py
│   ├── test_kubernetes.py
│   └── test_monitoring.py
├── docs/
│   ├── LEARNING_GUIDE.md          # This file
│   ├── kubernetes_guide.md        # K8s learning guide
│   ├── cicd_guide.md              # CI/CD guide
│   ├── monitoring_guide.md        # Monitoring setup
│   └── security_guide.md          # Security implementation
├── requirements.txt
└── README.md
```

---

## 📚 Step-by-Step Learning Path

### Phase 1: Kubernetes (Week 1-2)

#### Step 1.1: Kubernetes Basics (1 week)

**Learning Resources:**
- Kubernetes.io tutorial: https://kubernetes.io/docs/tutorials/
- Time: 1 week

**Prerequisites:**
- [ ] Install kubectl
- [ ] Set up local cluster (minikube, kind, or Docker Desktop)
- [ ] Understand basic K8s concepts (pods, services, deployments)

**What to Build:**

**1. Deployment:**
```yaml
# infrastructure/kubernetes/base/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ml-api
  labels:
    app: ml-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ml-api
  template:
    metadata:
      labels:
        app: ml-api
    spec:
      containers:
      - name: ml-api
        image: <your-ecr-repo>/ml-training-deployment:latest
        ports:
        - containerPort: 8000
        env:
        - name: MLFLOW_TRACKING_URI
          valueFrom:
            configMapKeyRef:
              name: ml-config
              key: mlflow_uri
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: ml-secrets
              key: database_url
        resources:
          requests:
            cpu: "250m"
            memory: "512Mi"
          limits:
            cpu: "500m"
            memory: "1Gi"
        livenessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /health
            port: 8000
          initialDelaySeconds: 5
          periodSeconds: 5
```

**2. Service:**
```yaml
# infrastructure/kubernetes/base/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: ml-api-service
spec:
  selector:
    app: ml-api
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8000
  type: LoadBalancer
```

**3. ConfigMap:**
```yaml
# infrastructure/kubernetes/base/configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ml-config
data:
  mlflow_uri: "http://mlflow-service:5000"
  model_registry: "production"
  log_level: "INFO"
```

**4. Secret (Template):**
```yaml
# infrastructure/kubernetes/base/secret.yaml.example
apiVersion: v1
kind: Secret
metadata:
  name: ml-secrets
type: Opaque
stringData:
  database_url: "postgresql://user:password@host:5432/db"
  api_key: "your-api-key"
```

**Demonstration:**
- [ ] Create all K8s manifests
- [ ] Deploy to local cluster
- [ ] Verify pods are running
- [ ] Access service
- [ ] Scale deployment
- [ ] Update deployment (rolling update)

**Checkpoint:** Can you deploy to Kubernetes?

---

#### Step 1.2: Helm Charts (2-3 days) - Tier 1

**Learning Resources:**
- Helm docs: https://helm.sh/docs/
- Time: 2-3 days

**What to Build:**
```yaml
# infrastructure/kubernetes/helm/ml-platform/Chart.yaml
apiVersion: v2
name: ml-platform
description: MLOps Platform Helm Chart
version: 0.1.0
appVersion: "1.0"

# infrastructure/kubernetes/helm/ml-platform/values.yaml
replicaCount: 3

image:
  repository: <your-ecr-repo>/ml-training-deployment
  tag: "latest"
  pullPolicy: IfNotPresent

service:
  type: LoadBalancer
  port: 80

resources:
  requests:
    cpu: "250m"
    memory: "512Mi"
  limits:
    cpu: "500m"
    memory: "1Gi"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 80
```

**Deploy with Helm:**
```bash
# Install chart
helm install ml-platform ./infrastructure/kubernetes/helm/ml-platform

# Upgrade
helm upgrade ml-platform ./infrastructure/kubernetes/helm/ml-platform

# Uninstall
helm uninstall ml-platform
```

**Demonstration:**
- [ ] Create Helm chart
- [ ] Use values.yaml for configuration
- [ ] Deploy with Helm
- [ ] Upgrade deployment
- [ ] Document Helm usage

**Checkpoint:** Can you deploy with Helm?

---

#### Step 1.2b: Kustomize for Kubernetes (2-3 days) - Tier 2

**Learning Resources:**
- Kustomize Documentation: https://kustomize.io/
- Kubernetes Kustomize: https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/
- Time: 2-3 days

**Why This Matters:**
- Inworld AI and other job listings require Kustomize
- Alternative to Helm for Kubernetes deployments
- Demonstrates flexibility with different tools
- Better for GitOps workflows

**What to Build:**

**1. Base Kustomization:**
```yaml
# infrastructure/kubernetes/base/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: ml-platform

resources:
  - deployment.yaml
  - service.yaml
  - configmap.yaml
  - secret.yaml
  - ingress.yaml

commonLabels:
  app: ml-api
  managed-by: kustomize

images:
  - name: ml-api
    newName: <your-ecr-repo>/ml-training-deployment
    newTag: latest
```

**2. Overlay for Staging:**
```yaml
# infrastructure/kubernetes/overlays/staging/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: ml-platform-staging

bases:
  - ../../base

namePrefix: staging-

replicas:
  - name: ml-api
    count: 2

patchesStrategicMerge:
  - deployment-patch.yaml
  - configmap-patch.yaml

configMapGenerator:
  - name: ml-config
    behavior: merge
    literals:
      - ENVIRONMENT=staging
      - LOG_LEVEL=DEBUG
      - MLFLOW_TRACKING_URI=http://mlflow-staging:5000

commonLabels:
  environment: staging
```

**3. Overlay for Production:**
```yaml
# infrastructure/kubernetes/overlays/production/kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

namespace: ml-platform-production

bases:
  - ../../base

namePrefix: prod-

replicas:
  - name: ml-api
    count: 5

patchesStrategicMerge:
  - deployment-patch.yaml
  - hpa-patch.yaml

configMapGenerator:
  - name: ml-config
    behavior: merge
    literals:
      - ENVIRONMENT=production
      - LOG_LEVEL=INFO
      - MLFLOW_TRACKING_URI=http://mlflow-production:5000

commonLabels:
  environment: production

resources:
  - hpa.yaml
  - pdb.yaml  # Pod Disruption Budget
```

**4. Deployment Patch:**
```yaml
# infrastructure/kubernetes/overlays/production/deployment-patch.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ml-api
spec:
  template:
    spec:
      containers:
      - name: ml-api
        resources:
          requests:
            cpu: "500m"
            memory: "1Gi"
          limits:
            cpu: "2000m"
            memory: "4Gi"
        env:
        - name: ENVIRONMENT
          value: "production"
        - name: ENABLE_METRICS
          value: "true"
```

**5. HPA for Production:**
```yaml
# infrastructure/kubernetes/overlays/production/hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: prod-ml-api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: prod-ml-api
  minReplicas: 5
  maxReplicas: 20
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

**6. Build and Apply:**
```bash
# Build staging configuration
kubectl kustomize infrastructure/kubernetes/overlays/staging > staging-manifests.yaml

# Build production configuration
kubectl kustomize infrastructure/kubernetes/overlays/production > production-manifests.yaml

# Apply staging
kubectl apply -k infrastructure/kubernetes/overlays/staging

# Apply production
kubectl apply -k infrastructure/kubernetes/overlays/production

# Or use kubectl directly
kubectl apply -k infrastructure/kubernetes/overlays/staging
```

**7. CI/CD Integration:**
```yaml
# ci-cd/.github/workflows/kustomize-deploy.yml
name: Deploy with Kustomize

on:
  push:
    branches: [main]

jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up kubectl
      uses: azure/setup-kubectl@v3
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1
    
    - name: Deploy to staging
      run: |
        kubectl apply -k infrastructure/kubernetes/overlays/staging
  
  deploy-production:
    runs-on: ubuntu-latest
    needs: deploy-staging
    if: github.ref == 'refs/heads/main'
    environment: production
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up kubectl
      uses: azure/setup-kubectl@v3
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v2
    
    - name: Deploy to production
      run: |
        kubectl apply -k infrastructure/kubernetes/overlays/production
```

**Demonstration:**
- [ ] Create base Kustomization
- [ ] Create staging overlay
- [ ] Create production overlay
- [ ] Apply patches for different environments
- [ ] Build and apply configurations
- [ ] Integrate with CI/CD
- [ ] Compare Kustomize vs Helm
- [ ] Document when to use each

**Checkpoint:** Can you deploy with Kustomize?

---

#### Step 1.3: Auto-Scaling (2-3 days) - Tier 1

**What to Build:**
```yaml
# infrastructure/kubernetes/base/hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: ml-api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: ml-api
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 80
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
```

**Demonstration:**
- [ ] Create HPA
- [ ] Test auto-scaling (load test)
- [ ] Monitor scaling events
- [ ] Document scaling behavior

**Checkpoint:** Does auto-scaling work?

---

### Phase 2: ML-Specific CI/CD (Week 3-4)

#### Step 2.1: GitHub Actions CI/CD (1 week)

**Learning Resources:**
- GitHub Actions docs: https://docs.github.com/en/actions
- Time: 1 week

**What to Build:**

**1. CI Pipeline:**
```yaml
# ci-cd/.github/workflows/ci.yml
name: CI Pipeline

on:
  pull_request:
    branches: [main]
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.11'
    
    - name: Install dependencies
      run: |
        pip install -r requirements.txt
        pip install -r requirements-dev.txt
    
    - name: Run tests
      run: pytest tests/ --cov=. --cov-report=xml
    
    - name: Upload coverage
      uses: codecov/codecov-action@v3

  validate-model:
    runs-on: ubuntu-latest
    needs: test
    steps:
    - uses: actions/checkout@v3
    
    - name: Validate model performance
      run: |
        python ci-cd/scripts/validate_model.py \
          --model-path models/latest \
          --min-accuracy 0.85
    
    - name: Validate data quality
      run: |
        python ci-cd/scripts/validate_data.py \
          --data-path data/raw \
          --schema-path data/schema.json

  build:
    runs-on: ubuntu-latest
    needs: [test, validate-model]
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1
    
    - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1
    
    - name: Build and push Docker image
      env:
        ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
        IMAGE_TAG: ${{ github.sha }}
      run: |
        docker build -t $ECR_REGISTRY/ml-training-deployment:$IMAGE_TAG .
        docker push $ECR_REGISTRY/ml-training-deployment:$IMAGE_TAG
```

**2. CD Pipeline:**
```yaml
# ci-cd/.github/workflows/cd.yml
name: CD Pipeline

on:
  push:
    branches: [main]

jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1
    
    - name: Deploy to ECS (Staging)
      run: |
        aws ecs update-service \
          --cluster ml-cluster \
          --service ml-api-staging \
          --force-new-deployment

  integration-tests:
    runs-on: ubuntu-latest
    needs: deploy-staging
    steps:
    - name: Run integration tests
      run: |
        pytest tests/integration/ \
          --staging-url ${{ secrets.STAGING_URL }}

  deploy-production:
    runs-on: ubuntu-latest
    needs: integration-tests
    if: github.ref == 'refs/heads/main'
    environment:
      name: production
      url: https://ml-api.example.com
    steps:
    - name: Deploy to ECS (Production)
      run: |
        aws ecs update-service \
          --cluster ml-cluster \
          --service ml-api-production \
          --force-new-deployment
```

**3. Model Validation Script:**
```python
# ci-cd/scripts/validate_model.py
import argparse
import mlflow
import json

def validate_model(model_path, min_accuracy):
    """Validate model meets performance requirements"""
    model = mlflow.pyfunc.load_model(model_path)
    
    # Load test data
    # Run evaluation
    accuracy = evaluate_model(model)
    
    if accuracy < min_accuracy:
        print(f"ERROR: Model accuracy {accuracy} below threshold {min_accuracy}")
        exit(1)
    
    print(f"SUCCESS: Model accuracy {accuracy} meets threshold {min_accuracy}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--model-path", required=True)
    parser.add_argument("--min-accuracy", type=float, default=0.85)
    args = parser.parse_args()
    
    validate_model(args.model_path, args.min_accuracy)
```

**Demonstration:**
- [ ] Create CI pipeline
- [ ] Create CD pipeline
- [ ] Add model validation
- [ ] Add data validation
- [ ] Test pipeline
- [ ] Document CI/CD process

**Checkpoint:** Does CI/CD pipeline work?

---

#### Step 2.2: Apache AirFlow for ML Workflows (1 week) - Tier 1.5

**Learning Resources:**
- AirFlow Documentation: https://airflow.apache.org/docs/
- AirFlow Tutorial: https://airflow.apache.org/docs/apache-airflow/stable/tutorial/index.html
- Time: 1 week

**Why This Matters:**
- ConsultNet and many other job listings require AirFlow
- Industry standard for workflow orchestration
- More flexible than Step Functions for complex ML pipelines
- Demonstrates understanding of different orchestration tools

**What to Build:**

**1. AirFlow DAG for ML Pipeline:**
```python
# ci-cd/airflow/dags/ml_training_pipeline.py
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator
from airflow.providers.amazon.aws.operators.s3 import S3FileTransformOperator
from airflow.providers.postgres.operators.postgres import PostgresOperator
from airflow.utils.dates import days_ago
from datetime import timedelta
import mlflow

default_args = {
    'owner': 'mlops',
    'depends_on_past': False,
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
}

def validate_data(**context):
    """Validate training data"""
    import pandas as pd
    from airflow.providers.amazon.aws.hooks.s3 import S3Hook
    
    s3_hook = S3Hook(aws_conn_id='aws_default')
    
    # Download data
    data_path = s3_hook.download_file(
        key='data/raw/training_data.csv',
        bucket_name='ml-training-bucket'
    )
    
    # Validate
    df = pd.read_csv(data_path)
    
    # Check data quality
    assert len(df) > 1000, "Insufficient data"
    assert df.isnull().sum().sum() < len(df) * 0.1, "Too many nulls"
    
    # Push to XCom for next task
    context['ti'].xcom_push(key='data_path', value=data_path)
    return data_path

def train_model(**context):
    """Train ML model"""
    import mlflow
    import mlflow.sklearn
    from sklearn.ensemble import RandomForestClassifier
    from sklearn.model_selection import train_test_split
    import pandas as pd
    import pickle
    
    # Get data path from previous task
    ti = context['ti']
    data_path = ti.xcom_pull(key='data_path', task_ids='validate_data')
    
    # Load data
    df = pd.read_csv(data_path)
    
    # Prepare features
    X = df.drop('target', axis=1)
    y = df['target']
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
    
    # Train model
    mlflow.set_experiment("airflow-ml-pipeline")
    
    with mlflow.start_run():
        model = RandomForestClassifier(n_estimators=100)
        model.fit(X_train, y_train)
        
        # Evaluate
        accuracy = model.score(X_test, y_test)
        
        # Log to MLflow
        mlflow.log_param("n_estimators", 100)
        mlflow.log_metric("accuracy", accuracy)
        mlflow.sklearn.log_model(model, "model")
        
        # Register model if accuracy is good
        if accuracy > 0.85:
            mlflow.register_model(
                f"runs:/{mlflow.active_run().info.run_id}/model",
                "production-model"
            )
        
        # Push results to XCom
        context['ti'].xcom_push(key='accuracy', value=accuracy)
        context['ti'].xcom_push(key='model_uri', 
                               value=f"runs:/{mlflow.active_run().info.run_id}/model")
    
    return accuracy

def validate_model(**context):
    """Validate model performance"""
    ti = context['ti']
    accuracy = ti.xcom_pull(key='accuracy', task_ids='train_model')
    
    if accuracy < 0.85:
        raise ValueError(f"Model accuracy {accuracy} below threshold 0.85")
    
    return accuracy

def deploy_model(**context):
    """Deploy model to production"""
    import boto3
    from airflow.providers.amazon.aws.hooks.s3 import S3Hook
    
    ti = context['ti']
    model_uri = ti.xcom_pull(key='model_uri', task_ids='train_model')
    
    # Load model from MLflow
    model = mlflow.pyfunc.load_model(model_uri)
    
    # Save to S3 for deployment
    s3_hook = S3Hook(aws_conn_id='aws_default')
    s3_hook.load_bytes(
        key='models/production/model.pkl',
        bucket_name='ml-models-bucket',
        data=pickle.dumps(model)
    )
    
    # Trigger deployment (e.g., update ECS service)
    ecs = boto3.client('ecs')
    ecs.update_service(
        cluster='ml-cluster',
        service='ml-api-service',
        forceNewDeployment=True
    )
    
    return "Model deployed"

# Define DAG
dag = DAG(
    'ml_training_pipeline',
    default_args=default_args,
    description='ML Training and Deployment Pipeline',
    schedule_interval=timedelta(days=1),  # Daily retraining
    start_date=days_ago(1),
    catchup=False,
    tags=['ml', 'training', 'deployment'],
)

# Define tasks
validate_data_task = PythonOperator(
    task_id='validate_data',
    python_callable=validate_data,
    dag=dag,
)

train_model_task = PythonOperator(
    task_id='train_model',
    python_callable=train_model,
    dag=dag,
)

validate_model_task = PythonOperator(
    task_id='validate_model',
    python_callable=validate_model,
    dag=dag,
)

deploy_model_task = PythonOperator(
    task_id='deploy_model',
    python_callable=deploy_model,
    dag=dag,
)

# Define task dependencies
validate_data_task >> train_model_task >> validate_model_task >> deploy_model_task
```

**2. AirFlow Configuration:**
```python
# ci-cd/airflow/config/airflow.cfg (excerpts)
[core]
dags_folder = /opt/airflow/dags
executor = LocalExecutor
sql_alchemy_conn = postgresql+psycopg2://airflow:airflow@postgres:5432/airflow
parallelism = 32
dag_concurrency = 16
max_active_runs_per_dag = 1

[scheduler]
job_heartbeat_sec = 5
scheduler_heartbeat_sec = 5

[webserver]
base_url = http://localhost:8080
web_server_port = 8080

[logging]
logging_level = INFO
```

**3. Docker Compose for AirFlow:**
```yaml
# ci-cd/airflow/docker-compose.yml
version: '3.8'

x-airflow-common:
  &airflow-common
  image: apache/airflow:2.8.0
  environment:
    &airflow-common-env
    AIRFLOW__CORE__EXECUTOR: LocalExecutor
    AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
    AIRFLOW__CORE__FERNET_KEY: ''
    AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION: 'true'
    AIRFLOW__CORE__LOAD_EXAMPLES: 'false'
    AIRFLOW__API__AUTH_BACKENDS: 'airflow.api.auth.backend.basic_auth'
    _PIP_ADDITIONAL_REQUIREMENTS: ${_PIP_ADDITIONAL_REQUIREMENTS:-}
  volumes:
    - ./dags:/opt/airflow/dags
    - ./logs:/opt/airflow/logs
    - ./plugins:/opt/airflow/plugins
  user: "${AIRFLOW_UID:-50000}:0"
  depends_on:
    &airflow-common-depends-on
    postgres:
      condition: service_healthy

services:
  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow
    volumes:
      - postgres-db-volume:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "airflow"]
      interval: 10s
      retries: 5
      timeout: 5s
    restart: always

  airflow-webserver:
    <<: *airflow-common
    command: webserver
    ports:
      - "8080:8080"
    healthcheck:
      test: ["CMD", "curl", "--fail", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-scheduler:
    <<: *airflow-common
    command: scheduler
    healthcheck:
      test: ["CMD-SHELL", 'airflow jobs check --job-type SchedulerJob --hostname "$${HOSTNAME}"']
      interval: 30s
      timeout: 10s
      retries: 5
    restart: always
    depends_on:
      <<: *airflow-common-depends-on
      airflow-init:
        condition: service_completed_successfully

  airflow-init:
    <<: *airflow-common
    entrypoint: /bin/bash
    command:
      - -c
      - |
        function ver() {
          printf "%04d%04d%04d%04d" $${1//./ }
        }
        airflow_version=$$(AIRFLOW__LOGGING__LOGGING_LEVEL=INFO && airflow version)
        airflow_version_comparable=$$(ver $${airflow_version})
        min_airflow_version=2.2.0
        min_airflow_version_comparable=$$(ver $${min_airflow_version})
        if (( airflow_version_comparable < min_airflow_version_comparable )); then
          echo
          echo -e "\033[1;31mERROR!!!: Too old Airflow version $${airflow_version}!\033[0m"
          echo "Minimum Airflow version is $${min_airflow_version}. Please upgrade!"
          exit 1
        fi
        if [[ -z "$${AIRFLOW_UID}" ]]; then
          echo
          echo -e "\033[1;33mWARNING!!!: AIRFLOW_UID not set!\033[0m"
          echo "If you are on Linux, you SHOULD follow the instructions to set "
          echo "AIRFLOW_UID environment variable, otherwise files will be owned by root."
          echo "For other operating systems you can safely ignore this warning"
          echo
        fi
        one_meg=1048576
        mem_available=$$(($$(getconf _PHYS_PAGES) * $$(getconf PAGE_SIZE) / one_meg))
        cpus_available=$$(grep -cE 'cpu[0-9]+' /proc/stat)
        disk_available=$$(df / | tail -1 | awk '{print $$4}')
        warning_resources="false"
        if (( mem_available < 4000 )) ; then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough memory available for Docker.\033[0m"
          echo "At least 4GB of memory required. You have $$(numfmt --to iec $$((mem_available * one_meg)))"
          echo
          warning_resources="true"
        fi
        if (( cpus_available < 2 )); then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough CPUS available for Docker.\033[0m"
          echo "At least 2 CPUs recommended. You have $${cpus_available}"
          echo
          warning_resources="true"
        fi
        if (( disk_available < one_meg * 10 )); then
          echo
          echo -e "\033[1;33mWARNING!!!: Not enough Disk Space available for Docker.\033[0m"
          echo "At least 10 GBs recommended. You have $$(numfmt --to iec $$((disk_available * 1024 )))"
          echo
          warning_resources="true"
        fi
        if [[ "$${warning_resources}" == "true" ]]; then
          echo
          echo -e "\033[1;33mWARNING!!!: You have not enough resources to run Airflow (see above)!\033[0m"
          echo "Please follow the instructions to increase amount of resources available:"
          echo "   https://airflow.apache.org/docs/apache-airflow/stable/start/docker.html#before-you-begin"
          echo
        fi
        mkdir -p /sources/logs /sources/dags /sources/plugins
        chown -R "$${AIRFLOW_UID}:0" /sources/{logs,dags,plugins}
        exec /entrypoint airflow version
    environment:
      <<: *airflow-common-env
      _AIRFLOW_DB_MIGRATE: 'true'
      _AIRFLOW_WWW_USER_CREATE: 'true'
      _AIRFLOW_WWW_USER_USERNAME: ${_AIRFLOW_WWW_USER_USERNAME:-airflow}
      _AIRFLOW_WWW_USER_PASSWORD: ${_AIRFLOW_WWW_USER_PASSWORD:-airflow}
    user: "0:0"
    volumes:
      - ${AIRFLOW_PROJ_DIR:-.}:/sources

volumes:
  postgres-db-volume:
```

**4. AirFlow Sensors and Operators:**
```python
# ci-cd/airflow/dags/ml_pipeline_with_sensors.py
from airflow import DAG
from airflow.sensors.filesystem import FileSensor
from airflow.sensors.python import PythonSensor
from airflow.operators.python import PythonOperator
from airflow.providers.amazon.aws.sensors.s3 import S3KeySensor
from datetime import datetime, timedelta

def check_data_quality(**context):
    """Sensor to check data quality before processing"""
    import pandas as pd
    from airflow.providers.amazon.aws.hooks.s3 import S3Hook
    
    s3_hook = S3Hook(aws_conn_id='aws_default')
    data_path = s3_hook.download_file(
        key='data/raw/training_data.csv',
        bucket_name='ml-training-bucket'
    )
    
    df = pd.read_csv(data_path)
    
    # Check if data is ready
    if len(df) > 1000 and df.isnull().sum().sum() < len(df) * 0.1:
        return True
    return False

dag = DAG(
    'ml_pipeline_with_sensors',
    default_args=default_args,
    schedule_interval=timedelta(hours=6),
    start_date=days_ago(1),
)

# Sensor to wait for data file
wait_for_data = S3KeySensor(
    task_id='wait_for_data',
    bucket_name='ml-training-bucket',
    bucket_key='data/raw/training_data.csv',
    aws_conn_id='aws_default',
    timeout=3600,  # Wait up to 1 hour
    poke_interval=60,  # Check every minute
    dag=dag,
)

# Sensor to check data quality
check_quality = PythonSensor(
    task_id='check_data_quality',
    python_callable=check_data_quality,
    timeout=1800,
    poke_interval=300,
    dag=dag,
)

# Training task
train_task = PythonOperator(
    task_id='train_model',
    python_callable=train_model,
    dag=dag,
)

# Define dependencies
wait_for_data >> check_quality >> train_task
```

**5. Terraform for AirFlow on ECS:**
```hcl
# infrastructure/terraform/aws/airflow.tf
resource "aws_ecs_task_definition" "airflow_webserver" {
  family                   = "airflow-webserver"
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = "512"
  memory                   = "1024"
  
  container_definitions = jsonencode([{
    name  = "airflow-webserver"
    image = "${aws_ecr_repository.airflow.repository_url}:latest"
    portMappings = [{
      containerPort = 8080
      protocol      = "tcp"
    }]
    environment = [
      {
        name  = "AIRFLOW__CORE__EXECUTOR"
        value = "LocalExecutor"
      }
    ]
  }])
}

resource "aws_ecs_service" "airflow_webserver" {
  name            = "airflow-webserver"
  cluster         = aws_ecs_cluster.ml_cluster.id
  task_definition = aws_ecs_task_definition.airflow_webserver.arn
  desired_count   = 1
  launch_type     = "FARGATE"
}
```

**Demonstration:**
- [ ] Set up AirFlow locally with Docker Compose
- [ ] Create ML training DAG
- [ ] Implement data validation task
- [ ] Implement model training task
- [ ] Implement model deployment task
- [ ] Add sensors for data availability
- [ ] Schedule DAG for daily retraining
- [ ] Monitor DAG runs in AirFlow UI
- [ ] Compare AirFlow vs Step Functions
- [ ] Document workflow orchestration decisions

**Checkpoint:** Can you orchestrate ML workflows with AirFlow?

---

### Phase 3: Monitoring (Week 5-6)

#### Step 3.1: Prometheus Setup (2-3 days) - Tier 1

**What to Build:**

**1. Prometheus Configuration:**
```yaml
# monitoring/prometheus/prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'ml-api'
    static_configs:
      - targets: ['ml-api-service:8000']
    metrics_path: '/metrics'
  
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true

rule_files:
  - 'rules/*.yml'

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']
```

**2. Alert Rules:**
```yaml
# monitoring/prometheus/rules/model_alerts.yml
groups:
  - name: model_alerts
    rules:
      - alert: HighModelLatency
        expr: ml_api_request_duration_seconds{quantile="0.95"} > 1.0
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High model inference latency"
          description: "95th percentile latency is {{ $value }}s"
      
      - alert: ModelAccuracyDegraded
        expr: ml_model_accuracy < 0.80
        for: 10m
        labels:
          severity: critical
        annotations:
          summary: "Model accuracy degraded"
          description: "Model accuracy is {{ $value }}"
      
      - alert: DataDriftDetected
        expr: ml_data_drift_score > 0.1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Data drift detected"
          description: "Drift score is {{ $value }}"
```

**3. Deploy Prometheus:**
```yaml
# infrastructure/kubernetes/base/prometheus.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
      - name: prometheus
        image: prom/prometheus:latest
        ports:
        - containerPort: 9090
        volumeMounts:
        - name: config
          mountPath: /etc/prometheus
      volumes:
      - name: config
        configMap:
          name: prometheus-config
```

**Demonstration:**
- [ ] Deploy Prometheus
- [ ] Configure scraping
- [ ] Create alert rules
- [ ] Test alerts
- [ ] Document monitoring setup

**Checkpoint:** Is Prometheus collecting metrics?

---

#### Step 3.2: Grafana Dashboards (2-3 days) - Tier 1

**What to Build:**

**1. Model Performance Dashboard:**
```json
// monitoring/grafana/dashboards/model-performance.json
{
  "dashboard": {
    "title": "ML Model Performance",
    "panels": [
      {
        "title": "Model Accuracy",
        "targets": [{
          "expr": "ml_model_accuracy",
          "legendFormat": "Accuracy"
        }]
      },
      {
        "title": "Inference Latency (p95)",
        "targets": [{
          "expr": "histogram_quantile(0.95, ml_api_request_duration_seconds_bucket)",
          "legendFormat": "p95 Latency"
        }]
      },
      {
        "title": "Request Rate",
        "targets": [{
          "expr": "rate(ml_api_requests_total[5m])",
          "legendFormat": "Requests/sec"
        }]
      },
      {
        "title": "Error Rate",
        "targets": [{
          "expr": "rate(ml_api_errors_total[5m])",
          "legendFormat": "Errors/sec"
        }]
      }
    ]
  }
}
```

**2. Cost Tracking Dashboard:**
```json
// monitoring/grafana/dashboards/cost-tracking.json
{
  "dashboard": {
    "title": "ML Infrastructure Costs",
    "panels": [
      {
        "title": "Daily AWS Costs",
        "targets": [{
          "expr": "aws_cost_daily"
        }]
      },
      {
        "title": "Cost by Service",
        "targets": [{
          "expr": "aws_cost_by_service"
        }]
      },
      {
        "title": "Cost per Prediction",
        "targets": [{
          "expr": "aws_cost_daily / ml_api_requests_total"
        }]
      }
    ]
  }
}
```

**Deploy Grafana:**
```yaml
# infrastructure/kubernetes/base/grafana.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
        env:
        - name: GF_SECURITY_ADMIN_PASSWORD
          valueFrom:
            secretKeyRef:
              name: grafana-secrets
              key: admin-password
        volumeMounts:
        - name: dashboards
          mountPath: /etc/grafana/provisioning/dashboards
      volumes:
      - name: dashboards
        configMap:
          name: grafana-dashboards
```

**Demonstration:**
- [ ] Deploy Grafana
- [ ] Create model performance dashboard
- [ ] Create cost tracking dashboard
- [ ] Create API health dashboard
- [ ] Create data drift dashboard
- [ ] Document dashboards

**Checkpoint:** Do you have working dashboards?

---

### Phase 4: Security (Week 7)

#### Step 4.1: Network Security (2-3 days) - Tier 2

**What to Build:**

**1. VPC Configuration (Terraform):**
```hcl
# infrastructure/terraform/aws/vpc.tf
resource "aws_vpc" "ml_vpc" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true
  
  tags = {
    Name = "ml-vpc"
  }
}

resource "aws_subnet" "private" {
  vpc_id            = aws_vpc.ml_vpc.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
  
  tags = {
    Name = "ml-private-subnet"
  }
}

resource "aws_security_group" "ml_api" {
  name        = "ml-api-sg"
  description = "Security group for ML API"
  vpc_id      = aws_vpc.ml_vpc.id
  
  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
    description = "HTTPS from internet"
  }
  
  ingress {
    from_port   = 8000
    to_port     = 8000
    protocol    = "tcp"
    security_groups = [aws_security_group.internal.id]
    description = "Internal API access"
  }
  
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
    description = "Allow all outbound"
  }
}
```

**2. Kubernetes Network Policies:**
```yaml
# security/network/network-policies.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: ml-api-network-policy
spec:
  podSelector:
    matchLabels:
      app: ml-api
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: monitoring
    ports:
    - protocol: TCP
      port: 8000
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: database
    ports:
    - protocol: TCP
      port: 5432
```

**Demonstration:**
- [ ] Create VPC with private subnets
- [ ] Configure security groups
- [ ] Create network policies
- [ ] Test network isolation
- [ ] Document security setup

**Checkpoint:** Is network security configured?

---

#### Step 4.2: RBAC (2-3 days) - Tier 2

**What to Build:**

**1. Kubernetes RBAC:**
```yaml
# security/rbac/kubernetes-rbac.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: ml-api-sa
  namespace: default

---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: ml-api-role
rules:
- apiGroups: [""]
  resources: ["configmaps", "secrets"]
  verbs: ["get", "list"]
- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get", "list", "watch"]

---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: ml-api-rolebinding
subjects:
- kind: ServiceAccount
  name: ml-api-sa
  namespace: default
roleRef:
  kind: Role
  name: ml-api-role
  apiGroup: rbac.authorization.k8s.io
```

**2. AWS IAM:**
```hcl
# infrastructure/terraform/aws/iam.tf
resource "aws_iam_role" "ecs_task_role" {
  name = "ecs-task-role"
  
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ecs-tasks.amazonaws.com"
      }
    }]
  })
}

resource "aws_iam_role_policy" "ecs_secrets" {
  name = "ecs-secrets-policy"
  role = aws_iam_role.ecs_task_role.id
  
  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Action = [
        "secretsmanager:GetSecretValue"
      ]
      Resource = aws_secretsmanager_secret.ml_secrets.arn
    }]
  })
}
```

**Demonstration:**
- [ ] Create K8s ServiceAccount
- [ ] Create K8s Role and RoleBinding
- [ ] Create AWS IAM role
- [ ] Test access control
- [ ] Document RBAC setup

**Checkpoint:** Is RBAC configured?

---

## ✅ Final Demonstration Checklist

### Must Demonstrate (Tier 1):

- [ ] **Kubernetes:** Complete deployment with all resources
- [ ] **Helm:** Deploy with Helm charts
- [ ] **Auto-scaling:** HPA working
- [ ] **CI/CD:** ML-specific pipeline
- [ ] **Prometheus:** Collecting metrics
- [ ] **Grafana:** Working dashboards
- [ ] **Terraform:** Infrastructure modules

### Should Demonstrate (Tier 2):

- [ ] **Network Security:** VPC, security groups, network policies
- [ ] **RBAC:** K8s and AWS IAM
- [ ] **Cost Monitoring:** Integration with dashboards
- [ ] **Alerting:** Model drift, performance alerts

### Nice to Demonstrate (Tier 3):

- [ ] **Advanced Monitoring:** Distributed tracing
- [ ] **Cost Optimization:** Auto-scaling, right-sizing

---

## 📖 Documentation Requirements

Create these documents:

1. **docs/kubernetes_guide.md** - Complete K8s learning guide
2. **docs/cicd_guide.md** - ML-specific CI/CD guide
3. **docs/monitoring_guide.md** - Prometheus + Grafana setup
4. **docs/security_guide.md** - Network security and RBAC

---

## 🎓 Learning Resources Summary

| Topic | Resource | Time |
|-------|----------|------|
| Kubernetes | https://kubernetes.io/docs/tutorials/ | 1 week |
| Helm | https://helm.sh/docs/ | 2-3 days |
| GitHub Actions | https://docs.github.com/en/actions | 1 week |
| Prometheus | https://prometheus.io/docs/ | 2-3 days |
| Grafana | https://grafana.com/docs/ | 2-3 days |
| Network Security | AWS VPC docs | 2-3 days |
| RBAC | K8s RBAC docs | 2-3 days |

---

## 🚀 Getting Started

1. **Start with Phase 1** - Learn Kubernetes
2. **Use Cursor AI** - Generate manifests, ask questions
3. **Build incrementally** - One component at a time
4. **Test everything** - Verify each step works
5. **Document as you go** - Show what you learned

**Remember:** Infrastructure is complex. Take it step by step!

---

## 📝 Next Steps

After completing this project:
1. Move to **production-ml-platform** (Project 3)
2. Add feature store, advanced A/B testing
3. Integrate with Projects 1 & 2
4. Build complete production platform

**You're building something impressive!** 🎯
