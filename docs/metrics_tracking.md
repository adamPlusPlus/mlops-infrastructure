# Metrics Tracking Guide

This guide explains how to implement comprehensive metrics tracking for the MLOps Infrastructure project to demonstrate production readiness and operational excellence.

## Overview

Tracking infrastructure metrics is crucial for:
- **Job Applications:** Show you understand production operations
- **Portfolio:** Demonstrate monitoring and observability skills
- **Interviews:** Provide examples of infrastructure optimization
- **Learning:** Understand system behavior and bottlenecks

## Metrics to Track

### 1. CI/CD Pipeline Metrics

#### Pipeline Performance

**What to Track:**
- Pipeline execution time
- Pipeline success/failure rate
- Deployment frequency
- Mean time to recovery (MTTR)
- Build time
- Test execution time

**Implementation:**

```yaml
# ci-cd/.github/workflows/metrics.yml
name: CI/CD Metrics

on:
  workflow_run:
    workflows: ["CI Pipeline", "CD Pipeline"]
    types: [completed]

jobs:
  track-metrics:
    runs-on: ubuntu-latest
    steps:
      - name: Track Pipeline Metrics
        run: |
          # Send metrics to Prometheus
          curl -X POST http://prometheus:9091/metrics/job/ci_cd \
            -d "pipeline_duration_seconds{workflow='${{ github.workflow }}', status='${{ github.event.workflow_run.conclusion }}'} ${{ github.event.workflow_run.conclusion == 'success' && '1' || '0' }}"
```

**Prometheus Metrics:**

```python
# ci-cd/scripts/metrics.py
from prometheus_client import Counter, Histogram, Gauge
import time

# Pipeline metrics
pipeline_runs_total = Counter(
    'cicd_pipeline_runs_total',
    'Total pipeline runs',
    ['pipeline_name', 'status']
)

pipeline_duration = Histogram(
    'cicd_pipeline_duration_seconds',
    'Pipeline execution time',
    ['pipeline_name', 'stage'],
    buckets=[10, 30, 60, 120, 300, 600, 1800]
)

deployment_frequency = Gauge(
    'cicd_deployments_total',
    'Total deployments',
    ['environment']
)

mttr_seconds = Histogram(
    'cicd_mttr_seconds',
    'Mean time to recovery',
    buckets=[60, 300, 600, 1800, 3600]
)
```

**Key Metrics to Report:**
- Pipeline success rate: `rate(cicd_pipeline_runs_total{status="success"}[7d]) / rate(cicd_pipeline_runs_total[7d])`
- Average pipeline time: `rate(cicd_pipeline_duration_seconds_sum[7d]) / rate(cicd_pipeline_duration_seconds_count[7d])`
- Deployment frequency: `increase(cicd_deployments_total[7d])`

---

### 2. Kubernetes Metrics

#### Cluster and Pod Metrics

**What to Track:**
- Pod count by namespace
- Pod restarts
- Resource requests vs limits
- Node utilization
- Pod scheduling failures

**Implementation:**

```yaml
# monitoring/prometheus/k8s-metrics.yml
# Prometheus already collects these via kube-state-metrics

# Key metrics available:
# - kube_pod_status_phase
# - kube_pod_container_status_restarts_total
# - kube_pod_container_resource_requests
# - kube_pod_container_resource_limits
# - kube_node_status_condition
```

**Custom Application Metrics:**

```python
# infrastructure/kubernetes/base/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ml-api
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/port: "8000"
    prometheus.io/path: "/metrics"
spec:
  # ... deployment spec
```

**Key Metrics to Report:**
- Pod uptime: `time() - kube_pod_created{namespace="default"}`
- Restart count: `sum(kube_pod_container_status_restarts_total)`
- Resource utilization: `(sum(kube_pod_container_resource_requests) / sum(kube_node_capacity)) * 100`

---

### 3. Infrastructure Cost Metrics

#### Cloud Resource Costs

**What to Track:**
- Daily infrastructure costs
- Cost by resource type (ECS, EKS, RDS, etc.)
- Cost trends over time
- Cost per deployment
- Reserved instance savings

**Implementation:**

