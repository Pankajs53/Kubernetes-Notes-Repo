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

## Pods vs. Containers

- **Containers in Docker**:
  - Containers require a container runtime to execute, with Docker's runtime being `dockerd`.
  - Docker containers are managed by the Docker Engine, which ensures that containers are running and healthy.

- **Pods in Kubernetes**:
  - Pods are the smallest deployable units in Kubernetes and are deployed on worker nodes.
  - To run a pod, a container runtime like `CRI-O`, `containerd`, or `dockershim` is necessary.
  - The **kubelet** ensures that pods and their containers are running and healthy on the nodes.
  - Inside each pod, containers share the same network namespace and storage, allowing for easy inter-container communication.

## Kube-proxy

- **Networking in Docker**:
  - Docker uses a bridge network to manage container networking.

- **Kubernetes Networking**:
  - **Kube-proxy** is responsible for networking in Kubernetes, managing communication between services and pods.
  - Every pod in Kubernetes has its own IP address, and `kube-proxy` helps in service discovery and load balancing.

## API Server

- **Role**:
  - The API server is the core component that handles all incoming requests in Kubernetes.
  - It exposes the Kubernetes cluster to the external world, taking requests from users or components like `kubectl`.

## Scheduler

- **Role**:
  - The Scheduler is responsible for assigning pods to nodes based on resource availability and constraints.
  - It works closely with the API server to ensure that pods are scheduled on the most suitable nodes.

## ETCD

- **Role**:
  - ETCD is the backing store for all cluster data in Kubernetes.
  - It stores cluster configuration and state as key-value pairs, ensuring consistency across the cluster.

## Controller Manager

- **Role**:
  - The Controller Manager runs various controllers that regulate the state of the cluster.
  - Examples include the **ReplicaSet Controller**, which ensures the desired number of pod replicas are running.

## Cloud Controller Manager (CCM)

- **Role**:
  - CCM integrates Kubernetes with cloud provider APIs, enabling features like load balancers, storage provisioning, and more.
  - It translates Kubernetes API requests into cloud provider-specific requests, ensuring smooth integration with cloud infrastructure.

---

# Key Kubernetes Concepts

## Pods

- **Definition**:
  - A pod is a group of one or more containers that share the same network and storage.
  - Pods are defined using YAML files, which provide a declarative way to manage their configuration.

- **Advantages of Multiple Containers in a Pod**:
  - **Shared Networking**: Containers in a pod can communicate via `localhost`.
  - **Shared Storage**: Containers can share volumes within the same pod.

- **Cluster IP**:
  - Kubernetes assigns a cluster-wide IP address to each pod, facilitating internal communication.

## Deployment

- **Role**:
  - A Deployment is a higher-level abstraction that manages pods, providing features like auto-scaling and self-healing.

## ReplicaSet

- **Role**:
  - A ReplicaSet ensures a specified number of pod replicas are running at all times.
  - It is a Kubernetes controller that helps maintain the desired state of the application.

## Controllers

- **Role**:
  - Controllers monitor the state of the cluster and make changes to bring the cluster to the desired state defined in the YAML configuration.

## Services

- **Role**:
  - Services provide a stable way to access a set of pods, even if pod IPs change.
  - They enable load balancing and service discovery by using labels and selectors instead of IP addresses.

- **Service Types**:
  1. **ClusterIP**: 
     - Default service type.
     - Exposes the service on a cluster-internal IP, making it accessible only within the cluster.
  2. **NodePort**:
     - Exposes the service on each node's IP at a static port (30000-32767).
     - Allows access from within the organization, typically used for internal applications.
  3. **LoadBalancer**:
     - Exposes the service externally using a cloud provider's load balancer.
     - Automatically provisions a public IP, making the application accessible from the internet.
     - This type works primarily in cloud environments.
