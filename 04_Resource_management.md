ResourceManagement in Kubernetes

In Kubernetes, you can specify how much of each resource a Container needs for its workload, which is known as its resource request, and the maximum amount it's allowed to use, known as its resource limit.

Resource Requests: This is the amount of a specific resource that the system will guarantee for the container to run. The Kubernetes scheduler uses this information to decide which node to place the Pod on. For instance, if you set the memory request of a container to 256Mi, Kubernetes will only schedule it on a node with at least 256 MiB of memory that can be allocated to the task.

Resource Limits: This is the maximum amount of a specific resource that a container can use. If a container exceeds a resource limit, it may be terminated. For example, if you set a CPU limit of 1 on a container, that container will not be able to use more than 1 CPU core.

You can set these resource requests and limits in the Pod specification. Here's an example:

Sure, I can help you with that! Here's a lab exercise for Kubernetes Resource Management:

## Lab Exercise: Kubernetes Resource Management

### Objective
In this lab exercise, you will learn how to manage resources in a Kubernetes cluster using resource requests and limits.

### Prerequisites
- A Kubernetes cluster
- kubectl CLI

### Steps

1. Create a deployment with a container that consumes a lot of CPU and memory resources. For example, you can use the nginx image and set the CPU and memory requests and limits to high values:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        resources:
          requests:
            cpu: "2"
            memory: "2Gi"
          limits:
            cpu: "4"
            memory: "4Gi"
```

2. Verify that the deployment is running and the container is consuming the requested resources:

```bash
kubectl get pods
kubectl describe pod <pod-name>
```


### Conclusion
In this lab exercise, you learned how to manage resources in a Kubernetes cluster using resource requests and limits, and how to automatically scale a deployment based on CPU utilization using a horizontal pod autoscaler.



In this example:

The requests mean the container is guaranteed to have 256 MiB of memory and 500 milliCPU cores.

The limits mean the container can use up to 512 MiB of memory and 1 CPU core.

If a Container exceeds its memory limit, it might be terminated. If it continually exceeds its memory limit, its Pod might be evicted. If a Container exceeds its memory request, it is likely that its Pod will be evicted when the node runs out of memory.

For CPU, however, Containers are not killed for excessive CPU usage. If a Container uses more CPU than its limit, it will be throttled and not be allowed to use any more. The throttling persists even when there are CPU cycles free and available on the Node.

These configurations allow you to ensure that your Pods and containers are scheduled efficiently and behave well under resource constraints, which can help keep your applications running smoothly.
