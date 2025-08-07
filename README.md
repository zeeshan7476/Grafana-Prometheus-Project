# Grafana-Prometheus-Project
# 📊 Kubernetes Monitoring Stack with Prometheus & Grafana on AWS

This project sets up a full monitoring stack using **Prometheus**, **Grafana**, and **Alertmanager** on a Kubernetes cluster running in an **AWS EC2 instance**. It uses **Helm charts**, and exposes the services using **NodePorts**.

---

## 🚀 Tech Stack

- ☁️ AWS EC2 (Ubuntu)
- ☸️ Kubernetes (Kind cluster)
- 📦 Helm
- 🔧 Prometheus
- 📊 Grafana
- 🚨 Alertmanager
- 🐳 Docker

---

## 🛠️ Setup Steps

### 1. System Update and Docker Setup

```bash
sudo apt-get update

2. Clone the Project
git clone https://github.com/LondeShubham153/k8s-kind-voting-app.git
cd k8s-kind-voting-app
sudo usermod -aG docker $USER && newgrp docker

3. Helm Repos and Installation

# Add Helm repositories
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update

# Create monitoring namespace
kubectl create namespace monitoring

# Install Prometheus & Grafana stack
helm install kind-prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set prometheus.service.nodePort=30000 \
  --set prometheus.service.type=NodePort \
  --set grafana.service.nodePort=31000 \
  --set grafana.service.type=NodePort \
  --set alertmanager.service.nodePort=32000 \
  --set alertmanager.service.type=NodePort \
  --set prometheus-node-exporter.service.type=NodePort

🔍 Accessing Dashboards
Get Grafana Admin Password
kubectl -n monitoring get secrets kind-prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d ; echo
Port Forward Grafana

POD_NAME=$(kubectl -n monitoring get pod -l "app.kubernetes.io/name=grafana" -o name)
kubectl -n monitoring port-forward $POD_NAME 3000
Then access Grafana at:
📍 http://localhost:3000
🧑‍💻 Default Username: admin
🔐 Password: (from above command)

📈 Kubernetes Services (Example)
| Service                      | Port  |
| ---------------------------- | ----- |
| kind-prometheus-grafana      | 31000 |
| kind-prometheus-alertmanager | 32000 |
| kind-prometheus-prometheus   | 30000 |

📂 Files Included
README.md: This file

kube-prometheus-stack values set via CLI

monitoring/ namespace configuration (via kubectl)

docker and helm setup in script form

🛡️ License
This project is open-source and available under the MIT License.