```python
# monitoring/cost/cost_monitor.py
import boto3
from datetime import datetime, timedelta
from prometheus_client import Gauge
import time

# Cost metrics
infrastructure_daily_cost = Gauge(
    'aws_infrastructure_daily_cost_usd',
    'Daily infrastructure cost'
)

cost_by_resource = Gauge(
    'aws_cost_by_resource_usd',
    'Cost by resource type',
    ['resource_type', 'service']
)

cost_per_deployment = Gauge(
    'aws_cost_per_deployment_usd',
    'Cost per deployment'
)

class InfrastructureCostTracker:
    def __init__(self):
        self.ce_client = boto3.client('ce')
    
    def update_metrics(self):
        """Update infrastructure cost metrics"""
        # Get daily costs
        end = datetime.now()
        start = end - timedelta(days=1)
        
        response = self.ce_client.get_cost_and_usage(
            TimePeriod={
                'Start': start.strftime('%Y-%m-%d'),
                'End': end.strftime('%Y-%m-%d')
            },
            Granularity='DAILY',
            Metrics=['BlendedCost'],
            Filter={
                'Tags': {
                    'Key': 'Project',
                    'Values': ['mlops-infrastructure']
                }
            }
        )
        
        if response['ResultsByTime']:
            cost = float(response['ResultsByTime'][0]['Total']['BlendedCost']['Amount'])
            infrastructure_daily_cost.set(cost)
        
        # Get costs by service
        response = self.ce_client.get_cost_and_usage(
            TimePeriod={
                'Start': (datetime.now() - timedelta(days=30)).strftime('%Y-%m-%d'),
                'End': datetime.now().strftime('%Y-%m-%d')
            },
            Granularity='MONTHLY',
            Metrics=['BlendedCost'],
            GroupBy=[
                {'Type': 'DIMENSION', 'Key': 'SERVICE'},
                {'Type': 'TAG', 'Key': 'ResourceType'}
            ]
        )
        
        for group in response['ResultsByTime'][0]['Groups']:
            service = group['Keys'][0]
            resource_type = group['Keys'][1] if len(group['Keys']) > 1 else 'unknown'
            cost = float(group['Metrics']['BlendedCost']['Amount'])
            cost_by_resource.labels(resource_type=resource_type, service=service).set(cost)
```

**Key Metrics to Report:**
- Daily infrastructure cost: `aws_infrastructure_daily_cost_usd`
- Monthly cost: `sum(aws_cost_by_resource_usd) * 30`
- Cost by service: `sum by (service) (aws_cost_by_resource_usd)`

---

### 4. Monitoring System Metrics

#### Prometheus and Grafana Health

**What to Track:**
- Prometheus scrape success rate
- Grafana dashboard load time
- Alert firing rate
- Metrics cardinality
- Storage usage

**Implementation:**

```yaml
# monitoring/prometheus/alerts.yml
groups:
  - name: monitoring_health
    rules:
      - alert: PrometheusTargetDown
        expr: up == 0
        for: 5m
        annotations:
          summary: "Prometheus target is down"
      
      - alert: HighScrapeFailureRate
        expr: rate(prometheus_target_scrapes_exceeded_sample_limit_total[5m]) > 0.1
        annotations:
          summary: "High scrape failure rate"
      
      - alert: GrafanaSlow
        expr: histogram_quantile(0.95, rate(grafana_http_request_duration_seconds_bucket[5m])) > 2
        annotations:
          summary: "Grafana is slow"
```

**Key Metrics to Report:**
- Targets monitored: `count(up == 1)`
- Scrape success rate: `rate(prometheus_target_scrapes_exceeded_sample_limit_total[5m])`
- Alert firing rate: `rate(alertmanager_alerts_received_total[5m])`

---

### 5. Auto-Scaling Metrics

#### HPA and Cluster Autoscaler

**What to Track:**
- Scaling events
- Desired vs actual replicas
- Scaling decision latency
- Resource pressure events

**Implementation:**

```yaml
# infrastructure/kubernetes/base/hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: ml-api-hpa
  annotations:
    prometheus.io/scrape: "true"
spec:
  # HPA automatically exposes metrics:
  # - kube_horizontalpodautoscaler_status_current_replicas
  # - kube_horizontalpodautoscaler_status_desired_replicas
  # - kube_horizontalpodautoscaler_spec_max_replicas
```

**Custom Scaling Metrics:**

```python
# monitoring/scaling/scaling_metrics.py
from prometheus_client import Counter, Gauge

scaling_events_total = Counter(
    'hpa_scaling_events_total',
    'Total HPA scaling events',
    ['direction', 'reason']  # direction: up, down; reason: cpu, memory
)

current_replicas = Gauge(
    'hpa_current_replicas',
    'Current number of replicas',
    ['deployment']
)

desired_replicas = Gauge(
    'hpa_desired_replicas',
    'Desired number of replicas',
    ['deployment']
)
```

**Key Metrics to Report:**
- Scaling events: `sum(increase(hpa_scaling_events_total[7d]))`
- Average replicas: `avg_over_time(hpa_current_replicas[7d])`
- Scaling frequency: `rate(hpa_scaling_events_total[1h])`

---

### 6. Data Pipeline Metrics

#### ETL and Data Quality

**What to Track:**
- Pipeline execution time
- Data volume processed
- Data quality scores
- Pipeline failures
- Retraining triggers

**Implementation:**

