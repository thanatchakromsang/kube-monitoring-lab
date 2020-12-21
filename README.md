# kube-monitoring-lab

Learn monitoring on kubernetes the right way by doing using OSS tools

## Prerequisites

- Minikube
- Helm 3+

## Setup Minikube

Setup Minikube 2 node cluster with docker driver and switched to minikube kubectl context

```bash
minikube start --driver=docker
minikube update-context
```

## Install Helm charts

Add required helm charts for it to be available during charts installation

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts

helm repo update
```

Create monitoring namespace

```bash
kubectl create namespace monitoring
```

install or update required charts with overrided values for labs

```bash
# Install loki-stack
helm upgrade --install loki \
  grafana/loki-stack \
  --namespace monitoring \
  -f charts/loki-stack/values-override.yaml

# Install jaeger
helm upgrade --install jaeger \
  jaegertracing/jaeger \
  --namespace monitoring \
  -f charts/jaeger/values-override.yaml

# Apply Grafana datasources
kubectl apply -f charts/jaeger/datasources.yaml

# Install kube-prometheus-stack
helm upgrade --install kube-prometheus-stack \
  prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  -f charts/kube-prometheus-stack/values-override.yaml
```
