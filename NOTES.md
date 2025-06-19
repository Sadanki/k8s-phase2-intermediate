**Phase 2: Kubernetes Intermediate Concepts**, written in a clear, structured, beginner-friendly format for learning and revision:

---

### 📒 `NOTES.md`

````markdown
# 🧠 Kubernetes Phase 2 – Intermediate Concepts Cheat Sheet

This document summarizes all the intermediate concepts used in Phase 2 with examples and practical usage.

---

## 🔍 1. Probes (Health Checks)

### ➤ Liveness Probe
- Checks if the application is **alive**.
- If it fails continuously, Kubernetes **restarts** the pod.

### ➤ Readiness Probe
- Checks if the application is **ready to serve traffic**.
- If it fails, the pod is **removed from the Service endpoint** but not restarted.

```yaml
livenessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 5

readinessProbe:
  httpGet:
    path: /
    port: 80
  initialDelaySeconds: 5
  periodSeconds: 5
````

---

## 💾 2. Volumes

### ➤ `emptyDir`

* Temporary storage shared **within the same Pod**.
* Data is **lost when the Pod is deleted**.
* Useful for sharing data between containers.

```yaml
volumes:
  - name: shared-data
    emptyDir: {}

volumeMounts:
  - name: shared-data
    mountPath: /data
```

### ➤ PVC (PersistentVolumeClaim)

* Requests persistent storage from Kubernetes.
* Survives pod restarts, useful for databases or logs.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

Mount in a pod:

```yaml
volumes:
  - name: persistent-storage
    persistentVolumeClaim:
      claimName: demo-pvc
```

---

## 🔧 3. Resource Requests & Limits

### ➤ Why use?

* Ensure fair resource allocation.
* Prevent pods from over-consuming resources.

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"
  limits:
    memory: "128Mi"
    cpu: "500m"
```

* **Requests** = minimum resources guaranteed
* **Limits** = maximum allowed

---

## 🔐 4. ConfigMap

Stores **non-sensitive** config data like strings or environment variables.

```bash
kubectl create configmap app-config --from-literal=WELCOME_MSG="Welcome!"
```

Usage in a pod:

```yaml
env:
  - name: WELCOME_MSG
    valueFrom:
      configMapKeyRef:
        name: app-config
        key: WELCOME_MSG
```

---

## 🐳 5. Custom Docker Image with Minikube

Use Minikube’s Docker daemon:

```bash
minikube docker-env | Invoke-Expression
```

Then build:

```bash
docker build -t custom-nginx:v1 .
```

This allows you to use the image inside Minikube **without pushing to Docker Hub**.

---

## 🌐 6. Service

Expose a pod or deployment via `NodePort`.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: phase2-service
spec:
  type: NodePort
  selector:
    app: probe-demo
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30012
```

Access with:

```bash
minikube service phase2-service
```

---

## 📦 7. Folder Summary

```
k8s-phase2-intermediate/
├── configmap.yaml             # Create WELCOME_MSG
├── emptydir-volume.yaml       # Shared volume demo
├── pvc-volume.yaml            # Persistent volume demo
├── probes-deployment.yaml     # Nginx with probes
├── resource-limits.yaml       # CPU/memory requests/limits
├── service.yaml               # NodePort to access app
├── index.html                 # Custom HTML content
├── README.md                  # Project usage
└── NOTES.md                   # This file 🧠
```

---

## ✅ Execution Order

1. `minikube docker-env | Invoke-Expression`
2. `docker build -t custom-nginx:v1 .`
3. `kubectl apply -f configmap.yaml`
4. `kubectl apply -f probes-deployment.yaml`
5. `kubectl apply -f service.yaml`
6. `minikube service phase2-service`

---

## 🙌 You’ve Learned

✅ Health checks with probes
✅ Sharing data using volumes
✅ Persistence using PVC
✅ Managing resources with limits
✅ Serving your own HTML content
✅ Docker + Minikube integration

---

Keep going! Phase 3 will introduce **Secrets, Ingress, HPA, and more**! 🚀

```

