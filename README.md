# k8s-cloudwatch

Kubernetes monitoring stack deployed on Minikube.
Covers Pods, Deployments, Services, Namespaces, ConfigMaps, Secrets, Ingress, and Helm.

## Stack
- Node.js App (cloudwatch-app) — :3001
- Prometheus — :9090
- Grafana — :3000
- cAdvisor (via Docker Compose version)

## Prerequisites
- Minikube running (`minikube start --driver=docker --cpus=2 --memory=3000`)
- kubectl configured
- Ingress addon enabled (`minikube addons enable ingress`)
- Helm installed

## Deploy with raw manifests

```bash
kubectl apply -f manifests/namespace.yaml
kubectl apply -f manifests/resourcequota.yaml
kubectl apply -f manifests/app/
kubectl apply -f manifests/prometheus/
kubectl apply -f manifests/grafana/
kubectl apply -f manifests/ingress.yaml
```

## Deploy with Helm (recommended)

```bash
kubectl create namespace monitoring
helm install cloudwatch helm/cloudwatch-chart
```

## Access services

Get Minikube IP:
```bash
minikube ip
```

Then open:
- App:        http://MINIKUBE_IP/app/hello
- Prometheus: http://MINIKUBE_IP/prometheus
- Grafana:    http://MINIKUBE_IP/grafana  (admin / cloudwatch123)

Or use port-forward:
```bash
kubectl port-forward service/grafana-service 3000:3000 -n monitoring
kubectl port-forward service/prometheus-service 9090:9090 -n monitoring
```

## Useful commands

```bash
kubectl get all -n monitoring
kubectl get ingress -n monitoring
kubectl logs deployment/grafana -n monitoring
kubectl describe ingress monitoring-ingress -n monitoring
helm list
helm upgrade cloudwatch helm/cloudwatch-chart
helm rollback cloudwatch 1
```

## Teardown

```bash
# Raw manifests
kubectl delete namespace monitoring

# Helm
helm uninstall cloudwatch
kubectl delete namespace monitoring

# Stop Minikube
minikube stop
```

## Author
Eljin — BS Computer Engineering, STI College Global City
