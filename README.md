# Kubernetes-Notes-Repo

# Docker vs. Kubernetes: Key Differences

- **Docker** is a container platform, while **Kubernetes** is a container orchestration platform.

- **Containers** are short-lived and can be killed or restarted at any time, which can cause issues like image pull failures or memory shortages.

## Problems with Docker:
1. **Single Host:** Docker operates on a single host, leading to issues like memory shortages affecting other containers.
2. **Auto Healing:** Docker doesn't automatically restart failed containers, requiring manual intervention.
3. **Auto Scaling:** Docker lacks automatic scaling, making it challenging to handle high traffic.
4. **Enterprise Support:** Docker alone doesn't offer enterprise features like load balancing, auto-healing, or security.

## How Kubernetes Solves These Problems:
1. **Multi-Host Cluster:** Kubernetes works with multiple hosts (nodes), isolating problematic containers and moving them to other nodes.
2. **Auto Scaling:** Kubernetes offers both manual scaling via replica sets and automatic scaling with Horizontal Pod Autoscaler (HPA).
3. **Auto Healing:** Kubernetes automatically restarts failed containers, ensuring minimal downtime.
4. **Enterprise Support:** Kubernetes provides robust features like load balancing, security, and auto-healing, making it suitable for production environments.

---

# Kubernetes Architecture
![Kubernetes Architecture](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)


Kubernetes has two main components:

## 1. Control Plane:
   - **API Server:** Exposes Kubernetes to external requests and manages incoming traffic.
   - **etcd:** Stores all cluster data as key-value pairs.
   - **Scheduler:** Assigns pods to nodes based on availability.
   - **Controller Manager:** Manages controllers, including auto-scaling via replica sets.
   - **Cloud Controller Manager (CCM):** Translates Kubernetes requests to cloud provider APIs (e.g., AWS, Azure).

## 2. Data Plane:
   - **Kubelet:** Ensures pods and containers are running and healthy.
   - **Kubeproxy:** Manages networking, assigns IPs to pods, and handles load balancing.
   - **Container Runtime:** Runs containers within pods using tools like CRI-O, containerd, or Docker.

---

# Pods vs. Containers

- **Containers:** Run applications, managed by container runtimes (e.g., Docker Engine).
- **Pods:** The smallest deployable unit in Kubernetes, containing one or more containers. Kubernetes uses kubelet to maintain and manage pods, ensuring they remain healthy.
