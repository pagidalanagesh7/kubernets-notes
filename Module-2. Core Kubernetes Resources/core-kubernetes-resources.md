# Module 2: Core Kubernetes Resources

This module dives into the essential Kubernetes resources that form the building blocks of container orchestration. Understanding these components is crucial for designing, deploying, and managing scalable and resilient applications in a Kubernetes environment.

---

## 📦 POD

**Definition:**  
A Pod is the smallest and most basic deployable object in Kubernetes. A Pod represents a single instance of a running process in a cluster.

**Key Concepts:**
- A Pod can host one or multiple tightly coupled containers.
- Containers in a Pod share:
  - **Network namespace**: same IP and port space.
  - **Volumes**: for sharing data.
- Pods are ephemeral — once terminated, they are not rescheduled automatically (handled by higher-level resources like Deployments or ReplicaSets).

**Use Case Example:**  
Running a single Nginx container or a helper container that logs data from a main app container.

---

## 🔁 Replication Controller

**Definition:**  
A legacy Kubernetes object that ensures a specified number of Pod replicas are running at any given time.

**Key Concepts:**
- Automatically creates or deletes Pods to match the desired replica count.
- Limited label selector support.
- Being deprecated in favor of ReplicaSets.

**Use Case Example:**  
Legacy applications using early versions of Kubernetes.

---

## 🔁 Replica Set

**Definition:**  
An evolution of the Replication Controller, providing more advanced and flexible label selectors.

**Key Concepts:**
- Maintains a stable set of running Pod replicas.
- Supports complex label-based selection.
- Used indirectly via Deployments in most modern use cases.

**Use Case Example:**  
Used behind the scenes when you create a Deployment for your app.

---

## 🔄 Daemon Set

**Definition:**  
A controller that ensures a copy of a specific Pod runs on all (or selected) nodes.

**Key Concepts:**
- Used for cluster-wide services like log shipping, monitoring agents, or storage daemons.
- Automatically adds Pods when new nodes join the cluster.
- Removes Pods from nodes when they are removed from the cluster.

**Use Case Example:**  
Running Fluentd, Filebeat, or Prometheus Node Exporter on all nodes.

---

## 🚀 Deployment

**Definition:**  
A higher-level abstraction that manages ReplicaSets and provides declarative updates for Pods.

**Key Concepts:**
- Simplifies rolling updates and rollbacks.
- Supports scaling operations.
- Automatically replaces failed Pods.
- Uses ReplicaSet under the hood to manage Pod replicas.

**Use Case Example:**  
Deploying a frontend service and updating its version with zero downtime.

---

## 🔄 Rolling Update

**Definition:**  
The default update strategy for Deployments that gradually replaces old versions of Pods with new ones.

**Key Concepts:**
- Ensures availability during updates.
- Configurable parameters: `maxSurge`, `maxUnavailable`.
- Allows live traffic to continue with minimal disruption.

**Use Case Example:**  
Upgrading from `v1.0` to `v1.1` of an application without downtime.

---

## ♻️ Recreate Strategy

**Definition:**  
An alternative deployment strategy where existing Pods are terminated before new Pods are created.

**Key Concepts:**
- No overlap between old and new Pods.
- Causes downtime during update.
- Useful when apps cannot run multiple versions concurrently (e.g., database schema changes).

**Use Case Example:**  
Applications with shared storage that can't tolerate simultaneous access from multiple versions.

---

## 🧠 Stateful Set

**Definition:**  
A controller designed for managing stateful applications, providing each Pod with a stable, unique identity and persistent storage.

**Key Concepts:**
- Pods have predictable names (e.g., `web-0`, `web-1`).
- Maintains sticky identity and order of deployment and termination.
- Works with PersistentVolumeClaim templates.

**Use Case Example:**  
Databases like MySQL, PostgreSQL, or Cassandra, where stable network identity and persistent storage are critical.

---

## Summary Table

| Resource             | Purpose                                                                 |
|----------------------|-------------------------------------------------------------------------|
| **Pod**              | Basic unit of deployment; container(s) running in the cluster.          |
| **Replication Controller** | Legacy replication manager (mostly replaced).                    |
| **Replica Set**      | Maintains set of identical Pods, supports advanced selectors.           |
| **Daemon Set**       | Runs a Pod on every node for infrastructure-level services.             |
| **Deployment**       | Manages Pods and ReplicaSets, enables rolling updates and rollbacks.    |
| **Rolling Update**   | Smooth, no-downtime update of Pods managed by Deployments.              |
| **Recreate Strategy**| Terminates old Pods before starting new ones, may cause downtime.       |
| **Stateful Set**     | Ensures stable network identity and storage for stateful applications.  |

