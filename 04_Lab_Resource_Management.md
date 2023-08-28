 Here's a lab exercise for Kubernetes Resource Management:

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

3. Create a horizontal pod autoscaler (HPA) to automatically scale the deployment based on CPU utilization:

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: nginx-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: nginx-deployment
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

4. Verify that the HPA is created and is scaling the deployment based on CPU utilization:

```bash
kubectl get hpa
kubectl describe hpa nginx-hpa
```

5. Generate load on the deployment using a tool like Apache Bench:

```bash
ab -n 1000 -c 10 http://<service-ip>
```

6. Verify that the HPA is scaling the deployment based on the increased CPU utilization:

```bash
kubectl get hpa -w
```

### Conclusion
In this lab exercise, you learned how to manage resources in a Kubernetes cluster using resource requests and limits, and how to automatically scale a deployment based on CPU utilization using a horizontal pod autoscaler.