```python
# data-pipeline/metrics/pipeline_metrics.py
from prometheus_client import Counter, Histogram, Gauge

pipeline_runs_total = Counter(
    'data_pipeline_runs_total',
    'Total pipeline runs',
    ['pipeline_name', 'status']
)

pipeline_duration = Histogram(
    'data_pipeline_duration_seconds',
    'Pipeline execution time',
    ['pipeline_name']
)

data_volume_processed = Counter(
    'data_volume_processed_bytes',
    'Total data processed',
    ['pipeline_name']
)

data_quality_score = Gauge(
    'data_quality_score',
    'Data quality score (0-1)',
    ['pipeline_name', 'check_type']
)

retraining_triggers_total = Counter(
    'retraining_triggers_total',
    'Total retraining triggers',
    ['trigger_type']  # schedule, drift, performance
)
```

**Key Metrics to Report:**
- Pipeline success rate: `rate(data_pipeline_runs_total{status="success"}[7d]) / rate(data_pipeline_runs_total[7d])`
- Data processed: `sum(increase(data_volume_processed_bytes[7d]))`
- Average quality score: `avg(data_quality_score)`

---

## Grafana Dashboard Configuration

### Key Dashboards to Create

**1. CI/CD Pipeline Dashboard:**
```json
{
  "panels": [
    {
      "title": "Pipeline Success Rate",
      "targets": [{
        "expr": "rate(cicd_pipeline_runs_total{status=\"success\"}[7d]) / rate(cicd_pipeline_runs_total[7d])"
      }]
    },
    {
      "title": "Average Pipeline Duration",
      "targets": [{
        "expr": "rate(cicd_pipeline_duration_seconds_sum[7d]) / rate(cicd_pipeline_duration_seconds_count[7d])"
      }]
    },
    {
      "title": "Deployment Frequency",
      "targets": [{
        "expr": "increase(cicd_deployments_total[7d])"
      }]
    }
  ]
}
```

**2. Infrastructure Health Dashboard:**
```json
{
  "panels": [
    {
      "title": "Pod Uptime",
      "targets": [{
        "expr": "time() - kube_pod_created"
      }]
    },
    {
      "title": "Resource Utilization",
      "targets": [{
        "expr": "(sum(kube_pod_container_resource_requests) / sum(kube_node_capacity)) * 100"
      }]
    },
    {
      "title": "Scaling Events",
      "targets": [{
        "expr": "sum(increase(hpa_scaling_events_total[1h]))"
      }]
    }
  ]
}
```

**3. Cost Tracking Dashboard:**
```json
{
  "panels": [
    {
      "title": "Daily Infrastructure Cost",
      "targets": [{
        "expr": "aws_infrastructure_daily_cost_usd"
      }]
    },
    {
      "title": "Cost by Service",
      "targets": [{
        "expr": "sum by (service) (aws_cost_by_resource_usd)"
      }]
    }
  ]
}
```

---

## Metrics to Report for Job Applications

### Key Numbers to Track

**CI/CD Metrics:**
- Pipeline success rate: `XX%`
- Average pipeline time: `X minutes`
- Deployment frequency: `X deployments/week`
- Mean time to recovery: `X minutes`

**Infrastructure Metrics:**
- Pod uptime: `XX days average`
- Resource utilization: `XX%`
- Scaling events: `XX events/week`
- Cluster availability: `99.X%`

**Cost Metrics:**
- Daily infrastructure cost: `$X.XX`
- Monthly cost: `$XXX.XX`
- Cost per deployment: `$X.XX`
- Cost optimization savings: `XX%`

**Monitoring Metrics:**
- Targets monitored: `XX targets`
- Alert firing rate: `X alerts/day`
- Dashboard load time: `X ms`

**Data Pipeline Metrics:**
- Pipeline success rate: `XX%`
- Data processed: `X GB/week`
- Average quality score: `0.XX`

---

## Implementation Checklist

- [ ] Set up Prometheus for CI/CD metrics
- [ ] Configure kube-state-metrics for K8s metrics
- [ ] Implement cost tracking (AWS Cost Explorer)
- [ ] Create Grafana dashboards
- [ ] Set up alerting rules
- [ ] Track data pipeline metrics
- [ ] Document key metrics in README
- [ ] Create metrics summary for portfolio

---

## Example Metrics Summary for Portfolio

```markdown
## Infrastructure Metrics (Last 30 Days)

- **Pipeline Success Rate:** 98.5%
- **Average Pipeline Time:** 8.5 minutes
- **Deployment Frequency:** 12 deployments/week
- **Mean Time to Recovery:** 15 minutes
- **Pod Uptime:** 99.8% average
- **Resource Utilization:** 65% average
- **Scaling Events:** 45 events
- **Daily Infrastructure Cost:** $12.50
- **Monthly Cost:** $375.00
- **Targets Monitored:** 25
- **Data Processed:** 2.3 TB
```

---

## Next Steps

1. **Set up Prometheus** for metrics collection
2. **Configure Grafana** dashboards
3. **Implement cost tracking** via AWS Cost Explorer
4. **Track for 2-4 weeks** to get real numbers
5. **Document metrics** in your portfolio
6. **Use in interviews** as concrete examples

**Remember:** Infrastructure metrics show operational maturity. Even small numbers demonstrate you understand production systems.

