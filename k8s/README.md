# GDB Kubernetes Deployment Guide

## Prerequisites
- kubectl configured with cluster access
- nginx ingress controller installed
- metrics-server installed (for HPA)

## Deploy Order

```bash
# 1. Namespace
kubectl apply -f namespace/namespace.yml

# 2. Secrets & ConfigMaps
kubectl apply -f secrets/secrets.yml
kubectl apply -f configmaps/configmap.yml

# 3. Database
kubectl apply -f databases/postgres.yml
kubectl wait --for=condition=ready pod -l app=postgres -n gdb --timeout=120s

# 4. Service Discovery
kubectl apply -f services/eureka-server.yml
kubectl wait --for=condition=ready pod -l app=eureka-server -n gdb --timeout=120s

# 5. Gateway
kubectl apply -f services/gateway-service.yml

# 6. All microservices
kubectl apply -f services/auth-service.yml
kubectl apply -f services/microservices.yml

# 7. Frontend
kubectl apply -f services/frontend.yml

# 8. Ingress
kubectl apply -f ingress/ingress.yml
```

## Verify Deployment

```bash
# Check all pods
kubectl get pods -n gdb

# Check services
kubectl get svc -n gdb

# Check ingress
kubectl get ingress -n gdb

# Check HPA
kubectl get hpa -n gdb

# View logs
kubectl logs -f deployment/auth-service -n gdb
```

## Rolling Update

```bash
kubectl set image deployment/auth-service \
  auth-service=gdb/auth-service:v2 -n gdb

kubectl rollout status deployment/auth-service -n gdb
```

## Rollback

```bash
kubectl rollout undo deployment/auth-service -n gdb
```
