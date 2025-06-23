
# Module 3: Storage & Configuration Management

This module focuses on how Kubernetes manages storage and configuration using various resources.

---

## 🔹 Topics Covered:

### 1. Service
A Kubernetes Service is an abstraction that exposes a set of Pods as a network service. It provides:

- Stable IP and DNS name
- Load balancing between Pods
- Various types: ClusterIP, NodePort, LoadBalancer, ExternalName

---

### 2. Persistent Volumes (PV)
Persistent Volumes are storage resources provisioned in a cluster:

- Independent of Pod lifecycle
- Created and managed by administrators
- Supports various backends (hostPath, NFS, cloud volumes, etc.)

---

### 3. Persistent Volume Claim (PVC)
A PVC is a user’s request for storage:

- Specifies size and access mode
- Automatically binds to an available PV
- Mounted into a Pod to persist data

---

### 4. Dynamic Volumes
Dynamic provisioning automates the creation of Persistent Volumes:

- Uses `StorageClass` for defining provisioner logic
- Eliminates manual PV management
- Supports cloud storage, CSI drivers, etc.

---

### 5. ConfigMaps & Secrets

#### ✅ ConfigMaps:
- Store non-sensitive configuration data
- Can be injected into Pods via files or environment variables

#### 🔐 Secrets:
- Store sensitive data (passwords, API keys)
- Base64-encoded and mountable in Pods
- Accessed securely via `secretKeyRef`

---

## 🧪 YAML Examples and Usage

### 🔹 1. Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: ClusterIP
```

---

### 🔹 2. Persistent Volume

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/data
```

---

### 🔹 3. Persistent Volume Claim

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

### 🔹 4. StorageClass + Dynamic PVC

#### StorageClass:
```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
```

#### PVC using StorageClass:
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: dynamic-pvc
spec:
  storageClassName: standard
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

---

### 🔹 5. ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  APP_ENV: production
  LOG_LEVEL: debug
```

#### Use in Pod:
```yaml
env:
- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: my-config
      key: APP_ENV
```

---

### 🔹 6. Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: YWRtaW4=     # base64 for "admin"
  password: cGFzc3dvcmQ= # base64 for "password"
```

#### Use in Pod:
```yaml
env:
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: my-secret
      key: username
```
