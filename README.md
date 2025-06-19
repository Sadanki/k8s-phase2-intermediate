
---

### 📘 `README.md`

```markdown
# 🚀 Kubernetes Phase 2: Intermediate Concepts

Welcome to **Phase 2** of your Kubernetes journey! This phase covers intermediate-level features like **liveness/readiness probes**, **volumes**, **resource limits**, **PVCs**, and **ConfigMaps**.

---

## 📁 Folder Structure

```

<pre> ``` k8s-phase2-intermediate/ ├── configmap.yaml # Define WELCOME_MSG as ConfigMap ├── emptydir-volume.yaml # Shared volume using emptyDir ├── index.html # Custom HTML file served via Nginx ├── NOTES.md # Deep notes and explanations ├── probes-deployment.yaml # Liveness & Readiness probes ├── pvc-volume.yaml # PersistentVolumeClaim demo ├── README.md # You are here ├── resource-limits.yaml # CPU/Memory constraints └── service.yaml # NodePort service for access ``` </pre>

````

---

## 🔧 What You’ll Learn

✅ Probes (Health Checks)  
✅ Volumes (`emptyDir` and PVC)  
✅ ConfigMap Basics  
✅ Resource Requests & Limits  
✅ Serving custom HTML with Docker  
✅ Cluster-wide separation using namespaces (optional)

---

## 🧪 Steps to Run Everything

> ⚠️ Ensure Docker is pointing to Minikube's environment:

```bash
minikube docker-env | Invoke-Expression
````

---

### 1. 🔨 Build Your Custom Image

```bash
docker build -t custom-nginx:v1 .
```

---

### 2. 🚀 Deploy the Probes Demo

```bash
kubectl apply -f probes-deployment.yaml
kubectl apply -f service.yaml
```

Then access the app via:

```bash
minikube service phase2-service
```

---

### 3. 💾 Run the emptyDir Shared Volume Demo

```bash
kubectl apply -f emptydir-volume.yaml
kubectl describe pod emptydir-demo
```

Check logs of both containers to verify shared data:

```bash
kubectl logs emptydir-demo -c writer
kubectl logs emptydir-demo -c reader
```

---

### 4. 📦 Run PVC (Persistent Volume Claim) Demo

```bash
kubectl apply -f pvc-volume.yaml
```

---

### 5. ⚙️ Test Resource Limits

```bash
kubectl apply -f resource-limits.yaml
kubectl describe pod resource-demo
```

---

## 🧹 Cleanup (Optional)

```bash
kubectl delete -f .
```

---

## ✍️ Author

**Vignesh Sadanki**
📍 GitHub: [Sadanki](https://github.com/Sadanki)

---

Happy Clustering! ☸️🐳🔥

````

---

### 📒 `NOTES.md`

```markdown
# 🧠 Kubernetes Phase 2 – Deep Notes

This document summarizes key concepts used in this phase.

---

## 📍 Probes

- **Liveness Probe**: Checks if the container is still alive. If it fails, Kubernetes restarts the pod.
- **Readiness Probe**: Checks if the pod is ready to accept traffic. If it fails, Kubernetes removes it from the Service endpoint.

---

## 📦 Volumes

### 🔹 emptyDir
- Shared between containers in the same Pod.
- Lives as long as the Pod exists.
- Data is lost when the Pod is deleted.

### 🔹 Persistent Volume Claim (PVC)
- Requests storage from the cluster.
- Can survive pod restarts.
- Useful for real applications that require data persistence.

---

## 🔐 ConfigMap

Used to inject configuration into pods as environment variables or mounted files.

```yaml
env:
  - name: WELCOME_MSG
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: WELCOME_MSG
````

---

## ⚙️ Resource Requests & Limits

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

* **Requests**: Minimum guaranteed resources
* **Limits**: Maximum the pod is allowed to consume

---

## 🔁 Docker + Minikube

Always remember to point Docker to Minikube before building:

```bash
minikube docker-env | Invoke-Expression
```

Then build:

```bash
docker build -t custom-nginx:v1 .
```

---

## 🌐 Service

Exposes your app to the browser:

```yaml
type: NodePort
```

Access with:

```bash
minikube service phase2-service
```

---

## ✅ Checklist

* [x] Probes added
* [x] ConfigMap defined
* [x] Volume + PVC covered
* [x] Resource limits configured
* [x] Custom HTML served
* [x] Custom Docker image built

```


