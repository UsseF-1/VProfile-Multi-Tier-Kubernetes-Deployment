# 📚 VProfile Kubernetes Deployment – Full Technical Documentation

---

# 🧭 1. Introduction

This document provides a **deep technical explanation** of the VProfile Kubernetes deployment project.

The goal of this project is to simulate a **production-grade, multi-tier application deployment** using Kubernetes, demonstrating real-world DevOps practices such as:

* Container orchestration
* Service discovery
* Persistent storage
* Secrets management
* Ingress-based routing

---

# 🏗️ 2. System Architecture

## 🔹 High-Level Architecture

```
User → Ingress → Service → Application Pod → Backend Services → Database (PVC)
```

## 🔹 Components

| Component   | Description                         |
| ----------- | ----------------------------------- |
| Ingress     | External entry point (HTTP routing) |
| Service     | Internal load balancer              |
| Pods        | Running containers                  |
| Deployments | Manage pod lifecycle                |
| PVC         | Persistent storage                  |
| Secret      | Secure credentials                  |

---

# 🔄 3. Request Flow (Detailed)

1. User sends HTTP request to domain
2. Request hits **Ingress Controller**
3. Ingress routes request to **vproapp-service**
4. Service forwards request to **Application Pod**
5. Application interacts with:

   * MySQL (Database)
   * Memcached (Cache)
   * RabbitMQ (Message Queue)
6. Database reads/writes data via **Persistent Volume**

---

# 🧱 4. Kubernetes Objects Breakdown

---

## 🔐 4.1 Secret

### Purpose:

Store sensitive information (passwords)

### Used for:

* MySQL root password
* RabbitMQ password

### Key Concepts:

* Base64 encoded (not encrypted)
* Injected as environment variables

---

## 💾 4.2 PersistentVolumeClaim (PVC)

### Purpose:

Provide persistent storage for MySQL

### Why Needed:

* Pods are ephemeral
* Data must survive restarts

### Flow:

```
PVC → StorageClass → AWS EBS → Mounted into Pod
```

---

## 🗄️ 4.3 MySQL Deployment

### Role:

* Stateful backend service
* Stores application data

### Key Features:

* Uses PVC for storage
* Uses Secret for password
* Runs on port 3306

---

## ⚡ 4.4 Memcached Deployment

### Role:

* Caching layer
* Improves performance

### Behavior:

* Stores frequently accessed data in memory
* Reduces DB load

---

## 🐰 4.5 RabbitMQ Deployment

### Role:

* Message broker
* Enables asynchronous processing

### Use Case:

* Decouples services
* Handles background jobs

---

## 🌐 4.6 Application Deployment (vproapp)

### Role:

* Main frontend application

### Features:

* Connects to all backend services
* Handles user requests

---

## ⏳ 4.7 Init Containers

### Purpose:

Ensure dependencies are ready before app starts

### Example:

* Wait for database DNS resolution

### Why Important:

Prevents application crash during startup

---

## 🔗 4.8 Services

### Purpose:

Enable communication between components

### Type Used:

`ClusterIP`

### Example Mapping:

| Service         | Connects To     |
| --------------- | --------------- |
| vprodb          | MySQL Pod       |
| vprocache01     | Memcached Pod   |
| vpromq01        | RabbitMQ Pod    |
| vproapp-service | Application Pod |

---

## 🌍 4.9 Ingress

### Purpose:

Expose application externally

### Function:

* Acts as HTTP router
* Maps domain → service

### Example:

```
vprofile.local → vproapp-service
```

---

# ⚙️ 5. Deployment Workflow

## Step 1: Create Cluster

Using:

* kOps / EKS / Minikube

---

## Step 2: Apply Manifests

```
kubectl apply -f kubedefs/
```

---

## Step 3: Verify Resources

```
kubectl get pods
kubectl get svc
kubectl get ingress
kubectl get pvc
```

---

## Step 4: Access Application

* Configure domain or `/etc/hosts`
* Open in browser

---

# 🔍 6. Internal Communication Logic

Kubernetes uses **DNS-based service discovery**

Example:

```
vprodb → resolves to DB service IP
```

Application connects using:

* Service name (NOT pod IP)
* Stable and reliable

---

# ⚠️ 7. Design Decisions

---

## 🔹 Why Deployments instead of Pods?

* Self-healing
* Rolling updates
* Scalability

---

## 🔹 Why Services?

* Pods are ephemeral
* Services provide stable endpoints

---

## 🔹 Why PVC?

* Database requires persistent storage
* Avoid data loss

---

## 🔹 Why Ingress?

* Single entry point
* Better routing control

---

## 🔹 Why Init Containers?

* Handle dependency readiness
* Avoid startup failures

---

# 🚨 8. Limitations (Current Implementation)

* Uses base64 instead of real encryption
* No TLS (HTTPS not enabled)
* No resource limits defined
* No monitoring/logging
* Uses placeholder images (e.g., nginx)

---

# 📈 9. Production Improvements

To make this production-ready:

## 🔐 Security

* Use encrypted secrets (e.g., AWS KMS)
* Enable TLS (HTTPS)

## ⚡ Performance

* Add resource limits
* Enable autoscaling (HPA)

## 📊 Observability

* Prometheus + Grafana
* Centralized logging (ELK)

## 🚀 DevOps

* CI/CD pipeline
* Helm charts

---

# 🧪 10. Troubleshooting Guide

---

## ❌ Pods not running

```
kubectl describe pod <pod-name>
```

---

## ❌ Service not routing

* Check labels match selector
* Verify endpoints

---

## ❌ PVC not bound

```
kubectl get pvc
kubectl get sc
```

---

## ❌ Ingress not working

* Ensure ingress controller is installed
* Check DNS mapping

---

# 🧠 11. Key Learnings

* Kubernetes abstracts infrastructure complexity
* Services enable reliable communication
* Storage must be handled explicitly
* Ingress simplifies external access
* Real systems require dependency handling

---

# 🏁 Conclusion

This project demonstrates a **real-world Kubernetes deployment pattern** for multi-tier applications.

It covers essential DevOps concepts and serves as a strong foundation for:

* Cloud-native development
* Microservices architecture
* Production system design

---

🔥 *This documentation reflects practical DevOps knowledge and can be used to showcase Kubernetes expertise in professional environments.*
