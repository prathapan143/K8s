**Kubernetes Architecture**
===========================
---

Kubernetes is a container orchestration platform that automates the deployment, scaling, and management of containerized applications. The architecture of Kubernetes consists of several components that work together to provide a scalable and reliable platform for running containerized workloads. Here's an overview of the Kubernetes architecture:

![image](https://github.com/nani05190682/kubernetes/assets/87597729/c3c5f7ed-a7a2-4cf3-9483-874dbbe1e431)


1. **Master node**: The master node is the control plane of Kubernetes and manages the overall state of the cluster. It consists of several components:

    - **API server**: The API server is the central management point for the Kubernetes cluster. It exposes the Kubernetes API, which allows users to interact with the cluster and manage its resources.

    - **etcd**: etcd is a distributed key-value store that stores the configuration data for the Kubernetes cluster, including information about nodes, pods, services, and other resources.

    - **Controller manager**: The controller manager runs various controllers that monitor the state of the cluster and take actions to ensure that the desired state is maintained. Examples of controllers include the replication controller, which manages the number of replicas of a pod, and the endpoint controller, which manages service endpoints.

    - **Scheduler**: The scheduler is responsible for scheduling pods to run on nodes in the cluster based on resource requirements and other constraints.

2. **Worker nodes**: Worker nodes are the nodes in the Kubernetes cluster that run the containerized workloads. Each worker node runs several components:

    - **Kubelet**: The kubelet is an agent that runs on each node and communicates with the API server to receive instructions for running pods.

    - **Container runtime**: The container runtime is responsible for running containers on the node. Examples of container runtimes include Docker and containerd.

    - **kube-proxy**: The kube-proxy is a network proxy that runs on each node and handles network traffic to and from the pods.

3. **Pods**: Pods are the smallest deployable units in Kubernetes and consist of one or more containers. Each pod runs on a single node and has its own IP address. Pods are managed by controllers, such as the replication controller, which ensure that the desired number of replicas are running at all times.

4. **Services**: Services provide a stable IP address and DNS name for a set of pods. They allow pods to communicate with each other and with external clients using a consistent endpoint, even if the underlying pods are scaled up or down.

This is a high-level overview of the Kubernetes architecture, and there are many more components and features that can be added to customize and extend the platform.


**Master Node Component**
---
---

**API Server**


The API server is a core component of Kubernetes that provides a central management point for the cluster. Its primary functionality is to expose the Kubernetes API, which allows users to interact with the cluster and manage its resources. Here are some of the key functions of the API server:

1. **Authentication and authorization**: The API server handles authentication and authorization for all API requests. It verifies the identity of the user or application making the request and checks whether they have the necessary permissions to perform the requested operation.

2. **Resource management**: The API server manages the state of all resources in the Kubernetes cluster, including nodes, pods, services, and deployments. It stores this information in etcd, a distributed key-value store.

3. **API endpoint routing**: The API server routes API requests to the appropriate controller or component that is responsible for handling the request. For example, if a user requests to create a new pod, the API server sends the request to the scheduler component, which determines which node in the cluster should run the pod.

4. **Event notification**: The API server generates events when resources are created, updated, or deleted in the cluster. These events can be used to trigger alerts or automate workflows.

5. **Admission control**: The API server enforces admission control policies that determine whether a resource can be created or updated in the cluster. This includes validating resource configurations and enforcing resource quotas.



**ETCD**


etcd is a distributed key-value store that is used by Kubernetes to store the configuration data for the cluster. It provides a reliable and highly-available way to store and retrieve data, which is critical for the operation of Kubernetes. Here are some of the key functions of etcd in a Kubernetes cluster:

1. **Cluster state management**: etcd is used to store the state of all resources in the Kubernetes cluster, including nodes, pods, services, and deployments. This information is stored as key-value pairs in a hierarchical structure.

2. **High availability**: etcd is designed to be highly available, even in the event of node failures or network partitions. It achieves this by replicating data across multiple nodes in the cluster and using a **consensus algorithm** to ensure that all nodes have a consistent view of the data.

3. **Data consistency**: etcd ensures that all nodes in the cluster have a consistent view of the data by using a distributed locking mechanism. This prevents multiple nodes from updating the same data at the same time, which could result in conflicts or inconsistencies.

4. **Watch API**: etcd provides a watch API that allows clients to receive notifications when specific keys are updated. This is used by Kubernetes controllers to monitor changes to resources and take appropriate actions.

5. **Backup and restore**: etcd provides tools for backing up and restoring the data stored in the key-value store. This is important for disaster recovery and for migrating data between clusters.

**Controller manager** 

The Controller Manager is a core component of Kubernetes that runs on the master node and is responsible for managing various controllers that monitor the state of the cluster and take actions to ensure that the desired state is maintained. Here are some of the key functions of the Controller Manager in a Kubernetes cluster:

1. **Replication controller**: The replication controller ensures that the desired number of replicas of a pod are running at all times. If a pod fails or is terminated, the replication controller automatically creates a new replica to replace it.

2. **Endpoint controller**: The endpoint controller manages service endpoints, which are the IP addresses and ports that are used to access a set of pods that provide a particular service.

3. **Namespace controller**: The namespace controller manages namespaces, which are used to organize resources in the cluster and provide a way to isolate resources between different users or applications.

4. **Service account and token controllers**: The service account and token controllers manage service accounts and tokens, which are used for authentication and authorization in the cluster.

5. **Node controller**: The node controller monitors the state of nodes in the cluster and takes action if a node becomes unavailable or unresponsive.

6. **Daemon set controller**: The daemon set controller ensures that a specific pod is running on every node in the cluster.

7. **Job controller**: The job controller manages batch jobs, which are short-lived tasks that run to completion and then terminate.

 The Controller Manager is a critical component of Kubernetes that ensures that the desired state of the cluster is maintained and that resources are running as expected. By automating these tasks, Kubernetes provides a scalable and reliable platform for running containerized workloads.


**Scheduler** 


The Scheduler is a core component of Kubernetes that runs on the master node and is responsible for scheduling pods to run on nodes in the cluster. Its primary function is to ensure that pods are scheduled to run on nodes that have the necessary resources to run them, such as CPU, memory, and storage. Here are some of the key functions of the Scheduler in a Kubernetes cluster:

1. **Node selection**: The Scheduler selects a node from the pool of available nodes that has the necessary resources to run the pod. It considers factors such as resource requirements, node capacity, and pod affinity and anti-affinity rules.

2. **Resource allocation**: The Scheduler ensures that there are enough resources available on the selected node to run the pod. It takes into account the resource requests and limits specified in the pod specification.

3. **Pod binding**: Once a suitable node is found, the Scheduler binds the pod to the node by updating the pod's status and sending a binding request to the API server.

4. **Pod scheduling**: The Scheduler monitors the state of the pod and ensures that it is scheduled to run on a node as soon as possible. If a node becomes unavailable or unsuitable for running the pod, the Scheduler reschedules it to run on a different node.

The Scheduler is a critical component of Kubernetes that ensures that pods are scheduled to run on nodes in a way that maximizes resource utilization and ensures high availability and reliability. By automating this process, Kubernetes provides a scalable and efficient platform for running containerized workloads.

Worker Node Components
===

___

**Kubelet**

The Kubelet is a core component of Kubernetes that runs on each node in the cluster and is responsible for managing the state of pods. Its primary function is to ensure that the pods scheduled to run on the node are running and healthy. Here are some of the key functions of the Kubelet in a Kubernetes cluster:

1. **Pod management**: The Kubelet is responsible for managing the lifecycle of pods on the node. It ensures that the pods are running and healthy, and restarts them if they fail or become unresponsive.

2. **Container management**: The Kubelet manages the state of containers within a pod, including starting and stopping containers as needed.

3. **Resource management**: The Kubelet monitors the resource usage of containers on the node and reports this information to the API server. It also enforces resource limits specified in pod specifications.

4. **Networking**: The Kubelet sets up networking for pods on the node, including assigning IP addresses and setting up network routes.

5. **Volume management**: The Kubelet manages volumes attached to pods on the node, including creating and mounting volumes as needed.

6. **Health monitoring**: The Kubelet monitors the health of pods and reports their status to the API server. If a pod becomes unhealthy, the Kubelet takes action to restart or replace it.

The Kubelet is a critical component of Kubernetes that ensures that pods are running and healthy on each node in the cluster. By automating these tasks, Kubernetes provides a scalable and reliable platform for running containerized workloads.

**Container Runtime Engine**

A container runtime is a software component that is responsible for running containers on a host system. It provides an environment for running containerized applications and manages the lifecycle of containers, including starting and stopping containers, managing container images, and providing network and storage interfaces.

In a Kubernetes cluster, the container runtime is responsible for running containers on worker nodes. Kubernetes supports several container runtimes, including Docker, containerd, and CRI-O. Here are some of the key functions of a container runtime:

1. **Container image management**: The container runtime is responsible for downloading and managing container images from a registry. It caches images locally to improve performance and ensure that they are available even if the registry is unavailable.

2. **Container lifecycle management**: The container runtime manages the lifecycle of containers, including starting and stopping containers, monitoring their resource usage, and handling events such as crashes or failures.

3. **Networking**: The container runtime provides networking interfaces for containers, including assigning IP addresses and managing network interfaces.

4. **Storage**: The container runtime provides storage interfaces for containers, including managing volumes and mounting them in containers.

5. **Security**: The container runtime provides security features for containers, such as sandboxing and resource isolation, to ensure that containers are isolated from each other and from the host system.

The container runtime is a critical component of a Kubernetes cluster that provides an environment for running containerized applications. By abstracting away the underlying infrastructure and providing a consistent interface for running containers, Kubernetes enables developers to focus on building and deploying applications without worrying about the underlying infrastructure.


**KubeProxy**

The Kube-Proxy is a core component of Kubernetes that runs on each node in the cluster and is responsible for managing network traffic to and from pods. Its primary function is to provide a network proxy and load balancing service for pods running on the node. Here are some of the key functions of the Kube-Proxy in a Kubernetes cluster:

1. **Service discovery**: The Kube-Proxy maintains a list of all services in the cluster and their corresponding endpoints. It ensures that traffic to a service is directed to the appropriate pod based on the service's selector.

2. **Load balancing**: The Kube-Proxy provides load balancing for services by distributing traffic across multiple pods that provide the same service. It uses different load balancing algorithms, such as round-robin or random, to ensure that traffic is evenly distributed.

3. **Network routing**: The Kube-Proxy sets up network routing rules to ensure that traffic to and from pods is routed correctly. It also sets up network address translation (NAT) rules to ensure that traffic from outside the cluster can reach pods running on the node.

4. **Service proxy**: The Kube-Proxy provides a network proxy for services, which allows clients to access the service without knowing the IP addresses of the pods that provide the service.

The Kube-Proxy is a critical component of Kubernetes that provides network routing, load balancing, and service discovery for pods running on each node in the cluster. By automating these tasks, Kubernetes provides a scalable and reliable platform for running containerized workloads.
