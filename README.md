# 🐙 Squid Proxy on Kubernetes

This repository contains a production-ready deployment of [Squid Proxy](http://www.squid-cache.org/) in a Kubernetes cluster. Squid is used as a forward proxy to manage and control outbound HTTP/HTTPS/FTP traffic from pods.

---

## 📌 Features

- Deploys Squid proxy as a containerized application in Kubernetes
- Configurable via Kubernetes ConfigMap
- Supports logging to GCP via annotations
- Environment variables (`HTTP_PROXY`, `HTTPS_PROXY`, etc.) can be consumed by other workloads
- Supports basic access rules and DNS optimizations

---

## 🗂️ Directory Structure
.
├── squid-config.yaml # ConfigMap with squid.conf
├── squid-deployment.yaml # Squid Deployment manifest
├── squid-service.yaml # Service manifest (ClusterIP or LoadBalancer)
├── squid-test-deployment.yaml # Test proxy working
└── README.md # This file

---

## 🚀 Deployment

### 1. Create Namespace (optional)
```bash
kubectl create namespace squid
```
### 2. Apply ConfigMap
```bash
kubectl apply -f squid-config.yaml -n squid
```
### 3. Deploy Squid Proxy
```bash
kubectl apply -f squid-deployment.yaml -n squid
```
### 4. Expose Squid as a Service
```bash
kubectl apply -f squid-service.yaml -n squid
```

## 🌐 Using the Proxy

To make your pods use the proxy, set the following environment variables in their Deployment:
```yaml
env:
- name: HTTP_PROXY
  value: "http://squid-proxy-svc.squid.svc.cluster.local:3128"
- name: HTTPS_PROXY
  value: "http://squid-proxy-svc.squid.svc.cluster.local:3128"
- name: FTP_PROXY
  value: "http://squid-proxy-svc.squid.svc.cluster.local:3128"
- name: NO_PROXY
  value: "localhost,127.0.0.1,10.0.0.0/8,172.16.0.0/12,192.168.0.0/16,.svc,.cluster.local"
```

