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
## Ingress

Before Ingress was introduced in Kubernetes, there were a couple of challenges with exposing and managing services:

### 1. Limited Load Balancing Features:

a) The load balancing provided by Kubernetes Services was relatively basic, typically using simple round-robin techniques. However, more advanced load balancing techniques are often needed in production environments, such as:
- Ratio-Based Load Balancing
- Sticky Sessions
- Path-Based Routing
- Domain-Based Routing
- Whitelisting and Blacklisting

b) These advanced load balancing features are typically provided by enterprise-level load balancers like Nginx and F5.

### 2. High Cost of Static IPs:

a) Exposing each service using a LoadBalancer type Service in Kubernetes required a static IP. For environments with a large number of services (e.g., 100 services), this could become very costly, as each service would need its own static IP.

b) Enterprise load balancers allow for more efficient IP management by requiring only one main static IP. This IP can handle traffic for multiple services based on domain-based routing, thereby reducing the overall cost.

### How Ingress Solves These Problems
Ingress in Kubernetes provides a solution to these issues by:

1. **Advanced Load Balancing and TLS Termination:**
   - Ingress allows you to perform enterprise-level load balancing with features such as path-based and domain-based routing. It also supports TLS termination, which handles HTTPS traffic.

2. **Cost Efficiency:**
   - Instead of needing a separate static IP for each service, Ingress allows multiple services to be exposed through a single IP address. This is achieved by routing requests to different services based on rules defined in the Ingress resource, thereby significantly reducing the need for multiple static IPs and the associated costs.

### Ingress Resources and Ingress Controllers
- Users create Ingress resources, while load balancing organizations create/write their own Ingress controllers. These controllers are typically shared on platforms like GitHub, along with instructions on how to install and use them, either via Helm charts or YAML files.
- As a user, it's essential to deploy an Ingress controller in addition to creating Ingress resources. The choice of Ingress controller depends on your needs. At the end of the day, Ingress controllers are a type of load balancer, and some may also function as an API Gateway, offering additional capabilities.
  
- The Ingress controller monitors or "watches" for Ingress resources. An Ingress resource is simply a YAML file that defines the routing rules.
  
- **Note:** If you're running Kubernetes locally, you'll need to map the cluster IP with the domain name through which you want to access the services in the `/etc/hosts` file.

  ## ConfigMaps and Secrets in Kubernetes

### ConfigMaps:
1. **Purpose:** ConfigMaps are used to store non-sensitive configuration data in key-value pairs that can be consumed by applications running in pods. The data in ConfigMaps can be used to configure application behavior without modifying the container image or application code. This is particularly useful when you don't have direct access to the container or want to externalize configuration.

2. **Usage:** ConfigMaps can be mounted into pods as environment variables, command-line arguments, or configuration files. This flexibility allows you to decouple configuration from application code, enabling changes to configuration without rebuilding Docker images.

3. **Problem Solved:** ConfigMaps help solve the problem of externalizing configuration data, making it easy to update application settings without modifying the application code or redeploying the container image.

4. **Limitations:** While ConfigMaps are suitable for storing general configuration data, they are not designed for sensitive information like passwords, API keys, or tokens. This is where Secrets come into play.

### Secrets:
1. **Purpose:** Secrets are used to store sensitive data, such as passwords, API keys, and tokens, in a secure manner. Unlike ConfigMaps, Secrets are designed with security in mind, ensuring that the data is encrypted both in transit and at rest within the Kubernetes cluster.

2. **Security:** When a Secret is created, Kubernetes stores it in the etcd database in an encrypted format. This prevents unauthorized users from easily accessing sensitive data even if they gain access to etcd. By default, Kubernetes uses Base64 encoding for data in Secrets, but you can configure additional encryption mechanisms for enhanced security.

3. **RBAC Integration:** Role-Based Access Control (RBAC) should be used to restrict access to Secrets, ensuring that only authorized users and service accounts can view or modify them. This further enhances security by enforcing the principle of least privilege.

### Why Use Secrets Instead of ConfigMaps?
1. **Encryption:** Secrets are encrypted by default, making them suitable for storing sensitive information. ConfigMaps, on the other hand, do not offer encryption and are stored in plain text within etcd.

2. **Security Best Practices:** Storing sensitive data in ConfigMaps is not recommended because it exposes the data to potential security risks. Secrets provide a secure way to handle such data, reducing the risk of unauthorized access.

### Updating ConfigMaps and Secrets:
1. **ConfigMap Updates:**
   - When you update a ConfigMap, the new data does not automatically propagate to existing pods using that ConfigMap as environment variables. To apply the updated configuration, you typically need to restart the pods, which can lead to temporary downtime.

2. **Using Volume Mounts:**
   - To avoid the need for pod restarts, you can mount ConfigMaps or Secrets as files inside a pod using VolumeMounts. When the ConfigMap or Secret is updated, the mounted files will be automatically updated without needing to restart the pods. This ensures that your application can dynamically pick up configuration changes without causing downtime.

