# Restaurant Infrastructure - Kubernetes

This directory contains the Kubernetes manifests to deploy the restaurant microservices.

## Prerequisites

- A Kubernetes cluster (Minikube, Kind, Docker Desktop, or a cloud provider).
- `kubectl` installed and configured.
- Docker images built for each service:
  - `restaurant-auth:latest`
  - `restaurant-orders:latest`
  - `restaurant-payments:latest`
  - `restaurant-api:latest`

  *Note: If using Minikube or Kind, make sure to load the images into the cluster or use a registry.*

## Deployment Order

Apply the files in the following order:

1. **Namespace**
   ```bash
   kubectl apply -f k8s/00-namespace.yaml
   ```

2. **Configuration & Secrets**
   *Edit `k8s/01-config-secrets.yaml` to set your actual secrets (Base64 encoded) and configuration.*
   ```bash
   kubectl apply -f k8s/01-config-secrets.yaml
   ```

3. **Infrastructure Dependencies**
   ```bash
   kubectl apply -f k8s/02-postgres.yaml
   kubectl apply -f k8s/03-redis.yaml
   kubectl apply -f k8s/04-kafka.yaml
   ```

4. **Microservices**
   ```bash
   kubectl apply -f k8s/10-auth.yaml
   kubectl apply -f k8s/11-orders.yaml
   kubectl apply -f k8s/12-payments.yaml
   ```

5. **API Gateway**
   ```bash
   kubectl apply -f k8s/13-api-gateway.yaml
   ```

## Accessing the Application

The API Gateway is exposed via a LoadBalancer service on port 3000.

- **Localhost (Docker Desktop/Minikube tunnel):** `http://localhost:3000`
- **Minikube:** Run `minikube service api-gateway-service -n restaurant-app` to get the URL.

The Frontend (running locally) should be configured to point to this URL.
