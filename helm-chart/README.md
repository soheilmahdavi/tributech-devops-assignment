 Architecture & Design
The solution uses a modular, umbrella-style Helm chart with the following subcharts:

Keycloak – Handles authentication. Deployed via the Bitnami Helm chart and configured to use PostgreSQL.

PostgreSQL – A relational database deployed using Bitnami’s PostgreSQL chart and consumed by Keycloak.

PGAdmin – Provides a UI to access and manage PostgreSQL.

Website – A Tributech-provided sample application that authenticates users via Keycloak.

All services are exposed using Kubernetes Ingress (NGINX) and configured via values.yaml files.

Component Interaction

User -> Website -> Keycloak (OAuth)
                   ↓
             PostgreSQL
             (user data)

Admin -> PGAdmin -> PostgreSQL

Local Setup & Testing
📦 Prerequisites
Docker

Minikube or Kind (tested on Minikube 1.29+)

Helm v3+

NGINX Ingress Controller installed

kubectl and helm CLI tools installed

Edit /etc/hosts on Windows or macOS to point .local domains to Minikube IP

Example /etc/hosts:

192.168.49.2 keycloak-dev.local
192.168.49.2 website.local
192.168.49.2 pgadmin.local

1. Start Minikube
minikube start --driver=docker
minikube addons enable ingress

2. Clone and install dependencies
git clone <your-fork-url>
cd tributech-devops-assignment/helm-chart
helm dependency update tributech

3. Deploy the stack
 helm install tributech-dev ./tributech -f tributech/values.yaml

4. Validate Components
kubectl get pods -A

Expected Pods:

tributech-dev-postgresql-0 – PostgreSQL

tributech-dev-keycloak-0 – Keycloak

tributech-dev-website-* – Sample webapp

tributech-dev-pgadmin-* – PGAdmin

Test URLs (after updating /etc/hosts):
Component | URL | Notes
Website | http://website.local | Should show sample UI
Keycloak | http://keycloak-dev.local | Login UI (admin user: user)
PGAdmin | http://pgadmin.local | DB management

Testing in Tributech Infrastructure

 Make sure the Kubernetes cluster has:

A running Ingress Controller

Default namespace or custom one specified

Secrets/Configs handled via values.yaml

Deployment Instructions
 helm upgrade --install tributech-dev ./tributech -f tributech/values.yaml --namespace your-namespace

If Ingress controller is already deployed, no changes needed. Otherwise, deploy NGINX ingress:
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install ingress-nginx ingress-nginx/ingress-nginx