### Example Use Case:
If your application reads configuration data from a file (mounted via a VolumeMount), you can update the ConfigMap or Secret, and the changes will be reflected in the file without restarting the pod. This is particularly useful in production environments where minimizing downtime is critical.

## RBAC (Role-Based Access Control)

RBAC in Kubernetes is a mechanism that allows you to manage access and permissions for users and applications interacting with the Kubernetes cluster. It primarily deals with:

1. **User Management**
2. **Managing Access to Applications Running on the Cluster**

### There are three major components in RBAC:

1. **Service Accounts/Users**
2. **Roles/ClusterRoles**
3. **RoleBindings/ClusterRoleBindings**

### Service Accounts/Users:
- **Kubernetes does not manage user identities directly; instead, it delegates user management to external identity providers.**
  - For example, in AWS (EKS), identity management can be handled through AWS IAM. IAM policies can be created to allow or restrict access to the Kubernetes API.

- **Service Accounts:**
  1. A Service Account is a special type of account in Kubernetes that is used by pods to interact with the Kubernetes API. Service Accounts are defined within a namespace and are typically used to manage permissions for applications running inside the cluster.
  2. Service Accounts can be created using a YAML file and are automatically attached to a pod if no Service Account is specified during pod creation. This default Service Account allows the pod to interact with the Kubernetes API with minimal permissions.

### Roles/ClusterRoles:
- **Roles and ClusterRoles define what actions a user or Service Account can perform within the cluster.**

  - **Role:**
    - A Role is a namespaced resource that defines permissions (such as accessing Pods, ConfigMaps, Secrets, etc.) within a specific namespace. It is defined using a YAML file and is similar to IAM policies in cloud providers.

  - **ClusterRole:**
    - A ClusterRole is similar to a Role but is not restricted to a single namespace. It can define permissions across the entire cluster. ClusterRoles are useful for granting permissions that are not specific to a single namespace, such as managing nodes or cluster-wide resources.

### RoleBindings/ClusterRoleBindings:
- **RoleBinding and ClusterRoleBinding are used to attach Roles and ClusterRoles to users or Service Accounts.**

  - **RoleBinding:**
    - A RoleBinding attaches a Role to a user or Service Account within a specific namespace, granting the defined permissions to that entity in that namespace.

  - **ClusterRoleBinding:**
    - A ClusterRoleBinding attaches a ClusterRole to a user or Service Account at the cluster level, granting the defined permissions cluster-wide.

### Example Workflow:
1. Define a Role in a namespace that allows certain actions, such as reading Secrets or managing Pods.
2. Create a Service Account for a specific application that will be running in the cluster.
3. Bind the Role to the Service Account using a RoleBinding. This grants the application the necessary permissions to perform its tasks within that namespace.

If the permissions are needed across multiple namespaces or cluster-wide, you would use ClusterRoles and ClusterRoleBindings instead.

---

## Custom Resources:

In Kubernetes, there are default resources like Pods, Services, and Deployments that come out of the box. However, sometimes these built-in resources may not be sufficient for all needs. To extend Kubernetes' API or introduce new types of resources, we can use Custom Resources (CRs).

### Custom Resources allow us to add new resource types to Kubernetes, tailored to specific needs that Kubernetes doesn't natively support. For example:
- If we need advanced security features that Kubernetes doesn't offer by default, we can introduce tools like Kube Hunter or Kube Bench.
- Applications like Argo CD bring GitOps capabilities to Kubernetes.

### Components of Custom Resources:

1. **Custom Resource Definition (CRD):**
   - A Custom Resource Definition (CRD) is an extension of the Kubernetes API that allows you to define new types of resources. When you create a CRD, it tells the Kubernetes API server to recognize and handle your new custom resource.
   - CRDs are how Kubernetes users create their own APIs that look and act like the built-in Kubernetes resources.

2. **Custom Controller:**
   - A Custom Controller is a process that watches the state of your custom resources and takes action to reconcile the current state with the desired state defined in the custom resource. Without a controller, the custom resource would just exist as data in etcd, and nothing would actually happen in response to it.
   - The controller is responsible for implementing the logic needed to manage the custom resource.

3. **Custom Resource (CR):**
   - A Custom Resource (CR) is an instance of a Custom Resource Definition. It is the YAML file that you write as a user or DevOps engineer, which defines the desired state of a particular instance of the custom resource.

### Example Scenario:
When you create a Deployment in Kubernetes, there’s already a Deployment CRD and a Deployment controller built into Kubernetes. You just need to write a YAML file that defines the desired state of the Deployment. The Kubernetes API server checks your YAML against the Deployment CRD to ensure it's correctly structured. The Deployment controller then monitors this resource and ensures that the desired state is achieved and maintained.

Similarly, for custom resources, if you want to add a new resource type or extend Kubernetes capabilities, you first need to define the CRD for that resource and ensure a custom controller is running to manage instances of the CRD. As a user or DevOps engineer, you then write a YAML file that creates an instance of the custom resource.

Without a custom controller, creating a custom resource through a YAML file won't result in any action—Kubernetes will store the data, but it won't do anything with it.


