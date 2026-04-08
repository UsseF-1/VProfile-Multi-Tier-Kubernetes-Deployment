# VProfile-Multi-Tier-Kubernetes-Deployment
Production-style Kubernetes deployment of a multi-tier application using MySQL, Memcached, RabbitMQ, Persistent Storage (PVC), Secrets, and Ingress.
# 🚀 VProfile Kubernetes Deployment (Production-Style Project)

## 📌 Project Overview

This project demonstrates a **production-style deployment of a multi-tier web application** on Kubernetes.

The application follows a typical microservices architecture and includes:

* Web Application (Frontend)
* Database (MySQL)
* Cache Layer (Memcached)
* Message Broker (RabbitMQ)

All components are containerized and orchestrated using **Kubernetes**, ensuring scalability, high availability, and fault tolerance.

---

## 🧱 Architecture

```
User → Ingress → Service → Application Pod → Backend Services → Database (Persistent Storage)
```

### 🔄 Request Flow

1. User sends a request to the application domain.
2. Request hits the **Ingress Controller** (NGINX).
3. Ingress routes traffic to the **Application Service**.
4. Service forwards traffic to the **Application Pod (vproapp)**.
5. Application communicates with:

   * MySQL (Database)
   * Memcached (Cache)
   * RabbitMQ (Message Queue)
6. Database persists data using **Persistent Volume (PVC + EBS)**.

---

## ⚙️ Technologies Used

* Kubernetes
* Docker (Containerization)
* NGINX Ingress Controller
* MySQL
* Memcached
* RabbitMQ
* AWS EBS (for persistent storage)

---

## 📁 Project Structure

```
kubedefs/
├── secret.yaml          # Stores sensitive data (DB & RabbitMQ passwords)
├── db-pvc.yaml          # Persistent Volume Claim for MySQL
├── db-deploy.yaml       # MySQL Deployment
├── db-service.yaml      # MySQL Service (ClusterIP)
├── mc-deploy.yaml       # Memcached Deployment
├── mc-service.yaml      # Memcached Service
├── rmq-deploy.yaml      # RabbitMQ Deployment
├── rmq-service.yaml     # RabbitMQ Service
├── app-deploy.yaml      # Application Deployment
├── app-service.yaml     # Application Service
└── ingress.yaml         # External access configuration
```

---

## 🔐 Secrets Management

Sensitive data such as database and message broker passwords are stored using **Kubernetes Secrets**.

* Prevents hardcoding credentials in configuration files
* Uses Base64 encoding (for demonstration purposes)

---

## 💾 Persistent Storage

The MySQL database uses:

* **PersistentVolumeClaim (PVC)**
* Backed by AWS EBS (via StorageClass)

This ensures:

* Data persistence across pod restarts
* Reliable storage for stateful applications

---

## 🔁 Deployments

Each service runs as a Kubernetes **Deployment**, providing:

* Self-healing (automatic pod restart)
* Easy updates (rolling updates)
* Scalability (replica management)

---

## 🔗 Services

Each component is exposed internally using **ClusterIP Services**:

| Service         | Purpose         |
| --------------- | --------------- |
| vprodb          | Database access |
| vprocache01     | Cache layer     |
| vpromq01        | Message broker  |
| vproapp-service | Frontend access |

---

## 🌐 Ingress (External Access)

An **Ingress resource** is used to expose the application externally:

* Routes traffic based on hostname
* Connects external Load Balancer → internal services

Example:

```
vprofile.local → vproapp-service
```

---

## ⏳ Init Containers

The application uses an **init container** to ensure backend services (e.g., database) are ready before the application starts.

This prevents startup failures and improves reliability.

---

## 🚀 Deployment Steps

### 1. Create Kubernetes Cluster

Using tools like:

* kOps
* EKS
* Minikube (for local testing)

---

### 2. Apply Kubernetes Manifests

```
kubectl apply -f kubedefs/
```

---

### 3. Verify Deployment

```
kubectl get pods
kubectl get svc
kubectl get ingress
kubectl get pvc
```

---

### 4. Access Application

* Use Ingress hostname (or configure `/etc/hosts`)
* Open in browser

---

## 🧪 Validation

* Check Pods are running
* Ensure Services are routing correctly
* Verify database persistence
* Confirm application connectivity to backend services

---

## ⚠️ Notes

* Replace placeholder images (e.g., nginx) with your actual application image
* Configure a real domain for Ingress in production
* Add TLS (HTTPS) for secure access
* Base64 encoding is not secure → use proper secrets management in production

---

## 📈 Future Improvements

* CI/CD Pipeline (GitHub Actions / Jenkins)
* Monitoring (Prometheus + Grafana)
* Logging (ELK Stack)
* Helm charts for templating
* Horizontal Pod Autoscaling (HPA)

---

## 👨‍💻 Author

This project is part of a hands-on DevOps learning journey, focusing on real-world Kubernetes deployments and production-ready practices.

---

## ⭐ Key Takeaways

* Kubernetes enables scalable and resilient deployments
* Separation of concerns using services and deployments
* Persistent storage is critical for stateful applications
* Ingress simplifies external traffic routing
* Secrets help manage sensitive data securely

---

🔥 *This project demonstrates real-world DevOps practices and is suitable for showcasing Kubernetes skills in portfolios and interviews.*
